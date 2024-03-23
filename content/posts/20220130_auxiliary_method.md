+++
title = "Learn Programming Languages with The \"Auxiliary Method\""
date = 2022-01-30T00:00:00+07:00
description = ""
type = "post"
showTableOfContents = "true"
image = "https://miro.medium.com/v2/resize:fit:640/format:webp/1*nPdEsahmk0JBHWE5RbqnIA.jpeg"
+++

![United We Stand from Yu-Gi-Oh! Trading Card Game](https://miro.medium.com/v2/resize:fit:640/format:webp/1*nPdEsahmk0JBHWE5RbqnIA.jpeg)

In this article, I want to share to you how I learn Kotlin language with a method I recently use, called "Auxiliary Method". "Auxiliary Method" is a method to learn a new programming language with help with another programming language that we already master.

# In a Nutshell: Why Kotlin?

Why I choose Kotlin? Well, because Kotlin currently is de jure supported programming language for Android development\* by Google. We want our application to be maintainable as long as possible, so we use a programming language that is officially supported.

Another factor is that Kotlin maintains type-safety by using "optional" (a wrapper class to contain actual value and prevent Null Pointer Exception) for a lot of their standard libraries, by using the question ("?") mark.

\* I guess Java is still quite popular for Android development, albeit declining.

# How to Learn A New Programming Language?

Before I explain about "Auxiliary Language" technique, I want to show you how I usually learn new programming languages. In short, I learn from tutorial videos first to know learn and understand the syntax, then I use Leetcode to apply my knowledge gained from tutorial.

## Know the Language First

You need to learn from tutorials. Why? Because you need to learn the syntax first and how to use them. For Kotlin, I recommend you to learn from these sources:
- [Kotlin Crash Course](https://www.youtube.com/watch?v=5flXf8nuq60)
- [Classes, Objects and Constructor in Kotlin — Kotlin Tutorial](https://www.youtube.com/watch?v=OUmBrKQfEAE)

After finishing the tutorials, now it's time to get used with the language.

## Get Used to the Language with Leetcode

After learning basic syntax from tutorials, challenge yourself by solving problems from Leetcode.

What is [Leetcode](https://leetcode.com/)? Leetcode is an online judge platform (similar to Hackerrank, Codeforces, Geeks For Geeks, Kattis, etc.) that gives us programming problems to solve. Problems in Leetcode usually are defined by its description, input (in a parameter), and output (as a return value). We write the code to try and solve those problems. Our code will be scored based on whether the actual output (generated from our code) matches expected output or not. If all actual output matches, then our code is "Accepted", meaning that our program has correct logic. Otherwise that means that we have to fix our code, either because of wrong syntax or wrong semantics.

Why online judge platforms? Well, based on my experience, online judge platforms allow us as programmers to apply what we learn at tutorial when solving coding questions. Also, solving programming questions allow us to **feel the "taste"** (the paradigm / the design) of that programming language, therefore we can feel whether we're comfortable or not with the language.

Using online judge also allows us to compare our codes (if accepted) with other programmers' solutions, whether our code is cleaner or not. Not just codes, though. We can also compare run time and memory usage between our codes and other people's codes.

## The Old Way

I used to directly try new programming languages to solve Leetcode problems. The way I do it is I try to think the logic first, then implement the program.

However, one crucial drawback arises. When I use this way, I'm thinking about the problem solving, but then I get distracted by Googling because when I try to implement, I forget the syntax.

I realized now that this maybe not an effective way to learn new programming languages. Why? Because solving coding questions at online judge platforms is a combination between problem solving and code implementation (in this case, using new language). When we try to solve a problem and we get distracted, most likely we will forget what we're thinking.

# The New Way: The "Auxiliary Method"

I start changing the approach when realizing that I should code in a language that I'm comfortable with (hence why it's called "auxiliary method"), then finding equivalent or similar syntax in new programming language (in this case, Kotlin). Kotlin is a new language for me, so better to be focused on solving the problem in programming language that I (or you) already master, then use new programming language. Remember: our goal is to learn new programming language, not to learn problem solving.

How to use "Auxiliary Method"? The way you use this method is as follows:

1) Choose a problem from Leetcode (or any other online judge platforms).
2) Choose one programming language that you already master or you're comfortable with. We call this as "auxiliary language". For example, my auxiliary languages are Java 8 and Python 3, depending on the problem.
3) Solve that problem with the auxiliary language first.
4) Then, try to mimic everything. From the for-loop, if-else, or other logics you do in your auxiliary language. If you can't find the equivalent syntax, find the nearest syntax. Do not deviate too much from your original logic.

Are there any drawbacks? The only drawback that I can think of from this technique is that you need to master at least 1 programming language. This will not work if you're learning your first programming language.

Okay then, let's start.

## Problem 1827. Minimum Operations to Make the Array Increasing

Leetcode question: [here](https://leetcode.com/problems/minimum-operations-to-make-the-array-increasing/).

So, if you read the problem statement, basically you have to return "minimum number of operations needed to make the array strictly increasing". Let's dissect the problem:

1) Strictly increasing: the next element's value has to be greater than the current's element. For example, an array `[1,2,2]`. Second element (2) is greater than first element (1), while third element is NOT greater than the second element. Hence, this array is NOT strictly increasing.
2) Minimum number of operations: the array given into the method may not be strictly increasing, so we have to modify the array. The modification is called operation. The only operation allowed in this problem is to increment element's value by 1.

Now that we understand the problem, let's code the solution using our auxiliary language:
```py
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        if len(nums) == 1:
            return 0
        curr_max = nums[0]
        min_ops = 0
        for i in range(1, len(nums)):
            if nums[i] > curr_max:
                curr_max = nums[i]
                continue
            new_num = curr_max + 1
            min_ops = min_ops + abs(new_num - nums[i])
            curr_max = new_num
        return min_ops
```

The solution is simple:
1) If array's length is 1, return 0. It's written in the problem's description that an array with only 1 element is already strictly increasing.
2) The rest is just to check whether the current element is already greater than previous element. If it's already greater, set current maximum value as current element's value. Else, just increment current maximum value by 1 and calculate the different between current element's value and current maximum value.

Now we know the solution, how to do it in Kotlin? Remember, **find the similar or equivalent syntax**.
```kotlin
class Solution {
    fun minOperations(nums: IntArray): Int {
        if(nums.size <= 1) {
            return 0
        }
        var currMax = nums[0]
        var minOps = 0
        for(i in 1..(nums.size - 1)) {
            val num = nums[i]
            if(num > currMax) {
                currMax = num
                continue
            }
            val newMax = currMax + 1
            minOps += newMax - num
            currMax = newMax
        }
        return minOps
    }
}
```

Try compare both programs. If you already finished those Kotlin tutorials, then there's nothing strange in Kotlin program, just normal variable assignment and iteration using Kotlin's `IntRange`.

Also, note that `currMax` and `minOps` variables are designed to change as the program goes along, that's why we use `var` instead of `val` keyword in newMax variable. The keyword `val` is equivalent to `const` in Typescript and `final` in Java.

## Problem 1588. Sum of All Odd Length Subarrays

Leetcode question: [here](https://leetcode.com/problems/sum-of-all-odd-length-subarrays/).

Now, the problem asks for "sum of all odd-length subarrays". Let's break this down:
1) Subarray: generally, subarray means an array that's constructed by 2 / more existing elements in the present array. But in this case, subarray is also "contiguous", which means subarray is built based on continuous elements.
2) Odd-length: every subarray has a length, just like an array. But, the problem asks for sum of odd-length subarrays, which means the length of the subarray must be an odd number. So, we have to exclude empty & even-length subarrays.

How to do it? Here's how to do it in Python 3:
```py
class Solution:
    def sumOddLengthSubarrays(self, arr: List[int]) -> int:
        result = 0
        for length in range(1, len(arr)+1, 2):
            for i in range(len(arr)):
                if i + length > len(arr):
                    continue
                subarray = arr[i:i+length]
                result = result + sum(subarray)
        return result
```

The solution is to determine the length of the subarray first. For every length possible, we take a slice (yes, that's a term in Python & Kotlin) from the original array. The slice starts from `i`, with the slice length of `length` variable.

Here's how to do it in Kotlin:
```kotlin
class Solution {
    fun sumOddLengthSubarrays(arr: IntArray): Int {
        var result: Int = 0
        for(length in (1..arr.size).step(2)) {
            for(i in 0..(arr.size - 1)) {
                if((i + length - 1) >= arr.size) {
                    continue;
                }
                val subarray = arr.slice(i..(i + length - 1))
                result += subarray.sum()
            }
        }
        return result
    }
}
```

Try compare both programs. In Kotlin, if we want to do the `for` iteration but with skipping one element (in other language, jump to next 2 elements), we use the method `step` at the range, hence why the code is `for(length in (1..arr.size).step(2))`.

The other thing is that Python's array slicing is "exclusive", while Kotlin's slicing (not only slicing, but also the IntRange  used in for loop) is "inclusive". Read more about "inclusive vs exclusive" [here](https://stackoverflow.com/questions/39010041/what-is-the-meaning-of-exclusive-and-inclusive-when-describing-number-ranges).

## Problem 1700. Number of Students Unable to Eat Lunch

Leetcode question: [here](https://leetcode.com/problems/number-of-students-unable-to-eat-lunch/).

The problem ask to return number of students unable to eat lunch. But, how the "eat" mechanism works? The students stand in a queue, and the sandwiches are in stack. If the student favors same flavor as the same sandwich, the student is out from the queue, while sandwich is out from the stack. What if the student doesn't favor the sandwich on top of the stack? Then the student goes to the end of the queue (hence why called "unable to eat lunch").

Here's the solution in Java 8:
```java
class Solution {
    public int countStudents(int[] students, int[] sandwiches) {
        final LinkedList<Integer> studentList = new LinkedList<>();
        for(int student: students) {
            studentList.addLast(student);
        }
        final LinkedList<Integer> sandwichList = new LinkedList<>();
        for(int sandwich: sandwiches) {
            sandwichList.addLast(sandwich);
        }

        int count = 0;
        while(sandwichList.size()>0 && count<studentList.size()) {
            int student = studentList.removeFirst();
            int sandwich = sandwichList.getFirst();
            if(student == sandwich) {
                sandwichList.removeFirst();
                count = 0;
            } else {
                studentList.addLast(student);
                count++;
            }
        }
        return studentList.size();
    }
}
```

How to do it in Kotlin? Here's how:
```kotlin
import java.util.LinkedListclass Solution {
    fun countStudents(students:IntArray, sandwiches:IntArray): Int {
        val studentList = LinkedList<Int>()
        for(student in students) {
            studentList.addLast(student)
        }        val sandwichList = LinkedList<Int>()
        for(sandwich in sandwiches) {
            sandwichList.addLast(sandwich)
        }

        var count = 0
        while(sandwichList.size>0 && count<studentList.size) {
            val student = studentList.removeFirst()
            val sandwich = sandwichList.getFirst()
            if(student == sandwich) {
                sandwichList.removeFirst()
                count = 0
            } else {
                studentList.addLast(student)
                count++
            }
        }
        return studentList.size
    }
}
```

Wait, what? Why Kotlin is able to use the same LinkedList as Java? Because Kotlin is "interoperable" with Java, meaning Kotlin can communicate with Java code and use Java's standard libraries. But, doesn't Kotlin have their own list? Yes, Kotlin has their own list implementation, but in this case, it's better to use what's available & fast, which is Java's LinkedList.

## Problem 55. Jump Game

Leetcode question: [here](https://leetcode.com/problems/jump-game/).

The problem asks you whether it's possible to jump (either directly / transiting) from first element to last element. Each array's element value (`n`) means that from this element, I can jump at most `n` elements. If the value is `0`, that means you're stuck at that element and cannot jump anywhere. Note that you cannot jump backward.

How to do it in Java? Here's how:
```java
class Solution {
    public boolean canJump(int[] nums) {
        if(nums.length == 1) {
            return true;
        }
        int i = nums.length - 2;
        int targetIndex = nums.length - 1;
        for(; i > 0; i--) {
            if(i + nums[i] >= targetIndex) {
                targetIndex = i;
            }
        }
        return (i + nums[i] >= targetIndex);
    }
}
```

Wait, why jump backward? Because you want to know whether it's possible to jump from first to last element, but you don't know whether to jump full ability (jump `n` elements) / not. So, now we reverse the thought process, and instead ask "Can I reach first element from last element?"

There's a second solution, using Dynamic Programming (DP). With DP, you can actually try all possibilities (jumping from 1 to `n` elements). But the problem is, DP solution for a greedy problem is slow.

How to solve in Kotlin? Here's how:
```kotlin
class Solution {
    fun canJump(nums: IntArray): Boolean {
        if(nums.size == 1) {
            return true
        }        var targetIndex = nums.size - 1
        var i = nums.size - 2
        while(i > 0) {
            if(i + nums[i] >= targetIndex) {
                targetIndex = i
            }
            i -= 1
        }
        return (i + nums[i] >= targetIndex)
    }
}
```

One drawback from Kotlin is that we cannot do backward in for iteration, because for loop in Kotlin is not flexible enough. Yes it's easier to do a forward for loop in Kotlin, either by specifying the range, by indices method (e.g. for(i in arr.indices)), or by foreach loop (e.g. for(number in numbers)). But, there's no easy way to do a backward iteration. The nearest syntax for backward loop is to use while loop, which is what I use.

# The End

Well, we reach the the end of the article. As I said earlier, this works for me because I can focus on applying syntax. I hope this works for you as well, although there is no guarantee this will work for others. My message to you is: keep learning & don't give up.

NOTE: For you who asked where are the import statements, that's because Leetcode automatically import most of Java libraries, so we don't need to reimport those.
