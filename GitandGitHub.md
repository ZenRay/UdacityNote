[toc]

## git 指令
git log

\-\-oneline: 单行显示内容

\-p:显示更改的文件

\-w:不显示空白行和空白变更

\-stat:显示变更的行

## commit 习惯
### Good Commit Messages
Let's take a quick stroll down Stickler Lane and ask the question:

How do I write a good commit message? And why should I care?

These are fantastic questions! I can't stress enough how important it is to spend some time writing a good commit message.

Now, what makes a "good" commit message? That's a great question and has been written about a number of times. Here are some important things to think about when crafting a good commit message:

**Do**

do keep the message short (less than 60-ish characters)
do explain what the commit does (not how or why!)

**Do not**

do not explain why the changes are made (more on this below)
do not explain how the changes are made (that's what git log -p is for!)
do not use the word "and"
if you have to use "and", your commit message is probably doing too many changes - break the changes into separate commits
e.g. "make the background color pink and increase the size of the sidebar"
The best way that I've found to come up with a commit message is to finish this phrase, "This commit will...". However, you finish that phrase, use that as your commit message.

Above all, be consistent in how you write your commit messages!

## commit style —— Udacity Git Commit Message Style Guide
1. Introduction

    This style guide acts as the official guide to follow in your projects. Udacity evaluators will use this guide to grade your projects. There are many opinions on the "ideal" style in the world of development. Therefore, in order to reduce the confusion on what style students should follow during the course of their projects, we urge all students to refer to this style guide for their projects.
    
2. Commit Messages

    Message Structure
    A commit messages consists of three distinct parts separated by a blank line: the title, an optional body and an optional footer. The layout looks like this:
    
    ```
    type: subject
    
    body
    
    footer
    ```

    The title consists of the type of the message and subject.

3. The Type
    
    The type is contained within the title and can be one of these types:
    
    * feat: a new feature
    * fix: a bug fix
    * docs: changes to documentation
    * style: formatting, missing semi colons, etc; no code change
    * refactor: refactoring production code
    * test: adding tests, refactoring test; no production code change
    * chore: updating build tasks, package manager configs, etc; no production code change

4. The Subject

    Subjects should be no greater than 50 characters, should begin with a capital letter and do not end with a period.

    Use an imperative tone to describe what a commit does, rather than what it did. For example, use change; not changed or changes.

5. The Body
    Not all commits are complex enough to warrant a body, therefore it is optional and only used when a commit requires a bit of explanation and context. Use the body to explain the what and why of a commit, not the how.

    When writing a body, the blank line between the title and the body is required and you should limit the length of each line to no more than 72 characters.

6. The Footer
The footer is optional and is used to reference issue tracker IDs.

### Example Commit Message

```
feat: Summarize changes in around 50 characters or less

More detailed explanatory text, if necessary. Wrap it to about 72
characters or so. In some contexts, the first line is treated as the
subject of the commit and the rest of the text as the body. The
blank line separating the summary from the body is critical (unless
you omit the body entirely); various tools like `log`, `shortlog`
and `rebase` can get confused if you run the two together.

Explain the problem that this commit is solving. Focus on why you
are making this change as opposed to how (the code explains that).
Are there side effects or other unintuitive consequenses of this
change? Here's the place to explain them.

Further paragraphs come after blank lines.

 - Bullet points are okay, too

 - Typically a hyphen or asterisk is used for the bullet, preceded
   by a single space, with blank lines in between, but conventions
   vary here

If you use an issue tracker, put references to them at the bottom,
like this:

Resolves: #123
See also: #456, #789
```
### 提交样例

```
功能：用 50 个或更少字符总结改动

如有必要，提供更详细的阐释文本。将其限制在 72 个字符左右。有些情况下，第一行被视为提交的主题，其余文本被视为正文。将摘要与正文分隔开的空行至关重要（除非你完全省略正文）；如果不分隔摘要和正文，“log”、“shortlog”和“rebase”等工具可能不知所措。

解释此次提交要解决的问题。重点是为什么要进行此次改动而不是如何改动（代码会说明这一点）。此次改动是否有任何副作用或其他不太直观的后果？可在此处解释这些方面。

空行后面可以有更多段落。

 - 还可以使用项目符号分项列出

 - 通常使用连字符或星号作为项目符号，前面有一个空格，中间是一些空行，但惯例不尽相同

如果你使用问题跟踪器，请在底部提供对问题的引用，
例如：

解决：# 123
另请参见：# 456、# 789
```


## 忽略追踪文件
利用 .gitignore 文件，将需要忽略的文件加入到该文件中即可。同样可以针对满足某类型的文件进行忽略追踪，相关说明如下：

* blank lines can be used for spacing
* \# - marks line as a comment
* \* - matches 0 or more characters
* ? - matches 1 character
* [abc] - matches a, b, or c
* \*\* - matches nested directories - a/\*\*/z matches
    * a/z
    * a/b/z
    * a/b/c/z

假设需要对 sample 文件夹中的 jpg 文件进行忽略的话，将相应说明防水加入 .gitignore 中，使用方法如下：

```
sample/*.jpg
```

## git tag 使用
git tag 的重要作用是对那些不想显性显示提交的说明，可以加 tag 方法作为某一类的说明。相关用法如下：

```
$ git tag -a v1.0   # 添加 tag 名称为 v1.0，⚠️也需要添加说明

$ git tag               # 验证和显示添加的 tag

$ git log --decorate    # 此命令不仅可以显示已经提交的 commit 的 log 而且可以显示已经提交的 tag

```


**注意：加 tag 需要使用标记 a**，另外如果需要删除 tag 的话，可以使用 `-d` 或者 `--delete` 参数来进行删除，命令为`git tag -d v1.0`，可以用于删除添加的 v1.0 的 tag。如果需要对已经提交的 commit 上进行添加 tag 的话，也可以使用 `git tag -a tag_name commit_sha`命令进行添加（其中 tag_name 为指定的 tag 名称，commit_sha 为已经提交了 commit 对应的 sha 值）

## 添加远程仓库
### 添加个人远程仓库—— GitHub
如果只是需要将本地文件添加到个人账户的远程仓库，只需要在 GitHub 上创建一个远程的 Repo ，然后可以通过 `git clone` 的命令来把远程仓库克隆到本地即可继续项目；同样可以通过其他方式，假设已经开始项目而是需要把已有的项目文件推送到远程，那么可以通过以下方式：

```
# 同样需要先在 GitHub 上创建远程仓库
# 在本地的命令行窗口输入以下命令
git remote add origin remote_repo_url       # reomte_repo_url 是远程仓库的 git url;该命令是将远程仓库添加到本地可远程操作
git push -u origin master       # 将本地文件推送到远程，后续推送可以不用 -u 的参数，直接使用 git push origin master
```

以上方式创建的个人仓库，不能用做协作处理共有项目。假设团队协作创建的远程 repo，需要将本地文件上传到远程的 repo 中，可以使用以下流程及命令：

1. first, this command has the sub command `add`
2. the word `origin` is used —— this is setting the shortname that we discussed earlier

    * Remember that the word origin here isn't special in any way.
    * If you want to change this to `repo-on-GitHub`, then (before running the command) just change the word "origin" to "repo-on-GitHub":

    ```
    $ git remote add repo-on-GitHub remote_repo_url
    ```
3. third, the full path to the repository is added (i.e. the URL to the remote repository on the web)

**注意⚠️：如果要验证是否已经添加了远程的地址到本地的中，可以使用`git remote -v` 来确认**


## 团队协作
1. Filtering Collaborator's Commits

	Being able to narrow down the commits to just the ones you're looking for can be a chore. Let's look at the different ways we can discover information that our collaborators have done!

2. Group By Commit Author

	This is not a massive project, but it does have well over 1,000 commits. A quick way that we can see how many commits each contributor has added to the repository is to use the `git shortlog` command —— displays an alphabetical list of names and the commit messages that go along with them. If we just want to see just the number of commits that each developer has made, we can add a couple of flags: `-s` to show just the number of commits (rather than each commit's message) and `-n` to sort them numerically (rather than alphabetically by author name).

3. Filter By Author

	Another way that we can display all of the commits by an author is to use the regular `git log` command but include the --author flag to filter the commits to the provided author.
	
	```
	$ git log --author=Surma        # 删选出 Surma 的提交
	$ git log --author="Paul Lewis"     # 如果有空格，那么需要在名称上添加引号
	```

4. Filter Commits By Search

	Before going through this section on filtering by searching, I feel like I need to stress how important it is to write good, descriptive commit messages. If you write a descriptive commit message, then it's so much easier to search through the commit messages, later, to find exactly what you're looking for.

	And remember, if the commit message is not enough for you to explain what the commit is for, you can provide a detailed description of exactly why the commit is needed in the description area.
	
	Let see an example of extra details in a commit in the lighthouse project by looking at commit `5966b66`:
	
	```
	$ git show 5966b66
	```
	
	So why do we care about all of this detail? For one thing, it's easier for you to go back and review the changes made to the repository, and it easier for others to review the changes to. Another thing is filtering commits by information in the current message or description area.
	
	We can filter commits with the `--grep` flag.
	
	How about we filter down to just the commits that reference the word "bug". We can do that with either of the following commands:
	
	```
	$ git log --grep=bug
	$ git log --grep bug
	```

### 问题检视——GitHub Issues
"issues" doesn't mean that there's actually a bug, it can just be any change that needs to be made to the project. GitHub's issue tractor is quite sophisticated. Each issue can:

* have a label or multiple labels applied to it
* can be assigned to an individual
* can be assigned a milestone (for example the issue will be resolved by the next major release)

But probably one of the most important aspects of the issue tracker is that each issue can have its own comments, so a conversation can form around the issue.

## Best Practices
### Write Descriptive Commit Messages
While we're talking about naming branches clearly that describe what changes the branch contains, I need to throw in another reminder about how critical it is to write clear, descriptive, commit messages. The more descriptive your branch name and commit messages are the more likely it is that the project's maintainer will not have to ask you questions about the purpose of your code or have dig into the code themselves. The less work the maintainer has to do, the faster they'll include your changes into the project.

### Create Small, Focused Commits
This has been stressed numerous times before but make sure when you are committing changes to the project that you make smaller commits. Don't make massive commits that record 10+ file changes and changes to hundreds of lines of code. You want to make smaller, more frequent commits that record just a handful of file changes with a smaller number of line changes.

Think about it this way: if the developer does not like a portion of the changes you're adding to a massive commit, there's no way for them to say, "I like commit A, but just not the part where you change the sidebar's background color." A commit can't be broken down into smaller chunks, so make sure your commits are in small enough chunks and that each commit is focused on altering just one thing. This way the maintainer can say I like commits A, B, C, D, and F but not commit E.

### Update The README
And lastly if any of the code changes that you're adding drastically changes the project you should update the README file to instruct others about this change.

### Recap
Before you start doing any work, make sure to look for the project's CONTRIBUTING.md file.

Next, it's a good idea to look at the GitHub issues for the project

* look at the existing issues to see if one is similar to the change you want to contribute
* if necessary create a new issue
* communicate the changes you'd like to make to the project maintainer in the issue

When you start developing, commit all of your work on a topic branch:

* do not work on the master branch
* make sure to give the topic branch clear, descriptive name

As a general best practice for writing commits:

* make frequent, smaller commits
* use clear and descriptive commit messages
* update the README file, if necessary


## 多人合作——多人 repo 同步

Remember that the word `origin` is just the default name that's used when you `git clone` a remote repository for the first time. We're going to use the `git remote` command to add a new shortname and URL to this list. This will give us a connection to the source repository.

Notice that I've used the name `upstream` as the shortname to reference the source repository. As with the `origin` shortname, the word `upstream` here is not special in any way; It's just a regular word. This could have been any word... like the word "banana". But the word "upstream" is typically used to refer to the source repository.

```
$ git remote add upstream folked_repo_name  # folked_repo_name 是指定的 folk 的源 repo url 地址——例如 https://github.com/udacity/course-collaboration-travel-plans.git
```

### Origin vs Upstream Clarification
One thing that can be a tiny bit confusing right now is the difference between the `origin` and `upstream`. What might be confusing is that `origin` does not refer to the source repository (also known as the "original" repository) that we forked from. Instead, it's pointing to our forked repository. So even though it has the word `origin` is not actually the original repository.

Remember that the names `origin` and `upstream` are just the default or de facto names that are used. If it's clearer for you to name your `origin` remote `mine` and the `upstream` remote `source-repo`, then by all means, go ahead and rename them. What you name your remote repositories in your local repository does not affect the source repository at all.

针对个人本地 repo 和 folk 的远程 repo，可能出现不同步，需要注意以上操作。

对应个人 repo 和 folk 的 repo 名称，可以使用 `git remote rename` 来进行更改，方式如下：

```
$ git remote rename origin mine     #将个人的 origin 名称改为 mine
$ git remote rename upstream source-repo        #将 folk upstream 名称为 source-repo
```

### Retrieving Upstream Changes
Now to get the changes from upstream remote repository, all we have to do is run a `git fetch` and use the `upstream` shortname rather than the `origin` shortname:

```
$ git fetch upstream master # 需要注意命令行的 fetch 只是对本地 repo 进行了处理，并没有 merge 和 commit 到个人的远程 repo 中

# 如果需要对本地文件进行 merge 和 push，可以通过以下方式
# to make sure I'm on the correct branch for merging
$ git checkout master

# merge in Lam's changes
$ git merge upstream/master

# send Lam's changes to *my* remote
$ git push origin master
```
如果仅是对 folk 的repo 需要一步完成 fetch 和 merge 操作，那么可以通过以下命令 `git pull upstream master`

## 多个 commit 处理 —— rebase 使用
rebase 可以针对多个 commit 进行合并，并且产生一个新的 commit。`$ git rebase -i HEAD~3`，针对改命令详细说明如下：

The git rebase command will move commits to have a new base. In the command `git rebase -i HEAD~3`, we're telling Git to use `HEAD~3` as the base where all of the other commits (`HEAD~2`, `HEAD~1`, and `HEAD`) will connect to.

The `-i` in the command stands for "interactive". You can perform a rebase in a non-interactive mode. While you're learning how to rebase, though, I definitely recommend that you do interactive rebasing.

其中 `HEAD～3` 确定了需要合并到哪个 commit 的位置，但是用于指定位置的对象名称不一定仅仅是使用 SHA，也可以使用 Branch name 和 tag name。

### Force Pushing
I had to force push the branch. I had to do this because GitHub was trying to prevent me from accidentally deleting commits. Because I used the `git rebase` command, I effectively erased the three separate commits that recorded my addition of Florida, Paris, and Scotland. I used `git rebase` to combine or squash all of these commits into one, single commit.

Using `git rebase` creates a new commit with a new SHA. When I tried using `git push` to send this commit up to GitHub, GitHub knew that accepting the push would erase the three separate commits, so it rejected it. So I had to force push the commits through using `git push -f`

------
需要注意：对远程仓库的版本历史修改，都是在本地修改的基础上进行的：本地修改完成后，再 push 到远程仓库。但是除了 git revert 可以直接 push，其他都会对原有的版本历史修改，只能使用强制 push

------

### Rebase Commands
Let's take another look at the different commands that you can do with git rebase:

* use `p` or `pick` – to keep the commit as is
* use `r` or `reword` – to keep the commit's content but alter the commit message
* use `e` or `edit` – to keep the commit's content but stop before committing so that you can:
    * add new content or files
    * remove content or files
    * alter the content that was going to be committed
* use `s` or `squash` – to combine this commit's changes into the previous commit (the commit above it in the list)
* use `f` or `fixup` – to combine this commit's change into the previous one but drop the commit message
* use `x` or `exec` – to run a shell command
* use `d` or `drop` – to delete the commit

### When to rebase
As you've seen, the git rebase command is incredibly powerful. It can help you edit commit messages, reorder commits, combine commits, etc. So it truly is a powerhouse of a tool. Now the question becomes "When should you rebase?".

Whenever you rebase commits, Git will create a new SHA for each commit! This has drastic implications. To Git, the SHA is the identifier for a commit, so a different identifier means it's a different commit, regardless if the content has changed at all.

So you should not rebase if you have already pushed the commits you want to rebase. If you're collaborating with other developers, then they might already be working with the commits you've pushed. If you then use git rebase to change things around and then force push the commits, then the other developers will now be out of sync with the remote repository. They will have to do some complicated surgery to their Git repository to get their repo back in a working state...and it might not even be possible for them to do that; they might just have to scrap all of their work and start over with your newly-rebased, force-pushed commits.

## Git commit 规范化工具
使用 [Commitizen](https://github.com/commitizen/cz-cli) —— 一个规范化的 commit 工具，特点是通过选择和步骤分解来提交 commit。安装流程如下：

```
# 首先全局安装工具
$ npm install -g commitizen
$ npm install -g cz-conventional-changelog

# 创建配置文件，并添加路径
$ echo '{ "path": "cz-conventional-changelog" }' > ~/.czrc

# 安装完成需要在项目目录里，初始化工具
$ commitizen init cz-conventional-changelog --save --save-exact
```

以后，凡是用到 `git commit` 命令，一律改为使用 `git cz`。

**注意**⚠️：推荐将相应的非项目文件和文件夹添加到 `.gitignore` 中。


## Git 配置代理
配置代理，使用 `git config` 进行配置，使用 `socks5` 方式，具体命令如下：

```
git config --global http.proxy socks5://127.0.0.1:1080
git config --global https.proxy socks5://127.0.0.1:1080

```

如果需要暂停配置代理可以使用以下方式：

```
git config --global http.proxy ""
git config --global https.proxy ""

```

## reference 
1. [Git Commit Message Style Guide](https://udacity.github.io/git-styleguide/)
2. [Git Branching - Rebasing](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)
3. [git-rebase](https://git-scm.com/docs/git-rebase)
4. [Become a GitHub Pro | Udacity](https://blog.udacity.com/2015/06/become-github-pro.html)

    >描述了使用 GitHub及其优势，通过哪些方式去查找合适的 opensource 、repo等。总之需要多尝试
5. [How to Contribute to Open Source | Open Source Guides](https://opensource.guide/how-to-contribute/)

    >了解怎么去 contirbute，另外可以通过文中的 checklist 来查找合适的项目去尝试
6. [commitizen/cz-cli: The commitizen command line utility.](https://github.com/commitizen/cz-cli)

    > git commit 规范化工具 repo
