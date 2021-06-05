Deployment
============

This folder stores all the kubernetes deployment yaml for different services

```shell
kubectl apply -f deployment/<app>-deployment.yaml

# visit service
minikube service <app>-service
# or alternatively forwarding the deployment port to localhost
kubectl port-forward deployment/<app>-deployment <local_port>:<deployment_port>
```

To use spark-driver-deployment as a server
```shell
kubectl exec --stdin --tty deployment.apps/spark-driver-deployment -- sh
```

Using spark-submit (in cluster mode)
```shell
/opt/spark/bin/spark-submit \
  --master k8s://https://kubernetes.default.svc.cluster.local:443 \
  --deploy-mode cluster \
  --name spark-pi \
  --class org.apache.spark.examples.SparkPi \
  --conf spark.executor.instances=1 \
  --conf spark.kubernetes.authenticate.driver.serviceAccountName=spark-service-account \
  --conf spark.kubernetes.container.image=datamechanics/spark:3.1-latest \
  local:///opt/spark/examples/jars/spark-examples_2.12-3.1.1.jar 1000
```

Using spark-shell
```shell
/opt/spark/bin/spark-shell \
  --master k8s://https://kubernetes.default.svc.cluster.local:443 \
  --conf spark.executor.instances=1 \
  --conf spark.kubernetes.authenticate.driver.serviceAccountName=spark-service-account \
  --conf spark.kubernetes.container.image=datamechanics/spark:3.1-latest \
  --conf spark.driver.host=spark-driver-service.default.svc.cluster.local \
  --conf spark.driver.port=8080 \
  --conf spark.hadoop.fs.s3a.impl=org.apache.hadoop.fs.s3a.S3AFileSystem \
  --conf spark.hadoop.fs.s3a.endpoint=http://minio-service:9000 \
  --conf spark.hadoop.fs.s3a.access.key=minioadmin \
  --conf spark.hadoop.fs.s3a.secret.key=minioadmin \
  --conf spark.hadoop.fs.s3a.path.style.access=true
```
```scala
val df = spark.range(10e6.toLong).withColumn("part", $"id" % 2)
df.write.option("path", "s3a://warehouse/default.db/test_table").partitionBy("part").mode("overwrite").saveAsTable("test_table")
```

Using spark-sql
```shell
/opt/spark/bin/spark-sql \
  --master k8s://https://kubernetes.default.svc.cluster.local:443 \
  --conf spark.executor.instances=1 \
  --conf spark.kubernetes.authenticate.driver.serviceAccountName=spark-service-account \
  --conf spark.kubernetes.container.image=datamechanics/spark:3.1-latest \
  --conf spark.driver.host=spark-driver-service.default.svc.cluster.local \
  --conf spark.driver.port=8080 \
  --conf spark.hadoop.fs.s3a.impl=org.apache.hadoop.fs.s3a.S3AFileSystem \
  --conf spark.hadoop.fs.s3a.endpoint=http://minio-service:9000 \
  --conf spark.hadoop.fs.s3a.access.key=minioadmin \
  --conf spark.hadoop.fs.s3a.secret.key=minioadmin \
  --conf spark.hadoop.fs.s3a.path.style.access=true
```
```sql
SELECT * FROM default.test_table LIMIT 10;
```
