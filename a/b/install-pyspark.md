# Install PySpark on Google Colab / Ubuntu and Solve Word Count

{% embed url="https://youtu.be/5KVJBklxqWM" %}

## Install Commands on Colab

```bash
!apt-get install openjdk-8-jdk-headless -qq > /dev/null
!wget -q https://downloads.apache.org/spark/spark-3.1.2/spark-3.1.2-bin-hadoop3.2.tgz
!tar xf spark-3.1.2-bin-hadoop3.2.tgz
!pip install -q findspark
import os
os.environ["SPARK_HOME"] = "/content/spark-3.1.2-bin-hadoop3.2"
import findspark
findspark.init()
```

## Install Commands on Ubuntu

```bash
conda create -n branch_name -q openjdk=8 pyspark=3.1.2
conda activate branch_name
```

## Code to Solve Word Count

```python
from pyspark.sql import SparkSession
spark = SparkSession.builder.master("local").appName("Colab").getOrCreate()
rdd=spark.sparkContext.parallelize([("file1.txt", "alpha beta delta sigma"),("file2.txt", "alpha theta sigma charlie"), ("file3.txt", "charlie beta")])
rdd_after_map=rdd.flatMap(lambda x: [(key,[x[0]]) for key in x[1].split()])
print(rdd_after_map.collect())
rdd_after_reduce = rdd_after_map.reduceByKey(lambda x, y: x + y)
print(rdd_after_reduce.collect())
```

