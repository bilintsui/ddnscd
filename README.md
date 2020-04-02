# ddnscd
Dynamic DNS Client Daemon
A Daemonized Client for DDNS Deployment.
**This program is only support cloudflare now.**

## Requirements
* Linux
* Perl, and module JSON(could install with "cpan install JSON")
* curl
* make

## Files
* ddnscd: Main program.
* ddnscd.service.example: Example of Service Unit file for systemd.
* config.json: config file of ddnscd.

## Usage
ddnscd <config_file>
* config_file: the path of your config file. (eg: /etc/ddnscd/config.json)

## Configs
### Example(you should delete all linefeeds in final configurations)
{<br/>
 "log":"/var/log/ddnscd.log",<br/>
 "tick":600,<br/>
 "lookup4":"",<br/>
 "lookup6":"",<br/>
 "root":"",<br/>
 "domain":"",<br/>
 "username":"",<br/>
 "password":""<br/>
 }<br/>
 * log: The path which you logs writes into.
 * tick: Integer in second. The frequency of program to check your current ip. 600 for check it every 10 minutes.
 * lookup4: A URL which provides your public IP(IPv4), it should return IP only.
 * lookup6: A URL which provides your public IP(IPv6), it should return IP only.
 * root: Your root domain. (eg: if you want to deploy "ddns.example.com", use "example.com" here.)
 * domain: Your domain which you want to deploy new IP for. (eg: if you want to deploy "ddns.example.com", use "ddns.example.com" here.)
 * username: Your account name. (In cloudflare, it should be your email address.)
 * password: Your password. (In cloudflare, it should be your Cloudflare API Key.)
