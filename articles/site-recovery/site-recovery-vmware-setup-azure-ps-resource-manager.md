---
title: " Hantera en processerver som körs i Azure (Resource Manager) | Microsoft Docs"
description: "Den här artikeln beskriver hur du ställer in en processerver (Resource Manager) i Azure."
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
ms.openlocfilehash: 2b9b31abd5d11d02935a74e47d26be9803cdc920
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-a-process-server-running-in-azure-resource-manager"></a><span data-ttu-id="4d653-103">Hantera en processerver som körs i Azure (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="4d653-103">Manage a process server running in Azure (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4d653-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="4d653-104">Resource Manager</span></span>](./site-recovery-vmware-setup-azure-ps-resource-manager.md)
> * [<span data-ttu-id="4d653-105">Klassisk</span><span class="sxs-lookup"><span data-stu-id="4d653-105">Classic </span></span>](./site-recovery-vmware-setup-azure-ps-classic.md)

<span data-ttu-id="4d653-106">Under återställning efter fel rekommenderas att distribuera processerver i Azure om det är fördröjningar mellan det virtuella Azure-nätverket och ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="4d653-106">During failback, it is recommended to deploy process server in Azure if there is high latency between the Azure Virtual Network and your on-premises network.</span></span> <span data-ttu-id="4d653-107">Den här artikeln beskriver hur du kan skapa, konfigurera och hantera processen-servrar som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="4d653-107">This article describes how you can set up, configure, and manage the process servers running in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="4d653-108">Den här artikeln som ska användas om du har använt **Resource Manager** som distributionsmodell för de virtuella datorerna under växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="4d653-108">This article is to be used if you used **Resource Manager** as the deployment model for the virtual machines during failover.</span></span> <span data-ttu-id="4d653-109">Om du använde **klassiska** som distributionsmodell, följer du stegen i [hur du ställer in och konfigurera en processerver (klassisk)](./site-recovery-vmware-setup-azure-ps-classic.md)</span><span class="sxs-lookup"><span data-stu-id="4d653-109">If you used **Classic** as the deployment model, follow the steps in [How to set up & configure a Failback process server (Classic)](./site-recovery-vmware-setup-azure-ps-classic.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4d653-110">Krav</span><span class="sxs-lookup"><span data-stu-id="4d653-110">Prerequisites</span></span>

[!INCLUDE [site-recovery-vmware-process-server-prerequ](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-on-azure"></a><span data-ttu-id="4d653-111">Distribuera en processerver i Azure</span><span class="sxs-lookup"><span data-stu-id="4d653-111">Deploy a process server on Azure</span></span>
1. <span data-ttu-id="4d653-112">I valvet > **Site Recovery-infrastruktur** (under rubriken ”hantera”) > **Konfigurationsservrar** (under ”för VMware och fysiska datorer” rubrik), väljer du konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="4d653-112">In the Vault > **Site Recovery Infrastructure** (under the "Manage" heading) > **Configuration Servers** (under "For VMware and Physical Machines" heading), select the configuration server.</span></span>
2. <span data-ttu-id="4d653-113">På sidan konfigurationsservern information som öppnas klickar du på ”+ bearbeta server”</span><span class="sxs-lookup"><span data-stu-id="4d653-113">In the Configuration Server details page that opens click "+ Process server"</span></span>

  ![Lägg till Galleri för process-server](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps.png)

3.  <span data-ttu-id="4d653-115">På den **Lägg till processerver** väljer du följande värden</span><span class="sxs-lookup"><span data-stu-id="4d653-115">On the **Add process server** page, select the following values</span></span>

  ![Lägg till galleriobjektet för processervern](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps-page-1.png)
|<span data-ttu-id="4d653-117">**Fältnamn**</span><span class="sxs-lookup"><span data-stu-id="4d653-117">**Field Name**</span></span>|<span data-ttu-id="4d653-118">**Värde**</span><span class="sxs-lookup"><span data-stu-id="4d653-118">**Value**</span></span>|
|-|-|
|<span data-ttu-id="4d653-119">Välj var processervern ska distribueras</span><span class="sxs-lookup"><span data-stu-id="4d653-119">Choose where you want to deploy your process server</span></span>|<span data-ttu-id="4d653-120">Välj värdet **distribuera en processerver i Azure**</span><span class="sxs-lookup"><span data-stu-id="4d653-120">Select the value **Deploy a failback process server in Azure**</span></span> |
|<span data-ttu-id="4d653-121">Prenumeration</span><span class="sxs-lookup"><span data-stu-id="4d653-121">Subscription</span></span>|<span data-ttu-id="4d653-122">Välj den Azure-prenumeration där du redundansväxlade virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="4d653-122">Select the Azure Subscription where you failed over the virtual machines</span></span>|
|<span data-ttu-id="4d653-123">Resursgrupp</span><span class="sxs-lookup"><span data-stu-id="4d653-123">Resource Group</span></span>|<span data-ttu-id="4d653-124">Du kan skapa en resursgrupp för att distribuera den här processen servern eller välja att distribuera processervern i en befintlig resursgrupp</span><span class="sxs-lookup"><span data-stu-id="4d653-124">You can create a Resource Group to deploy this process server or choose to deploy the process server in an existing Resource Group</span></span>|
|<span data-ttu-id="4d653-125">Plats</span><span class="sxs-lookup"><span data-stu-id="4d653-125">Location</span></span>|<span data-ttu-id="4d653-126">Välj Azure-datacenter där de virtuella datorerna där redundansväxlats till</span><span class="sxs-lookup"><span data-stu-id="4d653-126">Select the Azure Data Center into which the virtual machines where failed over into</span></span>|
|<span data-ttu-id="4d653-127">Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="4d653-127">Azure Network</span></span>|<span data-ttu-id="4d653-128">Välj den virtuella Azure-Network(VNet) som de virtuella datorerna där redundansväxlats till.</span><span class="sxs-lookup"><span data-stu-id="4d653-128">Select the Azure Virtual Network(VNet) that the virtual machines where failed over into.</span></span> <span data-ttu-id="4d653-129">Om du redundansväxlade virtuella datorer i flera virtuella Azure-nätverk måste en processerver distribueras per virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="4d653-129">If you failed over virtual machines into multiple Azure VNets, then you need a process server deployed per VNet</span></span>|

4. <span data-ttu-id="4d653-130">Fyll i resten av egenskaperna för processervern</span><span class="sxs-lookup"><span data-stu-id="4d653-130">Fill in the rest of the properties for the process server</span></span>

  ![Lägg till processerver sammanfattning](./media/site-recovery-vmware-setup-azure-ps-arm/add-ps-page-2.png)
|<span data-ttu-id="4d653-132">**Fältnamn**</span><span class="sxs-lookup"><span data-stu-id="4d653-132">**Field Name**</span></span>|<span data-ttu-id="4d653-133">**Värde**</span><span class="sxs-lookup"><span data-stu-id="4d653-133">**Value**</span></span>|
|-|-|
|<span data-ttu-id="4d653-134">Servernamn</span><span class="sxs-lookup"><span data-stu-id="4d653-134">Server Name</span></span>|<span data-ttu-id="4d653-135">Visningsnamnet & värdnamnet för den virtuella datorn på processen</span><span class="sxs-lookup"><span data-stu-id="4d653-135">Display name & Host name for your process server virtual machine</span></span>|
| <span data-ttu-id="4d653-136">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="4d653-136">User Name</span></span>|<span data-ttu-id="4d653-137">Ett användarnamn som är administratör på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="4d653-137">A user name that becomes an Administrator on that virtual machine</span></span>|
|<span data-ttu-id="4d653-138">Storage-konto</span><span class="sxs-lookup"><span data-stu-id="4d653-138">Storage Account</span></span>|<span data-ttu-id="4d653-139">Namnet på det Lagringskonto där den virtuella datorns virtuella disken placeras</span><span class="sxs-lookup"><span data-stu-id="4d653-139">Name of the Storage Account where the virtual machine's virtual disk's are placed</span></span>|
|<span data-ttu-id="4d653-140">Undernät</span><span class="sxs-lookup"><span data-stu-id="4d653-140">Subnet</span></span>|<span data-ttu-id="4d653-141">Undernätet i Azure-VNet som den virtuella datorn är ansluten</span><span class="sxs-lookup"><span data-stu-id="4d653-141">The subnet of the Azure VNet to which the virtual machine is connected</span></span>|
| <span data-ttu-id="4d653-142">IP-adress</span><span class="sxs-lookup"><span data-stu-id="4d653-142">IP Address</span></span>|<span data-ttu-id="4d653-143">IP-adress som du vill att processervern kan ta över en gång det startar</span><span class="sxs-lookup"><span data-stu-id="4d653-143">IP Address that you would like the process server to assume once it boots up</span></span>|
5. <span data-ttu-id="4d653-144">Klicka på OK för att börja distribuera den virtuella datorn på processen.</span><span class="sxs-lookup"><span data-stu-id="4d653-144">Click the OK button to start deploying the process server virtual machine.</span></span>

> [!NOTE]
> <span data-ttu-id="4d653-145">För att kunna använda den här processerver för återställning efter fel, måste du registrera den med konfigurationsservern lokalt.</span><span class="sxs-lookup"><span data-stu-id="4d653-145">To be able to use this process server for failback, you need to register it with the on-premises configuration server.</span></span>

## <a name="registering-the-process-server-running-in-azure-to-a-configuration-server-running-on-premises"></a><span data-ttu-id="4d653-146">Registrera processervern (som körs i Azure) till en Server för konfiguration (som körs lokalt)</span><span class="sxs-lookup"><span data-stu-id="4d653-146">Registering the process server (running in Azure) to a Configuration Server (running on-premises)</span></span>

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

## <a name="upgrading-the-process-server-to-latest-version"></a><span data-ttu-id="4d653-147">Uppgradera processervern till senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="4d653-147">Upgrading the process server to latest version.</span></span>

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-upgrade-process-server.md)]

## <a name="unregistering-the-process-server-running-in-azure-from-a-configuration-server-running-on-premises"></a><span data-ttu-id="4d653-148">Avregistrera processervern (som körs i Azure) från en Server för konfiguration (som körs lokalt)</span><span class="sxs-lookup"><span data-stu-id="4d653-148">Unregistering the process server (running in Azure) from a Configuration Server (running on-premises)</span></span>

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]
