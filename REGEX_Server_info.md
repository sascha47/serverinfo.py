# serverinfo.py
# REGEX used for server info

#!/usr/bin/python3
#!/bin/bash/python3


import subprocess
import re

hostname = subprocess.run("hostname",capture_output=True,text=True)
print(hostname.stdout.strip())

# cpu
cpu = subprocess.run("lscpu",capture_output=True,text=True)

match = re.search('CPU\(s\)\:\s*(\d)',cpu.stdout)
if match:
    cpus = match.group(1)
    print("cpu "+cpus)

# RAM
ram = subprocess.run("free -h",capture_output=True,text=True,shell=True)
match = re.search("(Mem\:)\s*(\d\S+\d)",ram.stdout)

if match:
    ram_size = match.group(2)
    print("Ram " + ram_size.strip())

#OS TYPE
os = subprocess.run("uname -a",capture_output=True,text=True,shell=True)
match = re.search("^(\D)\w+",os.stdout)

if match:
    os_type = match.group(0)
    print("OS " + os_type)


#OS Version

version = subprocess.run("uname -a",capture_output=True,text=True,shell=True)
match = re.search("~(\S+)\\b",version.stdout)

if match:
    ver = match.group(1)
    print("Version "+ver)

# DisKs

disk =subprocess.run("lsblk --nodeps -d",capture_output=True,text=True,shell=True)

match = re.search("xvda\\b.+(\\d)\\w",disk.stdout)

if match:
    disks = match.group(1)
    print("Disks " + disks)


#ip

ip = subprocess.run("ifconfig",capture_output=True,text=True,shell=True)
match = re.search("inet (\\d{1,3}\\.\\d{1,3}\\.\\d{1,3}\\.\\d{1,3})",ip.stdout)

if match:
    ip_addr = match.group(1)
    print("IP " + ip_addr)

#mac

mac = subprocess.run("ifconfig",capture_output=True,text=True,shell=True)
match = re.search("ether (\w{1,2}\:\w{1,2}\:\w{1,2}\:\d{1,2}\:\d{1,2}\:\d{1,2})",mac.stdout)

if match:
    macc = match.group(1)
    print("mac " + macc)
