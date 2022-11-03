# Factorio-Docker-Setup
A basic and lazy set of instructions for setting up a factorio server with docker. 
I'm running my server on a VM with debian and portainer. Portainer is not necessary and this guide should work on any linux distro with docker installed. 
This is mostly following the instructions included with the docker container, however this is just how I set up my server. 

The docker container used is located [here](https://hub.docker.com/r/dtandersen/factorio/). More details about configuring this container can be found there.

## Setting up the environment
The factorio docker is usually setup in the /opt directory. 
1. ```sudo mkdir -p /opt/factorio```
2. ```sudo chown 845:845 /opt/factorio```
3. Launch the docker container with one of the following steps. 

**For a new world.**

```
sudo docker run -d \
  -p 34197:34197/udp \
  -p 27015:27015/tcp \
  -v /opt/factorio:/factorio \
  --name factorio \
  --restart=always \
  factoriotools/factorio
  ```

**For an existing world.**

Place your save.zip into the ```factorio/saves``` directory. Run ```sudo chown -R 845:845 /opt/factorio``` to make sure that the save file has the same permissions as the rest of the environment. Replace the ```SAVE_NAME=<replaceme>``` with the name of your save without the .zip extension. 
```
sudo docker run -d \
  -p 34197:34197/udp \
  -p 27015:27015/tcp \
  -v /opt/factorio:/factorio \
  -e LOAD_LATEST_SAVE=false \
  -e SAVE_NAME=<replaceme> \
  --name factorio \
  --restart=always \
  factoriotools/factorio
```

If you don't need RCON then remove "-p 27015:27015/tcp \"

## Configuring the server
1. After your docker container launches, edit ```factorio/config/server-config.json```. (note: make sure you have read/write permissions, or just edit the config with sudo)
  
    a. The config file is pretty self-explainitory. I placed my username into the ```"username": "<Username Here>"``` and my password into ```"password": "<Password here>"``` You should be able to use a token instead of your password, which is more secure. However, I was unable to get this working. 

    b. I edited the name, description, tags, game_password, and the autosave_interval.
2. Be sure to port forward 34197 to the IP of your server in your router config. This is required if you want people to be able to join outside your LAN. You can also port forward 27015 if you need to access RCON outside of your LAN. 

3. (Optional) You can install ```UFW``` (a simple firewall) and ```fail2ban``` which will deny repeated failed login attempts. This will help secure your server from outside attackers. 

  ## Enjoy!
  I'll update this README as I learn more about installing the server. Please contact me if you have any suggestions for changes/additions to this document. 
  
Important Note: There are many more things you may want to consider doing to secure your server. However that is outside the scope of this Factorio Server installation guide. 

Resources Used: 

  1. https://hub.docker.com/r/dtandersen/factorio/ (source of the docker container)
  2. https://gist.github.com/othyn/e1287fd937c1e267cdbcef07227ed48c (a more detailed and probably better guide) 
