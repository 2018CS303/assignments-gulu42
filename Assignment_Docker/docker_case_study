# **Docker Case Study** - Using Docker To Make Infrastructure Available for Learning and Development

### **System Requirements( As mentioned on Slides )**:-
1. Dynamic Allocation of Linux systems for users
2. Each user should have independent Linux System
3. Specific training environment should be created in Container
4. User should not allow to access other containers/images
5. User should not allow to access docker command
6. Monitor participants containers
7. Debug/live demo for the participants if they have any doubts/bug in running applications.
8. Automate container creation and deletion.

## Dynamic Allocation of Linux systems for users
### (Requirements 1 to 5)
1. To allocate resources for different users on the same system, we keep a file with the user names and use the bash script `create_user_containers.sh` which creates a docker container for every user specified in the file.

    - `users.txt`
        ```
        user_1
        user_2
        user_3
        user_4
        ```
    - `create_user_containers.sh`
        ```sh
        echo -n "Enter users file: "
        read file

        while read user
            do
                docker create -it --name $user <Docker Image> /bin/bash
            done < $file
        ```
2. Run the shell script `create_user_containers.sh` and pass the `users.txt` file. This creates a docker container corresponding to each username from that file.

3. Here, we use a docker image corresponding to the environment we want. This way all users have the same environment and its easy to change it, just use a different image.

4. The user can then use the container allocated to him/her using the `use_container.sh` script described below.
    - `use_container.sh`
        ```sh
        echo -n "Enter your username: "
        read name
        docker start $name
        docker attach $name
        ```
5. This way, a user can enter their allocated system to perform their tasks without affecting other users. A user will not be able to access the resources of another user while he is in his container(because,well docker!). The entry can be based on some password based system to ensure a user enters only his/her container.

6. Doing this we have met the following requirements:
    - Dynamic Allocation of Linux systems for users
    - Each user should have independent Linux System (Users use seperate containers)
    - Specific training environment should be created in Container (Specified by the Docker Image)
    - User should not allow to access other containers/images (Not possible since every user is in a container)
    - User should not allow to access docker command (By default, a docker container can't use docker, so this has been achieved)

## Monitoring a users container
### (Requirement 6)
- To monitor a users container one can use the `monitor_container.sh` script described below
    - `monitor_container.sh`
      ```sh
        echo -n "Enter name of user to be monitored: "
        read name
        docker logs -f $name
      ```
- The docker logs display what is going on in the current container. This can be used to see what a participant is doing in real time. The `-f` flag is used to get the logs in real time.

## Entering a users container as an admin to help debugging
### (Requirement 7)

If an administrator wants to enter a container to help with debugging or perform a live demo, it can be done using the command
    ```
    $ docker attach <container_name>
    ```
 . Any changes made by the admin in this container will be visible to the participant.

## Automating deletion of the containers
### (Requirement 8)
- Automate the deletion using the `delete_containers.sh` script described below.

    - `delete_containers.sh`
        ```sh
        echo -n "Enter 'all' to delete all user containers or enter any other key to delete a specific user container: "
        read typ
        if [ "$typ" == "all" ]
        then
            echo -n "Enter the user list file: "
            read file
            while read user
                do
                    docker rm $user
                done < $file
        else
            echo -n "Enter the username: "
            read name
            docker rm $name
        fi
        ```
- This gives us two options:
    - Delete all containers present in the user file.
    - Delete a particular users container.
- This gives us an easy way to delete containers created.

**Note:** To run any shell script in the terminal use the following command:
```
sh <shell script>
```
or
```
bash <shell script>
```
