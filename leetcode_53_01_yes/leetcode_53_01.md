### 思路

用sum来维护当前的和，如果加上当前数和为负数时说明sum对接下来的数的和没有贡献，将sum置为0接着遍历，并更新ans. 

### 代码

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int length=nums.size();
        int ans=0,sum=0,num=nums[0];
        for(int i=0;i<length;i++){
            num=max(num,nums[i]);
            if(sum>=0)
                sum+=nums[i];
            else
                sum=nums[i];
            ans=max(sum,ans);
        }
        if(ans<= 0) return num;
        return ans;
    }
};
```