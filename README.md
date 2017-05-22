#
# Recommended minimum configuration:
#
acl manager proto cache_object
acl localhost src 127.0.0.1/32 ::1
acl to_localhost dst 127.0.0.0/8 0.0.0.0/32 ::1

# Example rule allowing access from your local networks.
# Adapt to list your (internal) IP networks from where browsing
# should be allowed
acl localnet src 10.0.0.0/8	# RFC1918 possible internal network
acl localnet src 172.16.0.0/12	# RFC1918 possible internal network
acl localnet src 192.168.0.0/16	# RFC1918 possible internal network
acl localnet src fc00::/7       # RFC 4193 local private network range
acl localnet src fe80::/10      # RFC 4291 link-local (directly plugged) machines

acl SSL_ports port 443
acl Safe_ports port 80		# http
acl Safe_ports port 21		# ftp
acl Safe_ports port 443		# https
acl Safe_ports port 70		# gopher
acl Safe_ports port 210		# wais
acl Safe_ports port 1025-65535	# unregistered ports
acl Safe_ports port 280		# http-mgmt
acl Safe_ports port 488		# gss-http
acl Safe_ports port 591		# filemaker
acl Safe_ports port 777		# multiling http
acl CONNECT method CONNECT

#
# Recommended minimum Access Permission configuration:
#
# Only allow cachemgr access from localhost
http_access allow manager localhost
http_access deny manager

# Deny requests to certain unsafe ports
http_access allow !Safe_ports

# Deny CONNECT to other than secure SSL ports
http_access allow CONNECT !SSL_ports

# We strongly recommend the following be uncommented to protect innocent
# web applications running on the proxy server who think the only
# one who can access services on "localhost" is a local user
#http_access deny to_localhost

#
# INSERT YOUR OWN RULE(S) HERE TO ALLOW ACCESS FROM YOUR CLIENTS
#

# Example rule allowing access from your local networks.
# Adapt localnet in the ACL section to list your (internal) IP networks
# from where browsing should be allowed
http_access allow localnet
http_access allow localhost

# And finally deny all other access to this proxy
http_access allow all

# Squid normally listens to port 3128
http_port 2504


##--------------------------------
## declare an acl that is true for all ipv6 destinations
acl to_ipv6 dst ipv6
http_access allow to_ipv6 !all

##tell Squid to listen on sequential ports and to designate a name for each inbound     connection. 
http_port 172.110.30.227:2504 name=2504
http_port 172.110.30.227:2505 name=2505
http_port 172.110.30.227:2506 name=2506
http_port 172.110.30.227:2507 name=2507
http_port 172.110.30.227:2508 name=2508
http_port 172.110.30.227:2509 name=2509
http_port 172.110.30.227:2510 name=2510
http_port 172.110.30.227:2511 name=2511
http_port 172.110.30.227:2512 name=2512
http_port 172.110.30.227:2513 name=2513

## designate acl based on inbound connection name
acl user1 myportname 2504
acl user2 myportname 2505 
acl user3 myportname 2506 
acl user4 myportname 2507 
acl user5 myportname 2508 
acl user6 myportname 2509
acl user7 myportname 2510
acl user8 myportname 2511
acl user9 myportname 2512
acl user10 myportname 2513

## define outgoing IPv6 per user
tcp_outgoing_address 2604:880:a:6::af user1
http_access allow user1
tcp_outgoing_address 2604:880:a:6::ae user2
http_access allow user2
tcp_outgoing_address 2604:880:a:7::e3b user3
http_access allow user3
tcp_outgoing_address 2604:880:a:5::bb9 user4
http_access allow user4
tcp_outgoing_address 2604:880:a:6::525 user5
http_access allow user5
tcp_outgoing_address 2604:880:a:6::d54 user6
http_access allow user6
tcp_outgoing_address 2604:880:a:2::13 user7
http_access allow user7
tcp_outgoing_address 2604:880:a:6::186 user8
http_access allow user8
tcp_outgoing_address 2604:880:a:6::187 user9
http_access allow user9
tcp_outgoing_address 2604:880:a:6::7d7 user10
http_access allow user10

##this last IPv6 always gets used for all outbound connections, even if commented out

##----------------------------------------------


# We recommend you to use at least the following line.
hierarchy_stoplist cgi-bin ?

# Uncomment and adjust the following to add a disk cache directory.
#cache_dir ufs /var/spool/squid 100 16 256

# Leave coredumps in the first cache dir
coredump_dir /var/spool/squid

# Add any of your own refresh_pattern entries above these.
refresh_pattern ^ftp:		1440	20%	10080
refresh_pattern ^gopher:	1440	0%	1440
refresh_pattern -i (/cgi-bin/|\?) 0	0%	0
refresh_pattern .		0	20%	4320

via off
forwarded_for off

request_header_access Allow allow all
request_header_access Authorization allow all
request_header_access WWW-Authenticate allow all
request_header_access Proxy-Authorization allow all
request_header_access Proxy-Authenticate allow all
request_header_access Cache-Control allow all
request_header_access Content-Encoding allow all
request_header_access Content-Length allow all
request_header_access Content-Type allow all
request_header_access Date allow all
request_header_access Expires allow all
request_header_access Host allow all
request_header_access If-Modified-Since allow all
request_header_access Last-Modified allow all
request_header_access Location allow all
request_header_access Pragma allow all
request_header_access Accept allow all
request_header_access Accept-Charset allow all
request_header_access Accept-Encoding allow all
request_header_access Accept-Language allow all
request_header_access Content-Language allow all
request_header_access Mime-Version allow all
request_header_access Retry-After allow all
request_header_access Title allow all
request_header_access Connection allow all
request_header_access Proxy-Connection allow all
request_header_access User-Agent allow all
request_header_access Cookie allow all
#request_header_access All deny all
