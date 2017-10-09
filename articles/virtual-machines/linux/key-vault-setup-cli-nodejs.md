---
title: "aaaSet in Key Vault för Linux virtuella datorer med hello Azure CLI 1.0 | Microsoft Docs"
description: "Hur hello tooset in Key Vault för användning med en virtuell dator i Azure Resource Manager med Azure CLI 1.0."
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: bccdd5ab-5ccf-4760-9039-92c6eafb15bd
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/24/2017
ms.author: singhkay
ms.openlocfilehash: 275022e4e7e26d7363784c289dd7512047c07bad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager-with-hello-azure-cli-10"></a><span data-ttu-id="b4faa-103">Ställ in Key Vault för virtuella datorer i Azure Resource Manager med hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="b4faa-103">Set up Key Vault for virtual machines in Azure Resource Manager with hello Azure CLI 1.0</span></span>
<span data-ttu-id="b4faa-104">I hello Azure Resource Manager-stacken modelleras hemligheter/certifikat som resurser som tillhandahålls av hello resursprovidern för Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="b4faa-104">In hello Azure Resource Manager stack, secrets/certificates are modeled as resources that are provided by hello resource provider of Key Vault.</span></span> <span data-ttu-id="b4faa-105">toolearn mer om Azure Key Vault finns [vad är Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="b4faa-105">toolearn more about Azure Key Vault, see [What is Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span></span> <span data-ttu-id="b4faa-106">För Key Vault toobe användas med Azure Resource Manager virtuella datorer, hello *EnabledForDeployment* egenskapen i Nyckelvalvet måste anges tootrue.</span><span class="sxs-lookup"><span data-stu-id="b4faa-106">In order for Key Vault toobe used with Azure Resource Manager virtual machines, hello *EnabledForDeployment* property on Key Vault must be set tootrue.</span></span> <span data-ttu-id="b4faa-107">Du kan göra detta i olika klienter.</span><span class="sxs-lookup"><span data-stu-id="b4faa-107">You can do this in various clients.</span></span> <span data-ttu-id="b4faa-108">Den här artikeln beskrivs hur du tooset in Key Vault för användning med Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="b4faa-108">This article shows you how tooset up Key Vault for use with Azure Virtual Machines.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="b4faa-109">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="b4faa-109">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="b4faa-110">Du kan göra hello med hjälp av något av följande versioner av CLI hello</span><span class="sxs-lookup"><span data-stu-id="b4faa-110">You can complete hello task using one of hello following CLI versions</span></span>

- <span data-ttu-id="b4faa-111">[Azure CLI 1.0](#quick-commands) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="b4faa-111">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="b4faa-112">[Azure CLI 2.0](../windows/key-vault-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="b4faa-112">[Azure CLI 2.0](../windows/key-vault-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="use-cli-10-tooset-up-key-vault"></a><span data-ttu-id="b4faa-113">Använda CLI 1.0 tooset in Key Vault</span><span class="sxs-lookup"><span data-stu-id="b4faa-113">Use CLI 1.0 tooset up Key Vault</span></span>
<span data-ttu-id="b4faa-114">toocreate nyckelvalv med hjälp av hello-kommandoradsgränssnittet (CLI), se [hantera Key Vault med hjälp av CLI](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).</span><span class="sxs-lookup"><span data-stu-id="b4faa-114">toocreate a key vault by using hello command-line interface (CLI), see [Manage Key Vault using CLI](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).</span></span>

<span data-ttu-id="b4faa-115">CLI version 1.0 har du toocreate hello nyckelvalv innan du tilldelar hello princip för programdistribution.</span><span class="sxs-lookup"><span data-stu-id="b4faa-115">For CLI 1.0, you have toocreate hello key vault before you assign hello deployment policy.</span></span> <span data-ttu-id="b4faa-116">Du kan sedan tilldela hello-princip med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="b4faa-116">You can then assign hello policy by using hello following command:</span></span>

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-tooset-up-key-vault"></a><span data-ttu-id="b4faa-117">Använda mallar tooset in Key Vault</span><span class="sxs-lookup"><span data-stu-id="b4faa-117">Use templates tooset up Key Vault</span></span>
<span data-ttu-id="b4faa-118">När du använder en mall måste tooset hello `enabledForDeployment` egenskapen för`true` för hello Key Vault-resurs.</span><span class="sxs-lookup"><span data-stu-id="b4faa-118">When you use a template, you need tooset hello `enabledForDeployment` property too`true` for hello Key Vault resource.</span></span>

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

<span data-ttu-id="b4faa-119">Andra alternativ som du kan konfigurera när du skapar ett nyckelvalv med hjälp av mallar, se [skapa ett nyckelvalv](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span><span class="sxs-lookup"><span data-stu-id="b4faa-119">For other options that you can configure when you create a key vault by using templates, see [Create a key vault](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span></span>
