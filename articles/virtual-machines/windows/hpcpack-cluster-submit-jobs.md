---
title: Skicka jobb till ett HPC Pack kluster i Azure | Microsoft Docs
description: "Lär dig hur du ställer in en lokal dator att skicka jobb till ett HPC Pack kluster i Azure"
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
ms.openlocfilehash: d5953f1e1dd2deb4d871bd67352a6a5b2ae13dbf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="submit-hpc-jobs-from-an-on-premises-computer-to-an-hpc-pack-cluster-deployed-in-azure"></a><span data-ttu-id="e5db6-103">Registrera HPC-jobb från lokala datorer till ett HPC Pack-kluster som distribuerats i Azure</span><span class="sxs-lookup"><span data-stu-id="e5db6-103">Submit HPC jobs from an on-premises computer to an HPC Pack cluster deployed in Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="e5db6-104">Konfigurera en lokal klientdator för att skicka jobb till en [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) kluster i Azure.</span><span class="sxs-lookup"><span data-stu-id="e5db6-104">Configure an on-premises client computer to submit jobs to a [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) cluster in Azure.</span></span> <span data-ttu-id="e5db6-105">Den här artikeln visar hur du ställer in en lokal dator med klientverktyg att skicka jobbet via HTTPS till klustret i Azure.</span><span class="sxs-lookup"><span data-stu-id="e5db6-105">This article shows you how to set up a local computer with client tools to submit job over HTTPS to the cluster in Azure.</span></span> <span data-ttu-id="e5db6-106">På så sätt kan kan flera kluster-användare skicka jobb till en molnbaserad HPC Pack kluster, utan att ansluta direkt till huvudnoden VM eller få åtkomst till en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e5db6-106">In this way, several cluster users can submit jobs to a cloud-based HPC Pack cluster, but without connecting directly to the head node VM or accessing an Azure subscription.</span></span>

![Skicka ett jobb till ett kluster i Azure][jobsubmit]

## <a name="prerequisites"></a><span data-ttu-id="e5db6-108">Krav</span><span class="sxs-lookup"><span data-stu-id="e5db6-108">Prerequisites</span></span>
* <span data-ttu-id="e5db6-109">**HPC Pack huvudnod distribueras i en Azure VM** -rekommenderar vi att du använder automatiserade verktyg som en [Azure quickstart mallen](https://azure.microsoft.com/documentation/templates/) eller en [Azure PowerShell-skript](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) att distribuera huvudnoden och kluster.</span><span class="sxs-lookup"><span data-stu-id="e5db6-109">**HPC Pack head node deployed in an Azure VM** - We recommend that you use automated tools such as an [Azure quickstart template](https://azure.microsoft.com/documentation/templates/) or an [Azure PowerShell script](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) to deploy the head node and cluster.</span></span> <span data-ttu-id="e5db6-110">Du behöver DNS-namnet på huvudnoden och autentiseringsuppgifterna för en Klusteradministratör för att slutföra stegen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="e5db6-110">You need the DNS name of the head node and the credentials of a cluster administrator to complete the steps in this article.</span></span>
* <span data-ttu-id="e5db6-111">**Klientdatorn** -du behöver en Windows- eller Windows Server-klientdator som kan köra HPC Pack klientverktyg (se [systemkrav](https://technet.microsoft.com/library/dn535781.aspx)).</span><span class="sxs-lookup"><span data-stu-id="e5db6-111">**Client computer** - You need a Windows or Windows Server client computer that can run HPC Pack client utilities (see [system requirements](https://technet.microsoft.com/library/dn535781.aspx)).</span></span> <span data-ttu-id="e5db6-112">Om du endast vill använda HPC Pack webbportal eller REST API för att skicka jobb kan du använda alla klientdatorer som du väljer.</span><span class="sxs-lookup"><span data-stu-id="e5db6-112">If you only want to use the HPC Pack web portal or REST API to submit jobs, you can use any client computer of your choice.</span></span>
* <span data-ttu-id="e5db6-113">**HPC Pack installationsmediet** - om du vill installera HPC Pack klientverktyg, ledigt installationspaketet för den senaste versionen av HPC Pack (HPC Pack 2012 R2) är tillgängliga från den [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024).</span><span class="sxs-lookup"><span data-stu-id="e5db6-113">**HPC Pack installation media** - To install the HPC Pack client utilities, the free installation package for the latest version of HPC Pack (HPC Pack 2012 R2) is available from the [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024).</span></span> <span data-ttu-id="e5db6-114">Kontrollera att du hämtar samma version av HPC Pack som är installerad på huvudnoden VM.</span><span class="sxs-lookup"><span data-stu-id="e5db6-114">Make sure that you download the same version of HPC Pack that is installed on the head node VM.</span></span>

## <a name="step-1-install-and-configure-the-web-components-on-the-head-node"></a><span data-ttu-id="e5db6-115">Steg 1: Installera och konfigurera webbkomponenterna på huvudnoden</span><span class="sxs-lookup"><span data-stu-id="e5db6-115">Step 1: Install and configure the web components on the head node</span></span>
<span data-ttu-id="e5db6-116">Se till att webbkomponenterna HPC Pack konfigureras på huvudnoden HPC Pack om du vill aktivera ett REST-gränssnitt att skicka jobb till klustret via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e5db6-116">To enable a REST interface to submit jobs to the cluster over HTTPS, ensure that the HPC Pack web components are configured on the HPC Pack head node.</span></span> <span data-ttu-id="e5db6-117">Om de inte redan har installerats måste du först installera komponenterna genom att köra HpcWebComponents.msi installationsfilen.</span><span class="sxs-lookup"><span data-stu-id="e5db6-117">If they aren't already installed, first install the web components by running the HpcWebComponents.msi installation file.</span></span> <span data-ttu-id="e5db6-118">Konfigurera sedan komponenterna genom att köra HPC PowerShell-skriptet **Set HPCWebComponents.ps1**.</span><span class="sxs-lookup"><span data-stu-id="e5db6-118">Then, configure the components by running the HPC PowerShell script **Set-HPCWebComponents.ps1**.</span></span>

<span data-ttu-id="e5db6-119">Detaljerade anvisningar finns [installera Microsoft HPC Pack Webbkomponenter](http://technet.microsoft.com/library/hh314627.aspx).</span><span class="sxs-lookup"><span data-stu-id="e5db6-119">For detailed procedures, see [Install the Microsoft HPC Pack Web Components](http://technet.microsoft.com/library/hh314627.aspx).</span></span>

> [!TIP]
> <span data-ttu-id="e5db6-120">Vissa mallar för Azure quickstart för HPC Pack installera och konfigurera webbkomponenterna automatiskt.</span><span class="sxs-lookup"><span data-stu-id="e5db6-120">Certain Azure quickstart templates for HPC Pack install and configure the web components automatically.</span></span> <span data-ttu-id="e5db6-121">Om du använder den [HPC Pack IaaS distributionsskriptet](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) för att skapa klustret, kan du eventuellt installerar och konfigurerar webbkomponenterna som en del av distributionen.</span><span class="sxs-lookup"><span data-stu-id="e5db6-121">If you use the [HPC Pack IaaS deployment script](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) to create the cluster, you can optionally install and configure the web components as part of the deployment.</span></span>
> 
> 

<span data-ttu-id="e5db6-122">**Att installera webbkomponenter**</span><span class="sxs-lookup"><span data-stu-id="e5db6-122">**To install the web components**</span></span>

1. <span data-ttu-id="e5db6-123">Ansluta till huvudnod VM med hjälp av autentiseringsuppgifterna för en Klusteradministratör.</span><span class="sxs-lookup"><span data-stu-id="e5db6-123">Connect to the head node VM by using the credentials of a cluster administrator.</span></span>
2. <span data-ttu-id="e5db6-124">Kör HpcWebComponents.msi på huvudnoden från HPC Pack installationsmapp.</span><span class="sxs-lookup"><span data-stu-id="e5db6-124">From the HPC Pack Setup folder, run HpcWebComponents.msi on the head node.</span></span>
3. <span data-ttu-id="e5db6-125">Följ stegen i guiden för att installera webbkomponenter</span><span class="sxs-lookup"><span data-stu-id="e5db6-125">Follow the steps in the wizard to install the web components</span></span>

<span data-ttu-id="e5db6-126">**Så här konfigurerar du webbkomponenterna**</span><span class="sxs-lookup"><span data-stu-id="e5db6-126">**To configure the web components**</span></span>

1. <span data-ttu-id="e5db6-127">Starta HPC PowerShell som administratör på huvudnoden.</span><span class="sxs-lookup"><span data-stu-id="e5db6-127">On the head node, start HPC PowerShell as an administrator.</span></span>
2. <span data-ttu-id="e5db6-128">Om du vill ändra katalogen till platsen för konfigurationsskript, skriver du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e5db6-128">To change directory to the location of the configuration script, type the following command:</span></span>
   
    ```powershell
    cd $env:CCP_HOME\bin
    ```
3. <span data-ttu-id="e5db6-129">Om du vill konfigurera REST-gränssnittet och starta HPC-webbtjänsten, Skriv följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e5db6-129">To configure the REST interface and start the HPC Web Service, type the following command:</span></span>
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```
4. <span data-ttu-id="e5db6-130">Välj det certifikat som motsvarar det offentliga DNS-namnet på huvudnoden när du uppmanas att välja ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="e5db6-130">When prompted to select a certificate, choose the certificate that corresponds to the public DNS name of the head node.</span></span> <span data-ttu-id="e5db6-131">Till exempel om du distribuerar huvudnod VM som använder den klassiska distributionsmodellen, certifikatnamnet ser ut som CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="e5db6-131">For example, if you deploy the head node VM using the classic deployment model, the certificate name looks like CN=&lt;*HeadNodeDnsName*&gt;.cloudapp.net.</span></span> <span data-ttu-id="e5db6-132">Om du använder Resource Manager-distributionsmodellen, certifikatnamnet ser ut som CN =&lt;*HeadNodeDnsName*&gt;.&lt; *region*&gt;. cloudapp.azure.com.</span><span class="sxs-lookup"><span data-stu-id="e5db6-132">If you use the Resource Manager deployment model, the certificate name looks like CN=&lt;*HeadNodeDnsName*&gt;.&lt;*region*&gt;.cloudapp.azure.com.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="e5db6-133">Väljer du det här certifikatet senare när du skicka jobb till huvudnoden från en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="e5db6-133">You select this certificate later when you submit jobs to the head node from an on-premises computer.</span></span> <span data-ttu-id="e5db6-134">Inte markera eller konfigurera ett certifikat som motsvarar namnet på huvudnod i Active Directory-domän (till exempel CN =*MyHPCHeadNode.HpcAzure.local*).</span><span class="sxs-lookup"><span data-stu-id="e5db6-134">Don't select or configure a certificate that corresponds to the computer name of the head node in the Active Directory domain (for example, CN=*MyHPCHeadNode.HpcAzure.local*).</span></span>
   > 
   > 
5. <span data-ttu-id="e5db6-135">Om du vill konfigurera webbportalen för jobbet, skriver du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e5db6-135">To configure the web portal for job submission, type the following command:</span></span>
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
6. <span data-ttu-id="e5db6-136">När skriptet har slutförts, stoppa och starta om tjänsten för Rapportjobbschemaläggning i HPC-kluster genom att skriva följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="e5db6-136">After the script completes, stop and restart the HPC Job Scheduler Service by typing the following commands:</span></span>
   
    ```powershell
    net stop hpcscheduler
    net start hpcscheduler
    ```

## <a name="step-2-install-the-hpc-pack-client-utilities-on-an-on-premises-computer"></a><span data-ttu-id="e5db6-137">Steg 2: Installera klientverktyg HPC Pack på en lokal dator</span><span class="sxs-lookup"><span data-stu-id="e5db6-137">Step 2: Install the HPC Pack client utilities on an on-premises computer</span></span>
<span data-ttu-id="e5db6-138">Om du vill installera klientverktyg HPC Pack på datorn hämta HPC Pack-installationsfiler (fullständig installation) från den [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024).</span><span class="sxs-lookup"><span data-stu-id="e5db6-138">If you want to install the HPC Pack client utilities on your computer, download the HPC Pack setup files (full installation) from the [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024).</span></span> <span data-ttu-id="e5db6-139">När du påbörjar installationen, välja installationsprogrammet för den **HPC Pack klientverktyg**.</span><span class="sxs-lookup"><span data-stu-id="e5db6-139">When you begin the installation, choose the setup option for the **HPC Pack client utilities**.</span></span>

<span data-ttu-id="e5db6-140">Om du vill använda klientverktygen HPC Pack för att skicka jobb till huvudnoden VM, måste du också exportera ett certifikat från huvudnod och installera den på klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="e5db6-140">To use the HPC Pack client tools to submit jobs to the head node VM, you also need to export a certificate from the head node and install it on the client computer.</span></span> <span data-ttu-id="e5db6-141">Certifikatet måste vara i. CER-format.</span><span class="sxs-lookup"><span data-stu-id="e5db6-141">The certificate must be in .CER format.</span></span>

<span data-ttu-id="e5db6-142">**Exportera certifikatet från huvudnod**</span><span class="sxs-lookup"><span data-stu-id="e5db6-142">**To export the certificate from the head node**</span></span>

1. <span data-ttu-id="e5db6-143">Lägg till snapin-modulen certifikat till en Microsoft Management Console för det lokala datorkontot på huvudnoden.</span><span class="sxs-lookup"><span data-stu-id="e5db6-143">On the head node, add the Certificates snap-in to a Microsoft Management Console for the Local Computer account.</span></span> <span data-ttu-id="e5db6-144">Anvisningar för hur du lägger till snapin-modulen finns [lägga till snapin-modulen certifikat i en MMC](https://technet.microsoft.com/library/cc754431.aspx).</span><span class="sxs-lookup"><span data-stu-id="e5db6-144">For steps to add the snap-in, see [Add the Certificates Snap-in to an MMC](https://technet.microsoft.com/library/cc754431.aspx).</span></span>
2. <span data-ttu-id="e5db6-145">I konsolträdet expanderar **certifikat – lokal dator** > **personliga**, och klicka sedan på **certifikat**.</span><span class="sxs-lookup"><span data-stu-id="e5db6-145">In the console tree, expand **Certificates – Local Computer** > **Personal**, and then click **Certificates**.</span></span>
3. <span data-ttu-id="e5db6-146">Leta upp det certifikat som du har konfigurerat för HPC Pack webbkomponenterna i [steg 1: Installera och konfigurera webbkomponenterna på huvudnoden](#step-1:-install-and-configure-the-web-components-on-the-head-node) (exempelvis, CN =&lt;*HeadNodeDnsName* &gt;. cloudapp.net).</span><span class="sxs-lookup"><span data-stu-id="e5db6-146">Locate the certificate that you configured for the HPC Pack web components in [Step 1: Install and configure the web components on the head node](#step-1:-install-and-configure-the-web-components-on-the-head-node) (for example, CN=&lt;*HeadNodeDnsName*&gt;.cloudapp.net).</span></span>
4. <span data-ttu-id="e5db6-147">Högerklicka på certifikatet och klickar på **alla aktiviteter** > **exportera**.</span><span class="sxs-lookup"><span data-stu-id="e5db6-147">Right-click the certificate, and click **All Tasks** > **Export**.</span></span>
5. <span data-ttu-id="e5db6-148">I guiden Exportera certifikat klickar du på **nästa**, och kontrollera att **Nej, exportera inte den privata nyckeln** är markerad.</span><span class="sxs-lookup"><span data-stu-id="e5db6-148">In the Certificate Export Wizard, click **Next**, and ensure that **No, do not export the private key** is selected.</span></span>
6. <span data-ttu-id="e5db6-149">Följ stegen i guiden för att exportera certifikatet i DER-kodad binärfil X.509 (. CER)-format.</span><span class="sxs-lookup"><span data-stu-id="e5db6-149">Follow the remaining steps of the wizard to export the certificate in DER encoded binary X.509 (.CER) format.</span></span>

<span data-ttu-id="e5db6-150">**Importera certifikatet på klientdatorn**</span><span class="sxs-lookup"><span data-stu-id="e5db6-150">**To import the certificate on the client computer**</span></span>

1. <span data-ttu-id="e5db6-151">Kopiera det certifikat som du exporterade från huvudnod till en mapp på klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="e5db6-151">Copy the certificate that you exported from the head node to a folder on the client computer.</span></span>
2. <span data-ttu-id="e5db6-152">Kör certmgr.msc på klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="e5db6-152">On the client computer, run certmgr.msc.</span></span>
3. <span data-ttu-id="e5db6-153">Expandera i Certifikathanteraren **certifikat – aktuell användare** > **betrodda rotcertifikatutfärdare**, högerklicka på **certifikat**, och sedan Klicka på **alla aktiviteter** > **importera**.</span><span class="sxs-lookup"><span data-stu-id="e5db6-153">In Certificate Manager, expand **Certificates – Current user** > **Trusted Root Certification Authorities**, right-click **Certificates**, and then click **All Tasks** > **Import**.</span></span>
4. <span data-ttu-id="e5db6-154">I guiden Importera certifikat klickar du på **nästa** och följ stegen för att importera certifikatet som du exporterade från huvudnod i arkivet för betrodda rotcertifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="e5db6-154">In the Certificate Import Wizard, click **Next** and follow the steps to import the certificate that you exported from the head node to the Trusted Root Certification Authorities store.</span></span>

> [!TIP]
> <span data-ttu-id="e5db6-155">Du kan se en säkerhetsvarning, eftersom certifikatutfärdare på huvudnoden känner inte igen av klienten.</span><span class="sxs-lookup"><span data-stu-id="e5db6-155">You might see a security warning, because the certification authority on the head node isn't recognized by the client computer.</span></span> <span data-ttu-id="e5db6-156">I testsyfte kan du ignorera den här varningen och slutföra Importera certifikat.</span><span class="sxs-lookup"><span data-stu-id="e5db6-156">For testing purposes, you can ignore this warning and complete the certificate import.</span></span>
> 
> 

## <a name="step-3-run-test-jobs-on-the-cluster"></a><span data-ttu-id="e5db6-157">Steg 3: Kör testjobb på klustret</span><span class="sxs-lookup"><span data-stu-id="e5db6-157">Step 3: Run test jobs on the cluster</span></span>
<span data-ttu-id="e5db6-158">Om du vill verifiera konfigurationen, försök att köra jobb på klustret i Azure från den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="e5db6-158">To verify your configuration, try running jobs on the cluster in Azure from the on-premises computer.</span></span> <span data-ttu-id="e5db6-159">Du kan till exempel använda HPC Pack GUI-verktyg och kommandon på kommandoraden för att skicka jobb till klustret.</span><span class="sxs-lookup"><span data-stu-id="e5db6-159">For example, you can use HPC Pack GUI tools or command-line commands to submit jobs to the cluster.</span></span> <span data-ttu-id="e5db6-160">Du kan också använda en webbaserad portal för att skicka jobb.</span><span class="sxs-lookup"><span data-stu-id="e5db6-160">You can also use a web-based portal to submit jobs.</span></span>

<span data-ttu-id="e5db6-161">**Att köra jobbet skicka kommandon på klientdatorn**</span><span class="sxs-lookup"><span data-stu-id="e5db6-161">**To run job submission commands on the client computer**</span></span>

1. <span data-ttu-id="e5db6-162">Starta Kommandotolken på en klientdator där klientverktyg HPC Pack installeras.</span><span class="sxs-lookup"><span data-stu-id="e5db6-162">On a client computer where the HPC Pack client utilities are installed, start a Command Prompt.</span></span>
2. <span data-ttu-id="e5db6-163">Ange en Exempelkommando.</span><span class="sxs-lookup"><span data-stu-id="e5db6-163">Type a sample command.</span></span> <span data-ttu-id="e5db6-164">Om du vill visa en lista över alla jobb i klustret, exempelvis ett kommando som liknar följande, beroende på det fullständiga DNS-namnet på huvudnoden:</span><span class="sxs-lookup"><span data-stu-id="e5db6-164">For example, to list all jobs on the cluster, type a command similar to one of the following, depending on the full DNS name of the head node:</span></span>
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all
    ```
   
    <span data-ttu-id="e5db6-165">eller</span><span class="sxs-lookup"><span data-stu-id="e5db6-165">or</span></span>
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.<region>.cloudapp.azure.com /all
    ```
   
   > [!TIP]
   > <span data-ttu-id="e5db6-166">Använd det fullständiga DNS-namnet på huvudnod inte IP-adress, i scheduler-URL.</span><span class="sxs-lookup"><span data-stu-id="e5db6-166">Use the full DNS name of the head node, not the IP address, in the scheduler URL.</span></span> <span data-ttu-id="e5db6-167">Om du anger IP-adressen, visas ett felmeddelande liknande ”servercertifikatet måste antingen ha en giltig certifikatkedja eller ska placeras i arkivet Betrodda rotcertifikatutfärdare”.</span><span class="sxs-lookup"><span data-stu-id="e5db6-167">If you specify the IP address, an error appears similar to "The server certificate needs to either have a valid chain of trust or to be placed in the trusted root store."</span></span>
   > 
   > 
3. <span data-ttu-id="e5db6-168">När du uppmanas, anger användarnamnet (i formatet &lt;DomainName&gt;\\&lt;användarnamn&gt;) och lösenord för administratören för HPC-kluster eller en annan användare i klustret som du har konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="e5db6-168">When prompted, type the user name (in the form &lt;DomainName&gt;\\&lt;UserName&gt;) and password of the HPC cluster administrator or another cluster user that you configured.</span></span> <span data-ttu-id="e5db6-169">Du kan välja att lagra autentiseringsuppgifter lokalt för flera jobbåtgärder.</span><span class="sxs-lookup"><span data-stu-id="e5db6-169">You can choose to store the credentials locally for more job operations.</span></span>
   
    <span data-ttu-id="e5db6-170">En lista över jobb visas.</span><span class="sxs-lookup"><span data-stu-id="e5db6-170">A list of jobs appears.</span></span>

<span data-ttu-id="e5db6-171">**Du använder HPC Job Manager på klientdatorn**</span><span class="sxs-lookup"><span data-stu-id="e5db6-171">**To use HPC Job Manager on the client computer**</span></span>

1. <span data-ttu-id="e5db6-172">Om du inte tidigare sparar autentiseringsuppgifter för domänen för en användare i klustret när du skickar in ett jobb, du kan lägga till autentiseringsuppgifter i Autentiseringshanteraren.</span><span class="sxs-lookup"><span data-stu-id="e5db6-172">If you didn't previously store domain credentials for a cluster user when submitting a job, you can add the credentials in Credential Manager.</span></span>
   
    <span data-ttu-id="e5db6-173">a.</span><span class="sxs-lookup"><span data-stu-id="e5db6-173">a.</span></span> <span data-ttu-id="e5db6-174">Starta Autentiseringshanteraren i Kontrollpanelen på klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="e5db6-174">In Control Panel on the client computer, start Credential Manager.</span></span>
   
    <span data-ttu-id="e5db6-175">b.</span><span class="sxs-lookup"><span data-stu-id="e5db6-175">b.</span></span> <span data-ttu-id="e5db6-176">Klicka på **Windows-autentiseringsuppgifter** > **lägga till en allmän autentiseringsuppgift**.</span><span class="sxs-lookup"><span data-stu-id="e5db6-176">Click **Windows Credentials** > **Add a generic credential**.</span></span>
   
    <span data-ttu-id="e5db6-177">c.</span><span class="sxs-lookup"><span data-stu-id="e5db6-177">c.</span></span> <span data-ttu-id="e5db6-178">Ange Internet-adress (till exempel https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler eller https://&lt;HeadNodeDnsName&gt;.&lt; region&gt;.cloudapp.azure.com/HpcScheduler), och användarnamn (&lt;DomainName&gt;\\&lt;användarnamn&gt;) och lösenord för Klusteradministratören eller någon annan kluster-användare som du har konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="e5db6-178">Specify the Internet address (for example, https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler or https://&lt;HeadNodeDnsName&gt;.&lt;region&gt;.cloudapp.azure.com/HpcScheduler), and the user name (&lt;DomainName&gt;\\&lt;UserName&gt;) and password of the cluster administrator or another cluster user that you configured.</span></span>
2. <span data-ttu-id="e5db6-179">Starta HPC Job Manager på klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="e5db6-179">On the client computer, start HPC Job Manager.</span></span>
3. <span data-ttu-id="e5db6-180">I den **Välj huvudnod** dialogrutan, ange Webbadressen till huvudnod i Azure (till exempel https://&lt;HeadNodeDnsName&gt;. cloudapp.net eller https://&lt;HeadNodeDnsName&gt;.&lt; region&gt;. cloudapp.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e5db6-180">In the **Select Head Node** dialog box, type the URL to the head node in Azure (for example, https://&lt;HeadNodeDnsName&gt;.cloudapp.net or https://&lt;HeadNodeDnsName&gt;.&lt;region&gt;.cloudapp.azure.com).</span></span>
   
    <span data-ttu-id="e5db6-181">HPC Job Manager öppnas och visar en lista över jobb på huvudnoden.</span><span class="sxs-lookup"><span data-stu-id="e5db6-181">HPC Job Manager opens and shows a list of jobs on the head node.</span></span>

<span data-ttu-id="e5db6-182">**Att använda webbportalen körs på huvudnoden**</span><span class="sxs-lookup"><span data-stu-id="e5db6-182">**To use the web portal running on the head node**</span></span>

1. <span data-ttu-id="e5db6-183">Starta en webbläsare på klientdatorn och ange ett av följande adresser, beroende på det fullständiga DNS-namnet på huvudnoden:</span><span class="sxs-lookup"><span data-stu-id="e5db6-183">Start a web browser on the client computer, and enter one of the following addresses, depending on the full DNS name of the head node:</span></span>
   
    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal
    ```
   
    <span data-ttu-id="e5db6-184">eller</span><span class="sxs-lookup"><span data-stu-id="e5db6-184">or</span></span>
   
    ```
    https://<HeadNodeDnsName>.<region>.cloudapp.azure.com/HpcPortal
    ```
2. <span data-ttu-id="e5db6-185">Ange autentiseringsuppgifter för domänen av administratör för HPC-kluster i dialogrutan säkerhet.</span><span class="sxs-lookup"><span data-stu-id="e5db6-185">In the security dialog box that appears, type the domain credentials of the HPC cluster administrator.</span></span> <span data-ttu-id="e5db6-186">(Du kan också lägga till andra användare i klustret i olika roller.</span><span class="sxs-lookup"><span data-stu-id="e5db6-186">(You can also add other cluster users in different roles.</span></span> <span data-ttu-id="e5db6-187">Se [hantera klustret användare](https://technet.microsoft.com/library/ff919335.aspx).)</span><span class="sxs-lookup"><span data-stu-id="e5db6-187">See [Managing Cluster Users](https://technet.microsoft.com/library/ff919335.aspx).)</span></span>
   
    <span data-ttu-id="e5db6-188">Webbportalen öppnar listvyn jobb.</span><span class="sxs-lookup"><span data-stu-id="e5db6-188">The web portal opens to the job list view.</span></span>
3. <span data-ttu-id="e5db6-189">För att skicka ett prov jobb som returnerar strängen ”Hello World” från klustret, klickar du på **nytt jobb** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="e5db6-189">To submit a sample job that returns the string “Hello World” from the cluster, click **New job** in the left-hand navigation.</span></span>
4. <span data-ttu-id="e5db6-190">På den **nytt jobb** sidan under **från skicka sidor**, klickar du på **HelloWorld**.</span><span class="sxs-lookup"><span data-stu-id="e5db6-190">On the **New Job** page, under **From submission pages**, click **HelloWorld**.</span></span> <span data-ttu-id="e5db6-191">Sidan skicka jobbet visas.</span><span class="sxs-lookup"><span data-stu-id="e5db6-191">The job submission page appears.</span></span>
5. <span data-ttu-id="e5db6-192">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="e5db6-192">Click **Submit**.</span></span> <span data-ttu-id="e5db6-193">Om du uppmanas ange autentiseringsuppgifter för domänen av administratör för HPC-kluster.</span><span class="sxs-lookup"><span data-stu-id="e5db6-193">If prompted, provide the domain credentials of the HPC cluster administrator.</span></span> <span data-ttu-id="e5db6-194">Jobbet har skickats och jobb-ID som visas på den **Mina jobb** sidan.</span><span class="sxs-lookup"><span data-stu-id="e5db6-194">The job is submitted, and the job ID appears on the **My Jobs** page.</span></span>
6. <span data-ttu-id="e5db6-195">Om du vill visa resultatet av jobbet som du har skickat klickar du på jobb-ID och klicka sedan på **uppgiftsvyn** att visa kommandoutdata (under **utdata**).</span><span class="sxs-lookup"><span data-stu-id="e5db6-195">To view the results of the job that you submitted, click the job ID, and then click **View Tasks** to view the command output (under **Output**).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e5db6-196">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e5db6-196">Next steps</span></span>
* <span data-ttu-id="e5db6-197">Du kan också skicka jobb till Azure klustret med det [HPC Pack REST API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).</span><span class="sxs-lookup"><span data-stu-id="e5db6-197">You can also submit jobs to the Azure cluster with the [HPC Pack REST API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).</span></span>
* <span data-ttu-id="e5db6-198">Om du vill skicka kluster-jobb från en Linux-klient finns i Python-exempel i den [HPC Pack 2012 R2 SDK och exempelkod](https://www.microsoft.com/download/details.aspx?id=41633).</span><span class="sxs-lookup"><span data-stu-id="e5db6-198">If you want to submit cluster jobs from a Linux client, see the Python sample in the [HPC Pack 2012 R2 SDK and Sample Code](https://www.microsoft.com/download/details.aspx?id=41633).</span></span>

<!--Image references-->
[jobsubmit]: ./media/virtual-machines-windows-hpcpack-cluster-submit-jobs/jobsubmit.png
