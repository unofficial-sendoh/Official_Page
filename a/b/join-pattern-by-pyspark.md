# Join Pattern by PySpark

[Explanation Video Link on Youtube](https://youtu.be/Z\_MnnW9tiFk)

```python
!apt-get install openjdk-8-jdk-headless -qq > /dev/null
!wget -q https://downloads.apache.org/spark/spark-3.1.2/spark-3.1.2-bin-hadoop3.2.tgz
!tar xf spark-3.1.2-bin-hadoop3.2.tgz
!pip install -q findspark
import os
os.environ["SPARK_HOME"] = "/content/spark-3.1.2-bin-hadoop3.2"
import findspark
findspark.init()
```

```python
from pyspark.sql import SparkSession
spark = SparkSession.builder.master("local").appName("Colab").getOrCreate()
data = [('USA','1','NewYork'),
  ('Japan','2','TOKYO'),
  ('USA','3','SanFrancisco'), 
]
columns = ["Country","ID","City"]
big_rdd_df = spark.createDataFrame(data=data, schema = columns)

data = [('20','1'),
  ('123','2'),
  ('233','4'), 
]
columns = ["Posts","ID"]
small_rdd_df = spark.createDataFrame(data=data, schema = columns)
big_rdd_map = big_rdd_df.rdd.map(lambda x : (x["ID"], [x["Country"], x["City"]]))
small_rdd_map = small_rdd_df.rdd.map(lambda x : (x["ID"], [x["Posts"]]))
spark.sparkContext.broadcast(small_rdd_map.collect())
```

```python
rdd_after_join = big_rdd_map.join(small_rdd_map)
print(rdd_after_join.collect())
rdd_after_join_map = rdd_after_join.map(lambda x: (x[0], x[1][0][0], x[1][0][1], x[1][1][0]))
rdd_after_join_df = rdd_after_join_map.toDF(["ID", "Country", "City", "Posts"])
rdd_after_join_df.show()
```

