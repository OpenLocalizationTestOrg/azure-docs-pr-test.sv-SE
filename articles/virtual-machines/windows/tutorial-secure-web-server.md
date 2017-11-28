---
title: Skydda IIS med SSL-certifikat i Azure | Microsoft Docs
description: "Lär dig att skydda IIS-webbserver med SSL-certifikat på en Windows-dator i Azure"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/14/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 6567853e9ef3cad63595dc0afe7a793bdc5d972c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="secure-iis-web-server-with-ssl-certificates-on-a-windows-virtual-machine-in-azure"></a><span data-ttu-id="866a3-103">Skydda IIS-webbserver med SSL-certifikat på en Windows-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="866a3-103">Secure IIS web server with SSL certificates on a Windows virtual machine in Azure</span></span>
<span data-ttu-id="866a3-104">Ett certifikat senare SSL (Secure Sockets) kan användas för kryptering av webbtrafik säker webbserver.</span><span class="sxs-lookup"><span data-stu-id="866a3-104">To secure web servers, a Secure Sockets Later (SSL) certificate can be used to encrypt web traffic.</span></span> <span data-ttu-id="866a3-105">Dessa SSL-certifikat kan lagras i Azure Key Vault och tillåta säker distribution av certifikat till Windows-datorer (VM) i Azure.</span><span class="sxs-lookup"><span data-stu-id="866a3-105">These SSL certificates can be stored in Azure Key Vault, and allow secure deployments of certificates to Windows virtual machines (VMs) in Azure.</span></span> <span data-ttu-id="866a3-106">I den här självstudiekursen får du lära du dig att:</span><span class="sxs-lookup"><span data-stu-id="866a3-106">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="866a3-107">Skapa ett Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="866a3-107">Create an Azure Key Vault</span></span>
> * <span data-ttu-id="866a3-108">Generera och överföra ett certifikat till Nyckelvalvet</span><span class="sxs-lookup"><span data-stu-id="866a3-108">Generate or upload a certificate to the Key Vault</span></span>
> * <span data-ttu-id="866a3-109">Skapa en virtuell dator och installera IIS-webbservern</span><span class="sxs-lookup"><span data-stu-id="866a3-109">Create a VM and install the IIS web server</span></span>
> * <span data-ttu-id="866a3-110">Mata in certifikatet i den virtuella datorn och konfigurera IIS med en SSL-bindning</span><span class="sxs-lookup"><span data-stu-id="866a3-110">Inject the certificate into the VM and configure IIS with an SSL binding</span></span>

<span data-ttu-id="866a3-111">Den här självstudien kräver Azure PowerShell-modul version 3.6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="866a3-111">This tutorial requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="866a3-112">Kör ` Get-Module -ListAvailable AzureRM` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="866a3-112">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="866a3-113">Om du behöver uppgradera [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="866a3-113">If you need to upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>


## <a name="overview"></a><span data-ttu-id="866a3-114">Översikt</span><span class="sxs-lookup"><span data-stu-id="866a3-114">Overview</span></span>
<span data-ttu-id="866a3-115">Azure Key Vault skyddar kryptografiska nycklar och hemligheter, dessa certifikat eller lösenord.</span><span class="sxs-lookup"><span data-stu-id="866a3-115">Azure Key Vault safeguards cryptographic keys and secrets, such certificates or passwords.</span></span> <span data-ttu-id="866a3-116">Key Vault hjälper till att förenkla hanteringen för certifikatet och gör att du kan behålla kontrollen över nycklar som har åtkomst till dessa certifikat.</span><span class="sxs-lookup"><span data-stu-id="866a3-116">Key Vault helps streamline the certificate management process and enables you to maintain control of keys that access those certificates.</span></span> <span data-ttu-id="866a3-117">Du kan skapa ett självsignerat certifikat i Nyckelvalvet eller ladda upp en befintlig, betrodda certifikat som du redan äger.</span><span class="sxs-lookup"><span data-stu-id="866a3-117">You can create a self-signed certificate inside Key Vault, or upload an existing, trusted certificate that you already own.</span></span>

<span data-ttu-id="866a3-118">I stället för att använda en anpassad VM-avbildning som inkluderar certifikat inbyggd-modulen Certifikat Injicera i en aktiv virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="866a3-118">Rather than using a custom VM image that includes certificates baked-in, you inject certificates into a running VM.</span></span> <span data-ttu-id="866a3-119">Den här processen säkerställer att de senaste certifikat installeras på en webbserver under distributionen.</span><span class="sxs-lookup"><span data-stu-id="866a3-119">This process ensures that the most up-to-date certificates are installed on a web server during deployment.</span></span> <span data-ttu-id="866a3-120">Om du vill förnya eller ersätta ett certifikat, behöver du också skapa en ny anpassad VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="866a3-120">If you renew or replace a certificate, you don't also have to create a new custom VM image.</span></span> <span data-ttu-id="866a3-121">Senaste certifikaten är automatiskt matas in när du skapar ytterligare virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="866a3-121">The latest certificates are automatically injected as you create additional VMs.</span></span> <span data-ttu-id="866a3-122">Under hela processen certifikat aldrig lämna Azure-plattformen eller exponeras i ett skript, kommandoradsverktyget historik eller mall.</span><span class="sxs-lookup"><span data-stu-id="866a3-122">During the whole process, the certificates never leave the Azure platform or are exposed in a script, command-line history, or template.</span></span>


## <a name="create-an-azure-key-vault"></a><span data-ttu-id="866a3-123">Skapa ett Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="866a3-123">Create an Azure Key Vault</span></span>
<span data-ttu-id="866a3-124">Innan du kan skapa ett Nyckelvalv och certifikat, skapa en resursgrupp med [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="866a3-124">Before you can create a Key Vault and certificates, create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="866a3-125">I följande exempel skapas en resursgrupp med namnet *myResourceGroupSecureWeb* i den *östra USA* plats:</span><span class="sxs-lookup"><span data-stu-id="866a3-125">The following example creates a resource group named *myResourceGroupSecureWeb* in the *East US* location:</span></span>

```powershell
$resourceGroup = "myResourceGroupSecureWeb"
$location = "East US"
New-AzureRmResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

<span data-ttu-id="866a3-126">Skapa sedan ett Nyckelvalv med [ny AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault).</span><span class="sxs-lookup"><span data-stu-id="866a3-126">Next, create a Key Vault with [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault).</span></span> <span data-ttu-id="866a3-127">Varje Key Vault kräver ett unikt namn och bör vara alla gemen.</span><span class="sxs-lookup"><span data-stu-id="866a3-127">Each Key Vault requires a unique name, and should be all lower case.</span></span> <span data-ttu-id="866a3-128">Ersätt `<mykeyvault>` i följande exempel med dina egna unika Key Vault-namn:</span><span class="sxs-lookup"><span data-stu-id="866a3-128">Replace `<mykeyvault>` in the following example with your own unique Key Vault name:</span></span>

```powershell
$keyvaultName="<mykeyvault>"
New-AzureRmKeyVault -VaultName $keyvaultName `
    -ResourceGroup $resourceGroup `
    -Location $location `
    -EnabledForDeployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a><span data-ttu-id="866a3-129">Generera ett certifikat och lagra i Key Vault</span><span class="sxs-lookup"><span data-stu-id="866a3-129">Generate a certificate and store in Key Vault</span></span>
<span data-ttu-id="866a3-130">För produktion, ska du importera ett giltigt certifikat som signerats av en betrodd provider med [importera AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/import-azurekeyvaultcertificate).</span><span class="sxs-lookup"><span data-stu-id="866a3-130">For production use, you should import a valid certificate signed by trusted provider with [Import-AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/import-azurekeyvaultcertificate).</span></span> <span data-ttu-id="866a3-131">Den här självstudiekursen i följande exempel visas hur du kan skapa ett självsignerat certifikat med [Lägg till AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/add-azurekeyvaultcertificate) som använder standardprincipen för certifikat från [ Nya AzureKeyVaultCertificatePolicy](/powershell/module/azurerm.keyvault/new-azurekeyvaultcertificatepolicy).</span><span class="sxs-lookup"><span data-stu-id="866a3-131">For this tutorial, the following example shows how you can generate a self-signed certificate with [Add-AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/add-azurekeyvaultcertificate) that uses the default certificate policy from [New-AzureKeyVaultCertificatePolicy](/powershell/module/azurerm.keyvault/new-azurekeyvaultcertificatepolicy).</span></span> 

```powershell
$policy = New-AzureKeyVaultCertificatePolicy `
    -SubjectName "CN=www.contoso.com" `
    -SecretContentType "application/x-pkcs12" `
    -IssuerName Self `
    -ValidityInMonths 12

Add-AzureKeyVaultCertificate `
    -VaultName $keyvaultName `
    -Name "mycert" `
    -CertificatePolicy $policy 
```


## <a name="create-a-virtual-machine"></a><span data-ttu-id="866a3-132">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="866a3-132">Create a virtual machine</span></span>
<span data-ttu-id="866a3-133">Ange en administratör användarnamn och lösenord för den virtuella datorn med [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="866a3-133">Set an administrator username and password for the VM with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="866a3-134">Nu kan du skapa den virtuella datorn med [ny AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="866a3-134">Now you can create the VM with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="866a3-135">I följande exempel skapar krävs virtuella nätverkskomponenter, OS-konfigurationen och sedan skapar en virtuell dator med namnet *myVM*:</span><span class="sxs-lookup"><span data-stu-id="866a3-135">The following example creates the required virtual network components, the OS configuration, and then creates a VM named *myVM*:</span></span>

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -Name "myVnet" `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$publicIP = New-AzureRmPublicIpAddress `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4 `
    -Name "myPublicIP"

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name "myNetworkSecurityGroupRuleRDP"  `
    -Protocol "Tcp" `
    -Direction "Inbound" `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

# Create an inbound network security group rule for port 443
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
    -Name "myNetworkSecurityGroupRuleWWW"  `
    -Protocol "Tcp" `
    -Direction "Inbound" `
    -Priority 1001 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 443 `
    -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -Name "myNetworkSecurityGroup" `
    -SecurityRules $nsgRuleRDP,$nsgRuleWeb

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
    -Name "myNic" `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIP.Id `
    -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS2" | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName "myVM" -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" -Skus "2016-Datacenter" -Version "latest" | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create virtual machine
New-AzureRmVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig

# Use the Custom Script Extension to install IIS
Set-AzureRmVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location $location `
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server -IncludeManagementTools"}'
```

<span data-ttu-id="866a3-136">Det tar några minuter för den virtuella datorn skapas.</span><span class="sxs-lookup"><span data-stu-id="866a3-136">It takes a few minutes for the VM to be created.</span></span> <span data-ttu-id="866a3-137">Det sista steget använder tillägget för Azure anpassat skript för att installera IIS-webbserver med [Set AzureRmVmExtension](/powershell/module/azurerm.compute/set-azurermvmextension).</span><span class="sxs-lookup"><span data-stu-id="866a3-137">The last step uses the Azure Custom Script Extension to install the IIS web server with [Set-AzureRmVmExtension](/powershell/module/azurerm.compute/set-azurermvmextension).</span></span>


## <a name="add-a-certificate-to-vm-from-key-vault"></a><span data-ttu-id="866a3-138">Lägga till ett certifikat till en virtuell dator från Nyckelvalvet</span><span class="sxs-lookup"><span data-stu-id="866a3-138">Add a certificate to VM from Key Vault</span></span>
<span data-ttu-id="866a3-139">Om du vill lägga till certifikatet från Nyckelvalvet till en virtuell dator, Hämta ID för certifikatet med [Get-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/get-azurekeyvaultsecret).</span><span class="sxs-lookup"><span data-stu-id="866a3-139">To add the certificate from Key Vault to a VM, obtain the ID of your certificate with [Get-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/get-azurekeyvaultsecret).</span></span> <span data-ttu-id="866a3-140">Lägga till certifikatet till den virtuella datorn med [Lägg till AzureRmVMSecret](/powershell/module/azurerm.compute/add-azurermvmsecret):</span><span class="sxs-lookup"><span data-stu-id="866a3-140">Add the certificate to the VM with [Add-AzureRmVMSecret](/powershell/module/azurerm.compute/add-azurermvmsecret):</span></span>

```powershell
$certURL=(Get-AzureKeyVaultSecret -VaultName $keyvaultName -Name "mycert").id

$vm=Get-AzureRMVM -ResourceGroupName $resourceGroup -Name "myVM"
$vaultId=(Get-AzureRmKeyVault -ResourceGroupName $resourceGroup -VaultName $keyVaultName).ResourceId
$vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $vaultId -CertificateStore "My" -CertificateUrl $certURL

Update-AzureRmVM -ResourceGroupName $resourceGroup -VM $vm
```


## <a name="configure-iis-to-use-the-certificate"></a><span data-ttu-id="866a3-141">Konfigurera IIS att använda certifikat</span><span class="sxs-lookup"><span data-stu-id="866a3-141">Configure IIS to use the certificate</span></span>
<span data-ttu-id="866a3-142">Använda tillägget för anpassat skript igen med [Set AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) att uppdatera konfigurationen av IIS.</span><span class="sxs-lookup"><span data-stu-id="866a3-142">Use the Custom Script Extension again with [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) to update the IIS configuration.</span></span> <span data-ttu-id="866a3-143">Den här uppdateringen gäller certifikatet matas in från Nyckelvalvet till IIS och konfigurerar web-bindning:</span><span class="sxs-lookup"><span data-stu-id="866a3-143">This update applies the certificate injected from Key Vault to IIS and configures the web binding:</span></span>

```powershell
$PublicSettings = '{
    "fileUris":["https://raw.githubusercontent.com/iainfoulds/azure-samples/master/secure-iis.ps1"],
    "commandToExecute":"powershell -ExecutionPolicy Unrestricted -File secure-iis.ps1"
}'

Set-AzureRmVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location $location `
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -SettingString $publicSettings
```


### <a name="test-the-secure-web-app"></a><span data-ttu-id="866a3-144">Testa det säkra webbprogrammet</span><span class="sxs-lookup"><span data-stu-id="866a3-144">Test the secure web app</span></span>
<span data-ttu-id="866a3-145">Hämta den offentliga IP-adressen på den virtuella datorn med [Get-AzureRmPublicIPAddress](/powershell/resourcemanager/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="866a3-145">Obtain the public IP address of your VM with [Get-AzureRmPublicIPAddress](/powershell/resourcemanager/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="866a3-146">I följande exempel hämtar IP-adressen för `myPublicIP` skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="866a3-146">The following example obtains the IP address for `myPublicIP` created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName $resourceGroup -Name "myPublicIP" | select "IpAddress"
```

<span data-ttu-id="866a3-147">Nu kan du öppna en webbläsare och ange `https://<myPublicIP>` i adressfältet.</span><span class="sxs-lookup"><span data-stu-id="866a3-147">Now you can open a web browser and enter `https://<myPublicIP>` in the address bar.</span></span> <span data-ttu-id="866a3-148">Om du vill acceptera säkerhetsmeddelande om du använder ett självsignerat certifikat, Välj **information** och sedan **går du vidare till webbsidan**:</span><span class="sxs-lookup"><span data-stu-id="866a3-148">To accept the security warning if you used a self-signed certificate, select **Details** and then **Go on to the webpage**:</span></span>

![Acceptera web Säkerhetsvarning för webbläsare](./media/tutorial-secure-web-server/browser-warning.png)

<span data-ttu-id="866a3-150">Skyddad IIS-webbplatsen visas som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="866a3-150">Your secured IIS website is then displayed as in the following example:</span></span>

![Visa körs säker IIS-webbplats](./media/tutorial-secure-web-server/secured-iis.png)


## <a name="next-steps"></a><span data-ttu-id="866a3-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="866a3-152">Next steps</span></span>

<span data-ttu-id="866a3-153">I den här självstudiekursen skyddas en IIS-webbserver med ett SSL-certifikat som lagras i Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="866a3-153">In this tutorial, you secured an IIS web server with an SSL certificate stored in Azure Key Vault.</span></span> <span data-ttu-id="866a3-154">Du har lärt dig hur till:</span><span class="sxs-lookup"><span data-stu-id="866a3-154">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="866a3-155">Skapa ett Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="866a3-155">Create an Azure Key Vault</span></span>
> * <span data-ttu-id="866a3-156">Generera och överföra ett certifikat till Nyckelvalvet</span><span class="sxs-lookup"><span data-stu-id="866a3-156">Generate or upload a certificate to the Key Vault</span></span>
> * <span data-ttu-id="866a3-157">Skapa en virtuell dator och installera IIS-webbservern</span><span class="sxs-lookup"><span data-stu-id="866a3-157">Create a VM and install the IIS web server</span></span>
> * <span data-ttu-id="866a3-158">Mata in certifikatet i den virtuella datorn och konfigurera IIS med en SSL-bindning</span><span class="sxs-lookup"><span data-stu-id="866a3-158">Inject the certificate into the VM and configure IIS with an SSL binding</span></span>

<span data-ttu-id="866a3-159">Den här länken om du vill se förskapad virtuella skriptexempel.</span><span class="sxs-lookup"><span data-stu-id="866a3-159">Follow this link to see pre-built virtual machine script samples.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="866a3-160">Windows skriptexempel för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="866a3-160">Windows virtual machine script samples</span></span>](./powershell-samples.md)