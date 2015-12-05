
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
將上述產生的python function makePlural傳遞到map()中達到每個水果字串字尾+s
```python
pluralRDD = wordsRDD.<FILL IN>
print pluralRDD.collect()
```
### TODO: Let's create the same RDD using a `lambda` function.
利用lambda匿名函數,讓每個進來的水果字串字尾+s, 不用上述的python function
```python
pluralLambdaRDD = wordsRDD.<FILL IN>
print pluralLambdaRDD.collect()
```
### TODO: Now use `map()` and a `lambda` function to return the number of characters in each word.
利用map()計算每個水果單字的length(), 請用len()這個python自帶的長度函數
```python
pluralLengths = (pluralRDD.<FILL IN>)
print pluralLengths
```
### TODO: The next step in writing our word counting program is to create a new type of RDD, called a pair RDD
Hint: We can create the pair RDD using the `map()` transformation with a `lambda()` function to create a new RDD.
請建一個pairRDD, 每一個水果單字進來都給他一個數字1
```python
wordPairs = wordsRDD.<FILL IN>
print wordPairs.collect()
```
### TODO: Use `groupByKey()` to generate a pair RDD of type `('word', iterator)`
請用groupByKey()產生一個pairRDD, 請參考Spark API如何下groupByKey()
```python
wordsGrouped = wordPairs.<FILL IN>
for key, value in wordsGrouped.collect():
   print '{0}: {1}'.format(key, list(value))
```
### TODO: Use `mapValues()` to obtain the counts
利用mapValues()計算每一個水果的出現的次數
```python
wordCountsGrouped = wordsGrouped.<FILL IN>
print wordCountsGrouped.collect()
```
### TODO: Counting using `reduceByKey`
請用reduceByKey()針對水果字串wordPairs做加總,需要得到每個水果對應的次數
```python
wordCounts = wordPairs.<FILL IN>
print wordCounts.collect()
```
### TODO: put all together
請將上述的map, reduceByKey, RDD的過程以一行表示, 並且用collect(), print之需得到最後 wordcount的答案.
```python
wordCountsCollected = (wordsRDD.<FILL IN>)
print wordCountsCollected
```
