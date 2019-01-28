#### Dota2 Senate

```java
class Solution {
    public String predictPartyVictory(String senate) {
        Queue<Integer> queue = new LinkedList();
        int[] people = new int[]{0, 0};
        int[] bans = new int[]{0, 0};

        for (char person: senate.toCharArray()) {
            int x = person == 'R' ? 1 : 0;
            people[x]++;
            queue.add(x);
        }

        while (people[0] > 0 && people[1] > 0) {
            int x = queue.poll();
            if (bans[x] > 0) {
                bans[x]--;
                people[x]--;
            } else {
                bans[x^1]++;
                queue.add(x);
            }
        }

        return people[1] > 0 ? "Radiant" : "Dire";
    }
}```


```python
def predictPartyVictory(self, senate):
    queue = collections.deque()
    people, bans = [0, 0], [0, 0]

    for person in senate:
        x = person == 'R'
        people[x] += 1
        queue.append(x)

    while all(people):
        x = queue.popleft()
        if bans[x]:
            bans[x] -= 1
            people[x] -= 1
        else:
            bans[x^1] += 1
            queue.append(x)

    return "Radiant" if people[1] else "Dire"```

