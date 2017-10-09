---
title: "aaaAutomated korrigering för SQL Server-datorer (klassisk) | Microsoft Docs"
description: "Beskriver funktionen för automatisk uppdatering av hello för SQL Server virtuella datorer som körs i Azure med hjälp av hello klassisk distribution läge."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 737b2f65-08b9-4f54-b867-e987730265a8
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: 2ef06b95705fc457605d6eb2fbc0afd490321843
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-classic"></a><span data-ttu-id="3e3c6-103">Automatisk uppdatering för SQLServer på virtuella Azure-datorer (klassisk)</span><span class="sxs-lookup"><span data-stu-id="3e3c6-103">Automated Patching for SQL Server in Azure Virtual Machines (Classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3e3c6-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="3e3c6-104">Resource Manager</span></span>](../sql/virtual-machines-windows-sql-automated-patching.md)
> * [<span data-ttu-id="3e3c6-105">Klassisk</span><span class="sxs-lookup"><span data-stu-id="3e3c6-105">Classic</span></span>](../classic/sql-automated-patching.md)
> 
> 

<span data-ttu-id="3e3c6-106">Automatisk korrigering upprättar en underhållsperiod för en Azure-dator som kör SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3e3c6-106">Automated Patching establishes a maintenance window for an Azure Virtual Machine running SQL Server.</span></span> <span data-ttu-id="3e3c6-107">Automatiska uppdateringar kan endast installeras under den här underhållsperioden.</span><span class="sxs-lookup"><span data-stu-id="3e3c6-107">Automated Updates can only be installed during this maintenance window.</span></span> <span data-ttu-id="3e3c6-108">För SQL Server säkerställer detta att uppdateringar och eventuella tillhörande omstarter inträffar på hello bästa möjliga tid för hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="3e3c6-108">For SQL Server, this ensures that system updates and any associated restarts occur at hello best possible time for hello database.</span></span> <span data-ttu-id="3e3c6-109">Automatisk korrigering beror på hello [tillägg för SQL Server IaaS Agent](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="3e3c6-109">Automated Patching depends on hello [SQL Server IaaS Agent Extension](../classic/sql-server-agent-extension.md).</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="3e3c6-110">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="3e3c6-110">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="3e3c6-111">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="3e3c6-111">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="3e3c6-112">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="3e3c6-112">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="3e3c6-113">tooview hello Resource Manager-versionen av den här artikeln finns [automatisk uppdatering för SQL Server i Azure virtuella datorer Resource Manager](../sql/virtual-machines-windows-sql-automated-patching.md).</span><span class="sxs-lookup"><span data-stu-id="3e3c6-113">tooview hello Resource Manager version of this article, see [Automated Patching for SQL Server in Azure Virtual Machines Resource Manager](../sql/virtual-machines-windows-sql-automated-patching.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3e3c6-114">Krav</span><span class="sxs-lookup"><span data-stu-id="3e3c6-114">Prerequisites</span></span>
<span data-ttu-id="3e3c6-115">toouse automatisk uppdatering, Överväg hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="3e3c6-115">toouse Automated Patching, consider hello following prerequisites:</span></span>

<span data-ttu-id="3e3c6-116">**Operativsystemet**:</span><span class="sxs-lookup"><span data-stu-id="3e3c6-116">**Operating System**:</span></span>

* <span data-ttu-id="3e3c6-117">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="3e3c6-117">Windows Server 2012</span></span>
* <span data-ttu-id="3e3c6-118">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="3e3c6-118">Windows Server 2012 R2</span></span>
* <span data-ttu-id="3e3c6-119">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="3e3c6-119">Windows Server 2016</span></span>

<span data-ttu-id="3e3c6-120">**SQL Server-version**:</span><span class="sxs-lookup"><span data-stu-id="3e3c6-120">**SQL Server version**:</span></span>

* <span data-ttu-id="3e3c6-121">SQLServer 2012</span><span class="sxs-lookup"><span data-stu-id="3e3c6-121">SQL Server 2012</span></span>
* <span data-ttu-id="3e3c6-122">SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="3e3c6-122">SQL Server 2014</span></span>
* <span data-ttu-id="3e3c6-123">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="3e3c6-123">SQL Server 2016</span></span>

<span data-ttu-id="3e3c6-124">**Azure PowerShell**:</span><span class="sxs-lookup"><span data-stu-id="3e3c6-124">**Azure PowerShell**:</span></span>

* <span data-ttu-id="3e3c6-125">[Installera hello senaste Azure PowerShell-kommandon](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3e3c6-125">[Install hello latest Azure PowerShell commands](/powershell/azure/overview).</span></span>

<span data-ttu-id="3e3c6-126">**SQL Server IaaS tillägget**:</span><span class="sxs-lookup"><span data-stu-id="3e3c6-126">**SQL Server IaaS Extension**:</span></span>

* <span data-ttu-id="3e3c6-127">[Installera hello SQL Server IaaS tillägget](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="3e3c6-127">[Install hello SQL Server IaaS Extension](../classic/sql-server-agent-extension.md).</span></span>

## <a name="settings"></a><span data-ttu-id="3e3c6-128">Inställningar</span><span class="sxs-lookup"><span data-stu-id="3e3c6-128">Settings</span></span>
<span data-ttu-id="3e3c6-129">hello beskrivs följande tabell hello-alternativ som kan konfigureras för automatisk uppdatering.</span><span class="sxs-lookup"><span data-stu-id="3e3c6-129">hello following table describes hello options that can be configured for Automated Patching.</span></span> <span data-ttu-id="3e3c6-130">För klassiska virtuella datorer kan använda du PowerShell tooconfigure inställningarna.</span><span class="sxs-lookup"><span data-stu-id="3e3c6-130">For classic VMs, you must use PowerShell tooconfigure these settings.</span></span>

| <span data-ttu-id="3e3c6-131">Inställning</span><span class="sxs-lookup"><span data-stu-id="3e3c6-131">Setting</span></span> | <span data-ttu-id="3e3c6-132">Möjliga värden</span><span class="sxs-lookup"><span data-stu-id="3e3c6-132">Possible values</span></span> | <span data-ttu-id="3e3c6-133">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3e3c6-133">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e3c6-134">**Automatisk uppdatering**</span><span class="sxs-lookup"><span data-stu-id="3e3c6-134">**Automated Patching**</span></span> |<span data-ttu-id="3e3c6-135">Aktivera/inaktivera (inaktiverat)</span><span class="sxs-lookup"><span data-stu-id="3e3c6-135">Enable/Disable (Disabled)</span></span> |<span data-ttu-id="3e3c6-136">Aktiverar eller inaktiverar automatisk uppdatering för en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="3e3c6-136">Enables or disables Automated Patching for an Azure virtual machine.</span></span> |
| <span data-ttu-id="3e3c6-137">**Underhållsschema**</span><span class="sxs-lookup"><span data-stu-id="3e3c6-137">**Maintenance schedule**</span></span> |<span data-ttu-id="3e3c6-138">Varje dag, måndag, tisdag, onsdag, torsdag, fredag, lördag, söndag</span><span class="sxs-lookup"><span data-stu-id="3e3c6-138">Everyday, Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday</span></span> |<span data-ttu-id="3e3c6-139">hello schema för att hämta och installera Windows, SQL Server och Microsoft-uppdateringar för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="3e3c6-139">hello schedule for downloading and installing Windows, SQL Server, and Microsoft updates for your virtual machine.</span></span> |
| <span data-ttu-id="3e3c6-140">**Starttimme för underhåll**</span><span class="sxs-lookup"><span data-stu-id="3e3c6-140">**Maintenance start hour**</span></span> |<span data-ttu-id="3e3c6-141">0-24</span><span class="sxs-lookup"><span data-stu-id="3e3c6-141">0-24</span></span> |<span data-ttu-id="3e3c6-142">hello lokala start tid tooupdate hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="3e3c6-142">hello local start time tooupdate hello virtual machine.</span></span> |
| <span data-ttu-id="3e3c6-143">**Underhåll fönstervaraktigheten**</span><span class="sxs-lookup"><span data-stu-id="3e3c6-143">**Maintenance window duration**</span></span> |<span data-ttu-id="3e3c6-144">30-180</span><span class="sxs-lookup"><span data-stu-id="3e3c6-144">30-180</span></span> |<span data-ttu-id="3e3c6-145">hello antal minuter som tillåts toocomplete hello hämtning och installation av uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="3e3c6-145">hello number of minutes permitted toocomplete hello download and installation of updates.</span></span> |
| <span data-ttu-id="3e3c6-146">**Patch-kategori**</span><span class="sxs-lookup"><span data-stu-id="3e3c6-146">**Patch Category**</span></span> |<span data-ttu-id="3e3c6-147">Viktigt</span><span class="sxs-lookup"><span data-stu-id="3e3c6-147">Important</span></span> |<span data-ttu-id="3e3c6-148">hello kategori uppdateringar toodownload och installation.</span><span class="sxs-lookup"><span data-stu-id="3e3c6-148">hello category of updates toodownload and install.</span></span> |

## <a name="configuration-with-powershell"></a><span data-ttu-id="3e3c6-149">Med PowerShell</span><span class="sxs-lookup"><span data-stu-id="3e3c6-149">Configuration with PowerShell</span></span>
<span data-ttu-id="3e3c6-150">I följande exempel hello, PowerShell är används tooconfigure automatisk uppdatering på en befintlig SQL Server-VM.</span><span class="sxs-lookup"><span data-stu-id="3e3c6-150">In hello following example, PowerShell is used tooconfigure Automated Patching on an existing SQL Server VM.</span></span> <span data-ttu-id="3e3c6-151">Hej **ny AzureVMSqlServerAutoPatchingConfig** kommando konfigurerar en ny underhållsperiod för automatiska uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="3e3c6-151">hello **New-AzureVMSqlServerAutoPatchingConfig** command configures a new maintenance window for automatic updates.</span></span>

    $aps = New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoPatchingSettings $aps | Update-AzureVM

<span data-ttu-id="3e3c6-152">Baserat på det här exemplet hello följande tabell beskrivs hello praktiken på hello målet virtuella Azure-datorn:</span><span class="sxs-lookup"><span data-stu-id="3e3c6-152">Based on this example, hello following table describes hello practical effect on hello target Azure VM:</span></span>

| <span data-ttu-id="3e3c6-153">Parameter</span><span class="sxs-lookup"><span data-stu-id="3e3c6-153">Parameter</span></span> | <span data-ttu-id="3e3c6-154">Verkan</span><span class="sxs-lookup"><span data-stu-id="3e3c6-154">Effect</span></span> |
| --- | --- |
| <span data-ttu-id="3e3c6-155">**DayOfWeek**</span><span class="sxs-lookup"><span data-stu-id="3e3c6-155">**DayOfWeek**</span></span> |<span data-ttu-id="3e3c6-156">Korrigeringsprogram installerade varje torsdag.</span><span class="sxs-lookup"><span data-stu-id="3e3c6-156">Patches installed every Thursday.</span></span> |
| <span data-ttu-id="3e3c6-157">**MaintenanceWindowStartingHour**</span><span class="sxs-lookup"><span data-stu-id="3e3c6-157">**MaintenanceWindowStartingHour**</span></span> |<span data-ttu-id="3e3c6-158">Begin uppdateringar på 11:00:00.</span><span class="sxs-lookup"><span data-stu-id="3e3c6-158">Begin updates at 11:00am.</span></span> |
| <span data-ttu-id="3e3c6-159">**MaintenanceWindowsDuration**</span><span class="sxs-lookup"><span data-stu-id="3e3c6-159">**MaintenanceWindowsDuration**</span></span> |<span data-ttu-id="3e3c6-160">Korrigeringsfiler måste installeras inom 120 minuter.</span><span class="sxs-lookup"><span data-stu-id="3e3c6-160">Patches must be installed within 120 minutes.</span></span> <span data-ttu-id="3e3c6-161">Baserat på hello starttid, måste de utföra av 1:00 pm.</span><span class="sxs-lookup"><span data-stu-id="3e3c6-161">Based on hello start time, they must complete by 1:00pm.</span></span> |
| <span data-ttu-id="3e3c6-162">**PatchCategory**</span><span class="sxs-lookup"><span data-stu-id="3e3c6-162">**PatchCategory**</span></span> |<span data-ttu-id="3e3c6-163">hello endast är möjligt för den här parametern ”viktigt”.</span><span class="sxs-lookup"><span data-stu-id="3e3c6-163">hello only possible setting for this parameter is “Important”.</span></span> |

<span data-ttu-id="3e3c6-164">Det kan ta flera minuter tooinstall och konfigurera hello SQL Server IaaS-Agent.</span><span class="sxs-lookup"><span data-stu-id="3e3c6-164">It could take several minutes tooinstall and configure hello SQL Server IaaS Agent.</span></span>

<span data-ttu-id="3e3c6-165">toodisable automatisk uppdatering kör hello samma skript utan hello - aktivera parametern toohello ny AzureVMSqlServerAutoPatchingConfig.</span><span class="sxs-lookup"><span data-stu-id="3e3c6-165">toodisable Automated Patching, run hello same script without hello -Enable parameter toohello New-AzureVMSqlServerAutoPatchingConfig.</span></span> <span data-ttu-id="3e3c6-166">Som med installation, det kan ta flera minuter toodisable automatisk uppdatering.</span><span class="sxs-lookup"><span data-stu-id="3e3c6-166">As with installation, it can take several minutes toodisable Automated Patching.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3e3c6-167">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3e3c6-167">Next steps</span></span>
<span data-ttu-id="3e3c6-168">Information om andra tillgängliga automation-aktiviteter finns [tillägg för SQL Server IaaS Agent](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="3e3c6-168">For information about other available automation tasks, see [SQL Server IaaS Agent Extension](../classic/sql-server-agent-extension.md).</span></span>

<span data-ttu-id="3e3c6-169">Mer information om hur du kör SQL Server på Azure Virtual Machines finns [SQL Server på Azure virtuella datorer – översikt](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3e3c6-169">For more information about running SQL Server on Azure VMs, see [SQL Server on Azure Virtual Machines overview](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

