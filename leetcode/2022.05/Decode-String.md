## [LeetCode 394. Decode String](https://leetcode.com/problems/decode-string/)

---

> Given an encoded string, return its decoded string. The encoding rule is: k[encoded_string], where the 
> encoded_string inside the square brackets is being repeated exactly k times. Note that k is guaranteed to 
> be a positive integer. You may assume that the input string is always valid; there are no extra white spaces, 
> square brackets are well-formed, etc. Furthermore, you may assume that the original data does not contain any 
> digits and that digits are only for those repeat numbers, k. For example, there will not be 
> input like 3a or 2[4].

#### Solution

Stack

Time Complexity: O(N * maxK^countK)

Space Complexity: O(Sum(N * maxK^countK))


```java
class Solution {
    public String decodeString(String s) {
        Stack<Character> stack = new Stack<>();
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == ']') {
                List<Character> decodedString = new ArrayList<>();
                // get the encoded string
                while (stack.peek() != '[') {
                    decodedString.add(stack.pop());
                }
                // pop [ from the stack
                stack.pop();
                int base = 1;
                int k = 0;
                // get the number k
                while (!stack.isEmpty() && Character.isDigit(stack.peek())) {
                    k = k + (stack.pop() - '0') * base;
                    base *= 10;
                }
                // decode k[decodedString], by pushing decodedString k times into stack
                while (k != 0) {
                    for (int j = decodedString.size() - 1; j >= 0; j--) {
                        stack.push(decodedString.get(j));
                    }
                    k--;
                }
            }
            // push the current character to stack
            else {
                stack.push(s.charAt(i));
            }
        }      
        // get the result from stack
        char[] result = new char[stack.size()];
        for (int i = result.length - 1; i >= 0; i--) {
            result[i] = stack.pop();
        }
        return new String(result);
    }
}


```