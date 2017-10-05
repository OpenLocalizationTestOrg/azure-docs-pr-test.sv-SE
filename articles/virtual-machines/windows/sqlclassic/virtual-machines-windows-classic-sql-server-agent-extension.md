---
title: "Automatisera hanteringsuppgifter på SQL virtuella datorer (klassisk) | Microsoft Docs"
description: "Det här avsnittet beskriver hur du hanterar SQL Server agent-tillägg som automatiserar specifika administrationsuppgifter för SQL Server. Dessa inkluderar automatisk säkerhetskopiering, automatisk uppdatering och Azure Key Vault-integrering. Det här avsnittet använder läget för klassisk distribution."
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
ms.openlocfilehash: 30fa9128cd51a7498449c991b58500ad9acdd3d4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-the-sql-server-agent-extension-classic"></a><span data-ttu-id="99650-105">Automatisera hanteringsuppgifter på Azure Virtual Machines med SQL Server Agent-tillägget (klassisk)</span><span class="sxs-lookup"><span data-stu-id="99650-105">Automate management tasks on Azure Virtual Machines with the SQL Server Agent Extension (Classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="99650-106">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="99650-106">Resource Manager</span></span>](../sql/virtual-machines-windows-sql-server-agent-extension.md)
> * [<span data-ttu-id="99650-107">Klassisk</span><span class="sxs-lookup"><span data-stu-id="99650-107">Classic</span></span>](../classic/sql-server-agent-extension.md)
> 
>
 
<span data-ttu-id="99650-108">SQL Server IaaS Agent tillägget (SQLIaaSAgent) körs på virtuella Azure-datorer att automatisera administrationsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="99650-108">The SQL Server IaaS Agent Extension (SQLIaaSAgent) runs on Azure virtual machines to automate administration tasks.</span></span> <span data-ttu-id="99650-109">Det här avsnittet innehåller en översikt över de tjänster som stöds av tillägget samt anvisningar för installation, status och borttagning.</span><span class="sxs-lookup"><span data-stu-id="99650-109">This topic provides an overview of the services supported by the extension as well as instructions for installation, status, and removal.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="99650-110">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="99650-110">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="99650-111">Den här artikeln täcker den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="99650-111">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="99650-112">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="99650-112">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="99650-113">Resource Manager-versionen av den här artikeln finns [SQL Server Agent-tillägg för SQL Server VMs Resource Manager](../sql/virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="99650-113">To view the Resource Manager version of this article, see [SQL Server Agent Extension for SQL Server VMs Resource Manager](../sql/virtual-machines-windows-sql-server-agent-extension.md).</span></span>

## <a name="supported-services"></a><span data-ttu-id="99650-114">Tjänster som stöds</span><span class="sxs-lookup"><span data-stu-id="99650-114">Supported services</span></span>
<span data-ttu-id="99650-115">SQL Server IaaS Agent tillägget stöder följande administrationsuppgifter:</span><span class="sxs-lookup"><span data-stu-id="99650-115">The SQL Server IaaS Agent Extension supports the following administration tasks:</span></span>

| <span data-ttu-id="99650-116">Funktionen för administration</span><span class="sxs-lookup"><span data-stu-id="99650-116">Administration feature</span></span> | <span data-ttu-id="99650-117">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="99650-117">Description</span></span> |
| --- | --- |
| <span data-ttu-id="99650-118">**Automatisk SQL-säkerhetskopiering**</span><span class="sxs-lookup"><span data-stu-id="99650-118">**SQL Automated Backup**</span></span> |<span data-ttu-id="99650-119">Automatiserar schemaläggning av säkerhetskopieringar för alla databaser för standardinstansen av SQL Server på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="99650-119">Automates the scheduling of backups for all databases for the default instance of SQL Server in the VM.</span></span> <span data-ttu-id="99650-120">Mer information finns i [automatisk säkerhetskopiering för SQL Server i Azure Virtual Machines (klassisk)](../classic/sql-automated-backup.md).</span><span class="sxs-lookup"><span data-stu-id="99650-120">For more information, see [Automated backup for SQL Server in Azure Virtual Machines (Classic)](../classic/sql-automated-backup.md).</span></span> |
| <span data-ttu-id="99650-121">**Automatisk SQL-uppdatering**</span><span class="sxs-lookup"><span data-stu-id="99650-121">**SQL Automated Patching**</span></span> |<span data-ttu-id="99650-122">Konfigurerar en underhållsperiod då uppdateringar till den virtuella datorn kan ske, så du kan undvika uppdateringar under Högbelastningstider för din arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="99650-122">Configures a maintenance window during which updates to your VM can take place, so  you can avoid updates during peak times for your workload.</span></span> <span data-ttu-id="99650-123">Mer information finns i [automatisk uppdatering för SQL Server i Azure Virtual Machines (klassisk)](../classic/sql-automated-patching.md).</span><span class="sxs-lookup"><span data-stu-id="99650-123">For more information, see [Automated patching for SQL Server in Azure Virtual Machines (Classic)](../classic/sql-automated-patching.md).</span></span> |
| <span data-ttu-id="99650-124">**Azure Key Vault-integrering**</span><span class="sxs-lookup"><span data-stu-id="99650-124">**Azure Key Vault Integration**</span></span> |<span data-ttu-id="99650-125">Kan du automatiskt installera och konfigurera Azure Key Vault på SQL Server-VM.</span><span class="sxs-lookup"><span data-stu-id="99650-125">Enables you to automatically install and configure Azure Key Vault on your SQL Server VM.</span></span> <span data-ttu-id="99650-126">Mer information finns i [konfigurera Azure Key Vault-integrering för SQL Server på Azure Virtual Machines (klassisk)](../classic/ps-sql-keyvault.md).</span><span class="sxs-lookup"><span data-stu-id="99650-126">For more information, see [Configure Azure Key Vault Integration for SQL Server on Azure VMs (Classic)](../classic/ps-sql-keyvault.md).</span></span> |

## <a name="prerequisites"></a><span data-ttu-id="99650-127">Krav</span><span class="sxs-lookup"><span data-stu-id="99650-127">Prerequisites</span></span>
<span data-ttu-id="99650-128">Krav för att använda SQL Server IaaS Agent tillägget på den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="99650-128">Requirements to use the SQL Server IaaS Agent Extension on your VM:</span></span>

### <a name="operating-system"></a><span data-ttu-id="99650-129">Operativsystem:</span><span class="sxs-lookup"><span data-stu-id="99650-129">Operating System:</span></span>
* <span data-ttu-id="99650-130">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="99650-130">Windows Server 2012</span></span>
* <span data-ttu-id="99650-131">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="99650-131">Windows Server 2012 R2</span></span>
* <span data-ttu-id="99650-132">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="99650-132">Windows Server 2016</span></span>

### <a name="sql-server-versions"></a><span data-ttu-id="99650-133">SQL Server-versioner:</span><span class="sxs-lookup"><span data-stu-id="99650-133">SQL Server versions:</span></span>
* <span data-ttu-id="99650-134">SQLServer 2012</span><span class="sxs-lookup"><span data-stu-id="99650-134">SQL Server 2012</span></span>
* <span data-ttu-id="99650-135">SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="99650-135">SQL Server 2014</span></span>
* <span data-ttu-id="99650-136">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="99650-136">SQL Server 2016</span></span>

### <a name="azure-powershell"></a><span data-ttu-id="99650-137">Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="99650-137">Azure PowerShell:</span></span>
<span data-ttu-id="99650-138">[Hämta och konfigurera de senaste Azure PowerShell-kommandon](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="99650-138">[Download and configure the latest Azure PowerShell commands](/powershell/azure/overview).</span></span>

<span data-ttu-id="99650-139">Starta Windows PowerShell och ansluta till din Azure-prenumeration med den **Add-AzureAccount** kommando.</span><span class="sxs-lookup"><span data-stu-id="99650-139">Start Windows PowerShell, and connect it to your Azure subscription with the **Add-AzureAccount** command.</span></span>

    Add-AzureAccount

<span data-ttu-id="99650-140">Om du har flera prenumerationer använder **Välj AzureSubscription** att välja den prenumeration som innehåller mål-klassiska VM.</span><span class="sxs-lookup"><span data-stu-id="99650-140">If you have multiple subscriptions, use **Select-AzureSubscription** to select the subscription that contains your target classic VM.</span></span>

    Select-AzureSubscription -SubscriptionName <subscriptionname>

<span data-ttu-id="99650-141">Nu kan du hämta en lista över de klassiska virtuella datorerna och deras associerade tjänstnamn med den **Get-AzureVM** kommando.</span><span class="sxs-lookup"><span data-stu-id="99650-141">At this point, you can get a list of the classic virtual machines and their associated service names with the **Get-AzureVM** command.</span></span>

    Get-AzureVM

## <a name="installation"></a><span data-ttu-id="99650-142">Installation</span><span class="sxs-lookup"><span data-stu-id="99650-142">Installation</span></span>
<span data-ttu-id="99650-143">För klassiska virtuella datorer, måste du använda PowerShell för att installera tillägget SQL Server IaaS-agenten och konfigurera tillhörande tjänster.</span><span class="sxs-lookup"><span data-stu-id="99650-143">For classic VMs, you must use PowerShell to install the SQL Server IaaS Agent Extension and configure its associated services.</span></span> <span data-ttu-id="99650-144">Använd den **Set AzureVMSqlServerExtension** PowerShell-cmdlet för att installera tillägget.</span><span class="sxs-lookup"><span data-stu-id="99650-144">Use the **Set-AzureVMSqlServerExtension** PowerShell cmdlet to install the extension.</span></span> <span data-ttu-id="99650-145">Följande kommando tillägget installeras på en Windows Server-VM (klassisk) och namnges ”SQLIaaSExtension”.</span><span class="sxs-lookup"><span data-stu-id="99650-145">For example, the following command installs the extension on a Windows Server VM (classic) and names it "SQLIaaSExtension".</span></span>

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -ReferenceName "SQLIaasExtension" -Version "1.2" | Update-AzureVM

<span data-ttu-id="99650-146">Om du uppdaterar till den senaste versionen av tillägget SQL IaaS-agenten måste du starta om den virtuella datorn när du har uppdaterat tillägget.</span><span class="sxs-lookup"><span data-stu-id="99650-146">If you update to the latest version of the SQL IaaS Agent Extension, you must restart your virtual machine after updating the extension.</span></span>

> [!NOTE]
> <span data-ttu-id="99650-147">Klassiska virtuella datorer har inte ett alternativ för att installera och konfigurera SQL IaaS Agent tillägget via portalen.</span><span class="sxs-lookup"><span data-stu-id="99650-147">Classic virtual machines do not have an option to install and configure the SQL IaaS Agent Extension through the portal.</span></span>
> 
> 

## <a name="status"></a><span data-ttu-id="99650-148">Status</span><span class="sxs-lookup"><span data-stu-id="99650-148">Status</span></span>
<span data-ttu-id="99650-149">Ett sätt att kontrollera att tillägget installeras är att visa agentens status i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="99650-149">One way to verify that the extension is installed is to view the agent status in the Azure Portal.</span></span> <span data-ttu-id="99650-150">Välj en virtuell dator som anges i bladet för virtuella datorer och klicka sedan på **tillägg**.</span><span class="sxs-lookup"><span data-stu-id="99650-150">Select a virtual machine listed in the virtual machine blade, and then click on **Extensions**.</span></span> <span data-ttu-id="99650-151">Du bör se den **SQLIaaSAgent** tillägg i listan.</span><span class="sxs-lookup"><span data-stu-id="99650-151">You should see the **SQLIaaSAgent** extension listed.</span></span>

![SQL Server IaaS-Agent tillägget i Azure-portalen](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

<span data-ttu-id="99650-153">Du kan också använda den **Get-AzureVMSqlServerExtension** Azure Powershell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="99650-153">You can also use the **Get-AzureVMSqlServerExtension** Azure Powershell cmdlet.</span></span>

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## <a name="removal"></a><span data-ttu-id="99650-154">Borttagning</span><span class="sxs-lookup"><span data-stu-id="99650-154">Removal</span></span>
<span data-ttu-id="99650-155">I Azure-portalen kan du avinstallera tillägget genom att klicka på knappen på den **tillägg** bladet för de virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="99650-155">In the Azure Portal, you can uninstall the extension by clicking the ellipsis on the **Extensions** blade of your virtual machine properties.</span></span> <span data-ttu-id="99650-156">Klicka på **avinstallera**.</span><span class="sxs-lookup"><span data-stu-id="99650-156">Then click **Uninstall**.</span></span>

![Avinstallera SQL Server IaaS-Agent tillägget i Azure-portalen](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

<span data-ttu-id="99650-158">Du kan också använda den **ta bort AzureVMSqlServerExtension** Powershell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="99650-158">You can also use the **Remove-AzureVMSqlServerExtension** Powershell cmdlet.</span></span>

    Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## <a name="next-steps"></a><span data-ttu-id="99650-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="99650-159">Next Steps</span></span>
<span data-ttu-id="99650-160">Börja med en av de tjänster som stöds av tillägget.</span><span class="sxs-lookup"><span data-stu-id="99650-160">Begin using one of the services supported by the extension.</span></span> <span data-ttu-id="99650-161">Mer information finns i avsnitt som refereras till i den [-tjänster som stöds](#supported-services) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="99650-161">For more details, see the topics referenced in the [Supported services](#supported-services) section of this article.</span></span>

<span data-ttu-id="99650-162">Mer information om hur du kör SQL Server på Azure Virtual Machines finns [SQL Server på Azure virtuella datorer – översikt](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="99650-162">For more information about running SQL Server on Azure Virtual Machines, see [SQL Server on Azure Virtual Machines overview](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

