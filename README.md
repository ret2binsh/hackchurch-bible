# Hack Church Bible

Hack Church is a group dedicated to making a comprehensive penetration testing methodology available to everyone. If you would like to contribute, please submit a PR or report an Issue. 

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
nmap -SC -sV -oA nmap/<FILENAME> <TARGET_MACHINE>
```
* `<FILENAME>`: Name of the output file (name of the box)
* `<TARGET_MACHINE>`: Target machine

## 2. Access
Use the sections below for common ways to gain more info or access via the ports available.

#### 21 FTP

#### 22 SSH
With credentials:
```
ssh 
```

#### 80 HTTP
View the website in a browser: `<TARGET_MACHINE>:80`

#### 443 HTTPS

#### 8080 HTTP (DEV)
See 80 HTTP

#### 8888 HTTP (DEV)
See 80 HTTP

#### 9200 ElasticSearch
List available indecies:
```
<TARGET_MACHINE>:9200/_cat/indecies?pretty=true
```

Search an index:
```
<TARGET_MACHINE>:9200/<INDEX>/_search?q=*<QUERY_STRING>*&size=<#>&pretty=true
```
* `<INDEX>`: The index to search
* `<QUERY_STRING>`: Search all the docs with the query string
* `<#>`: Number of results to show (default is 10)

## 3. PrivEsc
Once you gain access to machine, you'll need to execute post-exploit recon to determine vectors of priveledge escalation.

_Coming Soon_

## 4. Cover
Typically in CTF environments, you won't need to worry about covering your tracks.

_Coming Soon_

[1]: https://nmap.org/
[2]: https://github.com/rebootuser/LinEnum
[3]: https://github.com/gchq/CyberChef