# LeetCode：4-23

#### [575. 分糖果](https://leetcode-cn.com/problems/distribute-candies/)

给定一个**偶数**长度的数组，其中不同的数字代表着不同种类的糖果，每一个数字代表一个糖果。你需要把这些糖果**平均**分给一个弟弟和一个妹妹。返回妹妹可以获得的最大糖果的种类数。

**示例 1:**

```
输入: candies = [1,1,2,2,3,3]
输出: 3
解析: 一共有三种种类的糖果，每一种都有两个。
     最优分配方案：妹妹获得[1,2,3],弟弟也获得[1,2,3]。这样使妹妹获得糖果的种类数最多。
```

**示例 2 :**

```
输入: candies = [1,1,2,3]
输出: 2
解析: 妹妹获得糖果[2,3],弟弟获得糖果[1,1]，妹妹有两种不同的糖果，弟弟只有一种。这样使得妹妹可以获得的糖果种类数最多。
```

**代码 :**

```c++
int distributeCandies(vector<int>& candies) {
    /**
    	妹妹分得的糖为candies.size()/2颗
    	如果总的糖果种类n > candies.size()/2 ，则妹妹获得糖果种类最多为 candies.size()/2
	   	如果总的糖果种类n < candies.size()/2 ，则妹妹获得糖果种类最多为 n
    	所以本题本质为统计糖果的种类。
    **/
    int nSize = candies.size();
    int canCount = 0 ;
    unordered_map<int , int> unMap;
    for(int i = 0 ; i < nSize ; i++){
        if(unMap.find(candies[i]) != unMap.end())
            continue;
		unMap[candies[i]] = 1;
        canCount++;
        if(canCount >= nSize/2)
        	break;
    }
    return canCount > nSize/2 ? nSize/2 : canCount;
}
```

#### [594. 最长和谐子序列](https://leetcode-cn.com/problems/longest-harmonious-subsequence/)

和谐数组是指一个数组里元素的最大值和最小值之间的差别正好是1。

现在，给定一个整数数组，你需要在所有可能的子序列中找到最长的和谐子序列的长度。

**示例 1:**

```
输入: [1,3,2,2,5,2,3,7]
输出: 5
原因: 最长的和谐数组是：[3,2,2,2,3].
```

**说明:** 输入的数组长度最大不超过20,000.

**代码:**

```c++
int findLHS(vector<int>& nums) {
    /**
    1：记录所有的数字在数组冲出现的次数，存储在hash表中
    2：遍历hash表，res = max(i出现的次数 + （i+1）出现的次数 , res)
    **/
    unordered_map<int , int> unMap;
    int res = 0 ;
    for(int i = 0 ; i < nums.size() ; i++){
        unMap[nums[i]] ++;
    }
    for(auto i:unMap){
        if(unMap.find(i.first+1) != unMap.end())
            res = max(i.second + unMap[i.first + 1] , res);
    }
    return res;
}
```

#### [599. 两个列表的最小索引总和](https://leetcode-cn.com/problems/minimum-index-sum-of-two-lists/)

假设Andy和Doris想在晚餐时选择一家餐厅，并且他们都有一个表示最喜爱餐厅的列表，每个餐厅的名字用字符串表示。

你需要帮助他们用**最少的索引和**找出他们**共同喜爱的餐厅**。 如果答案不止一个，则输出所有答案并且不考虑顺序。 你可以假设总是存在一个答案。

**示例 1:**

```
输入:
["Shogun", "Tapioca Express", "Burger King", "KFC"]
["Piatti", "The Grill at Torrey Pines", "Hungry Hunter Steakhouse", "Shogun"]
输出: ["Shogun"]
解释: 他们唯一共同喜爱的餐厅是“Shogun”。
```

**示例 2:**

```
输入:
["Shogun", "Tapioca Express", "Burger King", "KFC"]
["KFC", "Shogun", "Burger King"]
输出: ["Shogun"]
解释: 他们共同喜爱且具有最小索引和的餐厅是“Shogun”，它有最小的索引和1(0+1)。
```

**代码:**

```c++
vector<string> findRestaurant(vector<string>& list1, vector<string>& list2) {
    vector< string > res;
    /**
    1:遍历第一个字符串数组，将字符串放入hash表中。
    2：遍历第二个字符串数组，在hash表中查找对应字符串
    	如果找到，则判断当前索引和是否更小，更小的话，清空vector 插入当前字符串 
    	                              和当前索引一样大，则直接插入当前字符串
    **/
    unordered_map<string , int> unMap;
    for(int i = 0 ; i < list1.size() ; i++){
        if(unMap.find(list1[i]) == unMap.end())
            unMap[list1[i]] = i;
    }
	int sum = INT_MAX ;// 记录 最小索引之和
    for(int i = 0 ; i < list2.size() ; i++){
        if(unMap.find(list2[i]) != unMap.end()){
            if(sum > i + unMap[list2[i]]){
                sum = i + unMap[list2[i]];
                res.clear();
                res.push_back(list2[i]);
            }else if(sum == i + unMap[list2[i]]){
                res.push_back(list2[i]);
            }
        }
    }
    return res;
}
```

#### [645. 错误的集合](https://leetcode-cn.com/problems/set-mismatch/)

集合 `S` 包含从1到 `n` 的整数。不幸的是，因为数据错误，导致集合里面某一个元素复制了成了集合里面的另外一个元素的值，导致集合丢失了一个整数并且有一个元素重复。

给定一个数组 `nums` 代表了集合 `S` 发生错误后的结果。你的任务是首先寻找到重复出现的整数，再找到丢失的整数，将它们以数组的形式返回。

**示例 1:**

```
输入: nums = [1,2,2,4]
输出: [2,3]
```

**注意:**

1. 给定数组的长度范围是 [2, 10000]。
2. 给定的数组是无序的。

**代码:**

```c++
vector<int> findErrorNums(vector<int>& nums) {
    /**
    	遍历数组，将数字出现次数记录在hashmap中，超过1个的数字插入res中。
    	从1遍历到n，在hashmap中找对应值，找不到则将数字插入到res中
    **/
    vector<int> res;
    unordered_map<int , int> unMap;
    for(int i = 0 ; i < nums.size() ; i++){
        unMap[nums[i]]++;
        if(unMap[nums[i]] == 2)
            res.push_back(nums[i]);
    }
    for(int i = 1; i <= nums.size() ; i++){
        if(unMap.find(i) == unMap.end()){
            res.push_back(i);
            break;
        }
    }
    return res;
}
```

#### [690. 员工的重要性](https://leetcode-cn.com/problems/employee-importance/)

给定一个保存员工信息的数据结构，它包含了员工**唯一的id**，**重要度** 和 **直系下属的id**。

比如，员工1是员工2的领导，员工2是员工3的领导。他们相应的重要度为15, 10, 5。那么员工1的数据结构是[1, 15, [2]]，员工2的数据结构是[2, 10, [3]]，员工3的数据结构是[3, 5, []]。注意虽然员工3也是员工1的一个下属，但是由于**并不是直系**下属，因此没有体现在员工1的数据结构中。

现在输入一个公司的所有员工信息，以及单个员工id，返回这个员工和他所有下属的重要度之和。

**示例 1:**

```
输入: [[1, 5, [2, 3]], [2, 3, []], [3, 3, []]], 1
输出: 11
解释:
员工1自身的重要度是5，他有两个直系下属2和3，而且2和3的重要度均为3。因此员工1的总重要度是 5 + 3 + 3 = 11。
```

**注意:**

1. 一个员工最多有一个**直系**领导，但是可以有多个**直系**下属
2. 员工数量不超过2000。

**代码:**

```c++
int getImport(Employee* em , unordered_map<int , Employee*>& uMap){
    //递归方式放下级累加所有的重要度
    int res = em->importance;
    for(int i = 0 ; i < em->subordinates.size() ; i++)
        res += getImport(uMap[em->subordinates[i]] , uMap);
    return res;
}
int getImportance(vector<Employee*> employees, int id) {
    //遍历employee ，根据员工ID存储hashmap 
    unordered_map<int , Employee*> unMap;
    Employee* tmp = NULL;
    for(int i = 0 ; i < employees.size() ; i++){
        unMap[employees[i]->id] = employees[i];
        if(employees[i]->id == id)
            tmp = employees[i];
    }
    if(tmp == NULL)
        return 0;
    return getImport(tmp , unMap);
}
```

