# Dynamic DNS Client Daemon
A daemonized client for DDNS deployment.

**NOTE:** This program is only support cloudflare now.

## Download
* For production use: Download source code for latest stable version (e.g. v1.x) in [release page](https://github.com/bilintsui/ddnscd/releases).
* For testing use: Download source code for latest pre-release version (e.g. v1.x-betax) in [release page](https://github.com/bilintsui/ddnscd/releases).
* For developer: Clone the master branch.

## Requirements
* Linux
* Perl, and module Module::Load, File::Basename, POSIX, WWW::Curl::Easy, JSON
* (Optional)Perl module: Net::SMTP, Authen::SASL, MIME::Lite (Required when you enable mail notice.)
* (Optional)Perl module: JSON (Required when your IPv4/IPv6 lookup service returns a JSON data.

## Files
* ddnscd: Main program.
* examples/config.json: Example configuration file of ddnscd.
* examples/ddnscd.service: Example systemd service unit file, run in simple mode.

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
	"confver": 5,
	"log":"/var/log/ddnscd/ddnscd.log",
	"loglevel":1,
	"tick":600,
	"lookup4":"https://api4.xmrx1999.com/ip.php",
	"lookup4key":"",
	"lookup6":"https://api6.xmrx1999.com/ip.php",
	"lookup6key":"",
	"smtp":
	{
		"host": "smtp.example.com",
		"port": 25,
		"ency": "PLAIN",
		"user": "example@example.com",
		"pass": "examplesmtppassword"
	},
	"deploys":
	[
		{
			"root":"example.com",
			"username":"user@example.com",
			"auth_type":"key",
			"password":"examplekey",
			"deploy":
			[
				{
					"name": "1.ddns",
					"automode": true,
					"interface4": "",
					"interface6": "",
					"notice": ""
				},
				{
					"name": "2.ddns",
					"automode": true,
					"interface4": "",
					"interface6": "",
					"notice": ""
				}
			]
		},
		{
			"root":"example2.com",
			"username":"user@example2.com",
			"auth_type":"token",
			"password":"exampletoken",
			"deploy":
			[
				{
					"name": "1.ddns",
					"automode": true,
					"interface4": "",
					"interface6": "",
					"notice": ""
				},
				{
					"name": "2.ddns",
					"automode": true,
					"interface4": "",
					"interface6": "",
					"notice": ""
				}
			]
		}
	]
}
</pre>
### Explanation
 * confver: Config Version. Currently 5 for v1.4-beta2 and onward.
 * log: Filename which logs saved to.
 * loglevel: Optional, default 1. 0: Errors only; 1: Errors and Updates; 2: All messages.
 * tick: Optional, required when runmode=simple/forking. Unit: second. Time of intervals between each run.
 * lookup4: A URL which provides your public IP(IPv4), it should return IP only.
 * lookup4key: JSON key name on root if IPv4 Lookup service returns a JSON.
 * lookup6: A URL which provides your public IP(IPv6), it should return IP only.
 * lookup6key: JSON key name on root if IPv4 Lookup service returns a JSON.
 * smtp: Optional, only effects when Mail notice service enabled.
 >* host: SMTP server address.
 >* port: SMTP server port.
 >* ency: Encryption type(case sensitive). PLAIN for SMTP(port usually 25), SSL for SMTPS(SSL, port usually 465), STARTTLS for SMTPS(STARTTLS, port usually 587).
 >* user: SMTP username which user you want to send email from.
 >* pass: SMTP password which user you want to send email from.
 * deploys: Deploy Information Section.
 >* root: Your root domain. (eg: if you want to deploy "ddns.example.com", use "example.com" here.)
 >* username: Your account name. (In cloudflare, it should be your email address.)
 >* auth_type: Your authentication type. (If you are using API Key, then use "key". If you are using API Token, then use "token".)
 >* password: Your password. (In cloudflare, it should be your Cloudflare API Key or your Cloudflare API Token, it depends on which method(auth_type) you used.)
 >* deploy: Domain information and options which you want to update/create.
 >>* name: Domain names which under your root domain. This name should without your root domain name. Use "@" or "" to refer your root domain.
 >>* automode: Optional, use true/false, default true. Program will auto detect your existed DNS records and updated them(legacy method). If disabled, you can ask the program to update IPv4, IPv6 seperately or both through specific interface.
 >>* interface4: Optional, only effects when automode=false. Define it if you want to update your IPv4 address. Use "" to use system default interface. Use interface name to Lookup your IPv4 address via specific interface.
 >>* interface6: Optional, only effects when automode=false. Same with interface4 option, just the IPv6 version.
 >>* notice: Optional, when IP changed, send email to email addresses listed here. Seperate with comma when noticing multiple recipicents. Example: recv1@example.com,recv2@example.com
