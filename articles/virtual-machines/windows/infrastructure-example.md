---
title: "Exemplet Azure-infrastrukturen genomgången | Microsoft Docs"
description: "Läs mer om viktiga design och implementeringslösning riktlinjer för att distribuera en exempel-infrastruktur i Azure."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7032b586-e4e5-4954-952f-fdfc03fc1980
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 84cefcdb85f1a3c753027e827abde010b461cda7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="example-azure-infrastructure-walkthrough-for-windows-vms"></a><span data-ttu-id="e2541-103">Exemplet Azure-infrastrukturen genomgång för virtuella Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="e2541-103">Example Azure infrastructure walkthrough for Windows VMs</span></span>

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

<span data-ttu-id="e2541-104">Den här artikeln beskriver hur bygga ut infrastruktur för ett exempel.</span><span class="sxs-lookup"><span data-stu-id="e2541-104">This article walks through building out an example application infrastructure.</span></span> <span data-ttu-id="e2541-105">Vi i detalj utformar en infrastruktur för en enkel onlinebutik som sammanför de riktlinjer och beslut runt namngivningskonventioner, tillgänglighetsuppsättningar, virtuella nätverk och belastningsutjämnare och faktiskt distribuerar virtuella datorer (VM).</span><span class="sxs-lookup"><span data-stu-id="e2541-105">We detail designing an infrastructure for a simple on-line store that brings together all the guidelines and decisions around naming conventions, availability sets, virtual networks and load balancers, and actually deploying your virtual machines (VMs).</span></span>

## <a name="example-workload"></a><span data-ttu-id="e2541-106">Exempel arbetsbelastning</span><span class="sxs-lookup"><span data-stu-id="e2541-106">Example workload</span></span>
<span data-ttu-id="e2541-107">Adventure Works Cycles vill skapa en onlinebutik program i Azure som består av:</span><span class="sxs-lookup"><span data-stu-id="e2541-107">Adventure Works Cycles wants to build an on-line store application in Azure that consists of:</span></span>

* <span data-ttu-id="e2541-108">Två IIS-servrar som kör klienten klientdelen i en webbnivå</span><span class="sxs-lookup"><span data-stu-id="e2541-108">Two IIS servers running the client front-end in a web tier</span></span>
* <span data-ttu-id="e2541-109">Två IIS-servrar bearbetar data och order i en program-nivå</span><span class="sxs-lookup"><span data-stu-id="e2541-109">Two IIS servers processing data and orders in an application tier</span></span>
* <span data-ttu-id="e2541-110">Två Microsoft SQL Server-instanser med AlwaysOn-Tillgänglighetsgrupper (två SQL-servrar och en majoritet nod vittne) för att lagra produktdata och order i en databasnivå</span><span class="sxs-lookup"><span data-stu-id="e2541-110">Two Microsoft SQL Server instances with AlwaysOn availability groups (two SQL Servers and a majority node witness) for storing product data and orders in a database tier</span></span>
* <span data-ttu-id="e2541-111">Två Active Directory-domänkontrollanter för kundkonton och leverantörer i en nivå för autentisering</span><span class="sxs-lookup"><span data-stu-id="e2541-111">Two Active Directory domain controllers for customer accounts and suppliers in an authentication tier</span></span>
* <span data-ttu-id="e2541-112">Alla servrar finns i två undernät:</span><span class="sxs-lookup"><span data-stu-id="e2541-112">All the servers are located in two subnets:</span></span>
  * <span data-ttu-id="e2541-113">ett frontend undernät för webbservrar</span><span class="sxs-lookup"><span data-stu-id="e2541-113">a front-end subnet for the web servers</span></span> 
  * <span data-ttu-id="e2541-114">en backend-undernät för programservrar, SQL-kluster och domänkontrollanter</span><span class="sxs-lookup"><span data-stu-id="e2541-114">a back-end subnet for the application servers, SQL cluster, and domain controllers</span></span>

![Diagram över olika nivåer för infrastruktur](./media/infrastructure-example/example-tiers.png)

<span data-ttu-id="e2541-116">Inkommande säker webbtrafik måste vara belastningsutjämnad webbservrarna som kunder bläddra online-butik.</span><span class="sxs-lookup"><span data-stu-id="e2541-116">Incoming secure web traffic must be load-balanced among the web servers as customers browse the on-line store.</span></span> <span data-ttu-id="e2541-117">Ordna bearbetning trafik i form av HTTP-begäranden från webben servrar måste balanseras mellan programservrarna.</span><span class="sxs-lookup"><span data-stu-id="e2541-117">Order processing traffic in the form of HTTP requests from the web servers must be balanced among the application servers.</span></span> <span data-ttu-id="e2541-118">Dessutom måste infrastrukturen utformas för hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="e2541-118">Additionally, the infrastructure must be designed for high availability.</span></span>

<span data-ttu-id="e2541-119">Den resulterande designen innehålla:</span><span class="sxs-lookup"><span data-stu-id="e2541-119">The resulting design must incorporate:</span></span>

* <span data-ttu-id="e2541-120">Ett konto och Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e2541-120">An Azure subscription and account</span></span>
* <span data-ttu-id="e2541-121">En enskild resursgrupp</span><span class="sxs-lookup"><span data-stu-id="e2541-121">A single resource group</span></span>
* <span data-ttu-id="e2541-122">Azure Managed Disks</span><span class="sxs-lookup"><span data-stu-id="e2541-122">Azure Managed Disks</span></span>
* <span data-ttu-id="e2541-123">Ett virtuellt nätverk med två undernät</span><span class="sxs-lookup"><span data-stu-id="e2541-123">A virtual network with two subnets</span></span>
* <span data-ttu-id="e2541-124">Tillgänglighetsuppsättningar för virtuella datorer med en liknande roll</span><span class="sxs-lookup"><span data-stu-id="e2541-124">Availability sets for the VMs with a similar role</span></span>
* <span data-ttu-id="e2541-125">Virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="e2541-125">Virtual machines</span></span>

<span data-ttu-id="e2541-126">Alla ovanstående följer du dessa namngivningsregler:</span><span class="sxs-lookup"><span data-stu-id="e2541-126">All the above follow these naming conventions:</span></span>

* <span data-ttu-id="e2541-127">Adventure Works Cycles använder **[IT arbetsbelastning]-[plats]-[Azure resurs]** som ett prefix</span><span class="sxs-lookup"><span data-stu-id="e2541-127">Adventure Works Cycles uses **[IT workload]-[location]-[Azure resource]** as a prefix</span></span>
  * <span data-ttu-id="e2541-128">I det här exemplet ”**azos**” (Azure online Store) är det arbetsbelastning namnet och ”**använder**” (östra USA 2) är platsen</span><span class="sxs-lookup"><span data-stu-id="e2541-128">For this example, "**azos**" (Azure On-line Store) is the IT workload name and "**use**" (East US 2) is the location</span></span>
* <span data-ttu-id="e2541-129">Virtuella nätverk använder AZOS-Använd-VN**[antal]**</span><span class="sxs-lookup"><span data-stu-id="e2541-129">Virtual networks use AZOS-USE-VN**[number]**</span></span>
* <span data-ttu-id="e2541-130">Tillgänglighetsuppsättningar använder azos-Använd-som-**[roll]**</span><span class="sxs-lookup"><span data-stu-id="e2541-130">Availability sets use azos-use-as-**[role]**</span></span>
* <span data-ttu-id="e2541-131">Namn på virtuella datorer använda azos-Använd-vm -**[vmname]**</span><span class="sxs-lookup"><span data-stu-id="e2541-131">Virtual machine names use azos-use-vm-**[vmname]**</span></span>

## <a name="azure-subscriptions-and-accounts"></a><span data-ttu-id="e2541-132">Azure-prenumerationer och konton</span><span class="sxs-lookup"><span data-stu-id="e2541-132">Azure subscriptions and accounts</span></span>
<span data-ttu-id="e2541-133">Adventure Works Cycles använder sin Enterprise-prenumeration med namnet Adventure Works Enterprise prenumeration för att förse faktureringen för den här IT-arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="e2541-133">Adventure Works Cycles is using their Enterprise subscription, named Adventure Works Enterprise Subscription, to provide billing for this IT workload.</span></span>

## <a name="storage"></a><span data-ttu-id="e2541-134">Lagring</span><span class="sxs-lookup"><span data-stu-id="e2541-134">Storage</span></span>
<span data-ttu-id="e2541-135">Adventure Works Cycles fastställt att de ska använda Azure hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="e2541-135">Adventure Works Cycles determined that they should use Azure Managed Disks.</span></span> <span data-ttu-id="e2541-136">När du skapar virtuella datorer används både tillgängligt lagringsutrymme lagringsnivåer:</span><span class="sxs-lookup"><span data-stu-id="e2541-136">When creating VMs, both storage available storage tiers are used:</span></span>

* <span data-ttu-id="e2541-137">**Standardlagring** för webbservrar, programservrar och domänkontrollanter och deras datadiskar.</span><span class="sxs-lookup"><span data-stu-id="e2541-137">**Standard storage** for the web servers, application servers, and domain controllers and their data disks.</span></span>
* <span data-ttu-id="e2541-138">**Premium-lagring** för SQL Server-datorer och deras datadiskar.</span><span class="sxs-lookup"><span data-stu-id="e2541-138">**Premium storage** for the SQL Server VMs and their data disks.</span></span>

## <a name="virtual-network-and-subnets"></a><span data-ttu-id="e2541-139">Virtuellt nätverk och undernät</span><span class="sxs-lookup"><span data-stu-id="e2541-139">Virtual network and subnets</span></span>
<span data-ttu-id="e2541-140">Eftersom det virtuella nätverket inte behöver ha en anslutning till Adventure arbete Cycles lokalt nätverk, valt de en endast molnbaserad virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="e2541-140">Because the virtual network does not need ongoing connectivity to the Adventure Work Cycles on-premises network, they decided on a cloud-only virtual network.</span></span>

<span data-ttu-id="e2541-141">De skapade ett virtuellt nätverk som endast molnbaserad med följande inställningar med hjälp av Azure portal:</span><span class="sxs-lookup"><span data-stu-id="e2541-141">They created a cloud-only virtual network with the following settings using the Azure portal:</span></span>

* <span data-ttu-id="e2541-142">Namn: AZOS-Använd-VN01</span><span class="sxs-lookup"><span data-stu-id="e2541-142">Name: AZOS-USE-VN01</span></span>
* <span data-ttu-id="e2541-143">Plats: Östra USA 2</span><span class="sxs-lookup"><span data-stu-id="e2541-143">Location: East US 2</span></span>
* <span data-ttu-id="e2541-144">Virtuellt nätverks-adressutrymme: 10.0.0.0/8</span><span class="sxs-lookup"><span data-stu-id="e2541-144">Virtual network address space: 10.0.0.0/8</span></span>
* <span data-ttu-id="e2541-145">Första undernätet:</span><span class="sxs-lookup"><span data-stu-id="e2541-145">First subnet:</span></span>
  * <span data-ttu-id="e2541-146">Namn: klientdel</span><span class="sxs-lookup"><span data-stu-id="e2541-146">Name: FrontEnd</span></span>
  * <span data-ttu-id="e2541-147">Adressutrymmet: 10.0.1.0/24</span><span class="sxs-lookup"><span data-stu-id="e2541-147">Address space: 10.0.1.0/24</span></span>
* <span data-ttu-id="e2541-148">Andra undernät:</span><span class="sxs-lookup"><span data-stu-id="e2541-148">Second subnet:</span></span>
  * <span data-ttu-id="e2541-149">Namn: BackEnd</span><span class="sxs-lookup"><span data-stu-id="e2541-149">Name: BackEnd</span></span>
  * <span data-ttu-id="e2541-150">Adressutrymmet: 10.0.2.0/24</span><span class="sxs-lookup"><span data-stu-id="e2541-150">Address space: 10.0.2.0/24</span></span>

## <a name="availability-sets"></a><span data-ttu-id="e2541-151">Tillgänglighetsuppsättningar</span><span class="sxs-lookup"><span data-stu-id="e2541-151">Availability sets</span></span>
<span data-ttu-id="e2541-152">För att upprätthålla hög tillgänglighet för alla fyra nivåer av deras onlinebutik valt Adventure Works Cycles fyra tillgänglighetsuppsättningar:</span><span class="sxs-lookup"><span data-stu-id="e2541-152">To maintain high availability of all four tiers of their on-line store, Adventure Works Cycles decided on four availability sets:</span></span>

* <span data-ttu-id="e2541-153">**azos används som web** för webbservrar</span><span class="sxs-lookup"><span data-stu-id="e2541-153">**azos-use-as-web** for the web servers</span></span>
* <span data-ttu-id="e2541-154">**azos används som appen** för programmet</span><span class="sxs-lookup"><span data-stu-id="e2541-154">**azos-use-as-app** for the application servers</span></span>
* <span data-ttu-id="e2541-155">**azos Använd som sql** för SQL-servrar</span><span class="sxs-lookup"><span data-stu-id="e2541-155">**azos-use-as-sql** for the SQL Servers</span></span>
* <span data-ttu-id="e2541-156">**azos används som dc** för domänkontrollanter</span><span class="sxs-lookup"><span data-stu-id="e2541-156">**azos-use-as-dc** for the domain controllers</span></span>

## <a name="virtual-machines"></a><span data-ttu-id="e2541-157">Virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="e2541-157">Virtual machines</span></span>
<span data-ttu-id="e2541-158">Adventure Works Cycles valt följande namn för sina virtuella Azure-datorer:</span><span class="sxs-lookup"><span data-stu-id="e2541-158">Adventure Works Cycles decided on the following names for their Azure VMs:</span></span>

* <span data-ttu-id="e2541-159">**azos-användning-vm-web01** för den första webbservern</span><span class="sxs-lookup"><span data-stu-id="e2541-159">**azos-use-vm-web01** for the first web server</span></span>
* <span data-ttu-id="e2541-160">**azos-användning-vm-web02** för den andra webbservern</span><span class="sxs-lookup"><span data-stu-id="e2541-160">**azos-use-vm-web02** for the second web server</span></span>
* <span data-ttu-id="e2541-161">**azos-användning-vm-app01** för den första programservern</span><span class="sxs-lookup"><span data-stu-id="e2541-161">**azos-use-vm-app01** for the first application server</span></span>
* <span data-ttu-id="e2541-162">**azos-användning-vm-app02** för andra programserver</span><span class="sxs-lookup"><span data-stu-id="e2541-162">**azos-use-vm-app02** for the second application server</span></span>
* <span data-ttu-id="e2541-163">**azos-användning-vm-sql01** för den första servern i SQL Server i klustret</span><span class="sxs-lookup"><span data-stu-id="e2541-163">**azos-use-vm-sql01** for the first SQL Server server in the cluster</span></span>
* <span data-ttu-id="e2541-164">**azos-användning-vm-sql02** för den andra SQL Server-servern i klustret</span><span class="sxs-lookup"><span data-stu-id="e2541-164">**azos-use-vm-sql02** for the second SQL Server server in the cluster</span></span>
* <span data-ttu-id="e2541-165">**azos-användning-vm-dc01** för den första domänkontrollanten</span><span class="sxs-lookup"><span data-stu-id="e2541-165">**azos-use-vm-dc01** for the first domain controller</span></span>
* <span data-ttu-id="e2541-166">**azos-användning-vm-dc02** för den andra domänkontrollanten</span><span class="sxs-lookup"><span data-stu-id="e2541-166">**azos-use-vm-dc02** for the second domain controller</span></span>

<span data-ttu-id="e2541-167">Det här är den resulterande konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="e2541-167">Here is the resulting configuration.</span></span>

![Sista infrastruktur för distribuerade i Azure](./media/infrastructure-example/example-config.png)

<span data-ttu-id="e2541-169">Den här konfigurationen omfattar:</span><span class="sxs-lookup"><span data-stu-id="e2541-169">This configuration incorporates:</span></span>

* <span data-ttu-id="e2541-170">Endast molnbaserad virtuellt nätverk med två undernät (klient- och Servergränssnitten)</span><span class="sxs-lookup"><span data-stu-id="e2541-170">A cloud-only virtual network with two subnets (FrontEnd and BackEnd)</span></span>
* <span data-ttu-id="e2541-171">Azure-hanterade diskar med både Standard- och Premium-diskar</span><span class="sxs-lookup"><span data-stu-id="e2541-171">Azure Managed Disks with both Standard and Premium disks</span></span>
* <span data-ttu-id="e2541-172">Fyra tillgänglighetsuppsättningar, ett för varje nivå av online-butik</span><span class="sxs-lookup"><span data-stu-id="e2541-172">Four availability sets, one for each tier of the on-line store</span></span>
* <span data-ttu-id="e2541-173">De virtuella datorerna för de fyra nivåerna</span><span class="sxs-lookup"><span data-stu-id="e2541-173">The virtual machines for the four tiers</span></span>
* <span data-ttu-id="e2541-174">En extern belastningsutjämnad uppsättning för HTTPS-baserade webbtrafik från Internet till webbservrar</span><span class="sxs-lookup"><span data-stu-id="e2541-174">An external load balanced set for HTTPS-based web traffic from the Internet to the web servers</span></span>
* <span data-ttu-id="e2541-175">Ett internt belastningsutjämnade uppsättningen okrypterad webbtrafik från webbservrarna till programservern</span><span class="sxs-lookup"><span data-stu-id="e2541-175">An internal load balanced set for unencrypted web traffic from the web servers to the application servers</span></span>
* <span data-ttu-id="e2541-176">En enskild resursgrupp</span><span class="sxs-lookup"><span data-stu-id="e2541-176">A single resource group</span></span>