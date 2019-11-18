### 思路

根据题意可知需要选取不相邻的值，使得它们的和尽可能得大。

定义dp\[][2]，dp\[i][0]代表第 i 个房屋不被选取时可得的最大值，dp\[i][1]代表第 i 个房屋被选取时可得的最大值。

那么对于第 i 个房屋而言，

- 假如不被选取，那么前一个房屋选取不选取都没关系，所以dp\[i][0]=max(dp\[i-1][0],dp\[i-1][1])；
- 假如被选取，那么前一个房屋一定不可以被选取，所以第 i 个房屋值可从第 i-2 个房屋的状态转移过来，即dp\[i][1]=max(dp\[i-2][0],dp\[i-2][1])+nums[i]=dp\[i][0]+nums[i]；

注意：输入vector可能为空



### 代码

```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        int length=nums.size();
        if(length==0) return 0;
        int dp[length+1][2]={0};
        dp[0][0]=0;
        dp[0][1]=nums[0];
        int ans=nums[0];
        for(int i=1;i<length;i++){
            dp[i][0]=max(dp[i-1][1],dp[i-1][0]);
            dp[i][1]=dp[i-1][0]+nums[i];
            ans=max(dp[i][0],dp[i][1]);
        }
        return ans;
    }
};
```