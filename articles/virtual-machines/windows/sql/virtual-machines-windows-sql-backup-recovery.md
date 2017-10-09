---
title: "aaaBackup och återställning av SQL Server | Microsoft Docs"
description: "Beskriver överväganden för säkerhetskopiering och återställning för SQL Server-databaser som körs på Azure Virtual Machines."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-resource-management
ms.assetid: 95a89072-0edf-49b5-88ed-584891c0e066
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/15/2016
ms.author: mikeray
ms.openlocfilehash: f85248fecdd5867d91d09650a1a34ad7c7caa920
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="backup-and-restore-for-sql-server-in-azure-virtual-machines"></a>Säkerhetskopiering och återställning för SQL Server i Azure Virtual Machines
## <a name="overview"></a>Översikt
Azure-lagring underhålls 3 kopior av varje Azure VM disk tooguarantee skydd mot dataförlust eller skadade data fysisk data. Därmed, till skillnad från lokalt behöver du inte tooworry om de här. Men du bör ändå säkerhetskopiera din SQL Server-databaser tooprotect mot fel i programmet eller användare (t.ex infoga felaktiga data eller ta bort en tabell) och som kan toorestore tooa tidpunkt.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

Du kan använda interna säkerhetskopieringen och återställa tekniker med anslutna diskar för hello målet hello säkerhetskopior för SQL Server körs i virtuella Azure-datorer. Det finns dock en begränsa toohello antalet diskar som du kan koppla tooan Azure-dator, baserat på hello [storlek hello virtuella](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Det finns också hello arbetet med disk management tooconsider.

Från och med SQL Server 2014 kan du säkerhetskopiera och återställa tooMicrosoft Azure Blob storage. SQL Server 2016 innehåller också förbättringar för det här alternativet. Dessutom för databasfiler som lagras i Microsoft Azure Blob storage, innehåller SQL Server 2016 ett alternativ för nästan omedelbar säkerhetskopieringar och snabb återställning med hjälp av Azure ögonblicksbilder. Den här artikeln innehåller en översikt över de här alternativen och ytterligare information finns på [SQL Server-säkerhetskopiering och återställning med Microsoft Azure Blob Storage-tjänsten](https://msdn.microsoft.com/library/jj919148.aspx).

> [!NOTE]
> En beskrivning av hello alternativ för hur du säkerhetskopierar mycket stora databaser finns [flera Terabyte SQL Server-databasen säkerhetskopiering strategier för Azure Virtual Machines](http://blogs.msdn.com/b/igorpag/archive/2015/07/28/multi-terabyte-sql-server-database-backup-strategies-for-azure-virtual-machines.aspx).
> 
> 

hello avsnitten nedan innehåller information specifik toohello olika versioner av SQL Server som stöds i en virtuell Azure-dator.

## <a name="sql-server-virtual-machines"></a>SQL Server-datorer
När SQL Server-instansen körs på en virtuell dator i Azure, finns dina databasfiler redan på datadiskar med i Azure. Diskarna live i Azure Blob storage. Så orsaker hello säkerhetskopiera din databas och hello metoder du vidta ändra något. Tänk hello följande. 

* Du behöver inte längre tooperform databasen säkerhetskopieringar tooprovide skydd mot maskinvara eller fel eftersom Microsoft Azure tillhandahåller detta skydd som en del av hello Microsoft Azure-tjänsten.
* Du måste fortfarande tooperform databasen säkerhetskopieringar tooprovide skydd mot fel eller för arkivering, regulatoriska orsaker och administrativa syften.
* Du kan lagra säkerhetskopian hello direkt i Azure. Mer information finns i följande avsnitt som vägledning för hello olika versioner av SQL Server hello.

## <a name="sql-server-2016"></a>SQL Server 2016
Har stöd för Microsoft SQL Server 2016 [säkerhetskopiering och återställning med Azure BLOB](https://msdn.microsoft.com/library/jj919148.aspx) funktioner finns i SQL Server 2014. Men även hello följande förbättringar:

| Förbättring av 2016 | Information |
| --- | --- |
| **Striping** |När du säkerhetskopierar tooMicrosoft Azure blob storage stöder SQL Server 2016 säkerhetskopiera toomultiple blobbar tooenable säkerhetskopiera stora databaser upp tooa maximalt 12,8 TB. |
| **Ögonblicksbild av säkerhetskopian** |Via hello Azure ögonblicksbilder ger SQL Server-fil – Ögonblicksbildsäkerhetskopia nästan omedelbar säkerhetskopior och snabb återställning för databasfiler med hjälp av hello Azure Blob storage-tjänst. Den här funktionen kan du toosimplify principer för säkerhetskopiering och återställning. Filen ögonblicksbilder Säkerhetskopiering stöder också punkt tidpunkt för återställning. Mer information finns i [säkerhetskopior av ögonblicksbilder för databasfilerna i Azure](https://msdn.microsoft.com/library/mt169363%28v=sql.130%29.aspx). |
| **Hanterade schemaläggning av säkerhetskopiering** |SQL Server-hanterad säkerhetskopiering tooAzure stöder nu anpassade scheman. Mer information finns i [SQL Server-hanterad säkerhetskopiering tooMicrosoft Azure](https://msdn.microsoft.com/library/dn449496.aspx). |

En genomgång av hello funktionerna i SQL Server 2016 när du använder Azure Blob storage finns [Självstudier: hello Microsoft Azure Blob storage-tjänst med SQL Server 2016 databaser](https://msdn.microsoft.com/library/dn466438.aspx).

## <a name="sql-server-2014"></a>SQL Server 2014
SQL Server 2014 innehåller hello följande förbättringar:

1. **Säkerhetskopiering och återställning tooAzure**:
   
   * *SQL Server-säkerhetskopiering tooURL* har nu stöd i SQL Server Management Studio. hello alternativet tooback in tooAzure är nu tillgänglig när du använder Säkerhetskopiering eller återställning av aktivitet eller guiden för underhåll plan i SQL Server Management Studio. Mer information finns i [SQL Server-säkerhetskopiering tooURL](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
   * *SQL Server hanteras Backup tooAzure* har nya funktioner som möjliggör hantering av automatisk säkerhetskopiering. Detta är särskilt användbart för att automatisera hanteringen av säkerhetskopiering för SQL Server 2014-instanser som körs på en Azure-dator. Mer information finns i [SQL Server-hanterad säkerhetskopiering tooMicrosoft Azure](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx).
   * *Automatisk säkerhetskopiering* ger ytterligare automation tooautomatically aktivera *SQL Server-hanterad säkerhetskopiering tooAzure* på alla befintliga och nya databaser i SQL Server-VM i Azure. Mer information finns i [Automatisk säkerhetskopiering av SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).
   * En översikt över alla hello alternativ för SQL Server 2014 Backup tooAzure finns [SQL Server-säkerhetskopiering och återställning med Microsoft Azure Blob Storage-tjänsten](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
2. **Kryptering**: SQL Server 2014 stöder kryptering av data när du skapar en säkerhetskopia. Den stöder flera krypteringsalgoritmer och hello använder osf ett certifikat eller en asymmetrisk nyckel. Mer information finns i [kryptering vid säkerhetskopiering](https://msdn.microsoft.com/library/dn449489%28v=sql.120%29.aspx).

## <a name="sql-server-2012"></a>SQLServer 2012
Detaljerad information om SQL Server-säkerhetskopiering och återställning i SQL Server 2012 finns [säkerhetskopiering och återställning av SQL Server-databaser (SQL Server 2012)](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx).

Från och med SQL Server 2012 SP1 kumulativ uppdatering 2, kan du säkerhetskopiera tooand återställning från hello Azure Blob Storage-tjänst. Den här förbättringen kan vara används tooback in SQL Server-databaser på en SQL-Server som körs på en virtuell Azure-dator eller en lokal instans. Mer information finns i [SQL Server-säkerhetskopiering och återställning med Azure Blob Storage-tjänsten](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Hello fördelarna med att använda hello Azure Blob storage-tjänst bland hello möjlighet toobypass hello 16 disk gränsen för anslutna diskar enkel hantering, hello direkt tillgängligheten för hello säkerhetskopian tooanother instans av SQL Server-instans som körs på en Azure virtuell dator eller en lokal instans för migrering eller katastrof återställning. En fullständig lista över fördelar toousing en Azure blob storage-tjänst för SQL Server-säkerhetskopiering finns hello *fördelar* i avsnittet [SQL Server-säkerhetskopiering och återställning med Azure Blob Storage-tjänsten](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Rekommendationer om bästa praxis och felsökningsinformation finns i [säkerhetskopiering och återställning Metodtips (Azure Blob Storage Service)](https://msdn.microsoft.com/library/jj919149%28v=sql.110%29.aspx).

## <a name="sql-server-2008"></a>SQLServer 2008
SQL Server-säkerhetskopiering och återställning i SQL Server 2008 R2 finns [säkerhetskopiera och återställa databaser i SQL Server (SQL Server 2008 R2)](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx).

SQL Server-säkerhetskopiering och återställning i SQL Server 2008 finns [säkerhetskopiera och återställa databaser i SQL Server (SQL Server 2008)](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx).

## <a name="next-steps"></a>Nästa steg
Om du planerar distributionen av SQL Server i en Azure VM, hittar du vägledning för etablering i hello följa kursen: [etablering av en SQL Server-dator på Azure med Azure Resource Manager](virtual-machines-windows-portal-sql-server-provision.md).

Även om säkerhetskopiering och återställning kan vara används toomigrate dina data, det finns potentiellt enklare data migrering sökvägar tooSQL Server på en virtuell dator i Azure. En fullständig beskrivning av migreringsalternativ och rekommendationer finns [migrera en databas tooSQL Server på en Azure VM](virtual-machines-windows-migrate-sql.md).

Granska andra [resurser för att köra SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).

