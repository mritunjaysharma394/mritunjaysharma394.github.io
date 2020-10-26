---
layout: post
title:  "How to automate code formatting for Python projects with GitHub Actions - A study "
summary: Tutorial to setup GitHub Actions for automating Python code formatting.
author: Mritunjay Sharma
date: '2020-09-10 23:32:23 +0530'
category: GitHub-Actions
thumbnail: /assets/img/posts/GithubActions.png
keywords: GitHub-Actions, python, workflows, yaml
permalink: /blog/github-tutorial-autoyapf
---

This is a beginner-friendly post that will tell you how easy it will become to conform to Python coding styles using [autoyapf](https://github.com/marketplace/actions/autoyapf) GitHub Action in your Python projects!

# Introduction
To begin with - let's discuss first what are GitHub Actions and what magic they bring :smiley:?

## GitHub Actions
GitHub Actions is that piece of magic which enables you to automate, customize, and execute your software development workflows right in your repository.

But hey? What are words without actions? And so what are **actions** without **workflows**? Yes, you got it right! For GitHub Actions to work you need to use *Workflows* but what are Workflows? 
A workflow is a configurable automated process made up of one or more jobs. Now, these jobs can be like the accounting of the trigger for the GitHub action to work - like push on the main branch.

Workflow files use YAML syntax and must have either a .yml or .yaml file extension. 

For GitHub Actions to work you must store workflow files in the `.github/workflows` directory of your repository.

### Brief Introduction to building a GitHub Action

> Note: In order to use an *existing GitHub action* all you require is *just a Workflow YAML file* in `.github/workflows`. The files mentioned in this part of the blog are required when you start building your own GitHub Actions :smile: 

In order to make almost any GitHub Action - you require at least these files:

* **Dockerfile** — GitHub actions are of three types - Docker actions, Javascript actions and composite run steps action. In our case- we are going to use Docker Action and so Dockerfile is used to define the container environment that runs the action.

* **action.yml** — This will define inputs, outputs, Entrypoint for our action, and other details like name of the action, description, author name, etc.

* **script** — An action might require a script to perform a task, in our case we just need an Entrypoint script named as `entrypoint.sh`

* **README.md** — This is optional, but if we want to publish our action on the Marketplace we need it. README.md file is useful to provide examples that are needed for the action to run without any hassles.

This is the basic skeleton of how the GitHub actions are built. If you want to start with making your own first action, check out this official Hello World [tutorial](https://docs.github.com/en/actions/creating-actions/creating-a-docker-container-action)

Let's move on to the main part of preparing and setting up the workflow for the **autoyapf** GitHub Action we are going to use.

## What is autoyapf?

[autoyapf](https://github.com/marketplace/actions/autoyapf) is a GitHub action for *yapf*, an open-source tool that automatically formats Python code to conform to the PEP 8 style guide. It is in essence, the algorithm that takes the code and reformats it to the best formatting that conforms to the style guide, even if the original code didn't violate the style guide.

This action is designed to be used in conjunction with the 'push' trigger. In simple words - everytime the maintainer pushes the code from their own playground or merges a Pull Request from a contributor it will be formatted automatically with the PEP 8 Style guidelines using the yapf tool. This action is a win-win situation for both contributors and maintainers of projects involving Python! 
 
## Choosing the Project

The first step is always choosing the repository where you will be using the GitHub action.
Since the GitHub action is made for python projects, I am going to use an open-source Python project that I created recently called [sedpy](https://github.com/mritunjaysharma394/sedpy)



## Preparing the Workflow

* We'll go to our repository and click on the **Actions** tab. That will then reflect an option like *set up a workflow yourself*. Click it.

<img src= "https://dev-to-uploads.s3.amazonaws.com/i/l19rc7u3na4xexkga6s5.png" width=1000>

> This will create a YAML file in the path: 
**.github/workflows/main.yml**

You can rename the file to whatever you prefer like, in this case, I will give it the name **autoyapf.yml** instead of **main.yml**. 

Now, let's dive in the **autoyapf.yml** file: 
```yaml
name: Formatting python code
on:
  push:
    paths:
    - '**.py'
```

What we have done until here is: 

* Given a name to the workflow: `Formatting python code`
* Defined what event will cause the workflow to trigger. In this case, it is triggered when the python file is pushed in the repository. 

Next step is to define the jobs, further, we added:
 ```yaml
jobs:
  autoyapf:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@master
        with:
          ref: ${{ github.head_ref }}
      - name: autoyapf
        id: autoyapf
        uses: mritunjaysharma394/autoyapf@v2
        with:
          args: --style pep8 --recursive --in-place .
      - name: Check for modified files
        id: git-check
        run: echo ::set-output name=modified::$(if git diff-index --quiet HEAD --; then echo "false"; else echo "true"; fi)
      - name: Push changes
        if: steps.git-check.outputs.modified == 'true'
        run: |
          git config --global user.name 'Mritunjay Sharma'
          git config --global user.email 'mritunjaysharma394@users.noreply.github.com'
          git remote set-url origin https://x-access-token:${{ secrets.GITHUB_TOKEN }}@github.com/${{ github.repository }}
          git commit -am "Automated autoyapf fixes"
          git push
```
Here's a brief detail about almost everything the above snippet does: 

* The jobs that the workflow has to perform will run on the GitHub hosted runner which is `ubuntu-latest` in our case.

```yaml
jobs:
  autoyapf:
    runs-on: ubuntu-latest
```
* We define the `steps` of jobs next.
  * The first step checks out the repository where we want to workflow to execute. Since I am using it inside [sedpy](https://github.com/mritunjaysharma394/sedpy), it will check out this repository's `master` branch

```yaml
steps:
      - uses: actions/checkout@master
        with:
          ref: ${{ github.head_ref }}
```
  * In the next step - we checked out the GitHub action 
[autoyapf](https://github.com/marketplace/actions/autoyapf) which will perform all the formatting using `yapf` tool with the arguments supplied as: `args: --style pep8 --recursive --in-place .`

```yaml
- name: autoyapf
        id: autoyapf
        uses: mritunjaysharma394/autoyapf@v2
        with:
          args: --style pep8 --recursive --in-place .
```
  * This step checks if the above step has made any changes in the code. If it has made any changes then the next step will push the changes to the `sedpy` repository. 

```yaml
- name: Check for modified files
        id: git-check
        run: echo ::set-output name=modified::$(if git diff-index --quiet HEAD --; then echo "false"; else echo "true"; fi)
```
  * This will check the condition as detailed above and we will push the changes using *run* utility that makes us use terminal-like commands in the `.yml` files.
  
```yaml
      - name: Push changes
        if: steps.git-check.outputs.modified == 'true'
        run: |
          git config --global user.name 'Mritunjay Sharma'
          git config --global user.email 'mritunjaysharma394@users.noreply.github.com'
          git remote set-url origin https://x-access-token:${{ secrets.GITHUB_TOKEN }}@github.com/${{ github.repository }}
          git commit -am "Automated autoyapf fixes"
          git push
```
Woohoo! :dancer: We have completed our workflow `autoyapf.yml` and the complete file looks something like this: 

```yaml
 
name: Formatting python code
on:
  push:
    paths:
    - '**.py'
jobs:
  autoyapf:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@master
        with:
          ref: ${{ github.head_ref }}
      - name: autoyapf
        id: autoyapf
        uses: mritunjaysharma394/autoyapf@v2
        with:
          args: --style pep8 --recursive --in-place .
      - name: Check for modified files
        id: git-check
        run: echo ::set-output name=modified::$(if git diff-index --quiet HEAD --; then echo "false"; else echo "true"; fi)
      - name: Push changes
        if: steps.git-check.outputs.modified == 'true'
        run: |
          git config --global user.name 'Mritunjay Sharma'
          git config --global user.email 'mritunjaysharma394@users.noreply.github.com'
          git remote set-url origin https://x-access-token:${{ secrets.GITHUB_TOKEN }}@github.com/${{ github.repository }}
          git commit -am "Automated autoyapf fixes"
          git push
```

See the below working example to see this GitHub action in action

{% gist https://gist.github.com/mritunjaysharma394/bc06d09130652ee9e438c49a8267e148 %}


If you have liked this action, please feel free to leave a star and yes it will be a blessing if you can contribute to it as well!

I hope this blog helped you how to start with creating workflows for GitHub actions (in our case: autoyapf) and you have learned something amazing! If you have doubts, suggestions or feedbacks, please feel free to ping me here:https://twitter.com/mritunjay394 !

Thank you so much for taking out time to read this blog! Sending you all the good vibes :sparkles:

