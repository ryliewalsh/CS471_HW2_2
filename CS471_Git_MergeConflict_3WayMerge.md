# Example of a Git Merge Conflict and its Resolution with a 3-way-merge

## Brief Description
This assignment will familiarize you with git merge conflicts and their resolutions using a 3-way-merge.

The major steps in this assignment are:
* adding a new branch with one commit
* adding a commit to the `master` that modifies the same portion of of a file that was modified in the branch
* merging the branch into `master` resulting in a conflict
* resolving the merge conflict using the 3-way-merge

## Prerequisites
* Install the Perforce [P4Merge](https://www.perforce.com/product/components/perforce-visual-merge-and-diff-tools) 3-way-merge tool.
* Setup P4Merge to be the default merge tool. Google the specific instructions for your platform, but the setup should consist of a few simple steps such as:

```bash
$ git config --global merge.tool p4merge
$ git config --global mergetool.p4merge.path <path to p4merge executable>
```

### P4Merge on `onyx`
[P4Merge](https://www.perforce.com/product/components/perforce-visual-merge-and-diff-tools) is already installed on `onyx`. To use it, you need to configure your git merge tool as follows:

```bash
$ git config --global merge.tool p4merge
$ git config --global mergetool.p4merge.path /usr/local/bin/p4merge
```

## Step 0 (Initialization)
Initialize a new local git repository called `ExampleMergeConflict` using:
```bash
$ mkdir ExampleMergeConflict
$ cd ExampleMergeConflict
$ git init
```

## Step 1
Create a file called `Main.java` containing the code:
```java
public class Main
{
    //this is the chronological revision 1 in master

    public static void main(String[] args)
    {
        System.out.println("Hello world");
    }

}
```
Commit the changes as "revision one (in master)"

## Step 2
Create a branch called `branch-feature` and switch to it.

Edit the following code in the `Main.java` file in the branch:
```java
public class Main
{
    //this is the chronological revision 2 in branch-feature

    public static void main(String[] args)
    {
        System.out.println("Hello world");
    }

    public void methodInBranch()
    {
        System.out.println("Method in branch");
    }
}
```
Commit the changes as "revision two (in branch-feature)"

## Step 3
Switch to the `master` branch and edit the following code in `Main.java`:
```java
public class Main
{
    //this is the chronological revision 3 in master

    public static void main(String[] args)
    {
        System.out.println("Hello world");
    }

    public void methodInMaster()
    {
        System.out.println("Method in master");
    }
}
```
Commit the changes as "revision three (in master)"

## Step 4
While still in the `master` branch, merge the branch `branch-feature` in `master` using:
```bash
$ git merge branch-feature
```

The previous command should result in a merge conflict.

## Step 5
Start the 3-way-merge tool using:
```bash
$ git mergetool
```
If the merge tool is configured correctly, this command should display the merge conflict in P4Merge (as shown in the lecture slides).

## Step 6
Resolve the conflict by including in the final (resolved) version of `Main.java`:
* only the latest change to the comment before the `main` method (i.e., "`//this is the chronological revision 3 in master`")
* both the `methodInBranch()` from `branch-feature` (Step 2) and the `methodInMaster()` from `master` (Step 3) branches.

(Do not close P4Merge before taking a screenshot)

## Step 6.1
Save the changes in P4Merge and close it.

## Step 7
In the git bash console, issue a `git status` to find out the :question:**git command**:question: for finalizing the merge conflict.

## Step 7.1
The previous `git status` command might reveal some `*.orig` files generated during the merge conflict that can be removed manually:
```bash
$ rm *.orig
```
Alternatively, git can be configured to not create these `*.orig` files during future merge conflicts:
```bash
$ git config --global mergetool.keepBackup false
```

## Step 8
Finalize the merge of the `branch-feature` into `master`, by running the :question:**git command**:question: discovered in Step 7, which should result in the creation of a new (merge) commit (e.g., "`Merge branch 'branch-feature'`").
