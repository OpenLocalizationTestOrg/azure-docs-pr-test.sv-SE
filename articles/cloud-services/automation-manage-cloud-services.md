---
title: Hantera Azure Cloud Services med Azure Automation | Microsoft Docs
description: "Läs mer om hur Azure Automation-tjänsten kan användas för att hantera Azure-molntjänster i större skala."
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
ms.openlocfilehash: 6b5acac1b8647c324988c316cd5602b3dba98a1d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="managing-azure-cloud-services-using-azure-automation"></a><span data-ttu-id="b5ccf-103">Hantera Azure Cloud Services med Azure Automation</span><span class="sxs-lookup"><span data-stu-id="b5ccf-103">Managing Azure Cloud Services using Azure Automation</span></span>
<span data-ttu-id="b5ccf-104">Den här guiden innehåller en introduktion till Azure Automation-tjänsten och hur det kan användas för att förenkla hanteringen av dina Azure-molntjänster.</span><span class="sxs-lookup"><span data-stu-id="b5ccf-104">This guide will introduce you to the Azure Automation service, and how it can be used to simplify management of your Azure cloud services.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="b5ccf-105">Vad är Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="b5ccf-105">What is Azure Automation?</span></span>
<span data-ttu-id="b5ccf-106">[Azure Automation](https://azure.microsoft.com/services/automation/) är en Azure-tjänst som förenklar molnhantering via automatisering.</span><span class="sxs-lookup"><span data-stu-id="b5ccf-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="b5ccf-107">Med Azure Automation kan tidskrävande, manuell, felbenägna och ofta återkommande uppgifter automatiseras för att öka tillförlitligheten, effektivitet och tid för värde för din organisation.</span><span class="sxs-lookup"><span data-stu-id="b5ccf-107">Using Azure Automation, long-running, manual, error-prone, and frequently repeated tasks can be automated to increase reliability, efficiency, and time to value for your organization.</span></span>

<span data-ttu-id="b5ccf-108">Azure Automation ger en Körningsmotor för arbetsflöden för hög tillförlitliga och hög tillgänglighet som kan skalas för att uppfylla dina behov när organisationen växer.</span><span class="sxs-lookup"><span data-stu-id="b5ccf-108">Azure Automation provides a highly-reliable and highly-available workflow execution engine that scales to meet your needs as your organization grows.</span></span> <span data-ttu-id="b5ccf-109">I Azure Automation kan processer vara inletts manuellt av 3 part eller med schemalagda intervall så att aktiviteter inträffa exakt vid behov.</span><span class="sxs-lookup"><span data-stu-id="b5ccf-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="b5ccf-110">Lägre operativ tillsyn och frigöra IT / DevOps-personal att fokusera på arbete som lägger till företag värde genom att flytta dina uppgifter för hantering av molnet att köras automatiskt av Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="b5ccf-110">Lower operational overhead and free up IT / DevOps staff to focus on work that adds business value by moving your cloud management tasks to be run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-cloud-services"></a><span data-ttu-id="b5ccf-111">Hur Azure Automation kan hantera Azure-molntjänster?</span><span class="sxs-lookup"><span data-stu-id="b5ccf-111">How can Azure Automation help manage Azure cloud services?</span></span>
<span data-ttu-id="b5ccf-112">Azure-molntjänster kan hanteras i Azure Automation med hjälp av PowerShell-cmdlets som är tillgängliga i den [Azure PowerShell verktygen](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span><span class="sxs-lookup"><span data-stu-id="b5ccf-112">Azure cloud services can be managed in Azure Automation by using the PowerShell cmdlets that are available in the [Azure PowerShell tools](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span></span> <span data-ttu-id="b5ccf-113">Azure Automation har dessa moln PowerShell cmdlet: ar tillgängliga direkt, så att du kan utföra alla cloud service management aktiviteter inom tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b5ccf-113">Azure Automation has these cloud service PowerShell cmdlets available out of the box, so that you can perform all of your cloud service management tasks within the service.</span></span> <span data-ttu-id="b5ccf-114">Du kan också koppla dessa cmdlets i Azure Automation med cmdlets för andra Azure-tjänster, att automatisera avancerade uppgifter över Azure-tjänster och 3 part.</span><span class="sxs-lookup"><span data-stu-id="b5ccf-114">You can also pair these cmdlets in Azure Automation with the cmdlets for other Azure services, to automate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="b5ccf-115">Vissa exempel användningsområden för Azure Automation för att hantera Azure Cloud Services är:</span><span class="sxs-lookup"><span data-stu-id="b5ccf-115">Some example uses of Azure Automation to manage Azure Cloud Services include:</span></span>

* [<span data-ttu-id="b5ccf-116">Kontinuerlig distribution av en tjänst i molnet när cscfg eller cspkg uppdateras i Azure Blob storage</span><span class="sxs-lookup"><span data-stu-id="b5ccf-116">Continous deployment of a Cloud Service whenever cscfg or cspkg is updated in Azure Blob storage</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Continuous-Deployment-of-A-eeebf3a6)
* [<span data-ttu-id="b5ccf-117">Starta om Molntjänsten instanser parallellt, en domän i taget</span><span class="sxs-lookup"><span data-stu-id="b5ccf-117">Rebooting Cloud Service instances in parallel, one upgrade domain at a time</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Reboot-Cloud-Service-PaaS-b337a06d)

## <a name="next-steps"></a><span data-ttu-id="b5ccf-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b5ccf-118">Next Steps</span></span>
<span data-ttu-id="b5ccf-119">Nu när du har lärt dig grunderna i Azure Automation och hur det kan användas för att hantera Azure-molntjänster kan du följa dessa länkar om du vill veta mer om Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="b5ccf-119">Now that you've learned the basics of Azure Automation and how it can be used to manage Azure cloud services, follow these links to learn more about Azure Automation.</span></span>

* [<span data-ttu-id="b5ccf-120">Azure Automation-översikt</span><span class="sxs-lookup"><span data-stu-id="b5ccf-120">Azure Automation Overview</span></span>](../automation/automation-intro.md)
* [<span data-ttu-id="b5ccf-121">Min första Runbook</span><span class="sxs-lookup"><span data-stu-id="b5ccf-121">My first runbook</span></span>](../automation/automation-first-runbook-graphical.md)
* [<span data-ttu-id="b5ccf-122">Azure Automation-inlärningsguide</span><span class="sxs-lookup"><span data-stu-id="b5ccf-122">Azure Automation learning map</span></span>](https://azure.microsoft.com/documentation/learning-paths/automation/)
