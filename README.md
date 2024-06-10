
# Snippets
<details>
	<summary>Varnish</summary>

### filter by request host header

    varnishlog -q 'ReqHeader ~ "Host: example.com"'

### filter by request url
	varnishlog -q 'ReqURL ~ "^/some/path/"'

### filter by client ip (behind reverse proxy)
	varnishlog -q 'ReqHeader ~ "X-Real-IP: .*123.123.123.123"'

### filter by request host header and show request url and referrer header
	varnishlog -q 'ReqHeader ~ "Host: (www\.)?example\.com"' -i "ReqURL" -I "ReqHeader:Referer:"

### filter for permanent redirects and show request host/url and new location
	varnishlog -q "RespStatus ~ 301" -i "ReqURL" -I "ReqHeader:Host:" -I "RespHeader:Location:" -i "RespStatus"

### filter for permanent and temporary redirects and filter for Location "http://s3" to just show (for example) redirects to something on an Amazon S3 bucket
	varnishlog -q '(RespStatus ~ 301 or RespStatus ~307) and RespHeader ~ "Location: https://s3"' \
	   -i "ReqURL" -I "ReqHeader:Host:" -I "RespHeader:Location:" -i "RespStatus" -I "ReqHeader:Referer:"

</details>

<details>
	<summary>GIT</summary>
	
### Branch nie został zmergowany a pojawiły się zmiany w masterze
	1. git chechout master
	2. git pull
	3. git checkout BRANCH
	4. git rebase master
	5. git push -f origin BRANCH

### Klonowanie konkretnego brancha
	1. git clone -b <branch> <remote_repo>
 
</details>

<details>
	<summary>Linux</summary>
	
### Restart hasła root Oralce Linux 8 / CentOS
	0. Boot menu > na pierwszej linii 'e'
	1. Dopisujemy rw init=/bin/bash na końcu linii zaczynającej się od 'linux'. Ctrl + x
	2. passwd
	3. touch /.autorelabel
	4. /usr/sbin/reboot –f
 
### Zmiana hostname Oracle Linux 8
	1. $ hostnamectl set-hostname jenkins_slave_04

### Rozszerzenie LVM
	1. echo 1 > /sys/block/sdb/device/rescan
	2. pvresize /dev/sdb
	3. pvs
	4. lvresize -r -L 50G sysvg/varlv
</details>

<details>
	<summary> Terraform / Terragrunt</summary>

 ### Import tip
 
</details>
