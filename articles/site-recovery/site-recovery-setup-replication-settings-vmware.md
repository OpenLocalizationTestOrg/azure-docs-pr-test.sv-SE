---
title: "aaaSet in replikeringsinställningar för Azure Site Recovery | Microsoft Docs"
description: "Beskriver hur toodeploy Site Recovery tooorchestrate replikering, redundans och återställning av virtuella Hyper-V-datorer i VMM-moln tooAzure."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: rayne-wiselman
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/05/2017
ms.author: sutalasi
ms.openlocfilehash: 618e92e42411732a2a1bb75c5e5ea8a433cd7d58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-replication-policy-for-vmware-tooazure"></a><span data-ttu-id="2d71c-103">Hantera replikeringsprincip för VMware tooAzure</span><span class="sxs-lookup"><span data-stu-id="2d71c-103">Manage replication policy for VMware tooAzure</span></span>


## <a name="create-a-replication-policy"></a><span data-ttu-id="2d71c-104">Skapa replikeringsprincip</span><span class="sxs-lookup"><span data-stu-id="2d71c-104">Create a replication policy</span></span>

1. <span data-ttu-id="2d71c-105">Välj **Hantera** > **Site Recovery-infrastruktur**.</span><span class="sxs-lookup"><span data-stu-id="2d71c-105">Select **Manage** > **Site Recovery Infrastructure**.</span></span>
2. <span data-ttu-id="2d71c-106">Välj **Replikeringsprinciper** under **For VMware and Physical machines** (För VMware-datorer och fysiska datorer).</span><span class="sxs-lookup"><span data-stu-id="2d71c-106">Select **Replication policies** under **For VMware and Physical machines**.</span></span>
3. <span data-ttu-id="2d71c-107">Välj **+Replikeringsprincip**.</span><span class="sxs-lookup"><span data-stu-id="2d71c-107">Select **+Replication policy**.</span></span>

    ![Skapa replikeringsprincip](./media/site-recovery-setup-replication-settings-vmware/createpolicy.png)

4. <span data-ttu-id="2d71c-109">Ange hello principnamn.</span><span class="sxs-lookup"><span data-stu-id="2d71c-109">Enter hello policy name.</span></span>

5. <span data-ttu-id="2d71c-110">I **Återställningspunktmål tröskelvärdet**, ange hello Återställningspunktmål gränsen.</span><span class="sxs-lookup"><span data-stu-id="2d71c-110">In **RPO threshold**, specify hello RPO limit.</span></span> <span data-ttu-id="2d71c-111">Aviseringar genereras när den kontinuerliga replikeringen överskrider den här gränsen.</span><span class="sxs-lookup"><span data-stu-id="2d71c-111">Alerts will be generated when continuous replication exceeds this limit.</span></span>
6. <span data-ttu-id="2d71c-112">I **kvarhållningstid för återställningspunkten**, ange (i timmar) hello varaktighet hello kvarhållningsperiod för varje återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="2d71c-112">In **Recovery point retention**, specify (in hours) hello duration of hello retention window for each recovery point.</span></span> <span data-ttu-id="2d71c-113">Skyddade datorer kan återställas tooany punkt inom en kvarhållningsperiod.</span><span class="sxs-lookup"><span data-stu-id="2d71c-113">Protected machines can be recovered tooany point within a retention window.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2d71c-114">In too24 timmars kvarhållning stöds för datorer replikerade toopremium lagring.</span><span class="sxs-lookup"><span data-stu-id="2d71c-114">Up too24 hours of retention is supported for machines replicated toopremium storage.</span></span> <span data-ttu-id="2d71c-115">In too72 timmars kvarhållning stöds för datorer replikerade toostandard lagring.</span><span class="sxs-lookup"><span data-stu-id="2d71c-115">Up too72 hours of retention is supported for machines replicated toostandard storage.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2d71c-116">En replikeringsprincip för redundans skapas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="2d71c-116">A replication policy for failback is automatically created.</span></span>

7. <span data-ttu-id="2d71c-117">I **Appkompatibel ögonblicksbildsfrekvens** anger du hur ofta (i minuter) återställningspunkter som innehåller programkonsekventa ögonblicksbilder ska skapas.</span><span class="sxs-lookup"><span data-stu-id="2d71c-117">In **App-consistent snapshot frequency**, specify how often (in minutes) recovery points that contain application-consistent snapshots will be created.</span></span>

8. <span data-ttu-id="2d71c-118">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2d71c-118">Click **OK**.</span></span> <span data-ttu-id="2d71c-119">hello princip ska skapas på 30 too60 sekunder.</span><span class="sxs-lookup"><span data-stu-id="2d71c-119">hello policy should be created in 30 too60 seconds.</span></span>

![Replikeringsprincipsgeneration](./media/site-recovery-setup-replication-settings-vmware/Creating-Policy.png)

## <a name="associate-a-configuration-server-with-a-replication-policy"></a><span data-ttu-id="2d71c-121">Associera en konfigurationsserver med en replikeringsprincip</span><span class="sxs-lookup"><span data-stu-id="2d71c-121">Associate a configuration server with a replication policy</span></span>
1. <span data-ttu-id="2d71c-122">Välj hello replikering princip toowhich du vill tooassociate hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="2d71c-122">Choose hello replication policy toowhich you want tooassociate hello configuration server.</span></span>
2. <span data-ttu-id="2d71c-123">Klicka på **Associera**.</span><span class="sxs-lookup"><span data-stu-id="2d71c-123">Click **Associate**.</span></span>
<span data-ttu-id="2d71c-124">![Associera konfigurationsserver](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-1.PNG)</span><span class="sxs-lookup"><span data-stu-id="2d71c-124">![Associate configuration server](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-1.PNG)</span></span>

3. <span data-ttu-id="2d71c-125">Välj hello konfigurationsservern hello listan över servrar.</span><span class="sxs-lookup"><span data-stu-id="2d71c-125">Select hello configuration server from hello list of servers.</span></span>
4. <span data-ttu-id="2d71c-126">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2d71c-126">Click **OK**.</span></span> <span data-ttu-id="2d71c-127">hello konfigurationsservern ska kopplas en tootwo minuter.</span><span class="sxs-lookup"><span data-stu-id="2d71c-127">hello configuration server should be associated in one tootwo minutes.</span></span>

![Associering av konfigurationsserver](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-2.png)

## <a name="edit-a-replication-policy"></a><span data-ttu-id="2d71c-129">Redigera en replikeringsprincip</span><span class="sxs-lookup"><span data-stu-id="2d71c-129">Edit a replication policy</span></span>
1. <span data-ttu-id="2d71c-130">Välj hello replikeringsprincip som du vill tooedit replikeringsinställningarna.</span><span class="sxs-lookup"><span data-stu-id="2d71c-130">Choose hello replication policy for which you want tooedit replication settings.</span></span>
<span data-ttu-id="2d71c-131">![Redigera replikeringsprincip](./media/site-recovery-setup-replication-settings-vmware/Select-Policy.png)</span><span class="sxs-lookup"><span data-stu-id="2d71c-131">![Edit replication policy](./media/site-recovery-setup-replication-settings-vmware/Select-Policy.png)</span></span>

2. <span data-ttu-id="2d71c-132">Klicka på **Redigera inställningar**.</span><span class="sxs-lookup"><span data-stu-id="2d71c-132">Click **Edit Settings**.</span></span>
<span data-ttu-id="2d71c-133">![Redigera inställningar för replikeringsprinciper](./media/site-recovery-setup-replication-settings-vmware/Edit-Policy.png)</span><span class="sxs-lookup"><span data-stu-id="2d71c-133">![Edit replication policy settings](./media/site-recovery-setup-replication-settings-vmware/Edit-Policy.png)</span></span>

3. <span data-ttu-id="2d71c-134">Ändra inställningarna för hello baserat på dina behov.</span><span class="sxs-lookup"><span data-stu-id="2d71c-134">Change hello settings based on your need.</span></span>
4. <span data-ttu-id="2d71c-135">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="2d71c-135">Click **Save**.</span></span> <span data-ttu-id="2d71c-136">hello princip ska sparas i toofive minut, beroende på hur många virtuella datorer använder den principen för lösenordsreplikering.</span><span class="sxs-lookup"><span data-stu-id="2d71c-136">hello policy should be saved in two toofive minutes, depending on how many VMs are using that replication policy.</span></span>

![Spara replikeringsprincip](./media/site-recovery-setup-replication-settings-vmware/Save-Policy.png)

## <a name="dissociate-a-configuration-server-from-a-replication-policy"></a><span data-ttu-id="2d71c-138">Koppla bort en konfigurationsserver från replikeringsprincipen</span><span class="sxs-lookup"><span data-stu-id="2d71c-138">Dissociate a configuration server from a replication policy</span></span>
1. <span data-ttu-id="2d71c-139">Välj hello replikering princip toowhich du vill tooassociate hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="2d71c-139">Choose hello replication policy toowhich you want tooassociate hello configuration server.</span></span>
2. <span data-ttu-id="2d71c-140">Klicka på **Koppla bort**.</span><span class="sxs-lookup"><span data-stu-id="2d71c-140">Click **Dissociate**.</span></span>
3. <span data-ttu-id="2d71c-141">Välj hello konfigurationsservern hello listan över servrar.</span><span class="sxs-lookup"><span data-stu-id="2d71c-141">Select hello configuration server from hello list of servers.</span></span>
4. <span data-ttu-id="2d71c-142">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2d71c-142">Click **OK**.</span></span> <span data-ttu-id="2d71c-143">hello konfigurationsservern ska tas bort i en tootwo minuter.</span><span class="sxs-lookup"><span data-stu-id="2d71c-143">hello configuration server should be dissociated in one tootwo minutes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2d71c-144">Du kan koppla bort en konfigurationsserver om det finns minst ett replikerade objekt med hjälp av hello princip.</span><span class="sxs-lookup"><span data-stu-id="2d71c-144">You cannot dissociate a configuration server if there is at least one replicated item using hello policy.</span></span> <span data-ttu-id="2d71c-145">Kontrollera att det inte finns några replikerade objekt med hjälp av hello princip innan du koppla bort hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="2d71c-145">Make sure there are no replicated items using hello policy before you dissociate hello configuration server.</span></span>

## <a name="delete-a-replication-policy"></a><span data-ttu-id="2d71c-146">Skapa en replikeringsprincip</span><span class="sxs-lookup"><span data-stu-id="2d71c-146">Delete a replication policy</span></span>

1. <span data-ttu-id="2d71c-147">Välj hello replikeringsprincip som du vill toodelete.</span><span class="sxs-lookup"><span data-stu-id="2d71c-147">Choose hello replication policy that you want toodelete.</span></span>
2. <span data-ttu-id="2d71c-148">Klicka på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="2d71c-148">Click **Delete**.</span></span> <span data-ttu-id="2d71c-149">hello principen ska tas bort efter 30 too60 sekunder.</span><span class="sxs-lookup"><span data-stu-id="2d71c-149">hello policy should be deleted in 30 too60 seconds.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2d71c-150">Du kan inte ta bort en replikeringsprincip om den har minst en konfiguration server hör tooit.</span><span class="sxs-lookup"><span data-stu-id="2d71c-150">You cannot delete a replication policy if it has at least one configuration server associated tooit.</span></span> <span data-ttu-id="2d71c-151">Kontrollera att det inte finns några replikerade objekt med hjälp av hello princip och ta bort alla hello associerade configuration-servrar innan du tar bort hello princip.</span><span class="sxs-lookup"><span data-stu-id="2d71c-151">Make sure there are no replicated items using hello policy and delete all hello associated configuration servers before you delete hello policy.</span></span>
