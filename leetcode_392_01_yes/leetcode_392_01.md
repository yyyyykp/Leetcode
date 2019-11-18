### 思路

题目tag是动规，但是不用动规O(n)也可以解决，直接按照顺序判断 s 中的字符是否出现在 t 中就好了

### 代码

```c++
class Solution {
public:
    bool isSubsequence(string s, string t) {
        int ls=s.length();
        int lt=t.length();
        if(ls==0) return 1;
        int j=0;
        for(int i=0;i<lt;i++){
            if(t[i]==s[j]){
                j++;
                if(j==ls) return 1;
            }
        }
        return 0;
    }
};
```