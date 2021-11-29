# brendans1818.github.io
# DOCKER INSTALL
**PreDocker Install: (commands to install docker on Ubuntu via the powerpoint)**
  - sudo apt update
  - sudo apt install ca-certificates curl gnupg lsb-release
  - curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
  - echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  - sudo apt update
  - sudo apt install docker-ce docker-ce-cli containerd.io
  - sudo usermod -aG docker bsm2317 
    - This command adds my user to the docker group so I don't need to sudo every time I run a docker command
# OPENVAS/GREENBONE INSTALL
  - https://utulsa.hosted.panopto.com/Panopto/Pages/Embed.aspx?id=75912983-0806-47a5-a3ad-acc9018aaec3
    - The link on panopto is what I followed in order to install openvas
  - sudo apt-get update 
    - make sure everything is up to date
  - sudo apt-get upgrade 
    - install available upgrades of all packages on my machine
  - sudo service docker status 
    - checks to make sure docker is running - which it is
  - sudo docker run -d -p 443:443 --name openvas mikesplain/openvas 
    - installs the container
  - After this open a web browser (chose firefox), go to https://localhost/
  - Will ask about the security risk, go to advance accept risk and continue
  - Log into greenbone using user and password
  - Create a new target (configurations → target)
  - Set up the name, and the IP then create it
  - After creating a target create a scan (Scan → tasks)
  - Change the name, leave everything else default, then click create
  - After creating a scan, click start. Refresh the page a few times to make sure it is running properly. It will show any vulnerabilities it finds. 
  - Side note: if you want to delete and reinstall the docker: 
    - sudo docker stop openvas
      - Stops the docker
  - sudo docker rm openvas 
    - Removes the docker. This allows you to reinstall it if you wish.
# DOCKER COMPOSE
version: '3.3'  
services:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;openvas:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ports:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- '443:443'  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;container_name: openvas  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;image: mikesplain/openvas     
# SCREENSHOTS
[![Screenshot-2021-11-14-135401.png](https://i.postimg.cc/x1b7CFc4/Screenshot-2021-11-14-135401.png)](https://postimg.cc/mPBVpj3Y)
[![Screenshot-2021-11-14-135149.png](https://i.postimg.cc/SQ9bjs3r/Screenshot-2021-11-14-135149.png)](https://postimg.cc/K4mHHZZk)
