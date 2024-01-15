# Dynamic DNS Client Daemon
A daemonized client for DDNS deployment.

**NOTE:** This program only supports Cloudflare now.

## Downloading
* For production use: download source code for the latest stable version (e.g. `v1.x`) in [Releases](https://github.com/bilintsui/ddnscd/releases/latest).
* For testing use: download source code for the latest pre-release version (e.g. `v1.x-betax`) in [Releases](https://github.com/bilintsui/ddnscd/releases).
* For developers: clone the `master` branch.

## Requirements
* Linux
* Perl, and module Module::Load, File::Basename, POSIX, WWW::Curl::Easy, JSON
* (Optional) Perl module: Net::SMTP, Authen::SASL, MIME::Lite (required when mail notice enabled).
* (Optional) Perl module: JSON (required when IPv4/IPv6 lookup service returns JSON data).

## Files
* ddnscd: main program.
* examples/config.json: example configuration file.
* examples/ddnscd.service: example systemd service unit file, run in simple mode.

## Usage
<pre>
ddnscd <-o|s|f [-p pidfile]> [-c conf]
ddnscd <-h|--help>
ddnscd <-v|--version>
</pre>

## Configurations
<pre>
{
  "confver": 5,
  "log": "/var/log/ddnscd/ddnscd.log",
  "loglevel": 1,
  "tick": 600,
  "lookup4": "https://api4.xmrx1999.com/ip.php",
  "lookup4key": "",
  "lookup6": "https://api6.xmrx1999.com/ip.php",
  "lookup6key": "",
  "smtp": {
    "host": "smtp.example.com",
    "port": 25,
    "ency": "PLAIN",
    "user": "example@example.com",
    "pass": "examplesmtppassword"
  },
  "deploys": [
    {
      "root": "example.com",
      "username": "user@example.com",
      "auth_type": "key",
      "password": "examplekey",
      "deploy": [
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
      "root": "example2.com",
      "username": "user@example2.com",
      "auth_type": "token",
      "password": "exampletoken",
      "deploy": [
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
* `confver`: config version, currently `5` for v1.4-beta2 and onward.
* `log`: filename which logs saved to.
* `loglevel`: optional, defaults to `1`. Accepted values:
  * `0`: only errors
  * `1`: errors and updates
  * `2`: all messages
* `tick`: optional (required when runmode is "simple" or "forking"), time of intervals between each run in seconds.
* `lookup4`: a URL which provides your public IPv4 address, it should only return an IPv4 address.
* `lookup4key`: JSON key name if IPv4 Lookup service returns a JSON, nested key not supported.
* `lookup6`: a URL which provides your public IPv6 address, it should only return an IPv6 address.
* `lookup6key`: JSON key name if IPv6 Lookup service returns a JSON, nested key not supported.
* `smtp`: optional, only effects when enabled mail notice service.
  * `host`: SMTP server address.
  * `port`: SMTP server port.
  * `ency`: encryption type, case sensitive. Accepted values:
    * `PLAIN` for SMTP (port usually 25)
    * `SSL` for SMTPS (SSL, port usually 465)
    * `STARTTLS` for SMTPS (STARTTLS, port usually 587)
  * `user`: SMTP username which user you want to send email from.
  * `pass`: SMTP password which user you want to send email from.
* `deploys`: deploy information section.
  * `root`: root domain (e.g. `example.com` when deploying "ddns.example.com").
  * `username`: account name (in Cloudflare, it should be your email address).
  * `auth_type`: authentication type (for Cloudflare). Accepted values:
    * `key`: `password` is a Cloudflare Global API Key.
    * `token`: `password` is a Cloudflare API Token.
  * `password`: password.
  * `deploy`: domain information and options which you want to update / create.
    * `name`: domain name under the root domain. Use `@` or left empty to refer to the root domain.
    * `automode`: optional, `true` / `false`, defaults to `true`.
      * Program will auto detect your existing DNS records and update them (legacy method). If disabled, you can ask the program to update IPv4 or IPv6 separately or both through a specific interface.
    * `interface4`: optional, only effects when `automode` is disabled. Define it when the IPv4 address needs to be updated. Left empty to use the system default interface. Use interface name to lookup your IPv4 address via a specific interface.
    * `interface6`: optional, only effects when `automode` is disabled. Same with `interface4`, just replaced to IPv6.
    * `notice`: optional, when IP changed, send email to email addresses listed here. Separate with commas when noticing multiple recipients.
      * Example: `recv1@example.com,recv2@example.com`

## Versioning
* Stable releases: `v1.x`, branch out to the REL1_x from master after bumping version, no actual changes will be made while bumping version, tagged with `1.x`, downloadable from Releases.
* Bug (critical) fix releases: `v1.x.x`, only happens when critical bugs need to be fixed immediately, but should commit to the master branch first, then cherry-pick them to the REL1_x branch. Tagged with `1.x.x` after version preparation is done, downloadable from Releases.
* Betas: `v1.x-betax`, committed to master branch directly, tagged with "1.x-betax" after version preparation is done, downloadable from Releases.
* Alphas: `v1.x-betax-alpha`, the most recent version without an exact patch version number, will not be tagged, recorded (to version.json), and released, only available when cloning the master branch.
