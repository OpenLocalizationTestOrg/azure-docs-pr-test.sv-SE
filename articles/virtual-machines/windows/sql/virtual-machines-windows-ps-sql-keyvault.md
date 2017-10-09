---
title: "aaaIntegrate Nyckelvalv med SQL Server på virtuella Windows-datorer i Azure (Resource Manager) | Microsoft Docs"
description: "Lär dig hur tooautomate hello konfigurationen av SQL Server-kryptering för användning med Azure Key Vault. Det här avsnittet beskrivs hur du skapar toouse Azure Key Vault-integrering med SQL Server-datorer med Resource Manager."
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
ms.openlocfilehash: 0d36d3d075d6538c18cd5ecb43c19a4b000a99e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-virtual-machines-resource-manager"></a><span data-ttu-id="052d5-104">Konfigurera Azure Key Vault-integrering för SQLServer på virtuella Azure-datorer (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="052d5-104">Configure Azure Key Vault Integration for SQL Server on Azure Virtual Machines (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="052d5-105">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="052d5-105">Resource Manager</span></span>](virtual-machines-windows-ps-sql-keyvault.md)
> * [<span data-ttu-id="052d5-106">Klassisk</span><span class="sxs-lookup"><span data-stu-id="052d5-106">Classic</span></span>](../classic/ps-sql-keyvault.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="052d5-107">Översikt</span><span class="sxs-lookup"><span data-stu-id="052d5-107">Overview</span></span>
<span data-ttu-id="052d5-108">Det finns flera funktioner för SQL Server-kryptering, t.ex [transparent datakryptering (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [kryptering på kolumnen (r)](https://msdn.microsoft.com/library/ms173744.aspx), och [säkerhetskopia av krypteringsnyckeln](https://msdn.microsoft.com/library/dn449489.aspx).</span><span class="sxs-lookup"><span data-stu-id="052d5-108">There are multiple SQL Server encryption features, such as [transparent data encryption (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [column level encryption (CLE)](https://msdn.microsoft.com/library/ms173744.aspx), and [backup encryption](https://msdn.microsoft.com/library/dn449489.aspx).</span></span> <span data-ttu-id="052d5-109">Dessa typer av kryptering kräver toomanage och lagra hello Kryptografiska nycklar som du använder för kryptering.</span><span class="sxs-lookup"><span data-stu-id="052d5-109">These forms of encryption require you toomanage and store hello cryptographic keys you use for encryption.</span></span> <span data-ttu-id="052d5-110">hello Azure Key Vault (AKV)-tjänsten är utformad tooimprove hello säkerhet och hantering av dessa nycklar i en säker och hög tillgänglighet på.</span><span class="sxs-lookup"><span data-stu-id="052d5-110">hello Azure Key Vault (AKV) service is designed tooimprove hello security and management of these keys in a secure and highly available location.</span></span> <span data-ttu-id="052d5-111">Hej [SQL Server-anslutningen](http://www.microsoft.com/download/details.aspx?id=45344) och aktiverar SQL Server toouse nycklarna från Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="052d5-111">hello [SQL Server Connector](http://www.microsoft.com/download/details.aspx?id=45344) enables SQL Server toouse these keys from Azure Key Vault.</span></span>

<span data-ttu-id="052d5-112">Om du kör SQL Server med lokala datorer, det är [steg som du kan följa tooaccess Azure Key Vault från din lokala SQL Server-datorn](https://msdn.microsoft.com/library/dn198405.aspx).</span><span class="sxs-lookup"><span data-stu-id="052d5-112">If you running SQL Server with on-premises machines, there are [steps you can follow tooaccess Azure Key Vault from your on-premises SQL Server machine](https://msdn.microsoft.com/library/dn198405.aspx).</span></span> <span data-ttu-id="052d5-113">Men för SQL Server på virtuella Azure-datorer, du kan spara tid genom att använda hello *Azure Key Vault-integrering* funktion.</span><span class="sxs-lookup"><span data-stu-id="052d5-113">But for SQL Server in Azure VMs, you can save time by using hello *Azure Key Vault Integration* feature.</span></span>

<span data-ttu-id="052d5-114">När den här funktionen aktiveras den automatiskt hello SQL Server-anslutningen installerar, konfigurerar hello EKM-providern tooaccess Azure Key Vault och skapar hello autentiseringsuppgifter tooallow du tooaccess ditt valv.</span><span class="sxs-lookup"><span data-stu-id="052d5-114">When this feature is enabled, it automatically installs hello SQL Server Connector, configures hello EKM provider tooaccess Azure Key Vault, and creates hello credential tooallow you tooaccess your vault.</span></span> <span data-ttu-id="052d5-115">Om du har använt hello stegen i hello tidigare nämnts lokalt dokumentation, ser du att den här funktionen automatiserar steg 2 och 3.</span><span class="sxs-lookup"><span data-stu-id="052d5-115">If you looked at hello steps in hello previously mentioned on-premises documentation, you can see that this feature automates steps 2 and 3.</span></span> <span data-ttu-id="052d5-116">hello enda fortfarande behövs toodo manuellt är toocreate hello nyckelvalvet och nycklar.</span><span class="sxs-lookup"><span data-stu-id="052d5-116">hello only thing you would still need toodo manually is toocreate hello key vault and keys.</span></span> <span data-ttu-id="052d5-117">Därifrån är hello hela installationen av SQL-VM automatisk.</span><span class="sxs-lookup"><span data-stu-id="052d5-117">From there, hello entire setup of your SQL VM is automated.</span></span> <span data-ttu-id="052d5-118">När installationen har slutförts som den här funktionen, kan du köra T-SQL-instruktioner toobegin kryptera dina databaser eller säkerhetskopieringar som vanligt.</span><span class="sxs-lookup"><span data-stu-id="052d5-118">Once this feature has completed this setup, you can execute T-SQL statements toobegin encrypting your databases or backups as you normally would.</span></span>

[!INCLUDE [AKV Integration Prepare](../../../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="enabling-and-configuring-akv-integration"></a><span data-ttu-id="052d5-119">Aktivera och konfigurera AKV-integreringen</span><span class="sxs-lookup"><span data-stu-id="052d5-119">Enabling and configuring AKV integration</span></span>
<span data-ttu-id="052d5-120">Du kan aktivera AKV-integreringen under etableringen eller konfigurera den för befintliga virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="052d5-120">You can enable AKV integration during provisioning or configure it for existing VMs.</span></span>

### <a name="new-vms"></a><span data-ttu-id="052d5-121">Nya virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="052d5-121">New VMs</span></span>
<span data-ttu-id="052d5-122">Om du etablerar en ny SQL Server-dator med Resource Manager innehåller en steg tooenable Azure Key Vault-integrering hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="052d5-122">If you are provisioning a new SQL Server virtual machine with Resource Manager, hello Azure portal provides a step tooenable Azure Key Vault integration.</span></span> <span data-ttu-id="052d5-123">hello Azure Key Vault-funktionen är endast tillgängligt för hello Enterprise, Developer och Evaluation-utgåvor av SQL Server.</span><span class="sxs-lookup"><span data-stu-id="052d5-123">hello Azure Key Vault feature is available only for hello Enterprise, Developer and Evaluation Editions of SQL Server.</span></span>

![SQL Azure Key Vault-integrering](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

<span data-ttu-id="052d5-125">En detaljerad genomgång av etablering finns [etablera en virtuell dator med SQL Server i hello Azure Portal](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="052d5-125">For a detailed walkthrough of provisioning, see [Provision a SQL Server virtual machine in hello Azure Portal](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

### <a name="existing-vms"></a><span data-ttu-id="052d5-126">Befintliga virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="052d5-126">Existing VMs</span></span>
<span data-ttu-id="052d5-127">Välj den virtuella datorn för SQL Server för befintliga SQL Server virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="052d5-127">For existing SQL Server virtual machines, select your SQL Server virtual machine.</span></span> <span data-ttu-id="052d5-128">Välj hello **SQL Server-konfigurationsfilen** avsnitt i hello **inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="052d5-128">Then select hello **SQL Server configuration** section of hello **Settings** blade.</span></span>

![SQL AKV-integreringen för befintliga virtuella datorer](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

<span data-ttu-id="052d5-130">I hello **SQL Server-konfigurationsfilen** bladet, klickar du på hello **redigera** knapp i hello Automated Key Vault-integrering avsnitt.</span><span class="sxs-lookup"><span data-stu-id="052d5-130">In hello **SQL Server configuration** blade, click hello **Edit** button in hello Automated Key Vault integration section.</span></span>

![Konfigurera SQL AKV-integreringen för befintliga virtuella datorer](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-configuration.png)

<span data-ttu-id="052d5-132">När du är klar klickar du på hello **OK** hello längst ned på hello-knappen **SQL Server-konfigurationsfilen** bladet toosave ändringarna.</span><span class="sxs-lookup"><span data-stu-id="052d5-132">When finished, click hello **OK** button on hello bottom of hello **SQL Server configuration** blade toosave your changes.</span></span>

> [!NOTE]
> <span data-ttu-id="052d5-133">Du kan också konfigurera AKV-integreringen med en mall.</span><span class="sxs-lookup"><span data-stu-id="052d5-133">You can also configure AKV integration using a template.</span></span> <span data-ttu-id="052d5-134">Mer information finns i [Azure quickstart-mall för Azure Key Vault-integrering](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).</span><span class="sxs-lookup"><span data-stu-id="052d5-134">For more information, see [Azure quickstart template for Azure Key Vault integration](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).</span></span>
> 
> 

[!INCLUDE [AKV Integration Next Steps](../../../../includes/virtual-machines-sql-server-akv-next-steps.md)]

