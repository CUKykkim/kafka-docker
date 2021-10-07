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

- 도커 명령어를 이용해 현재 수행중인 컨테이너의 상태를 확인한다.

```
CONTAINER ID   IMAGE                           COMMAND                  CREATED             STATUS             PORTS                                                           NAMES
aad46aac4f0e   wurstmeister/kafka:2.12-2.4.0   "start-kafka.sh"         About an hour ago   Up About an hour   0.0.0.0:9092->9092/tcp, :::9092->9092/tcp                       kafka-docker_kafka_1
981869204b81   zookeeper:3.4                   "/docker-entrypoint.…"   About an hour ago   Up About an hour   2888/tcp, 0.0.0.0:2181->2181/tcp, :::2181->2181/tcp, 3888/tcp   kafka-docker_zookeeper_1
```

## kafka 수행하기

- 다음 명령어를 이용해 kafka 컨테이너 내부로 진입한다.

```

```
