### 思路

使用pre标记到当前位置最小的数，如果一个数比pre小，就更新pre的值，否则就减去pre，判断大小更新ans。

即：ans=max(ans,prices[i]-pre)

### 代码

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int length=prices.size();
        if(length ==0 ) return 0;
        int pre=prices[0];
        int ans=0;
        for(int i=1;i<length;i++){
            if(prices[i]<pre) pre=prices[i];
            else{
                if(prices[i]>pre)
                    ans=max(ans,prices[i]-pre);
            }
        }
        return ans;
    }
};
```