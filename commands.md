## AZ-204 Training Day#1
az group create --location "north europe"`
  --name "myrg01" `
  --tags "optimization"

az vm create `
  --resource-group "myrg01" `
  --name "myVMmyrg01" `
  --image "UbuntuLTS" `
  --admin-username "demouser" `
  --admin-password "Demouser@123" `
  --location 'west europe' `
  --tags optimization=yes

az vm open-port --port 8080 --resource-group myrg01 --name myVMmyrg01

# SSH on web server
sudo apt-get -y update
sudo apt-get -y install nginx

# RG Cleanup
az group delete --name myrg01


(Get-AzAppServicePlan -ResourceGroupName "lab01rg" -Name "ASP-lab01rg-974c").Sku.Capacity
(Get-AzAppServicePlan -ResourceGroupName "lab01rg" -Name "ASP-lab01rg-974c").Sku.Size

#resize and scaling
Set-AzAppServicePlan -ResourceGroupName "lab01rg" -Name "ASP-lab01rg-974c" -NumberofWorkers 1 -Tier Standard -WorkerSize Meduim
Set-AzAppServicePlan -ResourceGroupName "myrg02lab01" -Name "asp-mywebappsp01lab01" -NumberofWorkers 1 -Tier Standard -WorkerSize Medium

# Powershell workflow
workflow recurseJobs {
  foreach -parallel ($i in 1..5) {
    Start-Job { Invoke-WebRequest -Uri 'https://mywebapp01lab01.azurewebsites.net/' }
  }
}
recurseJobs

# workflow example 2
workflow Start-ConfigWF2 {
  param ([string[]]$filename)
  foreach ($files in $filename) {
    sequence {
      # Parellel {
      ('-' * 20)
      $files
      ('-' * 20)
      Get-Content -Path $files
    }
  }
}
# calling workflow
Start-ConfigWF2 -filename 'Api.csproj', 'appsettings.json'

