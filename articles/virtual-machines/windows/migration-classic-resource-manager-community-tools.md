---
title: aaaCommunity-verktyg, flytta klassiska resurser tooAzure Resource Manager | Microsoft Docs
description: "Den här artikeln kataloger hello verktyg som tillhandahålls av hello community toohelp migrera IaaS-resurser från klassiska toohello Azure Resource Manager-distributionsmodellen."
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 228b697b-3950-49f5-84bb-283bb56621b1
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: 67d5624a14d12a2d9eb46eb12aef461b706d4589
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="community-tools-toomigrate-iaas-resources-from-classic-tooazure-resource-manager"></a><span data-ttu-id="11c96-103">Gemenskapen verktygen toomigrate IaaS-resurser från klassiska tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="11c96-103">Community tools toomigrate IaaS resources from classic tooAzure Resource Manager</span></span>
<span data-ttu-id="11c96-104">Den här artikeln kataloger hello verktyg som tillhandahålls av hello community tooassist migreringen av IaaS-resurser från klassiska toohello Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="11c96-104">This article catalogs hello tools that have been provided by hello community tooassist with migration of IaaS resources from classic toohello Azure Resource Manager deployment model.</span></span>

> [!NOTE]
> <span data-ttu-id="11c96-105">Dessa verktyg officiellt stöds inte av Microsoft-supporten.</span><span class="sxs-lookup"><span data-stu-id="11c96-105">These tools are not officially supported by Microsoft Support.</span></span> <span data-ttu-id="11c96-106">Därför är öppen källkod på GitHub och vi är glada tooaccept fso för korrigeringar eller fler scenarier.</span><span class="sxs-lookup"><span data-stu-id="11c96-106">Therefore they are open sourced on GitHub and we're happy tooaccept PRs for fixes or additional scenarios.</span></span> <span data-ttu-id="11c96-107">tooreport ett problem med hello GitHub utfärdar funktionen.</span><span class="sxs-lookup"><span data-stu-id="11c96-107">tooreport an issue, use hello GitHub issues feature.</span></span>
> 
> <span data-ttu-id="11c96-108">Migrera med de här verktygen kommer driftstopp för dina klassiska virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="11c96-108">Migrating with these tools will cause downtime for your classic Virtual Machine.</span></span> <span data-ttu-id="11c96-109">Om du letar efter migrering plattform som stöds finns</span><span class="sxs-lookup"><span data-stu-id="11c96-109">If you're looking for platform supported migration, visit</span></span> 
> 
>   * [<span data-ttu-id="11c96-110">Migrering av plattform som stöds av IaaS-resurser från klassiska tooAzure Resource Manager-stacken</span><span class="sxs-lookup"><span data-stu-id="11c96-110">Platform supported migration of IaaS resources from Classic tooAzure Resource Manager stack</span></span>](migration-classic-resource-manager-overview.md)
>   * [<span data-ttu-id="11c96-111">Tekniska ingående om plattformen stöds migreringen från klassiskt tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="11c96-111">Technical Deep Dive on Platform supported migration from Classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-deep-dive.md)
>   * [<span data-ttu-id="11c96-112">Migrera IaaS-resurser från klassiska tooAzure Resource Manager med Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="11c96-112">Migrate IaaS resources from Classic tooAzure Resource Manager using Azure PowerShell</span></span>](migration-classic-resource-manager-ps.md)
> 
> 

## <a name="asmmetadataparser"></a><span data-ttu-id="11c96-113">AsmMetadataParser</span><span class="sxs-lookup"><span data-stu-id="11c96-113">AsmMetadataParser</span></span>
<span data-ttu-id="11c96-114">Detta är en samling helper-verktyg som skapats som en del av enterprise migrering från Azure-tjänsthantering tooAzure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="11c96-114">This is a collection of helper tools created as part of enterprise migrations from Azure Service Management tooAzure Resource Manager.</span></span> <span data-ttu-id="11c96-115">Det här verktyget kan du tooreplicate infrastrukturen till en annan prenumeration som kan användas för att testa migrering och Stryk eventuella problem innan du kör hello migrering på prenumerationen i produktion.</span><span class="sxs-lookup"><span data-stu-id="11c96-115">This tool allows you tooreplicate your infrastructure into another subscription which can be used for testing migration and iron out any issues before running hello migration on your Production subscription.</span></span>

[<span data-ttu-id="11c96-116">Länken toohello verktyget dokumentation</span><span class="sxs-lookup"><span data-stu-id="11c96-116">Link toohello tool documentation</span></span>](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/AsmToArmMigrationApiToolset)

## <a name="migaz"></a><span data-ttu-id="11c96-117">migAz</span><span class="sxs-lookup"><span data-stu-id="11c96-117">migAz</span></span>
<span data-ttu-id="11c96-118">migAz är en ytterligare alternativ toomigrate en fullständig uppsättning klassiska IaaS resurser tooAzure Resource Manager IaaS-resurser.</span><span class="sxs-lookup"><span data-stu-id="11c96-118">migAz is an additional option toomigrate a complete set of classic IaaS resources tooAzure Resource Manager IaaS resources.</span></span> <span data-ttu-id="11c96-119">hello migrering kan ske inom hello samma prenumeration eller mellan olika prenumerationer och prenumerationstyper (ex: CSP prenumerationer).</span><span class="sxs-lookup"><span data-stu-id="11c96-119">hello migration can occur within hello same subscription or between different subscriptions and subscription types (ex: CSP subscriptions).</span></span>

[<span data-ttu-id="11c96-120">Länken toohello verktyget dokumentation</span><span class="sxs-lookup"><span data-stu-id="11c96-120">Link toohello tool documentation</span></span>](https://github.com/Azure/migAz)

## <a name="next-steps"></a><span data-ttu-id="11c96-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="11c96-121">Next Steps</span></span>

* [<span data-ttu-id="11c96-122">Översikt över plattformar som stöds migrering av IaaS-resurser från klassiska tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="11c96-122">Overview of platform-supported migration of IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="11c96-123">Tekniska ingående om plattformen stöds från klassiska tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="11c96-123">Technical deep dive on platform-supported migration from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="11c96-124">Planera för migrering av IaaS-resurser från klassiska tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="11c96-124">Planning for migration of IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="11c96-125">Använd PowerShell toomigrate IaaS-resurser från klassiska tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="11c96-125">Use PowerShell toomigrate IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="11c96-126">Använd CLI toomigrate IaaS-resurser från klassiska tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="11c96-126">Use CLI toomigrate IaaS resources from classic tooAzure Resource Manager</span></span>](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="11c96-127">Granska de vanligaste migreringsfelen</span><span class="sxs-lookup"><span data-stu-id="11c96-127">Review most common migration errors</span></span>](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="11c96-128">Granska hello vanliga mest frågor om migrera IaaS-resurser från klassiska tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="11c96-128">Review hello most frequently asked questions about migrating IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

