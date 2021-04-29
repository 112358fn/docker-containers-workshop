FROM ubuntu:20.04
# ---- Add your code here ----
# ---- Dockerfile: Step 5 ----
RUN apt update
RUN apt install cowsay -y
# ---- Copying files: Step 2 ----
COPY $PWD/cow_joke.txt cow_joke.txt
# ---- Copying files: Step 6 ----
CMD cat cow_joke.txt | /usr/games/cowsay -f tux
