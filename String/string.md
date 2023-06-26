# Practice again
- [] 28. Find the Index of the First Occurrence in a String


# Key

1. in C, string end up with '\0'
2. in C++, string has size member
1. two pointer



# 344. Reverse String

## must know
1. bit-wise operation
```cpp{.line-numbers}
s[i] ^= s[j];
s[j] ^= s[i];
s[i] ^= s[j];
```



# 541. Reverse String II

## must know
1. reverse(first, end)，not include end => \[first, end)
2. vector.end() point to end of vector. e.g. \vector<int>{1,2,3,end};
3. reverse(vector.begin(), vector.end()) => reverse 0 to end-1
## code
1. (i+k < s.size()) =>小於一倍K，則到K，不到1倍K則到end
```cpp
class Solution
{
public:
    string reverseStr(string s, int k)
    {
        for (int i = 0; i < s.size(); i += 2 * k)
        {
            if (i + k <= s.size())
            {
                reverse(s.begin() + i, s.begin() + i + k);
            }
            else
            {
                reverse(s.begin() + i, s.end());
            }
        }

        return s;
    }
};
```
# replace " " to "%20"
https://github.com/youngyangyang04/leetcode-master/blob/master/problems/%E5%89%91%E6%8C%87Offer05.%E6%9B%BF%E6%8D%A2%E7%A9%BA%E6%A0%BC.md
## approach
1. 由前往後，遇到空格增加兩個空間，用"%20"取代" "，O(n^2)
2. 先找幾個空格，增加2被空格數量的空間，由後往前用two pointer處理，O(n)

# 151. Reverse Words in a String

## must know
1. remove extra space
2. reverse full String
3. reverse every Words

## Code 
```cpp
class Solution {
public:
    void reverse(string& s, int start, int end){ //翻转，区间写法：左闭右闭 []
        for (int i = start, j = end; i < j; i++, j--) {
            swap(s[i], s[j]);
        }
    }

    void removeExtraSpaces(string& s) {//去除所有空格并在相邻单词之间添加空格, 快慢指针。
        int slow = 0;   //整体思想参考https://programmercarl.com/0027.移除元素.html
        for (int i = 0; i < s.size(); ++i) { //
            if (s[i] != ' ') { //遇到非空格就处理，即删除所有空格。
                if (slow != 0) s[slow++] = ' '; //手动控制空格，给单词之间添加空格。slow != 0说明不是第一个单词，需要在单词前添加空格。
                while (i < s.size() && s[i] != ' ') { //补上该单词，遇到空格说明单词结束。
                    s[slow++] = s[i++];
                }
            }
        }
        s.resize(slow); //slow的大小即为去除多余空格后的大小。
    }

    string reverseWords(string s) {
        removeExtraSpaces(s); //去除多余空格，保证单词之间之只有一个空格，且字符串首尾没空格。
        reverse(s, 0, s.size() - 1);
        int start = 0; //removeExtraSpaces后保证第一个单词的开始下标一定是0。
        for (int i = 0; i <= s.size(); ++i) {
            if (i == s.size() || s[i] == ' ') { //到达空格或者串尾，说明一个单词结束。进行翻转。
                reverse(s, start, i - 1); //翻转，注意是左闭右闭 []的翻转。
                start = i + 1; //更新下一个单词的开始下标start
            }
        }
        return s;
    }
};
```


