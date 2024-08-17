# MLOps-1: Get Started with DVC

DVC is "Git for data"!

## Initializing a project
Imagine we want to build an ML project from scratch. Let's start by creating a Git repository:

```
git init
```

We will use our current working directory as a DVC project. Let's initialize it by running `dvc init` inside a Git project:

```
dvc init
```

A few internal files are created that should be added to Git:

```
git status
git add .
git commit -m "Initialize DVC"

```

