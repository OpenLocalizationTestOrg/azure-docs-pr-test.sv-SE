---
title: "aaaExample Azure Infrastructure genomgången | Microsoft Docs"
description: "Läs mer om hello viktiga design och implementeringslösning riktlinjer för att distribuera en exempel-infrastruktur i Azure."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 281fc2c0-b533-45fa-81a3-728c0049c73d
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b6b307c0112203aa83e1fada81966fc265397a40
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="example-azure-infrastructure-walkthrough-for-linux-vms"></a><span data-ttu-id="177c1-103">Exemplet Azure-infrastrukturen genomgång för virtuella Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="177c1-103">Example Azure infrastructure walkthrough for Linux VMs</span></span>

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

<span data-ttu-id="177c1-104">Den här artikeln beskriver hur bygga ut infrastruktur för ett exempel.</span><span class="sxs-lookup"><span data-stu-id="177c1-104">This article walks through building out an example application infrastructure.</span></span> <span data-ttu-id="177c1-105">Vi i detalj utformar en infrastruktur för en enkel onlinebutik som sammanför alla hello riktlinjer och beslut runt namngivningskonventioner, tillgänglighetsuppsättningar, virtuella nätverk och belastningsutjämnare och faktiskt distribuerar virtuella datorer (VM).</span><span class="sxs-lookup"><span data-stu-id="177c1-105">We detail designing an infrastructure for a simple on-line store that brings together all hello guidelines and decisions around naming conventions, availability sets, virtual networks and load balancers, and actually deploying your virtual machines (VMs).</span></span>

## <a name="example-workload"></a><span data-ttu-id="177c1-106">Exempel arbetsbelastning</span><span class="sxs-lookup"><span data-stu-id="177c1-106">Example workload</span></span>
<span data-ttu-id="177c1-107">Adventure Works Cycles vill toobuild en onlinebutik program i Azure som består av:</span><span class="sxs-lookup"><span data-stu-id="177c1-107">Adventure Works Cycles wants toobuild an on-line store application in Azure that consists of:</span></span>

* <span data-ttu-id="177c1-108">Två nginx-servrar som kör hello klienten klientdelen i en webbnivå</span><span class="sxs-lookup"><span data-stu-id="177c1-108">Two nginx servers running hello client front-end in a web tier</span></span>
* <span data-ttu-id="177c1-109">Två nginx-servrar bearbetar data och order i en program-nivå</span><span class="sxs-lookup"><span data-stu-id="177c1-109">Two nginx servers processing data and orders in an application tier</span></span>
* <span data-ttu-id="177c1-110">Två MongoDB-servrar ingår i ett delat kluster för att lagra produktdata och order i en databasnivå</span><span class="sxs-lookup"><span data-stu-id="177c1-110">Two MongoDB servers part of a sharded cluster for storing product data and orders in a database tier</span></span>
* <span data-ttu-id="177c1-111">Två Active Directory-domänkontrollanter för kundkonton och leverantörer i en nivå för autentisering</span><span class="sxs-lookup"><span data-stu-id="177c1-111">Two Active Directory domain controllers for customer accounts and suppliers in an authentication tier</span></span>
* <span data-ttu-id="177c1-112">Alla hello-servrar finns i två undernät:</span><span class="sxs-lookup"><span data-stu-id="177c1-112">All hello servers are located in two subnets:</span></span>
  * <span data-ttu-id="177c1-113">en frontend-undernät för hello webbservrar</span><span class="sxs-lookup"><span data-stu-id="177c1-113">a front-end subnet for hello web servers</span></span> 
  * <span data-ttu-id="177c1-114">en backend-undernät för hello programservrar, MongoDB-kluster och domänkontrollanter</span><span class="sxs-lookup"><span data-stu-id="177c1-114">a back-end subnet for hello application servers, MongoDB cluster, and domain controllers</span></span>

![Diagram över olika nivåer för infrastruktur](./media/infrastructure-example/example-tiers.png)

<span data-ttu-id="177c1-116">Inkommande säker webbtrafik måste vara belastningsutjämnad bland hello webbservrar som kunder Bläddra hello onlinebutik.</span><span class="sxs-lookup"><span data-stu-id="177c1-116">Incoming secure web traffic must be load-balanced among hello web servers as customers browse hello on-line store.</span></span> <span data-ttu-id="177c1-117">Ordning bearbetar trafik i hello form av HTTP-begäranden från hello web servrar måste vara belastningsutjämnad bland hello programservrar.</span><span class="sxs-lookup"><span data-stu-id="177c1-117">Order processing traffic in hello form of HTTP requests from hello web servers must be load-balanced among hello application servers.</span></span> <span data-ttu-id="177c1-118">Dessutom måste hello infrastrukturen utformas för hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="177c1-118">Additionally, hello infrastructure must be designed for high availability.</span></span>

<span data-ttu-id="177c1-119">resulterande hello-design måste innehålla:</span><span class="sxs-lookup"><span data-stu-id="177c1-119">hello resulting design must incorporate:</span></span>

* <span data-ttu-id="177c1-120">Ett konto och Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="177c1-120">An Azure subscription and account</span></span>
* <span data-ttu-id="177c1-121">En enskild resursgrupp</span><span class="sxs-lookup"><span data-stu-id="177c1-121">A single resource group</span></span>
* <span data-ttu-id="177c1-122">Azure Managed Disks</span><span class="sxs-lookup"><span data-stu-id="177c1-122">Azure Managed Disks</span></span>
* <span data-ttu-id="177c1-123">Ett virtuellt nätverk med två undernät</span><span class="sxs-lookup"><span data-stu-id="177c1-123">A virtual network with two subnets</span></span>
* <span data-ttu-id="177c1-124">Tillgänglighetsuppsättningar för hello virtuella datorer med en liknande roll</span><span class="sxs-lookup"><span data-stu-id="177c1-124">Availability sets for hello VMs with a similar role</span></span>
* <span data-ttu-id="177c1-125">Virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="177c1-125">Virtual machines</span></span>

<span data-ttu-id="177c1-126">Alla hello ovan följer dessa namngivningsregler:</span><span class="sxs-lookup"><span data-stu-id="177c1-126">All hello above follow these naming conventions:</span></span>

* <span data-ttu-id="177c1-127">Adventure Works Cycles använder **[IT arbetsbelastning]-[plats]-[Azure resurs]** som ett prefix</span><span class="sxs-lookup"><span data-stu-id="177c1-127">Adventure Works Cycles uses **[IT workload]-[location]-[Azure resource]** as a prefix</span></span>
  * <span data-ttu-id="177c1-128">I det här exemplet ”**azos**” (Azure online Store) är hello arbetsbelastning namn och ”**använder**” (östra USA 2) är hello plats</span><span class="sxs-lookup"><span data-stu-id="177c1-128">For this example, "**azos**" (Azure On-line Store) is hello IT workload name and "**use**" (East US 2) is hello location</span></span>
* <span data-ttu-id="177c1-129">Virtuella nätverk använder AZOS-Använd-VN**[antal]**</span><span class="sxs-lookup"><span data-stu-id="177c1-129">Virtual networks use AZOS-USE-VN**[number]**</span></span>
* <span data-ttu-id="177c1-130">Tillgänglighetsuppsättningar använder azos-Använd-som-**[roll]**</span><span class="sxs-lookup"><span data-stu-id="177c1-130">Availability sets use azos-use-as-**[role]**</span></span>
* <span data-ttu-id="177c1-131">Namn på virtuella datorer använda azos-Använd-vm -**[vmname]**</span><span class="sxs-lookup"><span data-stu-id="177c1-131">Virtual machine names use azos-use-vm-**[vmname]**</span></span>

## <a name="azure-subscriptions-and-accounts"></a><span data-ttu-id="177c1-132">Azure-prenumerationer och konton</span><span class="sxs-lookup"><span data-stu-id="177c1-132">Azure subscriptions and accounts</span></span>
<span data-ttu-id="177c1-133">Adventure Works Cycles använder sin Enterprise-prenumeration med namnet Adventure Works Enterprise prenumeration, tooprovide fakturering för den här IT-arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="177c1-133">Adventure Works Cycles is using their Enterprise subscription, named Adventure Works Enterprise Subscription, tooprovide billing for this IT workload.</span></span>

## <a name="storage"></a><span data-ttu-id="177c1-134">Lagring</span><span class="sxs-lookup"><span data-stu-id="177c1-134">Storage</span></span>
<span data-ttu-id="177c1-135">Adventure Works Cycles fastställt att de ska använda Azure hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="177c1-135">Adventure Works Cycles determined that they should use Azure Managed Disks.</span></span> <span data-ttu-id="177c1-136">När du skapar virtuella datorer används både tillgängligt lagringsutrymme lagringsnivåer:</span><span class="sxs-lookup"><span data-stu-id="177c1-136">When creating VMs, both storage available storage tiers are used:</span></span>

* <span data-ttu-id="177c1-137">**Standardlagring** för hello webbservrar, programservrar och domänkontrollanter och deras datadiskar.</span><span class="sxs-lookup"><span data-stu-id="177c1-137">**Standard storage** for hello web servers, application servers, and domain controllers and their data disks.</span></span>
* <span data-ttu-id="177c1-138">**Premium-lagring** för hello MongoDB delat klusterservrar och deras datadiskar.</span><span class="sxs-lookup"><span data-stu-id="177c1-138">**Premium storage** for hello MongoDB sharded cluster servers and their data disks.</span></span>

## <a name="virtual-network-and-subnets"></a><span data-ttu-id="177c1-139">Virtuellt nätverk och undernät</span><span class="sxs-lookup"><span data-stu-id="177c1-139">Virtual network and subnets</span></span>
<span data-ttu-id="177c1-140">Eftersom hello virtuellt nätverk inte behöver pågående anslutning toohello Adventure arbete Cycles lokalt nätverk valt de endast molnbaserad virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="177c1-140">Because hello virtual network does not need ongoing connectivity toohello Adventure Work Cycles on-premises network, they decided on a cloud-only virtual network.</span></span>

<span data-ttu-id="177c1-141">De skapade ett virtuellt nätverk som endast molnbaserad med hello följande inställningar med hjälp av hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="177c1-141">They created a cloud-only virtual network with hello following settings using hello Azure portal:</span></span>

* <span data-ttu-id="177c1-142">Namn: AZOS-Använd-VN01</span><span class="sxs-lookup"><span data-stu-id="177c1-142">Name: AZOS-USE-VN01</span></span>
* <span data-ttu-id="177c1-143">Plats: Östra USA 2</span><span class="sxs-lookup"><span data-stu-id="177c1-143">Location: East US 2</span></span>
* <span data-ttu-id="177c1-144">Virtuellt nätverks-adressutrymme: 10.0.0.0/8</span><span class="sxs-lookup"><span data-stu-id="177c1-144">Virtual network address space: 10.0.0.0/8</span></span>
* <span data-ttu-id="177c1-145">Första undernätet:</span><span class="sxs-lookup"><span data-stu-id="177c1-145">First subnet:</span></span>
  * <span data-ttu-id="177c1-146">Namn: klientdel</span><span class="sxs-lookup"><span data-stu-id="177c1-146">Name: FrontEnd</span></span>
  * <span data-ttu-id="177c1-147">Adressutrymmet: 10.0.1.0/24</span><span class="sxs-lookup"><span data-stu-id="177c1-147">Address space: 10.0.1.0/24</span></span>
* <span data-ttu-id="177c1-148">Andra undernät:</span><span class="sxs-lookup"><span data-stu-id="177c1-148">Second subnet:</span></span>
  * <span data-ttu-id="177c1-149">Namn: BackEnd</span><span class="sxs-lookup"><span data-stu-id="177c1-149">Name: BackEnd</span></span>
  * <span data-ttu-id="177c1-150">Adressutrymmet: 10.0.2.0/24</span><span class="sxs-lookup"><span data-stu-id="177c1-150">Address space: 10.0.2.0/24</span></span>

## <a name="availability-sets"></a><span data-ttu-id="177c1-151">Tillgänglighetsuppsättningar</span><span class="sxs-lookup"><span data-stu-id="177c1-151">Availability sets</span></span>
<span data-ttu-id="177c1-152">toomaintain hög tillgänglighet för alla fyra nivåer av deras onlinebutik Adventure Works Cycles bestämt på fyra tillgänglighetsuppsättningar:</span><span class="sxs-lookup"><span data-stu-id="177c1-152">toomaintain high availability of all four tiers of their on-line store, Adventure Works Cycles decided on four availability sets:</span></span>

* <span data-ttu-id="177c1-153">**azos används som web** för hello webbservrar</span><span class="sxs-lookup"><span data-stu-id="177c1-153">**azos-use-as-web** for hello web servers</span></span>
* <span data-ttu-id="177c1-154">**azos används som appen** för hello programservrar</span><span class="sxs-lookup"><span data-stu-id="177c1-154">**azos-use-as-app** for hello application servers</span></span>
* <span data-ttu-id="177c1-155">**azos används som db** för hello servrar i hello MongoDB delat kluster</span><span class="sxs-lookup"><span data-stu-id="177c1-155">**azos-use-as-db** for hello servers in hello MongoDB sharded cluster</span></span>
* <span data-ttu-id="177c1-156">**azos används som dc** hello-domänkontrollanter</span><span class="sxs-lookup"><span data-stu-id="177c1-156">**azos-use-as-dc** for hello domain controllers</span></span>

## <a name="virtual-machines"></a><span data-ttu-id="177c1-157">Virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="177c1-157">Virtual machines</span></span>
<span data-ttu-id="177c1-158">Adventure Works Cycles valt hello följande namn för sina virtuella Azure-datorer:</span><span class="sxs-lookup"><span data-stu-id="177c1-158">Adventure Works Cycles decided on hello following names for their Azure VMs:</span></span>

* <span data-ttu-id="177c1-159">**azos-användning-vm-web01** för hello första webbserver</span><span class="sxs-lookup"><span data-stu-id="177c1-159">**azos-use-vm-web01** for hello first web server</span></span>
* <span data-ttu-id="177c1-160">**azos-användning-vm-web02** för hello andra webbservern</span><span class="sxs-lookup"><span data-stu-id="177c1-160">**azos-use-vm-web02** for hello second web server</span></span>
* <span data-ttu-id="177c1-161">**azos-användning-vm-app01** för hello första programserver</span><span class="sxs-lookup"><span data-stu-id="177c1-161">**azos-use-vm-app01** for hello first application server</span></span>
* <span data-ttu-id="177c1-162">**azos-användning-vm-app02** för hello andra programserver</span><span class="sxs-lookup"><span data-stu-id="177c1-162">**azos-use-vm-app02** for hello second application server</span></span>
* <span data-ttu-id="177c1-163">**azos-användning-vm-db01** för hello första MongoDB-servern i hello kluster</span><span class="sxs-lookup"><span data-stu-id="177c1-163">**azos-use-vm-db01** for hello first MongoDB server in hello cluster</span></span>
* <span data-ttu-id="177c1-164">**azos-användning-vm-db02** för hello andra MongoDB-servern i hello kluster</span><span class="sxs-lookup"><span data-stu-id="177c1-164">**azos-use-vm-db02** for hello second MongoDB server in hello cluster</span></span>
* <span data-ttu-id="177c1-165">**azos-användning-vm-dc01** för hello första domänkontrollant</span><span class="sxs-lookup"><span data-stu-id="177c1-165">**azos-use-vm-dc01** for hello first domain controller</span></span>
* <span data-ttu-id="177c1-166">**azos-användning-vm-dc02** för hello andra domänkontrollanten</span><span class="sxs-lookup"><span data-stu-id="177c1-166">**azos-use-vm-dc02** for hello second domain controller</span></span>

<span data-ttu-id="177c1-167">Här är hello resulterande konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="177c1-167">Here is hello resulting configuration.</span></span>

![Sista infrastruktur för distribuerade i Azure](./media/infrastructure-example/example-config.png)

<span data-ttu-id="177c1-169">Den här konfigurationen omfattar:</span><span class="sxs-lookup"><span data-stu-id="177c1-169">This configuration incorporates:</span></span>

* <span data-ttu-id="177c1-170">Endast molnbaserad virtuellt nätverk med två undernät (klient- och Servergränssnitten)</span><span class="sxs-lookup"><span data-stu-id="177c1-170">A cloud-only virtual network with two subnets (FrontEnd and BackEnd)</span></span>
* <span data-ttu-id="177c1-171">Azure hanterade diskar med hjälp av Standard- och Premium-diskar</span><span class="sxs-lookup"><span data-stu-id="177c1-171">Azure Managed Disks using both Standard and Premium disks</span></span>
* <span data-ttu-id="177c1-172">Fyra tillgänglighetsuppsättningar, ett för varje nivå av hello onlinebutik</span><span class="sxs-lookup"><span data-stu-id="177c1-172">Four availability sets, one for each tier of hello on-line store</span></span>
* <span data-ttu-id="177c1-173">hello virtuella datorer för hello fyra nivåer</span><span class="sxs-lookup"><span data-stu-id="177c1-173">hello virtual machines for hello four tiers</span></span>
* <span data-ttu-id="177c1-174">En extern belastningsutjämnad uppsättning för HTTPS-baserade webbtrafik från hello Internet toohello webbservrar</span><span class="sxs-lookup"><span data-stu-id="177c1-174">An external load balanced set for HTTPS-based web traffic from hello Internet toohello web servers</span></span>
* <span data-ttu-id="177c1-175">Ett internt belastningsutjämnade uppsättningen okrypterad webbtrafik från hello servrar toohello webbprogramservrar</span><span class="sxs-lookup"><span data-stu-id="177c1-175">An internal load balanced set for unencrypted web traffic from hello web servers toohello application servers</span></span>
* <span data-ttu-id="177c1-176">En enskild resursgrupp</span><span class="sxs-lookup"><span data-stu-id="177c1-176">A single resource group</span></span>
