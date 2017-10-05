---
title: "Hantera Azure API Management med hjälp av Azure Automation"
description: "Läs mer om hur Azure Automation-tjänsten kan användas för att hantera Azure API Management."
services: api-management, automation
documentationcenter: 
author: csand-msft
manager: eamono
editor: 
ms.assetid: 2e53c9af-f738-47f8-b1b6-593050a7c51b
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 73a1135482b88ea7c228bc8b228d47c57b2e70a4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="managing-azure-api-management-using-azure-automation"></a><span data-ttu-id="97f69-103">Hantera Azure API Management med hjälp av Azure Automation</span><span class="sxs-lookup"><span data-stu-id="97f69-103">Managing Azure API Management using Azure Automation</span></span>
<span data-ttu-id="97f69-104">Den här guiden innehåller en introduktion till Azure Automation-tjänsten och hur det kan användas för att förenkla hanteringen av Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="97f69-104">This guide will introduce you to the Azure Automation service, and how it can be used to simplify management of Azure API Management.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="97f69-105">Vad är Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="97f69-105">What is Azure Automation?</span></span>
<span data-ttu-id="97f69-106">[Azure Automation](https://azure.microsoft.com/services/automation/) är en Azure-tjänst som förenklar molnhantering via automatisering.</span><span class="sxs-lookup"><span data-stu-id="97f69-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="97f69-107">Med hjälp av Azure Automation, manuell, kan upprepas tidskrävande och felbenägna aktiviteter automatiseras för att öka tillförlitligheten, effektivitet och tid för värde för din organisation.</span><span class="sxs-lookup"><span data-stu-id="97f69-107">Using Azure Automation, manual, repeated, long-running, and error-prone tasks can be automated to increase reliability, efficiency, and time to value for your organization.</span></span>

<span data-ttu-id="97f69-108">Azure Automation ger en Körningsmotor för arbetsflöden för mycket tillförlitliga, hög tillgänglighet som kan skalas för att uppfylla dina behov.</span><span class="sxs-lookup"><span data-stu-id="97f69-108">Azure Automation provides a highly-reliable, highly-available workflow execution engine that scales to meet your needs.</span></span> <span data-ttu-id="97f69-109">I Azure Automation kan processer vara inletts manuellt av 3 part eller med schemalagda intervall så att aktiviteter inträffa exakt vid behov.</span><span class="sxs-lookup"><span data-stu-id="97f69-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="97f69-110">Minska operativ tillsyn och frigör IT och DevOps-personal att fokusera på arbete som lägger till affärsvärde genom att flytta dina uppgifter för hantering av molnet att köras automatiskt av Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="97f69-110">Reduce operational overhead and free up IT and DevOps staff to focus on work that adds business value by moving your cloud management tasks to be run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-api-management"></a><span data-ttu-id="97f69-111">Hur Azure Automation kan hantera Azure API Management?</span><span class="sxs-lookup"><span data-stu-id="97f69-111">How can Azure Automation help manage Azure API Management?</span></span>
<span data-ttu-id="97f69-112">API-hantering kan hanteras i Azure Automation med hjälp av den [Windows PowerShell cmdlets för Azure API Management API](https://azure.microsoft.com/updates/full-set-of-windows-powershell-cmdlets-for-azure-api-management-api/).</span><span class="sxs-lookup"><span data-stu-id="97f69-112">API Management can be managed in Azure Automation by using the [Windows PowerShell cmdlets for Azure API Management API](https://azure.microsoft.com/updates/full-set-of-windows-powershell-cmdlets-for-azure-api-management-api/).</span></span> <span data-ttu-id="97f69-113">Du kan skriva PowerShell arbetsflöde för skript för att utföra många av dina uppgifter för API Management med hjälp av cmdlets i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="97f69-113">Within Azure Automation you can write PowerShell workflow scripts to perform many of your API Management tasks using the cmdlets.</span></span> <span data-ttu-id="97f69-114">Du kan också koppla dessa cmdlets i Azure Automation med cmdlets för andra Azure-tjänster, att automatisera avancerade uppgifter över Azure-tjänster och 3 part.</span><span class="sxs-lookup"><span data-stu-id="97f69-114">You can also pair these cmdlets in Azure Automation with the cmdlets for other Azure services, to automate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="97f69-115">Här följer några exempel på hur API Management med automatisering:</span><span class="sxs-lookup"><span data-stu-id="97f69-115">Here are some examples of using API Management with Automation:</span></span>

* [<span data-ttu-id="97f69-116">Azure API Management – använda PowerShell för säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="97f69-116">Azure API Management – Using PowerShell for backup and restore</span></span>](https://blogs.msdn.microsoft.com/katriend/2015/10/02/azure-api-management-using-powershell-for-backup-and-restore/)

## <a name="next-steps"></a><span data-ttu-id="97f69-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="97f69-117">Next Steps</span></span>
<span data-ttu-id="97f69-118">Nu när du har lärt dig grunderna i Azure Automation och hur det kan användas för att hantera Azure API Management kan du följa dessa länkar om du vill veta mer.</span><span class="sxs-lookup"><span data-stu-id="97f69-118">Now that you've learned the basics of Azure Automation and how it can be used to manage Azure API Management, follow these links to learn more.</span></span>

* <span data-ttu-id="97f69-119">Se Azure Automation [komma igång-självstudiekurs](../automation/automation-first-runbook-graphical.md).</span><span class="sxs-lookup"><span data-stu-id="97f69-119">See the Azure Automation [getting started tutorial](../automation/automation-first-runbook-graphical.md).</span></span>

