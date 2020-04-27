### 解题思路

dp[i] 代表以 nums[i] 为结尾的子序列的最大值，那么状态转移方程为：

dp[i] = max(nums[i]+lastmax,nums[i]+0)，其中lastmax代表 i 之前 k 个位置最大的 dp[] 值（下面的题目提供了O(n)的计算思路）；0的情况时代表元素 i 为子序列的第一个元素，这种情况是因为前面的 dp[] 值都小于0。

然后同时在每一次遍历时更新结果值的最大值就好啦

### 代码实现

```c++
class Solution {
public:
    int constrainedSubsetSum(vector<int>& nums, int k) {
        deque<int>dequ;
        int dp[100005];
        memset(dp,0,sizeof(dp));
        int len = nums.size();
        dp[0]=nums[0];
        dequ.push_back(dp[0]);
        int ans = nums[0];
        int lastmax=dequ.front();
        for(int i=1;i<k;i++){
            dp[i] = nums[i]+max(0,lastmax);
            ans = max(ans,dp[i]);
            while(!dequ.empty()&&dequ.back()<dp[i]) dequ.pop_back();
            dequ.push_back(dp[i]);
            lastmax = dequ.front();
        }
        
        for(int i=k;i<len;i++){
            dp[i]=nums[i]+max(0,lastmax);
            ans = max(ans,dp[i]);
            if(dequ.front() == dp[i-k]) dequ.pop_front();
            while(!dequ.empty()&&dequ.back()<dp[i]) dequ.pop_back();
            dequ.push_back(dp[i]);
            lastmax=dequ.front();
        }
        return ans;
    }
};
```

leetcode 239题 求解一个数组从左到右每 k 长度的滑动窗口内的最大值。

https://leetcode-cn.com/problems/sliding-window-maximum/

我们的想法是标记可能成为最大值的元素，然后一边更新一边取最大值。借助于双端队列来实现。以nums = [1,3,-1,-3,5,3,6,7], 和 k = 3为例，思想模拟为：

| 遍历 | 操作完队列 | 操作                                | 原因                                                         |
| ---- | ---------- | ----------------------------------- | ------------------------------------------------------------ |
|      | []         |                                     |                                                              |
| i=0  | [1]        | 插入1                               | 队列为空                                                     |
| i=1  | [3]        | ①删去1<br />②插入3                  | ①3>1,1不可能成为最大值<br />②插入新值                        |
| i=2  | [3,-1]     | 插入-1                              | 虽然-1<3，但是-1有可能成为以-1为首的k个值的最大值，因此保留  |
| i=3  | [3,-1,-3]  | 插入-3                              | 虽然-3<-1，但是-3有可能成为以-3为首的k个值的最大值，因此保留 |
| i=4  | [5]        | ①删去3<br />②删去-1，-3<br />③插入5 | ①从上一个状态的 k 个值[i-k+1,i]到当前状态的 k 个值[i-k+2,i+1]，少了nums[i-k+1]，多了nums[i+1]，所以如果上个状态的最大值刚好是nums[i-k+1]，那么就需要删去该值，剔除它的影响<br />②-1<5，-3<5，-1和-3均不可能成为最大值<br />③队列为空，插入新值 |
| i=5  | [5,3]      | 插入3                               | 虽然3<5，但是3有可能成为以3为首的 k 个值得最大值，因此保留   |
| i=6  | [6]        | ①删除5,3<br />②插入6                | ①5<6,3<6，5和3均不可能成为最大值<br />②队列为空，插入新值    |
| i=7  | [7]        | ①删除6<br />②插入7                  | ①6<7，6不可能成为最大值<br />②队列为空，插入新值             |

对于每一次遍历，都构建了一个单调非递增双端队列，双端队列的第一个值都是当前滑动窗口得最大值。

总结来说：

在每一次遍历时，

- 如果上一个状态的最大值等于双向队列的第一个元素，则删除；
- 从队尾移除比当前元素小的值，因为它们不可能成为最大值；
- 插入当前元素;
- 当前队首元素即为当前状态下的最大值

Ps：对于前 k 个元素单独处理一下（不需要第一步和第四步）

### 代码

```C++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int>ans;
        int len = nums.size();
        if(len ==0||k==0) return ans;
        deque<int>dequ;
        for(int i=0;i<k;i++){
            while(!dequ.empty()&&dequ.back()<nums[i]) dequ.pop_back();
            dequ.push_back(nums[i]);
        }
        ans.push_back(dequ.front());
        for(int i=k;i<len;i++){
            if(dequ.front() == nums[i-k]) dequ.pop_front();
            while(!dequ.empty()&&dequ.back()<nums[i]) dequ.pop_back();
            dequ.push_back(nums[i]);
            ans.push_back(dequ.front());
        }
        return ans;
    }
};
```

