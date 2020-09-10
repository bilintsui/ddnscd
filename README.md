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
* config.json.example: config file of ddnscd.
* services/*: collections of systemd Service Unit files in different runmode.

## Usage
<pre>
ddnscd <-o|s|f [-p pidfile]> [-c conf]
ddnscd <-h|--help>
ddnscd <-v|--version>
</pre>

## Configs
### Example
<pre>
{
	"confver": 4,
	"log":"/var/log/dddnscd.log",
	"loglevel":1,
	"tick":600,
	"lookup4":"https://api4.xmrx1999.com/ip.php",
	"lookup6":"https://api6.xmrx1999.com/ip.php",
	"deploys":
	[
		{
			"root":"example.com",
			"username":"user@example.com",
			"password":"examplepassword",
			"deploy":
			[
				{
					"name": "1.ddns",
					"automode": true,
					"interface4": "",
					"interface6": ""
				},
				{
					"name": "2.ddns",
					"automode": true,
					"interface4": "",
					"interface6": ""
				}
			]
		},
		{
			"root":"example2.com",
			"username":"user@example2.com",
			"password":"examplepassword2",
			"deploy":
			[
				{
					"name": "1.ddns",
					"automode": true,
					"interface4": "",
					"interface6": ""
				},
				{
					"name": "2.ddns",
					"automode": true,
					"interface4": "",
					"interface6": ""
				}
			]
		}
	]
}
</pre>
### Explanation
 * confver: Config Version. Currently 4 for v1.2.
 * log: Filename which logs saved to.
 * loglevel: Optional, default 1. 0: Errors only; 1: Errors and Updates; 2: All messages.
 * tick: Optional, required when runmode=simple/forking. Unit: second. Time of intervals between each run.
 * lookup4: A URL which provides your public IP(IPv4), it should return IP only.
 * lookup6: A URL which provides your public IP(IPv6), it should return IP only.
 * deploys: Deploy Information Section.
 >* root: Your root domain. (eg: if you want to deploy "ddns.example.com", use "example.com" here.)
 >* username: Your account name. (In cloudflare, it should be your email address.)
 >* password: Your password. (In cloudflare, it should be your Cloudflare API Key.)
 >* deploy: Domain information and options which you want to update/create.
 >>* name: Domain names which under your root domain. This name should without your root domain name. Use "@" or "" to refer your root domain.
 >>* automode: Optional, use true/false, default true. Program will auto detect your existed DNS records and updated them(legacy method). If disabled, you can ask the program to update IPv4, IPv6 seperately or both through specific interface.
 >>* interface4: Optional, only effects when automode=false. Define it if you want to update your IPv4 address. Use "" to use system default interface. Use interface name to Lookup your IPv4 address via specific interface.
 >>* interface6: Optional, only effects when automode=false. Same with interface4 option, just the IPv6 version.