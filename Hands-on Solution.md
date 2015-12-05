# Solution of Hands-on
```python
wordsList = ["apple", "banana", "strawberry", "lemon", "apple", "banana", "apple", "apple", "apple", "apple", "apple", "lemon", "lemon", "lemon", "banana", "banana", "banana", "banana", "banana", "banana", "apple", "apple", "apple", "apple"]
wordsRDD = sc.parallelize(wordsList, 4)
# Print out the type of wordsRDD
print type(wordsRDD)
def makePlural(word):
   # Adds an 's' to `word`
   return word + 's'
print makePlural('cat')
# TODO: Now pass each item in the base RDD into a map()
pluralRDD = wordsRDD.map(makePlural)
print pluralRDD.collect()
# TODO: Let's create the same RDD using a `lambda` function.
pluralLambdaRDD = wordsRDD.map(lambda x: x+'s')
print pluralLambdaRDD.collect()
# TODO: Now use `map()` and a `lambda` function to return the number of characters in each word.
pluralLengths = (pluralRDD.map(lambda x: len(x)).collect())
print pluralLengths
# TODO: The next step in writing our word counting program is to create a new type of RDD, called a pair RDD
# Hint: # We can create the pair RDD using the `map()` transformation with a `lambda()` function to create a new RDD.
wordPairs = wordsRDD.map(lambda x: (x, 1))
print wordPairs.collect()
# TODO: Use `groupByKey()` to generate a pair RDD of type `('word', iterator)`
wordsGrouped = wordPairs.groupByKey()
for key, value in wordsGrouped.collect():
   print '{0}: {1}'.format(key, list(value))
# TODO: Use `mapValues() to obtain the counts
wordCountsGrouped = wordsGrouped.mapValues(lambda x: sum(x))
print wordCountsGrouped.collect()
# TODO: Counting using `reduceByKey`
wordCounts = wordPairs.reduceByKey(lambda x, y: x+y).collect()
print wordCounts.collect()
# TODO: put all together
wordCountsCollected = (wordsRDD.map(lambda x: (x, 1))
                      .reduceByKey(lambda x, y: x + y)
                      .collect())
print wordCountsCollected

```
