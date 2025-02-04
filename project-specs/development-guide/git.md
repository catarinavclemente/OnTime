# GIT

## Git Forks and Upstreams: How-to and a cool tip

[https://www.atlassian.com/git/tutorials/git-forks-and-upstreams](https://www.atlassian.com/git/tutorials/git-forks-and-upstreams)

Git usage

**Security**

[**https://www.howtogeek.com/devops/how-to-set-up-https-personal-access-tokens-for-github-authentication/**](https://www.howtogeek.com/devops/how-to-set-up-https-personal-access-tokens-for-github-authentication/)

You can generate a key with this command:\
`$ ssh-keygen -b 4096 -t rsa -f /home/ec2-user/.ssh/id_rsa`

On the remote system, add the contents of your public key file (for example, `~/id_rsa.pub`) to a new line in your `~/.ssh/authorized_keys` file; on the command line, enter:\
`$ cat` /home/ec2-user/.ssh/id\_rsa.pub. `>> ~/.ssh/authorized_keys`

You may want to check the contents of \~/.ssh/authorized\_keys to make sure your public key was added properly; on the command line, enter:\
`$ more ~/.ssh/authorized_keys`[\
](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)\
Then, set-up SSH from your EC2 instance, following the guide on the last link of the sources list, bellow.

Sources:\
[Creating/Converting/Move SSH keys to the right place\
](https://webgate.ec.europa.eu/fpfis/wikis/pages/viewpage.action?pageId=297601060#id-6.C9,SSH\&PhpStorm-Configurationfileforscripts)[Generate an hardware security key to authenticate to GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)\
[Adding a new SSH key to your GitHub account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)\


**GIT aliases**\
[https://alblue.bandlem.com/2011/04/git-tip-of-week-aliases.html](https://alblue.bandlem.com/2011/04/git-tip-of-week-aliases.html)\
\
**Saving locally**\
\*\*\*\*`git stash` temporarily shelves (or _stashes_) changes you've made to your working copy so you can work on something else, and then come back and re-apply them later on. Stashing is handy if you need to quickly switch context and work on something else, but you're mid-way through a code change and aren't quite ready to commit.\
[https://www.atlassian.com/git/tutorials/saving-changes/git-stash](https://www.atlassian.com/git/tutorials/saving-changes/git-stash)

#### Collaborating (WIP)

On GitHub, create a fork from the EC repo.\
ec-europa: _https://github.com/ec-europa/\<project-name_>-reference

Add this repo as your origin branch.

In addition to origin, it’s often convenient to have a connection to your teammates’ repositories. For example, if your co-worker maintain the same repository as you, from his own fork, you can add a connection as follows:

```
git remote add other_user dev.example.com/other_user_repo.git
```

you git@github.com:you/common\_repo.git (fetch)\
you git@github.com:you/common\_repo.git (push)

co\_worker git@github.com:co\_worker/common\_repo.git (fetch\
coworker git@github.com:coworker/common\_repo.git (push)\_\
\_\_\
upstream git@github.com:\_organization/common\_repo.git (fetch)\
upstream git@github.com:organization/common\_repo.git (push)

It is good practice to keep the feature branch always up to date with [trunk](https://www.atlassian.com/continuous-delivery/continuous-integration/trunk-based-development).\
If your branch is recent, the first option is to use rebase.

```
git checkout feature/my-feature
git rebase -i master
```

Sources: [https://www.atlassian.com/continuous-delivery/continuous-integration/trunk-based-development](https://www.atlassian.com/continuous-delivery/continuous-integration/trunk-based-development)

### Deployment into ACC

#### There are two options:

* **Traditional approach**
* **Using the auto-merge functionality**

#### Traditional approach

If we don't want to use the auto-merge we can proceed as the usual way, so **from our fork 's \<branch\_name> against reference's master branch.**

![](../../.gitbook/assets/image2020-8-13_15-57-34.png)

**PS:** Once the pipeline have green lines, we need to **contact QA Team and request a code review** before lead ACC. If we pass this code review QA directly merge our PR into master and a new drone execution will be triggered to deploy in ACC.

#### Auto-merge approach

We can use this functionality naming our fork's branch **"deploy",** so in order to trigger a new drone we need to open a **new PR from our fork's deploy branch against reference's master branch** as following:

![](../../.gitbook/assets/image2020-8-13_15-37-0.png)

Keep in mind that you can go straight to ACC but a QA review will be needed before lead PROD (unless you are hosted in a dedicated server, then deploy into PROD is under your own risk).\
Source: [https://webgate.ec.europa.eu/fpfis/wikis/x/4YZMQ](https://webgate.ec.europa.eu/fpfis/wikis/x/4YZMQ)

This command below will, afterwards, remove all of the items from the Git index (not from the working directory or local repository), and then will update the Git index, while respecting Git ignores. _PS. Index = Cache_

<mark style="background-color:orange;">git rm -r --cached . && git add . && git commit -am "EJPREV-00: Remove ignored files."</mark>



### git reflog

[<mark style="background-color:orange;">https://www.atlassian.com/git/tutorials/rewriting-history/git-reflog</mark>](https://www.atlassian.com/git/tutorials/rewriting-history/git-reflog)

