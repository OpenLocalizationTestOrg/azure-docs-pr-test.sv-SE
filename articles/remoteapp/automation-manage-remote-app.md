---
title: "aaaManage Azure RemoteApp med hjälp av Azure Automation | Microsoft Docs"
description: "Läs mer om hur hello Azure Automation-tjänsten kan vara används toomanage Azure RemoteApp."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: 589f01ef-f9c1-4e7b-a040-1b46862d3544
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: magoedte;csand
ms.openlocfilehash: a4cb23e292c797256e971acb3b363b025f140f16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-remoteapp-using-azure-automation"></a><span data-ttu-id="019f1-103">Hantera Azure RemoteApp med hjälp av Azure Automation</span><span class="sxs-lookup"><span data-stu-id="019f1-103">Managing Azure RemoteApp using Azure Automation</span></span>
> [!IMPORTANT]
> <span data-ttu-id="019f1-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="019f1-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="019f1-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="019f1-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="019f1-106">Den här guiden beskrivs toohello Azure Automation-tjänsten och hur det kan vara används toosimplify hantering av Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="019f1-106">This guide will introduce you toohello Azure Automation service, and how it can be used toosimplify management of Azure RemoteApp.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="019f1-107">Vad är Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="019f1-107">What is Azure Automation?</span></span>
<span data-ttu-id="019f1-108">[Azure Automation](../automation/automation-intro.md) är en Azure-tjänst som förenklar molnhantering via automatisering.</span><span class="sxs-lookup"><span data-stu-id="019f1-108">[Azure Automation](../automation/automation-intro.md) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="019f1-109">Med Azure Automation kan manuell, ofta återkommande, tidskrävande och felbenägna aktiviteter vara automatisk tooincrease tillförlitlighet, effektivitet och tid toovalue för din organisation.</span><span class="sxs-lookup"><span data-stu-id="019f1-109">Using Azure Automation, manual, frequently-repeated, long-running, and error-prone tasks can be automated tooincrease reliability, efficiency, and time toovalue for your organization.</span></span>

<span data-ttu-id="019f1-110">Azure Automation ger en Körningsmotor för arbetsflöden för mycket tillförlitliga, hög tillgänglighet som kan skalas toomeet dina behov.</span><span class="sxs-lookup"><span data-stu-id="019f1-110">Azure Automation provides a highly-reliable, highly-available workflow execution engine that scales toomeet your needs.</span></span> <span data-ttu-id="019f1-111">I Azure Automation kan processer vara inletts manuellt av 3 part eller med schemalagda intervall så att aktiviteter inträffa exakt vid behov.</span><span class="sxs-lookup"><span data-stu-id="019f1-111">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="019f1-112">Minska operativ tillsyn och frigör IT och DevOps personal toofocus på arbete som lägger till affärsvärde genom att flytta dina molntjänster management uppgifter toobe körs automatiskt av Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="019f1-112">Reduce operational overhead and free up IT and DevOps staff toofocus on work that adds business value by moving your cloud management tasks toobe run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-remoteapp"></a><span data-ttu-id="019f1-113">Hur Azure Automation kan hantera Azure RemoteApp?</span><span class="sxs-lookup"><span data-stu-id="019f1-113">How can Azure Automation help manage Azure RemoteApp?</span></span>
<span data-ttu-id="019f1-114">RemoteApp kan hanteras i Azure Automation med hello PowerShell-cmdlets som är tillgängliga i hello [Azure PowerShell verktygen](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span><span class="sxs-lookup"><span data-stu-id="019f1-114">RemoteApp can be managed in Azure Automation by using hello PowerShell cmdlets that are available in hello [Azure PowerShell tools](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span></span> <span data-ttu-id="019f1-115">Azure Automation har dessa RemoteApp PowerShell-cmdlets som är tillgängligt out of box hello, så att du kan utföra alla aktiviteter i hello tjänsten RemoteApp-hantering.</span><span class="sxs-lookup"><span data-stu-id="019f1-115">Azure Automation has these RemoteApp PowerShell cmdlets available out of hello box, so that you can perform all of your RemoteApp management tasks within hello service.</span></span> <span data-ttu-id="019f1-116">Du kan också koppla dessa cmdlets i Azure Automation med hello-cmdlets för andra Azure-tjänster, tooautomate komplicerade uppgifter över Azure-tjänster och 3 part.</span><span class="sxs-lookup"><span data-stu-id="019f1-116">You can also pair these cmdlets in Azure Automation with hello cmdlets for other Azure services, tooautomate complex tasks across Azure services and 3rd party systems.</span></span>

## <a name="next-steps"></a><span data-ttu-id="019f1-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="019f1-117">Next steps</span></span>
<span data-ttu-id="019f1-118">Nu när du har lärt dig hello grunderna i Azure Automation och hur det kan vara används toomanage Azure RemoteApp, följa dessa länkar toolearn mer om Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="019f1-118">Now that you've learned hello basics of Azure Automation and how it can be used toomanage Azure RemoteApp, follow these links toolearn more about Azure Automation.</span></span>

* <span data-ttu-id="019f1-119">Se hello Azure Automation [komma igång-Självstudier](../automation/automation-first-runbook-graphical.md)</span><span class="sxs-lookup"><span data-stu-id="019f1-119">See hello Azure Automation [Getting Started Tutorial](../automation/automation-first-runbook-graphical.md)</span></span>

