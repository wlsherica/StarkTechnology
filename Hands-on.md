
## Our fruits data list
```python
wordsList = ["apple", "banana", "strawberry", "lemon", "apple", "banana", "apple", "apple", "apple", "apple", "apple", "lemon", "lemon", "lemon", "banana", "banana", "banana", "banana", "banana", "banana", "apple", "apple", "apple", "apple"]
wordsRDD = sc.parallelize(wordsList, 4)
```

### Print out the type of wordsRDD
```python
print type(wordsRDD)
```
```python
def makePlural(word):
   return word + 's'
print makePlural('banana')
```
### TODO: Now pass each item in the base RDD into a map()
```python
pluralRDD = wordsRDD.<FILL IN>
print pluralRDD.collect()
```
### TODO: Let's create the same RDD using a `lambda` function.
```python
pluralLambdaRDD = wordsRDD.<FILL IN>
print pluralLambdaRDD.collect()
```
### TODO: Now use `map()` and a `lambda` function to return the number of characters in each word.
```python
pluralLengths = (pluralRDD.<FILL IN>)
print pluralLengths
```
### TODO: The next step in writing our word counting program is to create a new type of RDD, called a pair RDD
Hint: We can create the pair RDD using the `map()` transformation with a `lambda()` function to create a new RDD.
```python
wordPairs = wordsRDD.<FILL IN>
print wordPairs.collect()
```
### TODO: Use `groupByKey()` to generate a pair RDD of type `('word', iterator)`
```python
wordsGrouped = wordPairs.<FILL IN>
for key, value in wordsGrouped.collect():
   print '{0}: {1}'.format(key, list(value))
```
### TODO: Use `mapValues()` to obtain the counts
```python
wordCountsGrouped = wordsGrouped.<FILL IN>
print wordCountsGrouped.collect()
```
### TODO: Counting using `reduceByKey`
```python
wordCounts = wordPairs.<FILL IN>
print wordCounts.collect()
```
### TODO: put all together
```python
wordCountsCollected = (wordsRDD.<FILL IN>)
print wordCountsCollected
```
