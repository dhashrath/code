


#### Approach #1: Depth-First Search [Accepted]

**Intuition**

Draw an edge between two emails if they occur in the same account.  The problem comes down to finding connected components of this graph.

**Algorithm**

For each account, draw the edge from the first email to all other emails.  Additionally, we'll remember a map from emails to names on the side.  After finding each connected component using a depth-first search, we'll add that to our answer.

**Python**
```python
class Solution(object):
    def accountsMerge(self, accounts):
        em_to_name = {}
        graph = collections.defaultdict(set)
        for acc in accounts:
            name = acc[0]
            for email in acc[1:]:
                graph[acc[1]].add(email)
                graph[email].add(acc[1])
                em_to_name[email] = name

        seen = set()
        ans = []
        for email in graph:
            if email not in seen:
                seen.add(email)
                stack = [email]
                component = []
                while stack:
                    node = stack.pop()
                    component.append(node)
                    for nei in graph[node]:
                        if nei not in seen:
                            seen.add(nei)
                            stack.append(nei)
                ans.append([em_to_name[email]] + sorted(component))
        return ans
```

**Java**
```java
class Solution {
    public List<List<String>> accountsMerge(List<List<String>> accounts) {
        Map<String, String> emailToName = new HashMap();
        Map<String, ArrayList<String>> graph = new HashMap();
        for (List<String> account: accounts) {
            String name = "";
            for (String email: account) {
                if (name == "") {
                    name = email;
                    continue;
                }
                graph.computeIfAbsent(email, x-> new ArrayList<String>()).add(account.get(1));
                graph.computeIfAbsent(account.get(1), x-> new ArrayList<String>()).add(email);
                emailToName.put(email, name);
            }
        }

        Set<String> seen = new HashSet();
        List<List<String>> ans = new ArrayList();
        for (String email: graph.keySet()) {
            if (!seen.contains(email)) {
                seen.add(email);
                Stack<String> stack = new Stack();
                stack.push(email);
                List<String> component = new ArrayList();
                while (!stack.empty()) {
                    String node = stack.pop();
                    component.add(node);
                    for (String nei: graph.get(node)) {
                        if (!seen.contains(nei)) {
                            seen.add(nei);
                            stack.push(nei);
                        }
                    }
                }
                Collections.sort(component);
                component.add(0, emailToName.get(email));
                ans.add(component);
            }
        }
        return ans;
    }
}
```

**Complexity Analysis**

* Time Complexity: $$O(\sum a_i \log a_i)$$, where $$a_i$$ is the length of `accounts[i]`.  Without the log factor, this is the complexity to build the graph and search for each component.  The log factor is for sorting each component at the end.

* Space Complexity: $$O(\sum a_i)$$, the space used by our graph and our search.

---
#### Approach #2: Union-Find [Accepted]

**Intuition**

As in *Approach #1*, our problem comes down to finding the connected components of a graph.  This is a natural fit for a *Disjoint Set Union* (DSU) structure.

**Algorithm**

As in *Approach #1*, draw edges between emails if they occur in the same account.  For easier interoperability between our DSU template, we will map each email to some integer index by using `emailToID`.  Then, `dsu.find(email)` will tell us a unique id representing what component that email is in.

For more information on DSU, please look at *Approach #2* in the [article here](https://leetcode.com/articles/redundant-connection/).  For brevity, the solutions showcased below do not use *union-by-rank*.

**Python**
```python
class DSU:
    def __init__(self):
        self.p = range(10001)
    def find(self, x):
        if self.p[x] != x:
            self.p[x] = self.find(self.p[x])
        return self.p[x]
    def union(self, x, y):
        self.p[self.find(x)] = self.find(y)

class Solution(object):
    def accountsMerge(self, accounts):
        dsu = DSU()
        em_to_name = {}
        em_to_id = {}
        i = 0
        for acc in accounts:
            name = acc[0]
            for email in acc[1:]:
                em_to_name[email] = name
                if email not in em_to_id:
                    em_to_id[email] = i
                    i += 1
                dsu.union(em_to_id[acc[1]], em_to_id[email])

        ans = collections.defaultdict(list)
        for email in em_to_name:
            ans[dsu.find(em_to_id[email])].append(email)

        return [[em_to_name[v[0]]] + sorted(v) for v in ans.values()]
```

**Java**
```java
class Solution {
    public List<List<String>> accountsMerge(List<List<String>> accounts) {
        DSU dsu = new DSU();
        Map<String, String> emailToName = new HashMap();
        Map<String, Integer> emailToID = new HashMap();
        int id = 0;
        for (List<String> account: accounts) {
            String name = "";
            for (String email: account) {
                if (name == "") {
                    name = email;
                    continue;
                }
                emailToName.put(email, name);
                if (!emailToID.containsKey(email)) {
                    emailToID.put(email, id++);
                }
                dsu.union(emailToID.get(account.get(1)), emailToID.get(email));
            }
        }

        Map<Integer, List<String>> ans = new HashMap();
        for (String email: emailToName.keySet()) {
            int index = dsu.find(emailToID.get(email));
            ans.computeIfAbsent(index, x-> new ArrayList()).add(email);
        }
        for (List<String> component: ans.values()) {
            Collections.sort(component);
            component.add(0, emailToName.get(component.get(0)));
        }
        return new ArrayList(ans.values());
    }
}
class DSU {
    int[] parent;
    public DSU() {
        parent = new int[10001];
        for (int i = 0; i <= 10000; ++i)
            parent[i] = i;
    }
    public int find(int x) {
        if (parent[x] != x) parent[x] = find(parent[x]);
        return parent[x];
    }
    public void union(int x, int y) {
        parent[find(x)] = find(y);
    }
}
```

**Complexity Analysis**

* Time Complexity: $$O(A \log A)$$, where $$A = \sum a_i$$, and $$a_i$$ is the length of `accounts[i]`.  If we used union-by-rank, this complexity improves to $$O(A \alpha(A)) \approx O(A)$$, where $$\alpha$$ is the *Inverse-Ackermann* function.

* Space Complexity: $$O(A)$$, the space used by our DSU structure.

---

Analysis written by: [@awice](https://leetcode.com/awice).
