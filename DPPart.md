## DP Part

### 416 Partition Equal Subset Sum

Given a **non-empty** array containing **only positive integers**, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

**Note:**

1. Each of the array element will not exceed 100.
2. The array size will not exceed 200.



**Example 1:**

```
Input: [1, 5, 11, 5]

Output: true

Explanation: The array can be partitioned as [1, 5, 5] and [11].
```



**Example 2:**

```
Input: [1, 2, 3, 5]

Output: false

Explanation: The array cannot be partitioned into equal sum subsets.
```

```java
class Solution {
    public boolean canPartition(int[] nums) {
        if(nums == null || nums.length == 0 || nums.length == 1) return false;
        int sum = 0;
        for(int i = 0; i < nums.length; i++) {
            sum += nums[i];
        }
        if(sum%2 == 1) return false;
        boolean[][] dp = new boolean[nums.length+1][sum/2+1];
        dp[0][0] = true;
        for(int i = 1; i < nums.length+1; i++) {
            dp[i][0] = true;
        }
        for(int j = 1; j < sum/2+1; j++) {
            dp[0][j] = false;
        }
        for(int i = 1; i < nums.length+1; i++) {
            for(int j = 1; j < sum/2+1; j++) {
                dp[i][j] = dp[i-1][j];
                if(j >= nums[i-1]) {
                    dp[i][j] = (dp[i][j] || dp[i-1][j-nums[i-1]]);
                }
            }
        }
        return dp[nums.length][sum/2];
    }
}
```

time complexity O(sum*nums.length)

space complexity O(sum*nums.length)





can optimize the space complexity

```java
	public boolean canPartition(int[] nums) {
    int sum = 0;
    
    for (int num : nums) {
        sum += num;
    }
    
    if ((sum & 1) == 1) {
        return false;
    }
    sum /= 2;
    
    int n = nums.length;
    boolean[] dp = new boolean[sum+1];
    Arrays.fill(dp, false);
    dp[0] = true;
    
    for (int num : nums) {
        for (int i = sum; i > 0; i--) {
            if (i >= num) {
                dp[i] = dp[i] || dp[i-num];
            }
        }
    }
    
    return dp[sum];
}
```

## 5 Longest Palindromic String

```java
class Solution {
    public String longestPalindrome(String s) {
        if(s == null || s.length() == 0) return "";
        int len = s.length();
        boolean[][] dp = new boolean[len][len];
        int[] res = new int[2]; int max = 0;
        for(int i = 0; i < len; i++) {
            for(int j = 0; j < len; j++) {
                if(i == 0) {
                    dp[j][j] = true;
                }else if(i == 1 && i+j < len) {
                    dp[j][i+j] = (s.charAt(j) == s.charAt(j+1));
                }else if(i+j < len) {
                    dp[j][i+j] = dp[j+1][i+j-1] && (s.charAt(j) == s.charAt(i+j));
                }
                if(i+j < len && dp[j][i+j] && i > max) {
                    max = i;
                    res[0] = j; res[1] = i+j;
                }
            }
        }
        return s.substring(res[0], res[1]+1);
    }
}
```

Start from the center

```java
public String longestPalindrome(String s) {
    if (s == null || s.length() < 1) return "";
    int start = 0, end = 0;
    for (int i = 0; i < s.length(); i++) {
        int len1 = expandAroundCenter(s, i, i);
        int len2 = expandAroundCenter(s, i, i + 1);
        int len = Math.max(len1, len2);
        if (len > end - start) {
            start = i - (len - 1) / 2;
            end = i + len / 2;
        }
    }
    return s.substring(start, end + 1);
}

private int expandAroundCenter(String s, int left, int right) {
    int L = left, R = right;
    while (L >= 0 && R < s.length() && s.charAt(L) == s.charAt(R)) {
        L--;
        R++;
    }
    return R - L - 1;
}
```

