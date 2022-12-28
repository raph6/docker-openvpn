# Reloaded OpenVPN for Docker

## Motivation
This is a fork from the amazing [kylemanna/docker-openvpn](https://github.com/kylemanna/docker-openvpn) repo.

I decided to take over the project when I realized that the Docker hub image was no longer up to date and was therefore using an outdated version of OpenSSL

You can now use this to pull the image: 

`docker pull raph6/openvpn`

## Easy Usage
This using Cloudflare DNS servers : `1.1.1.1` and `1.0.0.1`, you can change it if you want

Dont forget to replace `/path/to/ovpn-data` and `CLIENTNAME` with your own values

Generate configuration:
```sh
docker run -v /path/to/ovpn-data:/etc/openvpn --net=none --rm raph6/openvpn ovpn_genconfig -n '1.1.1.1' -n '1.0.0.1' -C 'AES-256-CBC' -a 'SHA512' -u udp://51.83.42.97:1195
```

Generate certificates:
```sh
docker run -e EASYRSA_KEY_SIZE=4096 -v /path/to/ovpn-data:/etc/openvpn --rm -it raph6/openvpn ovpn_initpki
```

Start OpenVPN server:
```sh
docker run -v /path/to/ovpn-data:/etc/openvpn -d -p 1195:1194/udp --cap-add=NET_ADMIN --restart=always --name=openvpn-cloudflare raph6/openvpn
```

Generate a client certificate:
```sh
docker run -e EASYRSA_KEY_SIZE=4096 -v /path/to/ovpn-data:/etc/openvpn --rm -it raph6/openvpn easyrsa build-client-full CLIENTNAME nopass
```

Retrieve the client configuration with embedded certificates:
```sh
docker run -v /path/to/ovpn-data:/etc/openvpn --rm raph6/openvpn ovpn_getclient CLIENTNAME > CLIENTNAME.ovpn
```

Import your .ovpn file in your OpenVPN client and enjoy !

## Advanced Usage
You can find the complete documentation on: [kylemanna/docker-openvpn](https://github.com/kylemanna/docker-openvpn)
