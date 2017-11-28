---
title: "Automatisera hanteringsuppgifter på SQL virtuella datorer (Resource Manager) | Microsoft Docs"
description: "Det här avsnittet beskriver hur du hanterar SQL Server agent-tillägg som automatiserar specifika administrationsuppgifter för SQL Server. Dessa inkluderar automatisk säkerhetskopiering, automatisk uppdatering och Azure Key Vault-integrering. Det här avsnittet använder distributionsläget för Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: effe4e2f-35b5-490a-b5ef-b06746083da4
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/07/2017
ms.author: jroth
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7152d184bb6d1d4b81aeb47e2c7c9160ada36023
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-the-sql-server-agent-extension-resource-manager"></a><span data-ttu-id="432f8-105">Automatisera hanteringsuppgifter på Azure Virtual Machines med SQL Server Agent-tillägget (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="432f8-105">Automate management tasks on Azure Virtual Machines with the SQL Server Agent Extension (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="432f8-106">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="432f8-106">Resource Manager</span></span>](virtual-machines-windows-sql-server-agent-extension.md)
> * [<span data-ttu-id="432f8-107">Klassisk</span><span class="sxs-lookup"><span data-stu-id="432f8-107">Classic</span></span>](../classic/sql-server-agent-extension.md)
> 
> 

<span data-ttu-id="432f8-108">SQL Server IaaS Agent tillägget (SQLIaaSExtension) körs på virtuella Azure-datorer att automatisera administrationsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="432f8-108">The SQL Server IaaS Agent Extension (SQLIaaSExtension) runs on Azure virtual machines to automate administration tasks.</span></span> <span data-ttu-id="432f8-109">Det här avsnittet innehåller en översikt över de tjänster som stöds av tillägget samt anvisningar för installation, status och borttagning.</span><span class="sxs-lookup"><span data-stu-id="432f8-109">This topic provides an overview of the services supported by the extension as well as instructions for installation, status, and removal.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

<span data-ttu-id="432f8-110">Den klassiska versionen av den här artikeln finns [SQL Server Agent-tillägget för SQL Server VMs klassiska](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="432f8-110">To view the classic version of this article, see [SQL Server Agent Extension for SQL Server VMs Classic](../classic/sql-server-agent-extension.md).</span></span>

## <a name="supported-services"></a><span data-ttu-id="432f8-111">Tjänster som stöds</span><span class="sxs-lookup"><span data-stu-id="432f8-111">Supported services</span></span>
<span data-ttu-id="432f8-112">SQL Server IaaS Agent tillägget stöder följande administrationsuppgifter:</span><span class="sxs-lookup"><span data-stu-id="432f8-112">The SQL Server IaaS Agent Extension supports the following administration tasks:</span></span>

| <span data-ttu-id="432f8-113">Funktionen för administration</span><span class="sxs-lookup"><span data-stu-id="432f8-113">Administration feature</span></span> | <span data-ttu-id="432f8-114">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="432f8-114">Description</span></span> |
| --- | --- |
| <span data-ttu-id="432f8-115">**Automatisk SQL-säkerhetskopiering**</span><span class="sxs-lookup"><span data-stu-id="432f8-115">**SQL Automated Backup**</span></span> |<span data-ttu-id="432f8-116">Automatiserar schemaläggning av säkerhetskopieringar för alla databaser för standardinstansen av SQL Server på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="432f8-116">Automates the scheduling of backups for all databases for the default instance of SQL Server in the VM.</span></span> <span data-ttu-id="432f8-117">Mer information finns i [automatisk säkerhetskopiering för SQL Server i Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-backup.md).</span><span class="sxs-lookup"><span data-stu-id="432f8-117">For more information, see [Automated backup for SQL Server in Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-backup.md).</span></span> |
| <span data-ttu-id="432f8-118">**Automatisk SQL-uppdatering**</span><span class="sxs-lookup"><span data-stu-id="432f8-118">**SQL Automated Patching**</span></span> |<span data-ttu-id="432f8-119">Konfigurerar en underhållsperiod då uppdateringar till den virtuella datorn kan ske, så du kan undvika uppdateringar under Högbelastningstider för din arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="432f8-119">Configures a maintenance window during which updates to your VM can take place, so  you can avoid updates during peak times for your workload.</span></span> <span data-ttu-id="432f8-120">Mer information finns i [automatisk uppdatering för SQL Server i Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-patching.md).</span><span class="sxs-lookup"><span data-stu-id="432f8-120">For more information, see [Automated patching for SQL Server in Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-patching.md).</span></span> |
| <span data-ttu-id="432f8-121">**Azure Key Vault-integrering**</span><span class="sxs-lookup"><span data-stu-id="432f8-121">**Azure Key Vault Integration**</span></span> |<span data-ttu-id="432f8-122">Kan du automatiskt installera och konfigurera Azure Key Vault på SQL Server-VM.</span><span class="sxs-lookup"><span data-stu-id="432f8-122">Enables you to automatically install and configure Azure Key Vault on your SQL Server VM.</span></span> <span data-ttu-id="432f8-123">Mer information finns i [konfigurera Azure Key Vault-integrering för SQL Server på Azure Virtual Machines (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md).</span><span class="sxs-lookup"><span data-stu-id="432f8-123">For more information, see [Configure Azure Key Vault Integration for SQL Server on Azure VMs (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md).</span></span> |

<span data-ttu-id="432f8-124">När du installerade och körs, tillgängliggör SQL Server IaaS Agent tillägget dessa funktioner för administration på SQL Server-panelen på den virtuella datorn i Azure-portalen och via Azure PowerShell för SQL Server marketplace-bilder och via Azure PowerShell för manuella installationer av tillägget.</span><span class="sxs-lookup"><span data-stu-id="432f8-124">Once installed and running, the SQL Server IaaS Agent Extension makes these administration features available on the SQL Server panel of the virtual machine in the Azure Portal and through Azure PowerShell for SQL Server marketplace images, and through Azure PowerShell for manual installations of the extension.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="432f8-125">Krav</span><span class="sxs-lookup"><span data-stu-id="432f8-125">Prerequisites</span></span>
<span data-ttu-id="432f8-126">Krav för att använda SQL Server IaaS Agent tillägget på den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="432f8-126">Requirements to use the SQL Server IaaS Agent Extension on your VM:</span></span>

<span data-ttu-id="432f8-127">**Operativsystemet**:</span><span class="sxs-lookup"><span data-stu-id="432f8-127">**Operating System**:</span></span>

* <span data-ttu-id="432f8-128">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="432f8-128">Windows Server 2012</span></span>
* <span data-ttu-id="432f8-129">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="432f8-129">Windows Server 2012 R2</span></span>
* <span data-ttu-id="432f8-130">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="432f8-130">Windows Server 2016</span></span>

<span data-ttu-id="432f8-131">**SQL Server-versioner**:</span><span class="sxs-lookup"><span data-stu-id="432f8-131">**SQL Server versions**:</span></span>

* <span data-ttu-id="432f8-132">SQLServer 2012</span><span class="sxs-lookup"><span data-stu-id="432f8-132">SQL Server 2012</span></span>
* <span data-ttu-id="432f8-133">SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="432f8-133">SQL Server 2014</span></span>
* <span data-ttu-id="432f8-134">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="432f8-134">SQL Server 2016</span></span>

<span data-ttu-id="432f8-135">**Azure PowerShell**:</span><span class="sxs-lookup"><span data-stu-id="432f8-135">**Azure PowerShell**:</span></span>

* [<span data-ttu-id="432f8-136">Hämta och konfigurera de senaste Azure PowerShell-kommandon</span><span class="sxs-lookup"><span data-stu-id="432f8-136">Download and configure the latest Azure PowerShell commands</span></span>](/powershell/azure/overview)

## <a name="installation"></a><span data-ttu-id="432f8-137">Installation</span><span class="sxs-lookup"><span data-stu-id="432f8-137">Installation</span></span>
<span data-ttu-id="432f8-138">Tillägget SQL Server IaaS-Agent installeras automatiskt när du etablerar en galleriavbildningar för SQL Server-virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="432f8-138">The SQL Server IaaS Agent Extension is automatically installed when you provision one of the SQL Server virtual machine gallery images.</span></span> <span data-ttu-id="432f8-139">Om du behöver installera tillägget manuellt på något av dessa SQL Server-datorer kan du använda följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="432f8-139">If you need to reinstall the extension manually on one of these SQL Server VMs, use the following PowerShell command:</span></span>

```powershell
Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2" -Location "East US 2"
```

<span data-ttu-id="432f8-140">Det är också möjligt att installera tillägget SQL Server IaaS-Agent på en virtuell dator bara Operativsystemet Windows Server.</span><span class="sxs-lookup"><span data-stu-id="432f8-140">It is also possible to install the SQL Server IaaS Agent Extension on an OS-only Windows Server virtual machine.</span></span> <span data-ttu-id="432f8-141">Detta stöds endast om du har installerat SQL Server manuellt på den datorn.</span><span class="sxs-lookup"><span data-stu-id="432f8-141">This is only supported if you have also manually installed SQL Server on that machine.</span></span> <span data-ttu-id="432f8-142">Installera tillägget manuellt med hjälp av samma **Set AzureVMSqlServerExtension** PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="432f8-142">Then install the extension manually by using the same **Set-AzureVMSqlServerExtension** PowerShell cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="432f8-143">Om du manuellt installera tillägget SQL Server IaaS-Agent på en OS-endast Windows Server VM, kan du inte hantera konfigurationsinställningarna för SQL Server via Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="432f8-143">If you manually install the SQL Server IaaS Agent Extension on an OS-only Windows Server VM, you can not manage the SQL Server configuration settings through the Azure portal.</span></span> <span data-ttu-id="432f8-144">I det här scenariot måste du göra alla ändringar med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="432f8-144">In this scenario, you must make all changes with PowerShell.</span></span>

## <a name="status"></a><span data-ttu-id="432f8-145">Status</span><span class="sxs-lookup"><span data-stu-id="432f8-145">Status</span></span>
<span data-ttu-id="432f8-146">Ett sätt att kontrollera att tillägget installeras är att visa agentens status i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="432f8-146">One way to verify that the extension is installed is to view the agent status in the Azure Portal.</span></span> <span data-ttu-id="432f8-147">Välj **alla inställningar** i bladet för virtuella datorer och sedan klicka på **tillägg**.</span><span class="sxs-lookup"><span data-stu-id="432f8-147">Select **All settings** in the virtual machine blade, and then click on **Extensions**.</span></span> <span data-ttu-id="432f8-148">Du bör se den **SQLIaaSExtension** tillägg i listan.</span><span class="sxs-lookup"><span data-stu-id="432f8-148">You should see the **SQLIaaSExtension** extension listed.</span></span>

![SQL Server IaaS-Agent tillägget i Azure-portalen](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

<span data-ttu-id="432f8-150">Du kan också använda den **Get-AzureVMSqlServerExtension** Azure Powershell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="432f8-150">You can also use the **Get-AzureVMSqlServerExtension** Azure Powershell cmdlet.</span></span>

    Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

<span data-ttu-id="432f8-151">Föregående kommando bekräftar att agenten är installerad och ger allmän statusinformation.</span><span class="sxs-lookup"><span data-stu-id="432f8-151">The previous command confirms the agent is installed and provides general status information.</span></span> <span data-ttu-id="432f8-152">Du kan också hämta statusinformation om automatisk säkerhetskopiering och korrigering med följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="432f8-152">You can also get specific status information about Automated Backup and Patching with the following commands.</span></span>

    $sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a><span data-ttu-id="432f8-153">Borttagning</span><span class="sxs-lookup"><span data-stu-id="432f8-153">Removal</span></span>
<span data-ttu-id="432f8-154">I Azure-portalen kan du avinstallera tillägget genom att klicka på knappen på den **tillägg** bladet för de virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="432f8-154">In the Azure Portal, you can uninstall the extension by clicking the ellipsis on the **Extensions** blade of your virtual machine properties.</span></span> <span data-ttu-id="432f8-155">Klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="432f8-155">Then click **Delete**.</span></span>

![Avinstallera SQL Server IaaS-Agent tillägget i Azure-portalen](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

<span data-ttu-id="432f8-157">Du kan också använda den **ta bort AzureRmVMSqlServerExtension** Powershell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="432f8-157">You can also use the **Remove-AzureRmVMSqlServerExtension** Powershell cmdlet.</span></span>

    Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## <a name="next-steps"></a><span data-ttu-id="432f8-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="432f8-158">Next Steps</span></span>
<span data-ttu-id="432f8-159">Börja med en av de tjänster som stöds av tillägget.</span><span class="sxs-lookup"><span data-stu-id="432f8-159">Begin using one of the services supported by the extension.</span></span> <span data-ttu-id="432f8-160">Mer information finns i avsnitt som refereras till i den [-tjänster som stöds](#supported-services) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="432f8-160">For more details, see the topics referenced in the [Supported services](#supported-services) section of this article.</span></span>

<span data-ttu-id="432f8-161">Mer information om hur du kör SQL Server på Azure Virtual Machines finns [SQL Server på Azure virtuella datorer – översikt](virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="432f8-161">For more information about running SQL Server on Azure Virtual Machines, see [SQL Server on Azure Virtual Machines overview](virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

