---
title: "aaaSet in Azure Key Vault för virtuella Linux-datorer | Microsoft Docs"
description: "Hur hello tooset in Key Vault för användning med en virtuell dator i Azure Resource Manager med CLI 2.0."
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
ms.openlocfilehash: a5dc1fbe59a71b4456ba5b9bbacdb90440064757
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooset-up-key-vault-for-virtual-machines-with-hello-azure-cli-20"></a><span data-ttu-id="d3e91-103">Hur tooset in Key Vault för virtuella datorer med hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="d3e91-103">How tooset up Key Vault for virtual machines with hello Azure CLI 2.0</span></span>

<span data-ttu-id="d3e91-104">I hello Azure Resource Manager-stacken modelleras hemligheter/certifikat som resurser som tillhandahålls av Key Vault.</span><span class="sxs-lookup"><span data-stu-id="d3e91-104">In hello Azure Resource Manager stack, secrets/certificates are modeled as resources that are provided by Key Vault.</span></span> <span data-ttu-id="d3e91-105">toolearn mer om Azure Key Vault finns [vad är Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="d3e91-105">toolearn more about Azure Key Vault, see [What is Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span></span> <span data-ttu-id="d3e91-106">För Key Vault toobe används med Azure Resource Manager VMs hello *EnabledForDeployment* egenskapen i Nyckelvalvet måste anges tootrue.</span><span class="sxs-lookup"><span data-stu-id="d3e91-106">In order for Key Vault toobe used with Azure Resource Manager VMs, hello *EnabledForDeployment* property on Key Vault must be set tootrue.</span></span> <span data-ttu-id="d3e91-107">Den här artikeln lär du dig hur tooset in Key Vault för användning med virtuella Azure-datorer (VM) med hjälp av hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="d3e91-107">This article shows you how tooset up Key Vault for use with Azure virtual machines (VMs) using hello Azure CLI 2.0.</span></span> <span data-ttu-id="d3e91-108">Du kan också utföra dessa steg med hello [Azure CLI 1.0](key-vault-setup-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d3e91-108">You can also perform these steps with hello [Azure CLI 1.0](key-vault-setup-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="d3e91-109">tooperform dessa steg, behöver du hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="d3e91-109">tooperform these steps, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

## <a name="create-a-key-vault"></a><span data-ttu-id="d3e91-110">Skapa en Key Vault-lösning</span><span class="sxs-lookup"><span data-stu-id="d3e91-110">Create a Key Vault</span></span>
<span data-ttu-id="d3e91-111">Skapa en nyckelvalvet och tilldela hello princip för programdistribution med [az keyvault skapa](/cli/azure/keyvault#create).</span><span class="sxs-lookup"><span data-stu-id="d3e91-111">Create a key vault and assign hello deployment policy with [az keyvault create](/cli/azure/keyvault#create).</span></span> <span data-ttu-id="d3e91-112">hello följande exempel skapas ett nyckelvalv med namnet `myKeyVault` i hello `myResourceGroup` resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="d3e91-112">hello following example creates a key vault named `myKeyVault` in hello `myResourceGroup` resource group:</span></span>

```azurecli
az keyvault create -l westus -n myKeyVault -g myResourceGroup --enabled-for-deployment true
```

## <a name="update-a-key-vault-for-use-with-vms"></a><span data-ttu-id="d3e91-113">Uppdatera ett Nyckelvalv för användning med virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="d3e91-113">Update a Key Vault for use with VMs</span></span>
<span data-ttu-id="d3e91-114">Ange princip för hello-distribution på en befintlig nyckelvalv med [az keyvault uppdatering](/cli/azure/keyvault#update).</span><span class="sxs-lookup"><span data-stu-id="d3e91-114">Set hello deployment policy on an existing key vault with [az keyvault update](/cli/azure/keyvault#update).</span></span> <span data-ttu-id="d3e91-115">hello följande uppdateringar hello nyckelvalv med namnet `myKeyVault` i hello `myResourceGroup` resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="d3e91-115">hello following updates hello key vault named `myKeyVault` in hello `myResourceGroup` resource group:</span></span>

```azurecli
az keyvault update -n myKeyVault -g myResourceGroup --set properties.enabledForDeployment=true
```

## <a name="use-templates-tooset-up-key-vault"></a><span data-ttu-id="d3e91-116">Använda mallar tooset in Key Vault</span><span class="sxs-lookup"><span data-stu-id="d3e91-116">Use templates tooset up Key Vault</span></span>
<span data-ttu-id="d3e91-117">När du använder en mall måste tooset hello `enabledForDeployment` egenskapen för`true` för hello Key Vault-resursen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="d3e91-117">When you use a template, you need tooset hello `enabledForDeployment` property too`true` for hello Key Vault resource as follows:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="d3e91-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d3e91-118">Next steps</span></span>
<span data-ttu-id="d3e91-119">Andra alternativ som du kan konfigurera när du skapar ett Nyckelvalv med hjälp av mallar, se [skapa ett nyckelvalv](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span><span class="sxs-lookup"><span data-stu-id="d3e91-119">For other options that you can configure when you create a Key Vault by using templates, see [Create a key vault](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span></span>
