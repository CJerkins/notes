Setup Azure storage account with all defaults. Then follow the image to get SAS key.

![azure_img.png](../_resources/azure_img.png)

Install "rclone"
- For unix based systems run, `curl https://rclone.org/install.sh | sudo bash`, in terminal.
- For windows based systems goto, `https://rclone.org/downloads/`, to download installation file.

Follow directions from the link; https://rclone.org/azureblob/.
1. `rclone config`


for vpn in fight-server light-server spoon-client gilmore-client; do sudo systemctl stop openvpn@$vpn && sudo systemctl start openvpn@$vpn; done


podman run -d --network dns --restart always \
    --name pihole \
    -e TZ="America/New York" \
    -v "/mnt/data/etc-pihole/:/etc/pihole/" \
    -v "/mnt/data/pihole/etc-dnsmasq.d/:/etc/dnsmasq.d/" \
    --dns=127.0.0.1 \
    --dns=1.1.1.3 \
    --dns=1.0.0.3 \
    --hostname pi.hole \
    -e VIRTUAL_HOST="pi.hole" \
    -e PROXY_LOCATION="pi.hole" \
    -e ServerIP="192.168.2.3" \
    -e IPv6="False" \
    pihole/pihole:latest
	
	podman exec -it pihole pihole -a -p 
	
	
	mkdir -p /mnt/data/ntopng/redis
mkdir -p /mnt/data_ext/ntopng/lib
touch /mnt/data_ext/ntopng/GeoIP.conf
curl -Lo /mnt/data_ext/ntopng/ntopng.conf https://github.com/tusc/ntopng-udm/blob/master/ntopng/ntopng.conf?raw=true
curl -Lo /mnt/data_ext/ntopng/redis.conf https://github.com/tusc/ntopng-udm/blob/master/ntopng/redis.conf?raw=true

podman run -d --net=host --restart always \
   --name ntopng \
   -v /mnt/data_ext/ntopng/GeoIP.conf:/etc/GeoIP.conf \
   -v /mnt/data_ext/ntopng/ntopng.conf:/etc/ntopng/ntopng.conf \
   -v /mnt/data_ext/ntopng/redis.conf:/etc/redis/redis.conf \
   -v /mnt/data_ext/ntopng/lib:/var/lib/ntopng \
   docker.io/tusc/ntopng-udm:latest

	