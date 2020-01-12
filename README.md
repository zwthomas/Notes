# Nest Brain Setup
## [Docker on Linux](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
Start docker on boot:
```
systemctl enable docker
```
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
