# Reminders on Git commands

### What's Git ?
Git is a widely used version control/management tool. It allows you to store your projects, synchronize them between your different machines (personal computer, work computer...), and share them with your colleagues.

### How to clone a Git ?

Go on the Git webpage of interest, select the green "<> Code" button, and copy the URL (it can be the HTTPS or SSH one)
Then, through the terminal, go into your folder of interest on your laptop and use the following command :

````
git clone <url copied>
````

### How to update your local repository ? 

If someone updated the Git and you have some modifications, you can download all the modifications done on a Git, without applying them to the local branch :
````
git remote update
````
You can also use :
````
git fetch
````
The main difference between those 2 commands is that `git fetch` will download all the modifications of only one remote (by default, it's : `origin`), when `git remote update` will download all the modifications of all the remotes defined in the project. If you want to get the modifications of a specific branch only, you can also use :
````
git fetch <branch_name>
````


After, you will be able to see all the differences between your local repository and the Git. The command to check the status of the repository is :
````
git status
````
If you want to apply the modifications to your local repository, you can then use :
````
git pull
````

### How to update the Git ?

After you worked really hard on your code, you may want to push your changes (made on your local repository), to the git.
For this, you need to first, add them to the index (staging zone, meaning that it's "pre-commited"), using :
````
git add
````
You can use this command on different ways, to add only one specific file, or all the modifications (-a):
````
git add <python file name>
git add -A
````

If there is a file in the index area, but finally, you don't want its modification to be pushed on the git (like "undo git add"), you can use this command to remove the modification of the index (so it will be still on the git, but not the modifications made since the last push):
````
git restore --staged <file_name>
````

If you added a file, that you don't want on the Git, you can remove it completely from the git but without locally deleting it (it will obtain the status `untracked`):
````
git rm --cached <file_name>
````

Now, to create a history of tracked changes, you have to validate (*commit*) these changes and add a message describing them by executing the command
````
git commit -m "<the message, describing the modifications>"
````
Now that validation has been completed, your local version of the project is ahead of the version on the Git server. You can therefore push these changes to synchronize the two versions, with the command :
````
git push
````


### How to ignore files ?

There are always files that we don't want on the git (like plot outputs, test .py files ..etc).
The best way to not have to delete each unwanted file each time that you update your git, is to create a gitignore file.

Ignored files are managed via a `.gitignore` file located at the root of our project. In this file, you can add all the names of files and folders that you want to ignore permanently. 

The presence of a `.gitignore` file is essential in a Git project. Even with a minimalist IDE, some unnecessary configuration files may be generated and must be ignored. It is therefore essential to create a `.gitignore` for each new project.

Why does `.gitignore` start with a `.`?
Files whose names start with `.` are hidden files (they do not appear when you use a file manager). These are often configuration files.

For example, to avoid any `.DS_Store` file to be added to the git, you can write inside the `.gitignore`
````
.DS_Store
````
For the virtual environments :
````
.env
.venv
env/
venv/
````
If you don't want an entire folder, or even a specific sub-folder, it's also possible. For example :
````
input/
output/tmp
````
If you don't want any `.png` file :
````
*.png
````

You can also add other `.gitignore` in your folder and sub-folder, to add specific restricting rules to apply to this folder only.


### How to work with branches ?

A Git branch is an independent line of development in a repository. It allows you to work on new features or fixes in isolation, without affecting the stable version of the project on the main branch. Work with them provide several advantages :
1. **Isolation of changes**: Branches allow you to experiment and develop features without the risk of introducing bugs into the main code.
2. **Easier collaboration**: Multiple developers can work on different branches in parallel without the risk of conflicts.
3. **Version management**: Branches allow you to maintain different versions of a project, for example, a stable version for production and another for development.
4. **Code review and testing**: Branches facilitate code review and testing, as changes can be merged into the main branch after validation.
5. **Clear history**: They provide a clear development history, allowing you to track the evolution of each feature or fix.


The following conventions exist for naming branches:
1. The name of a branch always follows this format: `<prefix>/<name>`.
2. Possible `<prefix>` values are:
- `feature/`: for new features.
- `test/`: for tests.
- `bugfix/`: for bug fixes.
- `experiment/`: for trials.
- `improvement/`: for improvements (code quality).
3. `<name>` always follows the same naming convention (snake_case, kebab-case, camelCase etc).

You can create a new branch with :
```
git branch <prefix>/<name>
````
Then, switch the branch you're working on with
````
git checkout <prefix>/<name>
````
Or, to directly create and switch to the branch in a single command :
````
git checkout -b <prefix>/<name>
````

To list all local branches (and remote branches with the `-a` option):
```
git branch -a
```
You can see the branches that are currently being worked on and those that have already been merged with the `main` branch with the `--no-merged` and `--merged` options, respectively.
```
git branch -a --no-merged main
git branch -a --merged main
```

Then, once the work on the branch is complete, to merge the branch into the main, you must return to `main` and merge the desired branch into main:
````
git checkout main
git merge <prefix>/<name_of_my_branch>
````

You must delete branches that have already been merged into the main branch.
To delete a branch that has already been merged:
```
git branch -d <prefix>/<name>
```
To delete a branch that you are currently working on (so not merged yet) but are going to abandon:
```
git branch -D <prefix>/<name>
```
To avoid potential conflicts, others must therefore synchronize their own local repository with the remote main to retrieve these changes :
````
git checkout main
git pull origin main
````

### If you want to cancel a commit, or go back to a previous version

If you simply want to undo the last commit but keep the changes in your files (to fix them, for example):
````
git reset HEAD~1
````
If you want to delete the last commit but also erase the changes to local files :
````
git reset --hard HEAD~1
````
Here, 1 indicates the commit number you're touching (so here, ~1 corresponds to the last one)

If there is an old commit that you want to get back, you can first print the different logs (so commit done on the git, by you or the other colleagues)
````
git log --oneline
````
You can then create a branch to start again from this version (in this example, we will consider the hash `e4f5g6h`):
````
git checkout -b <fix>/<old_code> <e4f5g6h>
````
If you want to revert to the same code state as in an old commit (still at hash `e4f5g6h` for example), but without deleting the subsequent commits from the history:
````
git revert <e4f5g6h>..HEAD
````
This creates as many new “reverse” commits as there are commits after `e4f5g6h`, so that the final code is identical to `e4f5g6h`, while preserving the complete history (no rewriting on the history, to preserve everything that has been done, just in case).


### How to solve git conflict ?

If you have a conflict between 2 versions, if the conflict is not related to the same exact lines, you can use a command to merge the modifications :
````
git config --global pull.rebase false
````
Then add a commit with `git commit -m <your_message>`, and `git push`

If you have conflict, that can not be solved with the previous command, indicated by the following message :
````
CONFLICT (content): Merge conflict in .gitignore
Automatic merge failed; fix conflicts and then commit the result.
````
Open the code in an editor (like VSCode), it will show the modifications made and pushed on the Git `<number> (Current Change)`, and your local modifications `HEAD (Current Change)`
You can select `Resolve in Merge Editor`. You will have two windows displayed : one with the "incoming" change, and the other with the "current" change, and the "result" under (that shows the outcome, depending on the choices made to fix the conflict).
You can either accept one modification (incoming or current), accept a combination (with one before the other), or ignore a modification
Once you're satisfied, you can validate with `Complete Merge`.
Then, like previously, use `git commit -m <message>` and `git push`.


### How to update a library (GitHub and also PyPi) ?

1) If the version in GitHub is not the one that is available locally (updated by another user) -> needs to be actualized
    -> (to set the "pull" command to "merge" the local version [if we made modifications or not], and the online one, globally (= by default))
      ```
      git config pull.rebase false --global
      ```
   Then 
   ```
   git pull 
   ```

2) To commit all & add a message 
   ```
   git commit -a -m "write the comment here"
   ```

3) Add a tag (= version, readable by PyPi)
   ```
   git tag v0.0.4 
   ```
   (here nb of the version wanted)

4) Push both (code and tags) on Git
   ```
   git push
   git push --tags
   ```

5) Remove the old version on PyPi first (won't allow to add the file on another one) (dist folder)
   ```
   rm -rf dist 
   ```

6) Construct the "environment" for PyPi
   ```
   hatch build
   ```

7) Publish on PyPi 
   ```
   hatch publish
   ```


/!\ Need to "log in" via API token (easiest way) -> settings of the account and "Create API token"
/!\ The token code will appear only ONCE, so save it somewhere /!\
It will be asked when using "hatch publish" :
    username = "__token__" (2 underscores to fill)
    password = token code given 