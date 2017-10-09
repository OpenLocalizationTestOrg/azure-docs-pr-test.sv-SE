---
title: aaaManage virtuella datorer med Azure Automation | Microsoft Docs
description: "Läs mer om hur hello Azure Automation-tjänsten kan vara används toomanage virtuella Azure-datorer i större skala."
services: virtual-machines-windows, automation
documentationcenter: 
author: jodoglevy
manager: timlt
editor: 
ms.assetid: ce49f83a-f409-42ee-af74-a8ea2caa9ae8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2016
ms.author: timlt
ms.openlocfilehash: bfe7b3a51b6e82bd7cd5b0a83df7226476ed4f36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-virtual-machines-using-azure-automation"></a><span data-ttu-id="2a8e1-103">Hantera Azure Virtual Machines med Azure Automation</span><span class="sxs-lookup"><span data-stu-id="2a8e1-103">Managing Azure Virtual Machines using Azure Automation</span></span>
<span data-ttu-id="2a8e1-104">Den här guiden presenteras toohello Azure Automation-tjänsten och hur den kan användas toosimplify hantera dina virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="2a8e1-104">This guide introduces you toohello Azure Automation service and how it can be used toosimplify managing your Azure virtual machines.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="2a8e1-105">Vad är Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="2a8e1-105">What is Azure Automation?</span></span>
<span data-ttu-id="2a8e1-106">[Azure Automation](https://azure.microsoft.com/services/automation/) är en Azure-tjänst som förenklar molnhantering via automatisering.</span><span class="sxs-lookup"><span data-stu-id="2a8e1-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="2a8e1-107">Med hjälp av Azure Automation kan tidskrävande, manuell, felbenägna och ofta återkommande uppgifter vara automatisk tooincrease tillförlitlighet, effektivitet och tid-värde för din organisation.</span><span class="sxs-lookup"><span data-stu-id="2a8e1-107">By using Azure Automation, long-running, manual, error-prone, and frequently repeated tasks can be automated tooincrease reliability, efficiency, and time-to-value for your organization.</span></span>

<span data-ttu-id="2a8e1-108">Azure Automation ger en Körningsmotor för arbetsflöden för hög tillförlitliga och hög tillgänglighet som kan skalas toomeet dina behov när organisationen växer.</span><span class="sxs-lookup"><span data-stu-id="2a8e1-108">Azure Automation provides a highly reliable and highly available workflow execution engine that scales toomeet your needs as your organization grows.</span></span> <span data-ttu-id="2a8e1-109">I Azure Automation kan processer vara inletts manuellt med system från tredje part eller med schemalagda intervall så att aktiviteter inträffa exakt vid behov.</span><span class="sxs-lookup"><span data-stu-id="2a8e1-109">In Azure Automation, processes can be kicked off manually, by third-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="2a8e1-110">Du kan sänka operativ tillsyn och frigör IT och DevOps personal toofocus på arbete som lägger till affärsvärde genom att köra ditt moln hanteringsuppgifter automatiskt med Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="2a8e1-110">You can lower operational overhead and free up IT and DevOps staff toofocus on work that adds business value by running your cloud management tasks automatically with Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-virtual-machines"></a><span data-ttu-id="2a8e1-111">Hur Azure Automation kan hantera virtuella Azure-datorer?</span><span class="sxs-lookup"><span data-stu-id="2a8e1-111">How can Azure Automation help manage Azure virtual machines?</span></span>
<span data-ttu-id="2a8e1-112">Virtuella datorer kan hanteras i Azure Automation med hjälp av [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span><span class="sxs-lookup"><span data-stu-id="2a8e1-112">Virtual machines can be managed in Azure Automation by using [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span></span> <span data-ttu-id="2a8e1-113">Azure Automation innehåller hello Azure PowerShell-cmdlets, så att du kan utföra alla virtuella management aktiviteter inom hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2a8e1-113">Azure Automation includes hello Azure PowerShell cmdlets, so you can perform all of your virtual machine management tasks within hello service.</span></span> <span data-ttu-id="2a8e1-114">Du kan också koppla hello-cmdlets i Azure Automation med hello-cmdlets för andra Azure-tjänster, tooautomate komplicerade uppgifter över Azure-tjänster och program från tredje part.</span><span class="sxs-lookup"><span data-stu-id="2a8e1-114">You can also pair hello cmdlets in Azure Automation with hello cmdlets for other Azure services, tooautomate complex tasks across Azure services and third-party systems.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2a8e1-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2a8e1-115">Next steps</span></span>
<span data-ttu-id="2a8e1-116">Nu när du har lärt dig grunderna i hello Azure Automation och hur det kan vara används toomanage virtuella Azure-datorer, mer information:</span><span class="sxs-lookup"><span data-stu-id="2a8e1-116">Now that you've learned hello basics of Azure Automation and how it can be used toomanage Azure virtual machines, learn more:</span></span>

* [<span data-ttu-id="2a8e1-117">Azure Automation-översikt</span><span class="sxs-lookup"><span data-stu-id="2a8e1-117">Azure Automation Overview</span></span>](../../automation/automation-intro.md)
* [<span data-ttu-id="2a8e1-118">Min första Runbook</span><span class="sxs-lookup"><span data-stu-id="2a8e1-118">My first runbook</span></span>](../../automation/automation-first-runbook-graphical.md)
* [<span data-ttu-id="2a8e1-119">Azure Automation-inlärningsguide</span><span class="sxs-lookup"><span data-stu-id="2a8e1-119">Azure Automation learning map</span></span>](https://azure.microsoft.com/documentation/learning-paths/automation/)

