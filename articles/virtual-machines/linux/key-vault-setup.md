---
title: "Konfigurera Azure Key Vault för virtuella Linux-datorer | Microsoft Docs"
description: "Hur du ställer in Key Vault för användning med en virtuell dator i Azure Resource Manager med CLI 2.0."
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: bccdd5ab-5ccf-4760-9039-92c6eafb15bd
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 02/24/2017
ms.author: singhkay
ms.openlocfilehash: 2cc9b4c978e9a4deb0c8443c4b0f9e301a7cf492
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-set-up-key-vault-for-virtual-machines-with-the-azure-cli-20"></a><span data-ttu-id="b853e-103">Hur du ställer in Key Vault för virtuella datorer med Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b853e-103">How to set up Key Vault for virtual machines with the Azure CLI 2.0</span></span>

<span data-ttu-id="b853e-104">I Azure Resource Manager-stacken modelleras hemligheter/certifikat som resurser som tillhandahålls av Key Vault.</span><span class="sxs-lookup"><span data-stu-id="b853e-104">In the Azure Resource Manager stack, secrets/certificates are modeled as resources that are provided by Key Vault.</span></span> <span data-ttu-id="b853e-105">Mer information om Azure Key Vault finns [vad är Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="b853e-105">To learn more about Azure Key Vault, see [What is Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span></span> <span data-ttu-id="b853e-106">För Key Vault som ska användas med Azure Resource Manager VMs den *EnabledForDeployment* egenskapen i Nyckelvalvet måste anges till true.</span><span class="sxs-lookup"><span data-stu-id="b853e-106">In order for Key Vault to be used with Azure Resource Manager VMs, the *EnabledForDeployment* property on Key Vault must be set to true.</span></span> <span data-ttu-id="b853e-107">Den här artikeln visar hur du ställer in Key Vault för användning med virtuella Azure-datorer (VM) med hjälp av Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="b853e-107">This article shows you how to set up Key Vault for use with Azure virtual machines (VMs) using the Azure CLI 2.0.</span></span> <span data-ttu-id="b853e-108">Du kan också utföra dessa steg med [Azure CLI 1.0](key-vault-setup-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b853e-108">You can also perform these steps with the [Azure CLI 1.0](key-vault-setup-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="b853e-109">Om du vill utföra dessa steg behöver du senast [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och inloggad till en Azure-konto med hjälp av [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="b853e-109">To perform these steps, you need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

## <a name="create-a-key-vault"></a><span data-ttu-id="b853e-110">Skapa en Key Vault-lösning</span><span class="sxs-lookup"><span data-stu-id="b853e-110">Create a Key Vault</span></span>
<span data-ttu-id="b853e-111">Skapa en nyckelvalvet och tilldela princip för programdistribution med [az keyvault skapa](/cli/azure/keyvault#create).</span><span class="sxs-lookup"><span data-stu-id="b853e-111">Create a key vault and assign the deployment policy with [az keyvault create](/cli/azure/keyvault#create).</span></span> <span data-ttu-id="b853e-112">I följande exempel skapas ett nyckelvalv med namnet `myKeyVault` i den `myResourceGroup` resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="b853e-112">The following example creates a key vault named `myKeyVault` in the `myResourceGroup` resource group:</span></span>

```azurecli
az keyvault create -l westus -n myKeyVault -g myResourceGroup --enabled-for-deployment true
```

## <a name="update-a-key-vault-for-use-with-vms"></a><span data-ttu-id="b853e-113">Uppdatera ett Nyckelvalv för användning med virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="b853e-113">Update a Key Vault for use with VMs</span></span>
<span data-ttu-id="b853e-114">Ange princip för programdistribution på en befintlig nyckel valvet med [az keyvault uppdatering](/cli/azure/keyvault#update).</span><span class="sxs-lookup"><span data-stu-id="b853e-114">Set the deployment policy on an existing key vault with [az keyvault update](/cli/azure/keyvault#update).</span></span> <span data-ttu-id="b853e-115">Följande uppdateringar nyckelvalvet med namnet `myKeyVault` i den `myResourceGroup` resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="b853e-115">The following updates the key vault named `myKeyVault` in the `myResourceGroup` resource group:</span></span>

```azurecli
az keyvault update -n myKeyVault -g myResourceGroup --set properties.enabledForDeployment=true
```

## <a name="use-templates-to-set-up-key-vault"></a><span data-ttu-id="b853e-116">Använda mallar för att ställa in Key Vault</span><span class="sxs-lookup"><span data-stu-id="b853e-116">Use templates to set up Key Vault</span></span>
<span data-ttu-id="b853e-117">När du använder en mall måste du ange den `enabledForDeployment` egenskapen `true` för nyckeln valvet resurs på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b853e-117">When you use a template, you need to set the `enabledForDeployment` property to `true` for the Key Vault resource as follows:</span></span>

```json
{
    "type": "Microsoft.KeyVault/vaults",
    "name": "ContosoKeyVault",
    "apiVersion": "2015-06-01",
    "location": "<location-of-key-vault>",
    "properties": {
    "enabledForDeployment": "true",
    ....
    ....
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="b853e-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b853e-118">Next steps</span></span>
<span data-ttu-id="b853e-119">Andra alternativ som du kan konfigurera när du skapar ett Nyckelvalv med hjälp av mallar, se [skapa ett nyckelvalv](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span><span class="sxs-lookup"><span data-stu-id="b853e-119">For other options that you can configure when you create a Key Vault by using templates, see [Create a key vault](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span></span>
