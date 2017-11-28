---
title: "aaaManage Azure Storage med hjälp av Azure Automation"
description: "Läs mer om hur hello Azure Automation-tjänsten kan vara används toomanage Azure Storage i större skala."
services: storage, automation
documentationcenter: 
author: jodoglevy
manager: eamono
editor: 
ms.assetid: bac41c39-1d95-46aa-a481-ec17bbb21b40
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: eamono
ms.openlocfilehash: 5edfba1a9ce1f9c81b5bd0889de19099f3d86fc6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-storage-using-azure-automation"></a><span data-ttu-id="1c253-103">Hantera Azure Storage med hjälp av Azure Automation</span><span class="sxs-lookup"><span data-stu-id="1c253-103">Managing Azure Storage using Azure Automation</span></span>
<span data-ttu-id="1c253-104">Den här guiden beskrivs toohello Azure Automation-tjänsten och hur det kan vara används toosimplify hanteringen av dina Azure Storage-blobbar, tabeller och köer.</span><span class="sxs-lookup"><span data-stu-id="1c253-104">This guide will introduce you toohello Azure Automation service, and how it can be used toosimplify management of your Azure Storage blobs, tables, and queues.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="1c253-105">Vad är Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="1c253-105">What is Azure Automation?</span></span>
<span data-ttu-id="1c253-106">[Azure Automation](https://azure.microsoft.com/services/automation/) är en Azure-tjänst som förenklar molnhantering via automatisering.</span><span class="sxs-lookup"><span data-stu-id="1c253-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="1c253-107">Med Azure Automation upprepade tidskrävande, manuell, felbenägna och ofta aktiviteter kan automatisk tooincrease tillförlitlighet och effektivitet och minskar tiden toovalue för din organisation.</span><span class="sxs-lookup"><span data-stu-id="1c253-107">Using Azure Automation, long-running, manual, error-prone, and frequently repeated tasks can be automated tooincrease reliability and efficiency, and reduce time toovalue for your organization.</span></span>

<span data-ttu-id="1c253-108">Azure Automation ger en Körningsmotor för arbetsflöden för hög tillförlitliga och hög tillgänglighet som kan skalas toomeet dina behov när organisationen växer.</span><span class="sxs-lookup"><span data-stu-id="1c253-108">Azure Automation provides a highly-reliable and highly-available workflow execution engine that scales toomeet your needs as your organization grows.</span></span> <span data-ttu-id="1c253-109">I Azure Automation kan processer vara inletts manuellt av 3 part eller med schemalagda intervall så att aktiviteter inträffa exakt vid behov.</span><span class="sxs-lookup"><span data-stu-id="1c253-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="1c253-110">Lägre operativ tillsyn och frigöra IT / DevOps personal toofocus på arbete som lägger till affärsvärde genom att flytta dina molntjänster management uppgifter toobe körs automatiskt av Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="1c253-110">Lower operational overhead and free up IT / DevOps staff toofocus on work that adds business value by moving your cloud management tasks toobe run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-storage"></a><span data-ttu-id="1c253-111">Hur Azure Automation kan hantera Azure Storage?</span><span class="sxs-lookup"><span data-stu-id="1c253-111">How can Azure Automation help manage Azure Storage?</span></span>
<span data-ttu-id="1c253-112">Azure Storage kan hanteras i Azure Automation med hello PowerShell-cmdlets som är tillgängliga i [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span><span class="sxs-lookup"><span data-stu-id="1c253-112">Azure Storage can be managed in Azure Automation by using hello PowerShell cmdlets that are available in [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span></span> <span data-ttu-id="1c253-113">Azure Automation har dessa lagring PowerShell-cmdlets som är tillgängligt out of box hello, så att du kan utföra alla blob, tabell och kön hanteringsuppgifter i hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1c253-113">Azure Automation has these Storage PowerShell cmdlets available out of hello box, so that you can perform all of your blob, table, and queue management tasks within hello service.</span></span> <span data-ttu-id="1c253-114">Du kan också koppla dessa cmdlets i Azure Automation med hello-cmdlets för andra Azure-tjänster, tooautomate komplicerade uppgifter över Azure-tjänster och 3 part.</span><span class="sxs-lookup"><span data-stu-id="1c253-114">You can also pair these cmdlets in Azure Automation with hello cmdlets for other Azure services, tooautomate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="1c253-115">Hej [Azure Automation runbook-galleriet](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) innehåller en mängd produkten team och community runbooks tooget igång automatisering av hantering av Azure Storage, andra Azure-tjänster och 3 part.</span><span class="sxs-lookup"><span data-stu-id="1c253-115">hello [Azure Automation runbook gallery](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) contains a variety of product team and community runbooks tooget started automating management of Azure Storage, other Azure services, and 3rd party systems.</span></span> <span data-ttu-id="1c253-116">Galleriet runbooks är:</span><span class="sxs-lookup"><span data-stu-id="1c253-116">Gallery runbooks include:</span></span>

* [<span data-ttu-id="1c253-117">Ta bort Azure Storage-Blobbar som är vissa dagar gamla med hjälp av Automation-tjänsten</span><span class="sxs-lookup"><span data-stu-id="1c253-117">Remove Azure Storage Blobs that are Certain Days Old Using Automation Service</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Remove-Storage-Blobs-that-aae4b761)
* [<span data-ttu-id="1c253-118">Hämta en Blob från Azure Storage</span><span class="sxs-lookup"><span data-stu-id="1c253-118">Download a Blob from Azure Storage</span></span>](https://gallery.technet.microsoft.com/scriptcenter/a-Blob-from-Azure-Storage-6bc13745)
* [<span data-ttu-id="1c253-119">Säkerhetskopiera alla diskar för en enskild virtuell dator i Azure eller för alla virtuella datorer i en tjänst i molnet</span><span class="sxs-lookup"><span data-stu-id="1c253-119">Backup all disks for a single Azure VM or for all VMs in a Cloud Service</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Backup-all-disks-for-a-ede940d5)

## <a name="next-steps"></a><span data-ttu-id="1c253-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1c253-120">Next Steps</span></span>
<span data-ttu-id="1c253-121">Nu när du har lärt dig grunderna hello i Azure Automation och hur det kan vara används toomanage Azure Storage-blobbar, tabeller och köer kan du följa dessa länkar toolearn mer om Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="1c253-121">Now that you've learned hello basics of Azure Automation and how it can be used toomanage Azure Storage blobs, tables, and queues, follow these links toolearn more about Azure Automation.</span></span>

<span data-ttu-id="1c253-122">Se hello Azure Automation-genomgång [skapa eller importera en runbook i Azure Automation](../../automation/automation-creating-importing-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="1c253-122">See hello Azure Automation tutorial [Creating or importing a runbook in Azure Automation](../../automation/automation-creating-importing-runbook.md).</span></span>

