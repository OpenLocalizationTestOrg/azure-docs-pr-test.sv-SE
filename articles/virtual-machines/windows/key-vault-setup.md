---
title: "Ställ in nyckeln valvet för virtuella Windows-datorer i Azure Resource Manager | Microsoft Docs"
description: "Hur du ställer in Key Vault för användning med en virtuell dator i Azure Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 33a483e2-cfbc-4c62-a588-5d9fd52491e2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.author: kasing
ms.openlocfilehash: a5083a5216efbfd76fd912ec48c2f0ec3b30c4a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a><span data-ttu-id="74579-103">Ställ in Key Vault för virtuella datorer i Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="74579-103">Set up Key Vault for virtual machines in Azure Resource Manager</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

<span data-ttu-id="74579-104">I Azure Resource Manager-stacken modelleras hemligheter/certifikat som resurser som tillhandahålls av Key Vault-resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="74579-104">In Azure Resource Manager stack, secrets/certificates are modeled as resources that are provided by the resource provider of Key Vault.</span></span> <span data-ttu-id="74579-105">Mer information om Key Vault finns [vad är Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="74579-105">To learn more about Key Vault, see [What is Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span></span>

> [!NOTE]
> 1. <span data-ttu-id="74579-106">För Key Vault som ska användas med Azure Resource Manager virtuella datorer i **EnabledForDeployment** egenskapen i Nyckelvalvet måste anges till true.</span><span class="sxs-lookup"><span data-stu-id="74579-106">In order for Key Vault to be used with Azure Resource Manager virtual machines, the **EnabledForDeployment** property on Key Vault must be set to true.</span></span> <span data-ttu-id="74579-107">Du kan göra detta i olika klienter.</span><span class="sxs-lookup"><span data-stu-id="74579-107">You can do this in various clients.</span></span>
> 2. <span data-ttu-id="74579-108">Key Vault måste skapas i samma prenumeration och plats som den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="74579-108">The Key Vault needs to be created in the same subscription and location as the Virtual Machine.</span></span>
>
>

## <a name="use-powershell-to-set-up-key-vault"></a><span data-ttu-id="74579-109">Använd PowerShell för att ställa in Key Vault</span><span class="sxs-lookup"><span data-stu-id="74579-109">Use PowerShell to set up Key Vault</span></span>
<span data-ttu-id="74579-110">För att skapa ett nyckelvalv med hjälp av PowerShell Se [Kom igång med Azure Key Vault](../../key-vault/key-vault-get-started.md#vault).</span><span class="sxs-lookup"><span data-stu-id="74579-110">To create a key vault by using PowerShell, see [Get started with Azure Key Vault](../../key-vault/key-vault-get-started.md#vault).</span></span>

<span data-ttu-id="74579-111">Du kan använda följande PowerShell-cmdlet för nytt nyckelvalv:</span><span class="sxs-lookup"><span data-stu-id="74579-111">For new key vaults, you can use this PowerShell cmdlet:</span></span>

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

<span data-ttu-id="74579-112">Du kan använda följande PowerShell-cmdlet för befintliga nyckelvalv:</span><span class="sxs-lookup"><span data-stu-id="74579-112">For existing key vaults, you can use this PowerShell cmdlet:</span></span>

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="us-cli-to-set-up-key-vault"></a><span data-ttu-id="74579-113">Oss CLI för att ställa in Key Vault</span><span class="sxs-lookup"><span data-stu-id="74579-113">Us CLI to set up Key Vault</span></span>
<span data-ttu-id="74579-114">För att skapa ett nyckelvalv med hjälp av kommandoradsgränssnittet (CLI), se [hantera Key Vault med hjälp av CLI](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).</span><span class="sxs-lookup"><span data-stu-id="74579-114">To create a key vault by using the command-line interface (CLI), see [Manage Key Vault using CLI](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).</span></span>

<span data-ttu-id="74579-115">Du måste skapa nyckelvalvet innan du tilldelar princip för programdistribution för CLI.</span><span class="sxs-lookup"><span data-stu-id="74579-115">For CLI, you have to create the key vault before you assign the deployment policy.</span></span> <span data-ttu-id="74579-116">Det kan du göra med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="74579-116">You can do this by using the following command:</span></span>

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a><span data-ttu-id="74579-117">Använda mallar för att ställa in Key Vault</span><span class="sxs-lookup"><span data-stu-id="74579-117">Use templates to set up Key Vault</span></span>
<span data-ttu-id="74579-118">När du använder en mall måste du ange den `enabledForDeployment` egenskapen `true` för Key Vault-resurs.</span><span class="sxs-lookup"><span data-stu-id="74579-118">While you use a template, you need to set the `enabledForDeployment` property to `true` for the Key Vault resource.</span></span>

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

<span data-ttu-id="74579-119">Andra alternativ som du kan konfigurera när du skapar ett nyckelvalv med hjälp av mallar, se [skapa ett nyckelvalv](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span><span class="sxs-lookup"><span data-stu-id="74579-119">For other options that you can configure when you create a key vault by using templates, see [Create a key vault](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span></span>
