## Anagrams

### Problem Statement
Two strings are anagrams if they have the same count of every character. 
Example: "siri" and "iris" are anagrams, "whether" and "weather" are not.

### API
```java
public boolean areAnagrams(String a, String b);
```

### Approaches
#### Sort
* Sort the 2 strings and compare.
* Example:
  * "siri" sorts to "iirs"
  * "iris" sorts to "iirs"
  * the sorted strings are equal.
* O(n log n) - n is the maximum length of the 2 strings.

#### Using Map
* Steps
  1. For String a, create a Map `frequency` of characters and its count.
  2. Scan characters in String b, decrement count for corresponding character in `frequency`. 
     If character does not exist in `frequency`, then the strings are not anagrams.
  3. If `frequency` is empty, then the strings are anagrams.
* O(n) - n is the maximum length of the 2 strings.

#### Tip
* Prepare a customized example for the company you are interviewing with. For example, "apple siri" and "spiral pie" are anagrams.
  
