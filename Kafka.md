Kafka
=====


# 자주 사용하는 명령어

## 컨슈머 상태와 Offset확인
docker-compose exec broker kafka-consumer-groups --bootstrap-server broker:9092 --group local.use.pip.oracle-meta.json --describe

## Offset 리셋 예상 결과
docker-compose exec broker kafka-consumer-groups --bootstrap-server broker:9092 --group local.use.pip.oracle-meta.json --topic from-pip-topic --reset-offsets --to-earliest --dry-run

> 컨슈머 상태와 Offset을 describe로 확인하여 `CONSUMER-ID`,`HOST`,`CLIENT-ID`에 값이 있으면, 사용중으로 Offset 리셋 못 함. 컨슈머 종료 후 리켓 가능.

## Offset 리셋 실행
docker-compose exec broker kafka-consumer-groups --bootstrap-server broker:9092 --group local.use.pip.oracle-meta.json --topic from-pip-topic --reset-offsets --to-earliest --execute

## 토픽의 현재 offset 확인
docker-compose exec broker kafka-run-class kafka.tools.GetOffsetShell --broker-list broker:9092 --topic from-pip-topic

## 컨슈머 그룹 삭제
docker-compose exec broker kafka-consumer-groups --zookeeper zookeeper:2181 --delete --group <group_name>


## 토픽 정보 확인
docker-compose exec broker kafka-topics --describe --zookeeper zookeeper:2181 --topic from-pip-topic
