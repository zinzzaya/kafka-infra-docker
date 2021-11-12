# kafka-infra-docker

# 실행

1. docker-compose(require)
```
docker-compose up
```

2. 프로듀서, 컨슈머 실행(require)
```
gradle clean build bootrun
```

> 컨테이너별 로그 `docker-compose logs -f broker `


# kafka

## 토픽 만들기

```
docker-compose exec broker kafka-topics \
  --create \
  --bootstrap-server localhost:9092 \
  --replication-factor 1 \
  --partitions 2 \
  --topic from-kafka-topic
```

## 토픽 리스트
```
docker-compose exec broker kafka-topics --list --bootstrap-server broker:9092 
```

## 토픽 메세지 쓰기
```
docker-compose exec broker kafka-console-producer --topic from-kafka-topic --broker-list broker:9092
```

## 토픽 메세지 읽기
```
docker-compose exec broker kafka-console-consumer --bootstrap-server broker:9092 --topic from-kafka-topic --from-beginning
```

## 토픽 정보 
```
docker-compose exec broker kafka-topics --group kafka-topic-group1 --describe
```

## 토픽 그룹 정보 
```
docker-compose exec broker kafka-consumer-groups --bootstrap-server broker:9092 –-group kafka-topic-group1 --describe
```

## 콘솔 컨슈머
```
docker-compose exec broker kafka-console-consumer --bootstrap-server broker:9092 --topic from-kafka-topic --from-beginning
```


##유용한 플러그인
```
intellij kafkalytic
```
