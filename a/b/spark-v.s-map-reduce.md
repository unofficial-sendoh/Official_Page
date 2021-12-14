# Spark v.s Map Reduce and Summarization Pattern

[Explanation Video Link on Youtube](https://youtu.be/H5Pp2Uhmom4)

[B站英文解说视频链接](https://www.bilibili.com/video/BV1AP4y1G7TH/)

```bash
!apt-get install openjdk-8-jdk-headless -qq > /dev/null
!wget -q https://downloads.apache.org/spark/spark-3.1.2/spark-3.1.2-bin-hadoop3.2.tgz
!tar xf spark-3.1.2-bin-hadoop3.2.tgz
!pip install -q findspark
```

```python
import os
os.environ["SPARK_HOME"] = "/content/spark-3.1.2-bin-hadoop3.2"
import findspark
findspark.init()
from pyspark.sql import SparkSession
spark = SparkSession.builder.master("local").appName("Colab").getOrCreate()
rdd=spark.sparkContext.parallelize([("file1.txt", "alpha beta delta sigma"),("file2.txt", "alpha theta sigma charlie"), ("file3.txt", "charlie beta")])
rdd_after_map=rdd.flatMap(lambda x: [(key,[x[0]]) for key in x[1].split()])
print(rdd_after_map.collect())
rdd_after_reduce = rdd_after_map.reduceByKey(lambda x, y: x + y)
print(rdd_after_reduce.collect())
```

