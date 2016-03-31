# get_isis.py

SSH to Juniper, Cisco IOS, and Cisco IOS-XR routers
and get a list of interfaces with active ISIS adjacencies.
Display these interfaces in a table with the currently
reported interface traffic and IPv4 ISIS metric.

The default behavior is currently to run with no arguments
and prompt for the following:
  Hostname/IP, Device OS, Username, Password 

Also, get_isis can be used to connect via local SOCK proxy:
    Uncomment line 45 in modules.py
    Create a file in your ~/.ssh/proxy.config as follows:
      echo 'ProxyCommand proxy_cheater %h' > ~/.ssh/proxy.config
    Create a bash script to fix the IP/port for netcat:
      #!/bin/bash
      ADDR="$1"
      HOST=${ADDR%:*}  # get the part before the colon
      PORT=${ADDR##*:}  # get the part after the colon
      /bin/nc -x 127.0.0.1:1080 $HOST $PORT
    (Make it executable and in your PATH)
    Start a local SOCKS proxy connection on 127.0.0.1:1080


Sample Output:
################################

laptop:~$ python get_isis.py 
Hostname/IP:10.0.10.3
Device OS: junos
Username:admin
Password: 
+------------+-------+-------+----------+--------+---------------+
| Interface  |   In  |  Out  | Neighbor | Metric | Total_Traffic |
+------------+-------+-------+----------+--------+---------------+
| ge-0/0/2.0 | 480 b | 1.0 K |  vSRX2   | 10/10  |     -1520     |
| ge-0/0/3.0 | 384 b | 488 b |  vSRX4   | 10/10  |      -872     |
|   ae0.0    | 272 b | 488 b |  vSRX6   | 10/10  |      -760     |
+------------+-------+-------+----------+--------+---------------+
