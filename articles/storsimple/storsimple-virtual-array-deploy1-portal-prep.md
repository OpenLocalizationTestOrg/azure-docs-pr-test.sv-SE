---
title: "aaaPortal prep för virtuell StorSimple-matrisen | Microsoft Docs"
description: "Första kursen toodeploy virtuella StorSimple-matris innebär att förbereda hello Azure-portalen"
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 68a4cfd3-94c9-46cb-805c-46217290ce02
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5332b235e7296a9274f2e7dafcdf72f4b9cdadf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array---prepare-hello-azure-portal"></a><span data-ttu-id="eae4f-103">Distribuera StorSimple virtuell matris – förbereda hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="eae4f-103">Deploy StorSimple Virtual Array - Prepare hello Azure portal</span></span>

![](./media/storsimple-virtual-array-deploy1-portal-prep/getstarted4.png)
## <a name="overview"></a><span data-ttu-id="eae4f-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="eae4f-104">Overview</span></span>

<span data-ttu-id="eae4f-105">Detta är hello första artikeln i hello serie självstudier för distribution krävs toocompletely distribuera virtuella matrisen som en filserver eller en iSCSI-server som använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="eae4f-105">This is hello first article in hello series of deployment tutorials required toocompletely deploy your virtual array as a file server or an iSCSI server using hello Resource Manager model.</span></span> <span data-ttu-id="eae4f-106">Den här artikeln beskriver hello förberedelser krävs toocreate och konfigurera din StorSimple Enhetshanteraren service tidigare tooprovisioning virtuella matris.</span><span class="sxs-lookup"><span data-stu-id="eae4f-106">This article describes hello preparation required toocreate and configure your StorSimple Device Manager service prior tooprovisioning a virtual array.</span></span> <span data-ttu-id="eae4f-107">Den här artikeln innehåller också länkar ut tooa configuration checklista och konfiguration kraven för distribution.</span><span class="sxs-lookup"><span data-stu-id="eae4f-107">This article also links out tooa deployment configuration checklist and configuration prerequisites.</span></span>

<span data-ttu-id="eae4f-108">Du måste administratören privilegier toocomplete hello installationen och konfigurationen processen.</span><span class="sxs-lookup"><span data-stu-id="eae4f-108">You need administrator privileges toocomplete hello setup and configuration process.</span></span> <span data-ttu-id="eae4f-109">Vi rekommenderar att du granskar hello checklista för distributionskonfiguration innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="eae4f-109">We recommend that you review hello deployment configuration checklist before you begin.</span></span> <span data-ttu-id="eae4f-110">hello portal förberedelse tar mindre än 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="eae4f-110">hello portal preparation takes less than 10 minutes.</span></span>

<span data-ttu-id="eae4f-111">hello information som publicerats i den här artikeln gäller toohello distribution av virtuella StorSimple-matriser i hello Azure-portalen och Microsoft Azure Government-molnet.</span><span class="sxs-lookup"><span data-stu-id="eae4f-111">hello information published in this article applies toohello deployment of StorSimple Virtual Arrays in hello Azure portal and Microsoft Azure Government Cloud.</span></span>

### <a name="get-started"></a><span data-ttu-id="eae4f-112">Kom igång</span><span class="sxs-lookup"><span data-stu-id="eae4f-112">Get started</span></span>
<span data-ttu-id="eae4f-113">arbetsflöde för distribution av hello består av förbereda hello portal, etablera en virtuell matris i din virtualiserade miljö och hello-installationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="eae4f-113">hello deployment workflow consists of preparing hello portal, provisioning a virtual array in your virtualized environment, and completing hello setup.</span></span> <span data-ttu-id="eae4f-114">tooget igång med hello virtuella StorSimple-matris distribution som en filserver eller en iSCSI-server, måste toorefer toohello följande tabellform resurser.</span><span class="sxs-lookup"><span data-stu-id="eae4f-114">tooget started with hello StorSimple Virtual Array deployment as a file server or an iSCSI server, you need toorefer toohello following tabulated resources.</span></span>

#### <a name="deployment-articles"></a><span data-ttu-id="eae4f-115">Artiklar om distribution</span><span class="sxs-lookup"><span data-stu-id="eae4f-115">Deployment articles</span></span>

<span data-ttu-id="eae4f-116">toodeploy din virtuella StorSimple-matris finns toohello följande artiklar i hello föreskrivs sekvens.</span><span class="sxs-lookup"><span data-stu-id="eae4f-116">toodeploy your StorSimple Virtual Array, refer toohello following articles in hello prescribed sequence.</span></span>

| **#** | <span data-ttu-id="eae4f-117">**I det här steget**</span><span class="sxs-lookup"><span data-stu-id="eae4f-117">**In this step**</span></span> | <span data-ttu-id="eae4f-118">**Det gör du...**</span><span class="sxs-lookup"><span data-stu-id="eae4f-118">**You do this …**</span></span> | <span data-ttu-id="eae4f-119">**Och använda dessa dokument.**</span><span class="sxs-lookup"><span data-stu-id="eae4f-119">**And use these documents.**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="eae4f-120">1.</span><span class="sxs-lookup"><span data-stu-id="eae4f-120">1.</span></span> |<span data-ttu-id="eae4f-121">**Ställ in hello Azure-portalen**</span><span class="sxs-lookup"><span data-stu-id="eae4f-121">**Set up hello Azure portal**</span></span> |<span data-ttu-id="eae4f-122">Skapa och konfigurera din StorSimple Enhetshanteraren service tidigare tooprovisioning en virtuell StorSimple-matris.</span><span class="sxs-lookup"><span data-stu-id="eae4f-122">Create and configure your StorSimple Device Manager service prior tooprovisioning a StorSimple Virtual Array.</span></span> |[<span data-ttu-id="eae4f-123">Förbereda hello-portalen</span><span class="sxs-lookup"><span data-stu-id="eae4f-123">Prepare hello portal</span></span>](storsimple-virtual-array-deploy1-portal-prep.md) |
| <span data-ttu-id="eae4f-124">2.</span><span class="sxs-lookup"><span data-stu-id="eae4f-124">2.</span></span> |<span data-ttu-id="eae4f-125">**Etablera hello virtuella matris**</span><span class="sxs-lookup"><span data-stu-id="eae4f-125">**Provision hello Virtual Array**</span></span> |<span data-ttu-id="eae4f-126">Etablera för Hyper-V och Anslut tooa virtuella StorSimple-matris på ett värdsystem som kör Hyper-V på Windows Server 2012 R2, Windows Server 2012 eller Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="eae4f-126">For Hyper-V, provision and connect tooa StorSimple Virtual Array on a host system running Hyper-V on Windows Server 2012 R2, Windows Server 2012, or Windows Server 2008 R2.</span></span> <br></br> <br></br> <span data-ttu-id="eae4f-127">Etablera för VMware, och Anslut tooa virtuella StorSimple-matris på ett värdsystem som kör VMware ESXi 5.5 och senare.</span><span class="sxs-lookup"><span data-stu-id="eae4f-127">For VMware, provision and connect tooa StorSimple Virtual Array on a host system running VMware ESXi 5.5 and above.</span></span><br></br> |[<span data-ttu-id="eae4f-128">Etablera en virtuell matris i Hyper-V</span><span class="sxs-lookup"><span data-stu-id="eae4f-128">Provision a virtual array in Hyper-V</span></span>](storsimple-virtual-array-deploy2-provision-hyperv.md) <br></br> <br></br> [<span data-ttu-id="eae4f-129">Etablera en virtuell VMware-matris</span><span class="sxs-lookup"><span data-stu-id="eae4f-129">Provision a virtual array in VMware</span></span>](storsimple-virtual-array-deploy2-provision-vmware.md) |
| <span data-ttu-id="eae4f-130">3.</span><span class="sxs-lookup"><span data-stu-id="eae4f-130">3.</span></span> |<span data-ttu-id="eae4f-131">**Ställ in hello virtuella matris**</span><span class="sxs-lookup"><span data-stu-id="eae4f-131">**Set up hello Virtual Array**</span></span> |<span data-ttu-id="eae4f-132">Utföra installationen för din filserver, registrera din StorSimple-filserver och slutför hello Enhetsinställningar.</span><span class="sxs-lookup"><span data-stu-id="eae4f-132">For your file server, perform initial setup, register your StorSimple file server, and complete hello device setup.</span></span> <span data-ttu-id="eae4f-133">Du kan sedan etablera SMB-resurser.</span><span class="sxs-lookup"><span data-stu-id="eae4f-133">You can then provision SMB shares.</span></span> <br></br> <br></br> <span data-ttu-id="eae4f-134">Utföra installationen för iSCSI-server, registrera StorSimple iSCSI-servern och slutför hello Enhetsinställningar.</span><span class="sxs-lookup"><span data-stu-id="eae4f-134">For your iSCSI server, perform initial setup, register your StorSimple iSCSI server, and complete hello device setup.</span></span> <span data-ttu-id="eae4f-135">Du kan sedan etablera iSCSI-volymer.</span><span class="sxs-lookup"><span data-stu-id="eae4f-135">You can then provision iSCSI volumes.</span></span> |[<span data-ttu-id="eae4f-136">Konfigurera virtuella matris som filserver</span><span class="sxs-lookup"><span data-stu-id="eae4f-136">Set up virtual array as file server</span></span>](storsimple-virtual-array-deploy3-fs-setup.md)<br></br> <br></br>[<span data-ttu-id="eae4f-137">Konfigurera virtuella matris som iSCSI-server</span><span class="sxs-lookup"><span data-stu-id="eae4f-137">Set up virtual array as iSCSI server</span></span>](storsimple-virtual-array-deploy3-iscsi-setup.md) |

<span data-ttu-id="eae4f-138">Nu kan du börja tooset in hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="eae4f-138">You can now begin tooset up hello Azure portal.</span></span>

## <a name="configuration-checklist"></a><span data-ttu-id="eae4f-139">Checklista för konfiguration</span><span class="sxs-lookup"><span data-stu-id="eae4f-139">Configuration checklist</span></span>

<span data-ttu-id="eae4f-140">Checklista för konfiguration av hello beskriver hello-information som du behöver toocollect innan du konfigurerar hello programvara på din virtuella StorSimple-matrisen.</span><span class="sxs-lookup"><span data-stu-id="eae4f-140">hello configuration checklist describes hello information that you need toocollect before you configure hello software on your StorSimple Virtual Array.</span></span> <span data-ttu-id="eae4f-141">Förbereder den här informationen i tid hjälper förenklad hello processen för att distribuera hello StorSimple-enheten i din miljö.</span><span class="sxs-lookup"><span data-stu-id="eae4f-141">Preparing this information ahead of time helps streamline hello process of deploying hello StorSimple device in your environment.</span></span> <span data-ttu-id="eae4f-142">Beroende på om din virtuella StorSimple-matris distribueras som en filserver eller en iSCSI-server, behöver du något av följande checklistor hello.</span><span class="sxs-lookup"><span data-stu-id="eae4f-142">Depending upon whether your StorSimple Virtual Array is deployed as a file server or an iSCSI server, you need one of hello following checklists.</span></span>

* <span data-ttu-id="eae4f-143">Hämta hello [StorSimple virtuell matris File Server checklista för distributionskonfiguration](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).</span><span class="sxs-lookup"><span data-stu-id="eae4f-143">Download hello [StorSimple Virtual Array File Server Configuration Checklist](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).</span></span>
* <span data-ttu-id="eae4f-144">Hämta hello [virtuella StorSimple-matris iSCSI checklista för konfiguration av servern](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).</span><span class="sxs-lookup"><span data-stu-id="eae4f-144">Download hello [StorSimple Virtual Array iSCSI Server Configuration Checklist](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eae4f-145">Krav</span><span class="sxs-lookup"><span data-stu-id="eae4f-145">Prerequisites</span></span>

<span data-ttu-id="eae4f-146">Här hittar du hello konfigurationskraven för din StorSimple Enhetshanteraren tjänst din virtuella StorSimple-matris och hello datacenternätverket.</span><span class="sxs-lookup"><span data-stu-id="eae4f-146">Here you find hello configuration prerequisites for your StorSimple Device Manager service, your StorSimple Virtual Array, and hello datacenter network.</span></span>

### <a name="for-hello-storsimple-device-manager-service"></a><span data-ttu-id="eae4f-147">För hello StorSimple enheten Manager-tjänsten</span><span class="sxs-lookup"><span data-stu-id="eae4f-147">For hello StorSimple Device Manager service</span></span>

<span data-ttu-id="eae4f-148">Innan du börjar bör du kontrollera att:</span><span class="sxs-lookup"><span data-stu-id="eae4f-148">Before you begin, make sure that:</span></span>

* <span data-ttu-id="eae4f-149">Du har ditt Microsoft-konto med autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="eae4f-149">You have your Microsoft account with access credentials.</span></span>
* <span data-ttu-id="eae4f-150">Du har ditt Microsoft Azure lagringskonto med autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="eae4f-150">You have your Microsoft Azure storage account with access credentials.</span></span>
* <span data-ttu-id="eae4f-151">Microsoft Azure-prenumerationen ska aktiveras för StorSimple enheten Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="eae4f-151">Your Microsoft Azure subscription should be enabled for StorSimple Device Manager service.</span></span>

### <a name="for-hello-storsimple-virtual-array"></a><span data-ttu-id="eae4f-152">För hello virtuella StorSimple-matris</span><span class="sxs-lookup"><span data-stu-id="eae4f-152">For hello StorSimple Virtual Array</span></span>

<span data-ttu-id="eae4f-153">Innan du distribuerar en virtuell matris, kontrollerar du att:</span><span class="sxs-lookup"><span data-stu-id="eae4f-153">Before you deploy a virtual array, make sure that:</span></span>

* <span data-ttu-id="eae4f-154">Du har åtkomst tooa värdsystem som kör Hyper-V på Windows Server 2008 R2 eller senare eller VMware (ESXi 5.5 eller senare) som kan använda tooa etablera en enhet.</span><span class="sxs-lookup"><span data-stu-id="eae4f-154">You have access tooa host system running Hyper-V on Windows Server 2008 R2 or later or VMware (ESXi 5.5 or later) that can be used tooa provision a device.</span></span>
* <span data-ttu-id="eae4f-155">hello värdsystem är kan toodedicate hello följande resurser tooprovision virtuella matrisen:</span><span class="sxs-lookup"><span data-stu-id="eae4f-155">hello host system is able toodedicate hello following resources tooprovision your virtual array:</span></span>
  
  * <span data-ttu-id="eae4f-156">Minst 4 kärnor.</span><span class="sxs-lookup"><span data-stu-id="eae4f-156">A minimum of 4 cores.</span></span>
  * <span data-ttu-id="eae4f-157">Minst 8 GB RAM-minne.</span><span class="sxs-lookup"><span data-stu-id="eae4f-157">At least 8 GB of RAM.</span></span> <span data-ttu-id="eae4f-158">Om du planerar tooconfigure hello virtuella matris som filserver stöder 8 GB 2 miljoner filer.</span><span class="sxs-lookup"><span data-stu-id="eae4f-158">If you plan tooconfigure hello virtual array as file server, 8 GB supports 2 million files.</span></span> <span data-ttu-id="eae4f-159">Du måste 16 GB RAM-minne toosupport 2-4 miljoner filer.</span><span class="sxs-lookup"><span data-stu-id="eae4f-159">You need 16 GB RAM toosupport 2 - 4 million files.</span></span>
  * <span data-ttu-id="eae4f-160">Ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="eae4f-160">One network interface.</span></span>
  * <span data-ttu-id="eae4f-161">En virtuell disk 500 GB efter systemdata.</span><span class="sxs-lookup"><span data-stu-id="eae4f-161">A 500 GB virtual disk for system data.</span></span>

### <a name="for-hello-datacenter-network"></a><span data-ttu-id="eae4f-162">För hello datacenternätverket</span><span class="sxs-lookup"><span data-stu-id="eae4f-162">For hello datacenter network</span></span>

<span data-ttu-id="eae4f-163">Innan du börjar bör du kontrollera att:</span><span class="sxs-lookup"><span data-stu-id="eae4f-163">Before you begin, make sure that:</span></span>

* <span data-ttu-id="eae4f-164">hello nätverket i datacentret har konfigurerats enligt hello nätverkskrav för din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="eae4f-164">hello network in your datacenter is configured as per hello networking requirements for your StorSimple device.</span></span> <span data-ttu-id="eae4f-165">Mer information finns i hello [StorSimple virtuell matris systemkrav](storsimple-ova-system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="eae4f-165">For more information, see hello [StorSimple Virtual Array System Requirements](storsimple-ova-system-requirements.md).</span></span>
* <span data-ttu-id="eae4f-166">Din virtuella StorSimple-matris har en dedikerad 5 Mbit/s internetbandbredd (eller mer) vid alla tidpunkter.</span><span class="sxs-lookup"><span data-stu-id="eae4f-166">Your StorSimple Virtual Array has a dedicated 5 Mbps Internet bandwidth (or more) available at all times.</span></span> <span data-ttu-id="eae4f-167">Den här bandbredden ska inte delas med andra program.</span><span class="sxs-lookup"><span data-stu-id="eae4f-167">This bandwidth should not be shared with any other applications.</span></span>

## <a name="step-by-step-preparation"></a><span data-ttu-id="eae4f-168">Förberedelse av steg</span><span class="sxs-lookup"><span data-stu-id="eae4f-168">Step-by-step preparation</span></span>

<span data-ttu-id="eae4f-169">Använd hello följa instruktionerna tooprepare din portal för hello StorSimple enheten Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="eae4f-169">Use hello following step-by-step instructions tooprepare your portal for hello StorSimple Device Manager service.</span></span>

## <a name="step-1-create-a-new-service"></a><span data-ttu-id="eae4f-170">Steg 1: Skapa en ny tjänst</span><span class="sxs-lookup"><span data-stu-id="eae4f-170">Step 1: Create a new service</span></span>

<span data-ttu-id="eae4f-171">En instans av hello StorSimple enheten Manager-tjänsten kan hantera flera virtuella StorSimple-matriser.</span><span class="sxs-lookup"><span data-stu-id="eae4f-171">A single instance of hello StorSimple Device Manager service can manage multiple StorSimple Virtual Arrays.</span></span> <span data-ttu-id="eae4f-172">Utföra hello följande steg toocreate en instans av hello StorSimple Device Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="eae4f-172">Perform hello following steps toocreate an instance of hello StorSimple Device Manager service.</span></span> <span data-ttu-id="eae4f-173">Om du har en befintlig StorSimple Enhetshanteraren service toomanage din virtuella matriser, hoppa över det här steget och gå för[steg 2: hämta hello nyckel för tjänstregistrering](#step-2-get-the-service-registration-key).</span><span class="sxs-lookup"><span data-stu-id="eae4f-173">If you have an existing StorSimple Device Manager service toomanage your virtual arrays, skip this step and go too[Step 2: Get hello service registration key](#step-2-get-the-service-registration-key).</span></span>

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

> [!IMPORTANT]
> <span data-ttu-id="eae4f-174">Om du inte har aktiverat hello automatiskt skapande av lagringskonton med din tjänst måste toocreate minst ett lagringskonto efter att du har skapat en tjänst.</span><span class="sxs-lookup"><span data-stu-id="eae4f-174">If you did not enable hello automatic creation of a storage account with your service, you will need toocreate at least one storage account after you have successfully created a service.</span></span>
> 
> * <span data-ttu-id="eae4f-175">Om du inte har skapat ett lagringskonto automatiskt, gå för[konfigurera ett nytt lagringskonto för tjänsten hello](#optional-step-configure-a-new-storage-account-for-the-service) detaljerade anvisningar.</span><span class="sxs-lookup"><span data-stu-id="eae4f-175">If you did not create a storage account automatically, go too[Configure a new storage account for hello service](#optional-step-configure-a-new-storage-account-for-the-service) for detailed instructions.</span></span>
> * <span data-ttu-id="eae4f-176">Om du har aktiverat hello automatiskt skapande av lagringskonton går för[steg 2: hämta hello nyckel för tjänstregistrering](#step-2-get-the-service-registration-key).</span><span class="sxs-lookup"><span data-stu-id="eae4f-176">If you enabled hello automatic creation of a storage account, go too[Step 2: Get hello service registration key](#step-2-get-the-service-registration-key).</span></span>
> 
> 

## <a name="step-2-get-hello-service-registration-key"></a><span data-ttu-id="eae4f-177">Steg 2: Hämta nyckel för tjänstregistrering hello</span><span class="sxs-lookup"><span data-stu-id="eae4f-177">Step 2: Get hello service registration key</span></span>

<span data-ttu-id="eae4f-178">När hello StorSimple enheten Manager-tjänsten är igång kan behöver du tooget hello nyckel för tjänstregistrering.</span><span class="sxs-lookup"><span data-stu-id="eae4f-178">After hello StorSimple Device Manager service is up and running, you will need tooget hello service registration key.</span></span> <span data-ttu-id="eae4f-179">Den här nyckeln används tooregister och ansluta din StorSimple-enhet med hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="eae4f-179">This key is used tooregister and connect your StorSimple device with hello service.</span></span>

<span data-ttu-id="eae4f-180">Utför följande steg i hello hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="eae4f-180">Perform hello following steps in hello [Azure portal](https://portal.azure.com/).</span></span>

[!INCLUDE [storsimple-virtual-array-get-service-registration-key](../../includes/storsimple-virtual-array-get-service-registration-key.md)]

> [!NOTE]
> <span data-ttu-id="eae4f-181">hello nyckel för tjänstregistrering är används tooregister alla hello Enhetshanteraren för StorSimple-enheter som behöver tooregister med din StorSimple Device Manager-tjänst.</span><span class="sxs-lookup"><span data-stu-id="eae4f-181">hello service registration key is used tooregister all hello StorSimple Device Manager devices that need tooregister with your StorSimple Device Manager service.</span></span>
> 
> 

## <a name="step-3-download-hello-virtual-array-image"></a><span data-ttu-id="eae4f-182">Steg 3: Hämta hello virtuella matris bild</span><span class="sxs-lookup"><span data-stu-id="eae4f-182">Step 3: Download hello virtual array image</span></span>

<span data-ttu-id="eae4f-183">När du har hello nyckel för tjänstregistrering måste toodownload hello lämpliga virtuella matris avbildningen tooprovision virtuella matrisen på värdsystemet.</span><span class="sxs-lookup"><span data-stu-id="eae4f-183">After you have hello service registration key, you will need toodownload hello appropriate virtual array image tooprovision a virtual array on your host system.</span></span> <span data-ttu-id="eae4f-184">hello virtuella matris bilder är specifika för operativsystemet och kan hämtas från hello snabbstartsidan i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="eae4f-184">hello virtual array images are operating system specific and can be downloaded from hello Quick Start page in hello Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="eae4f-185">hello-program som körs på hello virtuella StorSimple-matrisen får endast användas med hello StorSimple enheten Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="eae4f-185">hello software running on hello StorSimple Virtual Array may only be used with hello StorSimple Device Manager service.</span></span>
> 
> 

<span data-ttu-id="eae4f-186">Utför följande steg i hello hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="eae4f-186">Perform hello following steps in hello [Azure portal](https://portal.azure.com/).</span></span>

#### <a name="tooget-hello-virtual-array-image"></a><span data-ttu-id="eae4f-187">tooget hello virtuella matris bild</span><span class="sxs-lookup"><span data-stu-id="eae4f-187">tooget hello virtual array image</span></span>

1. <span data-ttu-id="eae4f-188">Logga in på hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="eae4f-188">Sign into hello [Azure portal](https://portal.azure.com/).</span></span> 
2. <span data-ttu-id="eae4f-189">I hello Azure-portalen klickar du på **Bläddra > StorSimple enhetshanterare**.</span><span class="sxs-lookup"><span data-stu-id="eae4f-189">In hello Azure portal, click **Browse > StorSimple Device Managers**.</span></span>
3. <span data-ttu-id="eae4f-190">Välj en befintlig StorSimple Device Manager-tjänst.</span><span class="sxs-lookup"><span data-stu-id="eae4f-190">Select an existing StorSimple Device Manager service.</span></span> <span data-ttu-id="eae4f-191">I hello **StorSimple Enhetshanteraren** bladet, klickar du på **Snabbstart**.</span><span class="sxs-lookup"><span data-stu-id="eae4f-191">In hello **StorSimple Device Manager** blade, click **Quick Start**.</span></span> 
4. <span data-ttu-id="eae4f-192">Klicka på hello länken motsvarande toohello bild som du vill toodownload från hello Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="eae4f-192">Click hello link corresponding toohello image that you want toodownload from hello Microsoft Download Center.</span></span> <span data-ttu-id="eae4f-193">hello bildfiler är ungefär 4,8 GB.</span><span class="sxs-lookup"><span data-stu-id="eae4f-193">hello image files are approximately 4.8 GB.</span></span>
   
   * <span data-ttu-id="eae4f-194">VHDX för Hyper-V på Windows Server 2012 och senare</span><span class="sxs-lookup"><span data-stu-id="eae4f-194">VHDX for Hyper-V on Windows Server 2012 and later</span></span>
   * <span data-ttu-id="eae4f-195">VHD för Hyper-V på Windows Server 2008 R2 och senare</span><span class="sxs-lookup"><span data-stu-id="eae4f-195">VHD for Hyper-V on Windows Server 2008 R2 and later</span></span>
   * <span data-ttu-id="eae4f-196">VMDK för VMWare ESXi 5.5 och senare</span><span class="sxs-lookup"><span data-stu-id="eae4f-196">VMDK for VMWare ESXi 5.5 and later</span></span>
5. <span data-ttu-id="eae4f-197">Hämta och packa upp hello filen tooa lokal enhet, och ett meddelande om där hello uppackade filen finns.</span><span class="sxs-lookup"><span data-stu-id="eae4f-197">Download and unzip hello file tooa local drive, making a note of where hello unzipped file is located.</span></span>

## <a name="optional-step-configure-a-new-storage-account-for-hello-service"></a><span data-ttu-id="eae4f-198">Valfritt steg: konfigurera ett nytt lagringskonto för hello-tjänsten</span><span class="sxs-lookup"><span data-stu-id="eae4f-198">Optional step: Configure a new storage account for hello service</span></span>

<span data-ttu-id="eae4f-199">Det här steget är valfritt och bör bara genomföras om du inte har aktiverat hello automatiskt skapande av lagringskonton med din tjänst.</span><span class="sxs-lookup"><span data-stu-id="eae4f-199">This step is optional and should be performed only if you did not enable hello automatic creation of a storage account with your service.</span></span>

<span data-ttu-id="eae4f-200">Om du behöver toocreate ett Azure storage-konto i en annan region finns [hur toocreate ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) stegvisa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="eae4f-200">If you need toocreate an Azure storage account in a different region, see [How toocreate a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) for step-by-step instructions.</span></span>

<span data-ttu-id="eae4f-201">Utför följande steg i hello hello [Azure-portalen](https://ms.portal.azure.com/) på hello StorSimple Enhetshanteraren service sidan tooadd ett befintligt Microsoft Azure-lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="eae4f-201">Perform hello following steps in hello [Azure portal](https://ms.portal.azure.com/) on hello StorSimple Device Manager service page tooadd an existing Microsoft Azure storage account.</span></span>

#### <a name="tooadd-a-storage-account-credential-that-has-hello-same-azure-subscription-as-hello-device-manager-service"></a><span data-ttu-id="eae4f-202">tooadd storage-konto autentiseringsuppgifter som har hello samma Azure-prenumeration som hello Enhetshanteraren tjänst</span><span class="sxs-lookup"><span data-stu-id="eae4f-202">tooadd a storage account credential that has hello same Azure subscription as hello Device Manager service</span></span>

1. <span data-ttu-id="eae4f-203">Navigera tooyour Device Manager-tjänsten, väljer och dubbelklicka på den.</span><span class="sxs-lookup"><span data-stu-id="eae4f-203">Navigate tooyour Device Manager service, select and double-click it.</span></span> <span data-ttu-id="eae4f-204">Då öppnas hello **översikt** bladet.</span><span class="sxs-lookup"><span data-stu-id="eae4f-204">This opens hello **Overview** blade.</span></span>
2. <span data-ttu-id="eae4f-205">Välj **lagringskontouppgifter** inom hello **Configuration** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="eae4f-205">Select **Storage account credentials** within hello **Configuration** section.</span></span>
3. <span data-ttu-id="eae4f-206">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="eae4f-206">Click **Add**.</span></span>
4. <span data-ttu-id="eae4f-207">I hello **Lägg till ett lagringskonto** bladet hello följande:</span><span class="sxs-lookup"><span data-stu-id="eae4f-207">In hello **Add a storage account** blade, do hello following:</span></span>
   
    1. <span data-ttu-id="eae4f-208">För **prenumeration**väljer **aktuella**.</span><span class="sxs-lookup"><span data-stu-id="eae4f-208">For **Subscription**, select **Current**.</span></span>
   
    2. <span data-ttu-id="eae4f-209">Ange hello namnet på ditt Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="eae4f-209">Provide hello name of your Azure storage account.</span></span>
   
    3. <span data-ttu-id="eae4f-210">Välj **aktivera** toocreate en säker kanal för nätverkskommunikation mellan din StorSimple-enhet och hello moln.</span><span class="sxs-lookup"><span data-stu-id="eae4f-210">Select **Enable** toocreate a secure channel for network communication between your StorSimple device and hello cloud.</span></span> <span data-ttu-id="eae4f-211">Välj **inaktivera** endast om du arbetar inom ett privat moln.</span><span class="sxs-lookup"><span data-stu-id="eae4f-211">Select **Disable** only if you are operating within a private cloud.</span></span>
   
    4. <span data-ttu-id="eae4f-212">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="eae4f-212">Click **Add**.</span></span> <span data-ttu-id="eae4f-213">Du meddelas när hello storage-konto har skapats.</span><span class="sxs-lookup"><span data-stu-id="eae4f-213">You are notified after hello storage account is successfully created.</span></span><br></br>
   
     ![Lägg till en befintlig lagring konto autentiseringsuppgift](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-storageacct.png)

## <a name="next-step"></a><span data-ttu-id="eae4f-215">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eae4f-215">Next step</span></span>

<span data-ttu-id="eae4f-216">hello nästa steg är tooprovision en virtuell dator för din virtuella StorSimple-matris.</span><span class="sxs-lookup"><span data-stu-id="eae4f-216">hello next step is tooprovision a virtual machine for your StorSimple Virtual Array.</span></span> <span data-ttu-id="eae4f-217">Beroende på värdoperativsystemet finns hello detaljerade instruktioner i:</span><span class="sxs-lookup"><span data-stu-id="eae4f-217">Depending on your host operating system, see hello detailed instructions in:</span></span>

* [<span data-ttu-id="eae4f-218">Etablera en virtuell StorSimple-matris i Hyper-V</span><span class="sxs-lookup"><span data-stu-id="eae4f-218">Provision a StorSimple Virtual Array in Hyper-V</span></span>](storsimple-virtual-array-deploy2-provision-hyperv.md)
* [<span data-ttu-id="eae4f-219">Etablera en virtuell StorSimple-matris på VMware</span><span class="sxs-lookup"><span data-stu-id="eae4f-219">Provision a StorSimple Virtual Array in VMware</span></span>](storsimple-virtual-array-deploy2-provision-vmware.md)

