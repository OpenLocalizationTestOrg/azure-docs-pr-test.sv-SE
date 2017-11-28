---
title: aaaCreate SharePoint-servergrupper i Azure | Microsoft Docs
description: "Snabbt skapa en ny servergrupp för SharePoint 2013 eller SharePoint 2016 i Azure med hjälp av hello Azure portal marketplace."
services: virtual-machines-windows
documentationcenter: 
author: JoeDavies-MSFT
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 89b124da-019d-4179-86dd-ad418d05a4f2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: josephd
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d71c7177d9b99cf377bab767342508007285b452
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-sharepoint-server-farms-using-hello-azure-portal-marketplace"></a><span data-ttu-id="9a376-103">Skapa SharePoint-servergrupper med hello Azure portal marketplace</span><span class="sxs-lookup"><span data-stu-id="9a376-103">Create SharePoint server farms using hello Azure portal marketplace</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="sharepoint-2013-farms"></a><span data-ttu-id="9a376-104">SharePoint 2013-servergrupper</span><span class="sxs-lookup"><span data-stu-id="9a376-104">SharePoint 2013 farms</span></span>
<span data-ttu-id="9a376-105">Med marketplace-hello Microsoft Azure portal, kan du snabbt skapa förkonfigurerade SharePoint Server 2013-servergrupper.</span><span class="sxs-lookup"><span data-stu-id="9a376-105">With hello Microsoft Azure portal marketplace, you can quickly create pre-configured SharePoint Server 2013 farms.</span></span> <span data-ttu-id="9a376-106">Detta kan spara mycket tid när du behöver en grundläggande eller hög tillgänglighet SharePoint-grupp för en miljö för utveckling och testning eller om du utvärderar SharePoint Server 2013 som en lösning för samarbete för din organisation.</span><span class="sxs-lookup"><span data-stu-id="9a376-106">This can save you a lot of time when you need a basic or high-availability SharePoint farm for a dev/test environment or if you are evaluating SharePoint Server 2013 as a collaboration solution for your organization.</span></span>

> [!NOTE]
> <span data-ttu-id="9a376-107">Hej **SharePoint-servergrupp** objektet i hello Azure Marketplace för hello Azure-portalen har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="9a376-107">hello **SharePoint Server Farm** item in hello Azure Marketplace of hello Azure portal has been removed.</span></span> <span data-ttu-id="9a376-108">Den har ersatts med hello **SharePoint 2013 icke - hög tillgänglighet-grupp** och **SharePoint 2013-grupp för hög tillgänglighet** objekt.</span><span class="sxs-lookup"><span data-stu-id="9a376-108">It has been replaced with hello **SharePoint 2013 non-HA Farm** and **SharePoint 2013 HA Farm** items.</span></span>
>
>

<span data-ttu-id="9a376-109">hello grundläggande SharePoint-grupp består av tre virtuella datorer i den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="9a376-109">hello basic SharePoint farm consists of three virtual machines in this configuration.</span></span>

![sharepointfarm](./media/sharepoint-farm/Non-HAFarm.png)

<span data-ttu-id="9a376-111">Du kan använda den här gruppkonfigurationen för en förenklad inställning för utveckling av appar i SharePoint eller första gången utvärderingen av SharePoint 2013.</span><span class="sxs-lookup"><span data-stu-id="9a376-111">You can use this farm configuration for a simplified setup for SharePoint app development or your first-time evaluation of SharePoint 2013.</span></span>

<span data-ttu-id="9a376-112">toocreate hello grundläggande () SharePoint grupp med tre servrar:</span><span class="sxs-lookup"><span data-stu-id="9a376-112">toocreate hello basic (three-server) SharePoint farm:</span></span>

1. <span data-ttu-id="9a376-113">Klicka på [här](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).</span><span class="sxs-lookup"><span data-stu-id="9a376-113">Click [here](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).</span></span>
2. <span data-ttu-id="9a376-114">Klicka på **distribuera**.</span><span class="sxs-lookup"><span data-stu-id="9a376-114">Click **Deploy**.</span></span>
3. <span data-ttu-id="9a376-115">På hello **SharePoint 2013 icke - hög tillgänglighet-grupp** rutan klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="9a376-115">On hello **SharePoint 2013 non-HA Farm** pane, click **Create**.</span></span>
4. <span data-ttu-id="9a376-116">Ange inställningar på hello stegen för hello **skapa SharePoint 2013 icke - hög tillgänglighet-grupp** rutan och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="9a376-116">Specify settings on hello steps of hello **Create SharePoint 2013 non-HA Farm** pane, and then click **Create**.</span></span>

<span data-ttu-id="9a376-117">hello hög tillgänglighet SharePoint-grupp består av nio virtuella datorer i den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="9a376-117">hello high-availability SharePoint farm consists of nine virtual machines in this configuration.</span></span>

![sharepointfarm](./media/sharepoint-farm/HAFarm.png)

<span data-ttu-id="9a376-119">Du kan använda den här gruppen configuration tootest högre klienten belastning, hög tillgänglighet för hello externa SharePoint-webbplatsen och SQL Server AlwaysOn-Tillgänglighetsgrupper för en SharePoint-servergrupp.</span><span class="sxs-lookup"><span data-stu-id="9a376-119">You can use this farm configuration tootest higher client loads, high availability of hello external SharePoint site, and SQL Server AlwaysOn Availability Groups for a SharePoint farm.</span></span> <span data-ttu-id="9a376-120">Du kan också använda den här konfigurationen för utveckling av appar i SharePoint i en miljö med hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="9a376-120">You can also use this configuration for SharePoint app development in a high-availability environment.</span></span>

<span data-ttu-id="9a376-121">toocreate hello hög tillgänglighet (nio-server) SharePoint-grupp:</span><span class="sxs-lookup"><span data-stu-id="9a376-121">toocreate hello high-availability (nine-server) SharePoint farm:</span></span>

1. <span data-ttu-id="9a376-122">Klicka på [här](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).</span><span class="sxs-lookup"><span data-stu-id="9a376-122">Click [here](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).</span></span>
2. <span data-ttu-id="9a376-123">Klicka på **distribuera**.</span><span class="sxs-lookup"><span data-stu-id="9a376-123">Click **Deploy**.</span></span>
3. <span data-ttu-id="9a376-124">På hello **SharePoint 2013-grupp för hög tillgänglighet** rutan klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="9a376-124">On hello **SharePoint 2013 HA Farm** pane, click **Create**.</span></span>
4. <span data-ttu-id="9a376-125">Ange inställningar på hello sju steg som krävs för hello **skapa SharePoint 2013 hög tillgänglighet-grupp** rutan och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="9a376-125">Specify settings on hello seven steps of hello **Create SharePoint 2013 HA Farm** pane, and then click **Create**.</span></span>

> [!NOTE]
> <span data-ttu-id="9a376-126">Du kan inte skapa hello **SharePoint 2013 icke - hög tillgänglighet-grupp** eller **SharePoint 2013-grupp för hög tillgänglighet** med en kostnadsfri utvärderingsversion av Azure.</span><span class="sxs-lookup"><span data-stu-id="9a376-126">You cannot create hello **SharePoint 2013 non-HA Farm** or **SharePoint 2013 HA Farm** with an Azure Free Trial.</span></span>
>
>

<span data-ttu-id="9a376-127">hello Azure-portalen skapar båda dessa grupper i en endast molnbaserad virtuellt nätverk med en Internet-riktade webben.</span><span class="sxs-lookup"><span data-stu-id="9a376-127">hello Azure portal creates both of these farms in a cloud-only virtual network with an Internet-facing web presence.</span></span> <span data-ttu-id="9a376-128">Det finns ingen plats-till-plats VPN eller ExpressRoute-anslutning tillbaka tooyour organisationens nätverk.</span><span class="sxs-lookup"><span data-stu-id="9a376-128">There is no site-to-site VPN or ExpressRoute connection back tooyour organization network.</span></span>

> [!NOTE]
> <span data-ttu-id="9a376-129">När du skapar hello grundläggande eller hello Azure-portalen med hög tillgänglighet SharePoint-servergrupper, du kan inte ange en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="9a376-129">When you create hello basic or high-availability SharePoint farms using hello Azure portal, you cannot specify an existing resource group.</span></span> <span data-ttu-id="9a376-130">toowork runt den här begränsningen kan skapa dessa grupper med Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9a376-130">toowork around this limitation, create these farms with Azure PowerShell.</span></span> <span data-ttu-id="9a376-131">Mer information finns i [skapa SharePoint 2013 utveckling och testning i grupper med Azure PowerShell](https://technet.microsoft.com/library/mt743093.aspx#powershell).</span><span class="sxs-lookup"><span data-stu-id="9a376-131">For more information, see [Create SharePoint 2013 dev/test farms with Azure PowerShell](https://technet.microsoft.com/library/mt743093.aspx#powershell).</span></span>
>
>

## <a name="sharepoint-2016-farms"></a><span data-ttu-id="9a376-132">SharePoint 2016 servergrupper</span><span class="sxs-lookup"><span data-stu-id="9a376-132">SharePoint 2016 farms</span></span>
<span data-ttu-id="9a376-133">Se [i den här artikeln](https://technet.microsoft.com/library/mt723354.aspx) hello instruktioner toobuild hello efter enskild server SharePoint Server 2016 servergruppen.</span><span class="sxs-lookup"><span data-stu-id="9a376-133">See [this article](https://technet.microsoft.com/library/mt723354.aspx) for hello instructions toobuild hello following single-server SharePoint Server 2016 farm.</span></span>

![sharepointfarm](./media/sharepoint-farm/SP2016Farm.png)

## <a name="managing-hello-sharepoint-farms"></a><span data-ttu-id="9a376-135">Hantera hello SharePoint-servergrupper</span><span class="sxs-lookup"><span data-stu-id="9a376-135">Managing hello SharePoint farms</span></span>
<span data-ttu-id="9a376-136">Du kan administrera hello servrar av dessa anläggningar via anslutningar till fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="9a376-136">You can administer hello servers of these farms through Remote Desktop connections.</span></span> <span data-ttu-id="9a376-137">Mer information finns i [inloggning toohello virtuella](quick-create-portal.md#connect-to-virtual-machine).</span><span class="sxs-lookup"><span data-stu-id="9a376-137">For more information, see [Log on toohello virtual machine](quick-create-portal.md#connect-to-virtual-machine).</span></span>

<span data-ttu-id="9a376-138">Du kan konfigurera min platser, SharePoint-program och andra funktioner hello Central Administration av SharePoint-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="9a376-138">From hello Central Administration SharePoint site, you can configure My sites, SharePoint applications, and other functionality.</span></span> <span data-ttu-id="9a376-139">Mer information finns i [Konfigurera SharePoint](http://technet.microsoft.com/library/ee836142.aspx).</span><span class="sxs-lookup"><span data-stu-id="9a376-139">For more information, see [Configure SharePoint](http://technet.microsoft.com/library/ee836142.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9a376-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9a376-140">Next steps</span></span>
* <span data-ttu-id="9a376-141">Identifiera ytterligare [SharePoint konfigurationer](https://technet.microsoft.com/library/dn635309.aspx) i Azure infrastrukturtjänster.</span><span class="sxs-lookup"><span data-stu-id="9a376-141">Discover additional [SharePoint configurations](https://technet.microsoft.com/library/dn635309.aspx) in Azure infrastructure services.</span></span>
