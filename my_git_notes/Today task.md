Today I'm learn how to sync obsidian with GitHub

**why git :**
All developer use git because git allow to all developer work together from anywhere. 
it help collaborate all members in multiple projects. all developer see full history off code. if any bug and mistake in project git is allow to fix and revert 

**what is git ?**
git is distributed control version . it is a open source . it help to track the code and changes in large file or project.

**what is GitHub**?
git and GitHub is not same. git is a tool and GitHub is a large host of source of code in the world.

---**16.1.2026**---
--
**git SSH key concept**

why we use SSH key ?  
SSH key give me more secure and password less method for authenticating.
after I'm generate key then save in GitHub then clone private repo on my local machine.

**git commands**
--
**1: git --version** 
	( this cmd use for check git version)
	
**2: git configuration**
	   git config --global user.name "name" 
	   git config --global user.email "email"
	   this cmd use for the attach author info for any commit.
	   
3: **git init**
	   this is cmd use for make current for into git repository.

4: **git status**
		this cmd is show untrack, modify and stage files in git.

5: **git add .**
		before commit we are run this cmd because all file set in staging area 

6: **git commit -m**
		this cmd create snapshot of project.

7: **git checkout -b dev**
		Create & switch new branch

8: **git merge branch**
		create two branch dev & dev2, this edit same file in this branch after we marge dev to dev2 then show conflict

9: **git cherry pick** 
		this cmd use for specific commit  move other branch to main branch 
10: **git rebase** 
		Git rebase takes commits from your branch and re-applies them on top of another branch main
	
11: **diff git merge & rebase** 
			merge                                                                              rebase
		Creates a merge commit                                              No merge commit
		History becomes branched                                           History stays linear 
12: git push/pull
		
