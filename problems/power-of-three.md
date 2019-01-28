#### Power of Three

```java
public class Solution {
    public boolean isPowerOfThree(int n) {
        if (n < 1) {
            return false;
        }

        while (n % 3 == 0) {
            n /= 3;
        }

        return n == 1;
    }
}```


```java
String baseChange = Integer.toString(number, base);```


```java
boolean matches = myString.matches("123");```


```java
boolean powerOfThree = baseChange.matches("^10*$")```


```java
public class Solution {
    public boolean isPowerOfThree(int n) {
        return Integer.toString(n, 3).matches("^10*$");
    }
}```


```java
public class Solution {
    public boolean isPowerOfThree(int n) {
        return (Math.log10(n) / Math.log10(3)) % 1 == 0;
    }
}```


```java
return (Math.log(n) / Math.log(3) + epsilon) % 1 <= 2 * epsilon;```


```java
public boolean isPowerOfThree(int n)```


```java
public class Solution {
    public boolean isPowerOfThree(int n) {
        return n > 0 && 1162261467 % n == 0;
    }
}```


```java
public static void main(String[] args) {
    Solution sol = new Solution();
    int iterations = 1; // See table header for this value
    for (int i = 0; i < iterations; i++) {
        sol.isPowerOfThree(i);
    }
}```

