# Netability Azure Environment Creation - Commands Updated as of 8/18/21

Write-Host "Welcome to the Netability Azure Environment Rollout Script. 
If you are running this locally on your machine, stop now and run from Azure Cloud Shell in the tenant environment."

$rg = Read-Host "Please enter name for Resource Group"
$vnet_name = Read-Host "Enter name for virtual network"
$sub_name = Read-Host "Enter subnet name"
$ip_range = Read-Host "Enter IP Range for Customer (10.0.0.0/16)"
$ip_subnet = Read-Host "Enter primary subnet(10.0.1.0/24-/29)"
$ip_nsg = Read-Host "Enter name for Azure NSG"
$location = Read-Host "Please enter Azure tenant location. By default this script will use SouthCentralUS."
    if ($location -eq ''){
        $location = "SouthCentralUS"
    }
$vmname = Read-Host "Enter name for virtual machine"
$vmsize = Read-Host "Please enter the size for the Virtual Machine you'd like to create. If you are unsure, leave this blank."
    if ($vmsize -eq ''){
        do {
            Write-Host "Getting virtual machine options for your selected region"
            az vm list-skus --location $location --output table
            $vmsize = Read-Host "Please enter size for Virtual Machine. Note: Copy name from above table."
        }   while ($vmsize -eq '')
    }
#Confirm Entries
Write-Host "Please confirm the below inputs"
Write-Host $rg
Write-Host $location
Write-Host $vnet_name
Write-Host $sub_name
Write-Host $ip_range
Write-Host $ip_subnet
Write-Host $ip_nsg
Write-Host $vmsize
Write-Host $vmname
Write-Host $storage_acc
$confirm = Read-Host "Please confirm all entries are correct [Y/N]"
Until ($confirm -eq 'Y')
    if ($confirm -eq 'Y'){
        New-AzResourceGroup -Name $rg -Location $location
        $LanSub = New-AzVirtualNetworkSubnetConfig -Name $sub_name -AddressPrefix $ip_subnet
        New-AzVirtualNetwork -ResourceGroupName $rg -Location $location -Name $vnet_name -AddressPrefix $ip_range -Subnet $LanSub
        $virtual_mac = New-AzVM -Name $vmname -ResourceGroupName $rg -Location $location -VirtualNetworkName $vnet_name -SubnetName $sub_name -size $vmsize
        $virtual_mac = Set-AzVMOperatingSystem -VM $virtual_mac -Windows -ComputerName $vmname -ProvisionVMAgent -EnableAutoUpdate
        $virtual_mac = Add-AzVMNetworkInterface -VM $virtual_mac
        New-AzVM -ResourceGroupName $rg -Location $location -VM $virtual_mac -AsJob -Verbose
        Write-Host "Configuration Completed. Please check https://portal.azure.com in a few moments to see all resources populate."
}
