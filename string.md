# 字符串
<!-- GFM-TOC -->
* [参见字符串](https://docs.google.com/document/d/1BcjvBvmOlHtKoHfxKAHunLw2Oedu-s2tSBTsltmx-e0/edit?tab=t.0#heading=h.zc4m9ltemerb)
    * [1. 796 Roate String ](#1-字符串循环移位包含) Roate String (E) [Leetcode](https://leetcode.com/problems/rotate-string/description/?source=submission-noac)
    * [2. 字符串循环移位](#2-字符串循环移位)
    * [3. 151 字符串中单词的翻转](#3-字符串中单词的翻转) reverse-words-in-a-string (M) [Leetcode](https://leetcode.com/problems/reverse-words-in-a-string/description/)
    * [4. 242 Valid Anagram](#4-两个字符串包含的字符是否完全相同) valid-anagram (E) [Leetcode](https://leetcode.com/problems/valid-anagram/description/)
    * [5. 409 Longest Palindrome](#5-计算一组字符集合可以组成的回文字符串的最大长度) (E) [Leetcode](https://leetcode.com/problems/longest-palindrome/description/)
    * [6. 205 Isomorphic Strings](#6-字符串同构) (E) [Leetcode](https://leetcode.com/problems/isomorphic-strings/description/)
    * [7. 回文子字符串个数](#7-回文子字符串个数)
    * [8. 9 Palindrome Number](#8-判断一个整数是否是回文数) (E) [Leetcode](https://leetcode.com/problems/palindrome-number/description/)
    * [9. 统计二进制字符串中连续 1 和连续 0 数量相同的子字符串个数](#9-统计二进制字符串中连续-1-和连续-0-数量相同的子字符串个数)
<!-- GFM-TOC -->


## 1. 字符串循环移位包含
```c
#define LEN 201
bool rotateString(char* s, char* goal) {
    char a[LEN], b[LEN];
    if(strlen(s) > strlen(goal))
        return false;
    strcpy(a, s);
    strcpy(b, s);
    strcat(a, b);
    if(strstr(a, goal) != NULL)
        return true;
    return false;
}
```

## 2. 字符串循环移位
```html
s = "abcd123" k = 3
Return "123abcd"
```

将字符串向右循环移动 k 位。

将 abcd123 中的 abcd 和 123 单独翻转，得到 dcba321，然后对整个字符串进行翻转，得到 123abcd。

## 3. 字符串中单词的翻转
```c
char* reverse(char* s, int head, int tail) {
    char tmp;
    while(head < tail) {
        tmp = s[head];
        s[head++] = s[tail];
        s[tail--] = tmp;
    }
    return s;
}
void del(char* s, int id) {
    for(int i = id; i < strlen(s); i++) {
        s[i] = s[i + 1];
    }
}
char* reverseWords(char* s) {
    char* it = s;
    int l = 0, r = 0;
    //removing leading space
    while(*it++ == ' ') {
        s++;
    }
    //removing dup space
    it = s;
    for(int i = 0; s[i] != '\0'; i++) {
        if(s[i] == ' ') {
            if(s[i + 1] == ' ') {
                del(s, i + 1);
                i--;
            }
            if(s[i + 1] == '\0') {
                del(s, i);
            }
        }
    }
    reverse(s, 0, strlen(s) - 1);
    while(s[r + 1] != '\0') {
        if(s[r] != ' ')
            r++;
        else {
            reverse(s, l, r - 1);
            l = r++ + 1;
        }
    }
    reverse(s, l, r);
    return s;
}
```

## 4. 两个字符串包含的字符是否完全相同
```html
s = "anagram", t = "nagaram", return true.
s = "rat", t = "car", return false.
```

可以用 HashMap 来映射字符与出现次数，然后比较两个字符串出现的字符数量是否相同。

由于本题的字符串只包含 26 个小写字符，因此可以使用长度为 26 的整型数组对字符串出现的字符进行统计，不再使用 HashMap。

```c
bool isAnagram(char* s, char* t) {
    int alph[26] = {0};
    while(*s != '\0') {
        alph[*s++ - 'a']++;
    }
    while(*t != '\0') {
        if(alph[*t - 'a'] == 0)
            return false;
        else
            alph[*t - 'a']--;
        t++;
    }
    for(int i = 0; i < 26; i++) {
        if(alph[i] != 0)
            return false;
    }
    return true;
}
```

## 5. 计算一组字符集合可以组成的回文字符串的最大长度
```html
Input : "abccccdd"
Output : 7
Explanation : One longest palindrome that can be built is "dccaccd", whose length is 7.
```

使用长度为 256 的整型数组来统计每个字符出现的个数，每个字符有偶数个可以用来构成回文字符串。

因为回文字符串最中间的那个字符可以单独出现，所以如果有单独的字符就把它放到最中间。

```c
int longestPalindrome(char* s) {
    int map[128] = {0};
    if(s == NULL)
        return 0;
    int out = 0;
    while(*s != '\0') {
        map[*s++]++;
    }
    for(int i = 0; i < 128; i++) {
        out += map[i]/2 * 2;
        map[i] -= map[i]/2 * 2;
    }
    for(int i = 0; i < 128; i++) {
        if(map[i] >= 1)
            return out + 1;
    }
    return out;
}
```

## 6. 字符串同构

205\. Isomorphic Strings (Easy)
记录一个字符上次出现的位置，如果两个字符串中的字符上次出现的位置一样，那么就属于同构。
```c
bool isIsomorphic(char* s, char* t) {
    int map_s[256] = {0}, map_t[256] = {0}, len = strlen(s);
    for(int i = 1; i < len + 1; i++) {
        if(map_s[*s] == 0) {
            map_s[*s] = i;
        }
        if(map_t[*t] == 0){
            map_t[*t] = i;
        }
        if(map_s[*s++] != map_t[*t++])
            return false;
    }
    return true;
}
```

## 7. 回文子字符串个数

647\. Palindromic Substrings (Medium)

[Leetcode](https://leetcode.com/problems/palindromic-substrings/description/) / [力扣](https://leetcode-cn.com/problems/palindromic-substrings/description/)

```html
Input: "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
```

从字符串的某一位开始，尝试着去扩展子字符串。

```java
private int cnt = 0;

public int countSubstrings(String s) {
    for (int i = 0; i < s.length(); i++) {
        extendSubstrings(s, i, i);     // 奇数长度
        extendSubstrings(s, i, i + 1); // 偶数长度
    }
    return cnt;
}

private void extendSubstrings(String s, int start, int end) {
    while (start >= 0 && end < s.length() && s.charAt(start) == s.charAt(end)) {
        start--;
        end++;
        cnt++;
    }
}
```

## 8. 判断一个整数是否是回文数
```c
bool isPalindrome(int x) {
    long m = x, n = 0;
    if(x < 0)
        return false;
    while(m > 0) {
        n = 10 * n + m % 10;
        m /= 10;
    }
    return x == n;
}
```

## 9. 统计二进制字符串中连续 1 和连续 0 数量相同的子字符串个数

696\. Count Binary Substrings (Easy)

[Leetcode](https://leetcode.com/problems/count-binary-substrings/description/) / [力扣](https://leetcode-cn.com/problems/count-binary-substrings/description/)

```html
Input: "00110011"
Output: 6
Explanation: There are 6 substrings that have equal number of consecutive 1's and 0's: "0011", "01", "1100", "10", "0011", and "01".
```

```java
public int countBinarySubstrings(String s) {
    int preLen = 0, curLen = 1, count = 0;
    for (int i = 1; i < s.length(); i++) {
        if (s.charAt(i) == s.charAt(i - 1)) {
            curLen++;
        } else {
            preLen = curLen;
            curLen = 1;
        }

        if (preLen >= curLen) {
            count++;
        }
    }
    return count;
}
```
