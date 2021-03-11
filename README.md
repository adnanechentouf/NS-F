# NS-F
Network Security &amp; Firewall Repository

As a system engineer your expertise is asked to create a firewall ruleset for a hosting server.

The server is provided with the following services: Apache, ProFTPd and bind9. Please, do not allow zonetransfers (think twice). Also protect the server against ping flooding. The server is not allowed to make outgoing connections, except for the installation of security updates.

Deny pingflood -> Allow ICMP
iptables -t filter -A INPUT -p icmp --icmp-type echo-request -m limit --limit 10/minute -j ACCEPT
iptables -t filter -A INPUT -p icmp -j DROP
iptables -t filter -A  OUTPUT -p icmp --icmp-type echo-reply -j ACCEPT

No Zonetransfers
iptables -A INPUT -p tcp -m state --state NEW -m tcp --dport 53 -j ACCEPT
iptables -A INPUT -p udp -m state --state NEW -m udp --dport 53 -j DROP

Updates possible
iptables -A OUTPUT -d security.debian.org --dport 80 -j ACCEPT
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -P OUTPUT DROP

service iptables save
