If you don't know what git is, it's the next generation of version control. If you don'tknow what a version control system is, ....

A Version Control System stores changes you make in files. Normally the files you're changing are source coade files of some computer language, but they could be '''anything'''.  

Some only store the changes and don't help at all in managing your ''project'' (i.e. how those files fit together in some coherent software system). Some help a little more in managing the project, rather than the files themselves (which seems the easier of the two tasks). Some work locally, some have some element of a distributed environment. 

The shortest history of VCS systems possible: In the beginning, there was SCCS, then RSC, the CVS (which is mostly what I use now), then SVN based on CVS, then Git. 

There are probably more on the horizon. If you want the gorey details, see [https://en.wikipedia.org/wiki/Apache_Subversion the wikipedia article on version control systems]

I will only disucss CVS and Git, since I'm basically moving from the former to the latter. 

CVS works by maintaining a central ''repository'' (I'm going to call it the ''mother'') containing the ''archives'' of the files  you're working on. The ''archives'' are the record/history of all the changes you've made to the file. It also allows you to see the changes you've made, all the way back to the very first commit, and to take the development onto a ''branch'' and bring it back into the ''main'' branch. 

A quick aside on what a ''branch'' is. Consider the following, all too common, scenario. 

I released my software and then, a year later, someone complained about a bug in the released code. So I have to have a way to 1) go back to the state of the system as released, 2) fix the bug, 3) deliver patched code to the user without disturbing my current development. This is branching in a nutshell: the ability to grab the state of the repository at a moment in time, make changes, keep those changes separate from the current line of development, either create a new 'deliverable', or stir the changes back into the current line (or both). 

Branching is fundamental to almost any version control system. 

Okay, back from the aside. 

CVS worked, mostly, but makes access to that ''mother'' repository something of a problem. When someone does a checkout in CVS, one gets the 'Tip' revision of whatever branch you're on, by default the 'main' branch.


Git gets around this somewhat by eliminating the concept of the ''mother'' repository that is the final arbitrator of what is 'accepted' (to some extent). In Git, when someone does a ''checkout'' (called a ''clone'') of a Git repository, you get '''the entire repository''', including the history of '''all changes'''. 

In essence, everyone has the ''mother''. The work is completely distributed and people and checkout (''clone'') and ''add'' and ''commit'' to these local repositories. 

There is also the concept of a ''remote'' repository (which needn't be on another machine, but which usually is, and is frequently hosted by a Git hosting service (e.g. github.com). Using such ''remote'' repositories, when  those working on software want (and are allowed to), they may add to the ''remote'' repo by means of the ''push'' command. 



Does this lead to chaos and mayhem? Could do. But there are some other practices which control who gets to modify which repository. 

= Getting started with Git =

I recommend you start by doing everything at the shell prompt. There is a truly bewildering beastiary of git related tools out there, command-line tools (gh == ''Git Hub'', which, supposedly, helps you interfact with github.com) and Gui like programs (GitDesktop, gitk), modes for editors (Emacs + magit) and probably plugins for various IDEs like VSCode and RStudio.

But all of these require a solid base in the basic Git commands, so start with the command-line. Then, when you're comfortable, start investigating the tools that are useful to you. I'm assuming you have ''git'' installed. If not, go find a sysadmin and get it installed. It should be ''de riguer'' to have git installed these days.

Here's a precis of what you'll need to do. 

# Create a Local Repository
# Add files to the Local Repository
# Commit changes
# Create a remote repository
# Clone from a remote repository
# Link local to remote repository
# Push changes to remote
# Pull changes from remote
# 
= To create the local repository = 

There are two ways to get files into a git repository
# As simple files
# As an existing CVS repository. 

I don't know how to do the latter, so I won't cover it. 

That means, these files are just that, a set of directories with files in them, but *not* a CVS working copy (meaning there are no CVS subdirectories in the tree containing these files) If this is a CVS working copy, do a CVS export to another directory and work with that.

I'm assuming you have a directory tree which you want to store in a Git repository. Git works recursively (keep that in mind)

# CD to the top directory that has your source code. 

In the  case I'm demonstrating here, that directory is named ''made.up.example''

# Initialize the git repository

    % git init . [--bare] 
	
Creates the './.git' directory which is where git  stores its information about this repo. AT the moment, the repository is '''completely empty'''


# Add files

    % git add . # Adds all the files in '.'

There are two steps to storing new changes into the repo: ''adding'' and ''commiting''. Think of the ''add'' step more as ''staging'', i.e. you're telling Git that there will be some further operation on each file you've 'add''ed. 

This is different from CVS. When you ''add'' a file in CVS, it's a one-time thing. You're telling CVS that this is a file you want to track. Thereafter, you only ''commit'' changes. 

When you make changes in a Git repo, you first tell Git that the changes you've made are important to you, so you ''add'' (''stage'') those changes. Then you ''commit'' the changes. 

That way, if you decide you don't want to commit the changes for any particular file you've ''added'', you can ''remove'' (''unstage'') that file and only commit the files that remain in the ''added'' (''staged'') category.

As you can probably guess, I think they missed an opportunity in the naming here: they should have called it ''stage'' rather than ''add''.


# Commit the 'changes', which in this case is the creation event.

    % git commit -m "initial commit"
	Or whatever message you want
	

At the moment, all you have is a local repository. Eventually you want to add it to a remote resource, e.g. github.com. To do this, you have to specify a ''remote'' repository. And then you'll tell git that you want to ''push'' changes to that repository. (However, it's possible to have a ''remote'' repository that is actually ''local'', i.e. on your current machine. So far as I can tell, ''remote'' only means ''not this current repo'', not necessarily on a different machine which you access via some remote protocol like https or ssh.

= Connecting/working with remote repositories =

You have to connect this repository to a remote repository. As I pointed out above, this could just be another repository on the same machine, it doesn't have to be ''remote'' in the sense of being somewhere else on the internet. 

But the most common case (probably 99.9999999%) is to have it on a remote machine and, typically, on some version of github. This example will be on github.com, where I have a personal github account.

But to be clear, when you're working on your software (or whatever it is), all of your ''adds'', ''removes'', ''renames'' and, most importantly, you're ''commits'' happen '''only to the local repository!!!'''. It requires '''another step''', meaning '''another set of commands''' to send those changes to a remote repository! (the ''fetch'', ''clone'', ''push'' and ''pull'' commands)

So, you can be somewhat at ease. You can screwup as much as you want, but so ong as you haven't issued any ''fetch'', ''clone'',''push'', or ''pull'' commands, nothing in the remote has changed!


For this I'm using the command line utility ''gh'' (which is provided by github for managing these connections at the command line. But you can do this with vanilla Git (See [https://docs.github.com/en/get-started/getting-started-with-git/managing-remote-repositories#adding-a-remote-repository this article at GitHub Docs].) which is the '''Github CLI''' (i.e. the github command-line-interface). Basically, it wraps some sets of Git commands up into a bundle, so you can do some things more easily. But, as I said, you can to this with vanilla git commands.

See [https://cli.github.com/manual/installation This page on installing gh] at Github.com. It involves using a package manager named ''homebrew'' (Macintosh) or ''Linubrew'' on Linux. If you have ''homebrew'', do

     % brew intall gh
	 


# Adding a remote

The nice thing about the ''gh'' is that it steps you thgouth the process prompting you at each step and filling in some defaults. I'll show you the entire interaction. Everything with a '?' at the beginning of the line is a ''gh'' prompt, everything surrounded by angle brackets '<...>' are my comments.

	[tmp/made.up.example % gh repo create 
	? What would you like to do? Push an existing local repository to GitHub

	  <menu: create from scratch or add local>

	? Path to local repository .

	  <If you choose to add a local repo, it assumes you're 
	  in the working directory and defaults to '.'>

	? Repository name made.up.example
	  <It defaults to the name of the directory you're in.
	  which is the name of the directory where I'm working>

	? Description: Working with git and github (gh)

	  <A description. I guess this will end up in some file
	  in the github repo, but I don't know.>

	? Visibility Public
	  <public/private/Internal Don't know what 'internal' is>

	✓ Created repository whdaffer/made.up.example on GitHub
	? Add a remote? Yes

	? What should the new remote be called? origin

      <went with the default. If you look this up on the web, 
	  you'll see this is the default name for the remote. 
	  I don't know why.>
	
	  ?would you like to push commits from the current branch to "origin"? (Y/n)
  
	   <The default is 'Y', which means that my 
	    repo on github now exists and is populated 
		with my files>

And just to see what is my ''remote'' repo, I do

    git remote -v
    origin	git@github.com:whdaffer/made.up.example.git (fetch)
    origin	git@github.com:whdaffer/made.up.example.git (push)	
	
	
	
# Deleting a remote

Which really means telling git that this particular local Repo is no longer associated with the remote as it once was. This doesn't delete it from the server, it just severs the connection between the local and the remote. To actually delete it, you have to log onto your server and do it there. 

And, believe me, it takes some work! The '''really, really, really''' want you to understand that telling (e.g.) github.com to delete a repository is '''permanent!'''

I haven't done this, yet. Maybe when I'm finished with this madeup example I will. Remember,however, that deleting the either the connection to the  remote using the following command doesn't delete '''anything''' on your local machine! You still have your local repo. And, even deleting it on the server doesn't delete your local repo. 

You have to use the regular file-handling commands on whatever machine you're working on to delete the source-code itself.

The vanilla Git command is

    % git remote rm <remote-name>
	
In my case, it would be (this is untested!!!)


    % git remote rm git@github.com:whdaffer/made.up.example.git
	
If I then followed that with ...

     % cd ..; rm -rf ./make.up.example.git
	 
I would have removed my local repo. 

'''However''', the repository on the remote server will still exist and contain all the data I've added, committed and pushed to it! 

I say this just to emphasize how ''distributed'' this system is, and increase your peace of mind that it really takes quite a bit of work to completely eliminate all the files once you've put them in a local Git repo to which you have 1) connected a remote and 2) pushed your changes to that remote!


= cloning a remote repository =


Once you have them on a remote, you can then go to another machine and clone the repository. 

This will involve us in some nomenclature and discussion of authentication protocols. 

Information on how you ''clone'' a repository is contained in the URL that specifies the repo. In my case, the URL is 

    git@github.com:whdaffer/made.up.example.git
	
	
If I went to another of my machines and did

	
	
	
	
