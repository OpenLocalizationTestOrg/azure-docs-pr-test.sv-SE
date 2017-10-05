---
title: Hantera virtuella datorer med Azure Automation | Microsoft Docs
description: "Läs mer om hur Azure Automation-tjänsten kan användas för att hantera virtuella Azure-datorer i större skala."
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
ms.openlocfilehash: 15653c5d653ae538bdb66eaf0daee12c35858b45
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="managing-azure-virtual-machines-using-azure-automation"></a><span data-ttu-id="8e52c-103">Hantera Azure Virtual Machines med Azure Automation</span><span class="sxs-lookup"><span data-stu-id="8e52c-103">Managing Azure Virtual Machines using Azure Automation</span></span>
<span data-ttu-id="8e52c-104">Den här guiden ger en introduktion till Azure Automation-tjänsten och hur det kan användas för att förenkla hanteringen av dina virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="8e52c-104">This guide introduces you to the Azure Automation service and how it can be used to simplify managing your Azure virtual machines.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="8e52c-105">Vad är Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="8e52c-105">What is Azure Automation?</span></span>
<span data-ttu-id="8e52c-106">[Azure Automation](https://azure.microsoft.com/services/automation/) är en Azure-tjänst som förenklar molnhantering via automatisering.</span><span class="sxs-lookup"><span data-stu-id="8e52c-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="8e52c-107">Med hjälp av Azure Automation kan du automatiskt tidskrävande, manuell, felbenägna och ofta återkommande uppgifter för att öka tillförlitligheten, effektivitet och tid-värde för din organisation.</span><span class="sxs-lookup"><span data-stu-id="8e52c-107">By using Azure Automation, long-running, manual, error-prone, and frequently repeated tasks can be automated to increase reliability, efficiency, and time-to-value for your organization.</span></span>

<span data-ttu-id="8e52c-108">Azure Automation ger en Körningsmotor för arbetsflöden för hög tillförlitliga och hög tillgänglighet som kan skalas för att uppfylla dina behov när organisationen växer.</span><span class="sxs-lookup"><span data-stu-id="8e52c-108">Azure Automation provides a highly reliable and highly available workflow execution engine that scales to meet your needs as your organization grows.</span></span> <span data-ttu-id="8e52c-109">I Azure Automation kan processer vara inletts manuellt med system från tredje part eller med schemalagda intervall så att aktiviteter inträffa exakt vid behov.</span><span class="sxs-lookup"><span data-stu-id="8e52c-109">In Azure Automation, processes can be kicked off manually, by third-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="8e52c-110">Du kan sänka operativ tillsyn och frigör IT och DevOps-personal att fokusera på arbete som lägger till affärsvärde genom att köra ditt moln hanteringsuppgifter automatiskt med Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="8e52c-110">You can lower operational overhead and free up IT and DevOps staff to focus on work that adds business value by running your cloud management tasks automatically with Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-virtual-machines"></a><span data-ttu-id="8e52c-111">Hur Azure Automation kan hantera virtuella Azure-datorer?</span><span class="sxs-lookup"><span data-stu-id="8e52c-111">How can Azure Automation help manage Azure virtual machines?</span></span>
<span data-ttu-id="8e52c-112">Virtuella datorer kan hanteras i Azure Automation med hjälp av [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span><span class="sxs-lookup"><span data-stu-id="8e52c-112">Virtual machines can be managed in Azure Automation by using [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span></span> <span data-ttu-id="8e52c-113">Azure Automation innehåller Azure PowerShell-cmdlets, så att du kan utföra alla aktiviteter inom tjänsten virtual machine management.</span><span class="sxs-lookup"><span data-stu-id="8e52c-113">Azure Automation includes the Azure PowerShell cmdlets, so you can perform all of your virtual machine management tasks within the service.</span></span> <span data-ttu-id="8e52c-114">Du kan också koppla cmdlets i Azure Automation med cmdlets för andra Azure-tjänster, att automatisera avancerade uppgifter över Azure-tjänster och program från tredje part.</span><span class="sxs-lookup"><span data-stu-id="8e52c-114">You can also pair the cmdlets in Azure Automation with the cmdlets for other Azure services, to automate complex tasks across Azure services and third-party systems.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8e52c-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8e52c-115">Next steps</span></span>
<span data-ttu-id="8e52c-116">Nu när du har lärt dig grunderna i Azure Automation och hur det kan användas för att hantera virtuella Azure-datorer, mer information:</span><span class="sxs-lookup"><span data-stu-id="8e52c-116">Now that you've learned the basics of Azure Automation and how it can be used to manage Azure virtual machines, learn more:</span></span>

* [<span data-ttu-id="8e52c-117">Azure Automation-översikt</span><span class="sxs-lookup"><span data-stu-id="8e52c-117">Azure Automation Overview</span></span>](../../automation/automation-intro.md)
* [<span data-ttu-id="8e52c-118">Min första Runbook</span><span class="sxs-lookup"><span data-stu-id="8e52c-118">My first runbook</span></span>](../../automation/automation-first-runbook-graphical.md)
* [<span data-ttu-id="8e52c-119">Azure Automation-inlärningsguide</span><span class="sxs-lookup"><span data-stu-id="8e52c-119">Azure Automation learning map</span></span>](https://azure.microsoft.com/documentation/learning-paths/automation/)

