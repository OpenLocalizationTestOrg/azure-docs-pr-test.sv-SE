---
title: "aaaSet in Key Vault för virtuella Windows-datorer i Azure Resource Manager | Microsoft Docs"
description: "Hur tooset in Key Vault för användning med en virtuell dator i Azure Resource Manager."
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
ms.openlocfilehash: 53bbe90708202ecfdcf754822d04bf2469631f08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a><span data-ttu-id="13d03-103">Ställ in Key Vault för virtuella datorer i Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="13d03-103">Set up Key Vault for virtual machines in Azure Resource Manager</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

<span data-ttu-id="13d03-104">I Azure Resource Manager-stacken modelleras hemligheter/certifikat som resurser som tillhandahålls av hello resursprovidern för Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="13d03-104">In Azure Resource Manager stack, secrets/certificates are modeled as resources that are provided by hello resource provider of Key Vault.</span></span> <span data-ttu-id="13d03-105">toolearn mer om Key Vault finns [vad är Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="13d03-105">toolearn more about Key Vault, see [What is Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span></span>

> [!NOTE]
> 1. <span data-ttu-id="13d03-106">För Key Vault toobe användas med Azure Resource Manager virtuella datorer, hello **EnabledForDeployment** egenskapen i Nyckelvalvet måste anges tootrue.</span><span class="sxs-lookup"><span data-stu-id="13d03-106">In order for Key Vault toobe used with Azure Resource Manager virtual machines, hello **EnabledForDeployment** property on Key Vault must be set tootrue.</span></span> <span data-ttu-id="13d03-107">Du kan göra detta i olika klienter.</span><span class="sxs-lookup"><span data-stu-id="13d03-107">You can do this in various clients.</span></span>
> 2. <span data-ttu-id="13d03-108">hello Key Vault behov toobe skapas i hello samma prenumeration och plats som hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="13d03-108">hello Key Vault needs toobe created in hello same subscription and location as hello Virtual Machine.</span></span>
>
>

## <a name="use-powershell-tooset-up-key-vault"></a><span data-ttu-id="13d03-109">Använd PowerShell tooset in Key Vault</span><span class="sxs-lookup"><span data-stu-id="13d03-109">Use PowerShell tooset up Key Vault</span></span>
<span data-ttu-id="13d03-110">toocreate nyckelvalv med hjälp av PowerShell, se [Kom igång med Azure Key Vault](../../key-vault/key-vault-get-started.md#vault).</span><span class="sxs-lookup"><span data-stu-id="13d03-110">toocreate a key vault by using PowerShell, see [Get started with Azure Key Vault](../../key-vault/key-vault-get-started.md#vault).</span></span>

<span data-ttu-id="13d03-111">Du kan använda följande PowerShell-cmdlet för nytt nyckelvalv:</span><span class="sxs-lookup"><span data-stu-id="13d03-111">For new key vaults, you can use this PowerShell cmdlet:</span></span>

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

<span data-ttu-id="13d03-112">Du kan använda följande PowerShell-cmdlet för befintliga nyckelvalv:</span><span class="sxs-lookup"><span data-stu-id="13d03-112">For existing key vaults, you can use this PowerShell cmdlet:</span></span>

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="us-cli-tooset-up-key-vault"></a><span data-ttu-id="13d03-113">Oss CLI tooset in Key Vault</span><span class="sxs-lookup"><span data-stu-id="13d03-113">Us CLI tooset up Key Vault</span></span>
<span data-ttu-id="13d03-114">toocreate nyckelvalv med hjälp av hello-kommandoradsgränssnittet (CLI), se [hantera Key Vault med hjälp av CLI](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).</span><span class="sxs-lookup"><span data-stu-id="13d03-114">toocreate a key vault by using hello command-line interface (CLI), see [Manage Key Vault using CLI](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).</span></span>

<span data-ttu-id="13d03-115">CLI har du toocreate hello nyckelvalv innan du tilldelar hello princip för programdistribution.</span><span class="sxs-lookup"><span data-stu-id="13d03-115">For CLI, you have toocreate hello key vault before you assign hello deployment policy.</span></span> <span data-ttu-id="13d03-116">Du kan göra detta med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="13d03-116">You can do this by using hello following command:</span></span>

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-tooset-up-key-vault"></a><span data-ttu-id="13d03-117">Använda mallar tooset in Key Vault</span><span class="sxs-lookup"><span data-stu-id="13d03-117">Use templates tooset up Key Vault</span></span>
<span data-ttu-id="13d03-118">När du använder en mall måste tooset hello `enabledForDeployment` egenskapen för`true` för hello Key Vault-resurs.</span><span class="sxs-lookup"><span data-stu-id="13d03-118">While you use a template, you need tooset hello `enabledForDeployment` property too`true` for hello Key Vault resource.</span></span>

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

<span data-ttu-id="13d03-119">Andra alternativ som du kan konfigurera när du skapar ett nyckelvalv med hjälp av mallar, se [skapa ett nyckelvalv](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span><span class="sxs-lookup"><span data-stu-id="13d03-119">For other options that you can configure when you create a key vault by using templates, see [Create a key vault](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span></span>
