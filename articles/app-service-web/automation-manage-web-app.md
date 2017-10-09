---
title: aaaManage Azure Web App med Azure Automation | Microsoft Docs
description: "Läs mer om hur hello Azure Automation-tjänsten kan vara används toomanage Azure Web App."
services: app-service\web, automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: c960a44f-f941-401d-afba-a4bc0f0394eb
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: magoedte
ms.openlocfilehash: 6d80351a2927f26753cfbaead6e1c0c3c94e86e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-web-app-using-azure-automation"></a><span data-ttu-id="a059b-103">Hantera Azure Web App med Azure Automation</span><span class="sxs-lookup"><span data-stu-id="a059b-103">Managing Azure Web App using Azure Automation</span></span>
<span data-ttu-id="a059b-104">Den här guiden beskrivs toohello Azure Automation-tjänsten och hur det kan vara används toosimplify hantering av Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="a059b-104">This guide will introduce you toohello Azure Automation service, and how it can be used toosimplify management of Azure Web App.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="a059b-105">Vad är Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="a059b-105">What is Azure Automation?</span></span>
<span data-ttu-id="a059b-106">[Azure Automation](../automation/automation-intro.md) är en Azure-tjänst som förenklar molnhantering via automatisering.</span><span class="sxs-lookup"><span data-stu-id="a059b-106">[Azure Automation](../automation/automation-intro.md) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="a059b-107">Med Azure Automation kan manuell upprepade, tidskrävande och felbenägna aktiviteter vara automatisk tooincrease tillförlitlighet, effektivitet och tid toovalue för din organisation.</span><span class="sxs-lookup"><span data-stu-id="a059b-107">Using Azure Automation, manual, repeated, long-running, and error-prone tasks can be automated tooincrease reliability, efficiency, and time toovalue for your organization.</span></span>

<span data-ttu-id="a059b-108">Azure Automation ger en Körningsmotor för arbetsflöden för mycket tillförlitliga, hög tillgänglighet som kan skalas toomeet dina behov.</span><span class="sxs-lookup"><span data-stu-id="a059b-108">Azure Automation provides a highly-reliable, highly-available workflow execution engine that scales toomeet your needs.</span></span> <span data-ttu-id="a059b-109">I Azure Automation kan processer vara inletts manuellt av 3 part eller med schemalagda intervall så att aktiviteter inträffa exakt vid behov.</span><span class="sxs-lookup"><span data-stu-id="a059b-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="a059b-110">Minska operativ tillsyn och frigör IT och DevOps personal toofocus på arbete som lägger till affärsvärde genom att flytta dina molntjänster management uppgifter toobe körs automatiskt av Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="a059b-110">Reduce operational overhead and free up IT and DevOps staff toofocus on work that adds business value by moving your cloud management tasks toobe run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-web-app"></a><span data-ttu-id="a059b-111">Hur Azure Automation kan hantera Azure Web App?</span><span class="sxs-lookup"><span data-stu-id="a059b-111">How can Azure Automation help manage Azure Web App?</span></span>
<span data-ttu-id="a059b-112">Webbprogram kan hanteras i Azure Automation med hello PowerShell-cmdlets som är tillgängliga i hello [Azure PowerShell-moduler](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="a059b-112">Web App can be managed in Azure Automation by using hello PowerShell cmdlets that are available in hello [Azure PowerShell modules](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="a059b-113">Du kan [installera dessa Web App PowerShell-cmdlets i Azure Automation](https://azure.microsoft.com/blog/announcing-azure-resource-manager-support-azure-automation-runbooks/), så att du kan utföra alla aktiviteter inom hello service Web App management.</span><span class="sxs-lookup"><span data-stu-id="a059b-113">You can [install these Web App PowerShell cmdlets in Azure Automation](https://azure.microsoft.com/blog/announcing-azure-resource-manager-support-azure-automation-runbooks/), so that you can perform all of your Web App management tasks within hello service.</span></span> <span data-ttu-id="a059b-114">Du kan också koppla dessa cmdlets i Azure Automation med hello-cmdlets för andra Azure-tjänster tooautomate komplicerade uppgifter över Azure-tjänster och 3 part.</span><span class="sxs-lookup"><span data-stu-id="a059b-114">You can also pair these cmdlets in Azure Automation with hello cmdlets for other Azure services tooautomate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="a059b-115">Här följer några exempel för att hantera Apptjänster med automatisering:</span><span class="sxs-lookup"><span data-stu-id="a059b-115">Here are some examples of managing App Services with Automation:</span></span>

* [<span data-ttu-id="a059b-116">Skript för att hantera Web Apps</span><span class="sxs-lookup"><span data-stu-id="a059b-116">Scripts for managing Web Apps</span></span>](https://azure.microsoft.com/documentation/scripts/)

## <a name="next-steps"></a><span data-ttu-id="a059b-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a059b-117">Next steps</span></span>
<span data-ttu-id="a059b-118">Nu när du har lärt dig hello grunderna i Azure Automation och hur det kan vara används toomanage Azure Web App, följa dessa länkar toolearn mer om Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="a059b-118">Now that you've learned hello basics of Azure Automation and how it can be used toomanage Azure Web App, follow these links toolearn more about Azure Automation.</span></span>

* <span data-ttu-id="a059b-119">Se hello Azure Automation [komma igång-Självstudier](../automation/automation-first-runbook-graphical.md)</span><span class="sxs-lookup"><span data-stu-id="a059b-119">See hello Azure Automation [Getting Started Tutorial](../automation/automation-first-runbook-graphical.md)</span></span>

