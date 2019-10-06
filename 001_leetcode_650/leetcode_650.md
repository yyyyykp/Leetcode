假设每一次粘贴的次数是t1,t2,t3,....，那么得到的最小操作次数为$1\times (t1+1)\times (t2+1)\times (t3+1)$.

我们对n做质因子分解，以$30=2\times 3\times 5$为例，分解为3项代表copy paste paste...的整体操作为3次。即第一次操作完成得到数目为2（对1 copy一次，paste一次），第二次操作完成后得到的数目为$2\times 3=6$（对2 copy一次，paste两次）,第三次操作完成后得到的数目为$2\times 3\times 5=30$（对6 copy 一次，paste 四次），即$30=1\times(1+1)\times (1+2)\times (1+4)$。



```c++
class Solution
{
public:
    int minSteps(int n)
    {
        int ans=0;
        int i=2;
        while(i<=n)
        {
            if(n%i==0)
            {
                while(n%i==0)
                {
                    n/=i;
                    ans+=i;
                }
            }
            ++i;
        }
        return ans;
    }
};
```