# Contributing Guide

We welcome contributions from anyone, even if you are new to open source. We will help you with any technical issues and help improve your contribution so that it can be merged.

Every small change of refactoring, documentation, code commenting, questions, suggestions, adding a test, adding a primitive make the project better and will help making Dyalog APL bug free. 𓆣

## Basic Setup

To contribute, make sure your set up:

- Your username + email
- Your ~/.gitconfig
- Dyalog (preferably version >18.2 unicode)

### Fork ullu

Step 1. Create a fork of the project repository

Step 2. Clone the project repository from GitHub and set up your remote repository

```
git clone https://github.com/dyalog/ullu.git
cd ullu
git remote add REMOTE_NAME https://github.com/YOUR_GITHUB_USERNAME/ullu.git
```

`REMOTE_NAME` is the name of your remote repository and could be any name you like, for example your first name.

`YOUR_GITHUB_USERNAME` is your user name on GitHub and should be part of your account path.

You can use `git remote -v` to check if the new remote is set up correctly.

## Sending a Pull Request/Merge Request

Step 1. Create a new branch

```
git checkout -b fix1
```

Step 2. Make changes in relevant file(s)
Step 3. Commit the changes:

```
git add FILE1 (FILE2 ...)
git commit -m "YOUR_COMMIT_MESSAGE"
```

[Here](https://cbea.ms/git-commit/) are some great tips on writing good commit messages.

Step 4. Check to ensure that your changes look good
```
git log --pretty=oneline --graph --decorate --all
```

Step 5. Send the pull request
```
git push REMOTE_NAME fix1
```

The command will push the new branch fix1 into your remote repository REMOTE_NAME that you created earlier. Additionally, it will also display a link that you can click on to open the new pull request. After clicking on the link, write a title and a concise description then click the “Create” button.

Yay you are now all set. ٩(ˊᗜˋ*)و

## Contributing Code

### Adding a primitive

### Adding a test

## Contributing Docs

---

Note: By submitting a PR you agree to license your contribution under the ullu’s MIT [license](https://github.com/Dyalog/ullu/blob/main/LICENSE) unless explicitly noted otherwise.