---
title: "aaaSQL Server på Azure virtuella datorer och svar | Microsoft Docs"
description: "Den här artikeln innehåller svar toofrequently frågor och svar om att köra SQL Server på virtuella Azure-datorer."
services: virtual-machines-windows
documentationcenter: 
author: v-shysun
manager: felixwu
editor: 
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/23/2017
ms.author: v-shysun
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 70a8777bf765dcc69f433aa1fb59eb94929caab7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-sql-server-on-azure-virtual-machines"></a>Vanliga frågor och svar om SQL Server på Azure Virtual Machines
Det här avsnittet innehåller svar toosome av hello vanliga frågor om körs [SQL Server på Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar

1. **Hur skapar jag en virtuell Azure-dator med SQL Server**

    hello enklaste lösningen är toocreate en virtuell dator med SQL Server. En självstudiekurs om registrerar dig för Azure och skapar en SQL-VM från hello portal finns [etablera en virtuell dator med SQL Server i hello Azure Portal](virtual-machines-windows-portal-sql-server-provision.md). Du kan välja en avbildning av virtuell dator som använder per minut SQL Server-licensiering eller du kan använda en avbildning som du kan använda toobring din egen SQL Server-licens. Du har också hello möjlighet att manuellt installera SQL Server på en virtuell dator och återanvända en lokal licens. Om du tar din egen licens måste du ha [Licensmobilitet via Software Assurance på Azure](https://azure.microsoft.com/pricing/license-mobility/). Mer information finns i [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Prisvägledning för virtuella SQL Server Azure-datorer).

1. **Vad är hello skillnaden mellan SQL virtuella datorer och hello SQL Database-tjänsten?**

    Kör begreppsmässigt SQL Server på en virtuell Azure-dator är inte detsamma som kör SQL Server i en fjärransluten datacenter. Däremot [SQL-databas](../../../sql-database/sql-database-technical-overview.md) erbjuder databasen som en tjänst. Med SQL-databas har inte åtkomst toohello datorer som värd för dina databaser. En fullständig jämförelse finns [Välj ett moln SQL Server-alternativ: Azure SQL (PaaS) Database eller SQL Server på Azure Virtual Machines (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **Hur kan jag migrera Mina lokala SQL Server-databasen toohello molnet?**

    Först skapa en virtuell Azure-dator med en SQL Server-instans. Sedan migrera din lokala databaser toothat-instans. Strategier för migrering av data, se [migrera en SQL Server-databasen tooSQL Server i en Azure VM](virtual-machines-windows-migrate-sql.md).

1. **Kan jag installera en andra instans av SQL Server på hello samma virtuella dator? Kan jag ändra installerade funktioner för hello standardinstansen?**

    Ja. hello SQL Server-installationsmedia finns i en mapp på hello **C** enhet. Kör **Setup.exe** från denna plats tooadd nya SQL Server-instanser eller toochange andra installerade funktioner i SQL Server på hello-datorn. Observera att vissa funktioner, till exempel automatisk säkerhetskopiering, automatisk uppdatering och Azure Key Vault-integrering bara köras mot hello standardinstansen.

1. **Kan jag avinstallera hello standardinstansen av SQL Server?**

    Ja. Men det finns vissa saker. Enligt informationen i hello tidigare svar, funktioner som förlitar sig på hello [tillägg för SQL Server IaaS Agent](virtual-machines-windows-sql-server-agent-extension.md) fungerar bara på hello standardinstansen. Om du avinstallerar hello standardinstansen hello tillägget fortsätter toolook för det och kan ge fel i händelseloggen. Dessa fel är från hello följande två källor: **hantering av Microsoft SQL Server-inloggningsuppgifter** och **Microsoft SQL Server IaaS Agent**. Ett fel hello kan vara liknande toohello följande:
    
        A network-related or instance-specific error occurred while establishing a connection tooSQL Server. hello server was not found or was not accessible. 
        
    Om du toouninstall hello standardinstansen kan också avinstallera hello [tillägg för SQL Server IaaS Agent](virtual-machines-windows-sql-server-agent-extension.md) samt.

1. **Hur uppgraderar jag tooa nya version/utgåva av hello SQL Server i en Azure VM?**

    Det finns för närvarande ingen uppgradering på plats för SQL Server som körs i en Azure VM. Skapa en ny Azure virtuell dator med hello önskad SQL Server-version/utgåva och sedan migrera dina databaser toohello ny server med hjälp av standard [tekniker för migrering av data](virtual-machines-windows-migrate-sql.md).

1. **Hur kan jag installera min licensierad version av SQL Server på en Azure VM?**

    Det finns två sätt toodo detta. Du kan etablera en hello [avbildningar av virtuella datorer som har stöd för licenser](virtual-machines-windows-sql-server-iaas-overview.md#BYOL). Ett annat alternativ är toocopy hello SQL Server-installation media tooa virtuella Windows Server-datorn och installera SQL Server på hello VM. Licensiering skäl, måste du ha [Licensmobilitet via Software Assurance på Azure](https://azure.microsoft.com/pricing/license-mobility/). Mer information finns i [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Prisvägledning för virtuella SQL Server Azure-datorer).

1. **Kan jag ändra en VM-toouse min egen SQL Server-licens om den har skapats från en hello betalning per användning galleriavbildningar?**

    Nej. Du kan inte växla från per minut licensiering toousing din egen licens. Skapa en ny virtuell Azure-dator med någon av hello [avbildningar med BYOL](virtual-machines-windows-sql-server-iaas-overview.md#BYOL), och sedan migrera dina databaser toohello ny server med hjälp av standard [tekniker för migrering av data](virtual-machines-windows-migrate-sql.md).

1. **SQL Server Failover Cluster instanser (FCI) stöds på virtuella Azure-datorer?**

   Ja. Du kan [skapa ett redundanskluster i Windows på Windows Server 2016](virtual-machines-windows-portal-sql-create-failover-cluster.md) och använda Storage Spaces Direct (S2D) för hello klusterlagringen. Du kan också använda redundanskluster eller lagring tredjepartslösningar som beskrivs i [hög tillgänglighet och katastrofåterställning återställning för SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md#azure-only-high-availability-solutions).

1. **Måste jag toopay toolicense SQL Server på virtuella Azure-datorn om den används endast för vänteläge/redundans?**

    Du har inte toopay toolicense en SQL Server som ingår som en passiv sekundär replik i en distribution med hög tillgänglighet om du har Software Assurance och använda Licensmobilitet som beskrivs i [virtuella vanliga frågor om licensiering](http://azure.microsoft.com/pricing/licensing-faq/).

1. **Hur uppdateringar och service packs tillämpas på en SQL Server-VM?**

    Virtuella datorer ger dig kontroll över hello värddatorn, inklusive när och hur du tillämpar uppdateringar. Hello operativsystem kan du manuellt koppla windows-uppdateringar eller du kan aktivera en schemaläggningstjänsten kallas [automatisk uppdatering](virtual-machines-windows-sql-automated-patching.md). Automatisk uppdatering installerar alla uppdateringar som markeras som viktiga, inklusive SQL Server-uppdateringar i den kategorin. Andra valfria uppdateringar tooSQL servern måste installeras manuellt.

1. **Det är möjligt tooset konfigurationer som inte visas i hello galleriet för virtuella datorer (till exempel Windows 2008 R2 + SQL Server 2012)?**

    Nej. Galleriavbildningar för virtuell dator med SQL Server, måste du välja en hello tillhandahålls bilder.

1. **Hur installerar verktyg för SQL-Data på min Azure VM?**

     Hämta och installera verktyg för hello SQL-Data från [Microsoft SQL Server Data Tools - Business Intelligence för Visual Studio 2013](https://www.microsoft.com/en-us/download/details.aspx?id=42313).

## <a name="resources"></a>Resurser

En översikt över SQL Server på Azure Virtual Machines Se hello video [Azure VM är hello bästa plattformen för SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016). Du kan också få en bra introduktion i hello avsnittet [SQL Server på Azure virtuella datorer – översikt](virtual-machines-windows-sql-server-iaas-overview.md).

Andra resurser är:

* [Etablera en virtuell dator med SQL Server i hello Azure-portalen](virtual-machines-windows-portal-sql-server-provision.md)
* [Migrera en databas tooSQL Server på en Azure VM](virtual-machines-windows-migrate-sql.md)
* [Hög tillgänglighet och Haveriberedskap för SQLServer på virtuella Azure-datorer](virtual-machines-windows-sql-high-availability-dr.md)
* [Prestandametodtips för SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-performance.md)
* [Programmet mönster och utvecklingsstrategier för SQLServer på virtuella Azure-datorer](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md) 