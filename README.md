<p align="center">
    <img src="https://github.com/Boilrig/TorrentDog/blob/master/torrentdog_logo.png"/></a>
</p>

# TorrentDog

TorrentDog is a smart, loyal and secure docker cluster that routes all torrent through Wireguard via NordVPN while providing a user interferace for Transmission, Radarr and Sonarr.

*It's like having a loyal friend who fetches all your mail (media) for you but growls at the nosey mailman (governments/corporations) when he steps on your lawn.*


## How to use TorrentDog

TorrentDog is a docker cluster bringing together multiple services in one place and using either the step by step commandline setup below, or our docker-compose further down, ensures you use the correct settings as well as handle your immediate filestructure for quick setup.

This guide will specifically state what code needs to be changed in the docker commandlines, touching other settings will potentially invalidate the security practices TorrentDog recommends.

### First Steps - NordVPN

###### Note: NordVPN has been chosen for both their P2P speed and their WireGuard implementation, allowing you to maximise performance with no loss of security.


Details that need to be changed.

NordVPN Username
```
USER=user@email.com
```

NordVPN Password (Keep within the quotes to provide special character issues)
```
PASS='pas$word'
```

Server and Type
Use ```docker run --rm bubuntux/nordvpn sh -c "nordvpnd & sleep 1 && nordvpn countries"``` to grab the list of countries. Keeping ```-g p2p``` asks for P2P specific servers. Below, we are asking to connect to a United States P2P server.
```
CONNECT=us -g p2p
```
We recommend that you check your countries servers to ensure that there are available P2P servers,  check ```docker run --rm bubuntux/nordvpn sh -c "nordvpnd & sleep 1 && nordvpn cities [country]``` for specific cities within countries, remembering to keep ```-g p2p```.
```
docker run -ti --cap-add=NET_ADMIN --cap-add=SYS_MODULE --device /dev/net/tun --name vpn \
            --sysctl net.ipv4.conf.all.rp_filter=2 \
            -e USER=user@email.com -e PASS='pas$word' \
            -e CONNECT=us -g p2p -e TECHNOLOGY=NordLynx -d bubuntux/nordvpn
```
