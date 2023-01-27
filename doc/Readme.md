= To create the local repository = 

# CD to a directory that has your source code. 
In this case, the directory is named ''made.up.example''

# Initialize the git repository

    % git init . [--bare] 
	
	   Creates the './.git' directory which is where git 
	   stores its information about this repo
# Add the files	   

    % git add . # Adds all the files in '.'

# Commit the 'changes', which in this case is the creation event.

    % git commit -m "initial commit"
	Of whatever message you want
	

At the moment, all you have is a local repository. Eventually you want to add it to a remote resource, e.g. github.com. To do this, you have to specify a ''remote'' repository. And then you'll tell git that you want to ''push'' changes to that repository. (However, it's possible to have a ''remote'' repository that is actually ''local'', i.e. on your current machine. So far as I can tell, ''remote'' only means ''not this current repo'', not necessarily on a different machine which you access via some remote protocol like https or ssh.

= Connecting/working with remote repositories =
