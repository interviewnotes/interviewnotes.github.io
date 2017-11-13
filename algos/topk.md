## Top K

* Given a list of maybe repeated strings, get K strings that occur frequently, in descending order of frequency.
* Applications
  * Given a log file containing CPU times of tasks, find the top-K tasks that took the most total CPU time.
  * Given a list of search terms, find the top-K search terms.
  
### API
```java
public List<String> topK(List<String>);
```

### Approach
* Create a `map` of type `Map<String, Integer>` - each entry is a string value mapped to its count.

  > time: O(n) - n is the number of input strings

  > space: O(n)
  
* The `map` from the first step is not sorted. There are 2 approaches to extract the top K from the `map`:

#### Using ArrayList
1. Create an `ArrayList<Entry>` where `Entry` contains the string and its count.
2. Sort the `ArrayList`. Return first K elements, and discard others.

* If there are a million distinct strings, this approach will have allocate space and sort all of them.
  
> time: O(d log d) - d is distinct number of input strings
  
> space: O(d)
  
#### Using Heap
1. Create a `minHeap` of type `PriorityQueue<Entry>`, where `Entry` contains the string and its count.
2. For each entry in `Map<String, Integer>`, create a corresponding instance of `Entry`. 
  * If the size of `minHeap` is less than K, add `Entry` to `minHeap`.
  * Otherwise, if the top of `minHeap` is less than `Entry`'s count, then pop from `minHeap` and add the new `Entry`.
3. Pop all entries from min-Heap and add to a `List`
4. The `List` will have elements in ascending order of count. So, return the reversed list.
  
> time: O(d log k) - d is distinct number of input strings, heap operations require O(log k)
  
> space: O(k)
 
