FROM ubuntu:20.04
# ---- Add your code here ----
RUN apt update
RUN apt install cowsay -y
# ---- Dont' modify bellow this ----
COPY $PWD/cow_joke.txt cow_joke.txt
CMD cat cow_joke.txt | /usr/games/cowsay -f tux