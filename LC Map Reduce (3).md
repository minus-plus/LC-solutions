Word Count (Map Reduce)
Inverted Index (Map Reduce)
 
#### Word Count (Map Reduce)

**Description**   
Using map reduce to count word frequency.

**[题目链接](http://www.lintcode.com/en/problem/word-count-map-reduce/)**    

**Solution**   
**思路**  
StringTokenizer.   
map() and reduce都是应用于Iterable sequence的方法.  
>map(function, iterable, ...) @python  
Apply function to every item of iterable and return a list of the results. If additional iterable arguments are passed, function must take that many arguments and is applied to the items from all iterables in parallel. If one iterable is shorter than another it is assumed to be extended with None items. If function is None, the identity function is assumed; if there are multiple arguments, map() returns a list consisting of tuples containing the corresponding items from all iterables (a kind of transpose operation). The iterable arguments may be a sequence or any iterable object; the result is always a list.  
reduce(function, iterable[, initializer])  @python  
Apply function of two arguments cumulatively to the items of iterable, from left to right, so as to reduce the iterable to a single value. For example, reduce(lambda x, y: x+y, [1, 2, 3, 4, 5]) calculates ((((1+2)+3)+4)+5). The left argument, x, is the accumulated value and the right argument, y, is the update value from the iterable. If the optional initializer is present, it is placed before the items of the iterable in the calculation, and serves as a default when the iterable is empty. If initializer is not given and iterable contains only one item, the first item is returned. Roughly equivalent to:
```
def reduce(function, iterable, initializer=None):
    it = iter(iterable)
    if initializer is None:
        try:
            initializer = next(it)
        except StopIteration:
            raise TypeError('reduce() of empty sequence with no initial value')
    accum_value = initializer
    for x in it:
        accum_value = function(accum_value, x)
    return accum_value
```  
**代码**   
```java
/**
 * Definition of OutputCollector:
 * class OutputCollector<K, V> {
 *     public void collect(K key, V value);
 *         // Adds a key/value pair to the output buffer
 * }
 */
public class WordCount {

    public static class Map {
        public void map(String key, String value, OutputCollector<String, Integer> output) {
            // Write your code here
            // Output the results into output buffer.
            // Ps. output.collect(String key, int value);
            StringTokenizer tokenizer = new StringTokenizer(value);
            while (tokenizer.hasMoreTokens()) {
                String s = tokenizer.nextToken();
                output.collect(s, 1);
            }
        }
    }

    public static class Reduce {
        public void reduce(String key, Iterator<Integer> values,
                           OutputCollector<String, Integer> output) {
            // Write your code here
            // Output the results into output buffer.
            // Ps. output.collect(String key, int value);
            int sum = 0;
            while (values.hasNext()) {
                sum += values.next();
            }
            output.collect(key, sum);
        }
    }
}

```
* * *

####  Inverted Index (Map Reduce)

**Description**   
Use map reduce to build inverted index for given documents.

**[题目链接](http://www.lintcode.com/en/problem/inverted-index-map-reduce/)**    

**Solution**   
**思路**  
map， key is an document.  
reduce， key is a word.  

**代码**   
```java
/**
 * Definition of OutputCollector:
 * class OutputCollector<K, V> {
 *     public void collect(K key, V value);
 *         // Adds a key/value pair to the output buffer
 * }
 * Definition of Document:
 * class Document {
 *     public int id;
 *     public String content;
 * }
 */
public class InvertedIndex {

    public static class Map {
        public void map(String _, Document value,
                        OutputCollector<String, Integer> output) {
            // Write your code here
            // Output the results into output buffer.
            // Ps. output.collect(String key, int value);
            StringTokenizer tokenizer = new StringTokenizer(value.content);
            int id = value.id;
            while (tokenizer.hasMoreTokens()) {
                String word = tokenizer.nextToken();
                output.collect(word, id);
            }
        }
    }

    public static class Reduce {
        public void reduce(String key, Iterator<Integer> values,
                           OutputCollector<String, List<Integer>> output) {
            // Write your code here
            // Output the results into output buffer.
            // Ps. output.collect(String key, List<Integer> value);
            List<Integer> index = new ArrayList<Integer>();
            int previous = -1;
            while (values.hasNext()) {
                int current = values.next();
                if (current != previous) {
                    index.add(current);
                }
                previous = current;
            }
            output.collect(key, index);
        }
    }
}

```
* * *
####  Anagram (Map Reduce)

**Description**   
>Use Map Reduce to find anagrams in a given list of words.  
>
>Example
>Given ["lint", "intl", "inlt", "code"], return ["lint", "inlt", "intl"],["code"].  
>
>Given ["ab", "ba", "cd", "dc", "e"], return ["ab", "ba"], ["cd", "dc"], ["e"].  

**[题目链接](http://www.lintcode.com/en/problem/anagram-map-reduce/#)**    

**Solution**   
**思路**  
map时，将word sort并作为key存储。  
reduce时，将key的list of word merge。   


**代码**   
```java
/**
 * Definition of OutputCollector:
 * class OutputCollector<K, V> {
 *     public void collect(K key, V value);
 *         // Adds a key/value pair to the output buffer
 * }
 */
public class Anagram {

    public static class Map {
        public void map(String key, String value,
                        OutputCollector<String, String> output) {
            // Write your code here
            // Output the results into output buffer.
            // Ps. output.collect(String key, String value);
            StringTokenizer tokenizer = new StringTokenizer(value);
            while (tokenizer.hasMoreTokens()) {
                String word = tokenizer.nextToken();
                char[] chars = word.toCharArray();
                Arrays.sort(chars);
                output.collect(new String(chars),word);
            }
        }
    }

    public static class Reduce {
        public void reduce(String key, Iterator<String> values,
                           OutputCollector<String, List<String>> output) {
            // Write your code here
            // Output the results into output buffer.
            // Ps. output.collect(String key, List<String> value);
            List<String> list = new ArrayList<String>();
            while (values.hasNext()) {
                String word = values.next();
                list.add(word);
            }
            output.collect(key, list);
        }
    }
}

```
* * *

