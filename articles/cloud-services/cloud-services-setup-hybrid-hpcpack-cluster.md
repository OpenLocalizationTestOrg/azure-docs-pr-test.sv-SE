---
title: aaaSet upp en hybrid HPC Pack kluster i Azure | Microsoft Docs
description: "Lär dig hur toouse Microsoft HPC Pack och Azure tooset upp en liten, hybrid högpresterande databehandling HPC-kluster"
services: cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: hpc-pack
ms.assetid: 
ms.service: cloud-services
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: danlep
ms.openlocfilehash: 5ad30d78dcd0c6a95d2a61f25015232edab3563c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-hybrid-high-performance-computing-hpc-cluster-with-microsoft-hpc-pack-and-on-demand-azure-compute-nodes"></a><span data-ttu-id="48e0c-103">Skapa en hybrid med höga prestanda HPC-kluster med Microsoft HPC Pack och på begäran Azure compute-noder</span><span class="sxs-lookup"><span data-stu-id="48e0c-103">Set up a hybrid high performance computing (HPC) cluster with Microsoft HPC Pack and on-demand Azure compute nodes</span></span>
<span data-ttu-id="48e0c-104">Använd Microsoft HPC Pack 2012 R2 och Azure tooset upp en liten, hybrid med höga prestanda HPC-kluster.</span><span class="sxs-lookup"><span data-stu-id="48e0c-104">Use Microsoft HPC Pack 2012 R2 and Azure tooset up a small, hybrid high performance computing (HPC) cluster.</span></span> <span data-ttu-id="48e0c-105">hello klustret visas i den här artikeln består av en lokal HPC Pack huvudnod och vissa compute-noder som du distribuerar på begäran i ett Azure-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="48e0c-105">hello cluster shown in this article consists of an on-premises HPC Pack head node and some compute nodes you deploy on-demand in an Azure cloud service.</span></span> <span data-ttu-id="48e0c-106">Du kan sedan köra beräkning jobb på hello hybrid klustret.</span><span class="sxs-lookup"><span data-stu-id="48e0c-106">You can then run compute jobs on hello hybrid cluster.</span></span>

![Hybrid HPC-kluster][Overview] 

<span data-ttu-id="48e0c-108">Detta kallas ibland kursen visar en metod klustret ”burst toohello moln”, toouse skalbar, på begäran Azure-resurser toorun beräknings-intensiva program.</span><span class="sxs-lookup"><span data-stu-id="48e0c-108">This tutorial shows one approach, sometimes called cluster "burst toohello cloud," toouse scalable, on-demand Azure resources toorun compute-intensive applications.</span></span>

<span data-ttu-id="48e0c-109">Den här kursen förutsätter några tidigare erfarenheter med beräkningskluster eller HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="48e0c-109">This tutorial assumes no prior experience with compute clusters or HPC Pack.</span></span> <span data-ttu-id="48e0c-110">Det är avsedd endast toohelp som du distribuerar ett hybrid beräkningskluster snabbt i exempelsyfte.</span><span class="sxs-lookup"><span data-stu-id="48e0c-110">It is intended only toohelp you deploy a hybrid compute cluster quickly for demonstration purposes.</span></span> <span data-ttu-id="48e0c-111">Mer information och steg toodeploy hybrid HPC Pack kluster i större skala i en produktionsmiljö eller toouse HPC Pack 2016 finns hello [detaljerad vägledning](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span><span class="sxs-lookup"><span data-stu-id="48e0c-111">For considerations and steps toodeploy a hybrid HPC Pack cluster at greater scale in a production environment, or toouse HPC Pack 2016, see hello [detailed guidance](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span></span> <span data-ttu-id="48e0c-112">För andra scenarier med HPC Pack, inklusive automatisk Klusterdistribution i virtuella Azure-datorer, se [HPC-klusteralternativ with Microsoft HPC Pack i Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="48e0c-112">For other scenarios with HPC Pack, including automated cluster deployment in Azure virtual machines, see [HPC cluster options with Microsoft HPC Pack in Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48e0c-113">Krav</span><span class="sxs-lookup"><span data-stu-id="48e0c-113">Prerequisites</span></span>
* <span data-ttu-id="48e0c-114">**Azure-prenumeration** -om du inte har en Azure-prenumeration kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/free/) på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="48e0c-114">**Azure subscription** - If you don't have an Azure subscription, you can create a [free account](https://azure.microsoft.com/free/) in just a couple of minutes.</span></span>
* <span data-ttu-id="48e0c-115">**En lokal dator som kör Windows Server 2012 R2 eller Windows Server 2012** -använder den här datorn som hello huvudnod hello HPC-kluster.</span><span class="sxs-lookup"><span data-stu-id="48e0c-115">**An on-premises computer running Windows Server 2012 R2 or Windows Server 2012** - Use this computer as hello head node of hello HPC cluster.</span></span> <span data-ttu-id="48e0c-116">Om du inte redan kör Windows Server, kan du hämta och installera en [utvärderingsversion](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2).</span><span class="sxs-lookup"><span data-stu-id="48e0c-116">If you aren't already running Windows Server, you can download and install an [evaluation version](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2).</span></span>
  
  * <span data-ttu-id="48e0c-117">hello datorn vara domänansluten tooan Active Directory-domän.</span><span class="sxs-lookup"><span data-stu-id="48e0c-117">hello computer must be joined tooan Active Directory domain.</span></span> <span data-ttu-id="48e0c-118">Du kan konfigurera hello huvudnod dator för testning, som en domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="48e0c-118">For test purposes, you can configure hello head node computer as a domain controller.</span></span> <span data-ttu-id="48e0c-119">tooadd hello serverrollen Active Directory Domain Services och främja hello huvudnod datorn som en domänkontrollant, hello i dokumentationen för Windows Server.</span><span class="sxs-lookup"><span data-stu-id="48e0c-119">tooadd hello Active Directory Domain Services server role and promote hello head node computer as a domain controller, see hello documentation for Windows Server.</span></span>
  * <span data-ttu-id="48e0c-120">toosupport HPC Pack hello operativsystemet måste installeras i ett av dessa språk: engelska, japanska eller kinesiska (förenklad).</span><span class="sxs-lookup"><span data-stu-id="48e0c-120">toosupport HPC Pack, hello operating system must be installed in one of these languages: English, Japanese, or Chinese (Simplified).</span></span>
  * <span data-ttu-id="48e0c-121">Kontrollera att viktiga och viktiga uppdateringar har installerats.</span><span class="sxs-lookup"><span data-stu-id="48e0c-121">Verify that important and critical updates are installed.</span></span>
* <span data-ttu-id="48e0c-122">**HPC Pack 2012 R2** - [hämta](http://go.microsoft.com/fwlink/p/?linkid=328024) hello installationspaket för hello senaste versionen utan kostnad och kopiera hello filer toohello huvudnod datorn.</span><span class="sxs-lookup"><span data-stu-id="48e0c-122">**HPC Pack 2012 R2** - [Download](http://go.microsoft.com/fwlink/p/?linkid=328024) hello installation package for hello latest version free of charge and copy hello files toohello head node computer.</span></span> <span data-ttu-id="48e0c-123">Välj installationsfilerna i hello samma språk som din installation av Windows Server.</span><span class="sxs-lookup"><span data-stu-id="48e0c-123">Choose installation files in hello same language as your installation of Windows Server.</span></span>

    >[!NOTE]
    > <span data-ttu-id="48e0c-124">Om du vill toouse HPC Pack 2016 i stället för HPC Pack 2012 R2, krävs ytterligare konfiguration.</span><span class="sxs-lookup"><span data-stu-id="48e0c-124">If you want toouse HPC Pack 2016 instead of HPC Pack 2012 R2, additional configuration is needed.</span></span> <span data-ttu-id="48e0c-125">Se hello [detaljerad vägledning](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span><span class="sxs-lookup"><span data-stu-id="48e0c-125">See hello [detailed guidance](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span></span>
    > 
* <span data-ttu-id="48e0c-126">**Domänkonto** -kontot måste vara konfigurerad med behörighet som lokal administratör på hello huvudnod tooinstall HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="48e0c-126">**Domain account** - This account must be configured with local Administrator permissions on hello head node tooinstall HPC Pack.</span></span>
* <span data-ttu-id="48e0c-127">**TCP-anslutningar på port 443** från hello huvudnod tooAzure.</span><span class="sxs-lookup"><span data-stu-id="48e0c-127">**TCP connectivity on port 443** from hello head node tooAzure.</span></span>

## <a name="install-hpc-pack-on-hello-head-node"></a><span data-ttu-id="48e0c-128">Installera HPC Pack på hello huvudnod</span><span class="sxs-lookup"><span data-stu-id="48e0c-128">Install HPC Pack on hello head node</span></span>
<span data-ttu-id="48e0c-129">Du först installera Microsoft HPC Pack på din lokala dator som kör Windows Server.</span><span class="sxs-lookup"><span data-stu-id="48e0c-129">You first install Microsoft HPC Pack on your on-premises computer running Windows Server.</span></span> <span data-ttu-id="48e0c-130">Den här datorn blir hello huvudnod hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="48e0c-130">This computer becomes hello head node of hello cluster.</span></span>

1. <span data-ttu-id="48e0c-131">Logga in toohello huvudnod genom att använda ett domänkonto som har lokal administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="48e0c-131">Log on toohello head node by using a domain account that has local Administrator permissions.</span></span>

2. <span data-ttu-id="48e0c-132">Starta hello HPC Pack installationsguiden genom att köra Setup.exe från hello HPC Pack installationsfiler.</span><span class="sxs-lookup"><span data-stu-id="48e0c-132">Start hello HPC Pack Installation Wizard by running Setup.exe from hello HPC Pack installation files.</span></span>

3. <span data-ttu-id="48e0c-133">På hello **installation av HPC Pack 2012 R2** klickar du på **ny installation eller Lägg till nya funktioner tooan befintlig installation**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-133">On hello **HPC Pack 2012 R2 Setup** screen, click **New installation or add new features tooan existing installation**.</span></span>

    ![HPC Pack 2012 installationen][install_hpc1]

4. <span data-ttu-id="48e0c-135">På hello **användaravtal för Microsoft-programvara sidan**, klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-135">On hello **Microsoft Software User Agreement page**, click **Next**.</span></span>

5. <span data-ttu-id="48e0c-136">På hello **Välj installationstyp** klickar du på **skapar ett nytt HPC-kluster genom att skapa en huvudnod**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-136">On hello **Select Installation Type** page, click **Create a new HPC cluster by creating a head node**, and then click **Next**.</span></span>

6. <span data-ttu-id="48e0c-137">hello guiden körs flera förinstallation tester.</span><span class="sxs-lookup"><span data-stu-id="48e0c-137">hello wizard runs several pre-installation tests.</span></span> <span data-ttu-id="48e0c-138">Klicka på **nästa** på hello **installationsregler** om alla tester godkänns.</span><span class="sxs-lookup"><span data-stu-id="48e0c-138">Click **Next** on hello **Installation Rules** page if all tests pass.</span></span> <span data-ttu-id="48e0c-139">Annars granskar du informationen hello och göra ändringar i din miljö.</span><span class="sxs-lookup"><span data-stu-id="48e0c-139">Otherwise, review hello information provided and make any necessary changes in your environment.</span></span> <span data-ttu-id="48e0c-140">Kör sedan hello tester igen eller om nödvändigt start hello installationsguiden igen.</span><span class="sxs-lookup"><span data-stu-id="48e0c-140">Then run hello tests again or if necessary start hello Installation Wizard again.</span></span>
7. <span data-ttu-id="48e0c-141">På hello **HPC DB Configuration** Kontrollera **huvudnod** har valts för alla HPC-databaser och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-141">On hello **HPC DB Configuration** page, make sure **Head Node** is selected for all HPC databases, and then click **Next**.</span></span> 

    ![DB-konfiguration][install_hpc4]

8. <span data-ttu-id="48e0c-143">Standardinställningarna på hello återstående sidor i guiden hello.</span><span class="sxs-lookup"><span data-stu-id="48e0c-143">Accept default selections on hello remaining pages of hello wizard.</span></span> <span data-ttu-id="48e0c-144">På hello **installera nödvändiga komponenter** klickar du på **installera**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-144">On hello **Install Required Components** page, click **Install**.</span></span>
   
    ![Installera][install_hpc6]

9. <span data-ttu-id="48e0c-146">När hello installationen är klar, avmarkera **starta HPC Cluster Manager** och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-146">After hello installation completes, uncheck **Start HPC Cluster Manager** and then click **Finish**.</span></span> <span data-ttu-id="48e0c-147">(Du startar HPC Cluster Manager i ett senare steg.)</span><span class="sxs-lookup"><span data-stu-id="48e0c-147">(You start HPC Cluster Manager in a later step.)</span></span>
   
    ![Slutför][install_hpc7]

## <a name="prepare-hello-azure-subscription"></a><span data-ttu-id="48e0c-149">Förbereda hello Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="48e0c-149">Prepare hello Azure subscription</span></span>
<span data-ttu-id="48e0c-150">Utför följande steg i hello hello [Azure-portalen](https://portal.azure.com) med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="48e0c-150">Perform hello following steps in hello [Azure portal](https://portal.azure.com) with your Azure subscription.</span></span> <span data-ttu-id="48e0c-151">När du har slutfört de här stegen kan du distribuera Azure-noder från hello lokalt huvudnod.</span><span class="sxs-lookup"><span data-stu-id="48e0c-151">After completing these steps, you can deploy Azure nodes from hello on-premises head node.</span></span> 
  
  > [!NOTE]
  > <span data-ttu-id="48e0c-152">Även anteckna ditt Azure prenumerations-ID som du behöver senare.</span><span class="sxs-lookup"><span data-stu-id="48e0c-152">Also make a note of your Azure subscription ID, which you need later.</span></span> <span data-ttu-id="48e0c-153">Hitta hello-ID i **prenumerationer** hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="48e0c-153">Find hello ID in **Subscriptions** in hello portal.</span></span>
  > 

### <a name="upload-hello-default-management-certificate"></a><span data-ttu-id="48e0c-154">Överför hello standardhanteringscertifikat</span><span class="sxs-lookup"><span data-stu-id="48e0c-154">Upload hello default management certificate</span></span>
<span data-ttu-id="48e0c-155">HPC Pack installerar ett självsignerat certifikat på hello huvudnod, kallas hello standard Microsoft HPC Azure Management-certifikat som du kan ladda upp som ett Azure-hanteringscertifikat.</span><span class="sxs-lookup"><span data-stu-id="48e0c-155">HPC Pack installs a self-signed certificate on hello head node, called hello Default Microsoft HPC Azure Management certificate, that you can upload as an Azure management certificate.</span></span> <span data-ttu-id="48e0c-156">Det här certifikatet tillhandahålls för testning och proof of concept distributioner toosecure hello anslutning mellan hello huvudnod och Azure.</span><span class="sxs-lookup"><span data-stu-id="48e0c-156">This certificate is provided for testing and proof-of-concept deployments toosecure hello connection between hello head node and Azure.</span></span>

1. <span data-ttu-id="48e0c-157">Från hello huvudnod dator, logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="48e0c-157">From hello head node computer, sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="48e0c-158">Klicka på **prenumerationer** > *your_subscription_name*.</span><span class="sxs-lookup"><span data-stu-id="48e0c-158">Click **Subscriptions** > *your_subscription_name*.</span></span>

3. <span data-ttu-id="48e0c-159">Klicka på **hanteringscertifikat** > **överför**.4.</span><span class="sxs-lookup"><span data-stu-id="48e0c-159">Click **Management certificates** > **Upload**.4.</span></span> <span data-ttu-id="48e0c-160">Bläddra i hello huvudnod för hello filen C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer.</span><span class="sxs-lookup"><span data-stu-id="48e0c-160">Browse on hello head node for hello file C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer.</span></span> <span data-ttu-id="48e0c-161">Klicka på **överför**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-161">Then, click **Upload**.</span></span>

   
<span data-ttu-id="48e0c-162">Hej **standard HPC Azure Management** certifikatet visas i hello listan över hanteringscertifikat.</span><span class="sxs-lookup"><span data-stu-id="48e0c-162">hello **Default HPC Azure Management** certificate appears in hello list of management certificates.</span></span>

### <a name="create-an-azure-cloud-service"></a><span data-ttu-id="48e0c-163">Skapa en Azure-molntjänst</span><span class="sxs-lookup"><span data-stu-id="48e0c-163">Create an Azure cloud service</span></span>
> [!NOTE]
> <span data-ttu-id="48e0c-164">För bästa prestanda bör du skapa hello Molntjänsten och hello storage-konto (i ett senare steg) i hello samma geografiska region.</span><span class="sxs-lookup"><span data-stu-id="48e0c-164">For best performance, create hello cloud service and hello storage account (in a later step) in hello same geographic region.</span></span>
> 
> 

1. <span data-ttu-id="48e0c-165">I hello-portalen klickar du på **molntjänster (klassisk)** > **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-165">In hello portal, click **Cloud services (classic)** > **+Add**.</span></span>

2.  <span data-ttu-id="48e0c-166">Ange ett DNS-namn för hello service, välja en resursgrupp och en plats och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-166">Type a DNS name for hello service, choose a resource group and a location, and then click **Create**.</span></span>


### <a name="create-an-azure-storage-account"></a><span data-ttu-id="48e0c-167">Skapa ett Azure-lagringskonto</span><span class="sxs-lookup"><span data-stu-id="48e0c-167">Create an Azure storage account</span></span>
1. <span data-ttu-id="48e0c-168">I hello-portalen klickar du på **Storage-konton (klassiska)** > **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-168">In hello portal, click **Storage accounts (classic)** > **+Add**.</span></span>

2. <span data-ttu-id="48e0c-169">Ange ett namn för hello kontot och välj hello **klassiska** distributionsmodell.</span><span class="sxs-lookup"><span data-stu-id="48e0c-169">Type a name for hello account, and select hello **Classic** deployment model.</span></span>

3. <span data-ttu-id="48e0c-170">Välj en resursgrupp och en plats och låta andra inställningar till standardvärden.</span><span class="sxs-lookup"><span data-stu-id="48e0c-170">Choose a resource group and a location, and leave other settings at default values.</span></span> <span data-ttu-id="48e0c-171">Klicka sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-171">Then click **Create**.</span></span>

## <a name="configure-hello-head-node"></a><span data-ttu-id="48e0c-172">Konfigurera hello huvudnod</span><span class="sxs-lookup"><span data-stu-id="48e0c-172">Configure hello head node</span></span>
<span data-ttu-id="48e0c-173">toouse HPC Cluster Manager toodeploy Azure-noder och toosubmit jobb först göra konfigurationssteg krävs för klustret.</span><span class="sxs-lookup"><span data-stu-id="48e0c-173">toouse HPC Cluster Manager toodeploy Azure nodes and toosubmit jobs, first perform some required cluster configuration steps.</span></span>

1. <span data-ttu-id="48e0c-174">Starta HPC Cluster Manager på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="48e0c-174">On hello head node, start HPC Cluster Manager.</span></span> <span data-ttu-id="48e0c-175">Om hello **Välj huvudnod** dialogrutan visas klickar du på **lokal dator**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-175">If hello **Select Head Node** dialog box appears, click **Local Computer**.</span></span> <span data-ttu-id="48e0c-176">Hej **distribution uppgiftslista** visas.</span><span class="sxs-lookup"><span data-stu-id="48e0c-176">hello **Deployment To-do List** appears.</span></span>

2. <span data-ttu-id="48e0c-177">Under **krävs distributionsuppgifter**, klickar du på **konfigurera nätverket**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-177">Under **Required deployment tasks**, click **Configure your network**.</span></span>
   
    ![Konfigurera nätverket][config_hpc2]

3. <span data-ttu-id="48e0c-179">Välj i hello guiden för konfiguration av nätverk, **alla noder i ett företagsnätverk endast** (topologi 5).</span><span class="sxs-lookup"><span data-stu-id="48e0c-179">In hello Network Configuration Wizard, select **All nodes only on an enterprise network** (Topology 5).</span></span> <span data-ttu-id="48e0c-180">Den här konfigurationen är hello enklaste i exempelsyfte.</span><span class="sxs-lookup"><span data-stu-id="48e0c-180">This network configuration is hello simplest for demonstration purposes.</span></span>
   
    ![Topologi 5][config_hpc3]
   
4. <span data-ttu-id="48e0c-182">Klicka på **nästa** tooaccept standardvärdena på hello återstående sidor i guiden hello.</span><span class="sxs-lookup"><span data-stu-id="48e0c-182">Click **Next** tooaccept default values on hello remaining pages of hello wizard.</span></span> <span data-ttu-id="48e0c-183">Klicka sedan på hello **granska** klickar du på **konfigurera** toocomplete hello nätverkskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="48e0c-183">Then, on hello **Review** tab, click **Configure** toocomplete hello network configuration.</span></span>

5. <span data-ttu-id="48e0c-184">I hello **distribution uppgiftslista**, klickar du på **ange autentiseringsuppgifter för installation**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-184">In hello **Deployment To-do List**, click **Provide installation credentials**.</span></span>

6. <span data-ttu-id="48e0c-185">I hello **autentiseringsuppgifter** dialogrutan Skriv hello autentiseringsuppgifter för hello domänkonto som du använde tooinstall HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="48e0c-185">In hello **Installation Credentials** dialog box, type hello credentials of hello domain account that you used tooinstall HPC Pack.</span></span> <span data-ttu-id="48e0c-186">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-186">Then click **OK**.</span></span> 
   
    ![Installation av autentiseringsuppgifter][config_hpc6]
   
7. <span data-ttu-id="48e0c-188">I hello **distribution uppgiftslista**, klickar du på **konfigurera hello namngivning av nya noder**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-188">In hello **Deployment To-do List**, click **Configure hello naming of new nodes**.</span></span>

8. <span data-ttu-id="48e0c-189">I hello **ange nod Naming serien** dialogrutan godkänner hello standardbaserad namngivningskontext serien och klickar på **OK**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-189">In hello **Specify Node Naming Series** dialog box, accept hello default naming series and click **OK**.</span></span> <span data-ttu-id="48e0c-190">Slutför det här steget även om hello Azure noder som du lägger till i den här självstudiekursen namnges automatiskt.</span><span class="sxs-lookup"><span data-stu-id="48e0c-190">Complete this step even though hello Azure nodes you add in this tutorial are named automatically.</span></span>
   
    ![Namngivning av nod][config_hpc8]
   
9. <span data-ttu-id="48e0c-192">I hello **distribution uppgiftslista**, klickar du på **skapar en mall för noden**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-192">In hello **Deployment To-do List**, click **Create a node template**.</span></span> <span data-ttu-id="48e0c-193">Senare i hello självstudiekursen använda hello nod mallen tooadd Azure-noder toohello kluster.</span><span class="sxs-lookup"><span data-stu-id="48e0c-193">Later in hello tutorial, you use hello node template tooadd Azure nodes toohello cluster.</span></span>

10. <span data-ttu-id="48e0c-194">Hej i guiden Skapa nod, göra hello följande:</span><span class="sxs-lookup"><span data-stu-id="48e0c-194">In hello Create Node Template Wizard, do hello following:</span></span>
    
    <span data-ttu-id="48e0c-195">a.</span><span class="sxs-lookup"><span data-stu-id="48e0c-195">a.</span></span> <span data-ttu-id="48e0c-196">På hello **välja mallen nodtypen** klickar du på **nodmallen för Windows Azure**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-196">On hello **Choose Node Template Type** page, click **Windows Azure node template**, and then click **Next**.</span></span>
    
    ![Noden mall][config_hpc10]
    
    <span data-ttu-id="48e0c-198">b.</span><span class="sxs-lookup"><span data-stu-id="48e0c-198">b.</span></span> <span data-ttu-id="48e0c-199">Klicka på **nästa** tooaccept hello standard mallnamn.</span><span class="sxs-lookup"><span data-stu-id="48e0c-199">Click **Next** tooaccept hello default template name.</span></span>
    
    <span data-ttu-id="48e0c-200">c.</span><span class="sxs-lookup"><span data-stu-id="48e0c-200">c.</span></span> <span data-ttu-id="48e0c-201">På hello **ange prenumerationsinformation** anger Azure-prenumeration-ID: T (tillgänglig i din Azure kontoinformation).</span><span class="sxs-lookup"><span data-stu-id="48e0c-201">On hello **Provide Subscription Information** page, enter your Azure subscription ID (available in your Azure account information).</span></span> <span data-ttu-id="48e0c-202">I **hanteringscertifikat**, bläddra efter **standard Microsoft HPC Azure Management.**</span><span class="sxs-lookup"><span data-stu-id="48e0c-202">Then, in **Management certificate**, browse for **Default Microsoft HPC Azure Management.**</span></span> <span data-ttu-id="48e0c-203">Klicka sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-203">Then click **Next**.</span></span>
    
    ![Noden mall][config_hpc12]
    
    <span data-ttu-id="48e0c-205">d.</span><span class="sxs-lookup"><span data-stu-id="48e0c-205">d.</span></span> <span data-ttu-id="48e0c-206">På hello **ange tjänstinformation** sidan, Välj hello Molntjänsten och hello storage-konto som du skapade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="48e0c-206">On hello **Provide Service Information** page, select hello cloud service and hello storage account that you created in a previous step.</span></span> <span data-ttu-id="48e0c-207">Klicka sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-207">Then click **Next**.</span></span>
    
    ![Noden mall][config_hpc13]
    
    <span data-ttu-id="48e0c-209">e.</span><span class="sxs-lookup"><span data-stu-id="48e0c-209">e.</span></span> <span data-ttu-id="48e0c-210">Klicka på **nästa** tooaccept standardvärdena på hello återstående sidor i guiden hello.</span><span class="sxs-lookup"><span data-stu-id="48e0c-210">Click **Next** tooaccept default values on hello remaining pages of hello wizard.</span></span> <span data-ttu-id="48e0c-211">Klicka sedan på hello **granska** klickar du på **skapa** toocreate hello nodmallen.</span><span class="sxs-lookup"><span data-stu-id="48e0c-211">Then, on hello **Review** tab, click **Create** toocreate hello node template.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="48e0c-212">Som standard hello Azure nodmallen innehåller inställningar för toostart (tillhandahålla) och stoppa hello noder manuellt, använder HPC Cluster Manager.</span><span class="sxs-lookup"><span data-stu-id="48e0c-212">By default, hello Azure node template includes settings for you toostart (provision) and stop hello nodes manually, using HPC Cluster Manager.</span></span> <span data-ttu-id="48e0c-213">Du kan också konfigurera ett schema toostart och stoppa hello Azure-noder automatiskt.</span><span class="sxs-lookup"><span data-stu-id="48e0c-213">You can optionally configure a schedule toostart and stop hello Azure nodes automatically.</span></span>
    > 
    > 

## <a name="add-azure-nodes-toohello-cluster"></a><span data-ttu-id="48e0c-214">Lägga till Azure-noder toohello kluster</span><span class="sxs-lookup"><span data-stu-id="48e0c-214">Add Azure nodes toohello cluster</span></span>
<span data-ttu-id="48e0c-215">Nu använda hello nod mallen tooadd Azure-noder toohello kluster.</span><span class="sxs-lookup"><span data-stu-id="48e0c-215">Now use hello node template tooadd Azure nodes toohello cluster.</span></span> <span data-ttu-id="48e0c-216">Lägga till hello noder toohello klustret lagrar konfigurationsinformation om deras så att du kan starta (tillhandahålla) dem när som helst i hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="48e0c-216">Adding hello nodes toohello cluster stores their configuration information so that you can start (provision) them at any time in hello cloud service.</span></span> <span data-ttu-id="48e0c-217">Din prenumeration hämtar endast debiteras för Azure-noder när hello-instanser körs i hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="48e0c-217">Your subscription only gets charged for Azure nodes after hello instances are running in hello cloud service.</span></span>

<span data-ttu-id="48e0c-218">Följ dessa steg tooadd två Small noder.</span><span class="sxs-lookup"><span data-stu-id="48e0c-218">Follow these steps tooadd two Small nodes.</span></span>

1. <span data-ttu-id="48e0c-219">HPC Cluster Manager klickar du på **hantering** (kallas **resurshantering** i aktuella versioner av HPC Pack) > **Lägg till nod**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-219">In HPC Cluster Manager, click **Node Management** (called **Resource Management** in current versions of HPC Pack) > **Add Node**.</span></span>
   
    ![Lägg till nod][add_node1]

2. <span data-ttu-id="48e0c-221">I hello guiden för Lägg till nod på hello **väljer distributionsmetoden** klickar du på **lägga till Windows Azure-noder**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-221">In hello Add Node Wizard, on hello **Select Deployment Method** page, click **Add Windows Azure nodes**, and then click **Next**.</span></span>
   
    ![Lägg till Azure nod][add_node1_1]

3. <span data-ttu-id="48e0c-223">På hello **ange nya noder** sidan, Välj hello Azure nod mall du skapade tidigare (kallas som standard **AzureNode standardmallen**).</span><span class="sxs-lookup"><span data-stu-id="48e0c-223">On hello **Specify New Nodes** page, select hello Azure node template you created previously (called by default **Default AzureNode Template**).</span></span> <span data-ttu-id="48e0c-224">Ange **2** noder i storlek **små**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-224">Then specify **2** nodes of size **Small**, and then click **Next**.</span></span>
   
    ![Ange noder][add_node2]
   
4. <span data-ttu-id="48e0c-226">På hello **Slutför hello guiden Lägg till nod** klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-226">On hello **Completing hello Add Node Wizard** page, click **Finish**.</span></span>
    
     <span data-ttu-id="48e0c-227">Två Azure noder med namnet **AzureCN 0001** och **AzureCN 0002**, visas nu i HPC Cluster Manager.</span><span class="sxs-lookup"><span data-stu-id="48e0c-227">Two Azure nodes, named **AzureCN-0001** and **AzureCN-0002**, now appear in HPC Cluster Manager.</span></span> <span data-ttu-id="48e0c-228">Båda värdena i hello **inte distribuerade** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="48e0c-228">Both are in hello **Not-Deployed** state.</span></span>
   
    ![Tillagda noder][add_node3]

## <a name="start-hello-azure-nodes"></a><span data-ttu-id="48e0c-230">Starta hello Azure-noder</span><span class="sxs-lookup"><span data-stu-id="48e0c-230">Start hello Azure nodes</span></span>
<span data-ttu-id="48e0c-231">När du vill toouse hello klusterresurser i Azure, Använd HPC Cluster Manager toostart (tillhandahålla) hello Azure-noder och tar dem online.</span><span class="sxs-lookup"><span data-stu-id="48e0c-231">When you want toouse hello cluster resources in Azure, use HPC Cluster Manager toostart (provision) hello Azure nodes and bring them online.</span></span>

1. <span data-ttu-id="48e0c-232">HPC Cluster Manager klickar du på **hantering** (kallas **resurshantering** i aktuella versioner av HPC Pack), och välj hello Azure-noder.</span><span class="sxs-lookup"><span data-stu-id="48e0c-232">In HPC Cluster Manager, click **Node Management** (called **Resource Management** in current versions of HPC Pack), and select hello Azure nodes.</span></span>

2. <span data-ttu-id="48e0c-233">Klicka på **starta**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-233">Click **Start**, and then click **OK**.</span></span>
   
   ![Starta noder][add_node4]
   
    <span data-ttu-id="48e0c-235">hello noder övergång toohello **etablering** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="48e0c-235">hello nodes transition toohello **Provisioning** state.</span></span> <span data-ttu-id="48e0c-236">Visa hello etablering loggen tootrack hello etablering pågår.</span><span class="sxs-lookup"><span data-stu-id="48e0c-236">View hello provisioning log tootrack hello provisioning progress.</span></span>
   
    ![Etablera noder][add_node6]

3. <span data-ttu-id="48e0c-238">Efter några minuter hello Azure-noder Slutför etablering och är i hello **Offline** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="48e0c-238">After a few minutes, hello Azure nodes finish provisioning and are in hello **Offline** state.</span></span> <span data-ttu-id="48e0c-239">I det här tillståndet hello rollinstanser kör men ännu Acceptera inte klustret jobb.</span><span class="sxs-lookup"><span data-stu-id="48e0c-239">In this state, hello role instances are running but cannot yet accept cluster jobs.</span></span>

4. <span data-ttu-id="48e0c-240">tooconfirm som hello rollinstanser körs i hello Azure-portalen klickar du på **molntjänster (klassisk)** > *your_cloud_service_name*.</span><span class="sxs-lookup"><span data-stu-id="48e0c-240">tooconfirm that hello role instances are running, in hello Azure portal, click **Cloud Services (classic)** > *your_cloud_service_name*.</span></span>
   
   <span data-ttu-id="48e0c-241">Du bör se två **HpcWorkerRole** instanser (noder) som körs i hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="48e0c-241">You should see two **HpcWorkerRole** instances (nodes) running in hello service.</span></span> <span data-ttu-id="48e0c-242">HPC Pack också automatiskt distribuerar två **HpcProxy** instanser (storlek medel) toohandle kommunikation mellan hello huvudnod och Azure.</span><span class="sxs-lookup"><span data-stu-id="48e0c-242">HPC Pack also automatically deploys two **HpcProxy** instances (size Medium) toohandle communication between hello head node and Azure.</span></span>

   ![Kör instanser][view_instances1]

5. <span data-ttu-id="48e0c-244">toobring hello Azure-noder online toorun kluster-jobb, Välj hello noder, högerklicka och klicka sedan på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-244">toobring hello Azure nodes online toorun cluster jobs, select hello nodes, right-click, and then click **Bring Online**.</span></span>
   
    ![Offline-noder][add_node7]
   
    <span data-ttu-id="48e0c-246">HPC Cluster Manager visar att hello noder i hello **Online** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="48e0c-246">HPC Cluster Manager indicates that hello nodes are in hello **Online** state.</span></span>

## <a name="run-a-command-across-hello-cluster"></a><span data-ttu-id="48e0c-247">Köra ett kommando över hello kluster</span><span class="sxs-lookup"><span data-stu-id="48e0c-247">Run a command across hello cluster</span></span>
<span data-ttu-id="48e0c-248">toocheck hello installation, användning hello HPC Pack **clusrun** kommandot toorun ett kommando eller ett program på en eller flera klusternoder.</span><span class="sxs-lookup"><span data-stu-id="48e0c-248">toocheck hello installation, use hello HPC Pack **clusrun** command toorun a command or application on one or more cluster nodes.</span></span> <span data-ttu-id="48e0c-249">Ett enkelt exempel använder **clusrun** tooget hello IP-konfiguration hello Azure-noder.</span><span class="sxs-lookup"><span data-stu-id="48e0c-249">As a simple example, use **clusrun** tooget hello IP configuration of hello Azure nodes.</span></span>

1. <span data-ttu-id="48e0c-250">Öppna en kommandotolk som administratör på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="48e0c-250">On hello head node, open a command prompt as an administrator.</span></span>

2. <span data-ttu-id="48e0c-251">Skriv följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="48e0c-251">Type hello following command:</span></span>
   
    `clusrun /nodes:azurecn* ipconfig`

3. <span data-ttu-id="48e0c-252">Om du uppmanas ange administratörslösenordet klustret.</span><span class="sxs-lookup"><span data-stu-id="48e0c-252">If prompted, enter your cluster administrator password.</span></span> <span data-ttu-id="48e0c-253">Du bör se utdata liknande toohello följande för kommandot.</span><span class="sxs-lookup"><span data-stu-id="48e0c-253">You should see command output similar toohello following.</span></span>
   
    ![Clusrun][clusrun1]

## <a name="run-a-test-job"></a><span data-ttu-id="48e0c-255">Kör ett testjobb</span><span class="sxs-lookup"><span data-stu-id="48e0c-255">Run a test job</span></span>
<span data-ttu-id="48e0c-256">Nu skicka ett testjobb som körs på hello hybrid klustret.</span><span class="sxs-lookup"><span data-stu-id="48e0c-256">Now submit a test job that runs on hello hybrid cluster.</span></span> <span data-ttu-id="48e0c-257">Det här exemplet är ett enkelt parametrisk rensning jobb (en typ av filsystemen parallella beräkning).</span><span class="sxs-lookup"><span data-stu-id="48e0c-257">This example is a simple parametric sweep job (a type of intrinsically parallel computation).</span></span> <span data-ttu-id="48e0c-258">Det här exemplet körs underaktiviteter som lägger till ett heltal tooitself med hello **ange /a** kommando.</span><span class="sxs-lookup"><span data-stu-id="48e0c-258">This example runs subtasks that add an integer tooitself by using hello **set /a** command.</span></span> <span data-ttu-id="48e0c-259">Alla hello-noder i klustret hello bidrar toofinishing hello underaktiviteter för heltal från 1 too100.</span><span class="sxs-lookup"><span data-stu-id="48e0c-259">All hello nodes in hello cluster contribute toofinishing hello subtasks for integers from 1 too100.</span></span>

1. <span data-ttu-id="48e0c-260">HPC Cluster Manager klickar du på **jobbhantering** > **nytt parametrisk Sopa jobb**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-260">In HPC Cluster Manager, click **Job Management** > **New Parametric Sweep Job**.</span></span>
   
    ![Nytt jobb][test_job1]

2. <span data-ttu-id="48e0c-262">I hello **nytt parametrisk Sopa jobb** i dialogrutan **kommandoraden**, typen `set /a *+*` (skriva över hello standardkommandoraden som visas).</span><span class="sxs-lookup"><span data-stu-id="48e0c-262">In hello **New Parametric Sweep Job** dialog box, in **Command line**, type `set /a *+*` (overwriting hello default command line that appears).</span></span> <span data-ttu-id="48e0c-263">Lämna standardvärdena för hello återstående inställningar och klickar sedan på **skicka** toosubmit hello jobb.</span><span class="sxs-lookup"><span data-stu-id="48e0c-263">Leave default values for hello remaining settings, and then click **Submit** toosubmit hello job.</span></span>
   
    ![Parametrisk rensning][param_sweep1]

3. <span data-ttu-id="48e0c-265">När hello jobbet är klart, dubbelklickar du på hello **Mina Sopa uppgiften** jobb.</span><span class="sxs-lookup"><span data-stu-id="48e0c-265">When hello job is finished, double-click hello **My Sweep Task** job.</span></span>

4. <span data-ttu-id="48e0c-266">Klicka på **uppgiftsvyn**, och klicka sedan på en delaktivitet tooview hello beräknade utdata på samma nivå.</span><span class="sxs-lookup"><span data-stu-id="48e0c-266">Click **View Tasks**, and then click a subtask tooview hello calculated output of that subtask.</span></span>
   
    ![Uppgiften resultat][view_job361]

5. <span data-ttu-id="48e0c-268">toosee vilken nod som utförs hello beräkning för den underaktiviteten klickar du på **allokerade noder**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-268">toosee which node performed hello calculation for that subtask, click **Allocated Nodes**.</span></span> <span data-ttu-id="48e0c-269">(Klustret kan visa en annan nod-namn).</span><span class="sxs-lookup"><span data-stu-id="48e0c-269">(Your cluster might show a different node name.)</span></span>
   
    ![Uppgiften resultat][view_job362]

## <a name="stop-hello-azure-nodes"></a><span data-ttu-id="48e0c-271">Stoppa hello Azure-noder</span><span class="sxs-lookup"><span data-stu-id="48e0c-271">Stop hello Azure nodes</span></span>
<span data-ttu-id="48e0c-272">Stoppa hello Azure-noder tooavoid onödiga kostnader tooyour konto när du testa hello klustret.</span><span class="sxs-lookup"><span data-stu-id="48e0c-272">After you try out hello cluster, stop hello Azure nodes tooavoid unnecessary charges tooyour account.</span></span> <span data-ttu-id="48e0c-273">Det här steget stoppar hello Molntjänsten och tar bort hello Azure rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="48e0c-273">This step stops hello cloud service and removes hello Azure role instances.</span></span>

1. <span data-ttu-id="48e0c-274">HPC Cluster Manager i **hantering** (kallas **resurshantering** i tidigare versioner av HPC Pack), Välj både Azure-noder.</span><span class="sxs-lookup"><span data-stu-id="48e0c-274">In HPC Cluster Manager, in **Node Management** (called **Resource Management** in previous versions of HPC Pack), select both Azure nodes.</span></span> <span data-ttu-id="48e0c-275">Klicka på **stoppa**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-275">Then, click **Stop**.</span></span>
   
    ![Stoppa noder][stop_node1]

2. <span data-ttu-id="48e0c-277">I hello **stoppar Windows Azure-noder** dialogrutan klickar du på **stoppa**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-277">In hello **Stop Windows Azure nodes** dialog box, click **Stop**.</span></span> 
   
3. <span data-ttu-id="48e0c-278">hello noder övergång toohello **stoppar** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="48e0c-278">hello nodes transition toohello **Stopping** state.</span></span> <span data-ttu-id="48e0c-279">Efter några minuter HPC Cluster Manager visar att hello noder är **inte distribuerade**.</span><span class="sxs-lookup"><span data-stu-id="48e0c-279">After a few minutes, HPC Cluster Manager shows that hello nodes are **Not-Deployed**.</span></span>
   
    ![Inte distribuerad noder][stop_node4]

4. <span data-ttu-id="48e0c-281">tooconfirm hello rollinstanser körs inte längre i Azure, i hello Azure klickar du på **molntjänster (klassisk)** > *your_cloud_service_name*.</span><span class="sxs-lookup"><span data-stu-id="48e0c-281">tooconfirm that hello role instances are no longer running in Azure, in hello Azure portal, click **Cloud services (classic)** > *your_cloud_service_name*.</span></span> <span data-ttu-id="48e0c-282">Inga instanser har distribuerats i hello-produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="48e0c-282">No instances are deployed in hello production environment.</span></span> 
   
    <span data-ttu-id="48e0c-283">Detta avslutar hello kursen.</span><span class="sxs-lookup"><span data-stu-id="48e0c-283">This completes hello tutorial.</span></span>

## <a name="next-steps"></a><span data-ttu-id="48e0c-284">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="48e0c-284">Next steps</span></span>
* <span data-ttu-id="48e0c-285">Utforska hello dokumentationen för [HPC Pack](https://technet.microsoft.com/library/cc514029).</span><span class="sxs-lookup"><span data-stu-id="48e0c-285">Explore hello documentation for [HPC Pack](https://technet.microsoft.com/library/cc514029).</span></span>
* <span data-ttu-id="48e0c-286">tooset upp en hybrid HPC Pack Klusterdistribution i större skala, se [Burst tooAzure Worker Rollinstanser with Microsoft HPC Pack](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span><span class="sxs-lookup"><span data-stu-id="48e0c-286">tooset up a hybrid HPC Pack cluster deployment at greater scale, see [Burst tooAzure Worker Role Instances with Microsoft HPC Pack](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span></span>
* <span data-ttu-id="48e0c-287">Andra sätt toocreate ett HPC Pack kluster i Azure, inklusive med Azure Resource Manager-mallar finns i [HPC-klusteralternativ with Microsoft HPC Pack i Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="48e0c-287">For other ways toocreate an HPC Pack cluster in Azure, including using Azure Resource Manager templates, see [HPC cluster options with Microsoft HPC Pack in Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


[Overview]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/hybrid_cluster_overview.png
[install_hpc1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc1.png
[install_hpc4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc4.png
[install_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc6.png
[install_hpc7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc7.png
[install_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc10.png
[config_hpc2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc2.png
[config_hpc3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc3.png
[config_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc6.png
[config_hpc8]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc8.png
[config_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc10.png
[config_hpc12]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc12.png
[config_hpc13]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc13.png
[add_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1.png
[add_node1_1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1_1.png
[add_node2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node2.png
[add_node3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node3.png
[add_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node4.png
[add_node6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node6.png
[add_node7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node7.png
[view_instances1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances1.png
[clusrun1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/clusrun1.png
[test_job1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/test_job1.png
[param_sweep1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/param_sweep1.png
[view_job361]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job361.png
[view_job362]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job362.png
[stop_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node1.png
[stop_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node4.png
[view_instances2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances2.png
