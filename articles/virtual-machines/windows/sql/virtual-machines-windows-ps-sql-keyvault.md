---
title: "Integrera Nyckelvalv med SQLServer på virtuella Windows-datorer i Azure (Resource Manager) | Microsoft Docs"
description: "Lär dig hur du kan automatisera konfigurationen av SQL Server-kryptering för användning med Azure Key Vault. Det här avsnittet beskriver hur du använder Azure Key Vault-integrering med SQL Server virtuella datorer som skapats med Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: cd66dfb1-0e9b-4fb0-a471-9deaf4ab4ab8
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/23/2017
ms.author: jroth
ms.openlocfilehash: 32b9564fa5c9ca6864ade343fda309b2c3edf123
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-virtual-machines-resource-manager"></a><span data-ttu-id="3697a-104">Konfigurera Azure Key Vault-integrering för SQLServer på virtuella Azure-datorer (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="3697a-104">Configure Azure Key Vault Integration for SQL Server on Azure Virtual Machines (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3697a-105">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="3697a-105">Resource Manager</span></span>](virtual-machines-windows-ps-sql-keyvault.md)
> * [<span data-ttu-id="3697a-106">Klassisk</span><span class="sxs-lookup"><span data-stu-id="3697a-106">Classic</span></span>](../classic/ps-sql-keyvault.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="3697a-107">Översikt</span><span class="sxs-lookup"><span data-stu-id="3697a-107">Overview</span></span>
<span data-ttu-id="3697a-108">Det finns flera funktioner för SQL Server-kryptering, t.ex [transparent datakryptering (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [kryptering på kolumnen (r)](https://msdn.microsoft.com/library/ms173744.aspx), och [säkerhetskopia av krypteringsnyckeln](https://msdn.microsoft.com/library/dn449489.aspx).</span><span class="sxs-lookup"><span data-stu-id="3697a-108">There are multiple SQL Server encryption features, such as [transparent data encryption (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [column level encryption (CLE)](https://msdn.microsoft.com/library/ms173744.aspx), and [backup encryption](https://msdn.microsoft.com/library/dn449489.aspx).</span></span> <span data-ttu-id="3697a-109">Dessa typer av kryptering måste du hantera och lagra de kryptografiska nycklar som du använder för kryptering.</span><span class="sxs-lookup"><span data-stu-id="3697a-109">These forms of encryption require you to manage and store the cryptographic keys you use for encryption.</span></span> <span data-ttu-id="3697a-110">Tjänsten Azure Key Vault (AKV) är utformade för att förbättra säkerhet och hantering av dessa nycklar i en säker och hög tillgänglighet på.</span><span class="sxs-lookup"><span data-stu-id="3697a-110">The Azure Key Vault (AKV) service is designed to improve the security and management of these keys in a secure and highly available location.</span></span> <span data-ttu-id="3697a-111">Den [SQL Server-anslutningen](http://www.microsoft.com/download/details.aspx?id=45344) aktiverar SQL Server att använda de här nycklarna från Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="3697a-111">The [SQL Server Connector](http://www.microsoft.com/download/details.aspx?id=45344) enables SQL Server to use these keys from Azure Key Vault.</span></span>

<span data-ttu-id="3697a-112">Om du kör SQL Server med lokala datorer, det är [steg som du kan följa för att komma åt Azure Key Vault från din lokala SQL Server-datorn](https://msdn.microsoft.com/library/dn198405.aspx).</span><span class="sxs-lookup"><span data-stu-id="3697a-112">If you running SQL Server with on-premises machines, there are [steps you can follow to access Azure Key Vault from your on-premises SQL Server machine](https://msdn.microsoft.com/library/dn198405.aspx).</span></span> <span data-ttu-id="3697a-113">Men för SQL Server på virtuella Azure-datorer, du kan spara tid genom att använda den *Azure Key Vault-integrering* funktion.</span><span class="sxs-lookup"><span data-stu-id="3697a-113">But for SQL Server in Azure VMs, you can save time by using the *Azure Key Vault Integration* feature.</span></span>

<span data-ttu-id="3697a-114">När den här funktionen aktiveras den automatiskt installerar SQL Server-anslutningen, konfigurerar EKM-providern för att komma åt Azure Key Vault och autentiseringsuppgifter så att du kan komma åt ditt valv.</span><span class="sxs-lookup"><span data-stu-id="3697a-114">When this feature is enabled, it automatically installs the SQL Server Connector, configures the EKM provider to access Azure Key Vault, and creates the credential to allow you to access your vault.</span></span> <span data-ttu-id="3697a-115">Om du tittar på stegen i ovanstående lokalt dokumentation, ser du att den här funktionen automatiserar steg 2 och 3.</span><span class="sxs-lookup"><span data-stu-id="3697a-115">If you looked at the steps in the previously mentioned on-premises documentation, you can see that this feature automates steps 2 and 3.</span></span> <span data-ttu-id="3697a-116">Det enda du behöver fortfarande göra manuellt är att skapa nyckelvalvet och nycklar.</span><span class="sxs-lookup"><span data-stu-id="3697a-116">The only thing you would still need to do manually is to create the key vault and keys.</span></span> <span data-ttu-id="3697a-117">Därifrån är hela installationen av SQL-VM automatisk.</span><span class="sxs-lookup"><span data-stu-id="3697a-117">From there, the entire setup of your SQL VM is automated.</span></span> <span data-ttu-id="3697a-118">När installationen har slutförts som den här funktionen, kan du köra T-SQL-uttryck för att börja kryptera dina databaser eller säkerhetskopieringar som vanligt.</span><span class="sxs-lookup"><span data-stu-id="3697a-118">Once this feature has completed this setup, you can execute T-SQL statements to begin encrypting your databases or backups as you normally would.</span></span>

[!INCLUDE [AKV Integration Prepare](../../../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="enabling-and-configuring-akv-integration"></a><span data-ttu-id="3697a-119">Aktivera och konfigurera AKV-integreringen</span><span class="sxs-lookup"><span data-stu-id="3697a-119">Enabling and configuring AKV integration</span></span>
<span data-ttu-id="3697a-120">Du kan aktivera AKV-integreringen under etableringen eller konfigurera den för befintliga virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3697a-120">You can enable AKV integration during provisioning or configure it for existing VMs.</span></span>

### <a name="new-vms"></a><span data-ttu-id="3697a-121">Nya virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="3697a-121">New VMs</span></span>
<span data-ttu-id="3697a-122">Om du etablerar en ny SQL Server-dator med Resource Manager innehåller ett steg för att aktivera Azure Key Vault-integrering i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3697a-122">If you are provisioning a new SQL Server virtual machine with Resource Manager, the Azure portal provides a step to enable Azure Key Vault integration.</span></span> <span data-ttu-id="3697a-123">Azure Key Vault-funktionen är tillgänglig endast för Enterprise, Developer och Evaluation-utgåvor av SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3697a-123">The Azure Key Vault feature is available only for the Enterprise, Developer and Evaluation Editions of SQL Server.</span></span>

![SQL Azure Key Vault-integrering](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

<span data-ttu-id="3697a-125">En detaljerad genomgång av etablering finns [etablera en virtuell dator med SQL Server i Azure Portal](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="3697a-125">For a detailed walkthrough of provisioning, see [Provision a SQL Server virtual machine in the Azure Portal](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

### <a name="existing-vms"></a><span data-ttu-id="3697a-126">Befintliga virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="3697a-126">Existing VMs</span></span>
<span data-ttu-id="3697a-127">Välj den virtuella datorn för SQL Server för befintliga SQL Server virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3697a-127">For existing SQL Server virtual machines, select your SQL Server virtual machine.</span></span> <span data-ttu-id="3697a-128">Välj sedan den **SQL Server-konfigurationsfilen** avsnitt i den **inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="3697a-128">Then select the **SQL Server configuration** section of the **Settings** blade.</span></span>

![SQL AKV-integreringen för befintliga virtuella datorer](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

<span data-ttu-id="3697a-130">I den **SQL Server-konfigurationsfilen** bladet klickar du på den **redigera** i avsnittet automatisk Key Vault-integrering.</span><span class="sxs-lookup"><span data-stu-id="3697a-130">In the **SQL Server configuration** blade, click the **Edit** button in the Automated Key Vault integration section.</span></span>

![Konfigurera SQL AKV-integreringen för befintliga virtuella datorer](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-configuration.png)

<span data-ttu-id="3697a-132">När du är klar klickar du på den **OK** knappen längst ned på den **SQL Server-konfigurationsfilen** bladet för att spara dina ändringar.</span><span class="sxs-lookup"><span data-stu-id="3697a-132">When finished, click the **OK** button on the bottom of the **SQL Server configuration** blade to save your changes.</span></span>

> [!NOTE]
> <span data-ttu-id="3697a-133">Du kan också konfigurera AKV-integreringen med en mall.</span><span class="sxs-lookup"><span data-stu-id="3697a-133">You can also configure AKV integration using a template.</span></span> <span data-ttu-id="3697a-134">Mer information finns i [Azure quickstart-mall för Azure Key Vault-integrering](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).</span><span class="sxs-lookup"><span data-stu-id="3697a-134">For more information, see [Azure quickstart template for Azure Key Vault integration](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).</span></span>
> 
> 

[!INCLUDE [AKV Integration Next Steps](../../../../includes/virtual-machines-sql-server-akv-next-steps.md)]

