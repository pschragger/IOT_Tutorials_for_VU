# Device Management Install Tutorial #

In this tutorial you will setup and test a lwm2m server and client on you raspberry pi.

## Prerequistes #
1. laptop to use as your devlopment platform and terminal access ( ssh needs to be installed and working)
2. Raspberry Pi with dietpi installed and accessible via ssh on wifi or ethernet You can use the tutorial [ RPI boot on wifi ] (../RPI_BOOT_WIFI_tutorial) to complete this prereq
3. Raspberry PI must have access to internet via the wifi ( wifi needs to be configured for wan access )
4. Access to the lwm2m server/client code from the [eclipse leshan project] (https://www.eclipse.org/leshan/) at [ Leshan Github ](https://github.com/eclipse/leshan) 
## Steps ##

### Logon to Raspbery PI ###
1. using
```
ssh dietpi@(IPADDR_OF_PI)
```
2. use the password you setup in the pi configuration

### Install development environment on RPI

1. Install git
   - On command line in dietpi start dietpi-software
    ```
    sudo dietpi-software
    ```
    - search for git ( should be package 17 )
    - press spacebar to select ( asterick [*] should show
    - tab to ok and press enter
    - navigate to install on dietpi-software menu page
    - press enter and it should show that the software 
    ```
    - Git: Clone and manage Git repositories locally
    ```
    - navagate with tabs to <ok> and press enter
    - install should begin using apt scripts
    - it should take less then a minute or two depending on network traffic
    - check that is installed by using the command on the pi
    ```
    git --version
    ```
2. Install java jdk
      - On command line in dietpi start dietpi-software
    ```
    sudo dietpi-software
    ```
    - search for Java JDK ( should be package 8 )
    - press spacebar to select ( asterick [*] should show
    - tab to ok and press enter
    - navigate to install on dietpi-software menu page
    - press enter and it should show that the software 
    ```
    - Java JDK: OpenJDK Development Kit
    ```
    - navagate with tabs to <ok> and press enter
    - install should begin using apt scripts
    - it should take less then 10 minutes to install the openJDK jre and jdk depending on network traffic
    - check that is installed by using the command on the pi
    ```
    java --version
    ```
3. Install maven on the rapberry pi
   1. In the dietpi directory create a download directory
   ```
   mkdir download
   ```
   2. change directory into the download directory
   ```
   cd download
   ```
   3. download the latest maven tarball using wget, the following is an example - be sure to check the latest version on [apache maven site](https://maven.apache.org/download.cgi)  
   ```
   wget https://dlcdn.apache.org/maven/maven-3/3.8.6/binaries/apache-maven-3.8.6-bin.tar.gz
   ```
   4. unpack the tarball
   ```
   tar xzvf apache-maven-3.8.6-bin.tar.gz
   ```
   5. Add the bin directory of the created directory apache-maven-3.8.6 to the PATH environment variable
   6. Confirm installation using
   ```
   mvn -v
   ```
   7. Setup the environment variables to add maven to the paths taken from [maven install instructions](https://maven.apache.org/install.html)
      - add the bin directory to the path
      ```
      echo 'PATH="${PATH}:~/download/apache-maven-3.8.6/bin"' >>  ~/.bashrc
      ```
   8.  test maven installation using
   ```
   bash
   mvn -v
   ```
   results should look like
   ```
   Apache Maven 3.8.6 (84538c9988a25aec085021c365c560670ad80f63)
Maven home: /home/dietpi/download/apache-maven-3.8.6
Java version: 17.0.3, vendor: Debian, runtime: /usr/lib/jvm/java-17-openjdk-arm64
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "5.15.32-v8+", arch: "aarch64", family: "unix"
   ```
   9. create a JAVA_HOME environment variable pointing the install jdk

      - add the JAVA_HOME to the .bashrc (check your java directory matches the path provided below change as required )
      ```
      sudo echo 'JAVA_HOME="/usr/lib/jvm/java-17-openjdk-arm64"' >> ~/.bashrc
      ```
      - check that the install was complete with
      ```
      bash
      echo $JAVA_HOME
      ```
      result should look like
      ```
      /usr/lib/jvm/java-17-openjdk-arm64
      ```

4. Install leshan git and build  - instructions form [leshan git](https://github.com/eclipse/leshan)
   - clone the leshan repo using
   ```
   cd
   mkdir projects
   cd projects
   git clone https://github.com/eclipse/leshan.git
   ```
   cloning the directory should be less than a minute
   - build leshan
     ```
     cd ~/projects/leshan
     mvn clean install
     ```
   - The build may take 20 minutes or longer

### Test the leshan server
1. Start the server using
  ```
  cd ~/projects/leshan
  java -jar leshan-server-demo/target/leshan-server-demo-*-SNAPSHOT-jar-with-dependencies.jar &
  ```
  -  Connect on Leshan demo UI: http://RPI_IPADDR:8080

   My Raspberry PI is using 192.168.8.224 ( from the router admin client page)
   therefore I will use
   ```
   http://192.168.8.224:8080
   ```
   this will bring up te leshan register client page


  - run the leshan client to add it to the page
  ```
  java -jar leshan-client-demo/target/leshan-client-demo-*-SNAPSHOT-jar-with-dependencies.jar
  ```

  The result should show the registration of your DietPI host as a Temperature client endipoint
  This end point should continue to register every minute.
  using ctrl-c to stop the client

  ### check the server page in the leshan server page to see the active url info and public keys and server cert info

###  Continue to the ESP32 lwm2m client build ###
