## Problem

Design a special dictionary which has some words and allows you to search the words in it by a prefix and a suffix.

Implement the `WordFilter` class:

- `WordFilter(string[] words)` Initializes the object with the `words` in the dictionary.
- `f(string prefix, string suffix)` Returns *the index of the word in the dictionary* which has the prefix `prefix` and the suffix `suffix`. If there is more than one valid index, return **the largest** of them. If there is no such word in the dictionary, return `-1`.

## Example

```
Input
["WordFilter", "f"]
[[["apple"]], ["a", "e"]]
Output
[null, 0]

Explanation
WordFilter wordFilter = new WordFilter(["apple"]);
wordFilter.f("a", "e"); // return 0, because the word at index 0 has prefix = "a" and suffix = 'e".
```

## Code

```java
import java.util.*;

class WordFilter {
    
    HashMap<String, List<Integer>> mapPrefix = new HashMap<>();
    HashMap<String, List<Integer>> mapSuffix = new HashMap<>();
    
    public WordFilter(String[] words) {
        for(int w = 0; w < words.length; w++){
            for(int i = 0; i <= 10 && i <= words[w].length(); i++){
                String perfix = words[w].substring(0, i);
                if(!mapPrefix.containsKey(perfix)) {
                    mapPrefix.put(perfix, new ArrayList<>());
                }
                mapPrefix.get(perfix).add(w);
                
                String s = words[w].substring(words[w].length() - i);
                if(!mapSuffix.containsKey(s)) {
                    mapSuffix.put(s, new ArrayList<>());
                }                           
                mapSuffix.get(s).add(w);
            }
        }
    }

    public int f(String prefix, String suffix) {
        if(!mapPrefix.containsKey(prefix) || !mapSuffix.containsKey(suffix)) {
            return -1;
        }
        List<Integer> p = mapPrefix.get(prefix);
        List<Integer> s = mapSuffix.get(suffix);
        int i = p.size()-1;
        int j = s.size()-1;
        while(i >= 0 && j >= 0){
            if(p.get(i) < s.get(j)) {
                j--;
            }
            else if(p.get(i) > s.get(j)) {
                i--;
            }
            else {
                return p.get(i);
            }
        }
        return -1;
    }
}
```

------

## Link

https://leetcode.com/problems/prefix-and-suffix-search/