# ddnscd
>**Dynamic DNS Client Daemon**<br/>
>**A Daemonized Client for DDNS Deployment.**<br/>
>**This program is only support cloudflare now.**

## Requirements
* Linux
* Perl, and module JSON, File::Basename, POSIX
* curl

## Install Perl Module: JSON
* apt(like Debian, Ubuntu): apt install libjson-perl
* yum/dnf(like Fedora, RHEL, CentOS): yum install perl-JSON
* pacman(like ArchLinux): pacman -S perl-json

## Files
* ddnscd: Main program.
* ddnscd.service.example: Example of Service Unit file for systemd.
* config.json.example: config file of ddnscd.
* configs/*: collections of configs in different runmode.
* services/*: collections of systemd Service Unit files in different runmode.

## Usage
ddnscd <-o|s|f [-p pidfile]> [-c conf]<br/>
See more, use "ddnscd -h" or "ddnscd --help".

## Configs
### Example
>{<br/>
>>"confver": 3, <br/>
>>"log": "/var/log/ddnscd.log", <br/>
>>"loglevel": 1, <br/>
>>"tick": 600, <br/>
>>"lookup4": "https://api4.xmrx1999.com/ip.php", <br/>
>>"lookup6": "https://api6.xmrx1999.com/ip.php", <br/>
>>"deploys": [<br/>
>>>{<br/>
>>>>"root": "example.com", <br/>
>>>>"username": "user@example.com", <br/>
>>>>"password": "examplepassword", <br/>
>>>>"deploy": [<br/>
>>>>>"1.ddns", <br/>
>>>>>"2.ddns"<br/>
>>>>]<br/>
>>>}, <br/>
>>>{<br/>
>>>>"root": "example2.com", <br/>
>>>>"username": "user@example2.com", <br/>
>>>>"password": "examplepassword2", <br/>
>>>>"deploy": [<br/>
>>>>>"1.ddns", <br/>
>>>>>"2.ddns"<br/>
>>>>]<br/>
>>>}<br/>
>>]<br/>
>}<br/>
 * confver: Config Version. Currently 3 for v1.2 beta 6.
 * log: Filename which logs saved to.
 * loglevel: Optional, default 1. 0: Errors only; 1: Errors and Updates; 2: All messages.
 * tick: Optional, required when runmode=simple/forking. Unit: second. Time of intervals between each run.
 * lookup4: A URL which provides your public IP(IPv4), it should return IP only.
 * lookup6: A URL which provides your public IP(IPv6), it should return IP only.
 * deploys: Deploy Information Section.
 >* root: Your root domain. (eg: if you want to deploy "ddns.example.com", use "example.com" here.)
 >* username: Your account name. (In cloudflare, it should be your email address.)
 >* password: Your password. (In cloudflare, it should be your Cloudflare API Key.)
 >* deploy: Domain names which under your root domain. This name should without your root domain name. Use "@" or "" to refer your root domain.
