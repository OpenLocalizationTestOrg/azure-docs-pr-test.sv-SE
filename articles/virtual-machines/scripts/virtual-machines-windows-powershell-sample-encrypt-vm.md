---
title: Azure PowerShell-skript Sample - kryptera Windows VM | Microsoft Docs
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
ms.openlocfilehash: 9279fea482fcd8716bcd996985e10f500a4775ce
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="encrypt-a-windows-virtual-machine-with-azure-powershell"></a><span data-ttu-id="ca7ba-103">Kryptera en virtuell dator i Windows med Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="ca7ba-103">Encrypt a Windows virtual machine with Azure PowerShell</span></span>

<span data-ttu-id="ca7ba-104">Det här skriptet skapar en säker Azure Key Vault, krypteringsnycklar, Azure Active Directory-tjänstens huvudnamn och ett Windows-dator (VM).</span><span class="sxs-lookup"><span data-stu-id="ca7ba-104">This script creates a secure Azure Key Vault, encryption keys, Azure Active Directory service principal, and a Windows virtual machine (VM).</span></span> <span data-ttu-id="ca7ba-105">Den virtuella datorn krypteras sedan med hjälp av krypteringsnyckeln från Nyckelvalvet och tjänstens huvudnamn autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-105">The VM is then encrypted using the encryption key from Key Vault and service principal credentials.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="ca7ba-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="ca7ba-106">Sample script</span></span>

<span data-ttu-id="ca7ba-107">[!code-powershell[huvudsakliga](../../../powershell_scripts/virtual-machine/encrypt-vm/encrypt-windows-vm.ps1 "kryptera Virtuella diskar")]</span><span class="sxs-lookup"><span data-stu-id="ca7ba-107">[!code-powershell[main](../../../powershell_scripts/virtual-machine/encrypt-vm/encrypt-windows-vm.ps1 "Encrypt VM disks")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="ca7ba-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="ca7ba-108">Clean up deployment</span></span> 

<span data-ttu-id="ca7ba-109">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="ca7ba-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="ca7ba-110">Script explanation</span></span>

<span data-ttu-id="ca7ba-111">Det här skriptet använder följande kommandon för att skapa distributionen.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-111">This script uses the following commands to create the deployment.</span></span> <span data-ttu-id="ca7ba-112">Varje objekt i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-112">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="ca7ba-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="ca7ba-113">Command</span></span> | <span data-ttu-id="ca7ba-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="ca7ba-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="ca7ba-115">Ny AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="ca7ba-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="ca7ba-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="ca7ba-117">Ny AzureRmKeyVault</span><span class="sxs-lookup"><span data-stu-id="ca7ba-117">New-AzureRmKeyVault</span></span>](/powershell/module/azurerm.keyvault/new-azurermkeyvault) | <span data-ttu-id="ca7ba-118">Skapar ett Azure Key Vault för lagring av säkra data, till exempel krypteringsnycklar.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-118">Creates an Azure Key Vault to store secure data such as encryption keys.</span></span> |
| [<span data-ttu-id="ca7ba-119">Lägg till AzureKeyVaultKey</span><span class="sxs-lookup"><span data-stu-id="ca7ba-119">Add-AzureKeyVaultKey</span></span>](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey) | <span data-ttu-id="ca7ba-120">Skapar en krypteringsnyckel i Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-120">Creates an encryption key in Key Vault.</span></span> |
| [<span data-ttu-id="ca7ba-121">Ny AzureRmADServicePrincipal</span><span class="sxs-lookup"><span data-stu-id="ca7ba-121">New-AzureRmADServicePrincipal</span></span>](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) | <span data-ttu-id="ca7ba-122">Skapar ett Azure Active Directory-tjänstens huvudnamn för att autentisera och kontrollera åtkomst till krypteringsnycklarna på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-122">Creates an Azure Active Directory service principal to securely authenticate and control access to encryption keys.</span></span> |
| [<span data-ttu-id="ca7ba-123">Ange AzureRmKeyVaultAccessPolicy</span><span class="sxs-lookup"><span data-stu-id="ca7ba-123">Set-AzureRmKeyVaultAccessPolicy</span></span>](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) | <span data-ttu-id="ca7ba-124">Anger behörigheter på Nyckelvalvet att ge service principal åtkomst till krypteringsnycklarna.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-124">Sets permissions on the Key Vault to grant the service principal access to encryption keys.</span></span> |
| [<span data-ttu-id="ca7ba-125">Ny AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="ca7ba-125">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="ca7ba-126">Skapar en konfiguration av undernät.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-126">Creates a subnet configuration.</span></span> <span data-ttu-id="ca7ba-127">Den här konfigurationen används med processen virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-127">This configuration is used with the virtual network creation process.</span></span> |
| [<span data-ttu-id="ca7ba-128">Ny AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="ca7ba-128">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="ca7ba-129">Skapar ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-129">Creates a virtual network.</span></span> |
| [<span data-ttu-id="ca7ba-130">Ny AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="ca7ba-130">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="ca7ba-131">Skapar en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-131">Creates a public IP address.</span></span> |
| [<span data-ttu-id="ca7ba-132">Ny AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="ca7ba-132">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="ca7ba-133">Skapar en grupp regeln nätverkssäkerhetskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-133">Creates a network security group rule configuration.</span></span> <span data-ttu-id="ca7ba-134">Den här konfigurationen används för att skapa en regel för NSG när NSG: N har skapats.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-134">This configuration is used to create an NSG rule when the NSG is created.</span></span> |
| [<span data-ttu-id="ca7ba-135">Ny AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="ca7ba-135">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="ca7ba-136">Skapar en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-136">Creates a network security group.</span></span> |
| [<span data-ttu-id="ca7ba-137">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="ca7ba-137">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="ca7ba-138">Hämtar information om undernät.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-138">Gets subnet information.</span></span> <span data-ttu-id="ca7ba-139">Den här informationen används när du skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-139">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="ca7ba-140">Ny AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="ca7ba-140">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="ca7ba-141">Skapar ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-141">Creates a network interface.</span></span> |
| [<span data-ttu-id="ca7ba-142">Ny AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="ca7ba-142">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="ca7ba-143">Skapar en VM-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-143">Creates a VM configuration.</span></span> <span data-ttu-id="ca7ba-144">Den här konfigurationen omfattar information som VM-namn, operativsystem och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-144">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="ca7ba-145">Konfigurationen används under Skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-145">The configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="ca7ba-146">Ny AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="ca7ba-146">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="ca7ba-147">Skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-147">Create a virtual machine.</span></span> |
| [<span data-ttu-id="ca7ba-148">Get-AzureRmKeyVault</span><span class="sxs-lookup"><span data-stu-id="ca7ba-148">Get-AzureRmKeyVault</span></span>](/powershell/module/azurerm.keyvault/get-azurermkeyvault) | <span data-ttu-id="ca7ba-149">Hämtar nödvändig information för Nyckelvalvet</span><span class="sxs-lookup"><span data-stu-id="ca7ba-149">Gets required information on the Key Vault</span></span> |
| [<span data-ttu-id="ca7ba-150">Ange AzureRmVMDiskEncryptionExtension</span><span class="sxs-lookup"><span data-stu-id="ca7ba-150">Set-AzureRmVMDiskEncryptionExtension</span></span>](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) | <span data-ttu-id="ca7ba-151">Aktiverar kryptering på en virtuell dator med hjälp av service principal autentiseringsuppgifter och krypteringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-151">Enables encryption on a VM using the service principal credentials and encryption key.</span></span> |
| [<span data-ttu-id="ca7ba-152">Get-AzureRmVmDiskEncryptionStatus</span><span class="sxs-lookup"><span data-stu-id="ca7ba-152">Get-AzureRmVmDiskEncryptionStatus</span></span>](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) | <span data-ttu-id="ca7ba-153">Visar status för krypteringsprocessen VM.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-153">Shows the status of the VM encryption process.</span></span> |
| [<span data-ttu-id="ca7ba-154">Ta bort AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="ca7ba-154">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="ca7ba-155">Tar bort en resursgrupp och alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="ca7ba-155">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ca7ba-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ca7ba-156">Next steps</span></span>

<span data-ttu-id="ca7ba-157">Mer information om Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ca7ba-157">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="ca7ba-158">Ytterligare virtuella PowerShell-skript-exempel finns i den [Virtuella för Windows Azure-dokumentationen](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ca7ba-158">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
