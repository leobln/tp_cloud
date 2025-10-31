🌞 Tester cloud-init

```
#cloud-config
disable_root: false
system_info:
  default_user:
    name: leo_tp2

users:
  - name: leo_tp2
    sudo: ALL=(ALL) NOPASSWD:ALL
    groups: sudo
    shell: /bin/bash
    ssh_authorized_keys:
      - ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIMp9JKDgk9OZEzlc1hxYXvxaYEAVWwU6DLOQ+9i7nKhq clÃ© TP Cloud

  - name: leo2_tp2
    sudo: ALL=(ALL) NOPASSWD:ALL
    groups: sudo
    shell: /bin/bash
    ssh_authorized_keys:
      - ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIMp9JKDgk9OZEzlc1hxYXvxaYEAVWwU6DLOQ+9i7nKhq clÃ© TP Cloud
```



```
To check existing issues, please visit: https://github.com/Azure/azure-cli/issues
PS C:\Users\leobe\.ssh> az vm create `
>>   --resource-group totototo_group `
>>   --name azure1.tp2 `
>>   --image Ubuntu2204 `
>>   --custom-data "C:\chemin\vers\cloud-init.txt" `
>>   --ssh-key-values "C:\Users\leobe\.ssh\cloud_t.pub" `
>>   --size "Standard_B1s"
The default value of '--size' will be changed to 'Standard_D2s_v5' from 'Standard_DS1_v2' in a future release.
Selecting "northeurope" may reduce your costs. The region you've selected may cost more for the same services. You can disable this message in the future with the command "az config set core.display_region_identified=false". Learn more at https://go.microsoft.com/fwlink/?linkid=222571

{
  "fqdns": "",
  "id": "/subscriptions/28378ca2-d30f-411c-83c2-1c0241e89433/resourceGroups/totototo_group/providers/Microsoft.Compute/virtualMachines/azure1.tp2",
  "location": "francecentral",
  "macAddress": "60-45-BD-6C-CC-E2",
  "powerState": "VM running",
  "privateIpAddress": "172.16.0.4",
  "publicIpAddress": "4.233.63.34",
  "resourceGroup": "totototo_group"
}
PS C:\Users\leobe\.ssh> ssh leo_tp2@4.233.63.34
```



```

```
