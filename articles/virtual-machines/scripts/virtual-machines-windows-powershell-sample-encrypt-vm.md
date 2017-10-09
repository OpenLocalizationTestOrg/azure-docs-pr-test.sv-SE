---
title: aaaAzure PowerShell skriptexempel - kryptera en virtuell Windows-dator | Microsoft Docs
description: Azure PowerShell-skript Sample - kryptera Windows VM
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/02/2017
ms.author: iainfou
ms.openlocfilehash: 80c52a0b734a52a051ed30026b294840fd521143
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="encrypt-a-windows-virtual-machine-with-azure-powershell"></a><span data-ttu-id="97f93-103">Kryptera en virtuell dator i Windows med Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="97f93-103">Encrypt a Windows virtual machine with Azure PowerShell</span></span>

<span data-ttu-id="97f93-104">Det här skriptet skapar en säker Azure Key Vault, krypteringsnycklar, Azure Active Directory-tjänstens huvudnamn och ett Windows-dator (VM).</span><span class="sxs-lookup"><span data-stu-id="97f93-104">This script creates a secure Azure Key Vault, encryption keys, Azure Active Directory service principal, and a Windows virtual machine (VM).</span></span> <span data-ttu-id="97f93-105">hello VM krypteras sedan med hello krypteringsnyckeln från Nyckelvalvet och tjänstens huvudnamn autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="97f93-105">hello VM is then encrypted using hello encryption key from Key Vault and service principal credentials.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="97f93-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="97f93-106">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/encrypt-vm/encrypt-windows-vm.ps1 "Encrypt VM disks")]

## <a name="clean-up-deployment"></a><span data-ttu-id="97f93-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="97f93-107">Clean up deployment</span></span> 

<span data-ttu-id="97f93-108">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="97f93-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="97f93-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="97f93-109">Script explanation</span></span>

<span data-ttu-id="97f93-110">Det här skriptet använder hello följande kommandon toocreate hello distribution.</span><span class="sxs-lookup"><span data-stu-id="97f93-110">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="97f93-111">Varje objekt i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="97f93-111">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="97f93-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="97f93-112">Command</span></span> | <span data-ttu-id="97f93-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="97f93-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="97f93-114">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="97f93-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="97f93-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="97f93-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="97f93-116">Ny AzureRmKeyVault</span><span class="sxs-lookup"><span data-stu-id="97f93-116">New-AzureRmKeyVault</span></span>](/powershell/module/azurerm.keyvault/new-azurermkeyvault) | <span data-ttu-id="97f93-117">Skapar ett Azure Key Vault toostore säkra data, till exempel krypteringsnycklar.</span><span class="sxs-lookup"><span data-stu-id="97f93-117">Creates an Azure Key Vault toostore secure data such as encryption keys.</span></span> |
| [<span data-ttu-id="97f93-118">Lägg till AzureKeyVaultKey</span><span class="sxs-lookup"><span data-stu-id="97f93-118">Add-AzureKeyVaultKey</span></span>](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey) | <span data-ttu-id="97f93-119">Skapar en krypteringsnyckel i Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="97f93-119">Creates an encryption key in Key Vault.</span></span> |
| [<span data-ttu-id="97f93-120">Ny AzureRmADServicePrincipal</span><span class="sxs-lookup"><span data-stu-id="97f93-120">New-AzureRmADServicePrincipal</span></span>](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) | <span data-ttu-id="97f93-121">Skapar ett Azure Active Directory service principal toosecurely autentisera och styra tooencryption åtkomstnycklar.</span><span class="sxs-lookup"><span data-stu-id="97f93-121">Creates an Azure Active Directory service principal toosecurely authenticate and control access tooencryption keys.</span></span> |
| [<span data-ttu-id="97f93-122">Ange AzureRmKeyVaultAccessPolicy</span><span class="sxs-lookup"><span data-stu-id="97f93-122">Set-AzureRmKeyVaultAccessPolicy</span></span>](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) | <span data-ttu-id="97f93-123">Anger behörigheter på hello Key Vault toogrant hello service principal tooencryption åtkomstnycklar.</span><span class="sxs-lookup"><span data-stu-id="97f93-123">Sets permissions on hello Key Vault toogrant hello service principal access tooencryption keys.</span></span> |
| [<span data-ttu-id="97f93-124">Ny AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="97f93-124">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="97f93-125">Skapar en konfiguration av undernät.</span><span class="sxs-lookup"><span data-stu-id="97f93-125">Creates a subnet configuration.</span></span> <span data-ttu-id="97f93-126">Den här konfigurationen används med hello processen för att skapa virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="97f93-126">This configuration is used with hello virtual network creation process.</span></span> |
| [<span data-ttu-id="97f93-127">Ny AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="97f93-127">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="97f93-128">Skapar ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="97f93-128">Creates a virtual network.</span></span> |
| [<span data-ttu-id="97f93-129">Ny AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="97f93-129">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="97f93-130">Skapar en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="97f93-130">Creates a public IP address.</span></span> |
| [<span data-ttu-id="97f93-131">Ny AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="97f93-131">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="97f93-132">Skapar en grupp regeln nätverkssäkerhetskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="97f93-132">Creates a network security group rule configuration.</span></span> <span data-ttu-id="97f93-133">Den här konfigurationen är används toocreate en NSG regel när hello NSG skapas.</span><span class="sxs-lookup"><span data-stu-id="97f93-133">This configuration is used toocreate an NSG rule when hello NSG is created.</span></span> |
| [<span data-ttu-id="97f93-134">Ny AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="97f93-134">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="97f93-135">Skapar en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="97f93-135">Creates a network security group.</span></span> |
| [<span data-ttu-id="97f93-136">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="97f93-136">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="97f93-137">Hämtar information om undernät.</span><span class="sxs-lookup"><span data-stu-id="97f93-137">Gets subnet information.</span></span> <span data-ttu-id="97f93-138">Den här informationen används när du skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="97f93-138">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="97f93-139">Ny AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="97f93-139">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="97f93-140">Skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="97f93-140">Creates a network interface.</span></span> |
| [<span data-ttu-id="97f93-141">Ny AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="97f93-141">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="97f93-142">Skapar en VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="97f93-142">Creates a VM configuration.</span></span> <span data-ttu-id="97f93-143">Den här konfigurationen omfattar information som VM-namn, operativsystem och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="97f93-143">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="97f93-144">hello konfiguration används under skapande av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="97f93-144">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="97f93-145">Ny AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="97f93-145">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="97f93-146">Skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="97f93-146">Create a virtual machine.</span></span> |
| [<span data-ttu-id="97f93-147">Get-AzureRmKeyVault</span><span class="sxs-lookup"><span data-stu-id="97f93-147">Get-AzureRmKeyVault</span></span>](/powershell/module/azurerm.keyvault/get-azurermkeyvault) | <span data-ttu-id="97f93-148">Hämtar nödvändig information på hello Key Vault</span><span class="sxs-lookup"><span data-stu-id="97f93-148">Gets required information on hello Key Vault</span></span> |
| [<span data-ttu-id="97f93-149">Ange AzureRmVMDiskEncryptionExtension</span><span class="sxs-lookup"><span data-stu-id="97f93-149">Set-AzureRmVMDiskEncryptionExtension</span></span>](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) | <span data-ttu-id="97f93-150">Aktiverar kryptering på en virtuell dator med hjälp av hello service principal autentiseringsuppgifter och krypteringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="97f93-150">Enables encryption on a VM using hello service principal credentials and encryption key.</span></span> |
| [<span data-ttu-id="97f93-151">Get-AzureRmVmDiskEncryptionStatus</span><span class="sxs-lookup"><span data-stu-id="97f93-151">Get-AzureRmVmDiskEncryptionStatus</span></span>](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) | <span data-ttu-id="97f93-152">Visar hello status hello VM krypteringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="97f93-152">Shows hello status of hello VM encryption process.</span></span> |
| [<span data-ttu-id="97f93-153">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="97f93-153">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="97f93-154">Tar bort en resursgrupp och alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="97f93-154">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="97f93-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="97f93-155">Next steps</span></span>

<span data-ttu-id="97f93-156">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="97f93-156">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="97f93-157">Ytterligare virtuella PowerShell-skript-exempel finns i hello [Virtuella för Windows Azure-dokumentationen](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="97f93-157">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
