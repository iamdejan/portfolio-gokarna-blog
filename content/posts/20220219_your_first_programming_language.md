+++
title = "Your First Programming Language"
date = 2022-02-19T00:00:00+07:00
description = ""
type = "post"
showTableOfContents = "true"
image = "https://miro.medium.com/v2/resize:fit:720/format:webp/1*73ptjXXKoiLFBEoaMiGBlA.jpeg"
+++

![A journey of a thousand miles begins with a single step.](https://miro.medium.com/v2/resize:fit:720/format:webp/1*73ptjXXKoiLFBEoaMiGBlA.jpeg)

I said on a [post](/posts/20220130_auxiliary_method/) I wrote earlier that “Auxiliary Method” is only useful for learning 2nd, 3rd, 4th, and next programming language. That's true. While that post is helpful for existing programmers, that post cannot be used for people who are interested in learning programming from zero.

So what is the best programming language for first-time programmers?

# Different Programmers, Different Opinions

If you ask different programmers, you will get different answers. That's expected because programmers' opinions differ on what is an ideal programming language for beginners. Some prefer simplicity, while others prefer more controls. Some prefer readability, others prefer performance*. Hence why it's an endless debate about what is the best programming language.

And if you're new to the programming world, you will not notice at the beginning, but then you will realize that programming languages, just like other things, have fanboys. These fanboys fight for their “favorite” programming languages across social media, e.g. YouTube and Reddit. They ferociously defend their favorite programming language and thus draw newbies away.

So instead I will tell my experience on my first programming language, then we will look at things that I can change if I can turn back time (a.k.a. my recommendations).

DISCLAIMER: As I am a normal human, I also have my personal biases, but I make this article in good faith and try my best to present languages as fairly according to my experiences with them.

* High-performing codes and readable codes aren't always mutually exclusive. But, often that's the case, especially for simple programs.

# My Experiences

## Arduino

So my first experience with programming doesn't actually start at college, but rather with Arduino. If you don't know, Arduino allows you to actually program the board. The most basic program that everyone usually teaches is Arduino's blinking light, where you can turn on and off the light at a certain duration. But, it's also possible to do something more complex, such as converting texts into morse codes and controlling electrical currents given a certain circumstance.

If you have an Arduino (it's okay if you don't have one) and you want to learn Arduino's programming similar to the way I did it, you can learn from a book called [Programming Arduino: Getting Started With Sketches](https://www.amazon.com/Programming-Arduino-Getting-Started-Sketches/dp/1259641635/). It's okay for beginners, but there are some concepts (such as pointers) that I think beginners should not learn until they gain enough understanding of the programming side.

## C Language

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*UlXqJuBWTj7n45IjsEPe5g.png)

When I entered univerisity, the first programming language introduced to me is C. Yes, not even C++. It's the C programming language that Dennis Ritchie invented\*.

We learned about standard input (stdin), standard output (stdout), for loops, while loops, basic logic branching (if-else, switch-case), and a lot more. But what's interesting is that we're also taught of pointers. Not only in the concept, but since we're taught in C, we actually created the pointer by hand. And we also need to “free” the memory from the pointer once we're done with the memory accessed by the pointer.

Despite I enjoy the class, I don't think C (and C++ for that matter) is a good programming language for beginners. Why?
1) Pointers are manually managed: when you learn the first programming language, you want to understand how a program can be made using a set of instructions that you code. You don't want to mess with computer memory. But in C, if you create a pointer and you forget to “free” (release) the memory, you might inadvertently create a memory leak\*\*.
2) Unintuitive functions: when you learn C, there are some “basic” methods that is considered unintuitive to use. For example, take `printf` function. If you look at the examples of `printf`, you will see that it's not the easiest way to print or show dynamic texts on the screen. Let's look at the comparison between Python (`print(f”My name is {name}”)`) and C (`printf(“My name is %s”, name)`). You might be confused about the usage of `%s`, and then you have to memorize different letters for different data types. There are some methods that accept pointers but not variables as parameters, or vice versa.
3) Integer vs decimal: in some programming languages (e.g. Python, Ruby), there is only 1 type of number\*\*\*. This generic number type includes both integer and float (a term for decimal in programming), and thus making arithmetic operations easier. But in the case of C (and C++), there are actually 2 types of number, which is integer and float. This *shouldn't* be a problem, but there is a case where you want to divide but the dividend is not divisible by the divisor (e.g. `3 divided by 2`). In some languages, `3 / 2` results in `1.5`, but in C (and C++) it results in `1`. Why? That has something to do with integer division, which you can learn later after you gain enough understanding about programming languages.

I think three reasons are enough to explain why C is not suitable for first programming language. That doesn't mean that C is bad, it's just not suited for beginners.

\* Binus University taught either C99 (a version of C language that's released on 1999) and/or C11 (released on 2011). So, not in the sense of the original C that Dennis invented. But the syntax mostly is still the original syntax.

\*\* Memory leak is a bug where the computer doesn't have enough memory to use because some unused data are not cleaned up.

\*\*\* Maybe there is a differentiation between integer and decimal numbers in “easier” programming languages, but some languages are able to hide it.

# My Recommendations

Obviously, my learning path for a programmer is far from ideal. Often I wish that I can change my approach in learning programming, including choosing my first programming language. If I can turn back time, here are my recommendations.

## General Purpose

For general purpose programming language, basically my recommendations have to fulfill these criteria:
- Written in procedural way\*: there are many ways to write and structure code. The easiest way is procedural, where you write the code “as procedure.” This is the easiest because the structure is simple and you don't have to understand many concepts.
- Standard input/output: your first program will most likely be written in command prompt (a.k.a. terminal in Linux and Mac). Why? Because it's the easiest interface for programming. You don't need to create and manage windows to actually see the output. You start with a command prompt first, then once you understand the basics, you can continue with graphical applications.

With these criteria, I recommend you these programming languages:
1) Ruby: Ruby is a programming language that is designed as easily as possible. Ruby is considered easy because the syntax is designed to be beginner-friendly and Ruby is able to hide its complexity for beginners. It's easy up to the point where Ruby is considered a “magical” language. [This article](https://www.simonewebdesign.it/ruby-is-magic/) sums up well about Ruby's magic. Ruby is also used for websites and game development. If you want to learn Ruby, you can try watching [Ruby tutorial from Derek Banas](https://www.youtube.com/watch?v=Dji9ALCgfpM). The video is old, but Ruby doesn't change much**.
2) Python: Python is the most popular programming language for beginners. The reason Python is easy is similar to Ruby, which is Python is able to hide its complexities for beginners. Not only that, but the ecosystem*** is much larger than Ruby, since Python is used in [machine learning](https://www.tensorflow.org/), [server](https://fastapi.tiangolo.com/), [DevOps](https://www.ansible.com/), and [game development](https://www.pygame.org/news). Python's tutorials are abundant in the internet, but the ones that are popular in the internet are from [Tech With Tim](https://www.youtube.com/watch?v=sxTmJE4k0ho) and [TechWorld with Nana](https://www.youtube.com/watch?v=t8pPdKYpowI). Once you grasp the concept, you can continue with Raspberry Pi's path way for Python.
3) Kotlin: this seems absurd, because Kotlin is [*de jure* officially supported programming language for Android development](https://developer.android.com/kotlin/first). However, in my point of view, Kotlin is easy enough for first-time programmers (although not as easy as Python and Ruby), since Kotlin's syntax is shorter, thus making it easier for beginners. You can watch Kotlin crash course by [Traversy Media](https://www.youtube.com/watch?v=5flXf8nuq60). As for standard input to command prompt, you can read [this answer from StackOverflow](https://stackoverflow.com/a/41283570).

\* This doesn't mean that the programming language shouldn't have other paradigms (such as object-oriented programming). This just means that you don't write a class / other complex stuffs to create a Hello world program.

\*\* Most of the time, programming languages add features, not change the syntax. Although there is an example of syntax change from Python 2 to Python 3, it is quite rare.

\*\*\* You don't have to learn all tools in Python's ecosystem straight away. You just need to make sure that you're comfortable with Python first, then you can learn Python's ecosystem.

## Specific Purpose

I actually do not recommend directly programming for a specific purpose. The reason is that your understanding of programming might be limited by the specific purpose\* that you want to pursue. That being said, if you feel like you don't need to learn programming in general and/or you just want to solve a problem quickly, here are my recommendations:
- Blockchain: there is [Solidity programming language](https://soliditylang.org/) that is used to write [smart contracts](https://www.youtube.com/watch?v=ZE2HxTmxfrI) in [Ethereum](https://ethereum.org/en/). Tutorial for Solidity can be found on [freeCodeCamp.org channel](https://www.youtube.com/watch?v=ipwxYa-F1uY).
- Web development: I would recommend either [PHP](https://www.php.net/docs.php), Python, Ruby, or [TypeScript](https://www.typescriptlang.org/). Three of them are popular programming languages for web development because of framework each language has. PHP has [Laravel](https://laravel.com/), Ruby has [Ruby on Rails](https://rubyonrails.org/), and Python has [Django](https://www.djangoproject.com/) and [FastAPI](https://fastapi.tiangolo.com/). TypeScript doesn't have a specific framework, but you can use [Next.js](https://nextjs.org/).
- Game development: now this is quite complicated. First, there are many ways to make a game, from a drag and drop, game engines, game libraries, or even using low-level multimedia libraries. If your goal is only to create a game (without caring about programming languages), then you can start with [Scratch](https://scratch.mit.edu/) or [GDevelop](https://gdevelop.io/), then you can continue with Unreal Engine's Blueprint. But if you want to make games with code, my suggestion is to start with [Godot Engine](https://godotengine.org/) or Unity. Godot Engine created Python-like language called [GDScript](https://gdscript.com/tutorials/) (similar syntax with Python), while Unity (which uses C#) has extensive tutorials for beginners. Or, if you want, you can use Python's Pygame or Ruby's Gosu library, albeit those are harder\*\* than using game engines.

\* There is a counter argument for this. For specific purposed learning, you have a clear goal when learning a programming language. E.g. when you have a purpose of building a game, you know you’re done when the game is finished.

\*\* For first time learners, I do not recommend you to directly use Pygame or Gosu, as there is no graphical user interface (GUI) when you put game objects into the scene.

# Conclusion

I hope that you can start your journey in programming language. The most important thing to note is that dare to make mistakes. Follow tutorials, then deviate, then see the effects when you change things. Only by deviating from tutorials that you will learn more about programming.
