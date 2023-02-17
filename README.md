# Docker 


**To install docker in linux**
    - $ yum install docker -y

1. To check whether service start or not- docker engine run or not
    - $ service docker status 
    or
    - $ docker info
2. To start docker demon/engine
    - $ service docker start
3. To find images available on docker engine(locally)
    - $ docker images
4. To search images on docker hub 
    - $ docker search express
5. To pull image from docker hub to local 
    - $ docker pull bitnami/express
6. To pull and run image from docker hub
    - $ docker run bitnami/express
7. To give name to creating container 
    - $ docker run -it --name container_name ubuntu /bin/bash
    - here -it is interactive mode it run docker and enter in container(not create and leave out, for this use -td), 
    - & ubuntu is image name, bin/bash is for run commands in terminal which is created by -it 
8. To start container
    - $ docker start container_name(ubuntu,express etc)
9. To go inside container 
    - $ docker attach container_name
    - this attach will bring you in same process with existing process id - ppid (parent process id)
    - $ docker exec container_name
    - this exec will bring us inside container but in new process - pid
10. To see all container available 
    - $ docker ps -a 
    (-a is all)
11. To see only running container
    - $ docker ps
    (ps - is process status)
12. To stop container
    - $ docker stop container_name
13. To delete container
    - $ docker rm container_name
    To delete image
    - $ docker rmi image_name/img_id
14. check where is docker installed
    - $ which docker
15. If we want to check difference between base image and changes worked on it in container 
    - $ docker diff container_name updated_image_name
16. Info in file would show C,A & D 
    C - changes
    D - deletion
    A - Append Or Addition 
17. Create docker image from container
    - $ docker commit container_name image_name 
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
19. To create docker image from docker file & Run it 
    - $ docker build -t myimg . 
    (here, '.' is current directory, -t is tag )
    then see running image - - $ docker ps -a  OR docker images
    then create container using run command - docker run -it --name newcontname myimg /bin/bash
    - $ docker build . -t <your-username>/<name-for-image>
    - eg. docker build . -t ckorde/node-swagger-web-app
    - **Run Docker image and make container by publishing on port**
    - docker run -p host_port:container_port -d <your-username>/<name-for-image>
    - eg. - docker run -p 49160:9092 -d ckorde/node-swagger-web-app
    - -d runs the container in detached mode, leaving the container running in the background. The -p flag redirects a public port to a private port inside the container.
    - **Print app output**
    - $ docker logs <container id> (container id get from $ docker ps)
    - **Kill our running container**
    - $ docker kill <container id>

20. Example of Docker File - 
        FROM ubuntu
        WORKDIR /tmp
        RUN echo "This is sample docker file and creating image from it" > /tmp/testDockImg
        ENV myname chetan korde 
        COPY testfile1 /tmp
        ADD test.tar.gz /tmp
        
        (*** ENV variables can see using - $ echo $variable_name like - $ echo $myname here )

21. Create Shared Volume/Directory in Docker
    - https://www.youtube.com/watch?v=OoZxPUgpUUM
    1. **Container to Container - Create container and make volume and share it with other container**
    2. **Host to container share volume - shared with host machine where container created**
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

    10. **To create & share volume component use in docker file**
        VOLUME ["/my_volume_dir"] 
        
        (***Sample Code --->

            FROM ubuntu
            WORKDIR /tmp
            RUN echo "This image" > /tmp/createVolume
            ENV myname 'chetan korde'
            COPY testfile1 /tmp
            ADD test.tar.gz /tmp
            VOLUME ["/myvolume"]

        ***)
        then create image file, create container (command mentioned above)
        **Now we will see directory/volume

    11. Share volume share with other container
        - $ docker run -it  --name new_container_name --privileged=true --volumes-from previous_container_name ubuntu(image name) /bin/bash
        now new container can access volume, if we add new file in it it will be visible in previous container

    12. **Try to Create & share Volume Using Commands**
        - $ docker run -it --name cont_name -v /valume_name image_name /bin/bash
        (this -v will create new volume)
        - share volume: 
        - $ docker run -it new_cont_name --privileged=true --volumes-from cont_name_where_vol_created image_name /bin/bash 

    13. **Try to share directory/volume with host and docker engine**
        - $ docker run -it --name cont_name -v /home/ec2-user:/valume_name --privileged=true image_name /bin/bash 
        (':' this colon map hostdir with new vol, it also distinguish between host directory and new volume/dir while creating using command)
    
    14. **Other commands**
        - $ docker volume ls --> list of all volume 
        - $ docker volume create vol_name --> create normal volume locally
        - $ docker volume rm vol_name --> delete used volume
        - $ docker volume prune --> It remove all unused docker volumes
        - $ docker volume inspect vol_name --> to check details of volume
        - $ docker container inspect cont_name --> to check details of container

22. Port expose and port publish, difference between docker exec and docker attach 
    - https://www.youtube.com/watch?v=p4HuoL7hwXI
    - We can not change docker container's port it is defined already on the other hand we can change port in host
    - Docker port expose means we map host port with docker container port, request come to host 80 port then it is mapped with container and request come to container
    - **$ docker run -td --name cont_name -p 80:80 ubuntu**
    - eg - docker run -td --name swagger-container -p 9092:9092 ckorde/node-swagger-app
    - here td is tag demon it run not bring us in container, 80:80 means first port is host map with next container port 
    - P(publish) is override to expose command for port to communicate among containers in local as well as internet
    - Instead of P we can use expose but it will allow to communicate locally(communicate with all other container available on that host) only not on internet
    - If not mention either P(publish) or expose, then service in container will only be access within container
    - **$ docker port cont_name** 
    - above command will show all port mapped with host
    - **$ docker exec -it cont_name /bin/bash**
    - exec also used like attach, this exec used to go inside container but with new process, attach bring us in previous process running.
    - if we create container using ubuntu install server using - apt-get update, apt-get install apache2 -y
    - then cd /var/www/html then $ echo "hellow world!!" >index.html
    - service apache2 start

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

Shared By--->
https://levelup.gitconnected.com/how-to-deploy-an-application-to-aws-fargate-cd3cfaccc537

Node-Express App Image
https://nodejs.org/en/docs/guides/nodejs-docker-webapp/
