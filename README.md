
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

### Sprawdzenie rozmiaru plików w repo
	git rev-list --disk-usage=human --objects origin/BRANCH
	git rev-list --disk-usage=human --objects BRANCH
	git rev-list BRANCH | xargs -n1 git ls-tree -rl | sed -e 's/[^ ]* [^ ]* \(.*\)\t.*/\1/' | sort -u | awk '{ sum += $2 } END { print sum }'

### Połączenie commitów w jeden
	1. git rebase -i origin/master (wszystkie commity)
 	1a.git rebase -i HEAD~n (n liczba commitów do złączenia)
  	2. W edytorze wybieramy pierwszy commit jako pick, reszta squash
   	3. Poprawiamy commit message
    	4. git push --force-with-lease origin NAZWA-BRANCHA
 
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
TIP: jaby zrobić import w terraguncie obiektów które zostały stworzone przez for_each np. subnety to trzeba to zrobić tak:
	
 	terragrunt import 'azurerm_subnet.subnet["snet-house-westeurope-mgmt"]' "/subscriptions/fdd6ed9f-a00f-4c23-acb6-9dbc4275b190/resourceGroups/rg-conectivity-house/providers/Microsoft.Network/virtualNetworks/vnet-house-westeurope/subnets/snet-house-westeurope-mgmt"
  	terragrunt import 'azurerm_subnet_nat_gateway_association.ng["snet-house-westeurope-backoffice"]' /subscriptions/fdd6ed9f-a00f-4c23-acb6-9dbc4275b190/resourceGroups/rg-conectivity-house/providers/Microsoft.Network/virtualNetworks/vnet-house-westeurope/subnets/snet-house-westeurope-backoffice

a nie tak jak domyślny obiekt:

	terragrunt import azurerm_subnet.subnet /subscriptions/fdd6ed9f-a00f-4c23-acb6-9dbc4275b190/resourceGroups/rg-conectivity-house/providers/Microsoft.Network/virtualNetworks/vnet-house-westeurope/subnets/snet-house-westeurope-mgmt
	
</details>

<details>
	<summary>Network</summary>

### TCP Dump
 	tcpdump -nn -s0 -i eth0 -v host ! 35.235.243.224
  
  	tcpdump -i eth0 -nn -s0 -v host ! 10.255.169.193 -w source-10.0.18.29.pcap

</details>

<details>
	<summary>Jenkins</summary>
	
### Abort joba
	Jenkins.instance.getItemByFullName("Order_Process_Monitor_Web_Test_Run")
		.getBuildByNumber(56710)
		.finish(hudson.model.Result.ABORTED, 
			new java.io.IOException("Aborting build")
	);

### Konwertowanie credentiali na plaintext
 	println( hudson.util.Secret.decrypt("{AQAAABAAAABAX6K+NRmq4CqzVUNxoUom74P6LTUezbx6Z0ZtExkfIW8g3ukzf4RSrFbQhK2AXAs9EYEk8XCLyU+jSyYEISidOoQ10Dea+K1OstHMsRMtD1c=}") )

### Wykonanie joba przez CLI na zdalnym Jenkinsie
	java -jar jenkins-cli.jar -s https://jenkins-bss.lppdev.pl/ -auth jenkinsecom:113063704caa6d03eb9adf9f0b5370e699 build devops/test-devops

### Wylistowanie pluginów
 	Jenkins.instance.pluginManager.plugins.each{
  		plugin -> 
    			println ("${plugin.getDisplayName()} (${plugin.getShortName()}): ${plugin.getVersion()}")
	}

### Wyczyszczenie kolejki buildów
 	Jenkins.instance.queue.clear()
### Wyczyszcznie kolejki konkretnych buildów
	import hudson.model.*
	def q = Jenkins.instance.queue
	q.items.findAll { it.task.name.startsWith('REPLACEME') }.each { q.cancel(it.task) }

</details>

<details>
	<summary>GCP</summary>

### Wylistowanie adresów ip dla subnetu
	gcloud compute instances list  --filter="networkInterfaces.subnetwork:sandbox-subnet-onprem" --format="value(networkInterfaces[0].networkIP)"
	gcloud compute networks subnets list --format="value(NAME)"
	gcloud compute instances list  --filter="networkInterfaces.subnetwork:sandbox-subnet-backofficce" --format="value(networkInterfaces[0].networkIP)"
 
### Wylistowanie instancji wraz z adresami ip w statusie RUNNING
	ZONES="europe-west1-b,europe-west1-c,europe-west1-d"
	gcloud --project=prod-cropp compute instances list --zones=${ZONES} --format="table(name,status,networkInterfaces[0].networkIP)"|grep RUNNING

</details>

<details>
	<summary>Azure</summary>

### Usunięcie backendu z Load Balancera w Azure
Aby usunąć backend z Load Balancera, który wskazuje na np. VMSS to Azure nie pozwoli tego usunąć:


> Failed to delete load balancer backend pool 'default'. Error: Cannot remove backend address pool default from load balancer since it is in use by virtual machine scale set /subscriptions/fdd6ed9f-a00f-4c23-acb6-9dbc4275b190/resourceGroups/rg-house-varnish-prod/providers/Microsoft.Compute/virtualMachineScaleSets/vmss-house-varnish-weu-prod.


	az vmss update --resource-group rg-house-varnish-prod --name vmss-house-varnish-weu-prod --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerBackendAddressPools 0
 
	az vmss update-instances --instance-ids "*" --name vmss-house-varnish-weu-prod --resource-group rg-house-varnish-prod
	
 	az network lb delete --resouce-group rg-house-varnish-prod --name lb-test-priv
</details>

<details>
	<summary>RabbitMQ</summary>
	
### Dodanie usera
	
	rabbitmqctl add_user monitoring monitoring
	rabbitmqctl set_permissions -p / monitoring "" "" ".*"
	rabbitmqctl list_users

	Listing users ...
	user	tags
	magento	[administrator]
	monitoring	[]
	guest	[administrator]
	newrelic	[monitoring]
	
</details>
