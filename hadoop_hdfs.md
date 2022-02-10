# **HDFS Hands on (Lab)**

Training video: https://www.youtube.com/watch?v=pfxbNYbnb0c 



## Pre-req installations: 
Docker on windows https://www.youtube.com/watch?v=_9AWYlt86B8

Docker on mac https://hub.docker.com/editions/community/docker-ce-desktop-mac

## Setup your Hadoop docker: (HOST MACHINE)

`$ mkdir hadoop_docker`

`$ cd hadoop_docker`

`$ git clone https://github.com/m-semnani/bd-infra.git`

`$ cd bd-infra`

## Download the images:

`$ docker-compose up -d`

## Verify installation and running images: 

`$ docker ps`

![](/images/hadoop-images/image1.png)

1. Verify Namenode: http://localhost:50070/ (if you are running docker in your local machine).


2. Login to the namenode container
`$ docker exec -it <docker_namenode_name> bash`

    You will see the prompt change to something like below:
    ```
    root@25e58cd35002:/#
    ```

3. Check if hdfs is running fine:

  ```    
  root@66c0ce4e1cd6:/# hdfs -version
  openjdk version "1.8.0_131"
  OpenJDK Runtime Environment (build 1.8.0_131-8u131-b11-1~bpo8+1-b11)
  OpenJDK 64-Bit Server VM (build 25.131-b11, mixed mode)
  ```

4. Type cd / then create a local directory i.e. __<your_name>__: (Docker Local Directory)
  ```
  $ cd /
  $ mkdir <your_name>
  ```

5. Type `pwd`
  ```
  root@66c0ce4e1cd6:/karlo_ison# pwd
  /karlo_ison
  ```

6. Change directory to your_name
  ```
  $ cd <your_name>
  ```

7. Verify file system directories and create HDFS user directory:

    In Hadoop UI, go to utilities > browse the file system: (You should be able to see `tmp` and `user` directories)
    
    ![](/images/hadoop-images/image2.png)

8. Create an HDFS user directory on `user/<your_name>` and recursively change ownership
  ```
  $ hdfs dfs -mkdir /user/your_hdfs_name
  $ hdfs dfs -chown newuser1:hadoop /user/your_hdfs_name
  $ hdfs dfs -chmod 755 /user/your_hdfs_name
  ```
---

## Start the hands-on exercises:

  ### Exercise 1: 
  *(Please save screenshot of your command and output as jpg/png file named* **exercise_1**)

  - Verify that your username has been created on hdfs /user directory
  
    *Example*
    
    ![](/images/hadoop-images/image3.png)


  ### Exercise 2: 
  *(Please save the screenshot as jpg/png file named* ***exercise_2***)

  - Browse the file system under `user` and you should see your_hdfs_name

    *Example*
    
    ![](/images/hadoop-images/image4.png)

  - On your local directory (your_name) download the sample dataset: 

  Type 
  ```
  curl -O https://raw.githubusercontent.com/karloison/academy/main/hdfs_exercise_bands.txt
  ```

  - Copy the .txt file from local directory to HDFS 
  ```
  $ hdfs dfs -copyFromLocal hdfs_exercise_bands.txt /user/<your_hdfs_name>
  ```

  

  ### Exercise 3: 
  *(Please save the screenshot of the command and output as jpg/png file named* ***exercise_3a***)

  - Confirm the file is copied
  ```
  $ hdfs dfs -ls /user/<your_hdfs_name>
  ```
  *Example*
  
  ![](/images/hadoop-images/image5.png)

  - On Utilities > Browse the file system
  Browse the user directory to your_hdfs_name 
  *(Please save the screenshot as jpg/png file named* ***exercise_3b***)

  *Example*

  ![](/images/hadoop-images/image6.png)

  
  - Remove the sample dataset from your local docker directory, making sure you’re in your created directory:

    `$ pwd`
    
    Output should be like below:

    ![](/images/hadoop-images/image7.png)
    
    On your local Docker machine created directory __/<your_name>__
    
    `$ rm -rf hdfs_exercise_bands.txt`

<br>


### HDFS COMMAND REFERENCE: https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/FileSystemShell.html


## Preview / Move files Quiz:

*Note: Using your Shell commands knowledge, you can pipe multiple HDFS commands by using* `|`


### Exercise 4a: 

  - Append to the hdfs://namenode the first 20 lines of output from the sample dataset that will be saved to `/user/<your_hdfs_name>/first_20.out`

    If you see a warning `cat: Unable to write to output stream.`
    It’s ok, verify on the Hadoop UI that the file is created under your user, and check the output by previewing the file using hdfs commands.


### Exercise 4b:

  - Append to the hdfs://namenode the last 10 lines of output from the sample dataset that will be saved to `/user/<your_hdfs_name>/last_10.out`

    If you see a warning `cat: Unable to write to output stream.`
    It’s ok, verify on the Hadoop UI that the file is created under your user, and check the output by previewing the file using hdfs commands.


### Exercise 5: from the command reference above

  - Check the size of files and directories contained in the HDFS /user directory and append to a file named `file_size.out` that will be saved to `/user/<your_hdfs_name>/file_size.out` in the hdfs://namenode/

  - Output the free space of HDFS filesystem on a file named `free_hdfs.out` that will be saved to `/user/<your_hdfs_name>/free_hdfs.out` in the hdfs://namenode/

### Exercise 6: 

  *Copy from HDFS to Local*

  - Copy all the files on your HDFS directory `/user/<your_hdfs_name/` to your local machine (local directory created `your_name`).

  - Confirm that all the files has been copied to your local machine:
  ```
  $ cd /
  $ cd <your_name>
  $ ls 
  $ cd <your_hdfs_name>
  $ ls -ltr (you should see all the files from your hdfs directory copied to your_name/your_hdfs_name)
  ```

### Exercise 7:

  Save/Copy all the files into the __hadoop_docker__ directory created earlier on `Setup your Hadoop docker` step:

   Sample: Host machine directory
   ```
   kison@karloison hadoop_docker $ pwd
   /Users/kison/hadoop_docker
   ```
    
  - Execute Docker command: `docker cp <container_name:/source/dir /destination_dir`
  ```
  $ docker cp 66c0ce4e1cd6_namenode:/karlo_ison/karlo_ison /Users/kison/hadoop_docker 
  ```


# Note: Upload all saved files to Google drive
