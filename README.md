# docker-yeoman-angular

Docker image intended to create a development environment for an AngularJS v1 project

### Table of content
- [Installation](#installation): step-by-step instruction to getting this presentation running on your computer.

## Installation

1. Get the Docker image
* easy way:
```sh
$ sudo docker pull cdue/docker-yeoman-angular:latest
```

* or a bit more complicated (build it from the given DockerFile):
```sh
$ git clone https://github.com/cdue/docker-yeoman-angular.git
$ cd docker-yeoman-angular
$ sudo docker build -t "cdue/docker-yeoman-angular:latest" .
```

3. Run the Docker container

* Run a terminal
```
$ docker run --rm -it -v $(pwd):/app/ -p 9000:9000 "cdue/docker-yeoman-angular:latest"
```
if you need to log in as root:
```
$ docker run --rm -it -v $(pwd):/app/ -p 9000:9000 -u 0 "cdue/docker-yeoman-angular:latest"
```

This will mount your current directory as a volume in your container so that files created using 'yo' will also be available outside of the container.

Note: Windows users may add a '/' before $(pwd), and current directory must be located under their User directory (for example : /c/Users/[username]/[project_folder]).

Then you will be able to run any 'yo', 'bower', 'npm', 'grunt', 'karma' commands from inside the container.

For example:
```
$ yo angular
```
This will ask you a few questions about how to generate your angular project.
Note that you can use numpad or arrows+space to choose which modules you want to install.

4. Create a new angular app
As you already seen, you can generate an Angular project using 2 commands, but why wouldn't you only use 1:
```
$ docker run --rm -it -v /$(pwd):/app/ "cdue/docker-yeoman-angular:latest" yo angular
```

5. Run your web app

First, don't forget to update your GruntFile.js so that you can expose your app outside of the container:
Replace:
```
connect: {
  options: {
    port: 9000,
    // Change this to '0.0.0.0' to access the server from outside.
    hostname: 'localhost',
    livereload: 35729
  },
```
with:
```
connect: {
  options: {
    port: 9000,
    // Change this to '0.0.0.0' to access the server from outside.
    hostname: '0.0.0.0',
    livereload: 35729
  },
```

Then use Grunt to serve your app:
```
$ docker run --rm -it -v /$(pwd):/app/ -p 9000:9000 -p 35729:35729 "cdue/docker-yeoman-angular:latest" grunt serve
```


If your OS is Linux based, you can access your slides at :
http://127.0.0.1:9000

But if your running Windows, you need to get the docker-machine VM IP with:
```sh
$ docker-machine ip [your VM: default / my-default / ...]
```
and then use it:
for example:
http://192.168.99.100:9000

If you don't know your VM name, use:
```sh
$ docker-machine ls
```

6. Run tests on your app
```
$ docker run --rm -it -v /$(pwd):/app/ -p 9000:9000 -p 35729:35729 "cdue/docker-yeoman-angular:latest" grunt test
```
