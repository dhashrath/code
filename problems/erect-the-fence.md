#### Erect the Fence

```java
public class Solution {
    public int orientation(Point p, Point q, Point r) {
        return (q.y - p.y) * (r.x - q.x) - (q.x - p.x) * (r.y - q.y);
    }
    public boolean inBetween(Point p, Point i, Point q) {
        boolean a = i.x >= p.x && i.x <= q.x || i.x <= p.x && i.x >= q.x;
        boolean b = i.y >= p.y && i.y <= q.y || i.y <= p.y && i.y >= q.y;
        return a && b;
    }
    public List < Point > outerTrees(Point[] points) {
        HashSet < Point > hull = new HashSet < > ();
        if (points.length < 4) {
            for (Point p: points)
                hull.add(p);
            return new ArrayList<Point>(hull);
        }
        int left_most = 0;
        for (int i = 0; i < points.length; i++)
            if (points[i].x < points[left_most].x)
                left_most = i;
        int p = left_most;
        do {
            int q = (p + 1) % points.length;
            for (int i = 0; i < points.length; i++) {
                if (orientation(points[p], points[i], points[q]) < 0) {
                    q = i;
                }
            }
            for (int i = 0; i < points.length; i++) {
                if (i != p && i != q && orientation(points[p], points[i], points[q]) == 0 && inBetween(points[p], points[i], points[q])) {
                    hull.add(points[i]);
                }
            }
            hull.add(points[q]);
            p = q;
        }
        while (p != left_most);
        return new ArrayList<Point>(hull);
    }
}
```


```java
public class Solution {
    public int orientation(Point p, Point q, Point r) {
        return (q.y - p.y) * (r.x - q.x) - (q.x - p.x) * (r.y - q.y);
    }
    public int distance(Point p, Point q) {
        return (p.x - q.x) * (p.x - q.x) + (p.y - q.y) * (p.y - q.y);
    }
    private static Point bottomLeft(Point[] points) {
        Point bottomLeft = points[0];
        for (Point p: points)
            if (p.y < bottomLeft.y)
                bottomLeft = p;
        return bottomLeft;
    }
    public List < Point > outerTrees(Point[] points) {
        if (points.length <= 1)
            return Arrays.asList(points);
        Point bm = bottomLeft(points);
        Arrays.sort(points, new Comparator < Point > () {
            public int compare(Point p, Point q) {
                double diff = orientation(bm, p, q) - orientation(bm, q, p);
                if (diff == 0)
                    return distance(bm, p) - distance(bm, q);
                else
                    return diff > 0 ? 1 : -1;
            }
        });
        int i = points.length - 1;
        while (i >= 0 && orientation(bm, points[points.length - 1], points[i]) == 0)
            i--;
        for (int l = i + 1, h = points.length - 1; l < h; l++, h--) {
            Point temp = points[l];
            points[l] = points[h];
            points[h] = temp;
        }
        Stack < Point > stack = new Stack < > ();
        stack.push(points[0]);
        stack.push(points[1]);
        for (int j = 2; j < points.length; j++) {
            Point top = stack.pop();
            while (orientation(stack.peek(), top, points[j]) > 0)
                top = stack.pop();
            stack.push(top);
            stack.push(points[j]);
        }
        return new ArrayList < > (stack);
    }
}```


```java
public class Solution {
    public int orientation(Point p, Point q, Point r) {
        return (q.y - p.y) * (r.x - q.x) - (q.x - p.x) * (r.y - q.y);
    }
    public List < Point > outerTrees(Point[] points) {
        Arrays.sort(points, new Comparator < Point > () {
            public int compare(Point p, Point q) {
                return q.x - p.x == 0 ? q.y - p.y : q.x - p.x;
            }
        });
        Stack < Point > hull = new Stack < > ();
        for (int i = 0; i < points.length; i++) {
            while (hull.size() >= 2 && orientation(hull.get(hull.size() - 2), hull.get(hull.size() - 1), points[i]) > 0)
                hull.pop();
            hull.push(points[i]);
        }
        hull.pop();
        for (int i = points.length - 1; i >= 0; i--) {
            while (hull.size() >= 2 && orientation(hull.get(hull.size() - 2), hull.get(hull.size() - 1), points[i]) > 0)
                hull.pop();
            hull.push(points[i]);
        }
        return new ArrayList < > (new HashSet < > (hull));
    }
}```

