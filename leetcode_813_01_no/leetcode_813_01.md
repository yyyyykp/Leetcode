### 思路

以数组下标从0开始的前提讨论。

令dp\[i][k]代表前 i+1 个数分成 k 段，那么它的值可以由 **前 j (k-2<j<i-1})个元素分成 k-1 段** + **j+1 ~ i 作为一段** 转化得到。

### 代码

```c++
class Solution {
public:
    double largestSumOfAverages(vector<int>& A, int K) {
        int len = A.size();
        double sum[len+1];
        double dp[len+1][K+1];
        memset(dp,0,sizeof(dp));
        memset(sum,0,sizeof(sum));
        sum[0]=A[0];
        dp[0][1]=A[0];
        for(int i=1;i<len;i++){
            sum[i]=sum[i-1]+A[i];
            dp[i][1]=sum[i]/(i+1);
        }
        for(int i=1;i<len;i++){
            for(int k=2;k<=K;k++){
                for(int j=k-2;j<i;j++){
                    dp[i][k] = max(dp[i][k],dp[j][k-1]+(sum[i]-sum[j])/(i-j));
                }
            }
        }
        return dp[len-1][K];
    }
};
```

上述代码数组sum,dp下标是从0开始的，改成从1开始代码会更简洁一丢丢

```c++
class Solution {
public:
    double largestSumOfAverages(vector<int> &A, int K) {
        int len = A.size();
        double sum[len + 1];
        double dp[len + 1][K + 1];
        memset(dp, 0, sizeof(dp));
        memset(sum, 0, sizeof(sum));
        for (int i = 0; i < len; i++) {
            sum[i + 1] = sum[i] + A[i];
            dp[i + 1][1] = sum[i + 1] / (i + 1);
        }
        for (int k = 2; k <= K; k++) {
            for (int i = 2; i <= len; i++) {
                dp[i][k % 2] = 0;  //因为滚动的是 k 相关，要保证每一次更新之前抹去旧的数值，所以需要将k和i循环的顺序变一下
                for (int j = k - 1; j < i; j++) {
                    dp[i][k % 2] = max(dp[i][k % 2], dp[j][(k + 1) % 2] + (sum[i] - sum[j]) / (i - j));
                }
            }
        }
        return dp[len][K % 2];
    }
};
```

接下来的空间优化是从官方题解理解整理的：

- 空间复杂度优化①：

​		从上边的代码可以看出，dp\[i][k]的状态是从dp\[j][k-1]（j<i）转化而来的，所以数组的第二维便可以压缩成大小为2。

```c++
class Solution {
public:
    double largestSumOfAverages(vector<int>& A, int K) {
        int len = A.size();
        double sum[len+1];
        double dp[len+1][2];
        memset(dp,0,sizeof(dp));
        memset(sum,0,sizeof(sum));
        for(int i=0;i<len;i++){
            sum[i+1]=sum[i]+A[i];
            dp[i+1][1]=sum[i+1]/(i+1);
        }
        for(int i=2;i<=len;i++){
            for(int k=2;k<=K;k++){
                for(int j=k-1;j<i;j++){
                    dp[i][k%2] = max(dp[i][k%2],dp[j][(k-1)%2]+(sum[i]-sum[j])/(i-j));
                }
            }
        }
        cout<<dp[len][0]<<" "<<dp[len][1]<<endl;
        return dp[len][K%2];
    }
};
```



- 空间复杂度优化②：

​		考虑将数组优化成一维，由优化①来看，j<i 说明在计算 i 的状态时需要比 i 小的状态即dp\[j]\[](j<i)，换句话说，就是由前边的状态往后更新，这也就是为什么需要标记 k-1 对应的状态。

​		因此将状态转移策略转化为由后边状态往前更新，对应的思路是 dp(i,k) 代表从第 i 个元素到最后一个元素分成 k 段，那么它的状态转移策略便是：dp(i,k)=max(dp(i,k),dp(j,k-1)+average(i,j-1))，j>i。因为 i 比 j 小，所以在更新 i 对应的状态时，不会覆盖掉 j 也就是后边的状态，因此只用一维数组就可以了。

```c++
class Solution {
public:
    double largestSumOfAverages(vector<int>& A, int K) {
        int len = A.size();
        double sum[len+1];
        double dp[len+1];
        memset(dp,0,sizeof(dp));
        memset(sum,0,sizeof(sum));
        for(int i=0;i<len;i++){
            sum[i+1]=sum[i]+A[i];
        }
        for(int i=0;i<len;i++){
            dp[i]=(sum[len]-sum[i])/(len-i);
        }
        for(int k=2;k<=K;k++) {
            for(int i=0;i<len;i++){
                for(int j=i+1;j<len;j++){
                    dp[i]=max(dp[i],dp[j]+(sum[j]-sum[i])/(j-i));
                }
            }
        }
        return dp[0];
    }
};
```

