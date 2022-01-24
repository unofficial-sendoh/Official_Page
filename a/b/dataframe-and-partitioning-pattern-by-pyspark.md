# Dataframe and Partitioning Pattern by PySpark

[Explanation Video Link on Youtube](https://youtu.be/iRd2pf2HA\_4)

[B站中文解说视频链接](https://www.bilibili.com/video/BV1FZ4y1S7dF?share\_source=copy\_web)

```python
!apt-get install openjdk-8-jdk-headless -qq > /dev/null
!wget -q https://downloads.apache.org/spark/spark-3.1.2/spark-3.1.2-bin-hadoop3.2.tgz
!tar xf spark-3.1.2-bin-hadoop3.2.tgz
!pip install -q findspark

import os
os.environ["SPARK_HOME"] = "/content/spark-3.1.2-bin-hadoop3.2"
import findspark
findspark.init()

from pyspark.sql import SparkSession
from pyspark.sql.types import StructType,StructField, StringType
spark = SparkSession.builder.master("local").appName("Colab").getOrCreate()
data = [("James","US","36636"),
    ("Michael","US","40288"),
    ("Robert","US","42114"),
    ("Maria","US","39192"),
    ("Alice","US","23677"),
    ("Jen","US","89757"),
    ("Sendoh","JPN","74235"),
    ("Jen","JPN","45787"),
  ]

schema = StructType([ \
    StructField("Name",StringType(),True), \
    StructField("Country",StringType(),True), \
    StructField("id", StringType(), True), \
  ])
df = spark.createDataFrame(data=data,schema=schema)
rdd = df.rdd
rdd.glom().collect()

mappedRDD = rdd.map(lambda r: (r[1], r))

def partitioner(key):
    import random
    if key == "US":
        return random.randint(1, 2)
    else:
        return 3

numPartitions = 3
partionedRDD = mappedRDD.partitionBy(numPartitions, partitioner)
partionedRDD.glom().collect()
```
