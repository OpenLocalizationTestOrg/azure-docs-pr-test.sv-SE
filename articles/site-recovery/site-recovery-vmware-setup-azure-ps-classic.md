---
title: " Hantera en Server för Process som körs i Azure(Classic) | Microsoft Docs"
description: "Den här artikeln beskriver hur tooset in återställning processen Server(Classic) i Azure."
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
ms.openlocfilehash: eadcc0236c77c9ebbbc885c4a7ee81098f1f4e72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-process-server-running-in-azure-classic"></a><span data-ttu-id="b46d1-103">Hantera en Server för Process som körs i Azure (klassisk)</span><span class="sxs-lookup"><span data-stu-id="b46d1-103">Manage a Process Server running in Azure (Classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b46d1-104">Klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b46d1-104">Azure Classic </span></span>](./site-recovery-vmware-setup-azure-ps-classic.md)
> * [<span data-ttu-id="b46d1-105">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b46d1-105">Resource Manager</span></span>](./site-recovery-vmware-setup-azure-ps-resource-manager.md)

<span data-ttu-id="b46d1-106">Bör toodeploy Processerver i Azure under återställning efter fel, om det är fördröjningar mellan hello Azure Virtual Network och ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="b46d1-106">During failback, it is recommended toodeploy Process Server in Azure if there is high latency between hello Azure Virtual Network and your on-premises network.</span></span> <span data-ttu-id="b46d1-107">Den här artikeln beskriver hur du kan skapa, konfigurera och hantera hello servrar körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="b46d1-107">This article describes how you can set up, configure, and manage hello process servers running in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="b46d1-108">Den här artikeln är toobe används om du använde klassisk som hello distributionsmodell för hello virtuella datorer under växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="b46d1-108">This article is toobe used if you used Classic as hello deployment model for hello virtual machines during failover.</span></span> <span data-ttu-id="b46d1-109">Om du använde Resource Manager som hello modellen Följ hello distributionsstegen i [hur tooset upp och konfigurera en Processerver (Resource Manager)](./site-recovery-vmware-setup-azure-ps-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="b46d1-109">If you used Resource Manager as hello deployment model follow hello steps in [How tooset up & configure a Failback Process Server (Resource Manager)](./site-recovery-vmware-setup-azure-ps-resource-manager.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b46d1-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b46d1-110">Prerequisites</span></span>

[!INCLUDE [site-recovery-vmware-process-server-prereq](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-on-azure"></a><span data-ttu-id="b46d1-111">Distribuera en Processerver i Azure</span><span class="sxs-lookup"><span data-stu-id="b46d1-111">Deploy a Process Server on Azure</span></span>

1. <span data-ttu-id="b46d1-112">Skapa en virtuell dator med hello i Azure Marketplace **Microsoft Azure Site Recovery Process Server V2**</span><span class="sxs-lookup"><span data-stu-id="b46d1-112">In Azure Marketplace, create a virtual machine using hello **Microsoft Azure Site Recovery Process Server V2**</span></span> </br>
    <span data-ttu-id="b46d1-113">![Marketplace_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image.png)</span><span class="sxs-lookup"><span data-stu-id="b46d1-113">![Marketplace_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image.png)</span></span>
2. <span data-ttu-id="b46d1-114">Se till att du väljer hello distributionsmodell som **klassiska**</span><span class="sxs-lookup"><span data-stu-id="b46d1-114">Ensure that you select hello deployment model as **Classic**</span></span> </br><span data-ttu-id="b46d1-115">
  ![Marketplace_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image-classic.png)</span><span class="sxs-lookup"><span data-stu-id="b46d1-115">
![Marketplace_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image-classic.png)</span></span>
3. <span data-ttu-id="b46d1-116">I guiden för hello Skapa virtuell dator > grundläggande inställningar, se till att du väljer hello prenumerationen och platsen toowhere du redundansväxlade hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="b46d1-116">In hello Create virtual machine wizard > Basic Settings, ensure you select hello Subscription and Location toowhere you failed over hello virtual machines.</span></span></br><span data-ttu-id="b46d1-117">
  ![create_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-basic-info.png)</span><span class="sxs-lookup"><span data-stu-id="b46d1-117">
![create_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-basic-info.png)</span></span>
4. <span data-ttu-id="b46d1-118">Se till att hello den virtuella datorn är ansluten toohello Azure Virtual Network toowhich hello redundansväxlade virtuella datorn är ansluten.</span><span class="sxs-lookup"><span data-stu-id="b46d1-118">Ensure that hello virtual machine is connected toohello Azure Virtual Network toowhich hello failed over virtual machine is connected.</span></span></br><span data-ttu-id="b46d1-119">
  ![create_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-settings.png)</span><span class="sxs-lookup"><span data-stu-id="b46d1-119">
![create_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-settings.png)</span></span>
5. <span data-ttu-id="b46d1-120">När hello Processervern virtuella datorn har etablerats måste toolog i och registrera den med hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="b46d1-120">Once hello Process Server virtual machine is provisioned, you need toolog in and register it with hello Configuration Server.</span></span>

> [!NOTE]
> <span data-ttu-id="b46d1-121">toobe kan toouse denna Process-Server för återställning efter fel, behöver du tooregister med hello lokalt konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="b46d1-121">toobe able toouse this Process Server for failback, you need tooregister it with hello on-premises configuration server.</span></span>

## <a name="registering-hello-process-server-running-in-azure-tooa-configuration-server-running-on-premises"></a><span data-ttu-id="b46d1-122">Registrera hello (som körs i Azure) Processervern tooa konfigurationsservern (som körs lokalt)</span><span class="sxs-lookup"><span data-stu-id="b46d1-122">Registering hello Process Server (running in Azure) tooa Configuration Server (running on-premises)</span></span>

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

## <a name="upgrading-hello-process-server-toolatest-version"></a><span data-ttu-id="b46d1-123">Uppgradera hello processen toolatest serverversionen.</span><span class="sxs-lookup"><span data-stu-id="b46d1-123">Upgrading hello Process Server toolatest version.</span></span>

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-upgrade-process-server.md)]

## <a name="unregistering-hello-process-server-running-in-azure-from-a-configuration-server-running-on-premises"></a><span data-ttu-id="b46d1-124">Avregistrerar hello Processervern (som körs i Azure) från en Server för konfiguration (som körs lokalt)</span><span class="sxs-lookup"><span data-stu-id="b46d1-124">Unregistering hello Process Server (running in Azure) from a Configuration Server (running on-premises)</span></span>

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]
