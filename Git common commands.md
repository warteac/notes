###常用git命令###
+ create a branch and switch to it

$ git checkout -b mybranch  #switch to mybranch, create it if it doesn't exsist

$ git branch mybranch    #create

$ git checkout mybranch  #switch

+ view the current branch

$ git branch

+ delete a branch

$ git branch -d mybranch

+ clone the git repository

$ git clone https://myproject.git 

+ view the current state

$ git status #view the modified files 

$ git diff   #view the modified detail

+ submit your modification（add-commit-push), please checkout your branch and view the git status first to esure you submit the right content.

$ git add filename

$ git commit -m "comments"

$ git push origin mybranch

+ merge for master

$ git checkout master

$ git merge mybranch

+ git config the global name and email 

$ git config --global user.name "myname"

$ git config --global user.email "myemail@email.com"
