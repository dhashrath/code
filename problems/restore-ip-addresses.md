#### Restore IP Addresses

```java
class Solution {
  int n;
  String s;
  LinkedList<String> segments = new LinkedList<String>();
  ArrayList<String> output = new ArrayList<String>();

  public boolean valid(String segment) {
        /*
        Check if the current segment is valid :
        1. less or equal to 255      
        2. the first character could be '0' 
           only if the segment is equal to '0'
        */
    int m = segment.length();
    if (m > 3)
      return false;
    return (segment.charAt(0) != '0') ? (Integer.valueOf(segment) <= 255) : (m == 1);
  }

  public void update_output(int curr_pos) {
        /*
        Append the current list of segments 
        to the list of solutions
        */
    String segment = s.substring(curr_pos + 1, n);
    if (valid(segment)) {
      segments.add(segment);
      output.add(String.join(".", segments));
      segments.removeLast();
    }
  }

  public void backtrack(int prev_pos, int dots) {
        /*
        prev_pos : the position of the previously placed dot
        dots : number of dots to place
        */
    // The current dot curr_pos could be placed 
    // in a range from prev_pos + 1 to prev_pos + 4.
    // The dot couldn't be placed 
    // after the last character in the string.
    int max_pos = Math.min(n - 1, prev_pos + 4);
    for (int curr_pos = prev_pos + 1; curr_pos < max_pos; curr_pos++) {
      String segment = s.substring(prev_pos + 1, curr_pos + 1);
      if (valid(segment)) {
        segments.add(segment);  // place dot
        if (dots - 1 == 0)      // if all 3 dots are placed
          update_output(curr_pos);  // add the solution to output
        else
          backtrack(curr_pos, dots - 1);  // continue to place dots
        segments.removeLast();  // remove the last placed dot 
      }
    }
  }

  public List<String> restoreIpAddresses(String s) {
    n = s.length();
    this.s = s;
    backtrack(-1, 3);
    return output;
  }
}```


```python
class Solution:
    def restoreIpAddresses(self, s):
        """
        :type s: str
        :rtype: List[str]
        """
        def valid(segment):
            """
            Check if the current segment is valid :
            1. less or equal to 255      
            2. the first character could be '0' 
               only if the segment is equal to '0'
            """
            return int(segment) <= 255 if segment[0] != '0' else len(segment) == 1
            
        def update_output(curr_pos):
            """
            Append the current list of segments 
            to the list of solutions
            """
            segment = s[curr_pos + 1:n]
            if valid(segment):
                segments.append(segment)
                output.append('.'.join(segments))
                segments.pop()    
            
        def backtrack(prev_pos = -1, dots = 3):
            """
            prev_pos : the position of the previously placed dot
            dots : number of dots to place
            """
            # The current dot curr_pos could be placed 
            # in a range from prev_pos + 1 to prev_pos + 4.
            # The dot couldn't be placed 
            # after the last character in the string.
            for curr_pos in range(prev_pos + 1, min(n - 1, prev_pos + 4)):
                segment = s[prev_pos + 1:curr_pos + 1]
                if valid(segment):
                    segments.append(segment)  # place dot
                    if dots - 1 == 0:  # if all 3 dots are placed
                        update_output(curr_pos)  # add the solution to output
                    else:
                        backtrack(curr_pos, dots - 1)  # continue to place dots
                    segments.pop()  # remove the last placed dot
        
        n = len(s)
        output, segments = [], []
        backtrack() ```

