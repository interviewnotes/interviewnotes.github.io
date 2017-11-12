## Anagrams

Two strings are anagrams if they have the same count of every character. 
Example: "siri" and "iris" are anagrams, "whether" and "weather" are not (they are homophones).

### API
```java
public boolean areAnagrams(String a, String b);
```

### Approaches
#### Sorting
* Sort the 2 strings and compare.
* Example:
  * "siri" sorts to "iirs"
  * "iris" sorts to "iirs"
  * the sorted strings are equal.

> time: O(n log n) - n is the maximum length of the 2 strings.

> space: O(n) - if the string can't be sorted in-place (such as in Java), otherwise O(1)

#### Using Map
1. Create a `map` of character and its count.
2. For each character in `a`, update its count in `map`.
3. For each character in `b`
  * if character does not exist in `map`, then the strings are not anagrams.
  * decrement count for that character. 
  * if count is 0, then remove the character from `map`
4. If `map` is empty, then the strings are anagrams.

> time: O(n) - n is the maximum length of the 2 strings.

> space: O(n)

#### Tip
* Prepare a customized example for your interviewer. For example, "apple siri" and "spiral pie" are anagrams. Find anagrams at https://wordsmith.org/anagram/
  
