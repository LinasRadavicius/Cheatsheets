**Intro**
What to fuzz
Directories, files and extensions, hidden vHosts, PHP params, param values

**Seclists**
/opt/useful/SecLists

**Directory Fuzzing**

ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-big.txt -u http://83.136.254.23:38827/FUZZ

**Page Fuzzing**

if multiple wordlist you use -w everytime but at the end of the wordlists.txt:FUZZINGNAME so then you specify it in the param you want to fuzz too
ffuf -w users.txt:USER -w passwords.txt:PASS -u http://SERVER_IP:PORT/login -d "username=USER&password=PASS"

ffuf -w /opt/useful/seclists/Discovery/Web-Content/web-extensions-big.txt:EXTENSIONS -u "http://83.136.254.23:38827/blog/indexEXTENSIONS" -fr "403"
Found .php so then try file directorys 2.3 with .php

ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-big.txt:FILES -u "http://83.136.254.23:38827/blog/FILES.php"



**RECURSIVE FUZZING**

ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ -recursion -recursion-depth 1 -e .php -v

From <https://academy.hackthebox.com/module/54/section/483> 

ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u http://83.136.254.23:38827/FUZZ -recursion -recursion-depth 2 -e .php -v

FOR cleaner output:
ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u http://83.136.254.23:38827/FUZZ -recursion -recursion-depth 2 -e .php -o results.txt -of csv && cut -d ',' -f2 results.txt


**DOMAIN FUZZING**
/opt/useful/seclists/Discovery/DNS

**Sub-Domain fuzz:**

ffuf -w /opt/useful/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u "https://FUZZ.inlanefreight.com"

**vHost Fuzzing**

ffuf -w /opt/useful/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:PORT/ -H 'Host: FUZZ.academy.htb'

^ The IP of academy.htb must be known for fuzzing to work (in etc/hosts)

**Filtering Results:**
ffuf -w /opt/useful/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:PORT/ -H 'Host: FUZZ.academy.htb' -fs 986
-fs filters HTTP response size, which we saw after running it previously that 986 is when it doesnt exist




**PARAMATER FUZZING:**

ffuf -w /opt/useful/seclists/Discovery/Web-Content/burp-parameter-names.txt -u http://admin.academy.htb:32370/admin/admin.php?FUZZ=key
add -fs with the http response size for all requests on original scan -fs 798 (in my lab)

Doing same using POST ^:

ffuf -w /opt/useful/seclists/Discovery/Web-Content/burp-parameter-names.txt -u http://admin.academy.htb:32370/admin/admin.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "FUZZ=key" -fs 798


**Value Fuzzing:**

ffuf -w ids.txt -u http://admin.academy.htb:32370/admin/admin.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "id=FUZZ" -fs 768
Get flag : (only worked with POST request)
curl -s -X POST -d "id=73" http://admin.academy.htb:32370/admin/admin.php








**SKILL ASSESSMENT**

ffuf -w /opt/useful/seclists/Discovery/DNS/subdomains-top1million-20000.txt -u http://academy.htb:40364/ -H "Host: FUZZ.academy.htb" -fs 985

ffuf -w /opt/useful/seclists/Discovery/Web-Content/web-extensions-big.txt -u "http://academy.htb:40364/indexFUZZ" -fr "403"

ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://faculty.academy.htb:40364/FUZZ -recursion -recursion-depth 1 -e ".php, .phps, .php7" -v -fc 403

PARAMS
GET :
ffuf -w /opt/useful/seclists/Discovery/Web-Content/burp-parameter-names.txt -u http://faculty.academy.htb:40364/courses/linux-security.php7?FUZZ=key -fs 774

POST:
ffuf -w /opt/useful/seclists/Discovery/Web-Content/burp-parameter-names.txt -u http://faculty.academy.htb:40364/courses/linux-security.php7 -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "FUZZ=key" -fs 774

ffuf -w /opt/useful/seclists/Usernames/xato-net-10-million-usernames.txt -u http://faculty.academy.htb:40364/courses/linux-security.php7 -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=FUZZ" -fs 781

curl -s http://faculty.academy.htb:40364/courses/linux-security.php7 -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=harry"

