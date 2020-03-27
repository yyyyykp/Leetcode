### 思路

先假设没有同身高的情况存在，那么思路就是：按照身高值从小到大排序，然后对排序后得到的体重值序列求最长上升子序列的长度，考虑到时间复杂度，需要用lower_bound()优化达到O(nlogn)；

再考虑有同身高的情况，因为同一个身高的人只能选一名，所以同身高的人应该对体重从大到小排序。可以这样想，因为同身高的人体重是降序的，所以在最长上升子序列的选择中，不会有两个身高相同的人被同时选中（因为是降序，同身高内部无法构成上升子序列）。

### 代码

```c++
bool cmp(pair<int, int> x, pair<int, int> y)
{
    if (x.first == y.first)
        return x.second > y.second;
    return x.first < y.first;
}
class Solution
{
public:
    int bestSeqAtIndex(vector<int> &height, vector<int> &weight)
    {
        int dp[10007];
        int s = height.size();
        vector<pair<int, int>> person(s, make_pair(0, 0));
        for (int i = 0; i < s; i++)
        {
            person[i].first = height[i];
            person[i].second = weight[i];
        }
        sort(person.begin(), person.end(), cmp);
        memset(dp,0x3f3f3f3f,sizeof(dp));
        int ans = 0;
        for(int i=0;i<s;i++)
        {
            *lower_bound(dp,dp+s+1,person[i].second)=person[i].second;
        }
        return lower_bound(dp,dp+s+1,0x3f3f3f3f)-dp;
    }
};
```