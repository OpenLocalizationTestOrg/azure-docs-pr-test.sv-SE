---
title: "aaaManage Azure API Management med hjälp av Azure Automation"
description: "Läs mer om hur hello Azure Automation-tjänsten kan vara används toomanage Azure API Management."
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
ms.openlocfilehash: 05b8e3cad786fa701feb88f7b6d9629f16686d15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-api-management-using-azure-automation"></a><span data-ttu-id="a3892-103">Hantera Azure API Management med hjälp av Azure Automation</span><span class="sxs-lookup"><span data-stu-id="a3892-103">Managing Azure API Management using Azure Automation</span></span>
<span data-ttu-id="a3892-104">Den här guiden beskrivs toohello Azure Automation-tjänsten och hur det kan vara används toosimplify hantering av Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="a3892-104">This guide will introduce you toohello Azure Automation service, and how it can be used toosimplify management of Azure API Management.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="a3892-105">Vad är Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="a3892-105">What is Azure Automation?</span></span>
<span data-ttu-id="a3892-106">[Azure Automation](https://azure.microsoft.com/services/automation/) är en Azure-tjänst som förenklar molnhantering via automatisering.</span><span class="sxs-lookup"><span data-stu-id="a3892-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="a3892-107">Med Azure Automation kan manuell upprepade, tidskrävande och felbenägna aktiviteter vara automatisk tooincrease tillförlitlighet, effektivitet och tid toovalue för din organisation.</span><span class="sxs-lookup"><span data-stu-id="a3892-107">Using Azure Automation, manual, repeated, long-running, and error-prone tasks can be automated tooincrease reliability, efficiency, and time toovalue for your organization.</span></span>

<span data-ttu-id="a3892-108">Azure Automation ger en Körningsmotor för arbetsflöden för mycket tillförlitliga, hög tillgänglighet som kan skalas toomeet dina behov.</span><span class="sxs-lookup"><span data-stu-id="a3892-108">Azure Automation provides a highly-reliable, highly-available workflow execution engine that scales toomeet your needs.</span></span> <span data-ttu-id="a3892-109">I Azure Automation kan processer vara inletts manuellt av 3 part eller med schemalagda intervall så att aktiviteter inträffa exakt vid behov.</span><span class="sxs-lookup"><span data-stu-id="a3892-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="a3892-110">Minska operativ tillsyn och frigör IT och DevOps personal toofocus på arbete som lägger till affärsvärde genom att flytta dina molntjänster management uppgifter toobe körs automatiskt av Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="a3892-110">Reduce operational overhead and free up IT and DevOps staff toofocus on work that adds business value by moving your cloud management tasks toobe run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-api-management"></a><span data-ttu-id="a3892-111">Hur Azure Automation kan hantera Azure API Management?</span><span class="sxs-lookup"><span data-stu-id="a3892-111">How can Azure Automation help manage Azure API Management?</span></span>
<span data-ttu-id="a3892-112">API-hantering kan hanteras i Azure Automation med hjälp av hello [Windows PowerShell cmdlets för Azure API Management API](https://azure.microsoft.com/updates/full-set-of-windows-powershell-cmdlets-for-azure-api-management-api/).</span><span class="sxs-lookup"><span data-stu-id="a3892-112">API Management can be managed in Azure Automation by using hello [Windows PowerShell cmdlets for Azure API Management API](https://azure.microsoft.com/updates/full-set-of-windows-powershell-cmdlets-for-azure-api-management-api/).</span></span> <span data-ttu-id="a3892-113">I Azure Automation kan du skriva PowerShell arbetsflöde skript tooperform många av uppgifterna API-hantering med hello-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="a3892-113">Within Azure Automation you can write PowerShell workflow scripts tooperform many of your API Management tasks using hello cmdlets.</span></span> <span data-ttu-id="a3892-114">Du kan också koppla dessa cmdlets i Azure Automation med hello-cmdlets för andra Azure-tjänster, tooautomate komplicerade uppgifter över Azure-tjänster och 3 part.</span><span class="sxs-lookup"><span data-stu-id="a3892-114">You can also pair these cmdlets in Azure Automation with hello cmdlets for other Azure services, tooautomate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="a3892-115">Här följer några exempel på hur API Management med automatisering:</span><span class="sxs-lookup"><span data-stu-id="a3892-115">Here are some examples of using API Management with Automation:</span></span>

* [<span data-ttu-id="a3892-116">Azure API Management – använda PowerShell för säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="a3892-116">Azure API Management – Using PowerShell for backup and restore</span></span>](https://blogs.msdn.microsoft.com/katriend/2015/10/02/azure-api-management-using-powershell-for-backup-and-restore/)

## <a name="next-steps"></a><span data-ttu-id="a3892-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a3892-117">Next Steps</span></span>
<span data-ttu-id="a3892-118">Nu när du har lärt dig grunderna hello i Azure Automation och hur det kan vara används toomanage Azure API Management följa dessa länkar toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="a3892-118">Now that you've learned hello basics of Azure Automation and how it can be used toomanage Azure API Management, follow these links toolearn more.</span></span>

* <span data-ttu-id="a3892-119">Se hello Azure Automation [komma igång-självstudiekurs](../automation/automation-first-runbook-graphical.md).</span><span class="sxs-lookup"><span data-stu-id="a3892-119">See hello Azure Automation [getting started tutorial](../automation/automation-first-runbook-graphical.md).</span></span>

