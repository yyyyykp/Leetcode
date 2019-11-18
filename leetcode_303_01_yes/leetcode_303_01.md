### 思路

用dp[i]来维护第 i 位的前缀和，那么(i,j)=dp[j]-dp[i]

### 代码

```c++
class NumArray {
private:
    int length;
    int dp[10000]={0};

public:
    NumArray(vector<int>& nums) {
        length=nums.size();
        if(length == 0) return ;
        dp[0]=nums[0];
        for(int i=1;i<length;i++){
            dp[i]=dp[i-1]+nums[i];
        }
    }
    
    int sumRange(int i, int j) {
        return dp[j]-(i-1>=0?dp[i-1]:0);
    }
};
```