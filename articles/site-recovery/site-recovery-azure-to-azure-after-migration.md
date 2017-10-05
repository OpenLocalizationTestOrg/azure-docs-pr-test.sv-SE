---
title: "Förbereda datorer för att ställa in katastrofåterställning mellan Azure-regioner efter migrering till Azure med hjälp av Site Recovery | Microsoft Docs"
description: "Den här artikeln beskriver hur du förbereda datorer för att ställa in katastrofåterställning mellan Azure-regioner efter migrering till Azure med hjälp av Azure Site Recovery."
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
ms.openlocfilehash: 2aee0fb8d1ba1ff1584bee91b4d1cc34b654d97f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-azure-vms-to-another-region-after-migration-to-azure-by-using-azure-site-recovery"></a><span data-ttu-id="8fbce-103">Replikera virtuella Azure-datorer till en annan region efter migrering till Azure med hjälp av Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="8fbce-103">Replicate Azure VMs to another region after migration to Azure by using Azure Site Recovery</span></span>

>[!NOTE]
> <span data-ttu-id="8fbce-104">Azure Site Recovery replikering för virtuella Azure-datorer (VM) är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="8fbce-104">Azure Site Recovery replication for Azure virtual machines (VMs) is currently in preview.</span></span>

## <a name="overview"></a><span data-ttu-id="8fbce-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="8fbce-105">Overview</span></span>

<span data-ttu-id="8fbce-106">Den här artikeln hjälper dig att förbereda virtuella Azure-datorer för replikering mellan två Azure-regioner när dessa datorer har migrerats från en lokal miljö till Azure med hjälp av Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="8fbce-106">This article helps you prepare Azure virtual machines for replication between two Azure regions after these machines have been migrated from an on-premises environment to Azure by using Azure Site Recovery.</span></span>

## <a name="disaster-recovery-and-compliance"></a><span data-ttu-id="8fbce-107">Katastrofåterställning och efterlevnad</span><span class="sxs-lookup"><span data-stu-id="8fbce-107">Disaster recovery and compliance</span></span>
<span data-ttu-id="8fbce-108">Idag Flyttar fler och fler företag sina arbetsbelastningar till Azure.</span><span class="sxs-lookup"><span data-stu-id="8fbce-108">Today, more and more enterprises are moving their workloads to Azure.</span></span> <span data-ttu-id="8fbce-109">Med företag flytta verksamhetskritiska lokalt produktionsarbetsbelastningar till Azure, ställa in katastrofåterställning för dessa arbetsbelastningar är obligatoriskt för efterlevnad och för att skydda mot eventuella avbrott i en Azure-region.</span><span class="sxs-lookup"><span data-stu-id="8fbce-109">With enterprises moving mission-critical on-premises production workloads to Azure, setting up disaster recovery for these workloads is mandatory for compliance and to safeguard against any disruptions in an Azure region.</span></span>

## <a name="steps-for-preparing-migrated-machines-for-replication"></a><span data-ttu-id="8fbce-110">Steg för att förbereda migrerade datorer för replikering</span><span class="sxs-lookup"><span data-stu-id="8fbce-110">Steps for preparing migrated machines for replication</span></span>
<span data-ttu-id="8fbce-111">Så här förbereder du har migrerat datorer för att konfigurera replikering till en annan Azure-region:</span><span class="sxs-lookup"><span data-stu-id="8fbce-111">To prepare migrated machines for setting up replication to another Azure region:</span></span>

1. <span data-ttu-id="8fbce-112">Slutföra migreringen.</span><span class="sxs-lookup"><span data-stu-id="8fbce-112">Complete migration.</span></span>
2. <span data-ttu-id="8fbce-113">Installera Azure-agenten om det behövs.</span><span class="sxs-lookup"><span data-stu-id="8fbce-113">Install the Azure agent if needed.</span></span>
3. <span data-ttu-id="8fbce-114">Ta bort mobilitetstjänsten.</span><span class="sxs-lookup"><span data-stu-id="8fbce-114">Remove the Mobility service.</span></span>  
4. <span data-ttu-id="8fbce-115">Starta om den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8fbce-115">Restart the VM.</span></span>

<span data-ttu-id="8fbce-116">Dessa steg beskrivs i detalj i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8fbce-116">These steps are described in more detail in the following sections.</span></span>

### <a name="step-1-migrate-workloads-running-on-hyper-v-vms-vmware-vms-and-physical-servers-to-run-on-azure-vms"></a><span data-ttu-id="8fbce-117">Steg 1: Migrera arbetsbelastningar som körs på Hyper-V virtuella datorer, virtuella VMware-datorer och fysiska servrar körs på virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="8fbce-117">Step 1: Migrate workloads running on Hyper-V VMs, VMware VMs, and physical servers to run on Azure VMs</span></span>

<span data-ttu-id="8fbce-118">Konfigurera replikering och migrera dina lokala Hyper-V, VMware och fysiska arbetsbelastningar till Azure, följer du stegen i den [migrera Azure IaaS-virtuella datorer mellan Azure-regioner med Azure Site Recovery](site-recovery-migrate-to-azure.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="8fbce-118">To set up replication and migrate your on-premises Hyper-V, VMware, and physical workloads to Azure, follow the steps in the [Migrate Azure IaaS virtual machines between Azure regions with Azure Site Recovery](site-recovery-migrate-to-azure.md) article.</span></span> 

<span data-ttu-id="8fbce-119">Efter migreringen behöver du inte spara eller ta bort en växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="8fbce-119">After migration, you don't need to commit or delete a failover.</span></span> <span data-ttu-id="8fbce-120">I stället väljer den **slutföra migreringen** alternativet för varje dator som du vill migrera:</span><span class="sxs-lookup"><span data-stu-id="8fbce-120">Instead, select the **Complete Migration** option for each machine you want to migrate:</span></span>
1. <span data-ttu-id="8fbce-121">I **Replikerade objekt** högerklickar du på den virtuella datorn och på **Slutför migrering**.</span><span class="sxs-lookup"><span data-stu-id="8fbce-121">In **Replicated Items**, right-click the VM, and click **Complete Migration**.</span></span> <span data-ttu-id="8fbce-122">Klicka på **OK** att slutföra steget.</span><span class="sxs-lookup"><span data-stu-id="8fbce-122">Click **OK** to complete the step.</span></span> <span data-ttu-id="8fbce-123">Du kan följa förloppet i Egenskaper för Virtuella datorer genom att övervaka jobbet slutföra migrering i **Site Recovery-jobb**.</span><span class="sxs-lookup"><span data-stu-id="8fbce-123">You can track progress in the VM properties by monitoring the Complete Migration job in **Site Recovery jobs**.</span></span>
2. <span data-ttu-id="8fbce-124">Den **slutföra migreringen** åtgärden har slutförts migreringsprocessen, tar bort replikering för datorn och stoppar Site Recovery-faktureringen för datorn.</span><span class="sxs-lookup"><span data-stu-id="8fbce-124">The **Complete Migration** action completes the migration process, removes replication for the machine, and stops Site Recovery billing for the machine.</span></span>

   ![fullständig migrering](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

### <a name="step-2-install-the-azure-vm-agent-on-the-virtual-machine"></a><span data-ttu-id="8fbce-126">Steg 2: Installera Azure VM-agenten på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="8fbce-126">Step 2: Install the Azure VM agent on the virtual machine</span></span>
<span data-ttu-id="8fbce-127">Azure [VM-agenten](../virtual-machines/windows/classic/agents-and-extensions.md#azure-vm-agents-for-windows-and-linux) måste vara installerad på den virtuella datorn för Site Recovery-tillägget ska fungera och för att skydda den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8fbce-127">The Azure [VM agent](../virtual-machines/windows/classic/agents-and-extensions.md#azure-vm-agents-for-windows-and-linux) must be installed on the virtual machine for the Site Recovery extension to work and to help protect the VM.</span></span>

>[!IMPORTANT]
><span data-ttu-id="8fbce-128">Från och med version 9.7.0.0, på Windows-datorer installerar Mobilitetstjänstens installationsprogram också den senaste tillgängliga Virtuella Azure-agenten.</span><span class="sxs-lookup"><span data-stu-id="8fbce-128">Beginning with version 9.7.0.0, on Windows virtual machines, the Mobility service installer also installs the latest available Azure VM agent.</span></span> <span data-ttu-id="8fbce-129">Den virtuella datorn uppfyller installationen av nödvändiga för att använda alla VM-tillägg, inklusive tillägget Site Recovery på migreringen.</span><span class="sxs-lookup"><span data-stu-id="8fbce-129">On migration, the virtual machine meets the agent installation prerequisite for using any VM extension, including the Site Recovery extension.</span></span> <span data-ttu-id="8fbce-130">Virtuella Azure-agenten måste installeras manuellt endast om mobilitetstjänsten installeras på den migrerade datorn är version 9,6 eller tidigare.</span><span class="sxs-lookup"><span data-stu-id="8fbce-130">The Azure VM agent needs to be manually installed only if the Mobility service installed on the migrated machine is version 9.6 or earlier.</span></span>

<span data-ttu-id="8fbce-131">Följande tabell innehåller ytterligare information om att installera den Virtuella datoragenten och verifiera att den har installerats:</span><span class="sxs-lookup"><span data-stu-id="8fbce-131">The following table provides additional information about installing the VM agent and validating that it was installed:</span></span>

| <span data-ttu-id="8fbce-132">**Åtgärd**</span><span class="sxs-lookup"><span data-stu-id="8fbce-132">**Operation**</span></span> | <span data-ttu-id="8fbce-133">**Windows**</span><span class="sxs-lookup"><span data-stu-id="8fbce-133">**Windows**</span></span> | <span data-ttu-id="8fbce-134">**Linux**</span><span class="sxs-lookup"><span data-stu-id="8fbce-134">**Linux**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8fbce-135">Installation av VM-agenten</span><span class="sxs-lookup"><span data-stu-id="8fbce-135">Installing the VM agent</span></span> |<span data-ttu-id="8fbce-136">Ladda ned och installera [agentens MSI-fil](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="8fbce-136">Download and install the [agent MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).</span></span> <span data-ttu-id="8fbce-137">Du måste ha administratörsbehörighet för att slutföra installationen.</span><span class="sxs-lookup"><span data-stu-id="8fbce-137">You need administrator privileges to complete the installation.</span></span> |<span data-ttu-id="8fbce-138">Installera senaste [Linux-agenten](../virtual-machines/linux/agent-user-guide.md).</span><span class="sxs-lookup"><span data-stu-id="8fbce-138">Install the latest [Linux agent](../virtual-machines/linux/agent-user-guide.md).</span></span> <span data-ttu-id="8fbce-139">Du måste ha administratörsbehörighet för att slutföra installationen.</span><span class="sxs-lookup"><span data-stu-id="8fbce-139">You need administrator privileges to complete the installation.</span></span> <span data-ttu-id="8fbce-140">Vi rekommenderar att du installerar agenten från databasen för din distribution.</span><span class="sxs-lookup"><span data-stu-id="8fbce-140">We recommend installing the agent from your distribution repository.</span></span> <span data-ttu-id="8fbce-141">Vi *rekommenderar inte* Linux VM-agenten installeras direkt från GitHub.</span><span class="sxs-lookup"><span data-stu-id="8fbce-141">We *do not recommend* installing the Linux VM agent directly from GitHub.</span></span>  |
| <span data-ttu-id="8fbce-142">Verifiera installationen av VM</span><span class="sxs-lookup"><span data-stu-id="8fbce-142">Validating the VM agent installation</span></span> |<span data-ttu-id="8fbce-143">1. Bläddra till mappen C:\WindowsAzure\Packages i Azure-VM.</span><span class="sxs-lookup"><span data-stu-id="8fbce-143">1. Browse to the C:\WindowsAzure\Packages folder in the Azure VM.</span></span> <span data-ttu-id="8fbce-144">Du bör se filen WaAppAgent.exe.</span><span class="sxs-lookup"><span data-stu-id="8fbce-144">You should see the WaAppAgent.exe file.</span></span> <br><span data-ttu-id="8fbce-145">2. Högerklicka på filen, gå till **Egenskaper** och välj fliken **Information**.</span><span class="sxs-lookup"><span data-stu-id="8fbce-145">2. Right-click the file, go to **Properties**, and then select the **Details** tab.</span></span> <span data-ttu-id="8fbce-146">Den **produktversionen** fältet måste innehålla 2.6.1198.718 eller högre.</span><span class="sxs-lookup"><span data-stu-id="8fbce-146">The **Product Version** field should be 2.6.1198.718 or higher.</span></span> |<span data-ttu-id="8fbce-147">Saknas</span><span class="sxs-lookup"><span data-stu-id="8fbce-147">N/A</span></span> |


### <a name="step-3-remove-the-mobility-service-from-the-migrated-virtual-machine"></a><span data-ttu-id="8fbce-148">Steg 3: Ta bort mobilitetstjänsten från den migrerade virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="8fbce-148">Step 3: Remove the Mobility service from the migrated virtual machine</span></span>

<span data-ttu-id="8fbce-149">Om du har migrerat dina lokala VMware-datorer eller fysiska servrar på Windows-/ Linux, måste du manuellt ta bort eller avinstallera mobilitetstjänsten från den migrerade virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8fbce-149">If you have migrated your on-premises VMware machines or physical servers on Windows/Linux, you need to manually remove/uninstall the Mobility service from the migrated virtual machine.</span></span>

>[!IMPORTANT]
><span data-ttu-id="8fbce-150">Det här steget krävs inte för Hyper-V virtuella datorer migreras till Azure.</span><span class="sxs-lookup"><span data-stu-id="8fbce-150">This step is not required for Hyper-V VMs migrated to Azure.</span></span>

#### <a name="uninstall-the-mobility-service-on-a-windows-server-vm"></a><span data-ttu-id="8fbce-151">Avinstallera mobilitetstjänsten på en Windows Server-VM</span><span class="sxs-lookup"><span data-stu-id="8fbce-151">Uninstall the Mobility service on a Windows Server VM</span></span>
<span data-ttu-id="8fbce-152">Använd någon av följande metoder för att avinstallera mobilitetstjänsten på Windows Server-dator.</span><span class="sxs-lookup"><span data-stu-id="8fbce-152">Use one of the following methods to uninstall the Mobility service on a Windows Server computer.</span></span>

##### <a name="uninstall-by-using-the-windows-ui"></a><span data-ttu-id="8fbce-153">Avinstallera med hjälp av Windows-Användargränssnittet</span><span class="sxs-lookup"><span data-stu-id="8fbce-153">Uninstall by using the Windows UI</span></span>
1. <span data-ttu-id="8fbce-154">I Kontrollpanelen, Välj **program**.</span><span class="sxs-lookup"><span data-stu-id="8fbce-154">In the Control Panel, select **Programs**.</span></span>
2. <span data-ttu-id="8fbce-155">Välj **Microsoft Azure Site Recovery Mobility tjänsten eller Huvudtjänsten målservern**, och välj sedan **avinstallera**.</span><span class="sxs-lookup"><span data-stu-id="8fbce-155">Select **Microsoft Azure Site Recovery Mobility Service/Master Target server**, and then select **Uninstall**.</span></span>

##### <a name="uninstall-at-a-command-prompt"></a><span data-ttu-id="8fbce-156">Avinstallera i en kommandotolk</span><span class="sxs-lookup"><span data-stu-id="8fbce-156">Uninstall at a command prompt</span></span>
1. <span data-ttu-id="8fbce-157">Öppna ett kommandotolksfönster som administratör.</span><span class="sxs-lookup"><span data-stu-id="8fbce-157">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="8fbce-158">Om du vill avinstallera mobilitetstjänsten, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="8fbce-158">To uninstall the Mobility service, run the following command:</span></span>

   ```
   MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
   ```

#### <a name="uninstall-the-mobility-service-on-a-linux-computer"></a><span data-ttu-id="8fbce-159">Avinstallera mobilitetstjänsten på en Linux-dator</span><span class="sxs-lookup"><span data-stu-id="8fbce-159">Uninstall the Mobility service on a Linux computer</span></span>
1. <span data-ttu-id="8fbce-160">Logga in på Linux-servern som en **rot** användare.</span><span class="sxs-lookup"><span data-stu-id="8fbce-160">On your Linux server, sign in as a **root** user.</span></span>
2. <span data-ttu-id="8fbce-161">Gå till /user/local/ASR i en terminal.</span><span class="sxs-lookup"><span data-stu-id="8fbce-161">In a terminal, go to /user/local/ASR.</span></span>
3. <span data-ttu-id="8fbce-162">Om du vill avinstallera mobilitetstjänsten, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="8fbce-162">To uninstall the Mobility service, run the following command:</span></span>

   ```
   uninstall.sh -Y
   ```

### <a name="step-4-restart-the-vm"></a><span data-ttu-id="8fbce-163">Steg 4: Starta om den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="8fbce-163">Step 4: Restart the VM</span></span>

<span data-ttu-id="8fbce-164">När du avinstallerar mobilitetstjänsten startar du om den virtuella datorn innan du konfigurerar replikering till en annan Azure-region.</span><span class="sxs-lookup"><span data-stu-id="8fbce-164">After you uninstall the Mobility service, restart the VM before you set up replication to another Azure region.</span></span>


## <a name="next-steps"></a><span data-ttu-id="8fbce-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8fbce-165">Next steps</span></span>
- <span data-ttu-id="8fbce-166">Börja skydda dina arbetsbelastningar av [replikering av Azure virtuella datorer](site-recovery-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="8fbce-166">Start protecting your workloads by [replicating Azure virtual machines](site-recovery-azure-to-azure.md).</span></span>
- <span data-ttu-id="8fbce-167">Lär dig mer om [nätverk vägledning för att replikera virtuella datorer i Azure](site-recovery-azure-to-azure-networking-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="8fbce-167">Learn more about [networking guidance for replicating Azure virtual machines](site-recovery-azure-to-azure-networking-guidance.md).</span></span>
