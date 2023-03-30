# 1286. Iterator for Combination

Design the CombinationIterator class:

CombinationIterator(string characters, int combinationLength) Initializes the object with a string characters of sorted distinct lowercase English letters and a number combinationLength as arguments.
next() Returns the next combination of length combinationLength in lexicographical order.
hasNext() Returns true if and only if there exists a next combination.

回溯
```java
class CombinationIterator {
    List<String> list = new LinkedList<>();
    StringBuilder sb = new StringBuilder();
    int index = 0;
    public CombinationIterator(String characters, int combinationLength) {
        dfs(characters, combinationLength, 0);
    }

    void dfs(String characters, int combinationLength, int index){
        if(sb.length() == combinationLength){
            list.add(sb.toString());
        }
        for(int i = index; i < characters.length(); ++i){
            sb.append(characters.charAt(i));
            dfs(characters, combinationLength, i + 1);
            sb.deleteCharAt(sb.length() - 1);
        }
    }
    public String next() {
        return list.get(index++);
    }
    public boolean hasNext() {
        return index < list.size();
    }
}
```