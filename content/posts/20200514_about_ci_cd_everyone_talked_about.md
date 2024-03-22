+++
title = "About CI/CD Everyone Talked About (CSSE)"
date = 2020-05-14T00:00:00+07:00
description = ""
type = "post"
showTableOfContents = "true"
+++

*"Today, fast is no longer optional." - CircleCI Advertisement*

We live in a fast-paced world. Everyone wants to do things quickly. Businesses race to deliver value propositions. CLSA Securities released report in 2019 that online retails are in hyper-growth phase. A slight delay would change the competition drastically. That's why, engineers need a tool to deliver features as quick as possible. Enter CI/CD.

*DISCLAIMER: I don't have a dev-ops / system engineer experience. This article will be based on my 1-year experience as backend in DANA and 2-day Upscale program.*

# What Is CI/CD Anyway?

## Continuous Integration

Let's start with CI first. CI means Continuous Integration. But, what exactly is it?

![CI/CD graph by GitLab](https://media.licdn.com/dms/image/C5612AQHRLwzrgxLSog/article-inline_image-shrink_1500_2232/0/1589087093912?e=1716422400&v=beta&t=UfgGsENjc20ISUAOHuy-5S9m_2To9IwGqamqfxKYzBY)

This is a nice visualization from GitLab. Continuous Integration is the process where you develop a feature in a separate branch. Then, you want to **integrate that new feature into the existing system**, hence why you make a pull request (or merge request in GitLab).

But, it's not just about separate branch and pull request. There's something more. Everytime you want to integrate feature that you develop, **your code has to pass mandatory checks**. This is important so that you want to make sure that you do not introduce a new bug. Because when you introduce a new code, you introduce a potential bug. That's why there are mandatory checks.

What kinds of validation involved? As far as I know, usually there are these checks:
1) Build validation: this validation makes sure that your code can be built / compiled. If your code can't be compiled, then your feature doesn't work.
2) Test cases validation: all of the test cases will be ran, so when you have failed test cases, you can know as soon as possible. Failed test cases are dangerous for production environment, because then you have 50% chance of introducing a new bug (the other 50% is wrong / obsolete test cases).
3) Lint validation: this validation checks whether your code follows generally-followed coding style (also known as "lint"). For example, Ruby has Rubocop, Rust has Clippy, Golang has Golint, etc. But, what are coding styles? It's a guideline and/or convention defined either by language designers / by community, so that the code is standardized in an organization / open source projects. For example, [Airbnb has their own JavaScript style guide](https://github.com/airbnb/javascript), just like [Alibaba with Java](https://alibaba.github.io/Alibaba-Java-Coding-Guidelines/).
4) Code smell validation: I don't know if this is the same as lint / not. But, as far as I know, code smell validation detects bad coding, such as one-letter variables, too many parameters in a method, code duplication (breaks DRY principle), etc. [SonarQube](https://www.sonarqube.org/) is quite famous for this.

## Continuous Delivery

Okay, then, let's talk about CD, a.k.a. Continuous Delivery. Some people said Continuous Delivery. For me, both are similar, because in order to delivery our value, we have to deploy our code.

Continuous delivery is done after our integration passes validations and accepted by our supervisor. What happened in this part? There can be many things, such as:
1) Automated application release: this could happen if we set a CI/CD pipeline for mobile / desktop applications. As soon as the main branch (usually, master) changed, CI/CD pipeline will rebuild the application and generate the executable file, either "apk" / "exe" / "dmg" / etc. Then, it could be released on Github Release page if it's tagged, or it could be released on online platforms, such as Google Play Store / itch.io.
2) Automated deployment to server: if integration for backend is approved, then the code on main branch (usually, master) is updated. CI/CD will run again the validation processes, with additional step: update our code on the server, also known as deploy.
3) Automated push to library repository: ever heard of Nexus Repository? Nexus Repository is a place where we store built libraries / packages that can be reused by other projects. Continuous deployment means building the library & uploading it into library repository, such as Nexus.

# Why Do We Need It?

The first reason is already stated above. We want to go fast, because we live in an era of fierce business competition. As stated in The Three Ways (of dev-ops), the first way means we want to deliver value as fast as possible. But, fast paced development brings potential of bugs, that's why we want to "guard" the existing system with CI/CD. As Circle CI ad stated, "... but you also know that moving fast can break things. You could be just 1 bug away from everything coming to a standstill..."

The second reason is to automate things. Imagine if there's no CI/CD, the code reviewers will manually check the code and manually run the test cases, which is cumbersome. With CI/CD, code reviewers just need to check the main flow, without need to nit-picking the code (hopefully). And after the integration is done, the code can be released automatically.

# CI/CD Platforms

There are many platforms for CI/CD. You just need to pick one, based on your experience / needs.

## Jenkins

The first platform, is of course the legendary Jenkins :-) Jenkins is able to build and deploy code, as long as we create the pipeline. I don't have experiences with Jenkins, but I just know it because it's used to build Amethyst Game Engine. Many companies (such as DANA and Blibli) also use Jenkins. Jenkins is used in self-hosted environment, where you have full control of the build environment.

Recently, Jenkins released Blue Ocean, a plugin that modernized the pipeline design. Blue Ocean modernized the user interface, so that you can keep using Jenkins without having to deal with the old, confusin UI.

## Travis CI

Travis CI is taught at Upscale 3.0 program. Contrary to Jenkins, Travis CI is cloud-based, which means the pipeline you created will be executed on Travis CI server instead of your server. Travis CI is used by many developers and companies because of its simplicity. You just create a file named ".travis.yml", then fill in the configuration. The configuration consists of used language, language version, and [job lifecycle](https://docs.travis-ci.com/user/job-lifecycle/). You can look at examples of Travis CI script at [my repository](https://github.com/iamdejan/rust-convex-hull/blob/master/.travis.yml), as well as example repository by Mr. Faris (instructor of Upscale 3.0) [here](https://github.com/madebyais/tutorial-travis-ci/blob/master/.travis.yml). One major disadvantage of Travis CI is Github-only integration, at the moment. If you host your repository outside of Github, then as far as I know, you can't use Travis CI.

## Circle CI

Maybe you will know Circle CI from YouTube ad. Well, that's what happens with me. Circle CI is similar to Travis CI, that the service is cloud-based. They're similar also in the pipeline configuration using YML file. The different is Circle CI by default build using Docker, with options to use Virtual Machine. Circle CI is used in Godot CI tutorial [here](https://nakyle.com/godot-ci/).

## AppVeyor

Legend says that if you want to use CI/CD for Windows, use AppVeyor :-) AppVeyor is used by Godot Engine to automate builds for Windows platform.

## Drone CI

This is so far a CI/CD provider (other than Jenkins) that allow self-host service. DigitalOcean wrote a [tutorial](https://www.digitalocean.com/community/tutorials/how-to-set-up-continuous-integration-pipelines-with-drone-on-ubuntu-16-04) for installing Drone CI on our server.

## Bamboo

If you're a fan of Atlassian and its products (Jira, Bitbucket), then maybe you know about Bamboo. Its main strength is integration with Jira and Bitbucket. Since Atlassian products can be used in private server, Bamboo can also be used as a self-host service. However, that comes at a price, just like other Atlassian products.

## Et cetera...

There are so many, I can't mention all of them. Some of them are owned by cloud providers, like AWS CodeBuild and Azure Pipelines. Some are from startups.

# But, CI/CD Itself Is Not Enough

CI/CD offers us a guard for our system during integration and deployment phase. But, CI/CD merely is not enough, because dev-ops is not only about tools, but about the culture. So, what else do we need?

First, we need better development habit. TDD is a good start. Because you will make the test first, then you fix the code based on test result. But, TDD could be a disaster if badly designed / implemented. Also, the problem with TDD is that we're focused on unit. I prefer the BDD, because we're forced to think about scenarios, rather than unit.

The second thing we will need is on-duty culture, along with proper logging tools. For applications that are store-critical (focused heavily on application store stars), such as DANA / Go-Jek, on-duty becomes important, because users use their applications all the time, so it's the best option to minimize downtime. The developers need to be equipped with logging tools, such as Kibana. That way, we can monitor the system before users notice bug.

The third thing, we need to do experiments. Yes, we need a "dare to break things" culture. But, without a proper "laboratory", we can't do experiments. We have to setup sandbox environments for our experiments, otherwise we break things in production environment, and that's not good.

# Conclusion

Using CI/CD whenever available is a must, because it guards us from unwanted integrations / unwanted bugs. Yes it's true that we can't fix bad design, nor bad test cases, but at least there are some problems that can be eliminated with CI/CD.
