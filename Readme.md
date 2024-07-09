# Network Routing and Endpoint
Azure Administrator you've been tasked to implement the following requirements:

- Create and configure a virtual network in Azure.
- Deploy two virtual machines into different subnets of the virtual network.
- Ensure the virtual machines have public IP addresses that won't change over time.
- Protect the virtual machine public endpoints from being accessible from the internet.
- Ensure internal Azure virtual machines names and IP addresses can be resolved.
- Ensure a publicly available domain name can be resolved by external queries.

![Architecture diagram as explained in the text.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-network-routing-endpoints/media/lab-04.png)

## Create Vnet with multiple subnets
![image](https://github.com/karthi770/Azure_VNet_NSG_Bastion/assets/102706119/bf8b7d46-59d6-4331-aedc-ae77171ebc67)
![image](https://github.com/karthi770/Azure_VNet_NSG_Bastion/assets/102706119/ef28422a-6d59-4ed0-b071-4d5593a8ffa5)

![image](https://github.com/karthi770/Azure_VNet_NSG_Bastion/assets/102706119/411633d3-be36-4729-980e-8cbf9a0cb430)

## Create Virtual machines
Go to Powershell → advance settings → Enter the mandatory parameters → create storage
![image](https://github.com/karthi770/Jira_GitHub_intergration_Python/assets/102706119/1e52ccaf-52b2-4d78-901a-2c2dcf6e1436)
Upload the - az104-04-vms-loop-template.json and az104-04-vms-loop-parameters.json
The json can be downloaded from this github repo:
https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/tree/6aafb1e8822fe75d3955ec3a96825ca5346c8f3f/Allfiles/Interactive%20Lab%20Simulation%20Files/04
![image](https://github.com/karthi770/Jira_GitHub_intergration_Python/assets/102706119/a6435a87-dd5e-4f9c-96a2-15d483e718e2)
Change the desired password:
![image](https://github.com/karthi770/Jira_GitHub_intergration_Python/assets/102706119/d6b17514-41bf-4c86-afe2-defc9053de8c)
According to the documentation the below script runs the deployment of the VMs:
```sh
New-AzResourceGroupDeployment -Name "az104-04-vms" -ResourceGroupName "Network-routing" -TemplateParameterFile "az104-04-vms-loop-parameters.json" -TemplateFile "az104-04-vms-loop-template.json"
```
![image](https://github.com/karthi770/Jira_GitHub_intergration_Python/assets/102706119/c4c26330-68a8-4d7f-8b7b-4c530c9dc589)

## Configure private and public IP addresses of Azure VMs
Go to Network Interface card → IP configuration → select ipconfig1 → Allocation is Static → Associate IP address → Select the public IP address.
![image](https://github.com/karthi770/Jira_GitHub_intergration_Python/assets/102706119/cf24aa7c-1ae4-4f05-b9a1-3281b5965679)
Similarly do this step for the second VM.

## Configure Network security groups
![image](https://github.com/karthi770/Jira_GitHub_intergration_Python/assets/102706119/681bf5e4-fb65-4869-a66d-7db1fee09a80)
After creation click on ‘Go to resource’.

Go to the created Network security group and select the inbound security rules and add a new rule to allow RDP
![image](https://github.com/karthi770/Jira_GitHub_intergration_Python/assets/102706119/51bf7b48-cfb3-484e-9096-9adc4e21b886)![image](https://github.com/karthi770/Jira_GitHub_intergration_Python/assets/102706119/86d29153-dd64-4c24-aa67-647e6e0ec428)

Now go to Network interfaces → Click on Associate → Add NICs
![image](https://github.com/karthi770/Jira_GitHub_intergration_Python/assets/102706119/9663639d-b08c-408e-af16-faea65064f7e)


## Configure Azure DNS for internal name resolution:
Go to Private DNS zones:
![image](https://github.com/karthi770/Jira_GitHub_intergration_Python/assets/102706119/579f30a5-0b4e-41a2-b61d-a60998b74bca)

Once the private DNS creation is over → click on Go to resources → click on Virtual Network links → Click on Add.
![image](https://github.com/karthi770/Jira_GitHub_intergration_Python/assets/102706119/dff755d3-ea41-4908-a5fa-d9ba6247d522)

Go to over view and check for the record sets.
![image](https://github.com/karthi770/Jira_GitHub_intergration_Python/assets/102706119/7cae462d-d78d-4125-911a-4cfca2402845)

Connect to the first virtual machine with the RDP:
![image](https://github.com/karthi770/Jira_GitHub_intergration_Python/assets/102706119/d2e57508-3fdd-48a6-97ee-cd88a9569d0c)
In the RDP go to the poweshell and enter the command `nslookup az104-04-vm0.testnet.com` the output has the private IP of the virtual machines.
![image](https://github.com/karthi770/Jira_GitHub_intergration_Python/assets/102706119/b7c728a5-2c86-41fe-b825-97e40579a6a5)

## Configure Azure DNS for external name resolution
![image](https://github.com/karthi770/Jira_GitHub_intergration_Python/assets/102706119/f3771115-8f7d-4e7a-8d07-e75cf3426049)

![image](https://github.com/karthi770/Jira_GitHub_intergration_Python/assets/102706119/c5de5676-6a24-487b-a319-b63e209ab68d)

Run the command `nslookup az104-04-vm0.testnetpublic.com. ns1-32.azure-dns.com.` and check if the public IP is visible in the terminal.
![image](https://github.com/karthi770/Jira_GitHub_intergration_Python/assets/102706119/5c6a58ea-ef03-4fcf-8280-5a84132738e1)
