Tving Data Delivery
===================

# 시스템 구성

## 전체 구성
![구성](./images/tdd_1.jpeg)

## 프로듀서 구성
![구성](./images/tdd_2.jpeg)

## 카프카 구성
![구성](./images/tdd_3.jpeg)

## 컨슈머 구성
![구성](./images/tdd_4.jpeg)

# 실행

1. docker-compose
```
docker-compose up
```

2. mongodb cluster 구성
한번만 진행
```
docker-compose exec mongo1 mongo --eval 'rs.initiate({_id : "r0", members: [{ _id : 0, host : "mongo1:27017", priority : 1 },{ _id : 1, host :"mongo2:27017", priority : 0 },{ _id : 2, host : "mongo3:27017", priority : 0, arbiterOnly: true }]})'
```

3. MariaDB에 Quartz 스키마 만들기
MariaDB에 접속하여 데이터베이스 생성 후 테이블 생성
```
create database batchdb;

테이블 생성 : db/mariadb/sql/quartz-schema-mysql.sql
```

> MAC에서 종종 사용하는 MariaDB GUI TOOL로 [Sequel Pro](https://www.sequelpro.com/) 추천

4. MariaDB에 pip 데이터베이스 만들기

```
CREATE DATABASE pipdb CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;

CREATE USER 'pip-app'@'%' IDENTIFIED BY 'iwb123456' PASSWORD EXPIRE NEVER; 
GRANT ALL PRIVILEGES ON pipdb.* TO 'pip-app'@'%';
flush privileges;
```

5. MongoDB 호스트 등록
`/etc/hosts` 파일에 MongoDB 맵핑 추가
```
127.0.0.1 mongo1 mongo2 mongo3
```

5. 프로듀서, 컨슈머 실행
```
gradle clean build bootrun
```

> 컨테이너별 로그 `docker-compose logs -f mongo1 `


# kafka

## 토픽 만들기

```
docker-compose exec broker kafka-topics \
  --create \
  --bootstrap-server localhost:9092 \
  --replication-factor 1 \
  --partitions 2 \
  --topic from-epg-topic
```

## 토픽 리스트
```
docker-compose exec broker kafka-topics --list --bootstrap-server broker:9092 
```

## 토픽 메세지 쓰기
```
docker-compose exec broker kafka-console-producer --topic from-epg-topic --broker-list broker:9092
```

## 토픽 메세지 읽기
```
docker-compose exec broker kafka-console-consumer --bootstrap-server broker:9092 --topic from-pip-topic --from-beginning
```

## 토픽 정보 
```
docker-compose exec broker kafka-topics --group pip-topic-group1 --describe
```

## 토픽 그룹 정보 
```
docker-compose exec broker kafka-consumer-groups --bootstrap-server broker:9092 –-group pip-topic-group1 --describe
```

## 콘솔 컨슈머
```
docker-compose exec broker kafka-console-consumer --bootstrap-server broker:9092 --topic from-pip-topic --from-beginning
```


# Visual Studio Code

## 추천 플러그인
- Gradle Language Support 0.2.3
- Gradle Tasks 3.6.1
- Kafka 0.11.0
- MongoDB for VS Code 0.5.0
- Project Dashboard 2.3.1
- SQLTools 0.23.0


