---
title: "aaaManage Azure Key Vault med hjälp av Azure Automation | Microsoft Docs"
description: "Läs mer om hur hello Azure Automation-tjänsten kan vara används toomanage Azure Key Vault."
services: Key-Vault, automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: 4e780762-19b6-4ca6-b894-ebb44c538f35
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: magoedte;csand
ms.openlocfilehash: 7f46ecc1206a96e8aeb1d086285461cb5b205472
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-key-vault-using-azure-automation"></a><span data-ttu-id="3a730-103">Hantera Azure Key Vault med hjälp av Azure Automation</span><span class="sxs-lookup"><span data-stu-id="3a730-103">Managing Azure Key Vault using Azure Automation</span></span>
<span data-ttu-id="3a730-104">Den här guiden beskrivs toohello Azure Automation-tjänsten och hur det kan vara används toosimplify hanteringen av dina nycklar och hemligheter i Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="3a730-104">This guide will introduce you toohello Azure Automation service and how it can be used toosimplify management of your keys and secrets in Azure Key Vault.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="3a730-105">Vad är Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="3a730-105">What is Azure Automation?</span></span>
<span data-ttu-id="3a730-106">[Azure Automation](../automation/automation-intro.md) är en Azure-tjänst som förenklar molnhantering via automatisering och önskad tillståndskonfiguration.</span><span class="sxs-lookup"><span data-stu-id="3a730-106">[Azure Automation](../automation/automation-intro.md) is an Azure service for simplifying cloud management through process automation and desired state configuration.</span></span> <span data-ttu-id="3a730-107">Med Azure Automation kan manuell upprepade, tidskrävande och felbenägna aktiviteter vara automatisk tooincrease tillförlitlighet, effektivitet och tid toovalue för din organisation.</span><span class="sxs-lookup"><span data-stu-id="3a730-107">Using Azure Automation, manual, repeated, long-running, and error-prone tasks can be automated tooincrease reliability, efficiency, and time toovalue for your organization.</span></span>

<span data-ttu-id="3a730-108">Azure Automation ger en Körningsmotor för arbetsflöden för mycket tillförlitliga, hög tillgänglighet som kan skalas toomeet dina behov.</span><span class="sxs-lookup"><span data-stu-id="3a730-108">Azure Automation provides a highly-reliable, highly-available workflow execution engine that scales toomeet your needs.</span></span> <span data-ttu-id="3a730-109">I Azure Automation kan processer vara inletts manuellt av 3 part eller med schemalagda intervall så att aktiviteter inträffa exakt vid behov.</span><span class="sxs-lookup"><span data-stu-id="3a730-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="3a730-110">Minska operativ tillsyn och frigör IT och DevOps personal toofocus på arbete som lägger till affärsvärde genom att flytta dina molntjänster management uppgifter toobe körs automatiskt av Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="3a730-110">Reduce operational overhead and free up IT and DevOps staff toofocus on work that adds business value by moving your cloud management tasks toobe run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-key-vault"></a><span data-ttu-id="3a730-111">Hur Azure Automation kan hantera Azure Key Vault?</span><span class="sxs-lookup"><span data-stu-id="3a730-111">How can Azure Automation help manage Azure Key Vault?</span></span>
<span data-ttu-id="3a730-112">Key Vault kan hanteras i Azure Automation med hjälp av hello [AzureRM Key Vault-cmdlets](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) och [Azure klassiska Key Vault-cmdlets](https://msdn.microsoft.com/library/azure/dn868052.aspx).</span><span class="sxs-lookup"><span data-stu-id="3a730-112">Key Vault can be managed in Azure Automation by using hello [AzureRM Key Vault cmdlets](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) and [Azure Classic Key Vault cmdlets](https://msdn.microsoft.com/library/azure/dn868052.aspx).</span></span> <span data-ttu-id="3a730-113">hello Azure-modulen för hantering av den klassiska Key Vault finns automatiskt i Azure Automation och du kan importera hello [AzureRM KeyVault modulen](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) i Azure Automation så att du kan utföra många av din Key Vault-hantering uppgifter i hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3a730-113">hello Azure module for managing classic Key Vault is available automatically in Azure Automation, and you can import hello [AzureRM-KeyVault module](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) into Azure Automation, so that you can perform many of your Key Vault management tasks within hello service.</span></span> <span data-ttu-id="3a730-114">Du kan också koppla dessa cmdlets i Azure Automation med hello-cmdlets för andra Azure-tjänster, tooautomate komplicerade uppgifter över Azure-tjänster och 3 part.</span><span class="sxs-lookup"><span data-stu-id="3a730-114">You can also pair these cmdlets in Azure Automation with hello cmdlets for other Azure services, tooautomate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="3a730-115">Du kan utföra dessa uppgifter, bland annat med hello Azure Key Vault-cmdlets:</span><span class="sxs-lookup"><span data-stu-id="3a730-115">With hello Azure Key Vault cmdlets you can perform these tasks among others:</span></span> 

* <span data-ttu-id="3a730-116">Skapa och konfigurera ett nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="3a730-116">Create and configure a key vault</span></span>
* <span data-ttu-id="3a730-117">Skapa eller importera en nyckel</span><span class="sxs-lookup"><span data-stu-id="3a730-117">Create or import a key</span></span>
* <span data-ttu-id="3a730-118">Skapa eller uppdatera en hemlighet</span><span class="sxs-lookup"><span data-stu-id="3a730-118">Create or update a secret</span></span>
* <span data-ttu-id="3a730-119">Uppdatera attribut för en nyckel</span><span class="sxs-lookup"><span data-stu-id="3a730-119">Update attributes of a key</span></span>
* <span data-ttu-id="3a730-120">Hämta en nyckel eller hemlighet.</span><span class="sxs-lookup"><span data-stu-id="3a730-120">Get a key or secret</span></span>
* <span data-ttu-id="3a730-121">Ta bort en nyckel eller hemlighet.</span><span class="sxs-lookup"><span data-stu-id="3a730-121">Delete a key or secret</span></span>

<span data-ttu-id="3a730-122">Här följer några exempel på hur du använder PowerShell toomanage Key Vault:</span><span class="sxs-lookup"><span data-stu-id="3a730-122">Here are some examples of using PowerShell toomanage Key Vault:</span></span>  

* [<span data-ttu-id="3a730-123">Azure Key Vault - steg-för-steg</span><span class="sxs-lookup"><span data-stu-id="3a730-123">Azure Key Vault - Step by Step</span></span>](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step)
* [<span data-ttu-id="3a730-124">Installera och konfigurera en Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="3a730-124">Setting Up and Configuring an Azure Key Vault</span></span>](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault)

## <a name="next-steps"></a><span data-ttu-id="3a730-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3a730-125">Next steps</span></span>
<span data-ttu-id="3a730-126">Nu när du har lärt dig grunderna hello i Azure Automation och hur det kan vara används toomanage Azure Key Vault kan du följa dessa länkar toolearn mer om Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="3a730-126">Now that you've learned hello basics of Azure Automation and how it can be used toomanage Azure Key Vault, follow these links toolearn more about Azure Automation.</span></span>

* <span data-ttu-id="3a730-127">Se hello Azure Automation [komma igång-Självstudier](../automation/automation-first-runbook-graphical.md).</span><span class="sxs-lookup"><span data-stu-id="3a730-127">See hello Azure Automation [Getting Started Tutorial](../automation/automation-first-runbook-graphical.md).</span></span>
* <span data-ttu-id="3a730-128">Se hello [Azure Key Vault PowerShell-skript](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).</span><span class="sxs-lookup"><span data-stu-id="3a730-128">See hello [Azure Key Vault PowerShell scripts](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).</span></span>

