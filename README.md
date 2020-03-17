# Nest Brain Setup
## [Docker on Linux](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
Start docker on boot:
```
systemctl enable docker
```

## [Docker Compose](https://docs.docker.com/compose/install/)
## [Hassio Docker](https://www.home-assistant.io/docs/installation/docker/)
[Control TCL](https://www.reddit.com/r/homeassistant/comments/8tl2pg/turn_roku_tv_onoff/):
```
switch:
  - platform: command_line
    switches:
      #toggles office Roku TV
      office_tv_power:
        command_on: 'curl -X POST http://192.168.1.240:8060/keypress/PowerOn'
        command_off: 'curl -X POST http://192.168.1.240:8060/keypress/PowerOff'
        command_state: 'curl -s GET http://192.168.1.240:8060/query/device-info | grep power-mode | sed -e "s:<power-mode>DisplayOff</power-mode>:OFF:" -e "s:<power-mode>PowerOn</power-mode>:ON:"'
        value_template: '{{ value == "ON" }}'
```
## [OpenVPN-as](https://hub.docker.com/r/linuxserver/openvpn-as/)
Make sure a port is forwarded on the router to the ip of the machine this container is running on
```
docker create \
  --name=openvpn-as \
  --cap-add=NET_ADMIN \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=America/Chicago \
  -e INTERFACE=eth0  \
  -p 943:943 \
  -p 9443:9443 \
  -p 1194:1194/udp \
  -v ~/docker/openvpn-as:/config \
  --restart unless-stopped \
  linuxserver/openvpn-as
```

https://DOCKER-HOST-IP:943/admin

## Portainer
```
docker volume create portainer_data
docker run -d -p 9000:9000 \
--name portainer \
--restart always \
-v /var/run/docker.sock:/var/run/docker.sock \
-v portainer_data:/data portainer/portainer
```
192.168.XX.XX:9000
## [Plex](https://linuxize.com/post/how-to-install-plex-media-server-on-ubuntu-18-04/)
Mounting NAS
You are mounting the CIFS share as root (because you used sudo), so you cannot write as normal user. If your Linux Distribution and its kernel are recent enough that you could mount the network share as a normal user (but under a folder that the user own), you will have the proper credentials to write file (e.g. mount the shared folder somewhere under your home directory, like for instance $HOME/netshare/. Obviously, you would need to create the folder before mounting it).

An alternative is to specify the user and group ID that the mounted network share should used, this would allow that particular user and potentially group to write to the share. Add the following options to your mount: uid=<user>,gid=<group> and replace <user> and <group> respectively by your own user and default group, which you can find automatically with the id command.

`sudo mount -t cifs -o username=${USER},password=${PASSWORD},uid=$(id -u),gid=$(id -g) //server-address/folder /mount/path/on/ubuntu`
If the server is sending ownership information, you may need to add the forceuid and forcegid options.

`sudo mount -t cifs -o username=${USER},password=${PASSWORD},uid=$(id -u),gid=$(id -g),forceuid,forcegid, //server-address/folder /mount/path/on/ubuntu`
https://unix.stackexchange.com/questions/68079/mount-cifs-network-drive-write-permissions-and-chown#68081
# Pihole
https://blog.cryptoaustralia.org.au/instructions-for-setting-up-pi-hole/

[DNS over HTTPS](https://scotthelme.co.uk/securing-dns-across-all-of-my-devices-with-pihole-dns-over-https-1-1-1-1/)

# Proxmox

## Installing Windows 10
If you would like to use RDP to connect, you must install the Pro version of Windows 10.[Enable RDP](https://pureinfotech.com/enable-remote-desktop-windows-10/) after install

[virtio drivers](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/archive-virtio/)

VirtioIo driver install in needed to install: Balloon, NetKVM, viostor

https://pve.proxmox.com/wiki/Windows_10_guest_best_practices

https://youtu.be/vUbKj2En0aE

https://youtu.be/XPCGNAVu2Ec

## GPU passthrough not tested
https://www.reddit.com/r/homelab/comments/b5xpua/the_ultimate_beginners_guide_to_gpu_passthrough/

## Spice

[Client Download](https://virt-manager.org/download/) ([Alternative](https://releases.pagure.org/virt-viewer/)) After installed search for **Remote Viewer** on windows

https://github.com/nodesource/distributions/blob/master/README.md



```
sudo apt install xrdp
sudo systemctl enable xrdp
```


# Useful: 
https://sqlitebrowser.org/


### Git in terminal
```
git_branch() {
  git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}

export PS1="[\u@\h \W]\$(git_branch)\$ "


    PS1="${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00;36m\] \$(git_branch)\[\033[00m\]\$ "

```
