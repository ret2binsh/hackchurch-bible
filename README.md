# Hack Church Bible

Hack Church is a group dedicated to making a comprehensive penetration testing methodology available to everyone. If you would like to contribute, please submit a PR or report an Issue. 

## How to use this guide

### OS
This guide assumes you are using a pen-testing distro of linux. Many open-source distros exist and come with a common set of tools. 

### CLI Input/Output
The `preformatted text` refers to command line input/output. Some text `{{REPLACE_ME}}` (all caps, snake_cased, in between double curly braces), should be replaced with relevant info. Sometimes a command will have `# This is a comment` afterward to further provide context for a command. 

## Methodology
0. Setup
1. Recon
2. Access
3. PrivEsc
4. Cover

## 0. Setup
Organization is king. Before you even begin recon, organizing your local environment will aid in an efficent penetration test. 

### Directroy structure
Here is a recommended directory structure to work out of:
```
 ~/                           # Your home directory
 └── {{EVENT}}/               # Name of the event
     └── {{TARGET_MACHINE}}/  # Name or IP of the box
         ├── nmap             # Reports from nmap tool
         ├── www              # Staging files to serve over http
```

### Tools
* [nmap][1] - pre/post exploit recon
* [linenum][2] - post exploit recon
* [CyberChef][3] - encryption/encoding/compression swiss army knife

## 1. Recon
Use `nmap` to output a report of open ports and what service versions are running on them

```
nmap -SC -sV -oA nmap/{{FILENAME}} {{TARGET_MACHINE}}
```
* `FILENAME`: Name of the output file (name of the box)
* `TARGET_MACHINE`: Target machine

## 2. Access
Use the sections below for common ways to gain more info or access via the available ports.

#### 21 FTP
With credentials:
```
ftp {{USERNAME}}@{{TARGET_MACHINE}} 
```

#### 22 SSH
With credentials:
```
ssh {{USERNAME}}@{{TARGET_MACHINE}} 
```

#### 22 SSH Tunnels

SSH Forward Tunnel with a Local Listener:
```
ssh -L {{LOCAL_PORT}}:{{TARGET_MACHINE2}}:{{REMOTE_PORT}} {{USERNAME}}@{{TARGET_MACHINE1}}
ex. ssh -L 2000:target2:22 backdoor@target1
```

SSH Reverse Tunnel with a Remote Listener:
```
ssh -R {{REMOTE_PORT}}:127.0.0.1:{{LOCAL_PORT}} {{USERNAME}}@{{TARGET_MACHINE1}}
ex. ssh -R 3000:127.0.0.1:22 backdoor@target1
```

Two SSH Forward Tunnels with a Local Listener utilizing a single TCP connection to TARGET1:
```
ssh -L {{LOCAL_PORT1}}:{{TARGET_MACHINE2}}:{{REMOTE_PORT1}} -L {{LOCAL_PORT2}}:{{TARGET_MACHINE2}}:{{REMOTE_PORT2}} {{USERNAME}}@{{TARGET_MACHINE1}}
ex. ssh -L 2000:target2:22 -L 2001:target2:80 backdoor@target1
```

SSH Forward & Reverse Tunnels with Local & Reverse Listeners utilizing a single TCP connection to TARGET1:
```
ssh -L {{LOCAL_PORT1}}:{{TARGET_MACHINE2}}:{{REMOTE_PORT1}} -R {{REMOTE_PORT1}}:127.0.0.1:{{LOCAL_PORT}} {{USERNAME}}@{{TARGET_MACHINE1}}
ex. ssh -L 2000:target2:22 -R 3000:127.0.0.1:22 backdoor@target1
```

Tunnelception - Tunnel to Target 3 through a Tunnel to Target 2:
```
ssh -L {{LOCAL_PORT1}}:{{TARGET_MACHINE2}}:{{REMOTE_PORT1}} {{USERNAME}}@{{TARGET_MACHINE1}}
ssh -L {{LOCAL_PORT2}}:{{TARGET_MACHINE3}}:{{REMOTE_PORT1}} -p {{LOCAL_PORT1}} {{USERNAME}}@{{TARGET_MACHINE2}}
ex. ssh -L 2000:target2:22 backdoor@target1
    ssh -L 2001:target3:22 -p2000 backdoor2@target2
```

Add a Forward Tunnel while in an SSH session:
```
~C (must be a fresh new line so hit enter and try again if you don't drop into menu)
-L {{LOCAL_PORT}}:{{TARGET_MACHINE2}}:{{REMOTE_PORT}}
```

#### 80 HTTP
View the website in a browser: `http://{{TARGET_MACHINE}}`

#### 443 HTTPS
View the website in a browser: `https://{{TARGET_MACHINE}}`

#### 5601 Kibana
View the Kibana dashboard in a browser: `{{TARGET_MACHINE}}:5601`
See also [ELK Stack][4]

#### 8080 HTTP (DEV)
See also 80 HTTP

#### 8888 HTTP (DEV)
See also 80 HTTP

#### 9200 ElasticSearch
List available indecies:
```
{{TARGET_MACHINE}}:9200/_cat/indecies?pretty=true
```
See also [ELK Stack][4]

Search an index:
```
{{TARGET_MACHINE}}:9200/{{INDEX}}/_search?q={{KEYWORD}}&size={{#}}&pretty=true
```
* `{{INDEX}}`: The index to search
* `{{KEYWORD}}`: Search all the docs with the query string
* `{{#}}`: Number of results to show (default is 10)

#### 9600 Logstash
By default, Logstash uses port `9600`, but if the port is taken it will use the next port (`9601`, etc.)
See also [ELK Stack][4]

## 3. PrivEsc
Once you gain access to machine, you'll need to execute post-exploit recon to determine vectors of priveledge escalation.

### Locate a file or files
```
locate {{FILENAME}}
locate '*.{{FILETYPE}}`
```

**Note**:If you're looking for a recently created file make sure to run `updatedb` first

### Find files

Find a file across the entire filesystem
```
find / -iname {{FILENAME}}
```

Find all SUID binaries across filesystem
```
find / -perm -4000
```

"Find" all files within a specific directory and GREP for a matching string
```
find /etc -type f -exec grep {{STRING}} {} +
```

## 4. Cover
Typically in CTF environments, you won't need to worry about covering your tracks.

_Coming Soon_

[1]: https://nmap.org/
[2]: https://github.com/rebootuser/LinEnum
[3]: https://github.com/gchq/CyberChef
[4]: https://www.elastic.co/guide/index.html
