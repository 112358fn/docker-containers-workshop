# Docker session 1

## User vs kernel space
Let's check this 
In the host
```
cat /etc/lsb-release
uname -sr
```
and

In the container:
```
docker run -it --rm ubuntu:20.04
cat /etc/lsb-release
uname -sr
```

> ATTENTION: notice how the OS is different but the kernel is the same!

## Getting inside the container
In the previous step we used this to get inside the container:
```
docker run -it --rm ubuntu:20.04
```
Let's break it down:
* `docker`: we docker CLI to interact with docker's containers engine
* `run`: we pass the parameter to run (create a container and execute) from an image
* `-it`: this option of run allows to get stdin and stdout from the container (keyboard and terminal output)
* `--rm`: this options tells the engine to remove the container once it stops
* `ubuntu:20.04`: it's the docker image we will use for this container

## Images vs Containers
1. Intro to cowsay: [Read more here](http://manpages.ubuntu.com/manpages/bionic/man6/cowsay.6.html)
```
 _____________________________________
/ What did the coach say to the cows? \
\ "Now get out there and give me 2%!" /
 -------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

```
You can try differents animals with something like: `-f tux` or `-f stegosaurus`
2. Getting inside the container and installing cowsay
```
docker run -it --rm ubuntu:20.04
apt update
apt install cowsay -y
/usr/games/cowsay "Hello world"
```
3. Good! Now we should be able to get cowsay again, right?:
```
docker run -it --rm ubuntu:20.04
/usr/games/cowsay "Hello world"
``` 

> __Images__: are read-only
>
> __Containers__: are running instances of those images

## Dockerfile: Custom images
Let's create an image that already has cowsay installed:
1. Create a file named: `Dockerfile` (This file is already in your folder)
2. Reference of the Dockerfile: [official docs](https://docs.docker.com/engine/reference/builder/)
3. Building the image:
```
docker build -t myubuntu:YOUR_NAME .
# For example
# docker build -t myubuntu:ali .
# docker build -t myubuntu:charlotte .
# docker build -t myubuntu:deniz .
# docker build -t myubuntu:fabien .
```
4. The `RUN` instruction ([link to the doc](https://docs.docker.com/engine/reference/builder/#run))
5. Adding the needed `RUN` instructions:
```
RUN apt update
RUN apt install cowsay -y
```
6. Rebuilding the image
```
docker build -t myubuntu:YOUR_NAME .
```
7. Using the new image
```
docker run -it --rm myubuntu:YOUR_NAME
/usr/games/cowsay "Hello World"
```

## Running a command in a new container
From the help of docker run:
```
docker run --help

Usage:  docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

Run a command in a new container
```
That means that we can use our image and use it to run a particular command, for example `/usr/games/cowsay "Hello World"`

Let's try it:
```
docker run -it --rm myubuntu:YOUR_NAME /usr/games/cowsay "Hello World"
```
So all this time we have been using the default command from the `Dockerfile`!!! Check the last line of your `Dockerfile`

## Copying files vs Mounting volumes
When we need to get files inside our containers we have two options:
1. Include this files in the image: but that means the file will only change when we rebuild the image
2. Mount the file in the container as a volume.
Let's use both options to understand when to use each.

### Copying files
1. The `COPY` instruction ([link to the docs](https://docs.docker.com/engine/reference/builder/#copy))
2. We can add this to our `Dockerfile` to copy a file in our custom image:
```
COPY $PWD/cow_joke.txt cow_joke.txt
```
3. Now we need to rebuild our image so it includes our text file.
```
docker build -t myubuntu:YOUR_NAME .
```
4. We can use the command `cat cow_joke.txt` to read the file from the image
```
docker run --rm myubuntu:YOUR_NAME cat cow_joke.txt
```
5. We can even make the cow say the joke
```
docker run --rm myubuntu:YOUR_NAME /bin/bash -c 'cat cow_joke.txt | /usr/games/cowsay'
```
6. Let's simplify this using the `CMD` instruction in the `Dockerfile`
```
CMD cat cow_joke.txt | /usr/games/cowsay
```
> We need to rebuild the image since we have modify the `Dockerfile`
```
docker build -t myubuntu:YOUR_NAME .
```
7. Now we can simply do:
```
docker run --rm myubuntu:YOUR_NAME
```

### Mounting volumes
All this building can get tiring and what about if we need files back from the container ??

For that we can use use mounted volumes. [Read more here](https://docs.docker.com/storage/volumes/)

Let's say we now have a better cow joke like the one in `better_joke.txt`. We can use that file to override the original
joke and for that we use `-v` 
```
docker run --rm -v $PWD/better_joke.txt:/cow_joke.txt myubuntu:YOUR_NAME
```
