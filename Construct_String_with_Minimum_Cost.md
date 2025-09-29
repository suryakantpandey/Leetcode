# Problem: Construct String with Minimum Cost

**Difficulty:** Hard

------------------------------------------------------------------------

## Problem Statement

You are given a string `target`, an array of strings `words`, and an
integer array `costs`, both arrays of the same length.

Imagine an empty string `s`.

You can perform the following operation any number of times (including
zero):

-   Choose an index `i` in the range `[0, words.length - 1]`.\
-   Append `words[i]` to `s`.\
-   The cost of operation is `costs[i]`.

Return the minimum cost to make `s` equal to `target`. If it's not
possible, return `-1`.

### Example 1

**Input:**

    target = "abcdef"  
    words = ["abdef","abc","d","def","ef"]  
    costs = [100,1,1,10,5]  

**Output:**

    7

**Explanation:**\
- Select index `1` → append `"abc"` at cost 1 → `s = "abc"`\
- Select index `2` → append `"d"` at cost 1 → `s = "abcd"`\
- Select index `4` → append `"ef"` at cost 5 → `s = "abcdef"`\
- Total cost = **7**

------------------------------------------------------------------------

### Example 2

**Input:**

    target = "aaaa"  
    words = ["z","zz","zzz"]  
    costs = [1,10,100]  

**Output:**

    -1

**Explanation:**\
It is impossible to construct `"aaaa"` using the given words.

------------------------------------------------------------------------

## Constraints

-   `1 <= target.length <= 5 * 10^4`\
-   `1 <= words.length == costs.length <= 5 * 10^4`\
-   `1 <= words[i].length <= target.length`\
-   The total sum of `words[i].length` ≤ `5 * 10^4`\
-   `target` and `words[i]` consist only of lowercase English letters\
-   `1 <= costs[i] <= 10^4`

------------------------------------------------------------------------

## Approach

1.  **Aho-Corasick Automaton**:
    -   Build a trie for all words and construct suffix links to handle
        efficient substring matching.\
    -   Store `(length, cost)` pairs for words ending at each trie node.
2.  **Dynamic Programming (DP)**:
    -   `dp[i]` = minimum cost to form prefix `target[0...i-1]`.\
    -   Initialize `dp[0] = 0` and all others as `INT_MAX`.
3.  **Processing**:
    -   Traverse `target` through the automaton.\
    -   At each node, check all word matches ending at the current
        index.\
    -   Update `dp[end] = min(dp[end], dp[start] + cost)`.
4.  **Result**:
    -   Answer is `dp[n]` (where `n = target.length`).\
    -   If still `INT_MAX`, return `-1`.

------------------------------------------------------------------------

## Code

``` cpp
class Solution {
    struct AhoCorasick {
        struct Node {
            array<int, 26> next;
            int link;
            vector<pair<int, int>> outputs; // (length, cost)

            Node() {
                next.fill(-1);
                link = -1;
            }
        };

        vector<Node> trie;

        AhoCorasick() {
            trie.emplace_back();
        }

        void addWord(const string &word, int cost) {
            int node = 0;
            for (char ch : word) {
                int c = ch - 'a';
                if (trie[node].next[c] == -1) {
                    trie[node].next[c] = trie.size();
                    trie.emplace_back();
                }
                node = trie[node].next[c];
            }
            trie[node].outputs.push_back({(int)word.size(), cost});
        }

        void build() {
            queue<int> q;
            trie[0].link = 0;

            for (int c = 0; c < 26; c++) {
                int nxt = trie[0].next[c];
                if (nxt != -1) {
                    trie[nxt].link = 0;
                    q.push(nxt);
                } else {
                    trie[0].next[c] = 0;
                }
            }

            while (!q.empty()) {
                int v = q.front();
                q.pop();

                int link = trie[v].link;
                for (auto [len, cost] : trie[link].outputs) {
                    trie[v].outputs.push_back({len, cost});
                }

                for (int c = 0; c < 26; c++) {
                    int nxt = trie[v].next[c];
                    if (nxt != -1) {
                        trie[nxt].link = trie[link].next[c];
                        q.push(nxt);
                    } else {
                        trie[v].next[c] = trie[link].next[c];
                    }
                }
            }
        }

        void process(const string &text, vector<int> &dp) {
            int node = 0;
            for (int i = 0; i < text.size(); i++) {
                int c = text[i] - 'a';
                node = trie[node].next[c];

                for (auto [len, cost] : trie[node].outputs) {
                    int start = i - len + 1;
                    if (dp[start] != INT_MAX) {
                        dp[i + 1] = min(dp[i + 1], dp[start] + cost);
                    }
                }
            }
        }
    };

public:
    int minimumCost(string target, vector<string> &words, vector<int> &costs) {
        unordered_map<string, int> wordMap;
        for (int i = 0; i < words.size(); i++) {
            if (!wordMap.count(words[i]) || wordMap[words[i]] > costs[i]) {
                wordMap[words[i]] = costs[i];
            }
        }

        AhoCorasick aho;
        for (auto &[word, cost] : wordMap) {
            aho.addWord(word, cost);
        }
        aho.build();

        int n = target.size();
        vector<int> dp(n + 1, INT_MAX);
        dp[0] = 0;

        aho.process(target, dp);

        return dp[n] == INT_MAX ? -1 : dp[n];
    }
};
```

------------------------------------------------------------------------

## Complexity Analysis

-   **Time Complexity:**
    -   Building trie: `O(total length of words)`\
    -   Building suffix links: `O(total length of words * alphabet)`\
    -   Processing target: `O(|target| * alphabet)`\
    -   Overall ≈ `O(5 * 10^4)` (efficient enough).
-   **Space Complexity:**
    -   Trie nodes: proportional to total word length (`≤ 5 * 10^4`).\
    -   DP array: `O(|target|)`.\
    -   Total: `O(5 * 10^4)`.
