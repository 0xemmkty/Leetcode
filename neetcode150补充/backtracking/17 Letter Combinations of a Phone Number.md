# 17. Letter Combinations of a Phone Number

>Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

>A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

题中有图，看图很清晰。
大概：2（abc） 3（def）...9（wxyz）

## Solution 1
```
Example 1:
Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

```java
class Solution {
    List<String>res;//最终输出结果res
    //数字到字符串的映射表，如letter_map[2]="abc",即数字2对应abc
    String Map[]={" "," ","abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"};
    public List<String>letterCombinations(String digits){
        res=new ArrayList<>();
        if(digits==null || digits.length()==0)
            return res;
        interDigits(digits,new StringBuilder(),0);
        return  res;
    }
    //递归函数
    private void interDigits(String digits,StringBuilder letter,int index){
        //letter用来保存每次组合的结果
        //每进行完一次组合后，就走到递归边界，此时保存组合结果
        if(index==digits.length()){
            res.add(letter.toString());
            return;
        }
        //pos:获取映射表letter_map索引，如index是2，有'2'-'0'=2，获取下标为2,letter_map[2]就是"abc"
        int pos=digits.charAt(index)-'0';//index:即每个数字的索引
        String MapString=Map[pos];//获取index对应的数字的字符串
        //遍历字符串
        for(int i=0;i<MapString.length();i++){
            //程序递归部分
            letter.append(MapString.charAt(i));
            interDigits(digits,letter,index+1);//下一个数字
            letter.deleteCharAt(letter.length()-1);//清除上次保存的字符,防止脏数据(每次递归结束后，letter仍保存着上次的组合结果)
        }
    }
}
```
