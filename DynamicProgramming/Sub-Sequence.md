# Do Again
- 115
- 583
- 712
- 673

# 300

Doesn't need to be a continuous Increasing sub sequence. 

j<i
1. when nums[i] > nums[j], dp[i] = max(dp[i], dp[j]+1)

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        if (nums.size() <= 1) return nums.size();
        vector<int> dp(nums.size(), 1);
        int result = 0;
        for (int i = 1; i < nums.size(); i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) dp[i] = max(dp[i], dp[j] + 1);
            }
            if (dp[i] > result) result = dp[i]; // 取长的子序列
        }
        return result;
    }
};

```

# 674

Needs to be a continuous increasing sub sequence

```cpp
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        if (nums.size() == 0) return 0;
        int result = 1;
        vector<int> dp(nums.size() ,1);
        for (int i = 1; i < nums.size(); i++) {
            if (nums[i] > nums[i - 1]) { // 连续记录
                dp[i] = dp[i - 1] + 1;
            }
            if (dp[i] > result) result = dp[i];
        }
        return result;
    }
};
```

# 718

Longest continuous sub sequence in two array.
dp[i] means nums1[0] to nums1[i - 1] longest continuous sub sequence.
dp[j] means nums2[0] to nums2[j - 1] longest continuous sub sequence.
Tow array combine into one 2D array dp[i][j]
dp[i][j] means the status of nums1[i] and nums2[j]

```cpp
nums1[i-1] == nums2[j-1] 
```
This means nums[i-1] has same number as nums2[j-1], then dp[i][j] = dp[i-1][j-1]+1
```cpp
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        vector<vector<int>> dp (nums1.size() + 1, vector<int>(nums2.size() + 1, 0));
        int result = 0;
        for (int i = 1; i <= nums1.size(); i++) {
            for (int j = 1; j <= nums2.size(); j++) {
                if (nums1[i - 1] == nums2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                }
                if (dp[i][j] > result) result = dp[i][j];
            }
        }
        return result;
    }
};
```
Longest non continuous sub sequence in two array.

```cpp
if (text1[i - 1] == text2[j - 1]) {
	dp[i][j] = dp[i - 1][j - 1] + 1;
	}
```
If text1[i - 1] equals to text2[j-1], means two texts have same text at i-1 and j-1

```cpp
else {
	dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
}
```
If text1[i-1] is not equals to text2[j-1], find maximum continuous subsequence between text1[i-1][j] and text2[i][j-1]

# 1143

```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        vector<vector<int>> dp(text1.size() + 1, vector<int>(text2.size() + 1, 0));
        for (int i = 1; i <= text1.size(); i++) {
            for (int j = 1; j <= text2.size(); j++) {
                if (text1[i - 1] == text2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[text1.size()][text2.size()];
    }
};s
```

# 1035

Exactly the same with #1143
Not crossing line means common continuous subsequence with same order

```cpp
class Solution {
public:
    int maxUncrossedLines(vector<int>& A, vector<int>& B) {
        vector<vector<int>> dp(A.size() + 1, vector<int>(B.size() + 1, 0));
        for (int i = 1; i <= A.size(); i++) {
            for (int j = 1; j <= B.size(); j++) {
                if (A[i - 1] == B[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[A.size()][B.size()];
    }
};
```

# 53

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        if (nums.size() == 0) return 0;
        vector<int> dp(nums.size());
        dp[0] = nums[0];
        int result = dp[0];
        for (int i = 1; i < nums.size(); i++) {
            dp[i] = max(dp[i - 1] + nums[i], nums[i]); // 状态转移公式
            if (dp[i] > result) result = dp[i]; // result 保存dp[i]的最大值
        }
        return result;
    }
};
```

# 392

String __s__ represents subsequence and i for iteration.
String __t__ represents original sequence and j for iteration.

If s[i-1] is equal to t[j-1]. It means two string have same character.
If s[i-1] is not equal to t[j-1]. String t neads to remove t[j - 1]. Current length is dp[j-2], so dp[i][j] = dp[i][j-1].
If longest length equal to __s__ length, __s__ is subsequence of __t__

!Initialize!

```cpp
class Solution {
public:
    bool isSubsequence(string s, string t) {
        vector<vector<int>> dp(s.size() + 1, vector<int>(t.size() + 1, 0));
        for (int i = 1; i <= s.size(); i++) {
            for (int j = 1; j <= t.size(); j++) {
                if (s[i - 1] == t[j - 1]) dp[i][j] = dp[i - 1][j - 1] + 1;
                else dp[i][j] = dp[i][j - 1];
            }
        }
        if (dp[s.size()][t.size()] == s.size()) return true;
        return false;
    }
};
```
# 115

- Initialize
	string __s__ dp[i][[0] initialize to one
	target __t__ dp[0][j] initialize to zero

- If s[i - 1] is equal to t[j-1] => __s__ and __t__ previous situation(i-2,j-2) + __s__ previous(i-2,j-1)

```cpp
dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
```

- If s[i - 1] is not equal to t[j-1] => previous __s__ situation(i-2,j-1)

```cpp
dp[i][j] = dp[i - 1][j];
```


```cpp
class Solution {
public:
    int numDistinct(string s, string t) {
        vector<vector<uint64_t>> dp(s.size() + 1, vector<uint64_t>(t.size() + 1));
        for (int i = 0; i < s.size(); i++) dp[i][0] = 1;
        for (int j = 1; j < t.size(); j++) dp[0][j] = 0;
        for (int i = 1; i <= s.size(); i++) {
            for (int j = 1; j <= t.size(); j++) {
                if (s[i - 1] == t[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
                } else {
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }
        return dp[s.size()][t.size()];
    }
};
```

# 583

## Method 1

dp[i][j] means total words that need to be deleted at word1[0:i-1] and word2[0:j-1]
If word1[i-1] equals to word2[j-1], dp[i][j] = dp[i-1][j-1]. No words needs to be deleted at this situation.
If word1[i-1] is not equals to word2[j-1]. Find minimum between two(three) situataion.
- word1[i-1] situation plus current => dp[i-1][j] + 1
- word2[j-1] situation plus current => dp[i][j-1] + 1
- remove both word and plus 2 //duplicated with above situations

```cpp
class Solution
{
public:
    int minDistance(string word1, string word2)
    {
        vector<vector<int>> dp(word1.size() + 1, vector<int>(word2.size() + 1, 0));
        for (int i = 0; i <= word1.size(); i++)
            dp[i][0] = i;
        for (int j = 0; j <= word2.size(); j++)
            dp[0][j] = j;
        for (int i = 1; i <= word1.size(); i++)
        {
            for (int j = 1; j <= word2.size(); j++)
            {
                if (word1[i - 1] == word2[j - 1])
                {
                    dp[i][j] = dp[i - 1][j - 1];
                }
                else
                {
                    dp[i][j] = min(dp[i - 1][j] + 1, dp[i][j - 1] + 1);
                }
            }
        }
        return dp[word1.size()][word2.size()];
    }
};
```

## Method 2

Almost the same with __1143__
total size substract longest common sub sequence

```cpp
class Solution
{
public:
    int minDistance(string word1, string word2)
    {
        vector<vector<int>> dp(word1.size() + 1, vector<int>(word2.size() + 1, 0));

        for (int i = 1; i <= word1.size(); i++)
        {
            for (int j = 1; j <= word2.size(); j++)
            {
                if (word1[i - 1] == word2[j - 1])
                {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                }
                else
                {
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return word1.size() + word2.size() - dp[word1.size()][word2.size()] * 2;
    }
};
```

# 712 

Same idea with #583. Change word need to be removed into its ASCII code.

```cpp
class Solution {
public:
    int minimumDeleteSum(string s1, string s2) {
        vector<vector<int>>dp(s1.size()+1,vector<int>(s2.size()+1,0));
        for(int i = 1;i<=s1.size();i++)
        {
            dp[i][0] = dp[i-1][0] + s1[i-1];
        }
        for(int j = 1;j<=s2.size();j++)
        {
            dp[0][j] = dp[0][j-1] + s2[j-1];
        }
        for(int i = 1; i<= s1.size();i++)
        {
            for(int j = 1; j<= s2.size();j++)
            {
                if(s1[i-1]==s2[j-1])
                {
                    dp[i][j] = dp[i-1][j-1];
                }
                else
                {
                    dp[i][j] = min(dp[i-1][j] + s1[i-1],dp[i][j-1]+s2[j-1]);
                }
            }
        }
        return dp[s1.size()][s2.size()];
    }
};
```

# 72

Similar with 583

- If word1[i-1] equalts to word2[j-1], find previous status of each word(i-2,j-2)
- IF word2[i-1] is not equal to word2[j-1], there are three actions to do. __Delete__ a word, __add__ a word and __replace__ word. 

Find minimum from these three action.
1. Delete word1[i-1] so dp[i][j] is=> dp[i][j] = dp[i-1][j] 
2. Add a word to word1[i-1], this action is equal to delete a word from word2[j-1]=> dp[i][j] = dp[i][j-1]
3. Replace a word from one of the word string is equal to previous status of two words(dp[i-1][j-1]) plus one.

```cpp
    int minDistance(string word1, string word2)
    {
        vector<vector<int>> dp(word1.size() + 1, vector<int>(word2.size() + 1, 0));
        for (int i = 0; i <= word1.size(); i++)
            dp[i][0] = i;
        for (int j = 0; j <= word2.size(); j++)
            dp[0][j] = j;

        for (int i = 1; i <= word1.size(); i++)
        {
            for (int j = 1; j <= word2.size(); j++)
            {
                if (word1[i - 1] == word2[j - 1])
                {
                    dp[i][j] = dp[i - 1][j - 1];
                }
                else
                {
                    dp[i][j] = min({dp[i - 1][j - 1], dp[i - 1][j], dp[i][j - 1]}) + 1;
                }
            }
        }
        return dp[word1.size()][word2.size()];
    }
```

# 647

dp[i][j] represents interval between i and left is palindromic or not.
__i__ is left pointer and __r__ is right pointer
If j - i <= 1 means aa or a, this interval is palindromic.
If j - i > 1 means aba or cbacc, then left pointer goes right and right pointer goes left to check dp[i+1][j-1] is palindrmoic or not.
Normal 2d array dp[s.size() - 1][s.size()-1]

```cpp
class Solution {
public:
    int countSubstrings(string s) {
        vector<vector<bool>> dp(s.size(), vector<bool>(s.size(), false));
        int result = 0;
        for (int i = s.size() - 1; i >= 0; i--) {  // 注意遍历顺序
            for (int j = i; j < s.size(); j++) {
                if (s[i] == s[j]) {
                    if (j - i <= 1) { // 情况一 和 情况二
                        result++;
                        dp[i][j] = true;
                    } else if (dp[i + 1][j - 1]) { // 情况三
                        result++;
                        dp[i][j] = true;
                    }
                }
            }
        }
        return result;
    }
};
```

# 518

__i__ represents left pointer, __j__ represents right pointer
If s[i] == s[j], current interval length = dp[i+1][j-1] + 2
If s[i] != s[j], find maximum between dp[i+1][j] and dp[i][j-1] 

```cpp
class Solution
{
public:
    int longestPalindromeSubseq(string s)
    {
        vector<vector<int>> dp(s.size(), vector<int>(s.size(), 0));
        for (int i = 0; i < s.size(); i++)
        {
            dp[i][i] = 1;
        }
        for (int i = s.size() - 1; i >= 0; i--)
        {
            for (int j = i + 1; j < s.size(); j++)
            {
                if (s[i] == s[j])
                {
                    dp[i][j] = dp[i + 1][j - 1] + 2;
                }
                else
                {
                    dp[i][j] = max(dp[i + 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[0][s.size() - 1];
    }
};
```

#673
Sample to understand
10,20,30,30,60,60,70,1,2,3,3,6,6,7

Extend from #300

__dp[i]__ represents nums[0:i] max increase sub sequence

__count[i]__ represents sets of max sub sequence at dp[i]

result counts __counts[i]__ when ```dp[i] == maxCount```
```cpp
    int findNumberOfLIS(vector<int>& nums) {
	if (nums.size() <= 1) return nums.size();
	vector<int> dp(nums.size(), 1);
	vector<int> count(nums.size(), 1);
	int maxCount = 0;
	for (int i = 1; i < nums.size(); i++) {
		for (int j = 0; j < i; j++) {
			if (nums[i] > nums[j]) {
				if (dp[j] + 1 > dp[i]) {
					dp[i] = dp[j] + 1;
					count[i] = count[j];
				} else if (dp[j] + 1 == dp[i]) {
					count[i] += count[j];
				}
			}
			if (dp[i] > maxCount) maxCount = dp[i];
		}
	}
	int result = 0;
	for (int i = 0; i < nums.size(); i++) {
		if (maxCount == dp[i]) result += count[i];
	}
	return result;
    }
```