---
title: "Installera Mobilitetstjänsten (VMware eller fysisk till Azure) | Microsoft Docs"
description: "Lär dig mer om att installera Mobilitetstjänsten agenten för att skydda dina lokala datorer."
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
ms.openlocfilehash: 848284f37ae2470a169d8f8a8c9c0bb5b926abe3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="install-mobility-service-vmware-or-physical-to-azure"></a><span data-ttu-id="26fb5-103">Installera Mobilitetstjänsten (VMware eller fysisk till Azure)</span><span class="sxs-lookup"><span data-stu-id="26fb5-103">Install Mobility Service (VMware or physical to Azure)</span></span>
<span data-ttu-id="26fb5-104">Azure Site Recovery-Mobilitetstjänsten samlar in dataskrivningar på en dator och vidarebefordrar dem till processervern.</span><span class="sxs-lookup"><span data-stu-id="26fb5-104">Azure Site Recovery Mobility Service captures data writes on a computer, and then forwards them to the process server.</span></span> <span data-ttu-id="26fb5-105">Distribuera Mobilitetstjänsten till varje dator (VMware VM eller fysiska server) som du vill replikera till Azure.</span><span class="sxs-lookup"><span data-stu-id="26fb5-105">Deploy Mobility Service to every computer (VMware VM or physical server) that you want to replicate to Azure.</span></span> <span data-ttu-id="26fb5-106">Du kan distribuera Mobilitetstjänsten till servrar som du vill skydda med hjälp av följande metoder:</span><span class="sxs-lookup"><span data-stu-id="26fb5-106">You can deploy Mobility Service to the servers that you want to protect by using the following methods:</span></span>


* [<span data-ttu-id="26fb5-107">Installera Mobilitetstjänsten med hjälp av verktyg för programvarudistribution som System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="26fb5-107">Install Mobility Service by using software deployment tools like System Center Configuration Manager</span></span>](site-recovery-install-mobility-service-using-sccm.md)
* [<span data-ttu-id="26fb5-108">Installera Mobilitetstjänsten med hjälp av Azure Automation och Desired State Configuration Automation DSC)</span><span class="sxs-lookup"><span data-stu-id="26fb5-108">Install Mobility Service by using Azure Automation and Desired State Configuration (Automation DSC)</span></span>](site-recovery-automate-mobility-service-install.md)
* [<span data-ttu-id="26fb5-109">Installera Mobilitetstjänsten manuellt med hjälp av det grafiska användargränssnittet (GUI)</span><span class="sxs-lookup"><span data-stu-id="26fb5-109">Install Mobility Service manually by using the graphical user interface (GUI)</span></span>](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui)
* [<span data-ttu-id="26fb5-110">Installera Mobilitetstjänsten manuellt i en kommandotolk</span><span class="sxs-lookup"><span data-stu-id="26fb5-110">Install Mobility Service manually at a command prompt</span></span>](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt)
* [<span data-ttu-id="26fb5-111">Installera Mobilitetstjänsten genom push-installation från Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="26fb5-111">Install Mobility Service by push installation from Azure Site Recovery</span></span>](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery)


>[!IMPORTANT]
> <span data-ttu-id="26fb5-112">Från och med version 9.7.0.0, på Windows-datorer (VM), Mobilitetstjänsten installerar installationsprogrammet också den senaste tillgängliga [Virtuella Azure-datoragenten](../virtual-machines/windows/extensions-features.md#azure-vm-agent).</span><span class="sxs-lookup"><span data-stu-id="26fb5-112">Beginning with version 9.7.0.0, on Windows virtual machines (VMs), the Mobility Service installer also installs the latest available [Azure VM agent](../virtual-machines/windows/extensions-features.md#azure-vm-agent).</span></span> <span data-ttu-id="26fb5-113">När en dator växlar till Azure måste uppfyller datorn installationen av nödvändiga för att använda alla VM-tillägg.</span><span class="sxs-lookup"><span data-stu-id="26fb5-113">When a computer fails over to Azure, the computer meets the agent installation prerequisite for using any VM extension.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="26fb5-114">Krav</span><span class="sxs-lookup"><span data-stu-id="26fb5-114">Prerequisites</span></span>
<span data-ttu-id="26fb5-115">Gör följande förutsättningar innan du installerar Mobilitetstjänsten manuellt på servern:</span><span class="sxs-lookup"><span data-stu-id="26fb5-115">Complete these prerequisite steps before you manually install Mobility Service on your server:</span></span>
1. <span data-ttu-id="26fb5-116">Logga in på din server för konfiguration och öppna ett kommandotolksfönster som administratör.</span><span class="sxs-lookup"><span data-stu-id="26fb5-116">Sign in to your configuration server, and then open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="26fb5-117">Ändra katalogen till bin-mappen och sedan skapa en lösenfras fil:</span><span class="sxs-lookup"><span data-stu-id="26fb5-117">Change the directory to the bin folder, and then create a passphrase file:</span></span>

    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    genpassphrase.exe -v > MobSvc.passphrase
    ```
3. <span data-ttu-id="26fb5-118">Lagra filen lösenfras på en säker plats.</span><span class="sxs-lookup"><span data-stu-id="26fb5-118">Store the passphrase file in a secure location.</span></span> <span data-ttu-id="26fb5-119">Du kan använda filen under installationen av Mobilitetstjänsten.</span><span class="sxs-lookup"><span data-stu-id="26fb5-119">You use the file during the Mobility Service installation.</span></span>
4. <span data-ttu-id="26fb5-120">Mobility Service installationsprogram för alla operativsystem som stöds finns i mappen %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository.</span><span class="sxs-lookup"><span data-stu-id="26fb5-120">Mobility Service installers for all supported operating systems are in the %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository folder.</span></span>

### <a name="mobility-service-installer-to-operating-system-mapping"></a><span data-ttu-id="26fb5-121">Mobility-installationsprogram för operativsystem systemmappning</span><span class="sxs-lookup"><span data-stu-id="26fb5-121">Mobility Service installer-to-operating system mapping</span></span>

| <span data-ttu-id="26fb5-122">Installer filnamn för mallen</span><span class="sxs-lookup"><span data-stu-id="26fb5-122">Installer file template name</span></span>| <span data-ttu-id="26fb5-123">Operativsystem</span><span class="sxs-lookup"><span data-stu-id="26fb5-123">Operating system</span></span> |
|---|--|
|<span data-ttu-id="26fb5-124">Microsoft ASR\_UA\*Windows\*release.exe</span><span class="sxs-lookup"><span data-stu-id="26fb5-124">Microsoft-ASR\_UA\*Windows\*release.exe</span></span> | <span data-ttu-id="26fb5-125">Windows Server 2008 R2 SP1 (64-bitars)</span><span class="sxs-lookup"><span data-stu-id="26fb5-125">Windows Server 2008 R2 SP1 (64-bit)</span></span> </br> <span data-ttu-id="26fb5-126">Windows Server 2012 (64-bitars)</span><span class="sxs-lookup"><span data-stu-id="26fb5-126">Windows Server 2012 (64-bit)</span></span> </br> <span data-ttu-id="26fb5-127">Windows Server 2012 R2 (64-bitars)</span><span class="sxs-lookup"><span data-stu-id="26fb5-127">Windows Server 2012 R2 (64-bit)</span></span> |
|<span data-ttu-id="26fb5-128">Microsoft ASR\_UA\*RHEL6 64*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="26fb5-128">Microsoft-ASR\_UA\*RHEL6-64*release.tar.gz</span></span>| <span data-ttu-id="26fb5-129">Red Hat Enterprise Linux (RHEL) 6.4, 6.5, 6.6, 6.7, 6.8 (endast 64-bitars)</span><span class="sxs-lookup"><span data-stu-id="26fb5-129">Red Hat Enterprise Linux (RHEL) 6.4, 6.5, 6.6, 6.7, 6.8 (64-bit only)</span></span> </br> <span data-ttu-id="26fb5-130">CentOS 6.4, 6.5, 6.6, 6.7, 6.8 (endast 64-bitars)</span><span class="sxs-lookup"><span data-stu-id="26fb5-130">CentOS 6.4, 6.5, 6.6, 6.7, 6.8 (64-bit only)</span></span> |
|<span data-ttu-id="26fb5-131">Microsoft ASR\_UA\*RHEL7 64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="26fb5-131">Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz</span></span> | <span data-ttu-id="26fb5-132">Red Hat Enterprise Linux (RHEL) 7.1, 7.2 (endast 64-bitars)</span><span class="sxs-lookup"><span data-stu-id="26fb5-132">Red Hat Enterprise Linux (RHEL) 7.1, 7.2 (64-bit only)</span></span> </br> <span data-ttu-id="26fb5-133">CentOS 7.0, 7.1, 7.2 (endast 64-bitars)</span><span class="sxs-lookup"><span data-stu-id="26fb5-133">CentOS 7.0, 7.1, 7.2 (64-bit only)</span></span></br> <span data-ttu-id="26fb5-134">CentOs 7.3 (migrering)</span><span class="sxs-lookup"><span data-stu-id="26fb5-134">CentOs 7.3 (migration only)</span></span> |
|<span data-ttu-id="26fb5-135">Microsoft ASR\_UA\*SLES11-SP3-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="26fb5-135">Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz</span></span>| <span data-ttu-id="26fb5-136">SUSE Linux Enterprise Server 11 SP3 (endast 64-bitars)</span><span class="sxs-lookup"><span data-stu-id="26fb5-136">SUSE Linux Enterprise Server 11 SP3 (64-bit only)</span></span>|
|<span data-ttu-id="26fb5-137">Microsoft ASR\_UA\*SLES11-SP4-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="26fb5-137">Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz</span></span>| <span data-ttu-id="26fb5-138">SUSE Linux Enterprise Server 11 SP4 (endast 64-bitars)</span><span class="sxs-lookup"><span data-stu-id="26fb5-138">SUSE Linux Enterprise Server 11 SP4 (64-bit only)</span></span>|
|<span data-ttu-id="26fb5-139">Microsoft ASR\_UA\*OL6 64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="26fb5-139">Microsoft-ASR\_UA\*OL6-64\*release.tar.gz</span></span> | <span data-ttu-id="26fb5-140">Oracle Enterprise Linux 6.4, 6.5 (endast 64-bitars)</span><span class="sxs-lookup"><span data-stu-id="26fb5-140">Oracle Enterprise Linux 6.4, 6.5 (64-bit only)</span></span>|
|<span data-ttu-id="26fb5-141">Microsoft ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="26fb5-141">Microsoft-ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz</span></span> | <span data-ttu-id="26fb5-142">Ubuntu Linux 14.04 (endast 64-bitars)</span><span class="sxs-lookup"><span data-stu-id="26fb5-142">Ubuntu Linux 14.04 (64-bit only)</span></span>|


## <a name="install-mobility-service-manually-by-using-the-gui"></a><span data-ttu-id="26fb5-143">Installera Mobilitetstjänsten manuellt med hjälp av det grafiska Användargränssnittet</span><span class="sxs-lookup"><span data-stu-id="26fb5-143">Install Mobility Service manually by using the GUI</span></span>

>[!IMPORTANT]
> <span data-ttu-id="26fb5-144">Om du använder en **konfigurationsservern** att replikera **Azure IaaS-virtuella datorer** från ett Azure prenumeration eller en Region till en annan sedan **använda kommandoradsverktyget baserat installationen** metod</span><span class="sxs-lookup"><span data-stu-id="26fb5-144">If you are using a **Configuration Server** to replicate **Azure IaaS virtual machines** from one Azure Subscription/Region to another then **use the Command-line based installation** method</span></span>

[!INCLUDE [site-recovery-install-mob-svc-gui](../../includes/site-recovery-install-mob-svc-gui.md)]

## <a name="install-mobility-service-manually-at-a-command-prompt"></a><span data-ttu-id="26fb5-145">Installera Mobilitetstjänsten manuellt i en kommandotolk</span><span class="sxs-lookup"><span data-stu-id="26fb5-145">Install Mobility Service manually at a command prompt</span></span>

### <a name="command-line-installation-on-a-windows-computer"></a><span data-ttu-id="26fb5-146">Kommandoradsparametrarna för installation på en Windows-dator</span><span class="sxs-lookup"><span data-stu-id="26fb5-146">Command-line installation on a Windows computer</span></span>
[!INCLUDE [site-recovery-install-mob-svc-win-cmd](../../includes/site-recovery-install-mob-svc-win-cmd.md)]

### <a name="command-line-installation-on-a-linux-computer"></a><span data-ttu-id="26fb5-147">Kommandoradsparametrarna för installation på en Linux-dator</span><span class="sxs-lookup"><span data-stu-id="26fb5-147">Command-line installation on a Linux computer</span></span>
[!INCLUDE [site-recovery-install-mob-svc-lin-cmd](../../includes/site-recovery-install-mob-svc-lin-cmd.md)]


## <a name="install-mobility-service-by-push-installation-from-azure-site-recovery"></a><span data-ttu-id="26fb5-148">Installera Mobilitetstjänsten genom push-installation från Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="26fb5-148">Install Mobility Service by push installation from Azure Site Recovery</span></span>
<span data-ttu-id="26fb5-149">Alla måldatorer måste uppfylla följande krav för att göra en push-installation av Mobilitetstjänsten genom att använda Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="26fb5-149">To do a push installation of Mobility Service by using Site Recovery, all target computers must meet the following prerequisites.</span></span>

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-win](../../includes/site-recovery-prepare-push-install-mob-svc-win.md)]

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-lin](../../includes/site-recovery-prepare-push-install-mob-svc-lin.md)]


> [!NOTE]
<span data-ttu-id="26fb5-150">När Mobilitetstjänsten är installerad i Azure portal, väljer du den **replikera** knappen för att börja skydda dessa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="26fb5-150">After Mobility Service is installed, in the Azure portal, select the **Replicate** button to start protecting these VMs.</span></span>

## <a name="uninstall-mobility-service-on-a-windows-server-computer"></a><span data-ttu-id="26fb5-151">Avinstallera Mobilitetstjänsten på en dator med Windows Server</span><span class="sxs-lookup"><span data-stu-id="26fb5-151">Uninstall Mobility Service on a Windows Server computer</span></span>
<span data-ttu-id="26fb5-152">Använd någon av följande metoder för att avinstallera Mobilitetstjänsten på en Windows Server.</span><span class="sxs-lookup"><span data-stu-id="26fb5-152">Use one of the following methods to uninstall Mobility Service on a Windows Server computer.</span></span>

### <a name="uninstall-by-using-the-gui"></a><span data-ttu-id="26fb5-153">Avinstallera genom att använda det grafiska Användargränssnittet</span><span class="sxs-lookup"><span data-stu-id="26fb5-153">Uninstall by using the GUI</span></span>
1. <span data-ttu-id="26fb5-154">På Kontrollpanelen, Välj **program**.</span><span class="sxs-lookup"><span data-stu-id="26fb5-154">In Control Panel, select **Programs**.</span></span>
2. <span data-ttu-id="26fb5-155">Välj **Microsoft Azure Site Recovery Mobility tjänsten eller Huvudtjänsten målservern**, och välj sedan **avinstallera**.</span><span class="sxs-lookup"><span data-stu-id="26fb5-155">Select **Microsoft Azure Site Recovery Mobility Service/Master Target server**, and then select **Uninstall**.</span></span>

### <a name="uninstall-at-a-command-prompt"></a><span data-ttu-id="26fb5-156">Avinstallera i en kommandotolk</span><span class="sxs-lookup"><span data-stu-id="26fb5-156">Uninstall at a command prompt</span></span>
1. <span data-ttu-id="26fb5-157">Öppna ett kommandotolksfönster som administratör.</span><span class="sxs-lookup"><span data-stu-id="26fb5-157">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="26fb5-158">Om du vill avinstallera Mobilitetstjänsten, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="26fb5-158">To uninstall Mobility Service, run the following command:</span></span>

```
MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
```

## <a name="uninstall-mobility-service-on-a-linux-computer"></a><span data-ttu-id="26fb5-159">Avinstallera Mobilitetstjänsten på en Linux-dator</span><span class="sxs-lookup"><span data-stu-id="26fb5-159">Uninstall Mobility Service on a Linux computer</span></span>
1. <span data-ttu-id="26fb5-160">Logga in på Linux-servern som en **rot** användare.</span><span class="sxs-lookup"><span data-stu-id="26fb5-160">On your Linux server, sign in as a **root** user.</span></span>
2. <span data-ttu-id="26fb5-161">Gå till /user/local/ASR i en terminal.</span><span class="sxs-lookup"><span data-stu-id="26fb5-161">In a terminal, go to /user/local/ASR.</span></span>
3. <span data-ttu-id="26fb5-162">Om du vill avinstallera Mobilitetstjänsten, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="26fb5-162">To uninstall Mobility Service, run the following command:</span></span>

```
uninstall.sh -Y
```
