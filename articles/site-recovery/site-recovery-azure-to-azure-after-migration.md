---
title: "aaaPrepare datorer tooset in katastrofåterställning mellan Azure-regioner efter migrering tooAzure genom att använda Site Recovery | Microsoft Docs"
description: "Den här artikeln beskriver hur tooprepare datorer tooset in katastrofåterställning mellan Azure-regioner efter migrering tooAzure med hjälp av Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: ponatara
manager: abhemraj
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: ponatara
ms.openlocfilehash: b6274e3df210c1d8a7b8289cc85868ee6414e523
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-azure-vms-tooanother-region-after-migration-tooazure-by-using-azure-site-recovery"></a><span data-ttu-id="91382-103">Replikera virtuella datorer i Azure tooanother region efter migrering tooAzure med hjälp av Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="91382-103">Replicate Azure VMs tooanother region after migration tooAzure by using Azure Site Recovery</span></span>

>[!NOTE]
> <span data-ttu-id="91382-104">Azure Site Recovery replikering för virtuella Azure-datorer (VM) är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="91382-104">Azure Site Recovery replication for Azure virtual machines (VMs) is currently in preview.</span></span>

## <a name="overview"></a><span data-ttu-id="91382-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="91382-105">Overview</span></span>

<span data-ttu-id="91382-106">Den här artikeln hjälper dig att förbereda virtuella Azure-datorer för replikering mellan två Azure-regioner när dessa datorer har migrerats från en lokal miljö tooAzure med hjälp av Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="91382-106">This article helps you prepare Azure virtual machines for replication between two Azure regions after these machines have been migrated from an on-premises environment tooAzure by using Azure Site Recovery.</span></span>

## <a name="disaster-recovery-and-compliance"></a><span data-ttu-id="91382-107">Katastrofåterställning och efterlevnad</span><span class="sxs-lookup"><span data-stu-id="91382-107">Disaster recovery and compliance</span></span>
<span data-ttu-id="91382-108">Idag Flyttar fler och fler företag sina arbetsbelastningar tooAzure.</span><span class="sxs-lookup"><span data-stu-id="91382-108">Today, more and more enterprises are moving their workloads tooAzure.</span></span> <span data-ttu-id="91382-109">Med företag som flyttar verksamhetskritiska lokalt produktion är arbetsbelastningar tooAzure, ställa in katastrofåterställning för dessa arbetsbelastningar obligatoriskt för efterlevnad och toosafeguard mot eventuella avbrott i en Azure-region.</span><span class="sxs-lookup"><span data-stu-id="91382-109">With enterprises moving mission-critical on-premises production workloads tooAzure, setting up disaster recovery for these workloads is mandatory for compliance and toosafeguard against any disruptions in an Azure region.</span></span>

## <a name="steps-for-preparing-migrated-machines-for-replication"></a><span data-ttu-id="91382-110">Steg för att förbereda migrerade datorer för replikering</span><span class="sxs-lookup"><span data-stu-id="91382-110">Steps for preparing migrated machines for replication</span></span>
<span data-ttu-id="91382-111">tooprepare migrerade datorer för att konfigurera replikering tooanother Azure-region:</span><span class="sxs-lookup"><span data-stu-id="91382-111">tooprepare migrated machines for setting up replication tooanother Azure region:</span></span>

1. <span data-ttu-id="91382-112">Slutföra migreringen.</span><span class="sxs-lookup"><span data-stu-id="91382-112">Complete migration.</span></span>
2. <span data-ttu-id="91382-113">Installera hello Azure-agenten om det behövs.</span><span class="sxs-lookup"><span data-stu-id="91382-113">Install hello Azure agent if needed.</span></span>
3. <span data-ttu-id="91382-114">Ta bort hello mobilitetstjänsten.</span><span class="sxs-lookup"><span data-stu-id="91382-114">Remove hello Mobility service.</span></span>  
4. <span data-ttu-id="91382-115">Starta om hello VM.</span><span class="sxs-lookup"><span data-stu-id="91382-115">Restart hello VM.</span></span>

<span data-ttu-id="91382-116">Dessa steg beskrivs i detalj i följande avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="91382-116">These steps are described in more detail in hello following sections.</span></span>

### <a name="step-1-migrate-workloads-running-on-hyper-v-vms-vmware-vms-and-physical-servers-toorun-on-azure-vms"></a><span data-ttu-id="91382-117">Steg 1: Migrera arbetsbelastningar som körs på Hyper-V virtuella datorer, virtuella VMware-datorer och fysiska servrar toorun på Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="91382-117">Step 1: Migrate workloads running on Hyper-V VMs, VMware VMs, and physical servers toorun on Azure VMs</span></span>

<span data-ttu-id="91382-118">tooset upp replikering och migrera dina lokala Hyper-V, VMware och fysiska arbetsbelastningar tooAzure Följ hello stegen i hello [migrera Azure IaaS-virtuella datorer mellan Azure-regioner med Azure Site Recovery](site-recovery-migrate-to-azure.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="91382-118">tooset up replication and migrate your on-premises Hyper-V, VMware, and physical workloads tooAzure, follow hello steps in hello [Migrate Azure IaaS virtual machines between Azure regions with Azure Site Recovery](site-recovery-migrate-to-azure.md) article.</span></span> 

<span data-ttu-id="91382-119">Efter migreringen kan du inte behöver toocommit eller ta bort en växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="91382-119">After migration, you don't need toocommit or delete a failover.</span></span> <span data-ttu-id="91382-120">I stället väljer hello **slutföra migreringen** alternativet för varje dator som du vill toomigrate:</span><span class="sxs-lookup"><span data-stu-id="91382-120">Instead, select hello **Complete Migration** option for each machine you want toomigrate:</span></span>
1. <span data-ttu-id="91382-121">I **replikerade objekt**och högerklicka på hello VM, klicka på **slutföra migreringen**.</span><span class="sxs-lookup"><span data-stu-id="91382-121">In **Replicated Items**, right-click hello VM, and click **Complete Migration**.</span></span> <span data-ttu-id="91382-122">Klicka på **OK** toocomplete hello steg.</span><span class="sxs-lookup"><span data-stu-id="91382-122">Click **OK** toocomplete hello step.</span></span> <span data-ttu-id="91382-123">Du kan följa förloppet i hello VM egenskaper genom att övervaka hello fullständig migreringsjobb i **Site Recovery-jobb**.</span><span class="sxs-lookup"><span data-stu-id="91382-123">You can track progress in hello VM properties by monitoring hello Complete Migration job in **Site Recovery jobs**.</span></span>
2. <span data-ttu-id="91382-124">Hej **slutföra migreringen** åtgärden har slutförts hello migreringsprocessen, tar du bort replikeringen av hello datorn och stoppar Site Recovery-faktureringen för hello datorn.</span><span class="sxs-lookup"><span data-stu-id="91382-124">hello **Complete Migration** action completes hello migration process, removes replication for hello machine, and stops Site Recovery billing for hello machine.</span></span>

   ![fullständig migrering](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

### <a name="step-2-install-hello-azure-vm-agent-on-hello-virtual-machine"></a><span data-ttu-id="91382-126">Steg 2: Installera hello Azure VM-agenten på hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="91382-126">Step 2: Install hello Azure VM agent on hello virtual machine</span></span>
<span data-ttu-id="91382-127">hello Azure [VM-agenten](../virtual-machines/windows/classic/agents-and-extensions.md#azure-vm-agents-for-windows-and-linux) måste vara installerad på hello virtuell dator för hello Site Recovery tillägget toowork och toohelp skydda hello VM.</span><span class="sxs-lookup"><span data-stu-id="91382-127">hello Azure [VM agent](../virtual-machines/windows/classic/agents-and-extensions.md#azure-vm-agents-for-windows-and-linux) must be installed on hello virtual machine for hello Site Recovery extension toowork and toohelp protect hello VM.</span></span>

>[!IMPORTANT]
><span data-ttu-id="91382-128">Från och med version 9.7.0.0, på Windows-datorer installeras hello Mobilitetstjänstens installationsprogram också hello senaste tillgängliga Virtuella Azure-agenten.</span><span class="sxs-lookup"><span data-stu-id="91382-128">Beginning with version 9.7.0.0, on Windows virtual machines, hello Mobility service installer also installs hello latest available Azure VM agent.</span></span> <span data-ttu-id="91382-129">Om migrering uppfyller hello virtuella installationen av nödvändiga för att använda alla VM-tillägg, inklusive hello Site Recovery-tillägget.</span><span class="sxs-lookup"><span data-stu-id="91382-129">On migration, hello virtual machine meets the agent installation prerequisite for using any VM extension, including hello Site Recovery extension.</span></span> <span data-ttu-id="91382-130">hello Azure VM-agenten måste toobe manuellt installerade endast om hello mobilitetstjänsten installeras på hello migrerad dator är version 9,6 eller tidigare.</span><span class="sxs-lookup"><span data-stu-id="91382-130">hello Azure VM agent needs toobe manually installed only if hello Mobility service installed on hello migrated machine is version 9.6 or earlier.</span></span>

<span data-ttu-id="91382-131">hello innehåller följande tabell ytterligare information om hur du installerar hello VM-agenten och verifiera att den har installerats:</span><span class="sxs-lookup"><span data-stu-id="91382-131">hello following table provides additional information about installing hello VM agent and validating that it was installed:</span></span>

| <span data-ttu-id="91382-132">**Åtgärd**</span><span class="sxs-lookup"><span data-stu-id="91382-132">**Operation**</span></span> | <span data-ttu-id="91382-133">**Windows**</span><span class="sxs-lookup"><span data-stu-id="91382-133">**Windows**</span></span> | <span data-ttu-id="91382-134">**Linux**</span><span class="sxs-lookup"><span data-stu-id="91382-134">**Linux**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="91382-135">Installera hello VM-agent</span><span class="sxs-lookup"><span data-stu-id="91382-135">Installing hello VM agent</span></span> |<span data-ttu-id="91382-136">Hämta och installera hello [agent MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="91382-136">Download and install hello [agent MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).</span></span> <span data-ttu-id="91382-137">Du måste administratören privilegier toocomplete hello installation.</span><span class="sxs-lookup"><span data-stu-id="91382-137">You need administrator privileges toocomplete hello installation.</span></span> |<span data-ttu-id="91382-138">Installera hello senaste [Linux-agenten](../virtual-machines/linux/agent-user-guide.md).</span><span class="sxs-lookup"><span data-stu-id="91382-138">Install hello latest [Linux agent](../virtual-machines/linux/agent-user-guide.md).</span></span> <span data-ttu-id="91382-139">Du måste administratören privilegier toocomplete hello installation.</span><span class="sxs-lookup"><span data-stu-id="91382-139">You need administrator privileges toocomplete hello installation.</span></span> <span data-ttu-id="91382-140">Vi rekommenderar att du installerar hello agent från databasen för din distribution.</span><span class="sxs-lookup"><span data-stu-id="91382-140">We recommend installing hello agent from your distribution repository.</span></span> <span data-ttu-id="91382-141">Vi *rekommenderar inte* installera hello Linux VM-agenten direkt från GitHub.</span><span class="sxs-lookup"><span data-stu-id="91382-141">We *do not recommend* installing hello Linux VM agent directly from GitHub.</span></span>  |
| <span data-ttu-id="91382-142">Verifiera hello VM agentinstallation</span><span class="sxs-lookup"><span data-stu-id="91382-142">Validating hello VM agent installation</span></span> |<span data-ttu-id="91382-143">1. Bläddra toohello C:\WindowsAzure\Packages mapp i hello Azure VM.</span><span class="sxs-lookup"><span data-stu-id="91382-143">1. Browse toohello C:\WindowsAzure\Packages folder in hello Azure VM.</span></span> <span data-ttu-id="91382-144">Du bör se hello WaAppAgent.exe fil.</span><span class="sxs-lookup"><span data-stu-id="91382-144">You should see hello WaAppAgent.exe file.</span></span> <br><span data-ttu-id="91382-145">2. Högerklicka på filen hello finns för**egenskaper**, och välj sedan hello **information** fliken hello **produktversionen** fältet måste innehålla 2.6.1198.718 eller högre.</span><span class="sxs-lookup"><span data-stu-id="91382-145">2. Right-click hello file, go too**Properties**, and then select hello **Details** tab. hello **Product Version** field should be 2.6.1198.718 or higher.</span></span> |<span data-ttu-id="91382-146">Saknas</span><span class="sxs-lookup"><span data-stu-id="91382-146">N/A</span></span> |


### <a name="step-3-remove-hello-mobility-service-from-hello-migrated-virtual-machine"></a><span data-ttu-id="91382-147">Steg 3: Ta bort hello mobilitetstjänsten från hello migrerad virtuell dator</span><span class="sxs-lookup"><span data-stu-id="91382-147">Step 3: Remove hello Mobility service from hello migrated virtual machine</span></span>

<span data-ttu-id="91382-148">Om du har migrerat dina lokala VMware-datorer eller fysiska servrar på Windows-/ Linux, måste toomanually ta bort eller avinstallera hello mobilitetstjänsten från hello migrerade virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="91382-148">If you have migrated your on-premises VMware machines or physical servers on Windows/Linux, you need toomanually remove/uninstall hello Mobility service from hello migrated virtual machine.</span></span>

>[!IMPORTANT]
><span data-ttu-id="91382-149">Det här steget krävs inte för migrerade tooAzure för Hyper-V virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="91382-149">This step is not required for Hyper-V VMs migrated tooAzure.</span></span>

#### <a name="uninstall-hello-mobility-service-on-a-windows-server-vm"></a><span data-ttu-id="91382-150">Avinstallera hello mobilitetstjänsten på en Windows Server-VM</span><span class="sxs-lookup"><span data-stu-id="91382-150">Uninstall hello Mobility service on a Windows Server VM</span></span>
<span data-ttu-id="91382-151">Använd någon av hello följande metoder toouninstall hello mobilitetstjänsten på en Windows Server-dator.</span><span class="sxs-lookup"><span data-stu-id="91382-151">Use one of hello following methods toouninstall hello Mobility service on a Windows Server computer.</span></span>

##### <a name="uninstall-by-using-hello-windows-ui"></a><span data-ttu-id="91382-152">Avinstallera med hello användargränssnitt för Windows</span><span class="sxs-lookup"><span data-stu-id="91382-152">Uninstall by using hello Windows UI</span></span>
1. <span data-ttu-id="91382-153">I hello Kontrollpanelen, Välj **program**.</span><span class="sxs-lookup"><span data-stu-id="91382-153">In hello Control Panel, select **Programs**.</span></span>
2. <span data-ttu-id="91382-154">Välj **Microsoft Azure Site Recovery Mobility tjänsten eller Huvudtjänsten målservern**, och välj sedan **avinstallera**.</span><span class="sxs-lookup"><span data-stu-id="91382-154">Select **Microsoft Azure Site Recovery Mobility Service/Master Target server**, and then select **Uninstall**.</span></span>

##### <a name="uninstall-at-a-command-prompt"></a><span data-ttu-id="91382-155">Avinstallera i en kommandotolk</span><span class="sxs-lookup"><span data-stu-id="91382-155">Uninstall at a command prompt</span></span>
1. <span data-ttu-id="91382-156">Öppna ett kommandotolksfönster som administratör.</span><span class="sxs-lookup"><span data-stu-id="91382-156">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="91382-157">toouninstall Hej mobilitetstjänsten, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="91382-157">toouninstall hello Mobility service, run hello following command:</span></span>

   ```
   MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
   ```

#### <a name="uninstall-hello-mobility-service-on-a-linux-computer"></a><span data-ttu-id="91382-158">Avinstallera hello mobilitetstjänsten på en Linux-dator</span><span class="sxs-lookup"><span data-stu-id="91382-158">Uninstall hello Mobility service on a Linux computer</span></span>
1. <span data-ttu-id="91382-159">Logga in på Linux-servern som en **rot** användare.</span><span class="sxs-lookup"><span data-stu-id="91382-159">On your Linux server, sign in as a **root** user.</span></span>
2. <span data-ttu-id="91382-160">Gå för/användare/lokal/ASR i en terminal.</span><span class="sxs-lookup"><span data-stu-id="91382-160">In a terminal, go too/user/local/ASR.</span></span>
3. <span data-ttu-id="91382-161">toouninstall Hej mobilitetstjänsten, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="91382-161">toouninstall hello Mobility service, run hello following command:</span></span>

   ```
   uninstall.sh -Y
   ```

### <a name="step-4-restart-hello-vm"></a><span data-ttu-id="91382-162">Steg 4: Starta om hello VM</span><span class="sxs-lookup"><span data-stu-id="91382-162">Step 4: Restart hello VM</span></span>

<span data-ttu-id="91382-163">När du har avinstallerat hello mobilitetstjänsten omstart hello VM innan du konfigurera replikering tooanother Azure-region.</span><span class="sxs-lookup"><span data-stu-id="91382-163">After you uninstall hello Mobility service, restart hello VM before you set up replication tooanother Azure region.</span></span>


## <a name="next-steps"></a><span data-ttu-id="91382-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="91382-164">Next steps</span></span>
- <span data-ttu-id="91382-165">Börja skydda dina arbetsbelastningar av [replikering av Azure virtuella datorer](site-recovery-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="91382-165">Start protecting your workloads by [replicating Azure virtual machines](site-recovery-azure-to-azure.md).</span></span>
- <span data-ttu-id="91382-166">Lär dig mer om [nätverk vägledning för att replikera virtuella datorer i Azure](site-recovery-azure-to-azure-networking-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="91382-166">Learn more about [networking guidance for replicating Azure virtual machines](site-recovery-azure-to-azure-networking-guidance.md).</span></span>
