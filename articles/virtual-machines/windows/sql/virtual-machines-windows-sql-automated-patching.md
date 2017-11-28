---
title: "aaaAutomated korrigering för SQL Server-datorer (Resource Manager) | Microsoft Docs"
description: "Beskriver funktionen för automatisk uppdatering av hello för SQL Server virtuella datorer som körs i Azure med hjälp av hanteraren för filserverresurser."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 58232e92-318f-456b-8f0a-2201a541e08d
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: 8bb8d0fb265e69d7bbf1fa047f5ceef02e7c56fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-resource-manager"></a><span data-ttu-id="951a1-103">Automatisk uppdatering av SQL Server i Azure Virtual Machines (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="951a1-103">Automated Patching for SQL Server in Azure Virtual Machines (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="951a1-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="951a1-104">Resource Manager</span></span>](virtual-machines-windows-sql-automated-patching.md)
> * [<span data-ttu-id="951a1-105">Klassisk</span><span class="sxs-lookup"><span data-stu-id="951a1-105">Classic</span></span>](../classic/sql-automated-patching.md)
> 
> 

<span data-ttu-id="951a1-106">Automatisk korrigering upprättar en underhållsperiod för en Azure-dator som kör SQL Server.</span><span class="sxs-lookup"><span data-stu-id="951a1-106">Automated Patching establishes a maintenance window for an Azure Virtual Machine running SQL Server.</span></span> <span data-ttu-id="951a1-107">Automatiska uppdateringar kan endast installeras under den här underhållsperioden.</span><span class="sxs-lookup"><span data-stu-id="951a1-107">Automated Updates can only be installed during this maintenance window.</span></span> <span data-ttu-id="951a1-108">För SQL Server säkerställer den här rescriction att uppdateringar och eventuella tillhörande omstarter inträffar på hello bästa möjliga tid för hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="951a1-108">For SQL Server, this rescriction ensures that system updates and any associated restarts occur at hello best possible time for hello database.</span></span> <span data-ttu-id="951a1-109">Automatisk korrigering beror på hello [tillägg för SQL Server IaaS Agent](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="951a1-109">Automated Patching depends on hello [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

<span data-ttu-id="951a1-110">tooview hello klassiska versionen av den här artikeln finns [automatisk uppdatering för SQL Server i Azure virtuella datorer klassiska](../classic/sql-automated-patching.md).</span><span class="sxs-lookup"><span data-stu-id="951a1-110">tooview hello classic version of this article, see [Automated Patching for SQL Server in Azure Virtual Machines Classic](../classic/sql-automated-patching.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="951a1-111">Krav</span><span class="sxs-lookup"><span data-stu-id="951a1-111">Prerequisites</span></span>
<span data-ttu-id="951a1-112">toouse automatisk uppdatering, Överväg hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="951a1-112">toouse Automated Patching, consider hello following prerequisites:</span></span>

<span data-ttu-id="951a1-113">**Operativsystemet**:</span><span class="sxs-lookup"><span data-stu-id="951a1-113">**Operating System**:</span></span>

* <span data-ttu-id="951a1-114">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="951a1-114">Windows Server 2012</span></span>
* <span data-ttu-id="951a1-115">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="951a1-115">Windows Server 2012 R2</span></span>
* <span data-ttu-id="951a1-116">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="951a1-116">Windows Server 2016</span></span>

<span data-ttu-id="951a1-117">**SQL Server-version**:</span><span class="sxs-lookup"><span data-stu-id="951a1-117">**SQL Server version**:</span></span>

* <span data-ttu-id="951a1-118">SQLServer 2012</span><span class="sxs-lookup"><span data-stu-id="951a1-118">SQL Server 2012</span></span>
* <span data-ttu-id="951a1-119">SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="951a1-119">SQL Server 2014</span></span>
* <span data-ttu-id="951a1-120">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="951a1-120">SQL Server 2016</span></span>

<span data-ttu-id="951a1-121">**Azure PowerShell**:</span><span class="sxs-lookup"><span data-stu-id="951a1-121">**Azure PowerShell**:</span></span>

* <span data-ttu-id="951a1-122">[Installera hello senaste Azure PowerShell-kommandon](/powershell/azure/overview) om du planerar tooconfigure automatisk uppdatering med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="951a1-122">[Install hello latest Azure PowerShell commands](/powershell/azure/overview) if you plan tooconfigure Automated Patching with PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="951a1-123">Automatisk korrigering beroende hello tillägg för SQL Server IaaS Agent.</span><span class="sxs-lookup"><span data-stu-id="951a1-123">Automated Patching relies on hello SQL Server IaaS Agent Extension.</span></span> <span data-ttu-id="951a1-124">Aktuell SQL virtuella galleriavbildningar lägga till tillägget som standard.</span><span class="sxs-lookup"><span data-stu-id="951a1-124">Current SQL virtual machine gallery images add this extension by default.</span></span> <span data-ttu-id="951a1-125">Mer information finns i [tillägg för SQL Server IaaS Agent](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="951a1-125">For more information, see [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>
> 
> 

## <a name="settings"></a><span data-ttu-id="951a1-126">Inställningar</span><span class="sxs-lookup"><span data-stu-id="951a1-126">Settings</span></span>
<span data-ttu-id="951a1-127">hello beskrivs följande tabell hello-alternativ som kan konfigureras för automatisk uppdatering.</span><span class="sxs-lookup"><span data-stu-id="951a1-127">hello following table describes hello options that can be configured for Automated Patching.</span></span> <span data-ttu-id="951a1-128">hello faktiska konfigurationssteg varierar beroende på om du använder hello Azure-portalen eller Azure Windows PowerShell-kommandon.</span><span class="sxs-lookup"><span data-stu-id="951a1-128">hello actual configuration steps vary depending on whether you use hello Azure portal or Azure Windows PowerShell commands.</span></span>

| <span data-ttu-id="951a1-129">Inställning</span><span class="sxs-lookup"><span data-stu-id="951a1-129">Setting</span></span> | <span data-ttu-id="951a1-130">Möjliga värden</span><span class="sxs-lookup"><span data-stu-id="951a1-130">Possible values</span></span> | <span data-ttu-id="951a1-131">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="951a1-131">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="951a1-132">**Automatisk uppdatering**</span><span class="sxs-lookup"><span data-stu-id="951a1-132">**Automated Patching**</span></span> |<span data-ttu-id="951a1-133">Aktivera/inaktivera (inaktiverat)</span><span class="sxs-lookup"><span data-stu-id="951a1-133">Enable/Disable (Disabled)</span></span> |<span data-ttu-id="951a1-134">Aktiverar eller inaktiverar automatisk uppdatering för en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="951a1-134">Enables or disables Automated Patching for an Azure virtual machine.</span></span> |
| <span data-ttu-id="951a1-135">**Underhållsschema**</span><span class="sxs-lookup"><span data-stu-id="951a1-135">**Maintenance schedule**</span></span> |<span data-ttu-id="951a1-136">Varje dag, måndag, tisdag, onsdag, torsdag, fredag, lördag, söndag</span><span class="sxs-lookup"><span data-stu-id="951a1-136">Everyday, Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday</span></span> |<span data-ttu-id="951a1-137">hello schema för att hämta och installera Windows, SQL Server och Microsoft-uppdateringar för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="951a1-137">hello schedule for downloading and installing Windows, SQL Server, and Microsoft updates for your virtual machine.</span></span> |
| <span data-ttu-id="951a1-138">**Starttimme för underhåll**</span><span class="sxs-lookup"><span data-stu-id="951a1-138">**Maintenance start hour**</span></span> |<span data-ttu-id="951a1-139">0-24</span><span class="sxs-lookup"><span data-stu-id="951a1-139">0-24</span></span> |<span data-ttu-id="951a1-140">hello lokala start tid tooupdate hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="951a1-140">hello local start time tooupdate hello virtual machine.</span></span> |
| <span data-ttu-id="951a1-141">**Underhåll fönstervaraktigheten**</span><span class="sxs-lookup"><span data-stu-id="951a1-141">**Maintenance window duration**</span></span> |<span data-ttu-id="951a1-142">30-180</span><span class="sxs-lookup"><span data-stu-id="951a1-142">30-180</span></span> |<span data-ttu-id="951a1-143">hello antal minuter som tillåts toocomplete hello hämtning och installation av uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="951a1-143">hello number of minutes permitted toocomplete hello download and installation of updates.</span></span> |
| <span data-ttu-id="951a1-144">**Patch-kategori**</span><span class="sxs-lookup"><span data-stu-id="951a1-144">**Patch Category**</span></span> |<span data-ttu-id="951a1-145">Viktigt</span><span class="sxs-lookup"><span data-stu-id="951a1-145">Important</span></span> |<span data-ttu-id="951a1-146">hello kategori uppdateringar toodownload och installation.</span><span class="sxs-lookup"><span data-stu-id="951a1-146">hello category of updates toodownload and install.</span></span> |

## <a name="configuration-in-hello-portal"></a><span data-ttu-id="951a1-147">Konfigurationen i hello Portal</span><span class="sxs-lookup"><span data-stu-id="951a1-147">Configuration in hello Portal</span></span>
<span data-ttu-id="951a1-148">Du kan använda hello Azure portal tooconfigure automatisk uppdatering under etableringen eller för befintliga virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="951a1-148">You can use hello Azure portal tooconfigure Automated Patching during provisioning or for existing VMs.</span></span>

### <a name="new-vms"></a><span data-ttu-id="951a1-149">Nya virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="951a1-149">New VMs</span></span>
<span data-ttu-id="951a1-150">Använd hello Azure portal tooconfigure automatisk uppdatering när du skapar en ny virtuell dator för SQL Server i hello Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="951a1-150">Use hello Azure portal tooconfigure Automated Patching when you create a new SQL Server Virtual Machine in hello Resource Manager deployment model.</span></span>

<span data-ttu-id="951a1-151">I hello **SQL Server-inställningar** bladet väljer **automatisk uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="951a1-151">In hello **SQL Server settings** blade, select **Automated patching**.</span></span> <span data-ttu-id="951a1-152">hello följande Azure portal skärmbild visar hello **SQL automatisk uppdatering** bladet.</span><span class="sxs-lookup"><span data-stu-id="951a1-152">hello following Azure portal screenshot shows hello **SQL Automated Patching** blade.</span></span>

![SQL automatisk uppdatering i Azure-portalen](./media/virtual-machines-windows-sql-automated-patching/azure-sql-arm-patching.png)

<span data-ttu-id="951a1-154">Kontexten, i avsnittet hello slutförts på [etablera en virtuell dator i SQL Server i Azure](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="951a1-154">For context, see hello complete topic on [provisioning a SQL Server virtual machine in Azure](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

### <a name="existing-vms"></a><span data-ttu-id="951a1-155">Befintliga virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="951a1-155">Existing VMs</span></span>
<span data-ttu-id="951a1-156">Välj den virtuella datorn för SQL Server för befintliga SQL Server virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="951a1-156">For existing SQL Server virtual machines, select your SQL Server virtual machine.</span></span> <span data-ttu-id="951a1-157">Välj hello **SQL Server-konfigurationsfilen** avsnitt i hello **inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="951a1-157">Then select hello **SQL Server configuration** section of hello **Settings** blade.</span></span>

![SQL automatisk uppdatering av befintliga virtuella datorer](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-existing-vms.png)

<span data-ttu-id="951a1-159">I hello **SQL Server-konfigurationsfilen** bladet, klickar du på hello **redigera** knapp i hello automatisk uppdatering avsnitt.</span><span class="sxs-lookup"><span data-stu-id="951a1-159">In hello **SQL Server configuration** blade, click hello **Edit** button in hello Automated patching section.</span></span>

![Konfigurera SQL automatisk uppdatering för befintliga virtuella datorer](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-configuration.png)

<span data-ttu-id="951a1-161">När du är klar klickar du på hello **OK** hello längst ned på hello-knappen **SQL Server-konfigurationsfilen** bladet toosave ändringarna.</span><span class="sxs-lookup"><span data-stu-id="951a1-161">When finished, click hello **OK** button on hello bottom of hello **SQL Server configuration** blade toosave your changes.</span></span>

<span data-ttu-id="951a1-162">Om du aktiverar automatisk uppdatering för hello första gången, konfigurerar Azure hello SQL Server IaaS-Agent i hello bakgrund.</span><span class="sxs-lookup"><span data-stu-id="951a1-162">If you are enabling Automated Patching for hello first time, Azure configures hello SQL Server IaaS Agent in hello background.</span></span> <span data-ttu-id="951a1-163">Under denna tid kanske hello Azure-portalen inte visar att automatisk uppdatering har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="951a1-163">During this time, hello Azure portal might not show that Automated Patching is configured.</span></span> <span data-ttu-id="951a1-164">Vänta några minuter för hello agent toobe installerat kan konfigureras.</span><span class="sxs-lookup"><span data-stu-id="951a1-164">Wait several minutes for hello agent toobe installed, configured.</span></span> <span data-ttu-id="951a1-165">Efter att hello Azure visar portal hello nya inställningar.</span><span class="sxs-lookup"><span data-stu-id="951a1-165">After that hello Azure portal reflects hello new settings.</span></span>

> [!NOTE]
> <span data-ttu-id="951a1-166">Du kan också konfigurera automatisk uppdatering med hjälp av en mall.</span><span class="sxs-lookup"><span data-stu-id="951a1-166">You can also configure Automated Patching using a template.</span></span> <span data-ttu-id="951a1-167">Mer information finns i [Azure quickstart-mall för automatisk uppdatering](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autopatching-update).</span><span class="sxs-lookup"><span data-stu-id="951a1-167">For more information, see [Azure quickstart template for Automated Patching](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autopatching-update).</span></span>
> 
> 

## <a name="configuration-with-powershell"></a><span data-ttu-id="951a1-168">Med PowerShell</span><span class="sxs-lookup"><span data-stu-id="951a1-168">Configuration with PowerShell</span></span>
<span data-ttu-id="951a1-169">När du har etablerat din SQL-VM, använder du PowerShell tooconfigure automatisk uppdatering.</span><span class="sxs-lookup"><span data-stu-id="951a1-169">After provisioning your SQL VM, use PowerShell tooconfigure Automated Patching.</span></span>

<span data-ttu-id="951a1-170">I följande exempel hello, PowerShell är används tooconfigure automatisk uppdatering på en befintlig SQL Server-VM.</span><span class="sxs-lookup"><span data-stu-id="951a1-170">In hello following example, PowerShell is used tooconfigure Automated Patching on an existing SQL Server VM.</span></span> <span data-ttu-id="951a1-171">Hej **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig** kommando konfigurerar en ny underhållsperiod för automatiska uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="951a1-171">hello **AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig** command configures a new maintenance window for automatic updates.</span></span>

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $aps = AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Set-AzureRmVMSqlServerExtension -AutoPatchingSettings $aps -VMName $vmname -ResourceGroupName $resourcegroupname

<span data-ttu-id="951a1-172">Baserat på det här exemplet hello följande tabell beskrivs hello praktiken på hello målet virtuella Azure-datorn:</span><span class="sxs-lookup"><span data-stu-id="951a1-172">Based on this example, hello following table describes hello practical effect on hello target Azure VM:</span></span>

| <span data-ttu-id="951a1-173">Parameter</span><span class="sxs-lookup"><span data-stu-id="951a1-173">Parameter</span></span> | <span data-ttu-id="951a1-174">Verkan</span><span class="sxs-lookup"><span data-stu-id="951a1-174">Effect</span></span> |
| --- | --- |
| <span data-ttu-id="951a1-175">**DayOfWeek**</span><span class="sxs-lookup"><span data-stu-id="951a1-175">**DayOfWeek**</span></span> |<span data-ttu-id="951a1-176">Korrigeringsprogram installerade varje torsdag.</span><span class="sxs-lookup"><span data-stu-id="951a1-176">Patches installed every Thursday.</span></span> |
| <span data-ttu-id="951a1-177">**MaintenanceWindowStartingHour**</span><span class="sxs-lookup"><span data-stu-id="951a1-177">**MaintenanceWindowStartingHour**</span></span> |<span data-ttu-id="951a1-178">Begin uppdateringar på 11:00:00.</span><span class="sxs-lookup"><span data-stu-id="951a1-178">Begin updates at 11:00am.</span></span> |
| <span data-ttu-id="951a1-179">**MaintenanceWindowsDuration**</span><span class="sxs-lookup"><span data-stu-id="951a1-179">**MaintenanceWindowsDuration**</span></span> |<span data-ttu-id="951a1-180">Korrigeringsfiler måste installeras inom 120 minuter.</span><span class="sxs-lookup"><span data-stu-id="951a1-180">Patches must be installed within 120 minutes.</span></span> <span data-ttu-id="951a1-181">Baserat på hello starttid, måste de utföra av 1:00 pm.</span><span class="sxs-lookup"><span data-stu-id="951a1-181">Based on hello start time, they must complete by 1:00pm.</span></span> |
| <span data-ttu-id="951a1-182">**PatchCategory**</span><span class="sxs-lookup"><span data-stu-id="951a1-182">**PatchCategory**</span></span> |<span data-ttu-id="951a1-183">Hej bara möjligt inställningen för den här parametern är **viktigt**.</span><span class="sxs-lookup"><span data-stu-id="951a1-183">hello only possible setting for this parameter is **Important**.</span></span> |

<span data-ttu-id="951a1-184">Det kan ta flera minuter tooinstall och konfigurera hello SQL Server IaaS-Agent.</span><span class="sxs-lookup"><span data-stu-id="951a1-184">It could take several minutes tooinstall and configure hello SQL Server IaaS Agent.</span></span>

<span data-ttu-id="951a1-185">toodisable automatisk uppdatering kör hello samma skript utan hello **-aktivera** parametern toohello **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig**.</span><span class="sxs-lookup"><span data-stu-id="951a1-185">toodisable Automated Patching, run hello same script without hello **-Enable** parameter toohello **AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig**.</span></span> <span data-ttu-id="951a1-186">Hej frånvaron av hello **-aktivera** parametern signaler hello kommandot toodisable hello funktionen.</span><span class="sxs-lookup"><span data-stu-id="951a1-186">hello absence of hello **-Enable** parameter signals hello command toodisable hello feature.</span></span>

## <a name="next-steps"></a><span data-ttu-id="951a1-187">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="951a1-187">Next steps</span></span>
<span data-ttu-id="951a1-188">Information om andra tillgängliga automation-aktiviteter finns [tillägg för SQL Server IaaS Agent](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="951a1-188">For information about other available automation tasks, see [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

<span data-ttu-id="951a1-189">Mer information om hur du kör SQL Server på Azure Virtual Machines finns [SQL Server på Azure virtuella datorer – översikt](virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="951a1-189">For more information about running SQL Server on Azure VMs, see [SQL Server on Azure Virtual Machines overview](virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

