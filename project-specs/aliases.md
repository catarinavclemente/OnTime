# Creating aliases

.bashrc is a Bash shell script that Bash runs whenever it is started interactively. It initializes an interactive shell session. You can put any command in that file that you could type at the command prompt.

If you put commands here, you set up the shell for use in your particular environment, or to customize things to your preferences. A common thing to put in .bashrc are aliases that you want to always be available. In the directory below, you'll find the devops alias file (\~/.bashrc.d/00-devops.aliases.sh) and you can create/edit your own bash aliases file(s). \~/.bashrc.d/00-\[your-user].aliases.sh.

After adding/editing your alias file, from the project root, **activate alias** by typing the following source command:

source \[your bash aliases file name]\
source \~/.bashrc.d/00-vascoca-aliases.sh

**List aliases:**\
alias -p

https://www.si.edu/tbma/resource/method-maintaining-bashrc-file-across-multiple-workstations-using-homebrew-and-github

https://oit.ua.edu/wp-content/uploads/2020/12/Linux\_bash\_cheat\_sheet-1.pdf https://linuxhandbook.com/grep-command-examples/
