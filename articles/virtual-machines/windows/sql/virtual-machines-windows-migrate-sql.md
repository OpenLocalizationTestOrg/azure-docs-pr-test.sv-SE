---
title: "aaaMigrate en Server på en virtuell dator med SQL Server-databasen tooSQL | Microsoft Docs"
description: "Läs mer om hur toomigrate en lokal användare databasen tooSQL Server i en virtuell Azure-dator."
services: virtual-machines-windows
documentationcenter: 
author: sabotta
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 00fd08c6-98fa-4d62-a3b8-ca20aa5246b1
ms.service: virtual-machines-sql
ms.workload: iaas-sql-server
ms.tgt_pltfrm: vm-windows-sql-server
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: carlasab
ms.openlocfilehash: 9c7aba30304ea40796412d2ddc885f6d4a58be2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-a-sql-server-database-toosql-server-in-an-azure-vm"></a>Migrera en SQL Server-databasen tooSQL Server i en Azure VM

Det finns ett antal metoder toomigrate en lokal SQL Server användaren databasen tooSQL Server i en Azure VM. Den här artikeln kort diskuterar olika metoder och rekommenderar hello bästa metoden för olika scenarier.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="what-are-hello-primary-migration-methods"></a>Vad är hello primära metoder?
hello primära migreringsmetoderna är:

* Utför lokal säkerhetskopiering med komprimering och manuellt kopiera hello säkerhetskopian till hello Azure-dator
* Utföra en säkerhetskopiering tooURL och återställa till hello Azure virtuell dator från hello URL
* Koppla från och sedan kopiera hello data och loggfilen filer tooAzure blob-lagring och sedan koppla tooSQL Server i Azure VM från URL
* Omvandla lokala fysiska datorn tooHyper-V VHD överför tooAzure Blob storage och sedan distribuera som nya virtuella datorn med hjälp av överföra VHD
* Leverera hårddisken med hjälp av Windows Import/Export Service
* Om du har en AlwaysOn-distribution lokalt, Använd hello [guiden för Lägg till Azure-replik](../classic/sql-onprem-availability.md) toocreate en replik i Azure och växling vid fel, pekar användare toohello Azure-databasinstans
* Använda SQL Server [Transaktionsreplikering](https://msdn.microsoft.com/library/ms151176.aspx) tooconfigure hello Azure SQL Server-instans som en prenumerant och inaktivera replikering, pekar användare toohello Azure-databasinstans

> [!TIP]
> Du kan också använda dessa samma tekniker toomove databaser mellan SQL Server-datorer i Azure. Det finns till exempel ingen sättet tooupgrade en virtuell dator bild galleriet SQL Server från en version/utgåva tooanother. I det här fallet bör du skapa en ny SQL Server-VM med hello nya version/utgåva och Använd sedan någon av hello migrering tekniker i den här artikeln toomove dina databaser. 

## <a name="choosing-your-migration-method"></a>Välja din migreringsmetod
För optimala dataöverföringsprestanda migrera hello-databasfiler till hello Azure VM är en komprimerad säkerhetskopia.

toominimize driftstopp under migreringsprocessen för hello-databasen, använda något hello AlwaysOn eller hello Transaktionsreplikering.

Om det inte är möjligt toouse hello ovan metoder manuellt migrera din databas. Med den här metoden vanligtvis börja med en säkerhetskopia av databasen följt av en kopia av hello säkerhetskopiering av databasen till Azure och sedan utför en databasåterställning av. Du kan också kopiera hello databasfilerna sig själva till Azure och sedan koppla dem. Det finns flera metoder som du kan utföra den manuella processen för att migrera en databas till en Azure VM.

> [!NOTE]
> När du uppgraderar tooSQL Server 2014 eller SQL Server 2016 från äldre versioner av SQL Server, bör du överväga om ändringar krävs. Vi rekommenderar att du hanterar alla beroenden på funktioner som inte stöds av hello ny version av SQL Server som en del av migreringen. Mer information om hello stöds utgåvor och scenarier finns [uppgradera tooSQL Server](https://msdn.microsoft.com/library/bb677622.aspx).

hello följande tabell visas de olika hello primära metoder och beskriver när hello användning av varje metod som passar bäst.

| Metod | Databasversionen för källa | Målversionen av databasen | Källan databasen säkerhetskopieringsstorlek begränsning | Anteckningar |
| --- | --- | --- | --- | --- |
| [Utför lokal säkerhetskopiering med komprimering och manuellt kopiera hello säkerhetskopian till hello Azure-dator](#backup-and-restore) |SQLServer 2005 eller senare |SQLServer 2005 eller senare |[Lagringsgränsen för Azure VM](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Detta är en mycket enkel och väl testade teknik för att flytta databaser mellan datorer. |
| [Utföra en säkerhetskopiering tooURL och återställa till hello Azure virtuell dator från hello URL](#backup-to-url-and-restore) |SQL Server 2012 SP1 CU2 eller större |SQL Server 2012 SP1 CU2 eller större |< 12,8 TB för SQL Server 2016, annars < 1 TB | Den här metoden är bara ett annat sätt toomove hello säkerhetskopian toohello VM med hjälp av Azure storage. |
| [Koppla bort och sedan kopiera hello data och loggfilen filer tooAzure blob-lagring och sedan koppla tooSQL Server i Azure-dator från URL](#detach-and-attach-from-url) |SQLServer 2005 eller senare |SQLServer 2014 eller högre |[Lagringsgränsen för Azure VM](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Använd den här metoden när du planerar för[lagrar dessa filer med hjälp av hello Azure Blob storage-tjänst](https://msdn.microsoft.com/library/dn385720.aspx) och bifoga dem tooSQL servern körs i Azure VM, särskilt med mycket stora databaser |
| [Konvertera lokala datorn tooHyper-V virtuella hårddiskar, ladda upp tooAzure Blob storage och distribuera en ny virtuell dator med hjälp av överförda VHD](#convert-to-vm-and-upload-to-url-and-deploy-as-new-vm) |SQLServer 2005 eller senare |SQLServer 2005 eller senare |[Lagringsgränsen för Azure VM](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Använd när [ta din egen SQL Server-licens](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md), när du migrera en databas som ska köras på en äldre version av SQL Server eller migrera system och användardatabaser tillsammans som en del av hello migreringen av databasen som är beroende av andra användardatabaser och/eller systemdatabaser. |
| [Leverera hårddisken med hjälp av Windows Import/Export Service](#ship-hard-drive) |SQLServer 2005 eller senare |SQLServer 2005 eller senare |[Lagringsgränsen för Azure VM](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Använd hello [Windows Import/Export Service](../../../storage/common/storage-import-export-service.md) när manuell copy-metoden är för långsamt, såsom med mycket stora databaser |
| [Använd hello guiden för Lägg till Azure-replikering](../classic/sql-onprem-availability.md) |SQLServer 2012 eller senare |SQLServer 2012 eller senare |[Lagringsgränsen för Azure VM](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Minimerar avbrottstid, Använd när du har en lokal AlwaysOn-distribution |
| [Använda SQL Server-Transaktionsreplikering](https://msdn.microsoft.com/library/ms151176.aspx) |SQLServer 2005 eller senare |SQLServer 2005 eller senare |[Lagringsgränsen för Azure VM](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) |Använd när du behöver toominimize driftstopp och inte har en lokal AlwaysOn-distribution |

## <a name="backup-and-restore"></a>Säkerhetskopiering och återställning
Kopiera hello säkerhetskopiering toohello VM säkerhetskopiera databasen med komprimering, och sedan återställa hello-databasen. Om din säkerhetskopia är större än 1 TB, måste den stripe eftersom hello maximal storlek för en virtuell disk är 1 TB. Använd följande allmänna steg toomigrate en databas med hjälp av den här manuella metoden hello:

1. Utför en fullständig säkerhetskopiering tooan lokalt databasplatsen.
2. Skapa eller ladda upp en virtuell dator med hello version av SQL Server som önskas.
3. Konfigurera anslutning baserat på dina krav. Se [ansluta tooa SQL Server-dator i Azure (Resource Manager)](virtual-machines-windows-sql-connect.md).
4. Kopiera dina säkerhetskopierade filer tooyour VM kommandot remote desktop, Windows Explorer eller hello kopiera från en kommandotolk.

## <a name="backup-toourl-and-restore"></a>TooURL för säkerhetskopiering och återställning
I stället för att säkerhetskopiera tooa lokal fil kan du använda hello [säkerhetskopiering tooURL](https://msdn.microsoft.com/library/dn435916.aspx) och sedan återställa från URL: en toohello VM. Med SQL Server 2016 stripe säkerhetskopior stöds, rekommenderas för prestanda och krävs tooexceed hello storleksgränser per blob. För mycket stora databaser hello användning av hello [Windows Import/Export Service](../../../storage/common/storage-import-export-service.md) rekommenderas.

## <a name="detach-and-attach-from-url"></a>Koppla från och koppla från URL
Koppla från databasen och loggfilerna filerna och överför dem för[Azure Blob storage](https://msdn.microsoft.com/library/dn385720.aspx). Koppla sedan hello databasen från hello URL på Azure VM. Använd det här alternativet om du vill hello fysiska databasen filer tooreside i Blob storage. Detta kan vara användbart för mycket stora databaser. Använd följande allmänna steg toomigrate en databas med hjälp av den här manuella metoden hello:

1. Koppla bort hello databasfiler från hello lokalt databasinstansen.
2. Kopiera hello oberoende databasfiler i Azure blob storage med hjälp av hello [kommandoradsverktyget azcopy](../../../storage/common/storage-use-azcopy.md).
3. Koppla hello-databasfiler från hello Azure URL toohello SQL Server-instansen i hello Azure VM.

## <a name="convert-toovm-and-upload-toourl-and-deploy-as-new-vm"></a>Konvertera tooVM och ladda upp tooURL och distribuera som ny virtuell dator
Använd den här metoden toomigrate alla system- och databaser på en lokal SQL Server-instansen tooAzure virtuell dator. Använd följande allmänna steg toomigrate en hela SQL Server-instans med den här manuella metoden hello:

1. Konvertera fysiska eller virtuella datorer tooHyper-V virtuella hårddiskar med hjälp av [Microsoft Virtual Machine Converter](https://technet.microsoft.com/library/dn874008(v=ws.11).aspx).
2. Överföra VHD-filer tooAzure lagring med hjälp av hello [cmdlet Add-AzureVHD](https://msdn.microsoft.com/library/windowsazure/dn495173.aspx).
3. Distribuera en ny virtuell dator med hjälp av hello överföra VHD.

> [!NOTE]
> toomigrate ett helt program bör du använda [Azure Site Recovery](../../../site-recovery/site-recovery-overview.md)].

## <a name="ship-hard-drive"></a>Leverera hårddisk
Använd hello [Windows Import/Export Service metoden](../../../storage/common/storage-import-export-service.md) tootransfer stora mängder filen data tooAzure Blob storage i situationer där överför hello nätverket är orimligt hög eller inte är möjligt. Du skickar en eller flera hårddiskar som innehåller data tooan Azure datacenter, där dina data kommer att överföra tooyour storage-konto med den här tjänsten.

## <a name="next-steps"></a>Nästa steg
Mer information om hur du kör SQL Server på Azure Virtual Machines finns [SQL Server på Azure virtuella datorer – översikt](virtual-machines-windows-sql-server-iaas-overview.md).

Anvisningar om hur du skapar en Azure SQL Server-dator från en avbildning finns [Tips och trick om kloning Azure SQL virtuella datorer från avbildningar](https://blogs.msdn.microsoft.com/psssql/2016/07/06/tips-tricks-on-cloning-azure-sql-virtual-machines-from-captured-images/) på hello tekniker för CSS SQL Server-bloggen.

