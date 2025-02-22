# gitserver-srv
A lightweight Git Server Docker image built with Alpine Linux. Available on [GitHub](https://github.com/officialmofabs/gitserver) and [Docker Hub](https://hub.docker.com/r/officialmofabs/gitserver/)

!["image git server docker" "git server docker"](https://raw.githubusercontent.com/officialmofabs/gitserver/master/gitserver.jpg)

### Basic Usage

How to run the container in port 2222 with two volumes: keys volume for public keys and repos volume for git repositories:

	$ docker run -d -p 2222:22 -v ~/gitserver/keys:/gitserver/keys -v ~/gitserver/repos:/gitserver/repos officialmofabs/gitserver

How to use a public key:

    Copy them to keys folder: 
	- From host: $ cp ~/.ssh/id_rsa.pub ~/gitserver/keys
	- From remote: $ scp ~/.ssh/id_rsa.pub user@host:~/gitserver/keys
	You need restart the container when keys are updated:
	$ docker restart <container-id>
	
How to check that container works (you must to have a key):

	$ ssh git@<ip-docker-server> -p 2222
	...
	Welcome to gitserver!
	You've successfully authenticated, but I do not
	provide interactive shell access.
	...

How to create a new repo:

	$ cd myrepo
	$ git init --shared=true
	$ git add .
	$ git commit -m "my first commit"
	$ cd ..
	$ git clone --bare myrepo myrepo.git

How to upload a repo:

	From host:
	$ mv myrepo.git ~/gitserver/repos
	From remote:
	$ scp -r myrepo.git user@host:~/gitserver/repos

How clone a repository:

	$ git clone ssh://git@<ip-docker-server>:2222/gitserver/repos/myrepo.git

### Arguments

* **Expose ports**: 22
* **Volumes**:
 * */gitserver/keys*: Volume to store the users public keys
 * */gitserver/repos*: Volume to store the repositories

### SSH Keys

How generate a pair keys in client machine:

	$ ssh-keygen -t rsa

How upload quickly a public key to host volume:

	$ scp ~/.ssh/id_rsa.pub user@host:~/gitserver/keys

### Build Image

How to make the image:

	$ docker build -t gitserver .
	
### Docker-Compose

You can edit docker-compose.yml and run this container with docker-compose:

	$ docker-compose up -d
