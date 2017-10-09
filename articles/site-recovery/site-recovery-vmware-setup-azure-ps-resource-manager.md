---
title: " Hantera en processerver som körs i Azure (Resource Manager) | Microsoft Docs"
description: "Den här artikeln beskriver hur tooset upp en återställning efter fel bearbeta server (Resource Manager) i Azure."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: 70426b96095cc42befff6c4114fb56536284a667
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-process-server-running-in-azure-resource-manager"></a><span data-ttu-id="dd08a-103">Hantera en processerver som körs i Azure (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="dd08a-103">Manage a process server running in Azure (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="dd08a-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="dd08a-104">Resource Manager</span></span>](./site-recovery-vmware-setup-azure-ps-resource-manager.md)
> * [<span data-ttu-id="dd08a-105">Klassisk</span><span class="sxs-lookup"><span data-stu-id="dd08a-105">Classic </span></span>](./site-recovery-vmware-setup-azure-ps-classic.md)

<span data-ttu-id="dd08a-106">Bör toodeploy processerver i Azure under återställning efter fel, om det är fördröjningar mellan hello Azure Virtual Network och ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="dd08a-106">During failback, it is recommended toodeploy process server in Azure if there is high latency between hello Azure Virtual Network and your on-premises network.</span></span> <span data-ttu-id="dd08a-107">Den här artikeln beskriver hur du kan skapa, konfigurera och hantera hello servrar körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="dd08a-107">This article describes how you can set up, configure, and manage hello process servers running in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="dd08a-108">Den här artikeln är toobe används om du har använt **Resource Manager** som hello distributionsmodell för hello virtuella datorer under växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="dd08a-108">This article is toobe used if you used **Resource Manager** as hello deployment model for hello virtual machines during failover.</span></span> <span data-ttu-id="dd08a-109">Om du använde **klassiska** som hello distributionsmodell åtgärderna hello i [hur tooset upp och konfigurera en processerver (klassisk)](./site-recovery-vmware-setup-azure-ps-classic.md)</span><span class="sxs-lookup"><span data-stu-id="dd08a-109">If you used **Classic** as hello deployment model, follow hello steps in [How tooset up & configure a Failback process server (Classic)](./site-recovery-vmware-setup-azure-ps-classic.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dd08a-110">Krav</span><span class="sxs-lookup"><span data-stu-id="dd08a-110">Prerequisites</span></span>

[!INCLUDE [site-recovery-vmware-process-server-prerequ](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-on-azure"></a><span data-ttu-id="dd08a-111">Distribuera en processerver i Azure</span><span class="sxs-lookup"><span data-stu-id="dd08a-111">Deploy a process server on Azure</span></span>
1. <span data-ttu-id="dd08a-112">I hello valv > **Site Recovery-infrastruktur** (under hello ”hantera” rubrik) > **Konfigurationsservrar** (under ”för VMware och fysiska datorer” rubrik), väljer du hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="dd08a-112">In hello Vault > **Site Recovery Infrastructure** (under hello "Manage" heading) > **Configuration Servers** (under "For VMware and Physical Machines" heading), select hello configuration server.</span></span>
2. <span data-ttu-id="dd08a-113">Hello konfigurationsservern information på sidan som öppnas klickar du på ”+ bearbeta server”</span><span class="sxs-lookup"><span data-stu-id="dd08a-113">In hello Configuration Server details page that opens click "+ Process server"</span></span>

  ![Lägg till Galleri för process-server](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps.png)

3.  <span data-ttu-id="dd08a-115">På hello **Lägg till processerver** sidan Välj hello följande värden</span><span class="sxs-lookup"><span data-stu-id="dd08a-115">On hello **Add process server** page, select hello following values</span></span>

  ![Lägg till galleriobjektet för processervern](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps-page-1.png)
|<span data-ttu-id="dd08a-117">**Fältnamn**</span><span class="sxs-lookup"><span data-stu-id="dd08a-117">**Field Name**</span></span>|<span data-ttu-id="dd08a-118">**Värde**</span><span class="sxs-lookup"><span data-stu-id="dd08a-118">**Value**</span></span>|
|-|-|
|<span data-ttu-id="dd08a-119">Välj var du vill toodeploy processervern</span><span class="sxs-lookup"><span data-stu-id="dd08a-119">Choose where you want toodeploy your process server</span></span>|<span data-ttu-id="dd08a-120">Välj hello värde **distribuera en processerver i Azure**</span><span class="sxs-lookup"><span data-stu-id="dd08a-120">Select hello value **Deploy a failback process server in Azure**</span></span> |
|<span data-ttu-id="dd08a-121">Prenumeration</span><span class="sxs-lookup"><span data-stu-id="dd08a-121">Subscription</span></span>|<span data-ttu-id="dd08a-122">Välj hello Azure-prenumeration där du redundansväxlade hello virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="dd08a-122">Select hello Azure Subscription where you failed over hello virtual machines</span></span>|
|<span data-ttu-id="dd08a-123">Resursgrupp</span><span class="sxs-lookup"><span data-stu-id="dd08a-123">Resource Group</span></span>|<span data-ttu-id="dd08a-124">Du kan skapa en resursgrupp toodeploy processen servern eller välj toodeploy hello processervern i en befintlig resursgrupp</span><span class="sxs-lookup"><span data-stu-id="dd08a-124">You can create a Resource Group toodeploy this process server or choose toodeploy hello process server in an existing Resource Group</span></span>|
|<span data-ttu-id="dd08a-125">Plats</span><span class="sxs-lookup"><span data-stu-id="dd08a-125">Location</span></span>|<span data-ttu-id="dd08a-126">Välj hello Azure-Datacenter till vilka hello virtuella datorer där redundansväxlats till</span><span class="sxs-lookup"><span data-stu-id="dd08a-126">Select hello Azure Data Center into which hello virtual machines where failed over into</span></span>|
|<span data-ttu-id="dd08a-127">Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="dd08a-127">Azure Network</span></span>|<span data-ttu-id="dd08a-128">Välj hello Azure virtuella Network(VNet) som hello virtuella datorer där redundansväxlats till.</span><span class="sxs-lookup"><span data-stu-id="dd08a-128">Select hello Azure Virtual Network(VNet) that hello virtual machines where failed over into.</span></span> <span data-ttu-id="dd08a-129">Om du redundansväxlade virtuella datorer i flera virtuella Azure-nätverk måste en processerver distribueras per virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="dd08a-129">If you failed over virtual machines into multiple Azure VNets, then you need a process server deployed per VNet</span></span>|

4. <span data-ttu-id="dd08a-130">Fyll i hello resten av hello egenskaper för hello processervern</span><span class="sxs-lookup"><span data-stu-id="dd08a-130">Fill in hello rest of hello properties for hello process server</span></span>

  ![Lägg till processerver sammanfattning](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps-page-2.png)
|<span data-ttu-id="dd08a-132">**Fältnamn**</span><span class="sxs-lookup"><span data-stu-id="dd08a-132">**Field Name**</span></span>|<span data-ttu-id="dd08a-133">**Värde**</span><span class="sxs-lookup"><span data-stu-id="dd08a-133">**Value**</span></span>|
|-|-|
|<span data-ttu-id="dd08a-134">Servernamn</span><span class="sxs-lookup"><span data-stu-id="dd08a-134">Server Name</span></span>|<span data-ttu-id="dd08a-135">Visningsnamnet & värdnamnet för den virtuella datorn på processen</span><span class="sxs-lookup"><span data-stu-id="dd08a-135">Display name & Host name for your process server virtual machine</span></span>|
| <span data-ttu-id="dd08a-136">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="dd08a-136">User Name</span></span>|<span data-ttu-id="dd08a-137">Ett användarnamn som är administratör på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="dd08a-137">A user name that becomes an Administrator on that virtual machine</span></span>|
|<span data-ttu-id="dd08a-138">Lagringskonto</span><span class="sxs-lookup"><span data-stu-id="dd08a-138">Storage Account</span></span>|<span data-ttu-id="dd08a-139">Namnet på hello Storage-konto där hello virtuella datorns virtuella disken placeras</span><span class="sxs-lookup"><span data-stu-id="dd08a-139">Name of hello Storage Account where hello virtual machine's virtual disk's are placed</span></span>|
|<span data-ttu-id="dd08a-140">Undernät</span><span class="sxs-lookup"><span data-stu-id="dd08a-140">Subnet</span></span>|<span data-ttu-id="dd08a-141">hello undernät för hello Azure VNet toowhich hello virtuella datorn är ansluten</span><span class="sxs-lookup"><span data-stu-id="dd08a-141">hello subnet of hello Azure VNet toowhich hello virtual machine is connected</span></span>|
| <span data-ttu-id="dd08a-142">IP-adress</span><span class="sxs-lookup"><span data-stu-id="dd08a-142">IP Address</span></span>|<span data-ttu-id="dd08a-143">IP-adress som du vill att hello processen server tooassume när den startar</span><span class="sxs-lookup"><span data-stu-id="dd08a-143">IP Address that you would like hello process server tooassume once it boots up</span></span>|
5. <span data-ttu-id="dd08a-144">Klicka på hello OK-knapp toostart hello processen server virtuell dator distribueras.</span><span class="sxs-lookup"><span data-stu-id="dd08a-144">Click hello OK button toostart deploying hello process server virtual machine.</span></span>

> [!NOTE]
> <span data-ttu-id="dd08a-145">toobe kan toouse denna processerver för återställning efter fel, behöver du tooregister med hello lokalt konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="dd08a-145">toobe able toouse this process server for failback, you need tooregister it with hello on-premises configuration server.</span></span>

## <a name="registering-hello-process-server-running-in-azure-tooa-configuration-server-running-on-premises"></a><span data-ttu-id="dd08a-146">Registrera hello processen server (som körs i Azure) tooa konfigurationsservern (som körs lokalt)</span><span class="sxs-lookup"><span data-stu-id="dd08a-146">Registering hello process server (running in Azure) tooa Configuration Server (running on-premises)</span></span>

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

## <a name="upgrading-hello-process-server-toolatest-version"></a><span data-ttu-id="dd08a-147">Uppgradera hello toolatest processerverversionen.</span><span class="sxs-lookup"><span data-stu-id="dd08a-147">Upgrading hello process server toolatest version.</span></span>

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-upgrade-process-server.md)]

## <a name="unregistering-hello-process-server-running-in-azure-from-a-configuration-server-running-on-premises"></a><span data-ttu-id="dd08a-148">Avregistrera hello processervern (som körs i Azure) från en Server för konfiguration (som körs lokalt)</span><span class="sxs-lookup"><span data-stu-id="dd08a-148">Unregistering hello process server (running in Azure) from a Configuration Server (running on-premises)</span></span>

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]
