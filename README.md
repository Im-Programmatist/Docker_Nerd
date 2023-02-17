# Docker 


## To install docker in linux
    $ yum install docker -y

1. To check whether service start or not- docker engine run or not
    $ service docker status 
    or
    $ docker info
2. To start docker demon/engine
    $ service docker start
3. To find images available on docker engine(locally)
    $ docker images
4. To search images on docker hub 
    $ docker search express
5. To pull image from docker hub to local 
    $ docker pull bitnami/express
6. To pull and run image from docker hub
    $ docker run bitnami/express
7. To give name to creating container 
    $ docker run -it --name container_name ubuntu /bin/bash
    (here -it is interactive mode, ubuntu is image name, bin/bash is for run commands in terminal which is created by -it )
8. To start container
    $ docker start container_name(ubuntu,express etc)
9. To go inside container 
    $ docker attach container_name
10. To see all container available 
    $ docker ps -a 
    (-a is all)
11. To see only running container
    $ docker ps
    (ps - is process status)
12. To stop container
    $ docker stop container_name
13. To delete container
    $ docker rm container_name
14. check where is docker installed
    $ which docker
15. If we want to check difference between base image and changes worked on it in container 
    $ docker diff container_name updated_image_name
16. Info in file would show C,A & D 
    C - changes
    D - deletion
    A - Append Or Addition 
17. Create docker image from container
    $ docker commit container_name image_name 
    (image_name is new name for file, above command give SHA code)
18. Create docker file using commands 
    1. File name should be - Dockerfile (D- capital)
    2. Once we create file, we can change it as per need but file name should be as mentioned
    3. Docker File Components (All component words must be in caps)
    - https://www.youtube.com/watch?v=vEv0IxPEsW0
        1. FROM - It is at the top of the files & it is foe base image (we mention base from where or for what we are creating this image)
        2. RUN - To execute the commands, it will create layers in image
        3. MAINTAINER - Author Name/ Description about file
        4. COPY - Copy files from local system(docker vm), We need to provide source & destination
            (We can not download file from internet and any remote repo)
        5. ADD - Similar to COPY but, it provide feature to download file from internet, 
            also we extract file at docker image file (from .zip or .tar.zip)
        6. EXPOSE - To expose point such as port 8080 for tomcat, port 80 for nginx etc
        7. WORKDIR - To set working directory for  a container(where we want to work and make changes)
        8. CMD - Execute commands but during container creation 
        9. ENTRYPOINT - similar to CMD but has higher priority over CMD, 
            First commands will be executed by ENTRYPOINT only (if some task run before other it can place in this.)
        10. ENV - Environment Variables 
        11. ARG - argument command also known as build-time variables. Running containers can’t access values of ARG variables
    Sample file :- 
        FROM ubuntu
        RUN echo "CHetan Korde Patil" > /tmp/testfile
19. To create docker image from docker file 
    $ docker build -t myimg . 
    (here, '.' is current directory, -t is tag )
    then see running image - $ docker ps -a  OR docker images
    then create container using run command - docker run -it --name newcontname myimg /bin/bash

20. Example of Docker File - 
        FROM ubuntu
        WORKDIR /tmp
        RUN echo "This is sample docker file and creating image from it" > /tmp/testDockImg
        ENV myname chetan korde 
        COPY testfile1 /tmp
        ADD test.tar.gz /tmp
        
        (*** ENV variables can see using $ echo $variable_name like $ echo $myname here )

21. Create Shared Volume/Directory in Docker
    - https://www.youtube.com/watch?v=OoZxPUgpUUM
    1. Container to Container - Create container and make volume and share it with other container
    2. Host to container share volume - shared with host machine where container created
    3. Even if we stop container we can access volume to others
    4. Volume created in one container only, once create then we can share it with other container
    5. Volume only create at the time of creating container, we must mention it to create.
    6. Volume can be shared with n number of containers
    7. When we add something in volume it will reflect in other container where volume is shared and wise versa.
    8. *note that if we create cvolume in container and then make image from it, once we create new container
        from that image then that volume created in previous container not considered as shared value it will ne new and not reflect changes happening in previous containers volume
    9. Advantages - 
        1. decoupling from container - volume will be exist even if container stopped
        2. we can share data samong containers and also host to container and wise versa
        3. If one big file downloaded in container then other do not need to download it other can share it
        4. on deleting container volume not deleted
    10. To create volume component use in docker file 
        VOLUME ["/my_volume_dir"] 
        then create image file, create container (command mentioned above)
        **Now we will see directory/volume
    11. Share volume share with other container
        $ docker run -it  --name new_container_name --privileged=true --volumes-from previous_container_name ubuntu(image name) /bin/bash
        now new container can access volume, if we add new file in it it will be visible in previous container

22. Port expose and port publish, difference between docker exec and docker attach 
    - https://www.youtube.com/watch?v=p4HuoL7hwXI
23. Create docker hub account and publish image 
    - https://www.youtube.com/watch?v=3moRvwkqy_A

## References
https://docs.docker.com/get-started

https://medium.com/bb-tutorials-and-thoughts/docker-a-beginners-guide-to-dockerfile-with-a-sample-project-6c1ac1f17490

sample docker files
https://github.com/bbachi/dockerfile-examples/blob/master

https://www.youtube.com/watch?v=ZGnHJzPJdd0

https://docs.docker.com/desktop/install/windows-install/

https://www.youtube.com/watch?v=X3Wtjwu0vBI

Shared By Vicky--->
https://levelup.gitconnected.com/how-to-deploy-an-application-to-aws-fargate-cd3cfaccc537

Node-Express App Image
https://nodejs.org/en/docs/guides/nodejs-docker-webapp/
