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
# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-virtual-machines-resource-manager"></a>Konfigurera Azure Key Vault-integrering för SQLServer på virtuella Azure-datorer (Resource Manager)
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-ps-sql-keyvault.md)
> * [Klassisk](../classic/ps-sql-keyvault.md)
> 
> 

## <a name="overview"></a>Översikt
Det finns flera funktioner för SQL Server-kryptering, t.ex [transparent datakryptering (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [kryptering på kolumnen (r)](https://msdn.microsoft.com/library/ms173744.aspx), och [säkerhetskopia av krypteringsnyckeln](https://msdn.microsoft.com/library/dn449489.aspx). Dessa typer av kryptering kräver toomanage och lagra hello Kryptografiska nycklar som du använder för kryptering. hello Azure Key Vault (AKV)-tjänsten är utformad tooimprove hello säkerhet och hantering av dessa nycklar i en säker och hög tillgänglighet på. Hej [SQL Server-anslutningen](http://www.microsoft.com/download/details.aspx?id=45344) och aktiverar SQL Server toouse nycklarna från Azure Key Vault.

Om du kör SQL Server med lokala datorer, det är [steg som du kan följa tooaccess Azure Key Vault från din lokala SQL Server-datorn](https://msdn.microsoft.com/library/dn198405.aspx). Men för SQL Server på virtuella Azure-datorer, du kan spara tid genom att använda hello *Azure Key Vault-integrering* funktion.

När den här funktionen aktiveras den automatiskt hello SQL Server-anslutningen installerar, konfigurerar hello EKM-providern tooaccess Azure Key Vault och skapar hello autentiseringsuppgifter tooallow du tooaccess ditt valv. Om du har använt hello stegen i hello tidigare nämnts lokalt dokumentation, ser du att den här funktionen automatiserar steg 2 och 3. hello enda fortfarande behövs toodo manuellt är toocreate hello nyckelvalvet och nycklar. Därifrån är hello hela installationen av SQL-VM automatisk. När installationen har slutförts som den här funktionen, kan du köra T-SQL-instruktioner toobegin kryptera dina databaser eller säkerhetskopieringar som vanligt.

[!INCLUDE [AKV Integration Prepare](../../../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="enabling-and-configuring-akv-integration"></a>Aktivera och konfigurera AKV-integreringen
Du kan aktivera AKV-integreringen under etableringen eller konfigurera den för befintliga virtuella datorer.

### <a name="new-vms"></a>Nya virtuella datorer
Om du etablerar en ny SQL Server-dator med Resource Manager innehåller en steg tooenable Azure Key Vault-integrering hello Azure-portalen. hello Azure Key Vault-funktionen är endast tillgängligt för hello Enterprise, Developer och Evaluation-utgåvor av SQL Server.

![SQL Azure Key Vault-integrering](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

En detaljerad genomgång av etablering finns [etablera en virtuell dator med SQL Server i hello Azure Portal](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Befintliga virtuella datorer
Välj den virtuella datorn för SQL Server för befintliga SQL Server virtuella datorer. Välj hello **SQL Server-konfigurationsfilen** avsnitt i hello **inställningar** bladet.

![SQL AKV-integreringen för befintliga virtuella datorer](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

I hello **SQL Server-konfigurationsfilen** bladet, klickar du på hello **redigera** knapp i hello Automated Key Vault-integrering avsnitt.

![Konfigurera SQL AKV-integreringen för befintliga virtuella datorer](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-configuration.png)

När du är klar klickar du på hello **OK** hello längst ned på hello-knappen **SQL Server-konfigurationsfilen** bladet toosave ändringarna.

> [!NOTE]
> Du kan också konfigurera AKV-integreringen med en mall. Mer information finns i [Azure quickstart-mall för Azure Key Vault-integrering](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).
> 
> 

[!INCLUDE [AKV Integration Next Steps](../../../../includes/virtual-machines-sql-server-akv-next-steps.md)]

