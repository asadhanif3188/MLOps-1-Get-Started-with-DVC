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

*Note:* Now the **metadata about our data** is versioned alongside our source code, while the original data file was added to `.gitignore`.

## Storing and sharing

We can upload DVC-tracked data to a variety of storage systems (remote or local) referred to as "remotes". For simplicity, we will use a "local remote" for practice, which is just a directory in the local file system.

### Configuring a remote

Before pushing data to a remote we need to set it up using the `dvc remote add` command:

```
mkdir D:\workspace-mlops\dvcstore
dvc remote add -d myremote D:\workspace-mlops\dvcstore
```

*Note:* DVC supports many remote **storage types**, including *Amazon S3, NFS, SSH, Google Drive, Azure Blob Storage, and HDFS*.

An example for a common use case is configuring an *Amazon S3* remote:
```
dvc remote add -d storage s3://mybucket/dvcstore
```
For this to work, we'll need an AWS account and credentials set up to allow access.

### Uploading data
Now that a storage remote was configured, run dvc push to upload data:
```
dvc push
```

*Note:* `dvc push` copied the data cached locally to the remote storage we set up earlier.

Usually, we would also want to Git track any code changes that led to the data change (`git add, git commit and git push`).


### Retrieving data

Once DVC-tracked data and models are stored remotely, they can be downloaded with `dvc pull` when needed (e.g. in other copies of this project). Usually, we run it after `git pull` or `git clone`.

Let's try this now:

```
dvc pull
```

*Note:* After running `dvc push` above, the `dvc pull` command afterwards was short-circuited by DVC for efficiency. The project's `data/data.xml` file, our cache and the remote storage were all already in sync. We need to empty the cache and delete `data/data.xml` from our project if we want to have DVC actually moving data around. 

Let's do that now:
```
rm -rf .dvc\cache
rm -f data\data.xml
```

Now we can run `dvc pull` to retrieve the data from the remote:
```
dvc pull -v
```

## Making local changes

Next, let's say we obtained more data from some external source. We will simulate this by doubling the dataset contents:
```
cp data/data.xml data/dup-data.xml
cat data/dup-data.xml >> data/data.xml
rm -f data/dup-data.xml
```

After modifying the data, run `dvc add` again to track the latest version:
```
dvc add data/data.xml
```

Now we can run `dvc push` to upload the changes to the remote storage, followed by a `git commit` to track them:

```
dvc push
git add . && git commit -m "Dataset updated"
```


