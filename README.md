# Tereza

Tereza is an initiative to bring heavy big data to light cloud. 
Everything from storage to computation will be run in a cloud-native way.

## Get Started

TODO

## High Level Architecture

- L0: Resource Management Layer

    - [Kubernetes](https://kubernetes.io/)

        Back when Google first introduced MapReduce in 2003, they have a separate while dedicated internal 
        service called Borg to schedule jobs (and applications) across clusters, which was omitted by community 
        for their first version of Hadoop, the open source equivalent for Google MapReduce. Only in v2.0 of Hadoop 
        released several years later did they bring YARN (Yet Another Resource Negotiator) to address this issue.
        Since then, YARN has become the de-factor standard for resource management in big data area, though not without
        competitors trying, for example, Apache Mesos, aims at being distributed systems kernel, and yet not widely 
        adopted by major companies. Until now, with cloud-native compaign coming in the period with the maturity of 
        containerization, Kubernetes has become the de-factor standard for containerized applications management. 
        Stateless applications like web app are very easy to be containerized, but not for stateful applications, like
        most of the big data framework. Still, many frameworks are gradually adapting to support running on Kubernetes.
        I personally believe this is the future of the industry. Thus comes this project. So Kubernetes will undoubtedly
        be the choice of resource management service.

- L1: Storage Layer

    - S3 Compatible Object Storage: [MinIO](https://min.io/)
    
        Why not HDFS? In cloud-native era, we want further decouple storage with computation and make storage an 
        independent service which can be scaled separately. HDFS is too clumsy for this purpose (though with its own 
        merits you can't deny). S3 is so popular but since it's in AWS, we would want an open source solution, while 
        compatible with S3 API. That's where MinIO comes in.
    
    - Durable and Replayable Message Queue Service: [Kafka](https://kafka.apache.org/)
    
        Not much choice we have here since Kafka is almost industry standard for event streaming data storage
    
    - RDBMS: [MySQL](https://www.mysql.com/)
    
        To support the persistent requirement of L2

- L2: Metadata Service Layer

    - Hive Metastore
    
        To support the MinIO metadata querying requirement from L3 
    
    - Confluent Schema Registry
    
        To support the Kafka metadata querying requirement from L3

- L3: Computing Layer

    - [Presto](https://prestodb.io/)
    
        an open source distributed SQL query engine for running interactive analytic queries against data sources of 
        all sizes ranging from gigabytes to petabytes.
    
    - [Spark](https://spark.apache.org/)
    
        a unified analytics engine for big data processing, with built-in modules for streaming, SQL, machine learning 
        and graph processing.
    
    - [Flink](https://flink.apache.org/)
    
        Stateful Computations over Data Streams
    
    - [TensorFlow](https://www.tensorflow.org/)

        An end-to-end open source machine learning platform for everyone.
