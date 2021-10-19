# kafka 실습

## kafka 설치
- git을 통해 kafka docker 코드를 내려받는다.  (코드를 내려받기 전 반드시 적당한 디렉토리로 이동을 한다.)


```
git clone https://github.com/CUKykkim/kafka-docker.git
```

- 다음 받은 코드의 디렉토리로 이동을 한다.

```
cd kafka-docker
```

- 도커 명령어를 이용해 컨테이너를 띄운다.

```
docker-compose up
```

- 도커 명령어를 이용해 현재 수행중인 컨테이너의 상태를 확인한다. (카프카 구동 성공)

```
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                                                           NAMES
7abcdd9be877   5684f498a30c   "start-kafka.sh"         22 minutes ago   Up 8 seconds    0.0.0.0:9092->9092/tcp, :::9092->9092/tcp                       kafka-docker_kafka_1
98b2f1eafe60   77ab524848e3   "/docker-entrypoint.…"   22 minutes ago   Up 10 seconds   2888/tcp, 0.0.0.0:2181->2181/tcp, :::2181->2181/tcp, 3888/tcp   kafka-docker_zookeeper_1
```

## kafka 수행하기

- 다음 명령어를 이용해 kafka 컨테이너 내부로 진입한다.

```
docker exec -it  kafka-docker_kafka_1 /bin/bash
```

- kafka 디렉토리로 이동한다. 

```
cd opt/kafka
```

- kafka topic을 생성한다. 이때 cuk라는 topic이 생성된다.

```
bin/kafka-topics.sh --create --zookeeper zookeeper:2181 --replication-factor 1 --partitions 1 --topic cuk
```


- 생성된 kafka topic을 확인한다.

```
bin/kafka-topics.sh --list --zookeeper zookeeper:2181
```

- 생성된 topic에 관한 세부 정보를 확인한다.
```
bin/kafka-topics.sh --describe --zookeeper zookeeper:2181 --topic cuk
```

### Producer-Consumer 수행하기

먼저 간략하게 kafka에서 미리 구현해 놓은 Producer와 Consumer를 수행해 보도록한다.

- producer 수행

```
bin/kafka-console-producer.sh --broker-list kafka:9092 --topic cuk
>hello
>cuk
>ssssa
>sd
>a
>d
>sad
>as
>das
```

- 또 다른 탭을 열어 새탭에서 Conumser 수행

```
bin/kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic cuk --from-beginning
hello
cuk
ssssa
sd
a
d
sad
as
das
```

파이썬으로 producer와 consumner를 구현하여 수행해 본다. 

- producer.py 작성

```
import time
import random
import datetime
from kafka import KafkaProducer

bootstrap_servers = ['kafka:9092'] # kafka broker ip
topicName = 'cuk'
producer = KafkaProducer(bootstrap_servers = bootstrap_servers)

for i, _ in enumerate(range(5)):

    # test1 - send numeric type
    print(i)
    producer.send(topicName, str(i).encode())

    # test2 = send string type
    text = 'This is ' + str(i) + ' msg'
    print(text)

    tim = datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')
    producer.send(topicName, text.encode())
    producer.send(topicName, tim.encode())

    time.sleep(1)
```


- consumer.py 작성

```
from kafka import KafkaConsumer

consumer = KafkaConsumer('cuk',bootstrap_servers=['kafka:9092'])
for message in consumer:
    print ("%s:   value=%s" % (message.topic, message.value))
```

- producer.py 수행

```
python3 producer.py
```


- consumer.py 수행

```
python3 consumer.py
```

## kafka 수행 종료

- docker를 빠져나온뒤 모든 컨테이너 수행을 종료한다. 

```
docker-compose down
```