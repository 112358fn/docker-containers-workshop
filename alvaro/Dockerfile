FROM ubuntu:20.04
RUN apt update
RUN apt install -y cowsay
COPY $PWD/cow_joke.txt cow_joke.txt
CMD cat cow_joke.txt | /usr/games/cowsay

