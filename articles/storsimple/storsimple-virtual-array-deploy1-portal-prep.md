---
title: "Portalen prep för virtuell StorSimple-matrisen | Microsoft Docs"
description: "Första självstudierna för att distribuera StorSimple virtuell matris innebär att förbereda Azure-portalen"
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
ms.openlocfilehash: 3d0801053721f98ce7a2b0fcbe3c65da8dbdd8d3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-storsimple-virtual-array---prepare-the-azure-portal"></a><span data-ttu-id="18381-103">Distribuera StorSimple virtuell matris – förbereda den Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="18381-103">Deploy StorSimple Virtual Array - Prepare the Azure portal</span></span>

![](./media/storsimple-virtual-array-deploy1-portal-prep/getstarted4.png)
## <a name="overview"></a><span data-ttu-id="18381-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="18381-104">Overview</span></span>

<span data-ttu-id="18381-105">Det här är den första artikeln i serien i kurserna om distribution krävs för att distribuera helt virtuella matrisen som en filserver eller en iSCSI-server med hjälp av Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="18381-105">This is the first article in the series of deployment tutorials required to completely deploy your virtual array as a file server or an iSCSI server using the Resource Manager model.</span></span> <span data-ttu-id="18381-106">Den här artikeln beskriver förberedelser krävs för att skapa och konfigurera Enhetshanteraren för StorSimple-tjänsten innan du etablerar en virtuell matris.</span><span class="sxs-lookup"><span data-stu-id="18381-106">This article describes the preparation required to create and configure your StorSimple Device Manager service prior to provisioning a virtual array.</span></span> <span data-ttu-id="18381-107">Den här artikeln innehåller också länkar i en checklista för distributionskonfiguration och konfiguration krav.</span><span class="sxs-lookup"><span data-stu-id="18381-107">This article also links out to a deployment configuration checklist and configuration prerequisites.</span></span>

<span data-ttu-id="18381-108">Du måste ha administratörsbehörighet för att utföra installationen och konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="18381-108">You need administrator privileges to complete the setup and configuration process.</span></span> <span data-ttu-id="18381-109">Vi rekommenderar att du läser checklistan för distributionskonfiguration innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="18381-109">We recommend that you review the deployment configuration checklist before you begin.</span></span> <span data-ttu-id="18381-110">Portalen förberedelse tar mindre än 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="18381-110">The portal preparation takes less than 10 minutes.</span></span>

<span data-ttu-id="18381-111">Den information som publiceras i den här artikeln gäller för distribution av virtuella StorSimple-matriser i Azure portal och Microsoft Azure Government-molnet.</span><span class="sxs-lookup"><span data-stu-id="18381-111">The information published in this article applies to the deployment of StorSimple Virtual Arrays in the Azure portal and Microsoft Azure Government Cloud.</span></span>

### <a name="get-started"></a><span data-ttu-id="18381-112">Kom igång</span><span class="sxs-lookup"><span data-stu-id="18381-112">Get started</span></span>
<span data-ttu-id="18381-113">Arbetsflöde för distribution består av förbereda portalen, etablera en virtuell matris i din virtualiserade miljö och slutföra installationen.</span><span class="sxs-lookup"><span data-stu-id="18381-113">The deployment workflow consists of preparing the portal, provisioning a virtual array in your virtualized environment, and completing the setup.</span></span> <span data-ttu-id="18381-114">Om du vill komma igång med virtuella StorSimple-matris-distribution som en filserver eller en iSCSI-server, måste du referera till följande tabellform resurser.</span><span class="sxs-lookup"><span data-stu-id="18381-114">To get started with the StorSimple Virtual Array deployment as a file server or an iSCSI server, you need to refer to the following tabulated resources.</span></span>

#### <a name="deployment-articles"></a><span data-ttu-id="18381-115">Artiklar om distribution</span><span class="sxs-lookup"><span data-stu-id="18381-115">Deployment articles</span></span>

<span data-ttu-id="18381-116">För att distribuera din virtuella StorSimple-matris, finns i följande artiklar i föreskrivna sekvensen.</span><span class="sxs-lookup"><span data-stu-id="18381-116">To deploy your StorSimple Virtual Array, refer to the following articles in the prescribed sequence.</span></span>

| **#** | <span data-ttu-id="18381-117">**I det här steget**</span><span class="sxs-lookup"><span data-stu-id="18381-117">**In this step**</span></span> | <span data-ttu-id="18381-118">**Det gör du...**</span><span class="sxs-lookup"><span data-stu-id="18381-118">**You do this …**</span></span> | <span data-ttu-id="18381-119">**Och använda dessa dokument.**</span><span class="sxs-lookup"><span data-stu-id="18381-119">**And use these documents.**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="18381-120">1.</span><span class="sxs-lookup"><span data-stu-id="18381-120">1.</span></span> |<span data-ttu-id="18381-121">**Konfigurera Azure-portalen**</span><span class="sxs-lookup"><span data-stu-id="18381-121">**Set up the Azure portal**</span></span> |<span data-ttu-id="18381-122">Skapa och konfigurera din StorSimple Device Manager-tjänsten innan du etablerar en virtuell StorSimple-matris.</span><span class="sxs-lookup"><span data-stu-id="18381-122">Create and configure your StorSimple Device Manager service prior to provisioning a StorSimple Virtual Array.</span></span> |[<span data-ttu-id="18381-123">Förbereda portalen</span><span class="sxs-lookup"><span data-stu-id="18381-123">Prepare the portal</span></span>](storsimple-virtual-array-deploy1-portal-prep.md) |
| <span data-ttu-id="18381-124">2.</span><span class="sxs-lookup"><span data-stu-id="18381-124">2.</span></span> |<span data-ttu-id="18381-125">**Etablera virtuella matrisen**</span><span class="sxs-lookup"><span data-stu-id="18381-125">**Provision the Virtual Array**</span></span> |<span data-ttu-id="18381-126">Etablera för Hyper-V och ansluta till en virtuell StorSimple-matris på ett värdsystem som kör Hyper-V på Windows Server 2012 R2, Windows Server 2012 eller Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="18381-126">For Hyper-V, provision and connect to a StorSimple Virtual Array on a host system running Hyper-V on Windows Server 2012 R2, Windows Server 2012, or Windows Server 2008 R2.</span></span> <br></br> <br></br> <span data-ttu-id="18381-127">För VMware kan etablera och ansluta till en StorSimple virtuell matrisen på ett värdsystem som kör VMware ESXi 5.5 och senare.</span><span class="sxs-lookup"><span data-stu-id="18381-127">For VMware, provision and connect to a StorSimple Virtual Array on a host system running VMware ESXi 5.5 and above.</span></span><br></br> |[<span data-ttu-id="18381-128">Etablera en virtuell matris i Hyper-V</span><span class="sxs-lookup"><span data-stu-id="18381-128">Provision a virtual array in Hyper-V</span></span>](storsimple-virtual-array-deploy2-provision-hyperv.md) <br></br> <br></br> [<span data-ttu-id="18381-129">Etablera en virtuell VMware-matris</span><span class="sxs-lookup"><span data-stu-id="18381-129">Provision a virtual array in VMware</span></span>](storsimple-virtual-array-deploy2-provision-vmware.md) |
| <span data-ttu-id="18381-130">3.</span><span class="sxs-lookup"><span data-stu-id="18381-130">3.</span></span> |<span data-ttu-id="18381-131">**Konfigurera virtuella matrisen**</span><span class="sxs-lookup"><span data-stu-id="18381-131">**Set up the Virtual Array**</span></span> |<span data-ttu-id="18381-132">Utföra installationen för din filserver, registrera din StorSimple-filserver och slutför installationen av enheten.</span><span class="sxs-lookup"><span data-stu-id="18381-132">For your file server, perform initial setup, register your StorSimple file server, and complete the device setup.</span></span> <span data-ttu-id="18381-133">Du kan sedan etablera SMB-resurser.</span><span class="sxs-lookup"><span data-stu-id="18381-133">You can then provision SMB shares.</span></span> <br></br> <br></br> <span data-ttu-id="18381-134">Utföra installationen för iSCSI-server, registrera StorSimple iSCSI-servern och slutför installationen av enheten.</span><span class="sxs-lookup"><span data-stu-id="18381-134">For your iSCSI server, perform initial setup, register your StorSimple iSCSI server, and complete the device setup.</span></span> <span data-ttu-id="18381-135">Du kan sedan etablera iSCSI-volymer.</span><span class="sxs-lookup"><span data-stu-id="18381-135">You can then provision iSCSI volumes.</span></span> |[<span data-ttu-id="18381-136">Konfigurera virtuella matris som filserver</span><span class="sxs-lookup"><span data-stu-id="18381-136">Set up virtual array as file server</span></span>](storsimple-virtual-array-deploy3-fs-setup.md)<br></br> <br></br>[<span data-ttu-id="18381-137">Konfigurera virtuella matris som iSCSI-server</span><span class="sxs-lookup"><span data-stu-id="18381-137">Set up virtual array as iSCSI server</span></span>](storsimple-virtual-array-deploy3-iscsi-setup.md) |

<span data-ttu-id="18381-138">Du kan nu börja ställa in Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="18381-138">You can now begin to set up the Azure portal.</span></span>

## <a name="configuration-checklist"></a><span data-ttu-id="18381-139">Checklista för konfiguration</span><span class="sxs-lookup"><span data-stu-id="18381-139">Configuration checklist</span></span>

<span data-ttu-id="18381-140">Checklista för konfiguration beskrivs den information som du behöver samla in innan du konfigurerar programvaran på din virtuella StorSimple-matrisen.</span><span class="sxs-lookup"><span data-stu-id="18381-140">The configuration checklist describes the information that you need to collect before you configure the software on your StorSimple Virtual Array.</span></span> <span data-ttu-id="18381-141">Förbereder den här informationen i förväg hjälper till att förenkla processen för att distribuera StorSimple-enheten i din miljö.</span><span class="sxs-lookup"><span data-stu-id="18381-141">Preparing this information ahead of time helps streamline the process of deploying the StorSimple device in your environment.</span></span> <span data-ttu-id="18381-142">Beroende på om din virtuella StorSimple-matris distribueras som en filserver eller en iSCSI-server, behöver du något av följande checklistor.</span><span class="sxs-lookup"><span data-stu-id="18381-142">Depending upon whether your StorSimple Virtual Array is deployed as a file server or an iSCSI server, you need one of the following checklists.</span></span>

* <span data-ttu-id="18381-143">Hämta den [checklista för konfiguration av StorSimple virtuella matris, File Server](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).</span><span class="sxs-lookup"><span data-stu-id="18381-143">Download the [StorSimple Virtual Array File Server Configuration Checklist](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).</span></span>
* <span data-ttu-id="18381-144">Hämta den [virtuella StorSimple-matris iSCSI checklista för konfiguration av servern](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).</span><span class="sxs-lookup"><span data-stu-id="18381-144">Download the [StorSimple Virtual Array iSCSI Server Configuration Checklist](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="18381-145">Krav</span><span class="sxs-lookup"><span data-stu-id="18381-145">Prerequisites</span></span>

<span data-ttu-id="18381-146">Här hittar du konfigurationskraven för din StorSimple Device Manager-tjänst, din virtuella StorSimple-matris och datacenternätverket.</span><span class="sxs-lookup"><span data-stu-id="18381-146">Here you find the configuration prerequisites for your StorSimple Device Manager service, your StorSimple Virtual Array, and the datacenter network.</span></span>

### <a name="for-the-storsimple-device-manager-service"></a><span data-ttu-id="18381-147">För StorSimple Device Manager-tjänsten</span><span class="sxs-lookup"><span data-stu-id="18381-147">For the StorSimple Device Manager service</span></span>

<span data-ttu-id="18381-148">Innan du börjar bör du kontrollera att:</span><span class="sxs-lookup"><span data-stu-id="18381-148">Before you begin, make sure that:</span></span>

* <span data-ttu-id="18381-149">Du har ditt Microsoft-konto med autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="18381-149">You have your Microsoft account with access credentials.</span></span>
* <span data-ttu-id="18381-150">Du har ditt Microsoft Azure lagringskonto med autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="18381-150">You have your Microsoft Azure storage account with access credentials.</span></span>
* <span data-ttu-id="18381-151">Microsoft Azure-prenumerationen ska aktiveras för StorSimple enheten Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="18381-151">Your Microsoft Azure subscription should be enabled for StorSimple Device Manager service.</span></span>

### <a name="for-the-storsimple-virtual-array"></a><span data-ttu-id="18381-152">För den virtuella StorSimple-matrisen</span><span class="sxs-lookup"><span data-stu-id="18381-152">For the StorSimple Virtual Array</span></span>

<span data-ttu-id="18381-153">Innan du distribuerar en virtuell matris, kontrollerar du att:</span><span class="sxs-lookup"><span data-stu-id="18381-153">Before you deploy a virtual array, make sure that:</span></span>

* <span data-ttu-id="18381-154">Du har åtkomst till en värdsystem som kör Hyper-V på Windows Server 2008 R2 eller senare eller VMware (ESXi 5.5 eller senare) som kan användas för att en etablera en enhet.</span><span class="sxs-lookup"><span data-stu-id="18381-154">You have access to a host system running Hyper-V on Windows Server 2008 R2 or later or VMware (ESXi 5.5 or later) that can be used to a provision a device.</span></span>
* <span data-ttu-id="18381-155">Värddatorn är att dedikera följande resurser för att etablera virtuella matrisen:</span><span class="sxs-lookup"><span data-stu-id="18381-155">The host system is able to dedicate the following resources to provision your virtual array:</span></span>
  
  * <span data-ttu-id="18381-156">Minst 4 kärnor.</span><span class="sxs-lookup"><span data-stu-id="18381-156">A minimum of 4 cores.</span></span>
  * <span data-ttu-id="18381-157">Minst 8 GB RAM-minne.</span><span class="sxs-lookup"><span data-stu-id="18381-157">At least 8 GB of RAM.</span></span> <span data-ttu-id="18381-158">Om du planerar att konfigurera virtuella matrisen som filserver stöder 8 GB 2 miljoner filer.</span><span class="sxs-lookup"><span data-stu-id="18381-158">If you plan to configure the virtual array as file server, 8 GB supports 2 million files.</span></span> <span data-ttu-id="18381-159">Behöver du 16 GB RAM-MINNET 2-4 miljoner stödfiler.</span><span class="sxs-lookup"><span data-stu-id="18381-159">You need 16 GB RAM to support 2 - 4 million files.</span></span>
  * <span data-ttu-id="18381-160">Ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="18381-160">One network interface.</span></span>
  * <span data-ttu-id="18381-161">En virtuell disk 500 GB efter systemdata.</span><span class="sxs-lookup"><span data-stu-id="18381-161">A 500 GB virtual disk for system data.</span></span>

### <a name="for-the-datacenter-network"></a><span data-ttu-id="18381-162">För datacenternätverket</span><span class="sxs-lookup"><span data-stu-id="18381-162">For the datacenter network</span></span>

<span data-ttu-id="18381-163">Innan du börjar bör du kontrollera att:</span><span class="sxs-lookup"><span data-stu-id="18381-163">Before you begin, make sure that:</span></span>

* <span data-ttu-id="18381-164">Nätverket i datacentret har konfigurerats enligt nätverkskraven för din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="18381-164">The network in your datacenter is configured as per the networking requirements for your StorSimple device.</span></span> <span data-ttu-id="18381-165">Mer information finns i [StorSimple virtuell matris systemkrav](storsimple-ova-system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="18381-165">For more information, see the [StorSimple Virtual Array System Requirements](storsimple-ova-system-requirements.md).</span></span>
* <span data-ttu-id="18381-166">Din virtuella StorSimple-matris har en dedikerad 5 Mbit/s internetbandbredd (eller mer) vid alla tidpunkter.</span><span class="sxs-lookup"><span data-stu-id="18381-166">Your StorSimple Virtual Array has a dedicated 5 Mbps Internet bandwidth (or more) available at all times.</span></span> <span data-ttu-id="18381-167">Den här bandbredden ska inte delas med andra program.</span><span class="sxs-lookup"><span data-stu-id="18381-167">This bandwidth should not be shared with any other applications.</span></span>

## <a name="step-by-step-preparation"></a><span data-ttu-id="18381-168">Förberedelse av steg</span><span class="sxs-lookup"><span data-stu-id="18381-168">Step-by-step preparation</span></span>

<span data-ttu-id="18381-169">Använd följande steg för steg-instruktioner för att förbereda din portal för StorSimple Device Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="18381-169">Use the following step-by-step instructions to prepare your portal for the StorSimple Device Manager service.</span></span>

## <a name="step-1-create-a-new-service"></a><span data-ttu-id="18381-170">Steg 1: Skapa en ny tjänst</span><span class="sxs-lookup"><span data-stu-id="18381-170">Step 1: Create a new service</span></span>

<span data-ttu-id="18381-171">En instans av StorSimple enheten Manager-tjänsten kan hantera flera virtuella StorSimple-matriser.</span><span class="sxs-lookup"><span data-stu-id="18381-171">A single instance of the StorSimple Device Manager service can manage multiple StorSimple Virtual Arrays.</span></span> <span data-ttu-id="18381-172">Skapa en instans av StorSimple Device Manager-tjänsten genom att utföra stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="18381-172">Perform the following steps to create an instance of the StorSimple Device Manager service.</span></span> <span data-ttu-id="18381-173">Om du har en befintlig StorSimple Device Manager-tjänst för att hantera dina virtuella matriser, hoppa över det här steget och gå till [steg 2: Hämta nyckel för tjänstregistrering](#step-2-get-the-service-registration-key).</span><span class="sxs-lookup"><span data-stu-id="18381-173">If you have an existing StorSimple Device Manager service to manage your virtual arrays, skip this step and go to [Step 2: Get the service registration key](#step-2-get-the-service-registration-key).</span></span>

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

> [!IMPORTANT]
> <span data-ttu-id="18381-174">Om du inte har aktiverat automatiskt skapande av lagringskonton med din tjänst måste du skapa minst ett lagringskonto efter att du har skapat en tjänst.</span><span class="sxs-lookup"><span data-stu-id="18381-174">If you did not enable the automatic creation of a storage account with your service, you will need to create at least one storage account after you have successfully created a service.</span></span>
> 
> * <span data-ttu-id="18381-175">Om du inte har skapat ett lagringskonto automatiskt går du till [Konfigurera ett nytt lagringskonto för tjänsten](#optional-step-configure-a-new-storage-account-for-the-service) för detaljerade anvisningar.</span><span class="sxs-lookup"><span data-stu-id="18381-175">If you did not create a storage account automatically, go to [Configure a new storage account for the service](#optional-step-configure-a-new-storage-account-for-the-service) for detailed instructions.</span></span>
> * <span data-ttu-id="18381-176">Om du har aktiverat automatiskt skapande av ett lagringskonto går du till [steg 2: hämta nyckel för tjänstregistrering](#step-2-get-the-service-registration-key).</span><span class="sxs-lookup"><span data-stu-id="18381-176">If you enabled the automatic creation of a storage account, go to [Step 2: Get the service registration key](#step-2-get-the-service-registration-key).</span></span>
> 
> 

## <a name="step-2-get-the-service-registration-key"></a><span data-ttu-id="18381-177">Steg 2: Hämta nyckel för tjänstregistrering</span><span class="sxs-lookup"><span data-stu-id="18381-177">Step 2: Get the service registration key</span></span>

<span data-ttu-id="18381-178">När StorSimple Device Manager-tjänsten är igång måste du hämta tjänstregistreringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="18381-178">After the StorSimple Device Manager service is up and running, you will need to get the service registration key.</span></span> <span data-ttu-id="18381-179">Den här nyckeln används för att registrera och ansluta din StorSimple-enhet till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="18381-179">This key is used to register and connect your StorSimple device with the service.</span></span>

<span data-ttu-id="18381-180">Utför följande steg i den [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="18381-180">Perform the following steps in the [Azure portal](https://portal.azure.com/).</span></span>

[!INCLUDE [storsimple-virtual-array-get-service-registration-key](../../includes/storsimple-virtual-array-get-service-registration-key.md)]

> [!NOTE]
> <span data-ttu-id="18381-181">Nyckeln för tjänstregistrering används för att registrera alla Enhetshanteraren för StorSimple-enheter som behöver registreras med din StorSimple Device Manager-tjänst.</span><span class="sxs-lookup"><span data-stu-id="18381-181">The service registration key is used to register all the StorSimple Device Manager devices that need to register with your StorSimple Device Manager service.</span></span>
> 
> 

## <a name="step-3-download-the-virtual-array-image"></a><span data-ttu-id="18381-182">Steg 3: Ladda ned avbildningen virtuella matris</span><span class="sxs-lookup"><span data-stu-id="18381-182">Step 3: Download the virtual array image</span></span>

<span data-ttu-id="18381-183">När du har nyckeln för tjänstregistrering, behöver du hämta lämplig virtuell matris-avbildning för att etablera en virtuell matrisen på värdsystemet.</span><span class="sxs-lookup"><span data-stu-id="18381-183">After you have the service registration key, you will need to download the appropriate virtual array image to provision a virtual array on your host system.</span></span> <span data-ttu-id="18381-184">Virtuella matris-avbildningar är specifika för operativsystemet och kan hämtas från sidan Snabbstart i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="18381-184">The virtual array images are operating system specific and can be downloaded from the Quick Start page in the Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="18381-185">Programmet som körs på den virtuella StorSimple-matrisen får endast användas med Enhetshanteraren för StorSimple-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="18381-185">The software running on the StorSimple Virtual Array may only be used with the StorSimple Device Manager service.</span></span>
> 
> 

<span data-ttu-id="18381-186">Utför följande steg i den [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="18381-186">Perform the following steps in the [Azure portal](https://portal.azure.com/).</span></span>

#### <a name="to-get-the-virtual-array-image"></a><span data-ttu-id="18381-187">Den virtuella matris-bild</span><span class="sxs-lookup"><span data-stu-id="18381-187">To get the virtual array image</span></span>

1. <span data-ttu-id="18381-188">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="18381-188">Sign into the [Azure portal](https://portal.azure.com/).</span></span> 
2. <span data-ttu-id="18381-189">I Azure-portalen klickar du på **Bläddra > StorSimple enhetshanterare**.</span><span class="sxs-lookup"><span data-stu-id="18381-189">In the Azure portal, click **Browse > StorSimple Device Managers**.</span></span>
3. <span data-ttu-id="18381-190">Välj en befintlig StorSimple Device Manager-tjänst.</span><span class="sxs-lookup"><span data-stu-id="18381-190">Select an existing StorSimple Device Manager service.</span></span> <span data-ttu-id="18381-191">I den **StorSimple Enhetshanteraren** bladet, klickar du på **Snabbstart**.</span><span class="sxs-lookup"><span data-stu-id="18381-191">In the **StorSimple Device Manager** blade, click **Quick Start**.</span></span> 
4. <span data-ttu-id="18381-192">Klicka på länken som motsvarar den bild som du vill hämta från Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="18381-192">Click the link corresponding to the image that you want to download from the Microsoft Download Center.</span></span> <span data-ttu-id="18381-193">Bildfilerna är ungefär 4,8 GB.</span><span class="sxs-lookup"><span data-stu-id="18381-193">The image files are approximately 4.8 GB.</span></span>
   
   * <span data-ttu-id="18381-194">VHDX för Hyper-V på Windows Server 2012 och senare</span><span class="sxs-lookup"><span data-stu-id="18381-194">VHDX for Hyper-V on Windows Server 2012 and later</span></span>
   * <span data-ttu-id="18381-195">VHD för Hyper-V på Windows Server 2008 R2 och senare</span><span class="sxs-lookup"><span data-stu-id="18381-195">VHD for Hyper-V on Windows Server 2008 R2 and later</span></span>
   * <span data-ttu-id="18381-196">VMDK för VMWare ESXi 5.5 och senare</span><span class="sxs-lookup"><span data-stu-id="18381-196">VMDK for VMWare ESXi 5.5 and later</span></span>
5. <span data-ttu-id="18381-197">Hämta och packa upp filen till en lokal enhet, och ett meddelande om där den uppackade filen finns.</span><span class="sxs-lookup"><span data-stu-id="18381-197">Download and unzip the file to a local drive, making a note of where the unzipped file is located.</span></span>

## <a name="optional-step-configure-a-new-storage-account-for-the-service"></a><span data-ttu-id="18381-198">Valfritt steg: konfigurera ett nytt lagringskonto för tjänsten</span><span class="sxs-lookup"><span data-stu-id="18381-198">Optional step: Configure a new storage account for the service</span></span>

<span data-ttu-id="18381-199">Det här steget är valfritt och bör bara genomföras om du inte har aktiverat automatiskt skapande av lagringskonton med din tjänst.</span><span class="sxs-lookup"><span data-stu-id="18381-199">This step is optional and should be performed only if you did not enable the automatic creation of a storage account with your service.</span></span>

<span data-ttu-id="18381-200">Om du behöver skapa ett Azure storage-konto i en annan region finns [hur du skapar ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) stegvisa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="18381-200">If you need to create an Azure storage account in a different region, see [How to create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) for step-by-step instructions.</span></span>

<span data-ttu-id="18381-201">Utför följande steg i den [Azure-portalen](https://ms.portal.azure.com/) på tjänstsidan StorSimple Enhetshanteraren att lägga till ett befintligt Microsoft Azure-lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="18381-201">Perform the following steps in the [Azure portal](https://ms.portal.azure.com/) on the StorSimple Device Manager service page to add an existing Microsoft Azure storage account.</span></span>

#### <a name="to-add-a-storage-account-credential-that-has-the-same-azure-subscription-as-the-device-manager-service"></a><span data-ttu-id="18381-202">Om du vill lägga till en autentiseringsuppgift för storage-konto har som samma Azure-prenumerationen som tjänsten Device Manager</span><span class="sxs-lookup"><span data-stu-id="18381-202">To add a storage account credential that has the same Azure subscription as the Device Manager service</span></span>

1. <span data-ttu-id="18381-203">Gå till Enhetshanteraren tjänsten, väljer och dubbelklicka på den.</span><span class="sxs-lookup"><span data-stu-id="18381-203">Navigate to your Device Manager service, select and double-click it.</span></span> <span data-ttu-id="18381-204">Då öppnas den **översikt** bladet.</span><span class="sxs-lookup"><span data-stu-id="18381-204">This opens the **Overview** blade.</span></span>
2. <span data-ttu-id="18381-205">Välj **lagringskontouppgifter** inom den **Configuration** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="18381-205">Select **Storage account credentials** within the **Configuration** section.</span></span>
3. <span data-ttu-id="18381-206">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="18381-206">Click **Add**.</span></span>
4. <span data-ttu-id="18381-207">I den **Lägg till ett lagringskonto** bladet gör du följande:</span><span class="sxs-lookup"><span data-stu-id="18381-207">In the **Add a storage account** blade, do the following:</span></span>
   
    1. <span data-ttu-id="18381-208">För **prenumeration**väljer **aktuella**.</span><span class="sxs-lookup"><span data-stu-id="18381-208">For **Subscription**, select **Current**.</span></span>
   
    2. <span data-ttu-id="18381-209">Ange namnet på ditt Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="18381-209">Provide the name of your Azure storage account.</span></span>
   
    3. <span data-ttu-id="18381-210">Välj **aktivera** att skapa en säker kanal för nätverkskommunikation mellan din StorSimple-enhet och molnet.</span><span class="sxs-lookup"><span data-stu-id="18381-210">Select **Enable** to create a secure channel for network communication between your StorSimple device and the cloud.</span></span> <span data-ttu-id="18381-211">Välj **inaktivera** endast om du arbetar inom ett privat moln.</span><span class="sxs-lookup"><span data-stu-id="18381-211">Select **Disable** only if you are operating within a private cloud.</span></span>
   
    4. <span data-ttu-id="18381-212">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="18381-212">Click **Add**.</span></span> <span data-ttu-id="18381-213">Du meddelas när lagringskontot har skapats.</span><span class="sxs-lookup"><span data-stu-id="18381-213">You are notified after the storage account is successfully created.</span></span><br></br>
   
     ![Lägg till en befintlig lagring konto autentiseringsuppgift](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-storageacct.png)

## <a name="next-step"></a><span data-ttu-id="18381-215">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="18381-215">Next step</span></span>

<span data-ttu-id="18381-216">Nästa steg är att etablera en virtuell dator för din virtuella StorSimple-matrisen.</span><span class="sxs-lookup"><span data-stu-id="18381-216">The next step is to provision a virtual machine for your StorSimple Virtual Array.</span></span> <span data-ttu-id="18381-217">Beroende på din värdoperativsystemet detaljerade instruktioner finns i:</span><span class="sxs-lookup"><span data-stu-id="18381-217">Depending on your host operating system, see the detailed instructions in:</span></span>

* [<span data-ttu-id="18381-218">Etablera en virtuell StorSimple-matris i Hyper-V</span><span class="sxs-lookup"><span data-stu-id="18381-218">Provision a StorSimple Virtual Array in Hyper-V</span></span>](storsimple-virtual-array-deploy2-provision-hyperv.md)
* [<span data-ttu-id="18381-219">Etablera en virtuell StorSimple-matris på VMware</span><span class="sxs-lookup"><span data-stu-id="18381-219">Provision a StorSimple Virtual Array in VMware</span></span>](storsimple-virtual-array-deploy2-provision-vmware.md)

