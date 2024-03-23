+++
title = "Android Paintflags Configuration, Explained"
date = 2022-02-08T00:00:00+07:00
description = ""
type = "post"
showTableOfContents = "true"
image = "https://miro.medium.com/v2/resize:fit:720/format:webp/1*6lb-nVbLQaXesuHGSicgag.png"
+++

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*6lb-nVbLQaXesuHGSicgag.png)

When I worked on an Android Kotlin project, I stumbled upon this code:
```kotlin
if(isChecked) {
    tvTodoTitle.paintFlags = tvTodoTitle.paintFlags or Paint.STRIKE_THRU_TEXT_FLAG
} else {
    tvTodoTitle.paintFlags = tvTodoTitle.paintFlags and Paint.STRIKE_THRU_TEXT_FLAG.inv()
}
```

The purpose is simple. We need to strikethrough the text when the checkbox is checked or remove the strikethrough when the checkbox is unchecked. But for some reason, the way Android configures it is strange, at least for me.

The reason why I think it’s strange is that this is the first time I encounter bitmask as the configuration in any frameworks or SDKs. There are a few examples where the frameworks/libraries do not use bitmask:

- CSS: if you want to display something, you add an attribute directly to the class or id configuration in the CSS file.
- Laravel: most of the configurations done are using PHP’s array.
- Tailwind: you only need to use the existing class in your own view.
- Flutter: either you compose the widget class, or you just set the existing attribute the usual way (using class’ properties).

Then I realize, it’s actually a cool way to use bitmask for their configuration.

# A Brief Intro to Bitmask

Bitmask is simply an act of modifying one or more bits. More detail can be found at [Wikipedia](https://en.wikipedia.org/wiki/Mask_(computing)), but if you want, you can stay here and continue reading my explanation.

# About Bit

So, what is a bit? A bit (short term for binary digit) it’s the most basic unit or information in computing. It represents the logical value with two values: 0 (or false) and 1 (or true). The computer can only hold either value (not both) in a particular state.

The bit is also used to represent numbers in computer. Every time we process a number, the computer always processes it in binary base (2-based). Since computers already use bit, binary system is easier to process, compared to the day-to-day decimal system.
```
+-----------+--------------+
|  Decimal  | 8-Bit Binary |
+-----------+--------------+
| 0         |     00000000 |
| 1         |     00000001 |
| 2         |     00000010 |
| 3         |     00000011 |
| 4         |     00000100 |
| 5         |     00000101 |
| 6         |     00000110 |
| 7         |     00000111 |
| 8         |     00001000 |
| and so on |              |
+-----------+--------------+
```

The table shown above describes the comparison between decimal and binary systems. Whenever binary system repeats digit every 10 numbers (from 0 to10), binary repeats digit every 2 numbers (from 0, 1, then 10). Notice that I append zeros in front of the real numbers, just to show you the number representation in computer memory.

I’m not going to talk a lot about bit in depth here, just to show you how numbers are stored in bit. You can learn more about reading binary number [here](https://www.lifewire.com/how-to-read-binary-4692830). After that, go back and we’ll discuss about bit manipulation.

# Bit Manipulation and Bitmask

Do you know that you can manipulate the bit by hand (by hand I mean that you can manipulate bit in a certain position)? Well, there’s a specific way to do that, which is called bitmask.

Bitmask is a type of bit manipulation that can be used to change one or more bits. Specifically, it’s an auxiliary data (in our case, number) that can be used to change the original number.

In order to learn bit manipulations, we have to learn about bit operators first. Just like arithmetic computation, there are many bit operators that can be used with bitmask to alter integers.

```
+-------------+----------+-------------------+--------------+
|    Name     | Operator | Kotlin equivalent | Truth table? |
+-------------+----------+-------------------+--------------+
| AND         | &        | and               | yes          |
| OR          | |        | or                | yes          |
| XOR         | ^        | xor               | yes          |
| Shift left  | <<       | shl               | no           |
| Shift right | >>       | shr               | no           |
| Invert      | ~        | .inv() method     | no           |
+-------------+----------+-------------------+--------------+
```

For this post, I’m going to focus on all operators except XOR, since we need to know those operators for Paintflags configuration.

## AND and OR Operations

`AND` and `OR` operations are the most basic operations in bit manipulation. Each operation takes two numbers and produce the result according to this truth table:
```
+-----------+-----------+------------+-----------+
| Operand 1 | Operand 2 | AND result | OR result |
+-----------+-----------+------------+-----------+
|         0 |         0 |          0 |         0 |
|         0 |         1 |          0 |         1 |
|         1 |         0 |          0 |         1 |
|         1 |         1 |          1 |         1 |
+-----------+-----------+------------+-----------+
```

As you can see, `AND` operation only yields 1 if both operands are 1, while `OR` operation yields 1 if one of operands is 1. Based on this, we can use:
1) `AND` operation to change any bit to 0 (also known as unset). Example: `5 & 3 = 00000101 & 00000011 = 00000001 = 1`.
2) `OR` operation to change any bit to 1 (also known as set). Example: `5 | 3 = 00000101 | 00000011 = 00000111 = 7`.

## Inversion

Let’s say you have a number 7, represented in bits as `00000111`. What if I want to invert all bits (from 0 to 1, and from 1 to 0)? We use the inversion operator. In Java, you can invert the bits like this:
```java
int number = 7;
System.out.println((~number)); // will print -8
```

But, in Kotlin, instead of using ~ operator, you use .inv() method:
```kotlin
val number = 7
println(number.inv()) // will print -8
```

If you wonder why it prints as `-8`, it’s because the inverted bit is `11111000`. If the variable is unsigned integer (which doesn’t have negative value), it will show `4294967288`. But, because we use signed integer (integer with positive / negative sign), computer will show negative integer. You can learn more about negative integer representation [here](https://www.geeksforgeeks.org/representation-of-negative-binary-numbers/).

## Bitshift

Let’s say you have a number 6, represented by bits `00000110`. You want to multiply by power of 2 (e.g. 4), but for performance reason, you don’t want to use the multiply operator. Well, in this case, you can shift left by `sqrt(N)` times. Every shift to the left means you’re multiplying by 2. If you shift 2 times to the left, that means that you’re multiplying by 2 to the power of 2, which is 4.
```
+------------+----------+
| Shift left |   Bit    |
+------------+----------+
|          0 | 00000110 |
|          1 | 00001100 |
|          2 | 00011000 |
|          3 | 00110000 |
+------------+----------+
```

“Wait, but that means, if I shift right, I divide the number by power of 2?” YES you’re right! The reverse happens if you shift the bit to the right.

But, bit-shift isn’t only used to multiply / divide number. We can use it to control a particular bit. We’ll see more of this in Paintflags configuration.

# Paintflags Configuration

Let’s go back to the snippet at the beginning of the article:
```kotlin
if(isChecked) {
    tvTodoTitle.paintFlags = tvTodoTitle.paintFlags or Paint.STRIKE_THRU_TEXT_FLAG
} else {
    tvTodoTitle.paintFlags = tvTodoTitle.paintFlags and Paint.STRIKE_THRU_TEXT_FLAG.inv()
}
```

`paintFlags` attribute is a signed integer, hence it holds 64 bits. `STRIKE_THRU_TEXT_FLAG` is a constant with value of 16. Do you know how to represent number 16 in bits? Let’s remember that 16 is 2 to the power of 4, or in other words `1 << 4`. So, this is what number 16 would be in 64-bit integer:
```
00000000 00000000 00000000 00010000
```

Now, initially, `paintFlags` variable doesn’t set any bit. That means, all bits in `paintFlags` are `0`.

But, now, we want to apply the strike through configuration, so we decided to set the bit which shows the strike through. In this case, it’s the 5th least significant bit (meaning: 5th bit from the right). So, we have to shift left 4 times. Luckily, Kotlin already has the constant, so we just have to use OR operator to activate the setting:
```
00000000 00000000 00000000 00000000
00000000 00000000 00000000 00010000
----------------------------------- OR
00000000 00000000 00000000 00010000
```

Remember what I said about “setting” a bit? We use OR operator for that.

But, what about the `else` clause? What if we want to deactivate the strike through configuration? We just need to “unset” the 5th least significant bit. How do we unset? We use `AND` operator and make sure the 5th least significant bit in the bitmask is inverted, so that bit will be “set to 0”. Don’t worry, the other bits won’t be affected, because that’s the nature of AND operator: it changes only when the bitmask is 0.
```
00000000 00000000 00000000 00010000
11111111 11111111 11111111 11101111
----------------------------------- AND
00000000 00000000 00000000 00000000
```

Now, what happens if we, by mistake, use `OR` operator? That means we activate other properties to be shown:
```
00000000 00000000 00000000 00010000
11111111 11111111 11111111 11101111
----------------------------------- OR
11111111 11111111 11111111 11111111
```

Well, now we (maybe) gets messy UI.

## Why Use Bitmask?

Now you might wonder, why use bitmask for configurations? Why not just set or unset the usual way?

It turns out that you can have multiple configurations stored in a single integer. That helps Android saves storage. Since Android is a mobile system, being greedy with storage isn’t a wise thing to do. So maybe Android SDK developers thought that making it compact is better for the long run (which is a valid argument).

# Conclusion?

I hope now you have a high level understanding about bit and bitmask. Bit manipulation is useful not only for this kind of problem, but for other problems as well, such as [Sieve of Eratosthenes](https://cp-algorithms.com/algebra/sieve-of-eratosthenes.html), [Traveling Salesperson Problem](https://www.youtube.com/watch?v=JE0JE8ce1V0), and others.

This is just introduction, though. As you dig deeper into low-level systems, I’m pretty sure you will meet a lot of bit manipulations.
