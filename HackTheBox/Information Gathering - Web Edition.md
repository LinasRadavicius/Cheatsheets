**Tools*
DNS TOOLS: crt.sh | https://search.censys.io
^ Find subdomains using certs
**Automated Recon Frameworks:**
FinalRecon , Recon-ng, theHarvester, SpiderFoot. OSINT Framework

**FINALRECON**
Usage:
clone git
install pip3 requirements.txt
chmod +x finalrecon.py
./finalrecon.py --headers --whois --url http://inlanefreight.com


#****WHOIS!****
Identifying Key Personnel: WHOIS records often reveal the names, email addresses, and phone numbers of individuals responsible for managing the domain. This information can be leveraged for social engineering attacks or to identify potential targets for phishing campaigns.  

Discovering Network Infrastructure: Technical details like name servers and IP addresses provide clues about the target's network infrastructure. This can help penetration testers identify potential entry points or misconfigurations.
Historical Data Analysis: Accessing historical WHOIS records through services like WhoisFreaks can reveal changes in ownership, contact information, or technical details over time. This can be useful for tracking the evolution of the target's digital presence.


**DNS**
dig inlanefreight MX
dig inlanefreight.com ns
nslookup
dig axfr @nsztm1.digi.ninja zonetransfer.me

**SUBDOMAINS**
Typically represtned by A / AAAA or IPv6 records, also CNAME records can store them
dnsenum -enum inlanefreight.com -f /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -r


**Crawling, Robots, URIs**
BURP or ZAP have crawlers
Scrapy - pip3 install scrapy
ReconSpider (needs Scrapy for it to work)
wget -O ReconSpider.zip https://academy.hackthebox.com/storage/modules/144/ReconSpider.v1.2.zip  
python3 ReconSpider.py http://inlanefreight.com


**GOBUSTER**
gobuster vhost -u htttp://inlanefreight.htb:58460 -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain


**SKILL ASSESSMENT**
whois inlanefreight.com
curl -l http://inlanefreight.htb:50220/  

nikto -h inlanefreight.htb:50220 -Tuning b  
gobuster vhost -u http://inlanefreight.htb -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain
(don't forget to update /etc/hosts as you go)

  python3 ReconSpider3.py http://dev.web1337.inlanefreight.htb:50220 -o results.json

