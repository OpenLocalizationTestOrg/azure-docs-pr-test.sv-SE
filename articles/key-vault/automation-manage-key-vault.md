---
title: "Hantera Azure Key Vault med hjälp av Azure Automation | Microsoft Docs"
description: "Läs mer om hur Azure Automation-tjänsten kan användas för att hantera Azure Key Vault."
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
ms.openlocfilehash: dee39662472fe54776b591977f2b1ecb39d15b00
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="managing-azure-key-vault-using-azure-automation"></a><span data-ttu-id="12fff-103">Hantera Azure Key Vault med hjälp av Azure Automation</span><span class="sxs-lookup"><span data-stu-id="12fff-103">Managing Azure Key Vault using Azure Automation</span></span>
<span data-ttu-id="12fff-104">Den här guiden innehåller en introduktion till Azure Automation-tjänsten och hur det kan användas för att förenkla hanteringen av dina nycklar och hemligheter i Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="12fff-104">This guide will introduce you to the Azure Automation service and how it can be used to simplify management of your keys and secrets in Azure Key Vault.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="12fff-105">Vad är Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="12fff-105">What is Azure Automation?</span></span>
<span data-ttu-id="12fff-106">[Azure Automation](../automation/automation-intro.md) är en Azure-tjänst som förenklar molnhantering via automatisering och önskad tillståndskonfiguration.</span><span class="sxs-lookup"><span data-stu-id="12fff-106">[Azure Automation](../automation/automation-intro.md) is an Azure service for simplifying cloud management through process automation and desired state configuration.</span></span> <span data-ttu-id="12fff-107">Med hjälp av Azure Automation, manuell, kan upprepas tidskrävande och felbenägna aktiviteter automatiseras för att öka tillförlitligheten, effektivitet och tid för värde för din organisation.</span><span class="sxs-lookup"><span data-stu-id="12fff-107">Using Azure Automation, manual, repeated, long-running, and error-prone tasks can be automated to increase reliability, efficiency, and time to value for your organization.</span></span>

<span data-ttu-id="12fff-108">Azure Automation ger en Körningsmotor för arbetsflöden för mycket tillförlitliga, hög tillgänglighet som kan skalas för att uppfylla dina behov.</span><span class="sxs-lookup"><span data-stu-id="12fff-108">Azure Automation provides a highly-reliable, highly-available workflow execution engine that scales to meet your needs.</span></span> <span data-ttu-id="12fff-109">I Azure Automation kan processer vara inletts manuellt av 3 part eller med schemalagda intervall så att aktiviteter inträffa exakt vid behov.</span><span class="sxs-lookup"><span data-stu-id="12fff-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="12fff-110">Minska operativ tillsyn och frigör IT och DevOps-personal att fokusera på arbete som lägger till affärsvärde genom att flytta dina uppgifter för hantering av molnet att köras automatiskt av Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="12fff-110">Reduce operational overhead and free up IT and DevOps staff to focus on work that adds business value by moving your cloud management tasks to be run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-key-vault"></a><span data-ttu-id="12fff-111">Hur Azure Automation kan hantera Azure Key Vault?</span><span class="sxs-lookup"><span data-stu-id="12fff-111">How can Azure Automation help manage Azure Key Vault?</span></span>
<span data-ttu-id="12fff-112">Key Vault kan hanteras i Azure Automation med hjälp av den [AzureRM Key Vault-cmdlets](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) och [Azure klassiska Key Vault-cmdlets](https://msdn.microsoft.com/library/azure/dn868052.aspx).</span><span class="sxs-lookup"><span data-stu-id="12fff-112">Key Vault can be managed in Azure Automation by using the [AzureRM Key Vault cmdlets](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) and [Azure Classic Key Vault cmdlets](https://msdn.microsoft.com/library/azure/dn868052.aspx).</span></span> <span data-ttu-id="12fff-113">Azure-modulen för hantering av den klassiska Key Vault är tillgängligt i Azure Automation automatiskt och du kan importera den [AzureRM KeyVault modulen](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) i Azure Automation så att du kan utföra många av din Key Vault hanteringsuppgifter i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="12fff-113">The Azure module for managing classic Key Vault is available automatically in Azure Automation, and you can import the [AzureRM-KeyVault module](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) into Azure Automation, so that you can perform many of your Key Vault management tasks within the service.</span></span> <span data-ttu-id="12fff-114">Du kan också koppla dessa cmdlets i Azure Automation med cmdlets för andra Azure-tjänster, att automatisera avancerade uppgifter över Azure-tjänster och 3 part.</span><span class="sxs-lookup"><span data-stu-id="12fff-114">You can also pair these cmdlets in Azure Automation with the cmdlets for other Azure services, to automate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="12fff-115">Du kan utföra dessa uppgifter, bland annat med Azure Key Vault-cmdlets:</span><span class="sxs-lookup"><span data-stu-id="12fff-115">With the Azure Key Vault cmdlets you can perform these tasks among others:</span></span> 

* <span data-ttu-id="12fff-116">Skapa och konfigurera ett nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="12fff-116">Create and configure a key vault</span></span>
* <span data-ttu-id="12fff-117">Skapa eller importera en nyckel</span><span class="sxs-lookup"><span data-stu-id="12fff-117">Create or import a key</span></span>
* <span data-ttu-id="12fff-118">Skapa eller uppdatera en hemlighet</span><span class="sxs-lookup"><span data-stu-id="12fff-118">Create or update a secret</span></span>
* <span data-ttu-id="12fff-119">Uppdatera attribut för en nyckel</span><span class="sxs-lookup"><span data-stu-id="12fff-119">Update attributes of a key</span></span>
* <span data-ttu-id="12fff-120">Hämta en nyckel eller hemlighet.</span><span class="sxs-lookup"><span data-stu-id="12fff-120">Get a key or secret</span></span>
* <span data-ttu-id="12fff-121">Ta bort en nyckel eller hemlighet.</span><span class="sxs-lookup"><span data-stu-id="12fff-121">Delete a key or secret</span></span>

<span data-ttu-id="12fff-122">Här följer några exempel på att använda PowerShell för att hantera Key Vault:</span><span class="sxs-lookup"><span data-stu-id="12fff-122">Here are some examples of using PowerShell to manage Key Vault:</span></span>  

* [<span data-ttu-id="12fff-123">Azure Key Vault - steg-för-steg</span><span class="sxs-lookup"><span data-stu-id="12fff-123">Azure Key Vault - Step by Step</span></span>](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step)
* [<span data-ttu-id="12fff-124">Installera och konfigurera en Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="12fff-124">Setting Up and Configuring an Azure Key Vault</span></span>](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault)

## <a name="next-steps"></a><span data-ttu-id="12fff-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="12fff-125">Next steps</span></span>
<span data-ttu-id="12fff-126">Nu när du har lärt dig grunderna i Azure Automation och hur det kan användas för att hantera Azure Key Vault kan du följa dessa länkar om du vill veta mer om Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="12fff-126">Now that you've learned the basics of Azure Automation and how it can be used to manage Azure Key Vault, follow these links to learn more about Azure Automation.</span></span>

* <span data-ttu-id="12fff-127">Se Azure Automation [komma igång-självstudiekurs](../automation/automation-first-runbook-graphical.md).</span><span class="sxs-lookup"><span data-stu-id="12fff-127">See the Azure Automation [Getting Started Tutorial](../automation/automation-first-runbook-graphical.md).</span></span>
* <span data-ttu-id="12fff-128">Finns det [Azure Key Vault PowerShell-skript](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).</span><span class="sxs-lookup"><span data-stu-id="12fff-128">See the [Azure Key Vault PowerShell scripts](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).</span></span>

