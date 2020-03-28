## LeetCode每日一题（二十八）：单词的压缩编码

给定一个单词列表，我们将这个列表编码成一个索引字符串 `S` 与一个索引列表 `A`。

例如，如果这个列表是 `["time", "me", "bell"]`，我们就可以将其表示为 `S = "time#bell#"` 和 `indexes = [0, 2, 5]`。

对于每一个索引，我们可以通过从字符串 `S` 中索引的位置开始读取字符串，直到 `"#"` 结束，来恢复我们之前的单词列表。

那么成功对给定单词列表进行编码的最小字符串长度是多少呢？
 

示例：

```
输入: words = ["time", "me", "bell"]
输出: 10
说明: S = "time#bell#" ， indexes = [0, 2, 5] 。
```

提示：

* 1 <= words.length <= 2000
* 1 <= words[i].length <= 7
* 每个单词都是小写字母 。


## Solve

说实话，这题我一点思路也没有。就连字典树题解也没看懂...借鉴一些别人的解法，就当是学习新知识了。

解读题意：如果一个单词为另一个单词的后缀，例如： `me` 是 `time` 的后缀，则不需要再对 `me` 编码。后缀 `ime` 和 `e` 亦是如此。

编码出的字符串可以根据索引值恢复单词列表。最后输出的则是编码后最小字符串长度。


#### 1.排序+后缀匹配

根据单词的后缀进行排序，最后通过单词的后缀进行对比。

例如：

<pre>
time                  me
me                  time
sometime   -->  sometime
bell                bell
barbell          barbell
</pre>

后缀对比时，发现 `单词1` 为 `单词2` 的后缀时，舍弃 `单词1`，计算 `单词2` 的长度并添加 `"#"` 一个字符长度。

```c++
class Solution {
public:
    // 按照字母表顺序进行排序
    static bool compare(string& s1, string& s2) {
        int N1 = s1.length();
        int N2 = s2.length();
        for (int i = 0; i < min(N1, N2); i++) {
            char c1 = s1[N1-1-i];
            char c2 = s2[N2-1-i];
            if (c1 != c2) {
                return c1 < c2;
            }
        }
        return N1 < N2;
    }

    // 两个字符串后缀匹配
    bool endsWith(string& s, string& t) {
        int N1 = s.length();
        int N2 = t.length();
        if (N1 < N2) {
            return false;
        }
        for (int i = 0; i < N2; i++) {
            if (s[N1-N2+i] != t[i]) {
                return false;
            }
        }
        return true;
    }

    int minimumLengthEncoding(vector<string>& words) {
        int N = words.size();
        // 逆序字典序排序    
        sort(words.begin(), words.end(), compare);

        int res = 0;
        for (int i = 0; i < N; i++) {
            if (i+1 < N && endsWith(words[i+1], words[i])) {
                // 当前单词是下一个单词的后缀，丢弃
            } else {
                res += words[i].length() + 1; // 单词加上一个 '#' 的长度
            }
        }
        return res;
    }
}

作者：nettee
链接：https://leetcode-cn.com/problems/short-encoding-of-words/solution/wu-xu-zi-dian-shu-qing-qing-yi-fan-zhuan-jie-guo-j/
来源：力扣（LeetCode）
```


#### 2.字典树

去找到是否不同的单词具有相同的后缀，我们可以将其反序之后插入字典树中。例如，我们有 `"time"` 和 `"me"` ，可以将 `"emit"` 和 `"em"` 插入字典树中。

然后，字典树的叶子节点（没有孩子的节点）就代表没有后缀的单词，统计叶子节点代表的单词长度加一的和即为我们要的答案。

<div align="center">
    <img width="420px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/zi-dian-tree.PNG"></img>
</div>

```c++
class TrieNode{
    TrieNode* children[26];
public:
    int count;
    TrieNode() {
        for (int i = 0; i < 26; ++i) children[i] = NULL;
        count = 0;
    }
    TrieNode* get(char c) {
        if (children[c - 'a'] == NULL) {
            children[c - 'a'] = new TrieNode();
            count++;
        }
        return children[c - 'a'];
    }
};
class Solution {
public:
    int minimumLengthEncoding(vector<string>& words) {
        TrieNode* trie = new TrieNode();
        unordered_map<TrieNode*, int> nodes;

        for (int i = 0; i < (int)words.size(); ++i) {
            string word = words[i];
            TrieNode* cur = trie;
            for (int j = word.length() - 1; j >= 0; --j)
                cur = cur->get(word[j]);
            nodes[cur] = i;
        }

        int ans = 0;
        for (auto& [node, idx] : nodes) {
            if (node->count == 0) {
                ans += words[idx].length() + 1;
            }
        }
        return ans;
    }
};

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/short-encoding-of-words/solution/dan-ci-de-ya-suo-bian-ma-by-leetcode-solution/
来源：力扣（LeetCode）
```

<div align="center">
    <hr style="height:1px;"/>
    <br>
    <img width="200px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/wxgzh-hang.png"></img>
</div>