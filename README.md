# Hack Church Bible

Hack Church is a group dedicated to making a comprehensive penetration testing methodology available to everyone. If you would like to contribute, please submit a PR or report an Issue. 

## How to use this guide
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
 ~/                 # Your home directory
 └── EVENT/         # Name of the event
     └── BOX/       # Name or IP of the box
         ├── nmap   # Reports from nmap tool
         ├── www    # Staging files to serve over http
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
Use the sections below for common ways to gain more info or access via the ports available.

#### 21 FTP

#### 22 SSH
With credentials:
```
ssh {{USERNAME}}@{{TARGET_MACHINE}} 
```

Loopback with crednetials:
```
ssh -L {{LOCAL_PORT}}:{{TARGET_MACHINE}}:{{REMOTE_PORT}} {{USERNAME}}@{{TARGET_MACHINE}}
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
<TARGET_MACHINE>:9200/_cat/indecies?pretty=true
```
See also [ELK Stack][4]

Search an index:
```
<TARGET_MACHINE>:9200/<INDEX>/_search?q=*<QUERY_STRING>*&size=<#>&pretty=true
```
* `<INDEX>`: The index to search
* `<QUERY_STRING>`: Search all the docs with the query string
* `<#>`: Number of results to show (default is 10)

#### 960X Logstash
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

_Coming Soon_

## 4. Cover
Typically in CTF environments, you won't need to worry about covering your tracks.

_Coming Soon_

[1]: https://nmap.org/
[2]: https://github.com/rebootuser/LinEnum
[3]: https://github.com/gchq/CyberChef
[4]: https://www.elastic.co/guide/index.html
