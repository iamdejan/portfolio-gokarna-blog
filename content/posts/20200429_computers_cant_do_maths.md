+++
title = "Computers Can't Do Maths! (CSSE)"
date = 2020-04-19T00:00:00+07:00
description = ""
type = "post"
showTableOfContents = "true"
+++

NOTE: This article is originally posted on my [LinkedIn Pulse](https://www.linkedin.com/pulse/computers-cant-do-maths-csse-giovanni-dejan/).

```
Detective Nock: Could machines ever think as human beings do?
Alan Turing   : Most people say no.
Detective Nock: You're not most people.
Alan Turing   : Well, the problem is, you're asking a stupid question.

(The Imitation Game, 2014)
```

Do we ever think why we calculate things as it is? Do we ever wonder how computers calculate things? In this post, I will try to unlock the secret of evaluation expression in computer.

# How Do Humans Evaluate Expression?

## Addition

Let's try with a basic equation:
```
1 + 1 = 2
```

This is the most basic equation in maths, I presume. But, how do we know that 1 + 1 equals 2? Let's try to parse this express:
- `1` is the operand.
- `+` is the operator.

But, still, how do our brains know that 1 + 1 = 2? We have to think deeper. Let's try with numbers line (^ means target number).
```
1 2 3 4 5 6 7 8 9 ...
^ + + + + + + + + ...
```

In the numbers line above, we start at 1 (the leftmost number) and we apply plus operation (signed with + in the numbers line), which means that numbers on the right side of 1 could be the answer.

Then, we shift the target number by 1 unit to the right (based on possibilities above). That's why we got 2.
```
1 2 3 4 5 6 7 8 9 ...
  ^
```

Maybe, I think that's easiest explanation to think about addition. This is quite hard because we were taught maths from elementary school, so it's kinda like hardwired. But is it the right way our brain thinks? I don't know.

## Subtraction

What about subtraction? Well, it's reversed direction of addition, right? We just need to shift left. Let's simulate this equation:
```
5 - 3 = 2
```

How do we draw the numbers line?
```
1 2 3 4 5 6 7
- - - - ^
```

Numbers line above is the state when we parse from 5 (the first operand) and - (the operator). Once we parse the 2nd operand (number 3), the number line becomes like this:
```
1 2 3 4 5 6 7
  ^
```

We shift the target number by 3 units to the left. I hope you can see the big picture here.

## Multiplication and Division

What about multiplication and division? Multiplication is just repetition of additions. I think this doesn't need much of explanations. The tricky thing is about division, because we kinda need to "guess" the result. You don't believe what I'm saying? Let's examine this equation:
```
54 / 9 = 6
```

How do we know the answer is 6, and not other numbers? Because we know that 6 multiplied by 9 is 54. What if we don't know? We do a brute-force search.
```
1 * 9 = 9
2 * 9 = 18
3 * 9 = 27
4 * 9 = 36
5 * 9 = 45
6 * 9 = 54 <- AHA! We found it!
```

This is at least how my brain works on division. This is also the way we were taught about division at elementary school. If you have other ways of calculating division, please put your comments down below :-)

# How Computer Evaluates Expression... Sort of

Consider the first equation: 1 + 1 = 2. Now, if you use computer to calculate the expression, what will computer do?

## Half-Adder Logic

In the very low-level, the way computer adds two numbers is to use Half-Adder Logic. Below is the C++ code for Half-Adder Logic:
```cpp
int half_adder(int a, int b) {
    if(a == 0) {
        return b;
    } else {
        while(b != 0) {
            int carry = a & b;
            a ^= b;
            b = carry << 1;
        }
        return a;
    }
}
```

But of course, most programming languages do not implement this, since this is done at Arithmetic Logic Units (ALUs) level.

## Polish Notation

For evaluating a human-readable evaluation, computer can convert the expression to [Polish notation](https://en.wikipedia.org/wiki/Polish_notation). But what is Polish notation, by the way? Polish notation is a notation in which we put operator before the operands. Below is the example:
```
1 + 1 <- infix notation
+ 1 1 <- Polish (prefix) notation
```

But, how do computer converts expression from infix to prefix expression? You can look at [this article](https://www.geeksforgeeks.org/infix-to-prefix-conversion-using-two-stacks/).

# How Does Human Process Operator Precedence and Parentheses?

Expressions aren't always that easy. Sometimes, we encounter expressions with operator precedence and priority evaluations, like this expression:
```
4 + 18 / (9 - 3)
```

How do we, as human, calculate this? We were taught at the elementary school that the precedence goes as follows:
1) Parentheses.
2) Multiplication and division.
3) Addition and subtraction.

*NOTE: If 2 operators have same precedence, then we evaluate the leftmost expression first.*

For above expression, the first thing we do is to resolve sub-expression inside the parentheses:
```
(9 - 3)
```

After we resolve this, it is simplified to:
```
4 + 18 / 6
```

Then, we look at sub-expression that highest operator precedence, which is:
```
18 / 6
```

The last step is to finish the expression:
```
4 + 3
```

The result is `7`.

# A Glimpse of Shunting Yard Algorithm

Let's look again at this expression:
```
4 + 18 / (9 - 3)
```

For above expression to be calculated by computer, the computer needs to convert the expression into [Reverse Polish notation](https://en.wikipedia.org/wiki/Reverse_Polish_notation). Reverse Polish notation is a notation in which we put operator after operands. Example:
```
1 + 1 <- infix notation
+ 1 1 <- Polish (prefix) notation
1 1 + <- Reverse Polish (postfix) notation
```

If the equation is as complex as the last equation (with operator precedence and / or "priority expressions" (expressions inside parentheses)), we need to use Shunting Yard algorithm to convert the equation.

*Fun fact: Shunting Yard algorithm name is used by Edsger Dijkstra because the visualization resembles railroad shunting yard.*

[![Shunting Yard Algorithm by Edsger Dijkstra](/images/shunting-yard-algorithm.png)](http://en.wikipedia.org/wiki/Shunting-yard_algorithm)

Above is the visualization of Shunting Yard algorithm from Wikipedia, albeit not complete, because the example expression doesn't have parentheses. As you can see, there are 2 components of Shunting Yard algorithm:

1) Output queue: queue that holds the final result.
2) Operator stack: stack that holds operator(s) read from input.

The complete algorithm is as follows (taken from [Brilliant.org](https://brilliant.org/wiki/shunting-yard-algorithm/)):
```
for each token in expression:
    if token is number:
        put into output queue.
    else if token is operator:
        while operator on top of operator stack has greater precedence:
            if top of operator stack is left parenthesis:
                break from loop
            pop operator, put into output queue.
        put operator into operator stack.
    else if token is left parenthesis:
        put into operator stack.
    else if token is right parenthesis:
        while token on top of operator stack is not left parenthesis:
            pop token, put into output queue
        discard left parenthesis.
```

But, how to implement this algorithm? **It's time to code! :-)**

# Shunting Yard + BDD For The Win

I have to be honest. This is my first time to code Shunting Yard algorithm. I still don't know a lot about it. That's why we're about to tackle this problem with Behavior-Driven Development.

I started BDD a while ago at Upscale 3.0 program. In case you miss my article about Upscale, you can read it [here](https://www.linkedin.com/pulse/you-dont-need-code-upscale-30-giovanni-dejan/).

So, in BDD, just like TDD, you have to write the test first, then write minimum code to pass the test. In BDD, however, the test resembles the actual scenario that could happen. So, in BDD, you're **focused on the whole process**, unlike TDD where you're focused on unit. But of course since this is not a layered application, it still feels similar.

In the proper process of BDD, the [test master is expected to write the scenarios]((https://cucumber.io/blog/bdd/who-should-actually-write-bdd-scenarios/)), including edge cases. However, since this is a small project, we have to write our own scenarios. Don't worry, though, as I have prepared several scenarios:
1) Empty expression, a.k.a. no token. Expected result: Error (Empty token list).
2) Simple expression: 1 + 1. Expected result: Success (1 1 +).
3) Simple expression with 2 operators: 1 + 2 - 3. Expected result: Success (1 2 + 3 -).
4) Simple expression with 2 operators: 1 * 2 / 1. Expected result: Success (1 2 * 1 /).
5) Expression with 2 operators and 2 operator precedences: 1 + 2 * 3. Expected result: Success (1 2 3 * +).
6) Expression with parentheses: 4 + 18 / (9 - 3). Expected result: Success (4 18 9 3 - / +).
7) Expression with parentheses: (5 * 4 + 3) - 1. Expected result: Success (5 4 * 3 + 1 -).
8) Expression with nested parentheses: (1 + 2) * 3 + 4 * ((7 - 5) + 6). Expected result: Success (1 2 + 3 * 4 7 5 - 6 + * +).
9) Expression with nested parentheses: 2 * (3 + 5) / 4 + 1 * 9 + (4 * (6 / 3)). Expected result: Success (2 3 5 + * 4 / 1 9 * + 4 6 3 / * +).

*NOTE: Please do correct me if I'm wrong in writing test scenarios, because I just started learning BDD. I might have missed several scenarios.*

## Setup Project

Although I will present BDD using Rust, it's also possible for you to use other languages. For Java, you can use Cucumber. For Ruby, you can use Rspec. For Golang, you can use the usual Go test. For JavaScript / Typescript, you can use Chai + Mocha.

If you have cargo (Rust package manager) installed, you can run:
```
$ cargo new rust-shunting-yard --lib
```

Inside the project, you will find "src" folder. Inside "src" folder, you will find "lib.rs" file. This is where we will put our test cases and code. In "lib.rs" file, you will find:
```rs
#[cfg(test)]
mod tests {

}
```
This is the boilerplate for us to write tests.

If you don't have Rust compiler / you don't want to install, you can use [Rust Playground](https://play.rust-lang.org/). If you use Rust locally, run this command:
```
$ cargo test
```

Now, it's time to add code, incrementally. If you're confused about what has changed between subsection, you can use [Mergely](www.mergely.com/editor). I will also show diff file for each test case, so that you can see the difference in [KW Online Patch/Diff File Viewer](https://tools.knowledgewalls.com/online-patch-diff-viewer).

## 1) Empty Expression

Let's start with an empty expression. This is the test case that you will need.
```rs
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn blank_expression() {
        let token_list = Vec::new();
        let result = shunting_yard(token_list);
        assert_eq!(false, result.is_ok());
    }
}
```

This test expects "shunting_yard" function to return an error, because it is not valid to use Shunting Yard algorithm with empty token list.

Try to run the test using "cargo test" and see compile error. Of course, we did not have "shunting_yard" function. Let's make it on top of the "lib.rs".
```rs
pub fn shunting_yard(token_list: Vec<String>) -> Result<Vec<String>, String> {
    return Err("Empty token list".to_owned());
}
//... the rest of the code
```
Now, try to run the test again. The test case passes. This is the difference before and after you add minimum code to pass.

NOTE: The diff file can be found [here](https://gist.github.com/iamdejan/1cd3b9fe297f57f668f812c6e26ccf83#file-0001-diff).

## 2) Simple expression: 1 + 1

Now, add this test case. Note that in order to ease us, we have to separate each token (both operators and operands) with space.
```rs
//... the rest of the code

    #[test]
    fn simple_expression() {
        let token_list: Vec<String> = "1 + 1".to_owned()
            .split_ascii_whitespace()
            .map(|s| s.to_string())
            .collect();
        let result = shunting_yard(token_list);
        assert_eq!(true, result.is_ok());

        let expected_token: Vec<String> = "1 1 +".to_owned()
            .split_ascii_whitespace()
            .map(|s| s.to_string())
            .collect();
        assert_eq!(expected_token, result.unwrap());
    }
}
```

The test case will fail. Now, add minimum code to pass the test:
```rs
pub fn shunting_yard(token_list: Vec<String>) -> Result<Vec<String>, String> {
    if token_list.is_empty() {
        return Err("Empty token list".to_owned());
    }

    return Ok("1 1 +".to_owned().split_ascii_whitespace().map(|s| s.to_string()).collect());
}

//... the rest of test cases
```

NOTE: The diff file can be found [here](https://gist.github.com/iamdejan/1cd3b9fe297f57f668f812c6e26ccf83#file-0002-diff).

## 3) Simple expression with 2 operators: 1 + 2 - 3

Add this test case:
```rs
//... the rest of the code

    #[test]
    fn simple_expression_two_operators() {
        let token_list: Vec<String> = "1 + 2 - 3".to_owned()
            .split_ascii_whitespace()
            .map(|s| s.to_string())
            .collect();
        let result = shunting_yard(token_list);
        assert_eq!(true, result.is_ok());

        let expected_token: Vec<String> = "1 2 + 3 -".to_owned()
            .split_ascii_whitespace()
            .map(|s| s.to_string())
            .collect();
        assert_eq!(expected_token, result.unwrap());
    }
}
```

Run "cargo test" and it will fail. Now, write minimum code to pass the test. It will be like this:
```rs
use std::collections::LinkedList;

fn is_operand(token: &String) -> bool {
    return token.parse::<i64>().is_ok();
}

fn is_operator(token: &String) -> bool {
    return !is_operand(token);
}

pub fn shunting_yard(token_list: Vec<String>) -> Result<Vec<String>, String> {
    if token_list.is_empty() {
        return Err("Empty token list".to_owned());
    }

    let mut output_queue: Vec<String> = Vec::new();
    let mut operator_stack: LinkedList<String> = LinkedList::new();
    for i in 0..token_list.len() {
        let token = token_list[i].clone();
        if is_operator(&token) {
            while !operator_stack.is_empty() {
                let popped_operator: String = operator_stack.pop_back().unwrap();
                output_queue.push(popped_operator);
            }
            operator_stack.push_back(token);
        } else {
            output_queue.push(token);
        }
    }
    while !operator_stack.is_empty() {
        let popped_operator: String = operator_stack.pop_back().unwrap();
        output_queue.push(popped_operator);
    }
    return Ok(output_queue);
}

//... the rest of test cases
```

Whoa whoa! How this works? Let's look at part below the first "if". We declare "output_queue" and "operator_stack" for the algorithm. Next, for each of token, we will check if the token is operator / operand. If the token is operator, pop all tokens from "operator_stack" to "output_queue", and push current token.

NOTE: The diff file can be found [here](https://gist.github.com/iamdejan/1cd3b9fe297f57f668f812c6e26ccf83#file-0003-diff).

## 4) Simple expression with 2 operators: 1 * 2 / 1

Add this test case:
```rs
//... the rest of the code

    #[test]
    fn simple_expression_two_operators_multiplication_and_division() {
        let token_list: Vec<String> = "1 * 2 / 1".to_owned()
            .split_ascii_whitespace()
            .map(|s| s.to_string())
            .collect();
        let result = shunting_yard(token_list);
        assert_eq!(true, result.is_ok());

        let expected_token: Vec<String> = "1 2 * 1 /".to_owned()
            .split_ascii_whitespace()
            .map(|s| s.to_string())
            .collect();
        assert_eq!(expected_token, result.unwrap());
   }
}
```

Try to run the test case, and the test case will pass. Why? Because the operators are still on same precedence.

NOTE: The diff file can be found [here](https://gist.github.com/iamdejan/1cd3b9fe297f57f668f812c6e26ccf83#file-0004-diff).

## 5) Expression with 2 operators and 2 operator precedences: 1 + 2 * 3

Add this test case:
```rs
//... the rest of the code

    #[test]
    fn expression_two_operators_two_precedences() {
        let token_list: Vec<String> = "1 + 2 * 3".to_owned()
            .split_ascii_whitespace()
            .map(|s| s.to_string())
            .collect();
        let result = shunting_yard(token_list);
        assert_eq!(true, result.is_ok());

        let expected_token: Vec<String> = "1 2 3 * +".to_owned()
            .split_ascii_whitespace()
            .map(|s| s.to_string())
            .collect();
        assert_eq!(expected_token, result.unwrap());
    }
}
```

Try to run this this. It will fail, because we don't take care of operator precedence.

This is the rest of the code to pass the test case:
```rs
use std::collections::LinkedList;

fn is_operand(token: &String) -> bool {
    return token.parse::<i64>().is_ok();
}

fn is_operator(token: &String) -> bool {
    return !is_operand(token);
}

fn get_operator_level(token: &String) -> i64 {
    if token == "*" || token == "/" {
        return 2;
    }

    return 1;
}

fn left_operator_has_greater_precedence(left_token: &String, right_token: &String) -> bool {
    return get_operator_level(left_token) >= get_operator_level(right_token);
}

pub fn shunting_yard(token_list: Vec<String>) -> Result<Vec<String>, String> {
    if token_list.is_empty() {
        return Err("Empty token list".to_owned());
    }

    let mut output_queue: Vec<String> = Vec::new();
    let mut operator_stack: LinkedList<String> = LinkedList::new();
    for i in 0..token_list.len() {
        let token = token_list[i].clone();
        if is_operator(&token) {
            while !operator_stack.is_empty() && left_operator_has_greater_precedence(operator_stack.back().unwrap(), &token) {
                let popped_operator: String = operator_stack.pop_back().unwrap();
                output_queue.push(popped_operator);
            }
            operator_stack.push_back(token);
        } else {
            output_queue.push(token);
        }
    }
    while !operator_stack.is_empty() {
        let popped_operator: String = operator_stack.pop_back().unwrap();
        output_queue.push(popped_operator);
    }
    return Ok(output_queue);
}

//... the rest of test cases
```

Remember that we have multiplication (*) and addition (+) in this test case. As we know, they have different priorities. Multiplication (or division) expression **is calculated first**, then addition (or subtraction). So, we have to make sure in our postfix expression that operators with greater precedence are inserted to the output stack before the other operators. So, what do we do? Well, we need to make sure that **we pop from the operator stack until the top of the stack has less precedence than current operator**.

What about parentheses? It will be implemented on the next step.

NOTE: The diff file can be found [here](https://gist.github.com/iamdejan/1cd3b9fe297f57f668f812c6e26ccf83#file-0005-diff).

## 6) Expression with parentheses: 4 + 18 / (9 - 3)

It's time to add the test case with parentheses:
```rs
//... the rest of the code

    #[test]
    fn expression_with_parentheses() {
        let token_list: Vec<String> = "4 + 18 / ( 9 - 3 )".to_owned()
            .split_ascii_whitespace()
            .map(|s| s.to_string())
            .collect();
        let result = shunting_yard(token_list);
        assert_eq!(true, result.is_ok());

        let expected_token: Vec<String> = "4 18 9 3 - / +".to_owned()
            .split_ascii_whitespace()
            .map(|s| s.to_string())
            .collect();
        assert_eq!(expected_token, result.unwrap());
    }

}
```

If you run the test case, it will fail. Why? Because we haven't handled parentheses. Parentheses add complexity of Shunting-Yard algorithm, because that means we cannot pop all operators with greater precedence.

Now, add minimum code to pass the test case. This is code after adding minimum code:
```rs
use std::collections::LinkedList;

fn is_operand(token: &String) -> bool {
    return token.parse::<i64>().is_ok();
}

fn is_operator(token: &String) -> bool {
    return !is_operand(token) && token != "(" && token != ")";
}

fn get_operator_level(token: &String) -> i64 {
    if token == "*" || token == "/" {
        return 2;
    }

    return 1;
}

fn left_operator_has_greater_precedence(left_token: &String, right_token: &String) -> bool {
    return get_operator_level(left_token) >= get_operator_level(right_token);
}

pub fn shunting_yard(token_list: Vec<String>) -> Result<Vec<String>, String> {
    if token_list.is_empty() {
        return Err("Empty token list".to_owned());
    }

    let mut output_queue: Vec<String> = Vec::new();
    let mut operator_stack: LinkedList<String> = LinkedList::new();
    for i in 0..token_list.len() {
        let token = token_list[i].clone();
        if is_operand(&token) {
            output_queue.push(token);
        } else if is_operator(&token) {
            while !operator_stack.is_empty() && left_operator_has_greater_precedence(operator_stack.back().unwrap(), &token) {
                if operator_stack.back().unwrap() == "(" || operator_stack.back().unwrap() == ")" {
                    break;
                }
                let popped_operator: String = operator_stack.pop_back().unwrap();
                output_queue.push(popped_operator);
            }
            operator_stack.push_back(token);
        } else if token == "(" {
            operator_stack.push_back(token);
        } else if token == ")" {
            while !operator_stack.is_empty() && operator_stack.back().unwrap() != "(" {
                let popped_operator: String = operator_stack.pop_back().unwrap();
                output_queue.push(popped_operator);
            }
            operator_stack.pop_back().unwrap();
        }
    }

    while !operator_stack.is_empty() {
        let popped_operator: String = operator_stack.pop_back().unwrap();
        output_queue.push(popped_operator);
    }

    return Ok(output_queue);
}
```

Since the changes are quite much, I recommend you to open this [diff file](https://gist.github.com/iamdejan/1cd3b9fe297f57f668f812c6e26ccf83#file-0006-diff).

First change is to make sure that operator does not include left and right parenthesis. This could break our implementation.

Next, we make sure that we pop only until left parenthesis. Why? Because **operators before parenthesis must be evaluated after the parenthesis is evaluated**. That means that we have to keep the operators before left parenthesis at the stack.

Next, we evaluate the parentheses. If it's left parenthesis, just put into operator stack. If it's right parenthesis, we will pop from operator stack and insert to output queue until left parenthesis is found. Then, we discard (by popping) the left parenthesis.

## 7) Expression with parentheses: (5 * 4 + 3) - 1

Now, try to add this test case. It should pass.
```rs
//... the rest of the code

    #[test]
    fn simple_expression_two_operators_multiplication_and_division() {
        let token_list: Vec<String> = "( 5 * 4 + 3 ) - 1".to_owned()
            .split_ascii_whitespace()
            .map(|s| s.to_string())
            .collect();
        let result = shunting_yard(token_list);
        assert_eq!(true, result.is_ok());

        let expected_token: Vec<String> = "5 4 * 3 + 1 -".to_owned()
            .split_ascii_whitespace()
            .map(|s| s.to_string())
            .collect();
        assert_eq!(expected_token, result.unwrap());
   }
}
```

NOTE: The diff file can be found [here](https://gist.github.com/iamdejan/1cd3b9fe297f57f668f812c6e26ccf83#file-0007-diff).

## 8) Expression with nested parentheses: (1 + 2) * 3 + 4 * ((7 - 5) + 6)

Now, try to add this test case. It should pass.
```rs
//... the rest of the code

    #[test]
    fn simple_expression_two_operators_multiplication_and_division() {
        let token_list: Vec<String> = "( 1 + 2 ) * 3 + 4 * ( ( 7 - 5 ) + 6 )".to_owned()
            .split_ascii_whitespace()
            .map(|s| s.to_string())
            .collect();
        let result = shunting_yard(token_list);
        assert_eq!(true, result.is_ok());

        let expected_token: Vec<String> = "1 2 + 3 * 4 7 5 - 6 + * +".to_owned()
            .split_ascii_whitespace()
            .map(|s| s.to_string())
            .collect();
        assert_eq!(expected_token, result.unwrap());
   }
}
```

NOTE: The diff file can be found [here](https://gist.github.com/iamdejan/1cd3b9fe297f57f668f812c6e26ccf83#file-0008-diff).

## 9) Expression with nested parentheses: 2 * (3 + 5) / 4 + 1 * 9 + (4 * (6 / 3))

Now, try to add this test case. It should pass.
```rs
//... the rest of the code

    #[test]
    fn simple_expression_two_operators_multiplication_and_division() {
        let token_list: Vec<String> = "2 * ( 3 + 5 ) / 4 + 1 * 9 + ( 4 * ( 6 / 3 ) )".to_owned()
            .split_ascii_whitespace()
            .map(|s| s.to_string())
            .collect();
        let result = shunting_yard(token_list);
        assert_eq!(true, result.is_ok());

        let expected_token: Vec<String> = "2 3 5 + * 4 / 1 9 * + 4 6 3 / * +".to_owned()
            .split_ascii_whitespace()
            .map(|s| s.to_string())
            .collect();
        assert_eq!(expected_token, result.unwrap());
   }
}
```

NOTE: The diff file can be found [here](https://gist.github.com/iamdejan/1cd3b9fe297f57f668f812c6e26ccf83#file-0009-diff).

## Finished Code

You can see finished code for Shunting Yard algorithm at [my repository](https://github.com/iamdejan/rust-shunting-yard).

# How to Calculate Reverse Polish Notation?

We get output queue at the end of our implementation. But, don't forget. We just convert the expression, not evaluating it. Evaluating the expression requires another algorithm.

How do we evaluate postfix expression? Here is the pseudocode:
```
for each token in output queue:
    if it's operator:
        get 2 operands before
        calculate the expression with popped operands
        replace current oprator with expression result in output queue
        remove 2 operands before current index

return first token in output queue
```

But, how about the implementation? You can try my implementation in the Rust Playground.
```rs
fn is_operator(token: &String) -> bool {
    let token_str = token.as_str();
    return token_str == "+" || token_str == "-" || token_str == "*" || token_str == "/";
}

pub fn evaluate(mut postfix_expression: Vec<String>) -> i64 {
    let mut i: usize = 0;
    while i < postfix_expression.len() {
        let token: String = postfix_expression[i].clone();

        if is_operator(&token) {

            let left_operand: i64 = postfix_expression[i - 2].parse().unwrap();
            let right_operand: i64 = postfix_expression[i - 1].parse().unwrap();

            match token.as_str() {
                "+" => {
                    let result: i64 = left_operand + right_operand;
                    postfix_expression[i] = result.to_string();
                },
                "-" => {
                    let result: i64 = left_operand - right_operand;
                    postfix_expression[i] = result.to_string();
                },
                "*" => {
                    let result: i64 = left_operand * right_operand;
                    postfix_expression[i] = result.to_string();
                },
                "/" => {
                    let result: i64 = left_operand / right_operand;
                    postfix_expression[i] = result.to_string();
                },
                _ => {},
            }

            postfix_expression.remove(i - 1); i -= 1;
            postfix_expression.remove(i - 1); i -= 1;

        }

        i += 1;
    }

    let result: i64 = postfix_expression[0].parse().unwrap();
    return result;
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn expression() {
        let postfix_expression: Vec<String> = "1 1 +".to_owned()
            .split_ascii_whitespace()
            .map(|s| s.to_string())
            .collect();

        assert_eq!(2, evaluate(postfix_expression));
    }

    #[test]
    fn complex_expression() {
        let postfix_expression: Vec<String> = "2 3 5 + * 4 / 1 9 * + 4 6 3 / * +".to_owned()
            .split_ascii_whitespace()
            .map(|s| s.to_string())
            .collect();

        assert_eq!(21, evaluate(postfix_expression));
    }
}
```

Just like before, the part with `#[cfg(test)]` is for test cases. Try to run both tests, those will pass.

What actually do we do? We iterate each token. If current token is operator, then we calculate the result and replace current token. After that, we remove 2 operands before current token. The result is the only remaining token.

# The Hardest Part: Converting The Expression Into Tokens

So far, I've told you about converting and evaluating expressions. But, do you notice that I put space between tokens? It's done on purpose. Why? So we don't need to process the tokens. But, in real life, you might see people write expressions without spaces. And that's the hardest part for me: converting the expression people wrote into tokens. I will leave this part to you :-)

I hope you enjoy & learn something through reading my article. If you have any questions, just put into the comments. Thank you & have a good day!
