# LeetCode 4-26

#### [15. 三数之和](https://leetcode-cn.com/problems/3sum/)

难度中等2035

给你一个包含 *n* 个整数的数组 `nums`，判断 `nums` 中是否存在三个元素 *a，b，c ，*使得 *a + b + c =* 0 ？请你找出所有满足条件且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

 

**示例：**

```
给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

**大体思路**:

![image-20200426171414613](C:\Users\xuzg\Nutstore\.nutstore_eHpnZ2lzQDEyNi5jb20=\我的坚果云\Typora\image\51-思路)



**代码：**

```c++
vector<vector<int>> threeSum(vector<int>& nums) {
    vector<vector<int>> ans;
    if(nums.size()<3) return ans;
    sort(nums.begin(), nums.end());
    if(nums[0]>0) return ans;
    int i = 0;
    while(i<nums.size()){
        if(nums[i]>0) break;     
        int left = i+1, right = nums.size()-1;
        while(left < right){
            // 转换为long long避免加法过程中溢出
            long long y = static_cast<long long>(nums[i]);
            long long x = static_cast<long long>(nums[left]);
            long long z = static_cast<long long>(nums[right]);
            if(x + y > 0 - z)
                right--;
            else if(x + y <0 - z)
                left++;
            else{
                ans.push_back({nums[i], nums[left], nums[right]});
                // 相同的left和right不应该再次出现，因此跳过
                while(left < right && nums[left] == nums[left+1])
                    left++;
                while(left < right && nums[right] == nums[right-1])
                    right--;
                left++;
                right--;
            }
        }
        // 避免nums[i]作为第一个数重复出现
        while(i+1<nums.size()&&nums[i] == nums[i+1])
            i++;
        i++;
    }
    return ans;
}
```