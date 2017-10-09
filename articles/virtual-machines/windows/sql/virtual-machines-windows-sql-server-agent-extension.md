---
title: "aaaAutomate hanteringsuppgifter på SQL virtuella datorer (Resource Manager) | Microsoft Docs"
description: "Det här avsnittet beskrivs hur toomanage hello SQL Server agent-tillägg som automatiserar specifika administrationsuppgifter för SQL Server. Dessa inkluderar automatisk säkerhetskopiering, automatisk uppdatering och Azure Key Vault-integrering. Det här avsnittet använder läget för hello Resource Manager distribution."
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
ms.openlocfilehash: ae917612c4af59f12c0b083440673bdc555e9d56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-hello-sql-server-agent-extension-resource-manager"></a><span data-ttu-id="cd2a9-105">Automatisera hanteringsuppgifter på Azure Virtual Machines med hello tillägg för SQL Server Agent (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="cd2a9-105">Automate management tasks on Azure Virtual Machines with hello SQL Server Agent Extension (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="cd2a9-106">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="cd2a9-106">Resource Manager</span></span>](virtual-machines-windows-sql-server-agent-extension.md)
> * [<span data-ttu-id="cd2a9-107">Klassisk</span><span class="sxs-lookup"><span data-stu-id="cd2a9-107">Classic</span></span>](../classic/sql-server-agent-extension.md)
> 
> 

<span data-ttu-id="cd2a9-108">hello tillägg för SQL Server IaaS Agent (SQLIaaSExtension) körs på virtuella Azure-datorer tooautomate administrationsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="cd2a9-108">hello SQL Server IaaS Agent Extension (SQLIaaSExtension) runs on Azure virtual machines tooautomate administration tasks.</span></span> <span data-ttu-id="cd2a9-109">Det här avsnittet innehåller en översikt över hello-tjänster som stöds av hello tillägget samt anvisningar för installation, status och borttagning.</span><span class="sxs-lookup"><span data-stu-id="cd2a9-109">This topic provides an overview of hello services supported by hello extension as well as instructions for installation, status, and removal.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

<span data-ttu-id="cd2a9-110">tooview hello klassiska versionen av den här artikeln finns [SQL Server Agent-tillägget för SQL Server VMs klassiska](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="cd2a9-110">tooview hello classic version of this article, see [SQL Server Agent Extension for SQL Server VMs Classic](../classic/sql-server-agent-extension.md).</span></span>

## <a name="supported-services"></a><span data-ttu-id="cd2a9-111">Tjänster som stöds</span><span class="sxs-lookup"><span data-stu-id="cd2a9-111">Supported services</span></span>
<span data-ttu-id="cd2a9-112">hello tillägg för SQL Server IaaS Agent stöder hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="cd2a9-112">hello SQL Server IaaS Agent Extension supports hello following administration tasks:</span></span>

| <span data-ttu-id="cd2a9-113">Funktionen för administration</span><span class="sxs-lookup"><span data-stu-id="cd2a9-113">Administration feature</span></span> | <span data-ttu-id="cd2a9-114">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cd2a9-114">Description</span></span> |
| --- | --- |
| <span data-ttu-id="cd2a9-115">**Automatisk SQL-säkerhetskopiering**</span><span class="sxs-lookup"><span data-stu-id="cd2a9-115">**SQL Automated Backup**</span></span> |<span data-ttu-id="cd2a9-116">Automatiserar hello schemaläggning av säkerhetskopieringar för alla databaser för hello standardinstansen av SQL Server i hello VM.</span><span class="sxs-lookup"><span data-stu-id="cd2a9-116">Automates hello scheduling of backups for all databases for hello default instance of SQL Server in hello VM.</span></span> <span data-ttu-id="cd2a9-117">Mer information finns i [automatisk säkerhetskopiering för SQL Server i Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-backup.md).</span><span class="sxs-lookup"><span data-stu-id="cd2a9-117">For more information, see [Automated backup for SQL Server in Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-backup.md).</span></span> |
| <span data-ttu-id="cd2a9-118">**Automatisk SQL-uppdatering**</span><span class="sxs-lookup"><span data-stu-id="cd2a9-118">**SQL Automated Patching**</span></span> |<span data-ttu-id="cd2a9-119">Konfigurerar en underhållsperiod tooyour VM ta placera under vilka uppdateringar, så att du kan undvika uppdateringar under Högbelastningstider för din arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="cd2a9-119">Configures a maintenance window during which updates tooyour VM can take place, so  you can avoid updates during peak times for your workload.</span></span> <span data-ttu-id="cd2a9-120">Mer information finns i [automatisk uppdatering för SQL Server i Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-patching.md).</span><span class="sxs-lookup"><span data-stu-id="cd2a9-120">For more information, see [Automated patching for SQL Server in Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-patching.md).</span></span> |
| <span data-ttu-id="cd2a9-121">**Azure Key Vault-integrering**</span><span class="sxs-lookup"><span data-stu-id="cd2a9-121">**Azure Key Vault Integration**</span></span> |<span data-ttu-id="cd2a9-122">Aktiverar du tooautomatically installera och konfigurera Azure Key Vault på SQL Server-VM.</span><span class="sxs-lookup"><span data-stu-id="cd2a9-122">Enables you tooautomatically install and configure Azure Key Vault on your SQL Server VM.</span></span> <span data-ttu-id="cd2a9-123">Mer information finns i [konfigurera Azure Key Vault-integrering för SQL Server på Azure Virtual Machines (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md).</span><span class="sxs-lookup"><span data-stu-id="cd2a9-123">For more information, see [Configure Azure Key Vault Integration for SQL Server on Azure VMs (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md).</span></span> |

<span data-ttu-id="cd2a9-124">När installerad och körs, tillgängliggör hello tillägg för SQL Server IaaS Agent dessa funktioner för administration på hello SQL Server-panelen på hello virtuell dator i hello Azure-portalen och via Azure PowerShell för SQL Server marketplace-bilder och via Azure PowerShell för manuella installationer hello-tillägget.</span><span class="sxs-lookup"><span data-stu-id="cd2a9-124">Once installed and running, hello SQL Server IaaS Agent Extension makes these administration features available on hello SQL Server panel of hello virtual machine in hello Azure Portal and through Azure PowerShell for SQL Server marketplace images, and through Azure PowerShell for manual installations of hello extension.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="cd2a9-125">Krav</span><span class="sxs-lookup"><span data-stu-id="cd2a9-125">Prerequisites</span></span>
<span data-ttu-id="cd2a9-126">Krav för toouse hello SQL Server IaaS Agent tillägget på den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="cd2a9-126">Requirements toouse hello SQL Server IaaS Agent Extension on your VM:</span></span>

<span data-ttu-id="cd2a9-127">**Operativsystemet**:</span><span class="sxs-lookup"><span data-stu-id="cd2a9-127">**Operating System**:</span></span>

* <span data-ttu-id="cd2a9-128">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="cd2a9-128">Windows Server 2012</span></span>
* <span data-ttu-id="cd2a9-129">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="cd2a9-129">Windows Server 2012 R2</span></span>
* <span data-ttu-id="cd2a9-130">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="cd2a9-130">Windows Server 2016</span></span>

<span data-ttu-id="cd2a9-131">**SQL Server-versioner**:</span><span class="sxs-lookup"><span data-stu-id="cd2a9-131">**SQL Server versions**:</span></span>

* <span data-ttu-id="cd2a9-132">SQLServer 2012</span><span class="sxs-lookup"><span data-stu-id="cd2a9-132">SQL Server 2012</span></span>
* <span data-ttu-id="cd2a9-133">SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="cd2a9-133">SQL Server 2014</span></span>
* <span data-ttu-id="cd2a9-134">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="cd2a9-134">SQL Server 2016</span></span>

<span data-ttu-id="cd2a9-135">**Azure PowerShell**:</span><span class="sxs-lookup"><span data-stu-id="cd2a9-135">**Azure PowerShell**:</span></span>

* [<span data-ttu-id="cd2a9-136">Hämta och konfigurera hello senaste Azure PowerShell-kommandon</span><span class="sxs-lookup"><span data-stu-id="cd2a9-136">Download and configure hello latest Azure PowerShell commands</span></span>](/powershell/azure/overview)

## <a name="installation"></a><span data-ttu-id="cd2a9-137">Installation</span><span class="sxs-lookup"><span data-stu-id="cd2a9-137">Installation</span></span>
<span data-ttu-id="cd2a9-138">hello tillägg för SQL Server IaaS Agent installeras automatiskt när du etablerar en hello SQL Server virtuella galleriavbildningar.</span><span class="sxs-lookup"><span data-stu-id="cd2a9-138">hello SQL Server IaaS Agent Extension is automatically installed when you provision one of hello SQL Server virtual machine gallery images.</span></span> <span data-ttu-id="cd2a9-139">Om du behöver tooreinstall hello tillägget manuellt på något av dessa SQL Server-datorer kan använda hello följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="cd2a9-139">If you need tooreinstall hello extension manually on one of these SQL Server VMs, use hello following PowerShell command:</span></span>

```powershell
Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2" -Location "East US 2"
```

<span data-ttu-id="cd2a9-140">Det är också möjligt tooinstall hello tillägg för SQL Server IaaS Agent på en virtuell dator bara Operativsystemet Windows Server.</span><span class="sxs-lookup"><span data-stu-id="cd2a9-140">It is also possible tooinstall hello SQL Server IaaS Agent Extension on an OS-only Windows Server virtual machine.</span></span> <span data-ttu-id="cd2a9-141">Detta stöds endast om du har installerat SQL Server manuellt på den datorn.</span><span class="sxs-lookup"><span data-stu-id="cd2a9-141">This is only supported if you have also manually installed SQL Server on that machine.</span></span> <span data-ttu-id="cd2a9-142">Och sedan installera hello tillägget manuellt med hjälp av hello samma **Set AzureVMSqlServerExtension** PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="cd2a9-142">Then install hello extension manually by using hello same **Set-AzureVMSqlServerExtension** PowerShell cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="cd2a9-143">Om du manuellt installera hello tillägg för SQL Server IaaS Agent på en OS-endast Windows Server VM, kan du inte hantera hello SQL Server-konfigurationsinställningar via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cd2a9-143">If you manually install hello SQL Server IaaS Agent Extension on an OS-only Windows Server VM, you can not manage hello SQL Server configuration settings through hello Azure portal.</span></span> <span data-ttu-id="cd2a9-144">I det här scenariot måste du göra alla ändringar med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cd2a9-144">In this scenario, you must make all changes with PowerShell.</span></span>

## <a name="status"></a><span data-ttu-id="cd2a9-145">Status</span><span class="sxs-lookup"><span data-stu-id="cd2a9-145">Status</span></span>
<span data-ttu-id="cd2a9-146">Enkelriktade tooverify att hello tillägg har installerats är tooview hello agentens status hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cd2a9-146">One way tooverify that hello extension is installed is tooview hello agent status in hello Azure Portal.</span></span> <span data-ttu-id="cd2a9-147">Välj **alla inställningar** i hello virtuella bladet och klicka sedan på **tillägg**.</span><span class="sxs-lookup"><span data-stu-id="cd2a9-147">Select **All settings** in hello virtual machine blade, and then click on **Extensions**.</span></span> <span data-ttu-id="cd2a9-148">Du bör se hello **SQLIaaSExtension** tillägg i listan.</span><span class="sxs-lookup"><span data-stu-id="cd2a9-148">You should see hello **SQLIaaSExtension** extension listed.</span></span>

![SQL Server IaaS-Agent tillägget i Azure-portalen](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

<span data-ttu-id="cd2a9-150">Du kan också använda hello **Get-AzureVMSqlServerExtension** Azure Powershell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="cd2a9-150">You can also use hello **Get-AzureVMSqlServerExtension** Azure Powershell cmdlet.</span></span>

    Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

<span data-ttu-id="cd2a9-151">hello föregående kommando bekräftar hello-agenten är installerad och ger allmän statusinformation.</span><span class="sxs-lookup"><span data-stu-id="cd2a9-151">hello previous command confirms hello agent is installed and provides general status information.</span></span> <span data-ttu-id="cd2a9-152">Du kan också få statusinformation om automatisk säkerhetskopiering och korrigering med hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="cd2a9-152">You can also get specific status information about Automated Backup and Patching with hello following commands.</span></span>

    $sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a><span data-ttu-id="cd2a9-153">Borttagning</span><span class="sxs-lookup"><span data-stu-id="cd2a9-153">Removal</span></span>
<span data-ttu-id="cd2a9-154">I hello Azure-portalen, du kan avinstallera hello tillägg genom att klicka på hello ellips på hello **tillägg** bladet för de virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="cd2a9-154">In hello Azure Portal, you can uninstall hello extension by clicking hello ellipsis on hello **Extensions** blade of your virtual machine properties.</span></span> <span data-ttu-id="cd2a9-155">Klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="cd2a9-155">Then click **Delete**.</span></span>

![Avinstallera hello SQL Server IaaS Agent tillägget i Azure Portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

<span data-ttu-id="cd2a9-157">Du kan också använda hello **ta bort AzureRmVMSqlServerExtension** Powershell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="cd2a9-157">You can also use hello **Remove-AzureRmVMSqlServerExtension** Powershell cmdlet.</span></span>

    Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## <a name="next-steps"></a><span data-ttu-id="cd2a9-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cd2a9-158">Next Steps</span></span>
<span data-ttu-id="cd2a9-159">Börja använda någon av hello-tjänster som stöds av hello extension.</span><span class="sxs-lookup"><span data-stu-id="cd2a9-159">Begin using one of hello services supported by hello extension.</span></span> <span data-ttu-id="cd2a9-160">Mer information finns i hello information som refereras i hello [-tjänster som stöds](#supported-services) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="cd2a9-160">For more details, see hello topics referenced in hello [Supported services](#supported-services) section of this article.</span></span>

<span data-ttu-id="cd2a9-161">Mer information om hur du kör SQL Server på Azure Virtual Machines finns [SQL Server på Azure virtuella datorer – översikt](virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="cd2a9-161">For more information about running SQL Server on Azure Virtual Machines, see [SQL Server on Azure Virtual Machines overview](virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

