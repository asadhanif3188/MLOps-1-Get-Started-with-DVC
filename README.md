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

**_Now we're ready to use DVC!_**

## Tracking data

Working inside an initialized project directory, let's pick a piece of data to work with. We'll use an example **data.xml** file, though any text or binary file (or directory) will do. Start by running:
```
dvc get https://github.com/iterative/dataset-registry get-started/data.xml -o data/data.xml
```

*Note:* We used `dvc get` above to turn any Git repo into a "data registry". `dvc get` can download any file or directory tracked in a DVC repository.

Use `dvc add` to start tracking the dataset file:
```
dvc add data/data.xml
```

*Note:* DVC stores information about the added file in a special `.dvc` file named `data/data.xml.dvc`. This small, human-readable metadata file acts as a placeholder for the original data for the purpose of Git tracking.

Next, run the following commands to track changes in Git:
```
git add data/data.xml.dvc data/.gitignore
git add .
git commit -m "Add raw data"
```



