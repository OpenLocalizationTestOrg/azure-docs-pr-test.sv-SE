---
title: aaaSubmit jobb tooan HPC Pack kluster i Azure | Microsoft Docs
description: "Lär dig hur tooset upp en lokal dator toosubmit jobb tooan HPC Pack kluster i Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: 78f6833c-4aa6-4b3e-be71-97201abb4721
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 10/14/2016
ms.author: danlep
ms.openlocfilehash: 2918cf633917d8730487152e6a5ddb863eb8bb5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="submit-hpc-jobs-from-an-on-premises-computer-tooan-hpc-pack-cluster-deployed-in-azure"></a><span data-ttu-id="c6958-103">Skicka HPC-jobb från en lokal dator tooan HPC Pack kluster distribuerade i Azure</span><span class="sxs-lookup"><span data-stu-id="c6958-103">Submit HPC jobs from an on-premises computer tooan HPC Pack cluster deployed in Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="c6958-104">Konfigurera en lokal klient datorn toosubmit jobb tooa [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) kluster i Azure.</span><span class="sxs-lookup"><span data-stu-id="c6958-104">Configure an on-premises client computer toosubmit jobs tooa [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) cluster in Azure.</span></span> <span data-ttu-id="c6958-105">Den här artikeln visar hur tooset upp en lokal dator med klienten verktygen toosubmit jobb via HTTPS toohello kluster i Azure.</span><span class="sxs-lookup"><span data-stu-id="c6958-105">This article shows you how tooset up a local computer with client tools toosubmit job over HTTPS toohello cluster in Azure.</span></span> <span data-ttu-id="c6958-106">På så sätt kan flera kluster användarna kan skicka jobb tooa molnbaserade HPC Pack kluster, men utan att ansluta direkt toohello huvudnod VM eller få åtkomst till en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c6958-106">In this way, several cluster users can submit jobs tooa cloud-based HPC Pack cluster, but without connecting directly toohello head node VM or accessing an Azure subscription.</span></span>

![Skicka ett jobb tooa kluster i Azure][jobsubmit]

## <a name="prerequisites"></a><span data-ttu-id="c6958-108">Krav</span><span class="sxs-lookup"><span data-stu-id="c6958-108">Prerequisites</span></span>
* <span data-ttu-id="c6958-109">**HPC Pack huvudnod distribueras i en Azure VM** -rekommenderar vi att du använder automatiserade verktyg som en [Azure quickstart mallen](https://azure.microsoft.com/documentation/templates/) eller en [Azure PowerShell-skript](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toodeploy hello huvudnod och kluster.</span><span class="sxs-lookup"><span data-stu-id="c6958-109">**HPC Pack head node deployed in an Azure VM** - We recommend that you use automated tools such as an [Azure quickstart template](https://azure.microsoft.com/documentation/templates/) or an [Azure PowerShell script](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toodeploy hello head node and cluster.</span></span> <span data-ttu-id="c6958-110">Du behöver hello DNS-namnet på hello huvudnod och hello autentiseringsuppgifterna för en Klusteradministratör för att slutföra hello stegen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="c6958-110">You need hello DNS name of hello head node and hello credentials of a cluster administrator to complete hello steps in this article.</span></span>
* <span data-ttu-id="c6958-111">**Klientdatorn** -du behöver en Windows- eller Windows Server-klientdator som kan köra HPC Pack klientverktyg (se [systemkrav](https://technet.microsoft.com/library/dn535781.aspx)).</span><span class="sxs-lookup"><span data-stu-id="c6958-111">**Client computer** - You need a Windows or Windows Server client computer that can run HPC Pack client utilities (see [system requirements](https://technet.microsoft.com/library/dn535781.aspx)).</span></span> <span data-ttu-id="c6958-112">Om du bara vill toouse hello HPC Pack webbportal eller REST API toosubmit jobb kan du använda alla klientdatorer som du väljer.</span><span class="sxs-lookup"><span data-stu-id="c6958-112">If you only want toouse hello HPC Pack web portal or REST API toosubmit jobs, you can use any client computer of your choice.</span></span>
* <span data-ttu-id="c6958-113">**HPC Pack installationsmediet** -tooinstall hello HPC Pack klientverktyg, hello ledigt installationspaketet för den senaste versionen av HPC Pack (HPC Pack 2012 R2) är tillgängliga från den [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024).</span><span class="sxs-lookup"><span data-stu-id="c6958-113">**HPC Pack installation media** - tooinstall hello HPC Pack client utilities, hello free installation package for the latest version of HPC Pack (HPC Pack 2012 R2) is available from the [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024).</span></span> <span data-ttu-id="c6958-114">Kontrollera att du hämtar hello samma version av HPC Pack som är installerad på hello huvudnod VM.</span><span class="sxs-lookup"><span data-stu-id="c6958-114">Make sure that you download hello same version of HPC Pack that is installed on hello head node VM.</span></span>

## <a name="step-1-install-and-configure-hello-web-components-on-hello-head-node"></a><span data-ttu-id="c6958-115">Steg 1: Installera och konfigurera hello Webbkomponenter på hello huvudnod</span><span class="sxs-lookup"><span data-stu-id="c6958-115">Step 1: Install and configure hello web components on hello head node</span></span>
<span data-ttu-id="c6958-116">tooenable ett REST-gränssnittet toosubmit jobb toohello kluster via HTTPS, se till att hello HPC Pack Webbkomponenter konfigureras i hello HPC Pack huvudnod.</span><span class="sxs-lookup"><span data-stu-id="c6958-116">tooenable a REST interface toosubmit jobs toohello cluster over HTTPS, ensure that hello HPC Pack web components are configured on hello HPC Pack head node.</span></span> <span data-ttu-id="c6958-117">Om de inte redan har installerats måste du först installera hello webbkomponenterna genom att köra hello HpcWebComponents.msi installationsfilen.</span><span class="sxs-lookup"><span data-stu-id="c6958-117">If they aren't already installed, first install hello web components by running hello HpcWebComponents.msi installation file.</span></span> <span data-ttu-id="c6958-118">Konfigurera sedan hello komponenter genom att köra hello HPC PowerShell-skript **Set HPCWebComponents.ps1**.</span><span class="sxs-lookup"><span data-stu-id="c6958-118">Then, configure hello components by running hello HPC PowerShell script **Set-HPCWebComponents.ps1**.</span></span>

<span data-ttu-id="c6958-119">Detaljerade anvisningar finns [installera hello Microsoft HPC Pack Webbkomponenter](http://technet.microsoft.com/library/hh314627.aspx).</span><span class="sxs-lookup"><span data-stu-id="c6958-119">For detailed procedures, see [Install hello Microsoft HPC Pack Web Components](http://technet.microsoft.com/library/hh314627.aspx).</span></span>

> [!TIP]
> <span data-ttu-id="c6958-120">Vissa mallar för Azure quickstart för HPC Pack installera och konfigurera hello Webbkomponenter automatiskt.</span><span class="sxs-lookup"><span data-stu-id="c6958-120">Certain Azure quickstart templates for HPC Pack install and configure hello web components automatically.</span></span> <span data-ttu-id="c6958-121">Om du använder hello [HPC Pack IaaS distributionsskriptet](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toocreate hello klustret, du kan alternativt installera och konfigurera hello webbkomponenter som en del av hello distribution.</span><span class="sxs-lookup"><span data-stu-id="c6958-121">If you use hello [HPC Pack IaaS deployment script](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toocreate hello cluster, you can optionally install and configure hello web components as part of hello deployment.</span></span>
> 
> 

<span data-ttu-id="c6958-122">**tooinstall hello Webbkomponenter**</span><span class="sxs-lookup"><span data-stu-id="c6958-122">**tooinstall hello web components**</span></span>

1. <span data-ttu-id="c6958-123">Anslut toohello huvudnod VM via hello autentiseringsuppgifterna för en Klusteradministratör.</span><span class="sxs-lookup"><span data-stu-id="c6958-123">Connect toohello head node VM by using hello credentials of a cluster administrator.</span></span>
2. <span data-ttu-id="c6958-124">Kör HpcWebComponents.msi från hello HPC Pack installationsmapp, i hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="c6958-124">From hello HPC Pack Setup folder, run HpcWebComponents.msi on hello head node.</span></span>
3. <span data-ttu-id="c6958-125">Följ stegen i hello i hello guiden tooinstall hello Webbkomponenter</span><span class="sxs-lookup"><span data-stu-id="c6958-125">Follow hello steps in hello wizard tooinstall hello web components</span></span>

<span data-ttu-id="c6958-126">**tooconfigure hello Webbkomponenter**</span><span class="sxs-lookup"><span data-stu-id="c6958-126">**tooconfigure hello web components**</span></span>

1. <span data-ttu-id="c6958-127">Starta HPC PowerShell som administratör på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="c6958-127">On hello head node, start HPC PowerShell as an administrator.</span></span>
2. <span data-ttu-id="c6958-128">toochange toohello katalogplatsen för hello konfigurationsskript, typen hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c6958-128">toochange directory toohello location of hello configuration script, type hello following command:</span></span>
   
    ```powershell
    cd $env:CCP_HOME\bin
    ```
3. <span data-ttu-id="c6958-129">tooconfigure hello REST-gränssnittet och starta hello HPC-webbtjänsten, Skriv hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c6958-129">tooconfigure hello REST interface and start hello HPC Web Service, type hello following command:</span></span>
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```
4. <span data-ttu-id="c6958-130">När begärd tooselect ett certifikat väljer hello certifikat motsvarar som toohello offentliga DNS-namnet på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="c6958-130">When prompted tooselect a certificate, choose hello certificate that corresponds toohello public DNS name of hello head node.</span></span> <span data-ttu-id="c6958-131">Till exempel om du distribuerar hello huvudnod VM som använder hello klassiska distributionsmodellen, certifikatnamnet hello ser ut som CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="c6958-131">For example, if you deploy hello head node VM using hello classic deployment model, hello certificate name looks like CN=&lt;*HeadNodeDnsName*&gt;.cloudapp.net.</span></span> <span data-ttu-id="c6958-132">Om du använder hello Resource Manager-distributionsmodellen hello certifikatnamn ser ut som CN =&lt;*HeadNodeDnsName*&gt;.&lt; *region*&gt;. cloudapp.azure.com.</span><span class="sxs-lookup"><span data-stu-id="c6958-132">If you use hello Resource Manager deployment model, hello certificate name looks like CN=&lt;*HeadNodeDnsName*&gt;.&lt;*region*&gt;.cloudapp.azure.com.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="c6958-133">Väljer du det här certifikatet senare när du skickar jobb toohello huvudnoden från en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="c6958-133">You select this certificate later when you submit jobs toohello head node from an on-premises computer.</span></span> <span data-ttu-id="c6958-134">Inte markera eller konfigurera ett certifikat som motsvarar toohello datornamnet på hello huvudnod i hello Active Directory-domän (till exempel CN =*MyHPCHeadNode.HpcAzure.local*).</span><span class="sxs-lookup"><span data-stu-id="c6958-134">Don't select or configure a certificate that corresponds toohello computer name of hello head node in hello Active Directory domain (for example, CN=*MyHPCHeadNode.HpcAzure.local*).</span></span>
   > 
   > 
5. <span data-ttu-id="c6958-135">tooconfigure hello webbportalen för jobbet, typen hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c6958-135">tooconfigure hello web portal for job submission, type hello following command:</span></span>
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
6. <span data-ttu-id="c6958-136">När hello skriptet har slutförts hello stoppa och starta om tjänsten för Rapportjobbschemaläggning HPC genom att skriva följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="c6958-136">After hello script completes, stop and restart hello HPC Job Scheduler Service by typing hello following commands:</span></span>
   
    ```powershell
    net stop hpcscheduler
    net start hpcscheduler
    ```

## <a name="step-2-install-hello-hpc-pack-client-utilities-on-an-on-premises-computer"></a><span data-ttu-id="c6958-137">Steg 2: Installera hello HPC Pack klientverktyg på en lokal dator</span><span class="sxs-lookup"><span data-stu-id="c6958-137">Step 2: Install hello HPC Pack client utilities on an on-premises computer</span></span>
<span data-ttu-id="c6958-138">Om du vill tooinstall hello HPC Pack klientverktyg på datorn kan ladda ned installationsfilerna HPC Pack (fullständig installation) från hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024).</span><span class="sxs-lookup"><span data-stu-id="c6958-138">If you want tooinstall hello HPC Pack client utilities on your computer, download the HPC Pack setup files (full installation) from hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024).</span></span> <span data-ttu-id="c6958-139">När du börjar installationen hello väljer hello installationsalternativ för hello **HPC Pack klientverktyg**.</span><span class="sxs-lookup"><span data-stu-id="c6958-139">When you begin hello installation, choose hello setup option for hello **HPC Pack client utilities**.</span></span>

<span data-ttu-id="c6958-140">toouse hello HPC Pack klienten verktygen toosubmit jobb toohello huvudnod VM kan du även behöver tooexport ett certifikat från hello huvudnod och installera den på hello-klientdator.</span><span class="sxs-lookup"><span data-stu-id="c6958-140">toouse hello HPC Pack client tools toosubmit jobs toohello head node VM, you also need tooexport a certificate from hello head node and install it on hello client computer.</span></span> <span data-ttu-id="c6958-141">hello certifikatet måste vara i. CER-format.</span><span class="sxs-lookup"><span data-stu-id="c6958-141">hello certificate must be in .CER format.</span></span>

<span data-ttu-id="c6958-142">**tooexport hello certifikat från hello huvudnod**</span><span class="sxs-lookup"><span data-stu-id="c6958-142">**tooexport hello certificate from hello head node**</span></span>

1. <span data-ttu-id="c6958-143">Lägg till hello certifikat snapin-modulen tooa Microsoft Management Console för hello lokalt datorkonto på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="c6958-143">On hello head node, add hello Certificates snap-in tooa Microsoft Management Console for hello Local Computer account.</span></span> <span data-ttu-id="c6958-144">Anvisningar för hur tooadd hello snapin-modulen finns [lägga till hello snapin-modulen Certifikat tooan MMC](https://technet.microsoft.com/library/cc754431.aspx).</span><span class="sxs-lookup"><span data-stu-id="c6958-144">For steps tooadd hello snap-in, see [Add hello Certificates Snap-in tooan MMC](https://technet.microsoft.com/library/cc754431.aspx).</span></span>
2. <span data-ttu-id="c6958-145">Hello-konsolträdet, expandera **certifikat – lokal dator** > **personliga**, och klicka sedan på **certifikat**.</span><span class="sxs-lookup"><span data-stu-id="c6958-145">In hello console tree, expand **Certificates – Local Computer** > **Personal**, and then click **Certificates**.</span></span>
3. <span data-ttu-id="c6958-146">Hitta hello-certifikat som har konfigurerats för hello HPC Pack webbkomponenter i [steg 1: Installera och konfigurera hello Webbkomponenter på hello huvudnod](#step-1:-install-and-configure-the-web-components-on-the-head-node) (exempelvis, CN =&lt;*HeadNodeDnsName* &gt;. cloudapp.net).</span><span class="sxs-lookup"><span data-stu-id="c6958-146">Locate hello certificate that you configured for hello HPC Pack web components in [Step 1: Install and configure hello web components on hello head node](#step-1:-install-and-configure-the-web-components-on-the-head-node) (for example, CN=&lt;*HeadNodeDnsName*&gt;.cloudapp.net).</span></span>
4. <span data-ttu-id="c6958-147">Högerklicka på hello certifikatet och klicka på **alla aktiviteter** > **exportera**.</span><span class="sxs-lookup"><span data-stu-id="c6958-147">Right-click hello certificate, and click **All Tasks** > **Export**.</span></span>
5. <span data-ttu-id="c6958-148">I hello guiden Exportera certifikat, klickar du på **nästa**, och kontrollera att **Nej, exportera inte privat nyckel för hello** är markerad.</span><span class="sxs-lookup"><span data-stu-id="c6958-148">In hello Certificate Export Wizard, click **Next**, and ensure that **No, do not export hello private key** is selected.</span></span>
6. <span data-ttu-id="c6958-149">Följ hello återstående steg av hello guiden tooexport hello certifikat i DER-kodad binärfil X.509 (. CER)-format.</span><span class="sxs-lookup"><span data-stu-id="c6958-149">Follow hello remaining steps of hello wizard tooexport hello certificate in DER encoded binary X.509 (.CER) format.</span></span>

<span data-ttu-id="c6958-150">**tooimport hello certifikatet på hello-klientdatorn**</span><span class="sxs-lookup"><span data-stu-id="c6958-150">**tooimport hello certificate on hello client computer**</span></span>

1. <span data-ttu-id="c6958-151">Kopiera hello-certifikat som du exporterade från hello huvudnod tooa mapp på hello-klientdator.</span><span class="sxs-lookup"><span data-stu-id="c6958-151">Copy hello certificate that you exported from hello head node tooa folder on hello client computer.</span></span>
2. <span data-ttu-id="c6958-152">Kör certmgr.msc på hello klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="c6958-152">On hello client computer, run certmgr.msc.</span></span>
3. <span data-ttu-id="c6958-153">Expandera i Certifikathanteraren **certifikat – aktuell användare** > **betrodda rotcertifikatutfärdare**, högerklicka på **certifikat**, och sedan Klicka på **alla aktiviteter** > **importera**.</span><span class="sxs-lookup"><span data-stu-id="c6958-153">In Certificate Manager, expand **Certificates – Current user** > **Trusted Root Certification Authorities**, right-click **Certificates**, and then click **All Tasks** > **Import**.</span></span>
4. <span data-ttu-id="c6958-154">I hello guiden Importera certifikat klickar du på **nästa** och följ hello steg tooimport hello certifikat som du exporterade från hello huvudnod toohello betrodda rotcertifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="c6958-154">In hello Certificate Import Wizard, click **Next** and follow hello steps tooimport hello certificate that you exported from hello head node toohello Trusted Root Certification Authorities store.</span></span>

> [!TIP]
> <span data-ttu-id="c6958-155">Du kan se en säkerhetsvarning, eftersom hello certifikatutfärdare på hello huvudnod kan inte identifiera hello klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="c6958-155">You might see a security warning, because hello certification authority on hello head node isn't recognized by hello client computer.</span></span> <span data-ttu-id="c6958-156">I testsyfte kan ignorera du denna varning och fullständig hello Importera certifikat.</span><span class="sxs-lookup"><span data-stu-id="c6958-156">For testing purposes, you can ignore this warning and complete hello certificate import.</span></span>
> 
> 

## <a name="step-3-run-test-jobs-on-hello-cluster"></a><span data-ttu-id="c6958-157">Steg 3: Kör testjobb på hello klustret</span><span class="sxs-lookup"><span data-stu-id="c6958-157">Step 3: Run test jobs on hello cluster</span></span>
<span data-ttu-id="c6958-158">tooverify konfigurationen, försök jobb som körs på hello-kluster i Azure från hello lokal dator.</span><span class="sxs-lookup"><span data-stu-id="c6958-158">tooverify your configuration, try running jobs on hello cluster in Azure from hello on-premises computer.</span></span> <span data-ttu-id="c6958-159">Du kan till exempel använda HPC Pack GUI verktyg och kommandon på kommandoraden toosubmit jobb toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="c6958-159">For example, you can use HPC Pack GUI tools or command-line commands toosubmit jobs toohello cluster.</span></span> <span data-ttu-id="c6958-160">Du kan också använda en webbaserad portal toosubmit jobb.</span><span class="sxs-lookup"><span data-stu-id="c6958-160">You can also use a web-based portal toosubmit jobs.</span></span>

<span data-ttu-id="c6958-161">**toorun jobbet skicka kommandon på hello klientdator**</span><span class="sxs-lookup"><span data-stu-id="c6958-161">**toorun job submission commands on hello client computer**</span></span>

1. <span data-ttu-id="c6958-162">Starta Kommandotolken på en klientdator där hello HPC Pack klientverktyg är installerade.</span><span class="sxs-lookup"><span data-stu-id="c6958-162">On a client computer where hello HPC Pack client utilities are installed, start a Command Prompt.</span></span>
2. <span data-ttu-id="c6958-163">Ange en Exempelkommando.</span><span class="sxs-lookup"><span data-stu-id="c6958-163">Type a sample command.</span></span> <span data-ttu-id="c6958-164">Skriv till exempel toolist alla jobb på hello klustret ett kommando liknande tooone av följande, beroende på hello fullständiga DNS-namnet på hello huvudnod hello:</span><span class="sxs-lookup"><span data-stu-id="c6958-164">For example, toolist all jobs on hello cluster, type a command similar tooone of hello following, depending on hello full DNS name of hello head node:</span></span>
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all
    ```
   
    <span data-ttu-id="c6958-165">eller</span><span class="sxs-lookup"><span data-stu-id="c6958-165">or</span></span>
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.<region>.cloudapp.azure.com /all
    ```
   
   > [!TIP]
   > <span data-ttu-id="c6958-166">Använd hello fullständiga DNS-namnet på hello huvudnod, inte hello IP-adress i hello scheduler-URL.</span><span class="sxs-lookup"><span data-stu-id="c6958-166">Use hello full DNS name of hello head node, not hello IP address, in hello scheduler URL.</span></span> <span data-ttu-id="c6958-167">Om du anger hello IP-adress, visas ett felmeddelande liknande för ”hello servercertifikat måste tooeither har en giltig kedja med förtroende eller toobe placeras i arkivet Betrodda rotcertifikatutfärdare för hello”.</span><span class="sxs-lookup"><span data-stu-id="c6958-167">If you specify hello IP address, an error appears similar too"hello server certificate needs tooeither have a valid chain of trust or toobe placed in hello trusted root store."</span></span>
   > 
   > 
3. <span data-ttu-id="c6958-168">När du uppmanas ange hello användarnamn (i form av hello &lt;DomainName&gt;\\&lt;användarnamn&gt;) och lösenord för hello HPC Klusteradministratören eller en annan användare i klustret som du har konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="c6958-168">When prompted, type hello user name (in hello form &lt;DomainName&gt;\\&lt;UserName&gt;) and password of hello HPC cluster administrator or another cluster user that you configured.</span></span> <span data-ttu-id="c6958-169">Du kan välja toostore hello autentiseringsuppgifter lokalt för flera jobbåtgärder.</span><span class="sxs-lookup"><span data-stu-id="c6958-169">You can choose toostore hello credentials locally for more job operations.</span></span>
   
    <span data-ttu-id="c6958-170">En lista över jobb visas.</span><span class="sxs-lookup"><span data-stu-id="c6958-170">A list of jobs appears.</span></span>

<span data-ttu-id="c6958-171">**toouse HPC Job Manager på hello klientdator**</span><span class="sxs-lookup"><span data-stu-id="c6958-171">**toouse HPC Job Manager on hello client computer**</span></span>

1. <span data-ttu-id="c6958-172">Om du inte tidigare sparar autentiseringsuppgifter för domänen för en användare i klustret när du skickar in ett jobb, du kan lägga till hello autentiseringsuppgifter i Autentiseringshanteraren.</span><span class="sxs-lookup"><span data-stu-id="c6958-172">If you didn't previously store domain credentials for a cluster user when submitting a job, you can add hello credentials in Credential Manager.</span></span>
   
    <span data-ttu-id="c6958-173">a.</span><span class="sxs-lookup"><span data-stu-id="c6958-173">a.</span></span> <span data-ttu-id="c6958-174">Starta Autentiseringshanteraren i Kontrollpanelen på klientdatorn hello.</span><span class="sxs-lookup"><span data-stu-id="c6958-174">In Control Panel on hello client computer, start Credential Manager.</span></span>
   
    <span data-ttu-id="c6958-175">b.</span><span class="sxs-lookup"><span data-stu-id="c6958-175">b.</span></span> <span data-ttu-id="c6958-176">Klicka på **Windows-autentiseringsuppgifter** > **lägga till en allmän autentiseringsuppgift**.</span><span class="sxs-lookup"><span data-stu-id="c6958-176">Click **Windows Credentials** > **Add a generic credential**.</span></span>
   
    <span data-ttu-id="c6958-177">c.</span><span class="sxs-lookup"><span data-stu-id="c6958-177">c.</span></span> <span data-ttu-id="c6958-178">Ange hello Internet-adress (till exempel https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler eller https://&lt;HeadNodeDnsName&gt;.&lt; region&gt;.cloudapp.azure.com/HpcScheduler), och hello användarnamn (&lt;DomainName&gt;\\&lt;användarnamn&gt;) och lösenord för hello Klusteradministratören eller någon annan kluster-användare som du har konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="c6958-178">Specify hello Internet address (for example, https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler or https://&lt;HeadNodeDnsName&gt;.&lt;region&gt;.cloudapp.azure.com/HpcScheduler), and hello user name (&lt;DomainName&gt;\\&lt;UserName&gt;) and password of hello cluster administrator or another cluster user that you configured.</span></span>
2. <span data-ttu-id="c6958-179">Starta HPC Job Manager på hello klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="c6958-179">On hello client computer, start HPC Job Manager.</span></span>
3. <span data-ttu-id="c6958-180">I hello **Välj huvudnod** dialogrutan, typen hello URL toohello huvudnod i Azure (till exempel https://&lt;HeadNodeDnsName&gt;. cloudapp.net eller https://&lt;HeadNodeDnsName&gt;. &lt;region&gt;. cloudapp.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c6958-180">In hello **Select Head Node** dialog box, type hello URL toohello head node in Azure (for example, https://&lt;HeadNodeDnsName&gt;.cloudapp.net or https://&lt;HeadNodeDnsName&gt;.&lt;region&gt;.cloudapp.azure.com).</span></span>
   
    <span data-ttu-id="c6958-181">HPC Job Manager öppnas och visar en lista över jobb på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="c6958-181">HPC Job Manager opens and shows a list of jobs on hello head node.</span></span>

<span data-ttu-id="c6958-182">**toouse hello webbportal som körs på hello huvudnod**</span><span class="sxs-lookup"><span data-stu-id="c6958-182">**toouse hello web portal running on hello head node**</span></span>

1. <span data-ttu-id="c6958-183">Starta en webbläsare på hello klientdatorn och ange något av följande adresser, beroende på hello fullständiga DNS-namnet på hello huvudnod hello:</span><span class="sxs-lookup"><span data-stu-id="c6958-183">Start a web browser on hello client computer, and enter one of hello following addresses, depending on hello full DNS name of hello head node:</span></span>
   
    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal
    ```
   
    <span data-ttu-id="c6958-184">eller</span><span class="sxs-lookup"><span data-stu-id="c6958-184">or</span></span>
   
    ```
    https://<HeadNodeDnsName>.<region>.cloudapp.azure.com/HpcPortal
    ```
2. <span data-ttu-id="c6958-185">Ange hello domänautentiseringsuppgifter för hello HPC Klusteradministratören hello säkerhet dialogrutan som visas.</span><span class="sxs-lookup"><span data-stu-id="c6958-185">In hello security dialog box that appears, type hello domain credentials of hello HPC cluster administrator.</span></span> <span data-ttu-id="c6958-186">(Du kan också lägga till andra användare i klustret i olika roller.</span><span class="sxs-lookup"><span data-stu-id="c6958-186">(You can also add other cluster users in different roles.</span></span> <span data-ttu-id="c6958-187">Se [hantera klustret användare](https://technet.microsoft.com/library/ff919335.aspx).)</span><span class="sxs-lookup"><span data-stu-id="c6958-187">See [Managing Cluster Users](https://technet.microsoft.com/library/ff919335.aspx).)</span></span>
   
    <span data-ttu-id="c6958-188">hello webbportalen öppnas toohello jobbet listvy.</span><span class="sxs-lookup"><span data-stu-id="c6958-188">hello web portal opens toohello job list view.</span></span>
3. <span data-ttu-id="c6958-189">toosubmit ett exempel jobb som lämnar hello strängen ”Hello World” hello klustret, klicka på **nytt jobb** i hello vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="c6958-189">toosubmit a sample job that returns hello string “Hello World” from hello cluster, click **New job** in hello left-hand navigation.</span></span>
4. <span data-ttu-id="c6958-190">På hello **nytt jobb** sidan under **från skicka sidor**, klickar du på **HelloWorld**.</span><span class="sxs-lookup"><span data-stu-id="c6958-190">On hello **New Job** page, under **From submission pages**, click **HelloWorld**.</span></span> <span data-ttu-id="c6958-191">hello jobbet Skicka sida visas.</span><span class="sxs-lookup"><span data-stu-id="c6958-191">hello job submission page appears.</span></span>
5. <span data-ttu-id="c6958-192">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="c6958-192">Click **Submit**.</span></span> <span data-ttu-id="c6958-193">Om du uppmanas ange hello domänautentiseringsuppgifter för hello HPC Klusteradministratören.</span><span class="sxs-lookup"><span data-stu-id="c6958-193">If prompted, provide hello domain credentials of hello HPC cluster administrator.</span></span> <span data-ttu-id="c6958-194">hello jobbet har skickats och hello jobb-ID som visas på hello **Mina jobb** sidan.</span><span class="sxs-lookup"><span data-stu-id="c6958-194">hello job is submitted, and hello job ID appears on hello **My Jobs** page.</span></span>
6. <span data-ttu-id="c6958-195">tooview hello resultaten av hello jobb som du har skickat klickar du på hello jobb-ID och klicka sedan på **uppgiftsvyn** tooview hello kommandoutdata (under **utdata**).</span><span class="sxs-lookup"><span data-stu-id="c6958-195">tooview hello results of hello job that you submitted, click hello job ID, and then click **View Tasks** tooview hello command output (under **Output**).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c6958-196">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c6958-196">Next steps</span></span>
* <span data-ttu-id="c6958-197">Du kan också skicka jobb toohello Azure kluster med hello [HPC Pack REST API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).</span><span class="sxs-lookup"><span data-stu-id="c6958-197">You can also submit jobs toohello Azure cluster with hello [HPC Pack REST API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).</span></span>
* <span data-ttu-id="c6958-198">Om du vill toosubmit kluster-jobb från en Linux-klient, finns hello Python exempel i hello [HPC Pack 2012 R2 SDK och exempelkod](https://www.microsoft.com/download/details.aspx?id=41633).</span><span class="sxs-lookup"><span data-stu-id="c6958-198">If you want toosubmit cluster jobs from a Linux client, see hello Python sample in hello [HPC Pack 2012 R2 SDK and Sample Code](https://www.microsoft.com/download/details.aspx?id=41633).</span></span>

<!--Image references-->
[jobsubmit]: ./media/virtual-machines-windows-hpcpack-cluster-submit-jobs/jobsubmit.png
