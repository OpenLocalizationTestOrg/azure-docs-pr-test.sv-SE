---
title: "Ställ in Key Vault för Linux virtuella datorer med Azure CLI 1.0 | Microsoft Docs"
description: "Hur du ställer in Key Vault för användning med en virtuell dator i Azure Resource Manager med Azure CLI 1.0."
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
ms.openlocfilehash: fed612a354d45f34619f2a66bd40d78740c43ac7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager-with-the-azure-cli-10"></a><span data-ttu-id="c8d06-103">Ställ in Key Vault för virtuella datorer i Azure Resource Manager med Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="c8d06-103">Set up Key Vault for virtual machines in Azure Resource Manager with the Azure CLI 1.0</span></span>
<span data-ttu-id="c8d06-104">I Azure Resource Manager-stacken modelleras hemligheter/certifikat som resurser som tillhandahålls av Key Vault-resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="c8d06-104">In the Azure Resource Manager stack, secrets/certificates are modeled as resources that are provided by the resource provider of Key Vault.</span></span> <span data-ttu-id="c8d06-105">Mer information om Azure Key Vault finns [vad är Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="c8d06-105">To learn more about Azure Key Vault, see [What is Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span></span> <span data-ttu-id="c8d06-106">För Key Vault som ska användas med Azure Resource Manager virtuella datorer i *EnabledForDeployment* egenskapen i Nyckelvalvet måste anges till true.</span><span class="sxs-lookup"><span data-stu-id="c8d06-106">In order for Key Vault to be used with Azure Resource Manager virtual machines, the *EnabledForDeployment* property on Key Vault must be set to true.</span></span> <span data-ttu-id="c8d06-107">Du kan göra detta i olika klienter.</span><span class="sxs-lookup"><span data-stu-id="c8d06-107">You can do this in various clients.</span></span> <span data-ttu-id="c8d06-108">Den här artikeln visar hur du ställer in Key Vault för användning med Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="c8d06-108">This article shows you how to set up Key Vault for use with Azure Virtual Machines.</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="c8d06-109">CLI-versioner för att slutföra uppgiften</span><span class="sxs-lookup"><span data-stu-id="c8d06-109">CLI versions to complete the task</span></span>
<span data-ttu-id="c8d06-110">Du kan slutföra uppgiften med någon av följande CLI-versioner</span><span class="sxs-lookup"><span data-stu-id="c8d06-110">You can complete the task using one of the following CLI versions</span></span>

- <span data-ttu-id="c8d06-111">[Azure CLI 1.0](#quick-commands) – våra CLI för klassisk och resurs management på distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="c8d06-111">[Azure CLI 1.0](#quick-commands) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="c8d06-112">[Azure CLI 2.0](../windows/key-vault-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – vår nästa generations CLI för distributionsmodellen resurshantering</span><span class="sxs-lookup"><span data-stu-id="c8d06-112">[Azure CLI 2.0](../windows/key-vault-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for the resource management deployment model</span></span>

## <a name="use-cli-10-to-set-up-key-vault"></a><span data-ttu-id="c8d06-113">Använda CLI 1.0 för att ställa in Key Vault</span><span class="sxs-lookup"><span data-stu-id="c8d06-113">Use CLI 1.0 to set up Key Vault</span></span>
<span data-ttu-id="c8d06-114">För att skapa ett nyckelvalv med hjälp av kommandoradsgränssnittet (CLI), se [hantera Key Vault med hjälp av CLI](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).</span><span class="sxs-lookup"><span data-stu-id="c8d06-114">To create a key vault by using the command-line interface (CLI), see [Manage Key Vault using CLI](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).</span></span>

<span data-ttu-id="c8d06-115">Du måste skapa nyckelvalvet innan du tilldelar princip för programdistribution för CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="c8d06-115">For CLI 1.0, you have to create the key vault before you assign the deployment policy.</span></span> <span data-ttu-id="c8d06-116">Du kan sedan tilldela principen med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c8d06-116">You can then assign the policy by using the following command:</span></span>

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a><span data-ttu-id="c8d06-117">Använda mallar för att ställa in Key Vault</span><span class="sxs-lookup"><span data-stu-id="c8d06-117">Use templates to set up Key Vault</span></span>
<span data-ttu-id="c8d06-118">När du använder en mall måste du ange den `enabledForDeployment` egenskapen `true` för Key Vault-resurs.</span><span class="sxs-lookup"><span data-stu-id="c8d06-118">When you use a template, you need to set the `enabledForDeployment` property to `true` for the Key Vault resource.</span></span>

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

<span data-ttu-id="c8d06-119">Andra alternativ som du kan konfigurera när du skapar ett nyckelvalv med hjälp av mallar, se [skapa ett nyckelvalv](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span><span class="sxs-lookup"><span data-stu-id="c8d06-119">For other options that you can configure when you create a key vault by using templates, see [Create a key vault](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span></span>
