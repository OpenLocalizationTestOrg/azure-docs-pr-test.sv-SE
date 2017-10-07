---
title: "aaaAutomate hanteringsuppgifter på SQL virtuella datorer (klassisk) | Microsoft Docs"
description: "Det här avsnittet beskrivs hur toomanage hello SQL Server agent-tillägg som automatiserar specifika administrationsuppgifter för SQL Server. Dessa inkluderar automatisk säkerhetskopiering, automatisk uppdatering och Azure Key Vault-integrering. Det här avsnittet använder hello klassisk distribution läge."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: a9bda2e7-cdba-427c-bc30-77cde4376f3a
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a1dc011e0526845701eaf0c365007938f9ee32ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-hello-sql-server-agent-extension-classic"></a><span data-ttu-id="fb5a0-105">Automatisera hanteringsuppgifter på Azure Virtual Machines med hello tillägg för SQL Server Agent (klassisk)</span><span class="sxs-lookup"><span data-stu-id="fb5a0-105">Automate management tasks on Azure Virtual Machines with hello SQL Server Agent Extension (Classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fb5a0-106">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="fb5a0-106">Resource Manager</span></span>](../sql/virtual-machines-windows-sql-server-agent-extension.md)
> * [<span data-ttu-id="fb5a0-107">Klassisk</span><span class="sxs-lookup"><span data-stu-id="fb5a0-107">Classic</span></span>](../classic/sql-server-agent-extension.md)
> 
>
 
<span data-ttu-id="fb5a0-108">hello tillägg för SQL Server IaaS Agent (SQLIaaSAgent) körs på virtuella Azure-datorer tooautomate administrationsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="fb5a0-108">hello SQL Server IaaS Agent Extension (SQLIaaSAgent) runs on Azure virtual machines tooautomate administration tasks.</span></span> <span data-ttu-id="fb5a0-109">Det här avsnittet innehåller en översikt över hello-tjänster som stöds av hello tillägget samt anvisningar för installation, status och borttagning.</span><span class="sxs-lookup"><span data-stu-id="fb5a0-109">This topic provides an overview of hello services supported by hello extension as well as instructions for installation, status, and removal.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="fb5a0-110">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="fb5a0-110">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="fb5a0-111">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="fb5a0-111">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="fb5a0-112">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="fb5a0-112">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="fb5a0-113">tooview hello Resource Manager-versionen av den här artikeln finns [SQL Server Agent-tillägg för SQL Server VMs Resource Manager](../sql/virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="fb5a0-113">tooview hello Resource Manager version of this article, see [SQL Server Agent Extension for SQL Server VMs Resource Manager](../sql/virtual-machines-windows-sql-server-agent-extension.md).</span></span>

## <a name="supported-services"></a><span data-ttu-id="fb5a0-114">Tjänster som stöds</span><span class="sxs-lookup"><span data-stu-id="fb5a0-114">Supported services</span></span>
<span data-ttu-id="fb5a0-115">hello tillägg för SQL Server IaaS Agent stöder hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="fb5a0-115">hello SQL Server IaaS Agent Extension supports hello following administration tasks:</span></span>

| <span data-ttu-id="fb5a0-116">Funktionen för administration</span><span class="sxs-lookup"><span data-stu-id="fb5a0-116">Administration feature</span></span> | <span data-ttu-id="fb5a0-117">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fb5a0-117">Description</span></span> |
| --- | --- |
| <span data-ttu-id="fb5a0-118">**Automatisk SQL-säkerhetskopiering**</span><span class="sxs-lookup"><span data-stu-id="fb5a0-118">**SQL Automated Backup**</span></span> |<span data-ttu-id="fb5a0-119">Automatiserar hello schemaläggning av säkerhetskopieringar för alla databaser för hello standardinstansen av SQL Server i hello VM.</span><span class="sxs-lookup"><span data-stu-id="fb5a0-119">Automates hello scheduling of backups for all databases for hello default instance of SQL Server in hello VM.</span></span> <span data-ttu-id="fb5a0-120">Mer information finns i [automatisk säkerhetskopiering för SQL Server i Azure Virtual Machines (klassisk)](../classic/sql-automated-backup.md).</span><span class="sxs-lookup"><span data-stu-id="fb5a0-120">For more information, see [Automated backup for SQL Server in Azure Virtual Machines (Classic)](../classic/sql-automated-backup.md).</span></span> |
| <span data-ttu-id="fb5a0-121">**Automatisk SQL-uppdatering**</span><span class="sxs-lookup"><span data-stu-id="fb5a0-121">**SQL Automated Patching**</span></span> |<span data-ttu-id="fb5a0-122">Konfigurerar en underhållsperiod tooyour VM ta placera under vilka uppdateringar, så att du kan undvika uppdateringar under Högbelastningstider för din arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="fb5a0-122">Configures a maintenance window during which updates tooyour VM can take place, so  you can avoid updates during peak times for your workload.</span></span> <span data-ttu-id="fb5a0-123">Mer information finns i [automatisk uppdatering för SQL Server i Azure Virtual Machines (klassisk)](../classic/sql-automated-patching.md).</span><span class="sxs-lookup"><span data-stu-id="fb5a0-123">For more information, see [Automated patching for SQL Server in Azure Virtual Machines (Classic)](../classic/sql-automated-patching.md).</span></span> |
| <span data-ttu-id="fb5a0-124">**Azure Key Vault-integrering**</span><span class="sxs-lookup"><span data-stu-id="fb5a0-124">**Azure Key Vault Integration**</span></span> |<span data-ttu-id="fb5a0-125">Aktiverar du tooautomatically installera och konfigurera Azure Key Vault på SQL Server-VM.</span><span class="sxs-lookup"><span data-stu-id="fb5a0-125">Enables you tooautomatically install and configure Azure Key Vault on your SQL Server VM.</span></span> <span data-ttu-id="fb5a0-126">Mer information finns i [konfigurera Azure Key Vault-integrering för SQL Server på Azure Virtual Machines (klassisk)](../classic/ps-sql-keyvault.md).</span><span class="sxs-lookup"><span data-stu-id="fb5a0-126">For more information, see [Configure Azure Key Vault Integration for SQL Server on Azure VMs (Classic)](../classic/ps-sql-keyvault.md).</span></span> |

## <a name="prerequisites"></a><span data-ttu-id="fb5a0-127">Krav</span><span class="sxs-lookup"><span data-stu-id="fb5a0-127">Prerequisites</span></span>
<span data-ttu-id="fb5a0-128">Krav för toouse hello SQL Server IaaS Agent tillägget på den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="fb5a0-128">Requirements toouse hello SQL Server IaaS Agent Extension on your VM:</span></span>

### <a name="operating-system"></a><span data-ttu-id="fb5a0-129">Operativsystem:</span><span class="sxs-lookup"><span data-stu-id="fb5a0-129">Operating System:</span></span>
* <span data-ttu-id="fb5a0-130">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="fb5a0-130">Windows Server 2012</span></span>
* <span data-ttu-id="fb5a0-131">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="fb5a0-131">Windows Server 2012 R2</span></span>
* <span data-ttu-id="fb5a0-132">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="fb5a0-132">Windows Server 2016</span></span>

### <a name="sql-server-versions"></a><span data-ttu-id="fb5a0-133">SQL Server-versioner:</span><span class="sxs-lookup"><span data-stu-id="fb5a0-133">SQL Server versions:</span></span>
* <span data-ttu-id="fb5a0-134">SQLServer 2012</span><span class="sxs-lookup"><span data-stu-id="fb5a0-134">SQL Server 2012</span></span>
* <span data-ttu-id="fb5a0-135">SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="fb5a0-135">SQL Server 2014</span></span>
* <span data-ttu-id="fb5a0-136">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="fb5a0-136">SQL Server 2016</span></span>

### <a name="azure-powershell"></a><span data-ttu-id="fb5a0-137">Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="fb5a0-137">Azure PowerShell:</span></span>
<span data-ttu-id="fb5a0-138">[Hämta och konfigurera hello senaste Azure PowerShell-kommandon](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="fb5a0-138">[Download and configure hello latest Azure PowerShell commands](/powershell/azure/overview).</span></span>

<span data-ttu-id="fb5a0-139">Starta Windows PowerShell och ansluta tooyour Azure-prenumeration med hello **Add-AzureAccount** kommando.</span><span class="sxs-lookup"><span data-stu-id="fb5a0-139">Start Windows PowerShell, and connect it tooyour Azure subscription with hello **Add-AzureAccount** command.</span></span>

    Add-AzureAccount

<span data-ttu-id="fb5a0-140">Om du har flera prenumerationer använder **Välj AzureSubscription** tooselect hello prenumeration som innehåller mål-klassiska VM.</span><span class="sxs-lookup"><span data-stu-id="fb5a0-140">If you have multiple subscriptions, use **Select-AzureSubscription** tooselect hello subscription that contains your target classic VM.</span></span>

    Select-AzureSubscription -SubscriptionName <subscriptionname>

<span data-ttu-id="fb5a0-141">Nu kan du hämta en lista över hello klassiska virtuella datorer och deras associerade tjänstnamn med hello **Get-AzureVM** kommando.</span><span class="sxs-lookup"><span data-stu-id="fb5a0-141">At this point, you can get a list of hello classic virtual machines and their associated service names with hello **Get-AzureVM** command.</span></span>

    Get-AzureVM

## <a name="installation"></a><span data-ttu-id="fb5a0-142">Installation</span><span class="sxs-lookup"><span data-stu-id="fb5a0-142">Installation</span></span>
<span data-ttu-id="fb5a0-143">Klassiska virtuella datorer, måste du använda PowerShell tooinstall hello tillägg för SQL Server IaaS Agent och konfigurera tillhörande tjänster.</span><span class="sxs-lookup"><span data-stu-id="fb5a0-143">For classic VMs, you must use PowerShell tooinstall hello SQL Server IaaS Agent Extension and configure its associated services.</span></span> <span data-ttu-id="fb5a0-144">Använd hello **Set AzureVMSqlServerExtension** PowerShell cmdlet tooinstall hello tillägg.</span><span class="sxs-lookup"><span data-stu-id="fb5a0-144">Use hello **Set-AzureVMSqlServerExtension** PowerShell cmdlet tooinstall hello extension.</span></span> <span data-ttu-id="fb5a0-145">Följande kommando hello installerar hello tillägg på en Windows Server-VM (klassisk) och kallar det ”SQLIaaSExtension”.</span><span class="sxs-lookup"><span data-stu-id="fb5a0-145">For example, hello following command installs hello extension on a Windows Server VM (classic) and names it "SQLIaaSExtension".</span></span>

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -ReferenceName "SQLIaasExtension" -Version "1.2" | Update-AzureVM

<span data-ttu-id="fb5a0-146">Om du uppdaterar toohello senaste versionen av hello SQL IaaS Agent tillägget måste du starta om den virtuella datorn när du har uppdaterat hello-tillägget.</span><span class="sxs-lookup"><span data-stu-id="fb5a0-146">If you update toohello latest version of hello SQL IaaS Agent Extension, you must restart your virtual machine after updating hello extension.</span></span>

> [!NOTE]
> <span data-ttu-id="fb5a0-147">Klassiska virtuella datorer inte har en alternativet tooinstall och konfigurera hello SQL IaaS Agent tillägget hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="fb5a0-147">Classic virtual machines do not have an option tooinstall and configure hello SQL IaaS Agent Extension through hello portal.</span></span>
> 
> 

## <a name="status"></a><span data-ttu-id="fb5a0-148">Status</span><span class="sxs-lookup"><span data-stu-id="fb5a0-148">Status</span></span>
<span data-ttu-id="fb5a0-149">Enkelriktade tooverify att hello tillägg har installerats är tooview hello agentens status hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fb5a0-149">One way tooverify that hello extension is installed is tooview hello agent status in hello Azure Portal.</span></span> <span data-ttu-id="fb5a0-150">Välj en virtuell dator som anges i hello virtuella bladet och klicka sedan på **tillägg**.</span><span class="sxs-lookup"><span data-stu-id="fb5a0-150">Select a virtual machine listed in hello virtual machine blade, and then click on **Extensions**.</span></span> <span data-ttu-id="fb5a0-151">Du bör se hello **SQLIaaSAgent** tillägg i listan.</span><span class="sxs-lookup"><span data-stu-id="fb5a0-151">You should see hello **SQLIaaSAgent** extension listed.</span></span>

![SQL Server IaaS-Agent tillägget i Azure-portalen](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

<span data-ttu-id="fb5a0-153">Du kan också använda hello **Get-AzureVMSqlServerExtension** Azure Powershell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="fb5a0-153">You can also use hello **Get-AzureVMSqlServerExtension** Azure Powershell cmdlet.</span></span>

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## <a name="removal"></a><span data-ttu-id="fb5a0-154">Borttagning</span><span class="sxs-lookup"><span data-stu-id="fb5a0-154">Removal</span></span>
<span data-ttu-id="fb5a0-155">I hello Azure-portalen, du kan avinstallera hello tillägg genom att klicka på hello ellips på hello **tillägg** bladet för de virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="fb5a0-155">In hello Azure Portal, you can uninstall hello extension by clicking hello ellipsis on hello **Extensions** blade of your virtual machine properties.</span></span> <span data-ttu-id="fb5a0-156">Klicka på **avinstallera**.</span><span class="sxs-lookup"><span data-stu-id="fb5a0-156">Then click **Uninstall**.</span></span>

![Avinstallera hello SQL Server IaaS Agent tillägget i Azure Portal](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

<span data-ttu-id="fb5a0-158">Du kan också använda hello **ta bort AzureVMSqlServerExtension** Powershell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="fb5a0-158">You can also use hello **Remove-AzureVMSqlServerExtension** Powershell cmdlet.</span></span>

    Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## <a name="next-steps"></a><span data-ttu-id="fb5a0-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fb5a0-159">Next Steps</span></span>
<span data-ttu-id="fb5a0-160">Börja använda någon av hello-tjänster som stöds av hello extension.</span><span class="sxs-lookup"><span data-stu-id="fb5a0-160">Begin using one of hello services supported by hello extension.</span></span> <span data-ttu-id="fb5a0-161">Mer information finns i hello information som refereras i hello [-tjänster som stöds](#supported-services) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="fb5a0-161">For more details, see hello topics referenced in hello [Supported services](#supported-services) section of this article.</span></span>

<span data-ttu-id="fb5a0-162">Mer information om hur du kör SQL Server på Azure Virtual Machines finns [SQL Server på Azure virtuella datorer – översikt](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fb5a0-162">For more information about running SQL Server on Azure Virtual Machines, see [SQL Server on Azure Virtual Machines overview](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

