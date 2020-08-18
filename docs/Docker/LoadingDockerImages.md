# How to load Images in Docker

## Check for Images
- In terminal type `docker images`, this will show blank if no images is loaded
- `docker image ls` will also produce the same result

## Loading image

- Go To DockerHub and search for your desired image. In the image Homepage, there is a docker command, Copy and paste it in the terminal to load the load image
- if u already know the name of the image , `docker pull <name-of-the-image>` will pull it inside docker.

## Removing Images
- To remove an image, enter `docker rmi <name-of-the-image>`
- Remove multiple images by adding name of images on after another , `docker rmi <name-of-image-01> <name-of-image-03> <name-of-image-03>`