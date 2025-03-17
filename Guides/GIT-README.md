<h1 align="center">
  <img src="./assets/images/bb-1.png" alt="BBLogo" width="250" /></br></br>
  <strong style="font-size:60px;">Git Installation</strong>
</h1></br>

Open your terminal and install Git using Homebrew:
`brew install git`

Verify the installation was successful and confirm the version installed by typing:
`git --version`

Configure your Git username and email using the following commands, replacing my name with your own. These details will be associated with any commits that you create:

`git config --global user.name "Mikio Dehart"` 
`git config --global user.email "mikio.dehart@gmail.com"`

Configure the command line coloring for git (optional) by typing: 
`git config --global color.ui auto`

# Common Errors
In the event that the following error is received: 
```
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
    See git-pull(1) for details

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream develop origin/<branch>
```

You can specify what branch you want to pull:
`git pull origin master`

Or you could set it up so that your local master branch tracks github master branch as an upstream:
```
git branch --set-upstream-to=origin/master master
git pull
```
This branch tracking is set up for you automatically when you clone a repository (for the default branch only), but if you add a remote to an existing repository you have to set up the tracking yourself. Thankfully, the advice given by git makes that pretty easy to remember how to do.

# Common Commands
`git init <directory>` Create empty Git repo in specified directory. Run with no arguments to initialize the current directory as a git repository.

`git clone <url>` Retrieve an entire repository from a hosted location via URL
