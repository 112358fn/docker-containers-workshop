FROM ubuntu:20.04
# ---- Add your code here ----
RUN apt update
Run apt install cowsay -y
COPY $PWD/cow_joke.txt cow_joke.txt
# ---- Dont' modify bellow this ----
CMD cat cow_joke.txt | /usr/games/cowsay -f stegosaurus