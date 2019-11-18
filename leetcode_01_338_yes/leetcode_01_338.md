### 思路

0 | 1 | 1 2 | 1 2 2 3 | 1 2 2 3 2 3 3 4 |1 2 2 3 2 3 3 4 2 3 3 4 3 4 4 5|……

通过观察，以1 2 2 3 -> 1 2 2 3 2 3 3 4 为例，先粘贴一次 1 2 2 3，然后粘贴1223的后一半23，然后粘贴23的后一半3，最后再添加一个4（最后添加的数字，我们用一个变量单独标记加在vector中）。

时间复杂度：O(n)

### 代码

```c++
class Solution
{
public:
    vector<int> countBits(int num)
    {
        vector<int>vv;
        vv.push_back(0);
        if(num==0) return vv;
        vv.push_back(1);
        if(num==1) return vv;
        int countt=1,t=1,id=1,tempnum=2;
        while(true)
        {
            int cc=countt;
            while(cc>=1)
            {
                for(int i=id; i<id+cc; i++)
                {
                    vv.push_back(vv[i]);
                    ++tempnum;
                    if(tempnum>num) return vv;
                }
                cc/=2;
                id+=cc;
            }
            vv.push_back(++t);
            ++tempnum;
            if(tempnum>num) return vv;
            ++id;
            countt*=2;
        }
//        cout<<"*****";
//        for(int i=0; i<vv.size(); i++)
//            cout<<vv[i]<<" ";
//        cout<<"*****"<<endl;
        return vv;
    }
};
```