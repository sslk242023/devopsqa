---
title: "Installing Jenkins Guide"
date: 2019-05-12T1:17:21-07:00
layout: 'posts'
tags: ["Jenkins"]
---

## Installing Jenkins Guide
#

This guide uses Ubuntu 18.04, Java 8

Note: I have included a script to complete all the tasks below. Please change any usernames and passwords yourself. I have marked the lines that need usernames and passwords changed for security with "# Please change this/these values: "

Note: I am using Vagrant to create a VM so my user is vagrant and my home is located in /home/vagrant. If yours is different, make sure you update the script below to your environment.

    setup-jenkins.sh
    ```
    #!/bin/bash
    sudo touch /home/vagrant/apt-output.txt && sudo chmod 777 apt-output.txt
    sudo apt-get -y update &>> /home/vagrant/apt-output.txt
    sudo apt-get -y upgrade &>> /home/vagrant/apt-output.txt
    sudo apt-get install -y openjdk-8-jdk openjdk-8-jdk-headless openjdk-8-jre openjdk-8-jre-headless &>> /home/vagrant/apt-output.txt
    ```

### Installing Java
1. Run the following command to download both jdk and jre.
    ```
    sudo apt-get install -y openjdk-8-jdk openjdk-8-jdk-headless openjdk-8-jre openjdk-8-jre-headless
    ```

2. Set JAVA_HOME by running the following command:
    ```
    sudo cp /etc/environment /etc/environment.orig
    echo "JAVA_HOME='/usr/lib/jvm/java-8-openjdk-amd64'" | sudo tee -a /etc/environment
    source /etc/environment
    ```

### Installing WebDrivers and Selenium
Note: You can find where a package has been installed to by running the following command:
    ```
    dpkg -L packagename
    ```

1. Run the following command to download chromedriver:
    ```
    sudo apt-get -y install chromium-chromedriver
    ```
    Note: Installs to /usr/lib/chromium-browser/
2. Run the following command to download firefoxdriver:
    ```
    sudo apt-get -y install firefoxdriver
    ```
    Note: Installs to /usr/lib/firefoxdriver
3. Run the following commands to download and install geckodriver:
    ```
    wget https://github.com/mozilla/geckodriver/releases/download/v0.24.0/geckodriver-v0.24.0-linux64.tar.gz
    tar -xvzf geckodriver*
    chmod +x geckodriver
    mv geckodriver /usr/lib/geckodriver
    rm gecko*
    ```
4. Put the drivers in the Path
    ```
    echo "PATH='$PATH:/usr/lib/chromium-browser/:/usr/lib/firefoxdriver:/usr/lib/geckodriver'" | sudo tee -a /etc/environment
    source /etc/environment
    ```
    
### Installing Jenkins
1. Download Jenkins:
    ```
    wget -O jenkins.deb https://pkg.jenkins.io/debian-stable/binary/jenkins_2.164.3_all.deb
    ```
2. Install Jenkins:
    ```
    sudo dpkg -i jenkins.deb
    sudo apt-get install -fy
    ```
3. Get the IP address of the server:
    ```
    ip addr | grep "inet 192"
    ```
4. Open a web browser and go to the ipaddress:8080
   
5. Run the following command in order to unlock Jenkins.
    ```
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```

### Configuring Jenkins
1. After unlocking Jenkins, select "Install suggested plugins". Then wait as Jenkins gets started.
2. Create the first Admin User by giving a username, password, full name, and email address.
3. Give the URL where people can expect to find the Jenkins server.
4. Click the Jenkins dropdown > Manage Jenkins > Global Tool Configuration.
5. Click the Add JDK button then point to the location of the JDK. Then click save.
    ```
    Name: java-8-openjdk-amd64
    JAVA_HOME: /usr/lib/jvm/java-8-openjdk-amd64
    ```
6. 