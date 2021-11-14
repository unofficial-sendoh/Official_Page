# PySpark Fundamentals and Implement Top Ten Pattern

{% embed url="https://youtu.be/dhVaPHXEWsg" %}

{% embed url="https://www.bilibili.com/video/BV1A44y1v76k" %}

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
spark = SparkSession.builder.master("local").appName("Colab").getOrCreate()
rdd=spark.sparkContext.parallelize(["bird", "cat","dog","cat","dog","dog", "dog", "lion", "pig", "pig", "cat", "snake"], 2)
rdd.glom().collect()

def f(iterator):
  new_it = sorted(map(lambda x: (x, 1), iterator))
  from itertools import groupby
  groups = groupby(new_it, lambda y: y[0])
  KeyToCount = [(key, sum(j for i, j in group)) for key,group in groups]
  
  from heapq import nlargest
  largest_list = nlargest(2, KeyToCount, key=lambda e:e[1])

  for key in largest_list:
    yield key
  
rdd_after_map=rdd.mapPartitions(f)
rdd_after_map.glom().collect()

from operator import add
rdd_after_group=rdd_after_map.reduceByKey(add)
print(rdd_after_group.collect())

nlargest(2, rdd_after_group.collect(), key=lambda e:e[1])
```
