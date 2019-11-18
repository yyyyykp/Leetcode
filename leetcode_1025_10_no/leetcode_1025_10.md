### 思路

因为$0 < x < N ，所以N的值最小为1。

- 当 N 为奇数时，N的因子全部为奇数，那么 N-x 一定为偶数，则如果Alice拿到的是奇数，那么Bob得到的一定是偶数，Bob只需要拿 1，轮到Alice的时候一定都是奇数，直至最后剩的值为1，所以Alice必输。
- 所以当 N 为偶数时，Alice只需要先拿 1，将奇数的状态转移给Bob就可以赢了。

### 代码

```c++
class Solution {
public:
    bool divisorGame(int N) {
        if(N%2) return false;
        return true;
    }
};
```