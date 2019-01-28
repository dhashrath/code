#### Minimum Area Rectangle II

```java
import java.awt.Point;

class Solution {
    public double minAreaFreeRect(int[][] points) {
        int N = points.length;

        Point[] A = new Point[N];
        Set<Point> pointSet = new HashSet();
        for (int i = 0; i < N; ++i) {
            A[i] = new Point(points[i][0], points[i][1]);
            pointSet.add(A[i]);
        }

        double ans = Double.MAX_VALUE;
        for (int i = 0; i < N; ++i) {
            Point p1 = A[i];
            for (int j = 0; j < N; ++j) if (j != i) {
                Point p2 = A[j];
                for (int k = j+1; k < N; ++k) if (k != i) {
                    Point p3 = A[k];
                    Point p4 = new Point(p2.x + p3.x - p1.x, p2.y + p3.y - p1.y);

                    if (pointSet.contains(p4)) {
                        int dot = ((p2.x - p1.x) * (p3.x - p1.x) +
                                   (p2.y - p1.y) * (p3.y - p1.y));
                        if (dot == 0) {
                            double area = p1.distance(p2) * p1.distance(p3);
                            if (area < ans)
                                ans = area;
                        }
                    }
                }
            }
        }

        return ans < Double.MAX_VALUE ? ans : 0;
    }
}```


```python
class Solution(object):
    def minAreaFreeRect(self, points):
        EPS = 1e-7
        points = set(map(tuple, points))

        ans = float('inf')
        for p1, p2, p3 in itertools.permutations(points, 3):
            p4 = p2[0] + p3[0] - p1[0], p2[1] + p3[1] - p1[1]
            if p4 in points:
                v21 = complex(p2[0] - p1[0], p2[1] - p1[1])
                v31 = complex(p3[0] - p1[0], p3[1] - p1[1])
                if abs(v21.real * v31.real + v21.imag * v31.imag) < EPS:
                    area = abs(v21) * abs(v31)
                    if area < ans:
                        ans = area

        return ans if ans < float('inf') else 0```


```java
import java.awt.Point;

class Solution {
    public double minAreaFreeRect(int[][] points) {
        int N = points.length;
        Point[] A = new Point[N];
        for (int i = 0; i < N; ++i)
            A[i] = new Point(points[i][0], points[i][1]);

        Map<Integer, Map<Point, List<Point>>> seen = new HashMap();
        for (int i = 0; i < N; ++i)
            for (int j = i+1; j < N; ++j) {
                // center is twice actual to keep integer precision
                Point center = new Point(A[i].x + A[j].x, A[i].y + A[j].y);

                int r2 = (A[i].x - A[j].x) * (A[i].x - A[j].x);
                r2 += (A[i].y - A[j].y) * (A[i].y - A[j].y);
                if (!seen.containsKey(r2))
                    seen.put(r2, new HashMap<Point, List<Point>>());
                if (!seen.get(r2).containsKey(center))
                    seen.get(r2).put(center, new ArrayList<Point>());
                seen.get(r2).get(center).add(A[i]);
            }

        double ans = Double.MAX_VALUE;
        for (Map<Point, List<Point>> info: seen.values()) {
            for (Point center: info.keySet()) {  // center is twice actual
                List<Point> candidates = info.get(center);
                int clen = candidates.size();
                for (int i = 0; i < clen; ++i)
                    for (int j = i+1; j < clen; ++j) {
                        Point P = candidates.get(i);
                        Point Q = candidates.get(j);
                        Point Q2 = new Point(center);
                        Q2.translate(-Q.x, -Q.y);
                        double area = P.distance(Q) * P.distance(Q2);
                        if (area < ans)
                            ans = area;
                    }
            }
        }

        return ans < Double.MAX_VALUE ? ans : 0;
    }
}```


```python
class Solution(object):
    def minAreaFreeRect(self, points):
        points = [complex(*z) for z in points]
        seen = collections.defaultdict(list)
        for P, Q in itertools.combinations(points, 2):
            center = (P + Q) / 2
            radius = abs(center - P)
            seen[center, radius].append(P)

        ans = float("inf")
        for (center, radius), candidates in seen.iteritems():
            for P, Q in itertools.combinations(candidates, 2):
                ans = min(ans, abs(P - Q) * abs(P - (2*center - Q)))

        return ans if ans < float("inf") else 0```

