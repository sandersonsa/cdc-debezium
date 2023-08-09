# Para Ler
- https://debezium.io/documentation/reference/2.3/transformations/topic-routing.html

# [BASTANTE DOCUMENTAÇÃO LEGAL]
https://github.com/debezium/debezium-examples/blob/main/openshift/kafka-connector.yaml
# Quarkus com kafka
https://github.com/rmarting/kafka-clients-quarkus-sample/blob/main/README.md
# Registry schema
https://debezium.io/documentation/reference/stable/configuration/avro.html
https://www.apicur.io/registry/docs/apicurio-registry-operator/1.0.0/assembly-registry-storage.html

# exemplos
https://www.timeplus.com/post/cdc-in-action-with-debezium-and-timeplus
https://hevodata.com/learn/debezium-vs-kafka-connect/
https://klarrio.medium.com/database-replication-with-change-data-capture-over-kafka-975bc60cecce

https://access.redhat.com/documentation/pt-br/red_hat_build_of_debezium/2.1.4
https://debezium.io/blog/2020/02/25/lessons-learned-running-debezium-with-postgresql-on-rds/
https://github.com/foogaro/change-data-capture

Run the following commands:
    $ git clone https://github.com/solution-pattern-cdc/ansible.git
    $ cd ansible
    $ ansible-playbook playbooks/install.yml
Refer to the installation prerequisites for more details.
Solution Pattern

GitHub

Support: This community demo is supported directly by the Application Services TMM group.
- https://redhat-solution-patterns.github.io/solution-pattern-modernization-cdc/solution-pattern-modernization-cdc/index.html
- https://redhat-solution-patterns.github.io/solution-pattern-modernization-cdc/solution-pattern-modernization-cdc/03-demo.html#_pre_requisites


# debezium-demo

# docs
- https://access.redhat.com/documentation/en-us/red_hat_build_of_debezium/2.1.4/html-single/getting_started_with_debezium/index
- https://access.redhat.com/documentation/en-us/red_hat_build_of_debezium/2.1.4/html-single/installing_debezium_on_openshift/index
- https://access.redhat.com/documentation/en-us/red_hat_build_of_debezium/2.1.4/html-single/debezium_user_guide/index
- https://access.redhat.com/documentation/pt-br/red_hat_amq_streams/2.3/html-single/configuring_amq_streams_on_openshift/index
    Transformations
        - https://debezium.io/documentation/reference/stable/transformations/index.html
        - https://debezium.io/documentation/reference/stable/connectors/jdbc.html
        - https://debezium.io/blog/2017/09/25/streaming-to-another-database/
        - https://medium.com/swlh/sync-mysql-to-postgresql-using-debezium-and-kafkaconnect-d6612489fd64
        - https://stackoverflow.com/questions/76621796/connect-kafka-with-postgres-via-debezium-jdbc-sink-connector
        - https://github.com/solution-pattern-cdc
        - https://klarrio.medium.com/database-replication-with-change-data-capture-over-kafka-975bc60cecce

https://stackoverflow.com/questions/76621796/connect-kafka-with-postgres-via-debezium-jdbc-sink-connector
key.converter=io.confluent.connect.avro.AvroConverter
key.converter.schema.registry.url=http://localhost:8081
value.converter=io.confluent.connect.avro.AvroConverter
value.converter.schema.registry.url=http://localhost:8081


Debezium is a distributed platform that converts information from your existing databases into event streams, enabling applications to detect, and immediately respond to row-level changes in the databases.

Debezium is built on top of Apache Kafka and provides a set of Kafka Connect compatible connectors. Each of the connectors works with a specific database management system (DBMS). Connectors record the history of data changes in the DBMS by detecting changes as they occur, and streaming a record of each change event to a Kafka topic. Consuming applications can then read the resulting event records from the Kafka topic.



# Criar cluster AMQ Streams
https://gitlab.consulting.redhat.com/consulting-brazil/applicationpractice/quick-starts/automation/amq-streams/amq-streams-helm

- oc new-project amq-streams
## alterar o helm para usar o namespace acima

- helm uninstall amq-streams
- helm uninstall amq-streams-operator
- helm install -f helm-operator/values.yaml amq-streams-operator helm-operator/
- helm install amq-streams --values=helm/examples/kafka/values-persistent.yaml --values=helm/values-users.yaml --values=helm/values-topics.yaml helm/


# configurar kafka-ui
https://github.com/provectus/kafka-ui
oc new-app -l app=kafka-ui --name=kafka-ui -e DYNAMIC_CONFIG_ENABLED=true provectuslabs/kafka-ui

- criar volume com mountPath /etc/kafkaui/

# Deploying a MySQL database - database server configured with an example inventory database:
- oc new-app -l app=mysql --name=mysql quay.io/debezium/example-mysql:latest
# Configure credentials for the MySQL database
- oc set env deployment/mysql MYSQL_ROOT_PASSWORD=debezium MYSQL_USER=mysqluser MYSQL_PASSWORD=mysqlpw

# log into the sample inventory database.
- oc exec mysql-1-2gzx5 -it -- mysql -u mysqluser -pmysqlpw inventory
- oc exec mysql-6fc7878445-pkmn6 -it -- mysql -u mysqluser -pmysqlpw inventory

# Observe database
 - show databases;
 - select * from customers;

# Deploying Kafka Connect
After you deploy the MySQL database, use AMQ Streams to build a Kafka Connect container image that includes the Debezium MySQL connector plug-in. During the deployment process, you create and use the following custom resources (CRs):

- A KafkaConnect CR that defines your Kafka Connect instance and includes information about the MySQL connector artifacts to include in the image.
- A KafkaConnector CR that provides details that include information that the MySQL connector uses to access the source database. After AMQ Streams starts the Kafka Connect pod, you start the connector by applying the KafkaConnector CR.

# criar projeto debezium

- dbz-connect.yaml
- oc create -f dbz-connect.yaml
// - oc create is debezium-streams-connect

- debezium-inventory-connector.yaml
- oc create -f debezium-inventory-connector.yaml

# Check the status of the KafkaConnector
- oc describe KafkaConnector mysql-inv-connector -n amq-streams
# Verify that the connector created Kafka topics:
- oc get kafkatopics

# Check topic content.

 oc exec -n amq-streams -it amq-kafka-0 -- /opt/kafka/bin/kafka-console-consumer.sh \
    --bootstrap-server localhost:9092 \
    --from-beginning \
    --property print.key=true \
    --topic=dbserver1.inventory.customers
    <!-- --topic=dbserver1.inventory.products_on_hand -->

# Viewing change events

When the connector starts, it writes events to a set of Apache Kafka topics, each of which represents one of the tables in the MySQL database. The name of each topic begins with the name of the database server, dbserver1.

Conectar no banco de dados e habilitar um consumer
- oc exec mysql-6fc7878445-pkmn6 -it -- mysql -u mysqluser -pmysqlpw inventory

-  oc exec -n amq-streams -it amq-kafka-0 -- /opt/kafka/bin/kafka-console-consumer.sh \
    --bootstrap-server localhost:9092 \
    --from-beginning \
    --property print.key=true \
    --topic=dbserver1.inventory.customers

# executar operações na base de dados
- DELETE FROM customers WHERE id=1004;
- UPDATE customers SET first_name='Anne Marie' WHERE id=1004;

# parar connector
oc edit deployment/mysql-connect-dbz-i-connect
 change replicas para zero 0

# consultar pods
oc get pods -l strimzi.io/name=mysql-connect-dbz-i-connect



# tips and tricks

Database schema history topics for the Debezium Db2, MySQL, Oracle, and SQL Server connectors
For each of the preceding connectors a database schema history topic is required. Whether you manually create the database schema history topic, use the Kafka broker to create the topic automatically, or use Kafka Connect to create the topic, ensure that the topic is configured with the following settings:

Infinite or very long retention.
Replication factor of at least three in production environments.
Single partition.


By default, changes from one database table are written to a Kafka topic whose name corresponds to the table name. If needed, you can adjust the destination topic name by configuring Debezium’s topic routing transformation.
- Route records to a topic whose name is different from the table’s name
- Stream change event records for multiple tables into a single topic