# azure-cli
Azure tools. (Git, Ansible, Azire Cli)

``` bash
az deployment group create -g sans1weursggenerigene001 --name azure-cli --template-file azure_template/template.json --parameters @azure_template/parameters.json
az vm extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --vm-name cdsw \
  --resource-group sans1weursggenerigene001 \
  --settings '{"commandToExecute":"sudo yum install -y git && sudo git clone https://github.com/alesbatalla/azure-cli.git"}'



az vm delete  --name cdsw   --resource-group  sans1weursggenerigene001  -y
az disk delete --name cdsw_OSDisk_0 --resource-group sans1weursggenerigene001  -y
az disk delete --name cdsw_DataDisk_0 --resource-group sans1weursggenerigene001 -y
az disk delete --name cdsw_DataDisk_1 --resource-group sans1weursggenerigene001 -y
az network nic delete --name cdsw_ni_1 --resource-group sans1weursggenerigene001
az network public-ip delete -g sans1weursggenerigene001 -n cdsw-ip
```
