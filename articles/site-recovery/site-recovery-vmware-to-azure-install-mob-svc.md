---
title: "aaaInstall Mobilitetstjänsten (VMware eller fysiska tooAzure) | Microsoft Docs"
description: "Lär dig hur hello tooinstall Mobilitetstjänsten agent tooprotect lokala datorer."
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
ms.workload: backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: f7836e6b35d3838bae1eff927838ce4b245b9f56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-mobility-service-vmware-or-physical-tooazure"></a><span data-ttu-id="cae81-103">Installera Mobilitetstjänsten (VMware eller fysiska tooAzure)</span><span class="sxs-lookup"><span data-stu-id="cae81-103">Install Mobility Service (VMware or physical tooAzure)</span></span>
<span data-ttu-id="cae81-104">Azure Site Recovery-Mobilitetstjänsten samlar in dataskrivningar på en dator och vidarebefordrar dem toohello processervern.</span><span class="sxs-lookup"><span data-stu-id="cae81-104">Azure Site Recovery Mobility Service captures data writes on a computer, and then forwards them toohello process server.</span></span> <span data-ttu-id="cae81-105">Distribuera Mobilitetstjänsten tooevery dator (VMware VM eller fysiska servrar som du vill tooreplicate tooAzure).</span><span class="sxs-lookup"><span data-stu-id="cae81-105">Deploy Mobility Service tooevery computer (VMware VM or physical server) that you want tooreplicate tooAzure.</span></span> <span data-ttu-id="cae81-106">Du kan distribuera Mobilitetstjänsten toohello servrar som du vill tooprotect med hjälp av hello följande metoder:</span><span class="sxs-lookup"><span data-stu-id="cae81-106">You can deploy Mobility Service toohello servers that you want tooprotect by using hello following methods:</span></span>


* [<span data-ttu-id="cae81-107">Installera Mobilitetstjänsten med hjälp av verktyg för programvarudistribution som System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="cae81-107">Install Mobility Service by using software deployment tools like System Center Configuration Manager</span></span>](site-recovery-install-mobility-service-using-sccm.md)
* [<span data-ttu-id="cae81-108">Installera Mobilitetstjänsten med hjälp av Azure Automation och Desired State Configuration Automation DSC)</span><span class="sxs-lookup"><span data-stu-id="cae81-108">Install Mobility Service by using Azure Automation and Desired State Configuration (Automation DSC)</span></span>](site-recovery-automate-mobility-service-install.md)
* [<span data-ttu-id="cae81-109">Installera Mobilitetstjänsten manuellt med hjälp av hello grafiskt användargränssnitt (GUI)</span><span class="sxs-lookup"><span data-stu-id="cae81-109">Install Mobility Service manually by using hello graphical user interface (GUI)</span></span>](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui)
* [<span data-ttu-id="cae81-110">Installera Mobilitetstjänsten manuellt i en kommandotolk</span><span class="sxs-lookup"><span data-stu-id="cae81-110">Install Mobility Service manually at a command prompt</span></span>](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt)
* [<span data-ttu-id="cae81-111">Installera Mobilitetstjänsten genom push-installation från Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="cae81-111">Install Mobility Service by push installation from Azure Site Recovery</span></span>](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery)


>[!IMPORTANT]
> <span data-ttu-id="cae81-112">Från och med version 9.7.0.0, på Windows-datorer (VM), hello Mobilitetstjänsten installerar installationsprogrammet också hello senaste tillgängliga [Virtuella Azure-datoragenten](../virtual-machines/windows/extensions-features.md#azure-vm-agent).</span><span class="sxs-lookup"><span data-stu-id="cae81-112">Beginning with version 9.7.0.0, on Windows virtual machines (VMs), hello Mobility Service installer also installs hello latest available [Azure VM agent](../virtual-machines/windows/extensions-features.md#azure-vm-agent).</span></span> <span data-ttu-id="cae81-113">När en dator växlar tooAzure, uppfyller hello datorn hello agentinstallation som är nödvändiga för att använda alla VM-tillägg.</span><span class="sxs-lookup"><span data-stu-id="cae81-113">When a computer fails over tooAzure, hello computer meets hello agent installation prerequisite for using any VM extension.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cae81-114">Krav</span><span class="sxs-lookup"><span data-stu-id="cae81-114">Prerequisites</span></span>
<span data-ttu-id="cae81-115">Gör följande förutsättningar innan du installerar Mobilitetstjänsten manuellt på servern:</span><span class="sxs-lookup"><span data-stu-id="cae81-115">Complete these prerequisite steps before you manually install Mobility Service on your server:</span></span>
1. <span data-ttu-id="cae81-116">Logga in tooyour konfigurationsservern och öppna ett kommandotolksfönster som administratör.</span><span class="sxs-lookup"><span data-stu-id="cae81-116">Sign in tooyour configuration server, and then open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="cae81-117">Ändra hello directory toohello bin-mappen och sedan skapa en lösenfras fil:</span><span class="sxs-lookup"><span data-stu-id="cae81-117">Change hello directory toohello bin folder, and then create a passphrase file:</span></span>

    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    genpassphrase.exe -v > MobSvc.passphrase
    ```
3. <span data-ttu-id="cae81-118">Lagra hello lösenfras på en säker plats.</span><span class="sxs-lookup"><span data-stu-id="cae81-118">Store hello passphrase file in a secure location.</span></span> <span data-ttu-id="cae81-119">Du kan använda hello fil under hello installation av Mobilitetstjänsten.</span><span class="sxs-lookup"><span data-stu-id="cae81-119">You use hello file during hello Mobility Service installation.</span></span>
4. <span data-ttu-id="cae81-120">Mobility Service installationsprogram för alla operativsystem som stöds finns i hello %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository mappen.</span><span class="sxs-lookup"><span data-stu-id="cae81-120">Mobility Service installers for all supported operating systems are in hello %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository folder.</span></span>

### <a name="mobility-service-installer-to-operating-system-mapping"></a><span data-ttu-id="cae81-121">Mobility-installationsprogram för operativsystem systemmappning</span><span class="sxs-lookup"><span data-stu-id="cae81-121">Mobility Service installer-to-operating system mapping</span></span>

| <span data-ttu-id="cae81-122">Installer filnamn för mallen</span><span class="sxs-lookup"><span data-stu-id="cae81-122">Installer file template name</span></span>| <span data-ttu-id="cae81-123">Operativsystem</span><span class="sxs-lookup"><span data-stu-id="cae81-123">Operating system</span></span> |
|---|--|
|<span data-ttu-id="cae81-124">Microsoft ASR\_UA\*Windows\*release.exe</span><span class="sxs-lookup"><span data-stu-id="cae81-124">Microsoft-ASR\_UA\*Windows\*release.exe</span></span> | <span data-ttu-id="cae81-125">Windows Server 2008 R2 SP1 (64-bitars)</span><span class="sxs-lookup"><span data-stu-id="cae81-125">Windows Server 2008 R2 SP1 (64-bit)</span></span> </br> <span data-ttu-id="cae81-126">Windows Server 2012 (64-bitars)</span><span class="sxs-lookup"><span data-stu-id="cae81-126">Windows Server 2012 (64-bit)</span></span> </br> <span data-ttu-id="cae81-127">Windows Server 2012 R2 (64-bitars)</span><span class="sxs-lookup"><span data-stu-id="cae81-127">Windows Server 2012 R2 (64-bit)</span></span> |
|<span data-ttu-id="cae81-128">Microsoft ASR\_UA\*RHEL6 64*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="cae81-128">Microsoft-ASR\_UA\*RHEL6-64*release.tar.gz</span></span>| <span data-ttu-id="cae81-129">Red Hat Enterprise Linux (RHEL) 6.4, 6.5, 6.6, 6.7, 6.8 (endast 64-bitars)</span><span class="sxs-lookup"><span data-stu-id="cae81-129">Red Hat Enterprise Linux (RHEL) 6.4, 6.5, 6.6, 6.7, 6.8 (64-bit only)</span></span> </br> <span data-ttu-id="cae81-130">CentOS 6.4, 6.5, 6.6, 6.7, 6.8 (endast 64-bitars)</span><span class="sxs-lookup"><span data-stu-id="cae81-130">CentOS 6.4, 6.5, 6.6, 6.7, 6.8 (64-bit only)</span></span> |
|<span data-ttu-id="cae81-131">Microsoft ASR\_UA\*RHEL7 64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="cae81-131">Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz</span></span> | <span data-ttu-id="cae81-132">Red Hat Enterprise Linux (RHEL) 7.1, 7.2 (endast 64-bitars)</span><span class="sxs-lookup"><span data-stu-id="cae81-132">Red Hat Enterprise Linux (RHEL) 7.1, 7.2 (64-bit only)</span></span> </br> <span data-ttu-id="cae81-133">CentOS 7.0, 7.1, 7.2 (endast 64-bitars)</span><span class="sxs-lookup"><span data-stu-id="cae81-133">CentOS 7.0, 7.1, 7.2 (64-bit only)</span></span></br> <span data-ttu-id="cae81-134">CentOs 7.3 (migrering)</span><span class="sxs-lookup"><span data-stu-id="cae81-134">CentOs 7.3 (migration only)</span></span> |
|<span data-ttu-id="cae81-135">Microsoft ASR\_UA\*SLES11-SP3-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="cae81-135">Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz</span></span>| <span data-ttu-id="cae81-136">SUSE Linux Enterprise Server 11 SP3 (endast 64-bitars)</span><span class="sxs-lookup"><span data-stu-id="cae81-136">SUSE Linux Enterprise Server 11 SP3 (64-bit only)</span></span>|
|<span data-ttu-id="cae81-137">Microsoft ASR\_UA\*SLES11-SP4-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="cae81-137">Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz</span></span>| <span data-ttu-id="cae81-138">SUSE Linux Enterprise Server 11 SP4 (endast 64-bitars)</span><span class="sxs-lookup"><span data-stu-id="cae81-138">SUSE Linux Enterprise Server 11 SP4 (64-bit only)</span></span>|
|<span data-ttu-id="cae81-139">Microsoft ASR\_UA\*OL6 64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="cae81-139">Microsoft-ASR\_UA\*OL6-64\*release.tar.gz</span></span> | <span data-ttu-id="cae81-140">Oracle Enterprise Linux 6.4, 6.5 (endast 64-bitars)</span><span class="sxs-lookup"><span data-stu-id="cae81-140">Oracle Enterprise Linux 6.4, 6.5 (64-bit only)</span></span>|
|<span data-ttu-id="cae81-141">Microsoft ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="cae81-141">Microsoft-ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz</span></span> | <span data-ttu-id="cae81-142">Ubuntu Linux 14.04 (endast 64-bitars)</span><span class="sxs-lookup"><span data-stu-id="cae81-142">Ubuntu Linux 14.04 (64-bit only)</span></span>|


## <a name="install-mobility-service-manually-by-using-hello-gui"></a><span data-ttu-id="cae81-143">Installera Mobilitetstjänsten manuellt med hjälp av hello GUI</span><span class="sxs-lookup"><span data-stu-id="cae81-143">Install Mobility Service manually by using hello GUI</span></span>

>[!IMPORTANT]
> <span data-ttu-id="cae81-144">Om du använder en **konfigurationsservern** tooreplicate **Azure IaaS-virtuella datorer** från en Azure-prenumeration/Region tooanother sedan **använda hello baserat kommandoradsinstallation**  metod</span><span class="sxs-lookup"><span data-stu-id="cae81-144">If you are using a **Configuration Server** tooreplicate **Azure IaaS virtual machines** from one Azure Subscription/Region tooanother then **use hello Command-line based installation** method</span></span>

[!INCLUDE [site-recovery-install-mob-svc-gui](../../includes/site-recovery-install-mob-svc-gui.md)]

## <a name="install-mobility-service-manually-at-a-command-prompt"></a><span data-ttu-id="cae81-145">Installera Mobilitetstjänsten manuellt i en kommandotolk</span><span class="sxs-lookup"><span data-stu-id="cae81-145">Install Mobility Service manually at a command prompt</span></span>

### <a name="command-line-installation-on-a-windows-computer"></a><span data-ttu-id="cae81-146">Kommandoradsparametrarna för installation på en Windows-dator</span><span class="sxs-lookup"><span data-stu-id="cae81-146">Command-line installation on a Windows computer</span></span>
[!INCLUDE [site-recovery-install-mob-svc-win-cmd](../../includes/site-recovery-install-mob-svc-win-cmd.md)]

### <a name="command-line-installation-on-a-linux-computer"></a><span data-ttu-id="cae81-147">Kommandoradsparametrarna för installation på en Linux-dator</span><span class="sxs-lookup"><span data-stu-id="cae81-147">Command-line installation on a Linux computer</span></span>
[!INCLUDE [site-recovery-install-mob-svc-lin-cmd](../../includes/site-recovery-install-mob-svc-lin-cmd.md)]


## <a name="install-mobility-service-by-push-installation-from-azure-site-recovery"></a><span data-ttu-id="cae81-148">Installera Mobilitetstjänsten genom push-installation från Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="cae81-148">Install Mobility Service by push installation from Azure Site Recovery</span></span>
<span data-ttu-id="cae81-149">toodo en push-installation av Mobilitetstjänsten genom att använda Site Recovery alla måldatorer måste uppfylla följande förutsättningar hello.</span><span class="sxs-lookup"><span data-stu-id="cae81-149">toodo a push installation of Mobility Service by using Site Recovery, all target computers must meet hello following prerequisites.</span></span>

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-win](../../includes/site-recovery-prepare-push-install-mob-svc-win.md)]

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-lin](../../includes/site-recovery-prepare-push-install-mob-svc-lin.md)]


> [!NOTE]
<span data-ttu-id="cae81-150">När Mobilitetstjänsten är installerad i hello Azure-portalen markerar hello **replikera** knappen toostart skyddar dessa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="cae81-150">After Mobility Service is installed, in hello Azure portal, select hello **Replicate** button toostart protecting these VMs.</span></span>

## <a name="uninstall-mobility-service-on-a-windows-server-computer"></a><span data-ttu-id="cae81-151">Avinstallera Mobilitetstjänsten på en dator med Windows Server</span><span class="sxs-lookup"><span data-stu-id="cae81-151">Uninstall Mobility Service on a Windows Server computer</span></span>
<span data-ttu-id="cae81-152">Använd någon av hello följande metoder toouninstall Mobilitetstjänsten på en Windows Server-dator.</span><span class="sxs-lookup"><span data-stu-id="cae81-152">Use one of hello following methods toouninstall Mobility Service on a Windows Server computer.</span></span>

### <a name="uninstall-by-using-hello-gui"></a><span data-ttu-id="cae81-153">Avinstallera med hello GUI</span><span class="sxs-lookup"><span data-stu-id="cae81-153">Uninstall by using hello GUI</span></span>
1. <span data-ttu-id="cae81-154">På Kontrollpanelen, Välj **program**.</span><span class="sxs-lookup"><span data-stu-id="cae81-154">In Control Panel, select **Programs**.</span></span>
2. <span data-ttu-id="cae81-155">Välj **Microsoft Azure Site Recovery Mobility tjänsten eller Huvudtjänsten målservern**, och välj sedan **avinstallera**.</span><span class="sxs-lookup"><span data-stu-id="cae81-155">Select **Microsoft Azure Site Recovery Mobility Service/Master Target server**, and then select **Uninstall**.</span></span>

### <a name="uninstall-at-a-command-prompt"></a><span data-ttu-id="cae81-156">Avinstallera i en kommandotolk</span><span class="sxs-lookup"><span data-stu-id="cae81-156">Uninstall at a command prompt</span></span>
1. <span data-ttu-id="cae81-157">Öppna ett kommandotolksfönster som administratör.</span><span class="sxs-lookup"><span data-stu-id="cae81-157">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="cae81-158">toouninstall Mobilitetstjänsten, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cae81-158">toouninstall Mobility Service, run hello following command:</span></span>

```
MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
```

## <a name="uninstall-mobility-service-on-a-linux-computer"></a><span data-ttu-id="cae81-159">Avinstallera Mobilitetstjänsten på en Linux-dator</span><span class="sxs-lookup"><span data-stu-id="cae81-159">Uninstall Mobility Service on a Linux computer</span></span>
1. <span data-ttu-id="cae81-160">Logga in på Linux-servern som en **rot** användare.</span><span class="sxs-lookup"><span data-stu-id="cae81-160">On your Linux server, sign in as a **root** user.</span></span>
2. <span data-ttu-id="cae81-161">Gå för/användare/lokal/ASR i en terminal.</span><span class="sxs-lookup"><span data-stu-id="cae81-161">In a terminal, go too/user/local/ASR.</span></span>
3. <span data-ttu-id="cae81-162">toouninstall Mobilitetstjänsten, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cae81-162">toouninstall Mobility Service, run hello following command:</span></span>

```
uninstall.sh -Y
```
