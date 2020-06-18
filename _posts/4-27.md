# LeetCode 4-27

#### [30. 串联所有单词的子串](https://leetcode-cn.com/problems/substring-with-concatenation-of-all-words/)

给定一个字符串 **s** 和一些长度相同的单词 **words。**找出 **s** 中恰好可以由 **words** 中所有单词串联形成的子串的起始位置。

注意子串要与 **words** 中的单词完全匹配，中间不能有其他字符，但不需要考虑 **words** 中单词串联的顺序。

 

**示例 1：**

```
输入：
  s = "barfoothefoobarman",
  words = ["foo","bar"]
输出：[0,9]
解释：
从索引 0 和 9 开始的子串分别是 "barfoo" 和 "foobar" 。
输出的顺序不重要, [9,0] 也是有效答案。
```

**示例 2：**

```
输入：
  s = "wordgoodgoodgoodbestword",
  words = ["word","good","best","word"]
输出：[]
```

**代码：**

```c++
vector<int> findSubstring(string s, vector<string>& words) {
     //特殊情况直接排除
    vector<int> result;
    if(s.empty()||words.empty())return result;
    //单词数组中的单词的大小，个数，以及总长度
    int one_word=words[0].size() , word_num=words.size() , all_len=one_word*word_num , s_len = s.size();
    //建立单词->单词个数的映射
    unordered_map<string,int> m1;
    for(const auto& w:words)
        m1[w]++;
    if(all_len > s_len)
        return result;
    for(int i = 0 ; i <= s_len - all_len ; i++){
        int count = word_num , left = i;
        unordered_map<string , int> m2;
        bool flag = true;
        while(count > 0 ){
            count--;
            string tmp = s.substr(left , one_word);
            left += one_word;
            // 不匹配 则跳出循环 设置flag
            if(m1.find(tmp) == m1.end() || m2[tmp] > m1[tmp]){
                flag = false;
                break;
            }
            m2[tmp]++ ;
        }
        if(flag){
			// 匹配完所有的字符串 ， 比较两个hashmap 是否相等
            for(auto i : m2){
                if(i.second != m1[i.first]){
                    flag = false;
                    break;
                }
            }
            if(flag)
                result.push_back(i);
       }
    }
    return result;
}


vector<int> findSubstring(string s, vector<string>& words) {
	//特殊情况直接排除
    if(s.empty()||words.empty())return {};
    //存放结果的数组
    vector<int> result;
    //单词数组中的单词的大小，个数，以及总长度
    int one_word=words[0].size();
    int word_num=words.size();
    int all_len=one_word*word_num;
    if(s.size()  < all_len )
        return result;
    //建立单词->单词个数的映射
    unordered_map<string,int> m1;
    for(const auto& w:words)m1[w]++;
    for(int i=0;i<one_word;++i)
    {
        //left和rigth表示窗口的左右边界，count用来统计匹配的单词个数
        int left=i,right=i,count=0;
        unordered_map<string,int>m2;
        //开始滑动窗口
        while(right+one_word<=s.size())
        {
            //从s中提取一个单词拷贝到w
            string w=s.substr(right,one_word);
            right+=one_word;//窗口右边界右移一个单词的长度
            if(m1.count(w)==0){
                //此单词不在words中，表示匹配不成功,然后重置单词个数、窗口边界、以及m2
                count=0;
                left=right;
                m2.clear();
            }
            else{//该单词匹配成功，添加到m2中
                m2[w]++;
                count++;    
                while(m2.at(w)>m1.at(w)){//一个单词匹配多次，需要缩小窗口，也就是left右移
                    string t_w=s.substr(left,one_word);
                    count--;
                    m2[t_w]--;
                    left+=one_word;
                }
                if(count==word_num)result.push_back(left);
            }
        }
    }
    return result;
}
```