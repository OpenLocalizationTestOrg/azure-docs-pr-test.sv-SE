---
title: aaaManage Azure Cloud Services med Azure Automation | Microsoft Docs
description: "Läs mer om hur hello Azure Automation-tjänsten kan vara används toomanage Azure-molntjänster i större skala."
services: cloud-services, automation
documentationcenter: 
author: jodoglevy
manager: timlt
editor: 
ms.assetid: 3789810a-2892-4eef-bf29-c781c1b5af48
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2016
ms.author: timlt
ms.openlocfilehash: 8e920fb94955466bfec71cc332444f5f0ee497a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-cloud-services-using-azure-automation"></a><span data-ttu-id="a6d40-103">Hantera Azure Cloud Services med Azure Automation</span><span class="sxs-lookup"><span data-stu-id="a6d40-103">Managing Azure Cloud Services using Azure Automation</span></span>
<span data-ttu-id="a6d40-104">Den här guiden beskrivs toohello Azure Automation-tjänsten och hur det kan vara används toosimplify hantering av Azure-molntjänster.</span><span class="sxs-lookup"><span data-stu-id="a6d40-104">This guide will introduce you toohello Azure Automation service, and how it can be used toosimplify management of your Azure cloud services.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="a6d40-105">Vad är Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="a6d40-105">What is Azure Automation?</span></span>
<span data-ttu-id="a6d40-106">[Azure Automation](https://azure.microsoft.com/services/automation/) är en Azure-tjänst som förenklar molnhantering via automatisering.</span><span class="sxs-lookup"><span data-stu-id="a6d40-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="a6d40-107">Med Azure Automation kan tidskrävande, manuell, felbenägna och ofta återkommande uppgifter vara automatisk tooincrease tillförlitlighet, effektivitet och tid toovalue för din organisation.</span><span class="sxs-lookup"><span data-stu-id="a6d40-107">Using Azure Automation, long-running, manual, error-prone, and frequently repeated tasks can be automated tooincrease reliability, efficiency, and time toovalue for your organization.</span></span>

<span data-ttu-id="a6d40-108">Azure Automation ger en Körningsmotor för arbetsflöden för hög tillförlitliga och hög tillgänglighet som kan skalas toomeet dina behov när organisationen växer.</span><span class="sxs-lookup"><span data-stu-id="a6d40-108">Azure Automation provides a highly-reliable and highly-available workflow execution engine that scales toomeet your needs as your organization grows.</span></span> <span data-ttu-id="a6d40-109">I Azure Automation kan processer vara inletts manuellt av 3 part eller med schemalagda intervall så att aktiviteter inträffa exakt vid behov.</span><span class="sxs-lookup"><span data-stu-id="a6d40-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="a6d40-110">Lägre operativ tillsyn och frigöra IT / DevOps personal toofocus på arbete som lägger till affärsvärde genom att flytta dina molntjänster management uppgifter toobe körs automatiskt av Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="a6d40-110">Lower operational overhead and free up IT / DevOps staff toofocus on work that adds business value by moving your cloud management tasks toobe run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-cloud-services"></a><span data-ttu-id="a6d40-111">Hur Azure Automation kan hantera Azure-molntjänster?</span><span class="sxs-lookup"><span data-stu-id="a6d40-111">How can Azure Automation help manage Azure cloud services?</span></span>
<span data-ttu-id="a6d40-112">Azure-molntjänster kan hanteras i Azure Automation med hello PowerShell-cmdlets som är tillgängliga i hello [Azure PowerShell verktygen](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span><span class="sxs-lookup"><span data-stu-id="a6d40-112">Azure cloud services can be managed in Azure Automation by using hello PowerShell cmdlets that are available in hello [Azure PowerShell tools](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span></span> <span data-ttu-id="a6d40-113">Azure Automation har dessa moln PowerShell cmdlet: ar tillgängliga out of box hello, så att du kan utföra alla cloud service management aktiviteter inom hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a6d40-113">Azure Automation has these cloud service PowerShell cmdlets available out of hello box, so that you can perform all of your cloud service management tasks within hello service.</span></span> <span data-ttu-id="a6d40-114">Du kan också koppla dessa cmdlets i Azure Automation med hello-cmdlets för andra Azure-tjänster, tooautomate komplicerade uppgifter över Azure-tjänster och 3 part.</span><span class="sxs-lookup"><span data-stu-id="a6d40-114">You can also pair these cmdlets in Azure Automation with hello cmdlets for other Azure services, tooautomate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="a6d40-115">Vissa exempel som används av Azure Automation-toomanage Azure Cloud Services inkluderar:</span><span class="sxs-lookup"><span data-stu-id="a6d40-115">Some example uses of Azure Automation toomanage Azure Cloud Services include:</span></span>

* [<span data-ttu-id="a6d40-116">Kontinuerlig distribution av en tjänst i molnet när cscfg eller cspkg uppdateras i Azure Blob storage</span><span class="sxs-lookup"><span data-stu-id="a6d40-116">Continous deployment of a Cloud Service whenever cscfg or cspkg is updated in Azure Blob storage</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Continuous-Deployment-of-A-eeebf3a6)
* [<span data-ttu-id="a6d40-117">Starta om Molntjänsten instanser parallellt, en domän i taget</span><span class="sxs-lookup"><span data-stu-id="a6d40-117">Rebooting Cloud Service instances in parallel, one upgrade domain at a time</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Reboot-Cloud-Service-PaaS-b337a06d)

## <a name="next-steps"></a><span data-ttu-id="a6d40-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a6d40-118">Next Steps</span></span>
<span data-ttu-id="a6d40-119">Nu när du har lärt dig grunderna hello i Azure Automation och hur det kan vara används toomanage Azure-molntjänster, ska du följa dessa länkar toolearn mer om Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="a6d40-119">Now that you've learned hello basics of Azure Automation and how it can be used toomanage Azure cloud services, follow these links toolearn more about Azure Automation.</span></span>

* [<span data-ttu-id="a6d40-120">Azure Automation-översikt</span><span class="sxs-lookup"><span data-stu-id="a6d40-120">Azure Automation Overview</span></span>](../automation/automation-intro.md)
* [<span data-ttu-id="a6d40-121">Min första Runbook</span><span class="sxs-lookup"><span data-stu-id="a6d40-121">My first runbook</span></span>](../automation/automation-first-runbook-graphical.md)
* [<span data-ttu-id="a6d40-122">Azure Automation-inlärningsguide</span><span class="sxs-lookup"><span data-stu-id="a6d40-122">Azure Automation learning map</span></span>](https://azure.microsoft.com/documentation/learning-paths/automation/)
