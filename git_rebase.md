<h1>Simple Rebase Workflow for GitHub</h1>

This is a very simple git workflow using rebase, appropriate for small teams working with repos on GitHub. If this workflow is followed carefully, and supported by responsible allocation of tasks and files among collaborators, merge conflicts will be rare. When they do occur, they will be minor, and easily resolved using the local master and feature branch, _before_ the branch is pushed to the remote repository and any pull request is made.

## Command Summary / TL;DR
```ruby
# Begin work session
$ git checkout master
$ git pull
$ git checkout -b [feature_branch]
# Do work

# Prepare for merge
$ git status
$ git add [files]
$ git commit -m "Commit message content"
$ git checkout master
$ git pull
$ git checkout [feature_branch]
$ git rebase master
# Resolve conflicts, if any
$ git push origin [feature_branch]

# Create pull request on GitHub
# Merge remote feature branch to remote master

# Update local master branch
$ git checkout master
$ git pull

# Delete remote feature branch from GitHub, then...
$ git branch -d [feature_branch]

# Lather. Rinse. Repeat.
```

## Full Version

### Every time you begin a work session:
```ruby
# Make sure you are working on the most current merged version:
$ git checkout master
$ git pull

# Create a feature-branch that you will use for this work session only:
$ git checkout -b [feature_branch]
```

Do your work on the feature branch, committing often, with concise, descriptive commit messages. Push often (without pull requests) to back up your work on a remote branch.

Limit your work on the feature branch to the files and code tightly associated with the feature you are working on. For example, if you are editing the layout file (views/layout.erb) on a branch named "edit-branch" in a Sinatra web app, _don't_ make modifications to the models or add a migration in that branch.


### When work on the feature branch is complete...
When you have finished the work you intend to do on that feature branch, and are ready to make a pull request, use the following procedure. **Don't** make a pull request or merge code that is incomplete or broken; the remote master (the master branch on GitHub) should always be working and passing all tests. If your feature isn't working, it's not ready to be merged.

```ruby
# In your feature branch, double-check the files you've touched
$ git status

# Add the files affected by your work.
# Don't use "git add ." unless you are very sure all files are necessary.
$ git add [path/first_file.ext]
$ git add [path/second_file.ext]
# and so on...

# Commit the branch
$ git commit -m "Fix bugs in widget.rb"

# Make sure your branch is built
# on the most current version of the remote master
$ git checkout master
$ git pull
$ git checkout [feature_branch]
$ git rebase origin master
```

If there are conflicts between your code and the most current version of the remote master, they will show up at this point. Follow the directions provided by git to resolve them locally, _before_ pushing up the final version of the feature branch for the pull request.

### Pushing and Merging the Branch
Once any conflicts between the updated local master and the feature branch, it is safe to push the feature branch to GitHub and make the pull request to merge the branch to the remote master. Working from the feature branch:

```ruby
$ git push origin [feature_branch]
```

If you've managed things properly to this point, the final version of the branch should have no conflicts witht the master branch on GitHub, and should show "Able to merge". Create the pull request to merge the feature branch.

Once the team has has merged the feature branch to the remote master, your local master is obsolete: it does not include the changes merged to the remote master from the feature branch. The final step in the process is to do another pull from the remote master to your local master, so that your local repo is totally up-to-date and ready for additional work. Navigate to your local master branch, and pull again:

```ruby
$ git checkout master
$ git pull
```

### Branch management
Once your team has merged the feature branch, it should not be used again. Continuing to work in a feature branch that has been merged is inviting conflicts. Instead, delete the merged branch from GitHub through the UI, then delete it from your local repo with the following command, entered from the local master branch:

```ruby
$ git branch -d [feature_branch]
```

At this point, you are ready to begin your next work session; return to the top of this work flow to begin with a new branch. Even if you are continuing work on the same files and/or feature, start a new feature branch, to avoid introducing merge conflicts down the line.
