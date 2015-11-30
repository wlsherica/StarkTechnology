Introduction to Spark
========

## How to install

Please check [Code_SparkInstall](https://github.com/wlsherica/StarkTechnology/blob/master/Code_SparkInstall.md) out.

## RDD operations
Let's rock in our fruits journey :p
#### map
```python
lines = sc.parallelize(["apple", "banana", "strawberry", "lemon", "apple", "banana", "grapes", "watermelon", "strawberry", "lemon", "lemon"])
pairs = lines.map(lambda s: (s, 1))
fruit1 = pairs.reduceByKey(lambda a, b: a + b)
#[('strawberry', 2), ('watermelon', 1), ('lemon', 3), ('apple', 2), ('banana', 2), ('grapes', 1)]
```
#### mapValues
```python
x = sc.parallelize([("var1", ["apple", "banana", "lemon"]), ("var2", ["grapes"]), ("var3", ["watermelon", "strawberry"])])
x.mapValues(lambda x: len(x)).collect()
```
#### reduce
```python
sc.parallelize([10, 20, 30, 40, 50]).reduce(lambda a, b: a + b)

#another way 
from operator import add
sc.parallelize([10, 20, 30, 40, 50]).reduce(add)
```
How to sum up the fruits' values?
```python
fruit1.values().reduce(lambda x,y : x+y)
```
How to return an RDD with the keys of each tuple?
```python
fruit1.keys().collect()
```
#### reduceByKey
```python
x = sc.parallelize([("apple", 1), ("lemon", 2), ("apple", 3), ("apple", 9), ("lemon", 1), ("banana", 4), ("banana", 8)])
fruit2 = x.reduceByKey(lambda x, y: x + y)
fruit2.collect()
#[('lemon', 3), ('apple', 13), ('banana', 12)]
```
#### groupByKey
```python
sorted(rdd.groupByKey().mapValues(len).collect())
```
#### filter
```python
fruit1.take(3)
fruit1.filter(lambda x: x[1] > 2).collect()
```
#### takeOrdered
```python
sc.parallelize([5,4,3,1,2,6,7,9]).takeOrdered(6)
```
#### join
```python
# inner join
fruit1.join(fruit2).collect()
```
#### union
```python
fruit1.union(fruit2).collect()
```
#### cache
```python
fruit1.cache()
fruit1.persist().is_cached
fruit1.unpersist()
```

## Best Practice
### Bad input data

Check this bad input data in pyspark.

```python
input_rdd = sc.parallelize(["{\"value\": 1}",  # ok
                            "bad_json",  # Bad one
                            "{\"value\": 2}",  # ok
                            "{\"value\": 3"  # Missing brace.
                            ])
```
Try to convert string into JSON format to a sqlContext,

```python
sqlContext.jsonRDD(input_rdd).registerTempTable("mytable")
```
It looks good, however, 
```python
input_rdd.collect()
sqlContext.sql("select * from mytable").show()
```
Instead, we could fix this issue with python function,
```python
#do not forget to import json first
import json
def try_correct_json(json_string):
  try:
    json.loads(json_string)
    return [json_string]
  except ValueError:
    try:
      try_to_correct_json = json_string + "}"
      json.loads(try_to_correct_json)
      return [try_to_correct_json]
    except ValueError:
      return []
```
Then, use flatMap to apply our fixing function,
```python
corrected_input_rdd = input_rdd.flatMap(try_correct_json)
sqlContext.jsonRDD(corrected_input_rdd).registerTempTable("mytable")
```
Finally, we can apply this function to fix our bad JSON string. Then we will succeed to read these three lines.
```python
input_rdd.collect()
sqlContext.sql("select * from mytable").show()
```

### Number of data partitions?

There are several ways to find the partitions information, for example, viewing task stages with Spark UI
```python
inputdata = sc.parallelize(range(1,100))
inputdata.count()
```
Or inspect RDD partitions programatically in python,
```python
inputdata.getNumPartitions()
```
How about scala?
```scala
inputdata.partitions.size
```
