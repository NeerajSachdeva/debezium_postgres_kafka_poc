# debezium_postgres_kafka_poc
Pass write operation events on postgres to Kafka with the help of Debezium

1. spinup docker yaml
	docker-compose up -d

2. Go to Postgres image CLI
	a. connect to db
		psql -U docker -d exampledb -W
  # password is mentioned in docker file as docker for now.

	b. create table
		CREATE TABLE student(id integer primary key, name varchar);
		SELECT * FROM student;
	c. ALTER TABLE public.student REPLICA IDENTITY FULL;

2. run 
	curl -i -X POST -H "Accept:application/json" -H "Content-Type:application/json" 127.0.0.1:8083/connectors/ --data "@debezium.json"

docker run --tty --network postgres_debezium_cdc-master_default confluentinc/cp-kafkacat kafkacat -b kafka:9092 -C -s key=s -s value=avro -r http://schema-registry:8081 -t postgres.public.student
