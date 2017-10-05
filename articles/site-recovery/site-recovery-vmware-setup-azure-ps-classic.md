---
title: " Hantera en Server för Process som körs i Azure(Classic) | Microsoft Docs"
description: "Den här artikeln beskriver hur du ställer in en återställning efter fel processen Server(Classic) i Azure."
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
ms.openlocfilehash: 479bbd207bcf715138c340f9e4d2634120bab85c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-a-process-server-running-in-azure-classic"></a><span data-ttu-id="1aaad-103">Hantera en Server för Process som körs i Azure (klassisk)</span><span class="sxs-lookup"><span data-stu-id="1aaad-103">Manage a Process Server running in Azure (Classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1aaad-104">Klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1aaad-104">Azure Classic </span></span>](./site-recovery-vmware-setup-azure-ps-classic.md)
> * [<span data-ttu-id="1aaad-105">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="1aaad-105">Resource Manager</span></span>](./site-recovery-vmware-setup-azure-ps-resource-manager.md)

<span data-ttu-id="1aaad-106">Under återställning efter fel rekommenderas att distribuera Processerver i Azure om det är fördröjningar mellan det virtuella Azure-nätverket och ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="1aaad-106">During failback, it is recommended to deploy Process Server in Azure if there is high latency between the Azure Virtual Network and your on-premises network.</span></span> <span data-ttu-id="1aaad-107">Den här artikeln beskriver hur du kan skapa, konfigurera och hantera processen-servrar som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="1aaad-107">This article describes how you can set up, configure, and manage the process servers running in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="1aaad-108">Den här artikeln som ska användas om du använde klassisk som distributionsmodell för de virtuella datorerna under växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="1aaad-108">This article is to be used if you used Classic as the deployment model for the virtual machines during failover.</span></span> <span data-ttu-id="1aaad-109">Om du använde Resource Manager som distributionsmodell följer du stegen i [hur du ställer in och konfigurera en Processerver (Resource Manager)](./site-recovery-vmware-setup-azure-ps-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="1aaad-109">If you used Resource Manager as the deployment model follow the steps in [How to set up & configure a Failback Process Server (Resource Manager)](./site-recovery-vmware-setup-azure-ps-resource-manager.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1aaad-110">Krav</span><span class="sxs-lookup"><span data-stu-id="1aaad-110">Prerequisites</span></span>

[!INCLUDE [site-recovery-vmware-process-server-prereq](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-on-azure"></a><span data-ttu-id="1aaad-111">Distribuera en Processerver i Azure</span><span class="sxs-lookup"><span data-stu-id="1aaad-111">Deploy a Process Server on Azure</span></span>

1. <span data-ttu-id="1aaad-112">I Azure Marketplace, skapar du en virtuell dator med hjälp av den **Microsoft Azure Site Recovery Process Server V2**</span><span class="sxs-lookup"><span data-stu-id="1aaad-112">In Azure Marketplace, create a virtual machine using the **Microsoft Azure Site Recovery Process Server V2**</span></span> </br>
    <span data-ttu-id="1aaad-113">![Marketplace_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image.png)</span><span class="sxs-lookup"><span data-stu-id="1aaad-113">![Marketplace_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image.png)</span></span>
2. <span data-ttu-id="1aaad-114">Se till att du väljer distributionsmodellen som **klassiska**</span><span class="sxs-lookup"><span data-stu-id="1aaad-114">Ensure that you select the deployment model as **Classic**</span></span> </br><span data-ttu-id="1aaad-115">
  ![Marketplace_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image-classic.png)</span><span class="sxs-lookup"><span data-stu-id="1aaad-115">
![Marketplace_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image-classic.png)</span></span>
3. <span data-ttu-id="1aaad-116">I guiden Skapa virtuell dator > grundläggande inställningar, se till att du väljer prenumeration och plats där du misslyckades för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1aaad-116">In the Create virtual machine wizard > Basic Settings, ensure you select the Subscription and Location to where you failed over the virtual machines.</span></span></br><span data-ttu-id="1aaad-117">
  ![create_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-basic-info.png)</span><span class="sxs-lookup"><span data-stu-id="1aaad-117">
![create_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-basic-info.png)</span></span>
4. <span data-ttu-id="1aaad-118">Kontrollera att den virtuella datorn är ansluten till det virtuella Azure-nätverket som den redundansväxlade virtuella datorn är ansluten.</span><span class="sxs-lookup"><span data-stu-id="1aaad-118">Ensure that the virtual machine is connected to the Azure Virtual Network to which the failed over virtual machine is connected.</span></span></br><span data-ttu-id="1aaad-119">
  ![create_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-settings.png)</span><span class="sxs-lookup"><span data-stu-id="1aaad-119">
![create_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-settings.png)</span></span>
5. <span data-ttu-id="1aaad-120">När den virtuella datorn på Processervern har etablerats måste du logga in och registrera den med konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="1aaad-120">Once the Process Server virtual machine is provisioned, you need to log in and register it with the Configuration Server.</span></span>

> [!NOTE]
> <span data-ttu-id="1aaad-121">För att kunna använda den här Processerver för återställning efter fel, måste du registrera den med konfigurationsservern lokalt.</span><span class="sxs-lookup"><span data-stu-id="1aaad-121">To be able to use this Process Server for failback, you need to register it with the on-premises configuration server.</span></span>

## <a name="registering-the-process-server-running-in-azure-to-a-configuration-server-running-on-premises"></a><span data-ttu-id="1aaad-122">Registrera Processervern (som körs i Azure) till en Server för konfiguration (som körs lokalt)</span><span class="sxs-lookup"><span data-stu-id="1aaad-122">Registering the Process Server (running in Azure) to a Configuration Server (running on-premises)</span></span>

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

## <a name="upgrading-the-process-server-to-latest-version"></a><span data-ttu-id="1aaad-123">Uppgradera Processervern till senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="1aaad-123">Upgrading the Process Server to latest version.</span></span>

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-upgrade-process-server.md)]

## <a name="unregistering-the-process-server-running-in-azure-from-a-configuration-server-running-on-premises"></a><span data-ttu-id="1aaad-124">Avregistrera Processervern (som körs i Azure) från en Server för konfiguration (som körs lokalt)</span><span class="sxs-lookup"><span data-stu-id="1aaad-124">Unregistering the Process Server (running in Azure) from a Configuration Server (running on-premises)</span></span>

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]
