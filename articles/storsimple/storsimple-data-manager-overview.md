---
title: "Översikt över Azure StorSimple Data Manager aaaMicrosoft | Microsoft Docs"
description: "En översikt över hello StorSimple Data Manager-tjänsten (privat förhandsvisning)"
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/22/2016
ms.author: vidarmsft
ms.openlocfilehash: 5d29f7d26be9f2b36857526bdea770d991cece6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-data-manager-overview-private-preview"></a><span data-ttu-id="b51ac-103">Översikt över StorSimple Data Manager (privat förhandsvisning)</span><span class="sxs-lookup"><span data-stu-id="b51ac-103">StorSimple Data Manager overview (Private Preview)</span></span>

## <a name="overview"></a><span data-ttu-id="b51ac-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="b51ac-104">Overview</span></span>

<span data-ttu-id="b51ac-105">Microsoft Azure StorSimple är en hybrid cloud lagringslösning som adresser hello svårigheter av Ostrukturerade data som vanligtvis är kopplad till filresurser.</span><span class="sxs-lookup"><span data-stu-id="b51ac-105">Microsoft Azure StorSimple is a hybrid cloud storage solution that addresses hello complexities of unstructured data commonly associated with file shares.</span></span> <span data-ttu-id="b51ac-106">StorSimple använder lagringsutrymmet i molnet som ett tillägg av hello lokalt lösning och automatiskt nivåer data över hello lokal lagring och lagringsutrymmet i molnet.</span><span class="sxs-lookup"><span data-stu-id="b51ac-106">StorSimple uses cloud storage as an extension of hello on-premises solution and automatically tiers data across hello on-premises storage and cloud storage.</span></span> <span data-ttu-id="b51ac-107">Inbyggt dataskydd med lokala och molnbaserade ögonblicksbilder, eliminerar hello behovet av att en sprawling lagringsinfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="b51ac-107">Integrated data protection, with local and cloud snapshots, eliminates hello need for a sprawling storage infrastructure.</span></span> <span data-ttu-id="b51ac-108">Arkiveringen och katastrofåterställning är också sömlös hello moln fungerar som en annan plats.</span><span class="sxs-lookup"><span data-stu-id="b51ac-108">Archival and disaster recovery is also seamless with hello cloud acting as an offsite location.</span></span>

<span data-ttu-id="b51ac-109">hello omvandling datatjänst som som presenteras i detta dokument, kan du tooseamlessly åtkomst hello StorSimple data i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="b51ac-109">hello data transformation service that we are introducing in this document, allows you tooseamlessly access hello StorSimple data in hello cloud.</span></span> <span data-ttu-id="b51ac-110">Den här tjänsten innehåller API: er tooextract data från StorSimple och presentera den tooother Azure services i ett format som lätt kan användas.</span><span class="sxs-lookup"><span data-stu-id="b51ac-110">This service provides APIs tooextract data from StorSimple and present it tooother Azure services in formats that can be readily consumed.</span></span> <span data-ttu-id="b51ac-111">hello-format som stöds i den här förhandsgranskningen är Azure-blobbar och Azure Media Services tillgångar.</span><span class="sxs-lookup"><span data-stu-id="b51ac-111">hello formats supported in this preview are Azure blobs and Azure Media Services assets.</span></span> <span data-ttu-id="b51ac-112">Den här omvandlingen kan du tooeasily överföring av tjänster, till exempel Azure Media Services, Azure HDInsight, Azure Machine Learning och Azure Search toooperate på StorSimple 8000-serien lokala enhet.</span><span class="sxs-lookup"><span data-stu-id="b51ac-112">This transformation enables you tooeasily wire up services such as Azure Media Services, Azure HDInsight, Azure Machine Learning, and Azure Search toooperate data on StorSimple 8000 series on-premises device.</span></span>

<span data-ttu-id="b51ac-113">En övergripande blockdiagram som illustrerar detta visas nedan.</span><span class="sxs-lookup"><span data-stu-id="b51ac-113">A high-level block diagram illustrating this is shown below.</span></span>

![Översiktsdiagram](./media//storsimple-data-manager-overview/high-level-diagram.png)

<span data-ttu-id="b51ac-115">Det här dokumentet förklarar hur du registrerar dig för en privat förhandsgranskning av den här tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b51ac-115">This document explains how you can sign up for a private preview of this service.</span></span> <span data-ttu-id="b51ac-116">Här beskrivs också hur du kan använda den här tjänsten toowrite program som använder StorSimple data och andra Azure-tjänster i molnet hello.</span><span class="sxs-lookup"><span data-stu-id="b51ac-116">It also explains how you can use this service toowrite applications that use StorSimple data and other Azure services in hello cloud.</span></span>

## <a name="sign-up-for-data-manager-preview"></a><span data-ttu-id="b51ac-117">Registrera dig för förhandsversionen Data Manager</span><span class="sxs-lookup"><span data-stu-id="b51ac-117">Sign up for Data Manager preview</span></span>
<span data-ttu-id="b51ac-118">Granska hello följande krav innan du registrerar dig för hello Data Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b51ac-118">Before you sign up for hello Data Manager service, review hello following prerequisites.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="b51ac-119">Krav</span><span class="sxs-lookup"><span data-stu-id="b51ac-119">Prerequisites</span></span>

<span data-ttu-id="b51ac-120">Den här övningen förutsätter att du har</span><span class="sxs-lookup"><span data-stu-id="b51ac-120">This exercise assumes that you have</span></span>
* <span data-ttu-id="b51ac-121">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b51ac-121">an active Azure subscription.</span></span>
* <span data-ttu-id="b51ac-122">åtkomst tooa registrerad StorSimple 8000-serieenhet</span><span class="sxs-lookup"><span data-stu-id="b51ac-122">access tooa registered StorSimple 8000 series device</span></span>
* <span data-ttu-id="b51ac-123">alla hello nycklar som är associerade med hello StorSimple 8000-serieenhet.</span><span class="sxs-lookup"><span data-stu-id="b51ac-123">all hello keys associated with hello StorSimple 8000 series device.</span></span>

### <a name="sign-up"></a><span data-ttu-id="b51ac-124">Registrera dig</span><span class="sxs-lookup"><span data-stu-id="b51ac-124">Sign up</span></span>

<span data-ttu-id="b51ac-125">Hej StorSimple Data Manager är i privat förhandsvisning.</span><span class="sxs-lookup"><span data-stu-id="b51ac-125">hello StorSimple Data Manager is in private preview.</span></span> <span data-ttu-id="b51ac-126">Utför hello följande steg toosign för en privat förhandsgranskning av den här tjänsten:</span><span class="sxs-lookup"><span data-stu-id="b51ac-126">Perform hello following steps toosign up for a private preview of this service:</span></span>

1.  <span data-ttu-id="b51ac-127">Logga in på hello Azure-portalen med hello StorSimple Data Manager-tillägg på: [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager).</span><span class="sxs-lookup"><span data-stu-id="b51ac-127">Log into hello Azure portal with hello StorSimple Data Manager extension at: [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager).</span></span> <span data-ttu-id="b51ac-128">Använd dina autentiseringsuppgifter för Azure-toolog i.</span><span class="sxs-lookup"><span data-stu-id="b51ac-128">Use your Azure credentials toolog in.</span></span>

2.  <span data-ttu-id="b51ac-129">Klicka på hello  **+**  ikonen toocreate en tjänst.</span><span class="sxs-lookup"><span data-stu-id="b51ac-129">Click hello **+** icon toocreate a service.</span></span> <span data-ttu-id="b51ac-130">Klicka på **lagring** och klicka sedan på **finns alla** i hello bladet som öppnas.</span><span class="sxs-lookup"><span data-stu-id="b51ac-130">Click **Storage** and then click **See All** in hello blade that opens up.</span></span>

    ![Sökikonen StorSimple Data Manager](./media/storsimple-data-manager-overview/search-data-manager-icon.png)

3. <span data-ttu-id="b51ac-132">Ikonen hello StorSimple Data Manager.</span><span class="sxs-lookup"><span data-stu-id="b51ac-132">You see hello StorSimple Data Manager icon.</span></span>

    ![Välj Data StorSimple Manager-ikon](./media/storsimple-data-manager-overview/select-data-manager-icon.png)

4. <span data-ttu-id="b51ac-134">StorSimple Data Manager-ikonen och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="b51ac-134">Click StorSimple Data Manager icon and then click **Create**.</span></span> <span data-ttu-id="b51ac-135">Välj hello prenumeration som du vill tooenable för hello privat förhandsgranskning och klicka sedan på **registrera mig!**</span><span class="sxs-lookup"><span data-stu-id="b51ac-135">Pick hello subscription that you want tooenable for hello private preview and then click **Sign me up!**</span></span>

    ![Registrera mig](./media/storsimple-data-manager-overview/sign-me-up.png)

5. <span data-ttu-id="b51ac-137">Detta skickar en begäran tooonboard du.</span><span class="sxs-lookup"><span data-stu-id="b51ac-137">This sends a request tooonboard you.</span></span> <span data-ttu-id="b51ac-138">Vi kommer att publicera du så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="b51ac-138">We will onboard you as soon as possible.</span></span> <span data-ttu-id="b51ac-139">När prenumerationen har aktiverats kan skapa du en StorSimple Data Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b51ac-139">After your subscription is enabled, you can create a StorSimple Data Manager service.</span></span>

6. <span data-ttu-id="b51ac-140">tooeasily komma åt hello StorSimple Data Manager-tjänsten klickar du på hello stjärnikonen toopin den tooyour Favoriter.</span><span class="sxs-lookup"><span data-stu-id="b51ac-140">tooeasily access hello StorSimple Data Manager service, click hello star icon toopin it tooyour favorites.</span></span>

    ![Åtkomst till StorSimple Data chefer](./media/storsimple-data-manager-overview/access-data-managers.png)


## <a name="next-steps"></a><span data-ttu-id="b51ac-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b51ac-142">Next steps</span></span>

<span data-ttu-id="b51ac-143">[Använda StorSimple Data-hanterarens användargränssnitt tootransform dina data](storsimple-data-manager-ui.md).</span><span class="sxs-lookup"><span data-stu-id="b51ac-143">[Use StorSimple Data Manager UI tootransform your data](storsimple-data-manager-ui.md).</span></span>
