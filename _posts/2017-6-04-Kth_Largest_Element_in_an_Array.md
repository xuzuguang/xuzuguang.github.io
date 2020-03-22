---

layout: 	post
title:  	"数组中的第K个最大元素解题思路"
date:   	2017-06-04 22:14
categories: 	算法 
tags: 	算法  
excerpt:	数组中的第K个最大元素
mathjax: 	true

---

[TOC]

# 数组中的第K个最大元素

## 概述

> 在未排序的数组中找到第 k 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。
>
> 示例 1:
>
> 输入: [3,2,1,5,6,4] 和 k = 2
> 输出: 5
>
> 示例 2:
>
> 输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
> 输出: 4

## 排序解法

> 思路：首先对数据进行排序，排序后遍历找到第K个最大的元素。
>
> 算法时间复杂度为O(N logN),空间复杂度为O(1)



## 最大堆解法

>  思路：创建一个最小堆，将数组中元素加入堆中，并保持堆的大小为K。
>
> 时间复杂度为O(N logK)
>
> 这种解法：针对其他数据结构依然可以用。比如，将数据换成链表或者二叉树。
>
> 主要问题：c++中只有最大堆  没有最小堆  所以 关于构建最小堆为关键

````c++
    int findKthLargest(vector<int>& nums, int k) {
        // 使用最小堆
        priority_queue<int, vector<int>, greater<int>> pq;
        for(int i = 0 ; i < nums.size() ;i++){
            if (pq.size() >= k && pq.top() >= nums[i]) 
                continue;
            pq.push(nums[i]);
            if (pq.size() > k)
                pq.pop();
        }
        return pq.top();
    }
````

### priority_queue 使用

> Priority_queue ，优先队列 。在优先队列中，优先级高的元素先出队列，也就是元素是按照从大到小的顺序输出。
>
> 那么，如果想将想将优先队列改为从小到大输出呢？ 两种方式：

1. priority_queue<int, vector<int>,greater<int> > qq1;

````c++
priority_queue<int> qq;
qq.push(2);
qq.push(4);
qq.push(6);
qq.push(5);
while(!qq.empty()){
    std::cout<< qq.top() << " ";
    qq.pop();
}
std::cout<<std::endl;
// 其输出结果为： 6 5 4 2

// 这么写  优先队列为从小到大输出
// 如果想 从大到小输出，可以将greater改为less
priority_queue<int, vector<int>,greater<int> > qq1;
qq1.push(2);
qq1.push(4);
qq1.push(6);
qq1.push(5);
while(!qq1.empty()){
    std::cout<< qq1.top() << " ";
    qq1.pop();
}
std::cout<<std::endl;
// 其输出结果为： 2 4 5 6
````

2. 针对自定义数据类型

````c++
struct node {
    friend bool operator< (node& n1 , node& n2){
        return n1.value > n2.value;
    }
    int value;
    node(int i = 0):value(i){}
};
std::priority_queue<node> qq;
node tmp1(2);
node tmp2(1);
node tmp3(5);
node tmp4(3);
qq.push(tmp1);
qq.push(tmp2);
qq.push(tmp3);
qq.push(tmp4);
while(!qq.empty()){
    std::cout<<qq.top()<<" ";
    qq.pop().value;
}
std::cout<<std::endl;
// 其输出结果为： 2 4 5 6
````



