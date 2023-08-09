# debezium-test

Shows the change data capture (CDC) capabilities of Red Hat Integration with connectors.

This demo consists of three scenarios that you can use to demonstrate the CDC capabilities of Red Hat Integration with Debezium, Apache Kafka, and Apache Camel Kafka connectors.

Duration: Each basic scenario can be completed in 10-20 minutes. The advanced scenario depends on the features being demonstrated.

Audience: Developers, architects, and data integrators

Product(s): Red Hat OpenShift Container Platform

Specifications
TBD

Supporting Materials
TBD

Prerequisites
Debezium
AMQ Streams
Scenarios
Introducing Debezium

In this scenario, you capture events from a MySQL database to Apache Kafka. You can review the data structure of the event for each of the possible changes in the database: create, update, and delete.

This is a basic demo where everything is set up for you to go over the solution pattern from the Solution Explorer web application.

Data Replication

This scenario covers how to avoid dual writes. You have a legacy PHP application that needs to update an Elasticsearch index, with the new orders captured for future searches. CDC tasks are performed by Debezium connectors and include the use of single message transformations. These tasks are followed by the deployment of the new Camel Kafka connectors.

This is a basic demo where everything is set up for you to go over the solution pattern from the Solution Explorer web application.

Advanced Demonstrations

This scenario can be used to demonstrate advanced configurations and requirements for Debezium.

This is an advanced demo that includes only base PostgreSQL and MongoDB deployments and a Kafka cluster. You can use it to show how to configure the database for CDC, deploy a Kafka Connect cluster using AMQ Streams, and configure a connector using either the REST API or the OpenShift custom resource definition.

tutorial-web-app-instructions