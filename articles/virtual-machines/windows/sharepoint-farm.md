---
title: Skapa SharePoint-servergrupper i Azure | Microsoft Docs
description: "Snabbt skapa en ny servergrupp för SharePoint 2013 eller SharePoint 2016 i Azure med hjälp av Azure portal marketplace."
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
ms.openlocfilehash: 00648ee35962b22fb7eeceaa6d9df7305c801abf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-sharepoint-server-farms-using-the-azure-portal-marketplace"></a><span data-ttu-id="b52e5-103">Skapa SharePoint-servergrupper med hjälp av Azure portal marketplace</span><span class="sxs-lookup"><span data-stu-id="b52e5-103">Create SharePoint server farms using the Azure portal marketplace</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="sharepoint-2013-farms"></a><span data-ttu-id="b52e5-104">SharePoint 2013-servergrupper</span><span class="sxs-lookup"><span data-stu-id="b52e5-104">SharePoint 2013 farms</span></span>
<span data-ttu-id="b52e5-105">Med Microsoft Azure portal marketplace, kan du snabbt skapa förkonfigurerade SharePoint Server 2013-servergrupper.</span><span class="sxs-lookup"><span data-stu-id="b52e5-105">With the Microsoft Azure portal marketplace, you can quickly create pre-configured SharePoint Server 2013 farms.</span></span> <span data-ttu-id="b52e5-106">Detta kan spara mycket tid när du behöver en grundläggande eller hög tillgänglighet SharePoint-grupp för en miljö för utveckling och testning eller om du utvärderar SharePoint Server 2013 som en lösning för samarbete för din organisation.</span><span class="sxs-lookup"><span data-stu-id="b52e5-106">This can save you a lot of time when you need a basic or high-availability SharePoint farm for a dev/test environment or if you are evaluating SharePoint Server 2013 as a collaboration solution for your organization.</span></span>

> [!NOTE]
> <span data-ttu-id="b52e5-107">Den **SharePoint-servergrupp** objektet i Azure Marketplace för Azure-portalen har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="b52e5-107">The **SharePoint Server Farm** item in the Azure Marketplace of the Azure portal has been removed.</span></span> <span data-ttu-id="b52e5-108">Den har ersatts med den **SharePoint 2013 icke - hög tillgänglighet-grupp** och **SharePoint 2013-grupp för hög tillgänglighet** objekt.</span><span class="sxs-lookup"><span data-stu-id="b52e5-108">It has been replaced with the **SharePoint 2013 non-HA Farm** and **SharePoint 2013 HA Farm** items.</span></span>
>
>

<span data-ttu-id="b52e5-109">Den grundläggande SharePoint-gruppen består av tre virtuella datorer i den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="b52e5-109">The basic SharePoint farm consists of three virtual machines in this configuration.</span></span>

![sharepointfarm](./media/sharepoint-farm/Non-HAFarm.png)

<span data-ttu-id="b52e5-111">Du kan använda den här gruppkonfigurationen för en förenklad inställning för utveckling av appar i SharePoint eller första gången utvärderingen av SharePoint 2013.</span><span class="sxs-lookup"><span data-stu-id="b52e5-111">You can use this farm configuration for a simplified setup for SharePoint app development or your first-time evaluation of SharePoint 2013.</span></span>

<span data-ttu-id="b52e5-112">För att skapa den grundläggande (tre servrar) SharePoint-gruppen:</span><span class="sxs-lookup"><span data-stu-id="b52e5-112">To create the basic (three-server) SharePoint farm:</span></span>

1. <span data-ttu-id="b52e5-113">Klicka på [här](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).</span><span class="sxs-lookup"><span data-stu-id="b52e5-113">Click [here](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).</span></span>
2. <span data-ttu-id="b52e5-114">Klicka på **distribuera**.</span><span class="sxs-lookup"><span data-stu-id="b52e5-114">Click **Deploy**.</span></span>
3. <span data-ttu-id="b52e5-115">På den **SharePoint 2013 icke - hög tillgänglighet-grupp** rutan klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="b52e5-115">On the **SharePoint 2013 non-HA Farm** pane, click **Create**.</span></span>
4. <span data-ttu-id="b52e5-116">Ange inställningar på stegen i den **skapa SharePoint 2013 icke - hög tillgänglighet-grupp** rutan och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="b52e5-116">Specify settings on the steps of the **Create SharePoint 2013 non-HA Farm** pane, and then click **Create**.</span></span>

<span data-ttu-id="b52e5-117">Hög tillgänglighet SharePoint-grupp består av nio virtuella datorer i den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="b52e5-117">The high-availability SharePoint farm consists of nine virtual machines in this configuration.</span></span>

![sharepointfarm](./media/sharepoint-farm/HAFarm.png)

<span data-ttu-id="b52e5-119">Du kan använda den här konfigurationen för servergruppen för att testa högre belastning på klienten, hög tillgänglighet för externa SharePoint-webbplatsen och SQL Server AlwaysOn-Tillgänglighetsgrupper för en SharePoint-servergrupp.</span><span class="sxs-lookup"><span data-stu-id="b52e5-119">You can use this farm configuration to test higher client loads, high availability of the external SharePoint site, and SQL Server AlwaysOn Availability Groups for a SharePoint farm.</span></span> <span data-ttu-id="b52e5-120">Du kan också använda den här konfigurationen för utveckling av appar i SharePoint i en miljö med hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="b52e5-120">You can also use this configuration for SharePoint app development in a high-availability environment.</span></span>

<span data-ttu-id="b52e5-121">Skapa SharePoint-servergruppen hög tillgänglighet (nio-server):</span><span class="sxs-lookup"><span data-stu-id="b52e5-121">To create the high-availability (nine-server) SharePoint farm:</span></span>

1. <span data-ttu-id="b52e5-122">Klicka på [här](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).</span><span class="sxs-lookup"><span data-stu-id="b52e5-122">Click [here](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).</span></span>
2. <span data-ttu-id="b52e5-123">Klicka på **distribuera**.</span><span class="sxs-lookup"><span data-stu-id="b52e5-123">Click **Deploy**.</span></span>
3. <span data-ttu-id="b52e5-124">På den **SharePoint 2013-grupp för hög tillgänglighet** rutan klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="b52e5-124">On the **SharePoint 2013 HA Farm** pane, click **Create**.</span></span>
4. <span data-ttu-id="b52e5-125">Ange inställningar på sju steg för den **skapa SharePoint 2013 hög tillgänglighet-grupp** rutan och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="b52e5-125">Specify settings on the seven steps of the **Create SharePoint 2013 HA Farm** pane, and then click **Create**.</span></span>

> [!NOTE]
> <span data-ttu-id="b52e5-126">Du kan inte skapa den **SharePoint 2013 icke - hög tillgänglighet-grupp** eller **SharePoint 2013-grupp för hög tillgänglighet** med en kostnadsfri utvärderingsversion av Azure.</span><span class="sxs-lookup"><span data-stu-id="b52e5-126">You cannot create the **SharePoint 2013 non-HA Farm** or **SharePoint 2013 HA Farm** with an Azure Free Trial.</span></span>
>
>

<span data-ttu-id="b52e5-127">Azure-portalen skapar båda dessa grupper i en endast molnbaserad virtuellt nätverk med en Internet-riktade webben.</span><span class="sxs-lookup"><span data-stu-id="b52e5-127">The Azure portal creates both of these farms in a cloud-only virtual network with an Internet-facing web presence.</span></span> <span data-ttu-id="b52e5-128">Det finns ingen plats-till-plats VPN eller ExpressRoute-anslutning till organisationens nätverk.</span><span class="sxs-lookup"><span data-stu-id="b52e5-128">There is no site-to-site VPN or ExpressRoute connection back to your organization network.</span></span>

> [!NOTE]
> <span data-ttu-id="b52e5-129">När du skapar grundläggande eller hög tillgänglighet SharePoint-servergrupper med Azure-portalen, du kan inte ange en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="b52e5-129">When you create the basic or high-availability SharePoint farms using the Azure portal, you cannot specify an existing resource group.</span></span> <span data-ttu-id="b52e5-130">Skapa dessa grupper med Azure PowerShell för att kringgå den här begränsningen.</span><span class="sxs-lookup"><span data-stu-id="b52e5-130">To work around this limitation, create these farms with Azure PowerShell.</span></span> <span data-ttu-id="b52e5-131">Mer information finns i [skapa SharePoint 2013 utveckling och testning i grupper med Azure PowerShell](https://technet.microsoft.com/library/mt743093.aspx#powershell).</span><span class="sxs-lookup"><span data-stu-id="b52e5-131">For more information, see [Create SharePoint 2013 dev/test farms with Azure PowerShell](https://technet.microsoft.com/library/mt743093.aspx#powershell).</span></span>
>
>

## <a name="sharepoint-2016-farms"></a><span data-ttu-id="b52e5-132">SharePoint 2016 servergrupper</span><span class="sxs-lookup"><span data-stu-id="b52e5-132">SharePoint 2016 farms</span></span>
<span data-ttu-id="b52e5-133">Se [i den här artikeln](https://technet.microsoft.com/library/mt723354.aspx) anvisningar att skapa följande enskild server SharePoint Server 2016-grupp.</span><span class="sxs-lookup"><span data-stu-id="b52e5-133">See [this article](https://technet.microsoft.com/library/mt723354.aspx) for the instructions to build the following single-server SharePoint Server 2016 farm.</span></span>

![sharepointfarm](./media/sharepoint-farm/SP2016Farm.png)

## <a name="managing-the-sharepoint-farms"></a><span data-ttu-id="b52e5-135">Hantera SharePoint-servergrupper</span><span class="sxs-lookup"><span data-stu-id="b52e5-135">Managing the SharePoint farms</span></span>
<span data-ttu-id="b52e5-136">Du kan administrera servrar i dessa grupper via anslutningar till fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="b52e5-136">You can administer the servers of these farms through Remote Desktop connections.</span></span> <span data-ttu-id="b52e5-137">Mer information finns i [logga in på den virtuella datorn](quick-create-portal.md#connect-to-virtual-machine).</span><span class="sxs-lookup"><span data-stu-id="b52e5-137">For more information, see [Log on to the virtual machine](quick-create-portal.md#connect-to-virtual-machine).</span></span>

<span data-ttu-id="b52e5-138">Du kan konfigurera min platser, SharePoint-program och andra funktioner i Central Administration av SharePoint-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="b52e5-138">From the Central Administration SharePoint site, you can configure My sites, SharePoint applications, and other functionality.</span></span> <span data-ttu-id="b52e5-139">Mer information finns i [Konfigurera SharePoint](http://technet.microsoft.com/library/ee836142.aspx).</span><span class="sxs-lookup"><span data-stu-id="b52e5-139">For more information, see [Configure SharePoint](http://technet.microsoft.com/library/ee836142.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b52e5-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b52e5-140">Next steps</span></span>
* <span data-ttu-id="b52e5-141">Identifiera ytterligare [SharePoint konfigurationer](https://technet.microsoft.com/library/dn635309.aspx) i Azure infrastrukturtjänster.</span><span class="sxs-lookup"><span data-stu-id="b52e5-141">Discover additional [SharePoint configurations](https://technet.microsoft.com/library/dn635309.aspx) in Azure infrastructure services.</span></span>
