# Wireguard Docker Project Documentation
<font color="gold"> Elizabeth Christensen</font>
<font color="white">
Create a digitalocean account  
Create a 20.04 ubuntu droplet with the following specifications:  
- Choose cheapest droplet = $4/month  
- Basic  
- Regular Intel CPU  
- Set a password  
- Choose a data center  
- Choose what optional addons you want  
Open a terminal inside of the droplet and run the following commands to install Wireguard:  
mkdir -p ~/wireguard/ 
mkdir -p ~/wireguard/config/ 
nano ~/wireguard/docker-compose.yml 
Once inside of the docker-compose.yml file, paste the following:   
version: '3.8'  
services:  
  wireguard:  
    container_name: wireguard  
    image: linuxserver/wireguard  
    environment:  
      - PUID=1000  
      - PGID=1000  
      - TZ=Asia/Hong_Kong  
      - SERVERURL=1.2.3.4  
      - SERVERPORT=51820  
      - PEERS=pc1,pc2,phone1  
      - PEERDNS=auto  
      - INTERNAL_SUBNET=10.0.0.0  
    ports:  
      - 51820:51820/udp  
    volumes:  
      - type: bind  
        source: ./config/  
        target: /config/  
      - type: bind  
        source: /lib/modules  
        target: /lib/modules  
    restart: always  
    cap_add:  
      - NET_ADMIN  
      - SYS_MODULE  
    sysctls:  
      - net.ipv4.conf.all.src_valid_mark=1  
Change TZ to your timezone (example: America/Chicago)  
Change SERVERURL to the IP of the droplet  
run cd ~/wireguard/  
run docker-compose up -d  
which prompts you to install docker with: apt install docker-compose  
run docker-compose up -d again  

# Connecting your phone to Wireguard  
Download the Wireguard app  
run docker-compose logs -f Wireguard  
Click +, then 'add from QR code'  
Scan the QR code populated in the Droplet's terminal  
Click allow when prompted  
Hit the switch to turn the tunnel on and voila  

# Connecting your PC to the Wireguard  
Need to copy over the conf file onto your own computer from the droplet  
In a terminal on your own PC, run:  
scp -P ssh root@(your droplet ip here):~/wireguard/config/peer_pc2/peer_pc2.conf C:\Users\Owner\Downloads  
Install Wireguard on your PC:  
https://www.wireguard.com/install/  
Select add a new tunnel and select the .conf file  
Viola  

</font>
