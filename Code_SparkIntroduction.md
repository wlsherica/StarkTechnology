Introduction to Spark
========

## How to install

## RDD operations

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
