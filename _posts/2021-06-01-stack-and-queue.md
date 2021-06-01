---
published: false
---
Stack and Queue are both linear data structure. Both of them can be considered as linked lists with limited reading and writing rights.Therefore, stack and queue's problems can also be resolved by more powerful list as well.

Queue's interface is implemented by Linked List in java. Queue can also be implemented by array in 2 ways that are iteratively using one array or using dynamically incremental space's ArrayList.

Stack's interface is implemented by Stack class, which is also a list, in java. One of stack's famous application is used by recursion. When we call a recursion, we will push all programs' info into stack. When we return from a recursion's call, we will pop all program's info from the stack. Here the program's info is a sub problem.

### Problems

#### [40 Implement Queue by Two Stacks](https://www.lintcode.com/problem/40/)

Description

As the title described, you should only use two stacks to implement a queue's actions.

The queue should support `push(element)`, `pop()` and `top()` where pop is pop the first(a.k.a front) element in the queue.

Both pop and top methods should return the value of first element.

Suppose the queue is not empty when the pop() function is called.

Example 1:

Input:

```
Queue Operations = 
    push(1)
    pop()    
    push(2)
    push(3)
    top()    
    pop()  
```

Output:

```
1
2
2
```

Explanation:

Both pop and top methods should return the value of the first element.

Challenge

implement it by two stacks, do not use any other data structure and push, pop and top should be O(1) by *AVERAGE*.

Analysis:

One stack is used for pushing. The other stack is used for popping. Here we need to pay attention, if the popping stack is not empty, we will not move any element from pushing stack to it. We will only dump all elements from pushing stack to popping stack when the popping stack is empty.

```java
public class MyQueue {
    Stack<Integer> pushStack;
    Stack<Integer> popStack;

    public MyQueue() {
        pushStack = new Stack<>();
        popStack = new Stack<>();
    }

    /*
     * @param element: An integer
     * @return: nothing
     */
    public void push(int element) {
        pushStack.push(element);
    }

    /*
     * @return: An integer
     */
    public int pop() {
        if(popStack.isEmpty()) {
            while(!pushStack.isEmpty()) {
                popStack.push(pushStack.pop());
            }
        }

        return popStack.pop();

    }

    /*
     * @return: An integer
     */
    public int top() {
        if(popStack.isEmpty()) {
            while(!pushStack.isEmpty()) {
                popStack.push(pushStack.pop());
            }
        }
        return popStack.peek();
    }
}
```

#### [12  Min Stack](https://www.lintcode.com/problem/12/description)

Description

Implement a stack with following functions:

- `push(val)` push val into the stack
- `pop()` pop the top element and return it
- `min()` return the smallest number in the stack

All above should be in O(1) cost

`min()` will never be called when there is no number in the stack.

Example 1:

Input:

```
push(1)
min()
push(2)
min()
push(3)
min()
```

Output:

```
1
1
1
```

Analysis:

We can use two stacks to solve this question. One stack is for normal stack functionality.One stack is saving the min value of all numbers before its same position number in the first normal stack.

```java
public class MinStack {
    
    Stack<Integer> stack;
    Stack<Integer> min;
    
    public MinStack() {
        stack = new Stack<>();
        min = new Stack<>();
    }

    /*
     * @param number: An integer
     * @return: nothing
     */
    public void push(int number) {
        stack.push(number);
        if(min.isEmpty()) {
            min.push(number);
        }else {
            int preMin = min.peek();
            if(preMin < number) {
                min.push(preMin);
            }else {
                min.push(number);
            }
        }
        

    }

    /*
     * @return: An integer
     */
    public int pop() {
        min.pop();
        return stack.pop();
    }

    /*
     * @return: An integer
     */
    public int min() {
        return min.peek();
    }
}
```



#### [423  Valid Parentheses](https://www.lintcode.com/problem/423/)

Description:

Given a string containing just the characters `'(', ')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

The brackets must close in the correct order, `"()"` and `"()[]{}"` are all valid but `"(]"` and `"([)]"` are not.

Example

**Example 1:**

```
Input: "([)]"
Output: False
```

**Example 2:**

```
Input: "()[]{}"
Output: True
```

Challenge

Use O(n) time, n is the number of parentheses.

Analysis:

This question requires to valid a parentheses string. An open bracket will be a valid bracket if we can find its later corresponding closed bracket. That is to say, one element's result depends on its latter elements. We can use stack.

[Parenthesis Related Problem](./CleanCodePractice.md)

```java
    public boolean isValidParentheses(String s) {                Stack<Character> stack = new Stack<>();        char[] chars = s.toCharArray();        for(char c : chars) {            if(c == '(' || c == '{' || c == '[') {                stack.push(c);            } else if(!stack.isEmpty() && ((c == ')' && stack.peek() == '(')             || (c == ']' && stack.peek() == '[')             || (c == '}' && stack.peek() == '{'))) {                stack.pop();            }else {                return false;            }        }        return stack.isEmpty();    }
```

#### [424  Evaluate Reverse Polish Notation](https://www.lintcode.com/problem/424/)

Description:

Evaluate the value of an arithmetic expression in [Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation).

Valid operators are `+`, `-`, `*`, `/`. Each operand may be an **integer** or another expression.

If you use python, you should use `int(a / b)` to calc the `/` operation rather than `a // b`.

Example

Example 1:

```
Input: ["2", "1", "+", "3", "*"] Output: 9Explanation: ["2", "1", "+", "3", "*"] -> (2 + 1) * 3 -> 9
```

Example 2:

```
Input: ["4", "13", "5", "/", "+"]Output: 6Explanation: ["4", "13", "5", "/", "+"] -> 4 + 13 / 5 -> 6
```



Analysis:

When we see an operator, we need to get its former closest two numbers and do the calculation. Any number's final result is based on its later operators. Therefore, we will think of using stack to store all numbers.

```java
public int evalRPN(String[] tokens) {  String operators = "+-*/";  Stack<Integer> stack = new Stack<>();  for(String t : tokens) {    if(operators.contains(t)) {      int operand2 = stack.pop();      int operand1 = stack.pop();      int temp = 0;      if( t.equals("+")) {        temp = operand1 + operand2;      }else if (t.equals("-")) {        temp = operand1 - operand2;      }else if(t.equals("*")) {        temp = operand1 * operand2;      }else {        temp = operand1 / operand2;      }      stack.push(temp);    }else {       stack.push(Integer.valueOf(t));    }  }  return stack.peek();}
```

