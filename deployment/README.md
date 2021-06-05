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

To use spark-driver as a server
```shell
kubectl exec --stdin --tty deployment.apps/spark-driver-deployment -- sh
```
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
```shell
/opt/spark/bin/spark-shell \
  --master k8s://https://kubernetes.default.svc.cluster.local:443 \
  --conf spark.executor.instances=1 \
  --conf spark.kubernetes.authenticate.driver.serviceAccountName=spark-service-account \
  --conf spark.kubernetes.container.image=datamechanics/spark:3.1-latest \
  --conf spark.driver.host=spark-driver-service.default.svc.cluster.local \
  --conf spark.driver.port=8080
```
```scala
sc.hadoopConfiguration.set("fs.s3a.impl", "org.apache.hadoop.fs.s3a.S3AFileSystem")
sc.hadoopConfiguration.set("fs.s3a.endpoint", "http://minio-service:9000")
sc.hadoopConfiguration.set("fs.s3a.access.key", "minioadmin")
sc.hadoopConfiguration.set("fs.s3a.secret.key", "minioadmin")
sc.hadoopConfiguration.set("fs.s3a.path.style.access", "true")

val df = spark.range(10e6.toLong).withColumn("part", $"id" % 2)
df.write.format("parquet").partitionBy("part").mode("overwrite").save("s3a://mybucket/result")
```
