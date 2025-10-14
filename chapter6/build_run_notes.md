## How To Build & Run

using the dockerfile in the chapter6 root, this is the build context

```docker build -t <yourImageName> .```

then check the image

```docker images``` 

then create a container instance from the image with port mapping

```docker run -p 80:80 --name apiContainerName <yourImageName>```