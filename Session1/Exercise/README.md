# Session1 - Exercise
## Folder Content
This folder contains the template for the `Dockerfile`, a few `cow_joke*` and a single `better_joke.txt`.

The idea is to set up folders for the participants with their own `Dockerfile` and `cow_joke.txt`.

If they are working in the same host the images can be tagged with the participant name. To further differenciate the images `cowsay` can be passed `-f` to show different animals instead of the cows (Ex.:`-f kitty`).

When working in the same host the different images with different animals can be used to show the different jokes using the mount volumes. This showcase the idea of overrriding files in the image. For this same reason, the `better_joke.txt` is the same across the participants.
