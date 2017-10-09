---
title: aaaSecure IIS med SSL-certifikat i Azure | Microsoft Docs
description: "Lär dig hur toosecure hello IIS webbserver med SSL-certifikat på en Windows-dator i Azure"
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
ms.openlocfilehash: 7a9e0ce07be2f55095fdb5347b64faf5caa4f7e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="secure-iis-web-server-with-ssl-certificates-on-a-windows-virtual-machine-in-azure"></a><span data-ttu-id="f8b50-103">Skydda IIS-webbserver med SSL-certifikat på en Windows-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="f8b50-103">Secure IIS web server with SSL certificates on a Windows virtual machine in Azure</span></span>
<span data-ttu-id="f8b50-104">toosecure webbservrar som kan vara ett certifikat senare SSL (Secure Sockets) används tooencrypt webbtrafik.</span><span class="sxs-lookup"><span data-stu-id="f8b50-104">toosecure web servers, a Secure Sockets Later (SSL) certificate can be used tooencrypt web traffic.</span></span> <span data-ttu-id="f8b50-105">Dessa SSL-certifikat kan lagras i Azure Key Vault och tillåta säker distribution av certifikat tooWindows virtuella maskiner (VMs) i Azure.</span><span class="sxs-lookup"><span data-stu-id="f8b50-105">These SSL certificates can be stored in Azure Key Vault, and allow secure deployments of certificates tooWindows virtual machines (VMs) in Azure.</span></span> <span data-ttu-id="f8b50-106">I den här självstudiekursen får du lära du dig att:</span><span class="sxs-lookup"><span data-stu-id="f8b50-106">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f8b50-107">Skapa ett Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="f8b50-107">Create an Azure Key Vault</span></span>
> * <span data-ttu-id="f8b50-108">Generera och överför ett certifikat toohello Key Vault</span><span class="sxs-lookup"><span data-stu-id="f8b50-108">Generate or upload a certificate toohello Key Vault</span></span>
> * <span data-ttu-id="f8b50-109">Skapa en virtuell dator och installera hello IIS-webbserver</span><span class="sxs-lookup"><span data-stu-id="f8b50-109">Create a VM and install hello IIS web server</span></span>
> * <span data-ttu-id="f8b50-110">Mata in hello certifikat i hello VM och konfigurera IIS med en SSL-bindning</span><span class="sxs-lookup"><span data-stu-id="f8b50-110">Inject hello certificate into hello VM and configure IIS with an SSL binding</span></span>

<span data-ttu-id="f8b50-111">Den här kursen kräver hello Azure PowerShell module 3,6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="f8b50-111">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="f8b50-112">Kör ` Get-Module -ListAvailable AzureRM` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="f8b50-112">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="f8b50-113">Om du behöver tooupgrade finns [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="f8b50-113">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>


## <a name="overview"></a><span data-ttu-id="f8b50-114">Översikt</span><span class="sxs-lookup"><span data-stu-id="f8b50-114">Overview</span></span>
<span data-ttu-id="f8b50-115">Azure Key Vault skyddar kryptografiska nycklar och hemligheter, dessa certifikat eller lösenord.</span><span class="sxs-lookup"><span data-stu-id="f8b50-115">Azure Key Vault safeguards cryptographic keys and secrets, such certificates or passwords.</span></span> <span data-ttu-id="f8b50-116">Key Vault hjälper till att förenkla hanteringen för hello certifikat och aktiverar toomaintain kontrollen över nycklar som har åtkomst till dessa certifikat.</span><span class="sxs-lookup"><span data-stu-id="f8b50-116">Key Vault helps streamline hello certificate management process and enables you toomaintain control of keys that access those certificates.</span></span> <span data-ttu-id="f8b50-117">Du kan skapa ett självsignerat certifikat i Nyckelvalvet eller ladda upp en befintlig, betrodda certifikat som du redan äger.</span><span class="sxs-lookup"><span data-stu-id="f8b50-117">You can create a self-signed certificate inside Key Vault, or upload an existing, trusted certificate that you already own.</span></span>

<span data-ttu-id="f8b50-118">I stället för att använda en anpassad VM-avbildning som inkluderar certifikat inbyggd-modulen Certifikat Injicera i en aktiv virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="f8b50-118">Rather than using a custom VM image that includes certificates baked-in, you inject certificates into a running VM.</span></span> <span data-ttu-id="f8b50-119">Den här processen säkerställer att hello senaste certifikat installeras på en webbserver under distributionen.</span><span class="sxs-lookup"><span data-stu-id="f8b50-119">This process ensures that hello most up-to-date certificates are installed on a web server during deployment.</span></span> <span data-ttu-id="f8b50-120">Om du förnya eller ersätta ett certifikat kan har du också inte toocreate en ny anpassad VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="f8b50-120">If you renew or replace a certificate, you don't also have toocreate a new custom VM image.</span></span> <span data-ttu-id="f8b50-121">hello senaste certifikat är automatiskt matas in när du skapar ytterligare virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="f8b50-121">hello latest certificates are automatically injected as you create additional VMs.</span></span> <span data-ttu-id="f8b50-122">Under hela processen hello hello certifikat aldrig lämna hello Azure-plattformen eller exponeras i ett skript, kommandoradsverktyget historik eller mall.</span><span class="sxs-lookup"><span data-stu-id="f8b50-122">During hello whole process, hello certificates never leave hello Azure platform or are exposed in a script, command-line history, or template.</span></span>


## <a name="create-an-azure-key-vault"></a><span data-ttu-id="f8b50-123">Skapa ett Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="f8b50-123">Create an Azure Key Vault</span></span>
<span data-ttu-id="f8b50-124">Innan du kan skapa ett Nyckelvalv och certifikat, skapa en resursgrupp med [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="f8b50-124">Before you can create a Key Vault and certificates, create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="f8b50-125">hello följande exempel skapar en resursgrupp med namnet *myResourceGroupSecureWeb* i hello *östra USA* plats:</span><span class="sxs-lookup"><span data-stu-id="f8b50-125">hello following example creates a resource group named *myResourceGroupSecureWeb* in hello *East US* location:</span></span>

```powershell
$resourceGroup = "myResourceGroupSecureWeb"
$location = "East US"
New-AzureRmResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

<span data-ttu-id="f8b50-126">Skapa sedan ett Nyckelvalv med [ny AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault).</span><span class="sxs-lookup"><span data-stu-id="f8b50-126">Next, create a Key Vault with [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault).</span></span> <span data-ttu-id="f8b50-127">Varje Key Vault kräver ett unikt namn och bör vara alla gemen.</span><span class="sxs-lookup"><span data-stu-id="f8b50-127">Each Key Vault requires a unique name, and should be all lower case.</span></span> <span data-ttu-id="f8b50-128">Ersätt `<mykeyvault>` i följande exempel med ditt eget unikt namn för Key Vault hello:</span><span class="sxs-lookup"><span data-stu-id="f8b50-128">Replace `<mykeyvault>` in hello following example with your own unique Key Vault name:</span></span>

```powershell
$keyvaultName="<mykeyvault>"
New-AzureRmKeyVault -VaultName $keyvaultName `
    -ResourceGroup $resourceGroup `
    -Location $location `
    -EnabledForDeployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a><span data-ttu-id="f8b50-129">Generera ett certifikat och lagra i Key Vault</span><span class="sxs-lookup"><span data-stu-id="f8b50-129">Generate a certificate and store in Key Vault</span></span>
<span data-ttu-id="f8b50-130">För produktion, ska du importera ett giltigt certifikat som signerats av en betrodd provider med [importera AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/import-azurekeyvaultcertificate).</span><span class="sxs-lookup"><span data-stu-id="f8b50-130">For production use, you should import a valid certificate signed by trusted provider with [Import-AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/import-azurekeyvaultcertificate).</span></span> <span data-ttu-id="f8b50-131">Den här självstudien hello följande exempel visar hur du kan skapa ett självsignerat certifikat med [Lägg till AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/add-azurekeyvaultcertificate) som använder hello standardprincipen för certifikat från [ Nya AzureKeyVaultCertificatePolicy](/powershell/module/azurerm.keyvault/new-azurekeyvaultcertificatepolicy).</span><span class="sxs-lookup"><span data-stu-id="f8b50-131">For this tutorial, hello following example shows how you can generate a self-signed certificate with [Add-AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/add-azurekeyvaultcertificate) that uses hello default certificate policy from [New-AzureKeyVaultCertificatePolicy](/powershell/module/azurerm.keyvault/new-azurekeyvaultcertificatepolicy).</span></span> 

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


## <a name="create-a-virtual-machine"></a><span data-ttu-id="f8b50-132">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f8b50-132">Create a virtual machine</span></span>
<span data-ttu-id="f8b50-133">Ange ett administratörsanvändarnamn och lösenord för hello virtuell dator med [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="f8b50-133">Set an administrator username and password for hello VM with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="f8b50-134">Nu kan du skapa hello virtuell dator med [ny AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="f8b50-134">Now you can create hello VM with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="f8b50-135">hello följande exempel skapar hello krävs virtuella nätverkskomponenter hello OS-konfiguration, och skapar sedan en virtuell dator med namnet *myVM*:</span><span class="sxs-lookup"><span data-stu-id="f8b50-135">hello following example creates hello required virtual network components, hello OS configuration, and then creates a VM named *myVM*:</span></span>

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

# Use hello Custom Script Extension tooinstall IIS
Set-AzureRmVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location $location `
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server -IncludeManagementTools"}'
```

<span data-ttu-id="f8b50-136">Det tar några minuter för hello VM toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="f8b50-136">It takes a few minutes for hello VM toobe created.</span></span> <span data-ttu-id="f8b50-137">hello sista steget använder hello Azure-tillägget för anpassat skript tooinstall hello IIS-webbserver med [Set AzureRmVmExtension](/powershell/module/azurerm.compute/set-azurermvmextension).</span><span class="sxs-lookup"><span data-stu-id="f8b50-137">hello last step uses hello Azure Custom Script Extension tooinstall hello IIS web server with [Set-AzureRmVmExtension](/powershell/module/azurerm.compute/set-azurermvmextension).</span></span>


## <a name="add-a-certificate-toovm-from-key-vault"></a><span data-ttu-id="f8b50-138">Lägga till ett certifikat tooVM från Nyckelvalvet</span><span class="sxs-lookup"><span data-stu-id="f8b50-138">Add a certificate tooVM from Key Vault</span></span>
<span data-ttu-id="f8b50-139">tooadd hello certifikat från Nyckelvalvet tooa VM, hämta hello-ID för ditt certifikat med [Get-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/get-azurekeyvaultsecret).</span><span class="sxs-lookup"><span data-stu-id="f8b50-139">tooadd hello certificate from Key Vault tooa VM, obtain hello ID of your certificate with [Get-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/get-azurekeyvaultsecret).</span></span> <span data-ttu-id="f8b50-140">Lägg till hello certifikat toohello VM med [Lägg till AzureRmVMSecret](/powershell/module/azurerm.compute/add-azurermvmsecret):</span><span class="sxs-lookup"><span data-stu-id="f8b50-140">Add hello certificate toohello VM with [Add-AzureRmVMSecret](/powershell/module/azurerm.compute/add-azurermvmsecret):</span></span>

```powershell
$certURL=(Get-AzureKeyVaultSecret -VaultName $keyvaultName -Name "mycert").id

$vm=Get-AzureRMVM -ResourceGroupName $resourceGroup -Name "myVM"
$vaultId=(Get-AzureRmKeyVault -ResourceGroupName $resourceGroup -VaultName $keyVaultName).ResourceId
$vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $vaultId -CertificateStore "My" -CertificateUrl $certURL

Update-AzureRmVM -ResourceGroupName $resourceGroup -VM $vm
```


## <a name="configure-iis-toouse-hello-certificate"></a><span data-ttu-id="f8b50-141">Konfigurera IIS toouse hello-certifikat</span><span class="sxs-lookup"><span data-stu-id="f8b50-141">Configure IIS toouse hello certificate</span></span>
<span data-ttu-id="f8b50-142">Använd hello tillägget för anpassat skript igen med [Set AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooupdate hello IIS-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="f8b50-142">Use hello Custom Script Extension again with [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooupdate hello IIS configuration.</span></span> <span data-ttu-id="f8b50-143">Den här uppdateringen gäller hello certifikat matas in från Nyckelvalvet tooIIS och konfigurerar hello web bindning:</span><span class="sxs-lookup"><span data-stu-id="f8b50-143">This update applies hello certificate injected from Key Vault tooIIS and configures hello web binding:</span></span>

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


### <a name="test-hello-secure-web-app"></a><span data-ttu-id="f8b50-144">Testa hello säkra webbprogram</span><span class="sxs-lookup"><span data-stu-id="f8b50-144">Test hello secure web app</span></span>
<span data-ttu-id="f8b50-145">Hämta hello offentliga IP-adressen för den virtuella datorn med [Get-AzureRmPublicIPAddress](/powershell/resourcemanager/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="f8b50-145">Obtain hello public IP address of your VM with [Get-AzureRmPublicIPAddress](/powershell/resourcemanager/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="f8b50-146">hello följande exempel hämtar hello IP-adress för `myPublicIP` skapade tidigare:</span><span class="sxs-lookup"><span data-stu-id="f8b50-146">hello following example obtains hello IP address for `myPublicIP` created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName $resourceGroup -Name "myPublicIP" | select "IpAddress"
```

<span data-ttu-id="f8b50-147">Nu kan du öppna en webbläsare och ange `https://<myPublicIP>` i hello adressfältet.</span><span class="sxs-lookup"><span data-stu-id="f8b50-147">Now you can open a web browser and enter `https://<myPublicIP>` in hello address bar.</span></span> <span data-ttu-id="f8b50-148">Välj tooaccept hello-Säkerhetsvarning om du använder ett självsignerat certifikat **information** och sedan **finns på webbsidan toohello**:</span><span class="sxs-lookup"><span data-stu-id="f8b50-148">tooaccept hello security warning if you used a self-signed certificate, select **Details** and then **Go on toohello webpage**:</span></span>

![Acceptera web Säkerhetsvarning för webbläsare](./media/tutorial-secure-web-server/browser-warning.png)

<span data-ttu-id="f8b50-150">Skyddad IIS-webbplatsen visas som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="f8b50-150">Your secured IIS website is then displayed as in hello following example:</span></span>

![Visa körs säker IIS-webbplats](./media/tutorial-secure-web-server/secured-iis.png)


## <a name="next-steps"></a><span data-ttu-id="f8b50-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f8b50-152">Next steps</span></span>

<span data-ttu-id="f8b50-153">I den här självstudiekursen skyddas en IIS-webbserver med ett SSL-certifikat som lagras i Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="f8b50-153">In this tutorial, you secured an IIS web server with an SSL certificate stored in Azure Key Vault.</span></span> <span data-ttu-id="f8b50-154">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="f8b50-154">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f8b50-155">Skapa ett Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="f8b50-155">Create an Azure Key Vault</span></span>
> * <span data-ttu-id="f8b50-156">Generera och överför ett certifikat toohello Key Vault</span><span class="sxs-lookup"><span data-stu-id="f8b50-156">Generate or upload a certificate toohello Key Vault</span></span>
> * <span data-ttu-id="f8b50-157">Skapa en virtuell dator och installera hello IIS-webbserver</span><span class="sxs-lookup"><span data-stu-id="f8b50-157">Create a VM and install hello IIS web server</span></span>
> * <span data-ttu-id="f8b50-158">Mata in hello certifikat i hello VM och konfigurera IIS med en SSL-bindning</span><span class="sxs-lookup"><span data-stu-id="f8b50-158">Inject hello certificate into hello VM and configure IIS with an SSL binding</span></span>

<span data-ttu-id="f8b50-159">Följ den här länken toosee inbyggd skriptexempel för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="f8b50-159">Follow this link toosee pre-built virtual machine script samples.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f8b50-160">Windows skriptexempel för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f8b50-160">Windows virtual machine script samples</span></span>](./powershell-samples.md)