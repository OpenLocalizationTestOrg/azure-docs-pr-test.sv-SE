---
title: "Konfigurera käll- och för fysisk serverreplikering till Azure med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hur du gör inställningar för källa och mål för replikering av fysiska servrar till Azure-lagring med Azure Site Recovery-tjänsten"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 2e3d03bc-4e53-4590-8a01-f702d3cc9500
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: e89bbf5a2c1d71852e49da43d3106a05ebfc28a8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="step-7-set-up-the-source-and-target-for-physical-server-replication-to-azure"></a><span data-ttu-id="91ba6-103">Steg 7: Konfigurera käll- och för fysisk serverreplikering till Azure</span><span class="sxs-lookup"><span data-stu-id="91ba6-103">Step 7: Set up the source and target for physical server replication to Azure</span></span>

<span data-ttu-id="91ba6-104">Den här artikeln beskriver hur du konfigurerar inställningar för källan och målet vid replikering av lokala fysiska servrar till Azure med hjälp av den [Azure Site Recovery](site-recovery-overview.md) tjänsten i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="91ba6-104">This article describes how to configure source and target settings when replicating on-premises physical servers to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

<span data-ttu-id="91ba6-105">Publicera kommentarer och frågor längst ned i den här artikeln eller i den [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="91ba6-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-the-source-environment"></a><span data-ttu-id="91ba6-106">Konfigurera källmiljön</span><span class="sxs-lookup"><span data-stu-id="91ba6-106">Set up the source environment</span></span>

<span data-ttu-id="91ba6-107">Ställ in konfigurationsservern, registrera i valvet och identifiera datorer.</span><span class="sxs-lookup"><span data-stu-id="91ba6-107">Set up the configuration server, register it in the vault, and discover machines.</span></span>

1. <span data-ttu-id="91ba6-108">Klicka på **Site Recovery** > **steg 1: förbereda infrastrukturen** > **källa**.</span><span class="sxs-lookup"><span data-stu-id="91ba6-108">Click **Site Recovery** > **Step 1: Prepare Infrastructure** > **Source**.</span></span>
2. <span data-ttu-id="91ba6-109">Om du inte har en konfigurationsserver, klickar du på **+ konfigurationsservern**.</span><span class="sxs-lookup"><span data-stu-id="91ba6-109">If you don’t have a configuration server, click **+Configuration server**.</span></span>
3. <span data-ttu-id="91ba6-110">I **Lägg till Server**, kontrollera att **konfigurationsservern** visas i **servertyp**.</span><span class="sxs-lookup"><span data-stu-id="91ba6-110">In **Add Server**, check that **Configuration Server** appears in **Server type**.</span></span>
4. <span data-ttu-id="91ba6-111">Hämta installationsfilen för enhetlig installationsprogram för Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="91ba6-111">Download the Site Recovery Unified Setup installation file.</span></span>
5. <span data-ttu-id="91ba6-112">Ladda ned valvregistreringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="91ba6-112">Download the vault registration key.</span></span> <span data-ttu-id="91ba6-113">Du behöver detta när du kör installationsprogrammet för enhetlig.</span><span class="sxs-lookup"><span data-stu-id="91ba6-113">You need this when you run Unified Setup.</span></span> <span data-ttu-id="91ba6-114">Nyckeln är giltig i fem dagar efter att du har genererat den.</span><span class="sxs-lookup"><span data-stu-id="91ba6-114">The key is valid for five days after you generate it.</span></span>

   ![Konfigurera källan](./media/vmware-walkthrough-source-target/set-source2.png)


## <a name="register-the-configuration-server-in-the-vault"></a><span data-ttu-id="91ba6-116">Registrera konfigurationsservern i valvet</span><span class="sxs-lookup"><span data-stu-id="91ba6-116">Register the configuration server in the vault</span></span>

<span data-ttu-id="91ba6-117">Gör följande innan du startar och sedan installera enhetlig för att installera konfigurationsservern, processervern och huvudmålservern.</span><span class="sxs-lookup"><span data-stu-id="91ba6-117">Do the following before you start, then run Unified Setup to install the configuration server, the process server, and the master target server.</span></span> <span data-ttu-id="91ba6-118">Videon beskriver hur du konfigurerar komponenterna för VMware VM till Azure-replikering.</span><span class="sxs-lookup"><span data-stu-id="91ba6-118">The video describes setting up the components for VMware VM to Azure replication.</span></span> <span data-ttu-id="91ba6-119">Dock gäller samma process för fysisk server till Azure-replikering.</span><span class="sxs-lookup"><span data-stu-id="91ba6-119">However, the same process is valid for physical server to Azure replication.</span></span>

- <span data-ttu-id="91ba6-120">Kontrollera att systemklockan är synkroniserad med i konfigurationsservern VM och en [tidsserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service).</span><span class="sxs-lookup"><span data-stu-id="91ba6-120">On the configuration server VM, make sure that the system clock is synchronized with a [Time Server](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service).</span></span> <span data-ttu-id="91ba6-121">Den måste matcha.</span><span class="sxs-lookup"><span data-stu-id="91ba6-121">It should match.</span></span> <span data-ttu-id="91ba6-122">Installationen misslyckas om det är 15 minuter framför eller bakom.</span><span class="sxs-lookup"><span data-stu-id="91ba6-122">If it's 15 minutes in front or behind, setup might fail.</span></span>
- <span data-ttu-id="91ba6-123">Kör installationsprogrammet som en lokal administratör på configuration server-datorn.</span><span class="sxs-lookup"><span data-stu-id="91ba6-123">Run setup as a Local Administrator on the configuration server machine.</span></span>
- <span data-ttu-id="91ba6-124">Kontrollera att TLS 1.0 är aktiverat på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="91ba6-124">Make sure TLS 1.0 is enabled on the VM.</span></span>


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> <span data-ttu-id="91ba6-125">Konfigurationsservern kan också installeras [från kommandoraden](http://aka.ms/installconfigsrv).</span><span class="sxs-lookup"><span data-stu-id="91ba6-125">The configuration server can also be installed [from the command line](http://aka.ms/installconfigsrv).</span></span>




## <a name="set-up-the-target-environment"></a><span data-ttu-id="91ba6-126">Konfigurera målmiljön</span><span class="sxs-lookup"><span data-stu-id="91ba6-126">Set up the target environment</span></span>

<span data-ttu-id="91ba6-127">Innan du konfigurerar målmiljön, kontrollera att du har ett Azure storage-konto och virtuella nätverk som har upprättats.</span><span class="sxs-lookup"><span data-stu-id="91ba6-127">Before you set up the target environment, make sure you have an Azure storage account and virtual network set up.</span></span>

1. <span data-ttu-id="91ba6-128">Klicka på **Förbered infrastruktur** > **Mål** och välj den Azure-prenumeration som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="91ba6-128">Click **Prepare infrastructure** > **Target**, and select the Azure subscription you want to use.</span></span>
2. <span data-ttu-id="91ba6-129">Ange om ditt mål distributionsmodell är Resource Manager-baserade eller klassiska.</span><span class="sxs-lookup"><span data-stu-id="91ba6-129">Specify whether your target deployment model is Resource Manager-based, or classic.</span></span>
3. <span data-ttu-id="91ba6-130">Site Recovery kontrollerar att du har ett eller flera kompatibla Azure-lagringskonton och Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="91ba6-130">Site Recovery checks that you have one or more compatible Azure storage accounts and networks.</span></span>

   ![mål](./media/physical-walkthrough-source-target/gs-target.png)

4. <span data-ttu-id="91ba6-132">Om du inte har skapat ett lagringskonto eller nätverket, klickar du på **+ lagringskonto** eller **+ nätverk**, för att skapa ett konto för hanteraren för filserverresurser eller nätverk infogad.</span><span class="sxs-lookup"><span data-stu-id="91ba6-132">If you haven't created a storage account or network, click **+Storage account** or **+Network**, to create a Resource Manager account or network inline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="91ba6-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="91ba6-133">Next steps</span></span>

<span data-ttu-id="91ba6-134">Gå till [steg 8: skapa en replikeringsprincip](physical-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="91ba6-134">Go to [Step 8: Set up a replication policy](physical-walkthrough-replication.md)</span></span>
