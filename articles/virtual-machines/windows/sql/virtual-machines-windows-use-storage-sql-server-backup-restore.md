---
title: "aaaHow toouse Azure storage för SQL Server-säkerhetskopiering och återställning | Microsoft Docs"
description: "Lär dig hur tooback in SQL Server tooAzure lagring. Förklarar hello fördelarna med att säkerhetskopiera SQL-databaser tooAzure lagring."
services: virtual-machines-windows
documentationcenter: 
author: MikeRayMSFT
manager: jhubbard
tags: azure-service-management
ms.assetid: 0db7667d-ef63-4e2b-bd4d-574802090f8b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/31/2017
ms.author: mikeray
ms.openlocfilehash: 67ebe8377be97df1312f8c1345e23576aabe0c4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-storage-for-sql-server-backup-and-restore"></a>Använd Azure Storage för SQL Server-säkerhetskopiering och återställning
## <a name="overview"></a>Översikt
Från och med SQL Server 2012 SP1 CU2 kan kan nu du skriva SQL Server säkerhetskopieras direkt toohello Azure Blob storage-tjänst. Du kan använda den här funktionen tooback in tooand återställning från hello Azure Blob-tjänsten med en lokal SQL Server-databas eller en SQL Server-databas i en virtuell Azure-dator. Säkerhetskopiering toocloud erbjuder fördelarna med tillgänglighet, obegränsad georeplikerad extern lagring och enkel migrering av data tooand från hello molnet. Du kan utfärda instruktioner för säkerhetskopiering eller återställning med hjälp av Transact-SQL eller SMO.

SQL Server 2016 innehåller nya funktioner; Du kan använda [fil – ögonblicksbildsäkerhetskopia](http://msdn.microsoft.com/library/mt169363.aspx) tooperform nästan omedelbar säkerhetskopior och otroligt snabb återställning.

Det här avsnittet förklarar varför du kan välja toouse Azure storage för SQL-säkerhetskopiering och beskriver hello komponenter som ingår. Du kan använda hello resurserna hello slutet av hello artikel tooaccess genomgång och ytterligare information toostart med hjälp av den här tjänsten med SQL Server-säkerhetskopieringar.

## <a name="benefits-of-using-hello-azure-blob-service-for-sql-server-backups"></a>Fördelarna med att använda hello Azure Blob-tjänsten för SQL Server-säkerhetskopieringar
Det finns flera utmaningar som kan uppstå när du säkerhetskopierar SQL Server. De här utmaningarna omfattar lagringshantering risken för lagring, komma åt toooff plats lagrings- och maskinvarukonfiguration. Många av dessa problem åtgärdas genom att använda hello Azure Blob store-tjänsten för SQL Server-säkerhetskopiering. Tänk hello följande fördelar:

* **Användarvänlighet**: lagra säkerhetskopiorna i Azure BLOB kan vara ett praktiskt, flexibelt och enkelt tooaccess externt alternativ. Skapa extern lagring för dina säkerhetskopieringar för SQL Server kan vara lika enkelt som att ändra din befintliga skript/jobb toouse hello **säkerhetskopiering tooURL** syntax. Extern lagring bör vara tillräckligt långt från hello produktion databasen plats tooprevent en enda katastrof som kan påverka både hello utanför kontoret och produktion databasplatser. Genom att välja för[geo-replikering i Azure-blobbar](../../../storage/common/storage-redundancy.md), du har ett extra skyddslager i en katastrof som kan påverka hello hela region hello-händelse.
* **Säkerhetskopiering Arkiv**: hello Azure Blob Storage-tjänst som ger ett bättre alternativ toohello som ofta används för band alternativet tooarchive säkerhetskopior. Lagring på band kan kräva fysiska transport tooan utanför anläggningen och åtgärder tooprotect hello media. Lagra säkerhetskopiorna i Azure Blob Storage tillhandahåller ett ögonblick, hög tillgänglighet, och en varaktiga arkivering alternativet.
* **Hanterad maskinvara**: det finns inga omkostnaderna för maskinvaruhantering av med Azure-tjänster. Azure-tjänster hantera hello maskinvara och ange geo-replikering för redundans och skydd mot maskinvarufel.
* **Obegränsad lagring**: genom att aktivera en direkt säkerhetskopiering tooAzure blobbar som du har åtkomst toovirtually obegränsad lagring. Du kan också har säkerhetskopiera tooan Azure virtuella disken begränsningar baserat på storleken på datorn. Det finns en gräns toohello diskar kan du koppla tooan virtuella Azure-datorn för säkerhetskopiering. Den här gränsen är 16 diskar för en extra stor instans och färre för mindre instanser.
* **Säkerhetskopiera tillgänglighet**: säkerhetskopior som lagras i Azure BLOB är tillgängliga från valfri plats och när som helst och enkelt kan nås för återställningar tooeither en lokal SQL Server eller en annan SQL Server som körs i en Azure-dator, utan måste hello för att koppla/frånkoppla databasen eller hämta och bifoga hello VHD.
* **Kostnad**: betalar bara för hello-tjänst som används. Kan vara kostnadseffektiva som ett externt och säkerhetskopiering Arkiv-alternativ. Se hello [Azure prisnivå Kalkylatorn](http://go.microsoft.com/fwlink/?LinkId=277060 "Priskalkylatorn"), och hello [priser för Azure artikel](http://go.microsoft.com/fwlink/?LinkId=277059 "priser artikel") mer information.
* **Lagring ögonblicksbilder**: när databasfilerna i en Azure blob och du använder SQL Server 2016, kan du använda [fil – ögonblicksbildsäkerhetskopia](http://msdn.microsoft.com/library/mt169363.aspx) tooperform nästan omedelbar säkerhetskopior och otroligt snabb återställning.

Mer information finns i [SQL Server-säkerhetskopiering och återställning med Azure Blob Storage-tjänsten](http://go.microsoft.com/fwlink/?LinkId=271617).

följande två avsnitt hello införa hello Azure Blob storage-tjänst, inklusive hello krävs SQL Server-komponenter. Det är viktigt toounderstand hello komponenter och deras interaktion toosuccessfully Använd säkerhetskopiering och återställning från hello Azure Blob storage-tjänst.

## <a name="azure-blob-storage-service-components"></a>Azure Blob Storage tjänstkomponenter
hello används följande Azure komponenter när du säkerhetskopierar toohello Azure Blob storage-tjänst.

| Komponent | Beskrivning |
| --- | --- |
| **Storage-konto** |Hej lagringskonto är hello startpunkten för alla storage-tjänster. tooaccess en Azure Blob Storage-tjänst, först skapa ett Azure Storage-konto. Mer information om Azure Blob storage-tjänst finns [hur toouse hello Azure Blob Storage-tjänst](https://azure.microsoft.com/develop/net/how-to-guides/blob-storage/) |
| **Behållaren** |En behållare grupperar en uppsättning blobbar och kan lagra ett obegränsat antal Blobbar. toowrite en säkerhetskopiering SQL Server-tooan Azure Blob-tjänsten, du måste ha minst hello Rotbehållare skapas. |
| **BLOB** |En fil av valfri typ och storlek. Blobbar är adresserbara via följande URL-format hello: **https://[storage account].blob.core.windows.net/[container]/[blob]**. Läs mer om Sidblobbar [förstå Block och Sidblobbar](http://msdn.microsoft.com/library/azure/ee691964.aspx) |

## <a name="sql-server-components"></a>SQL Server-komponenter
hello används följande SQL Server-komponenter när du säkerhetskopierar toohello Azure Blob storage-tjänst.

| Komponent | Beskrivning |
| --- | --- |
| **URL: EN** |En URL anger en identifierare URI (Uniform Resource) tooa unika säkerhetskopia. hello URL är används tooprovide hello plats och namn för hello SQL Server-säkerhetskopia. hello URL: en måste peka tooan faktiska blob, inte bara en behållare. Om hello blob inte finns, skapas. Om en befintlig blob anges SÄKERHETSKOPIERINGEN misslyckas, såvida inte hello > med formatalternativ har angetts. hello följande är ett exempel på hello-URL som du anger i hello kommandot BACKUP: **http[s]://[storageaccount].blob.core.windows.net/[container]/[FILENAME.bak]**. HTTPS rekommenderas men krävs inte. |
| **Autentiseringsuppgifter** |hello information som är nödvändiga tooconnect och autentisera tooAzure Blob storage-tjänst lagras som en autentiseringsuppgift.  En SQL Server-autentiseringsuppgift måste skapas för SQL Server toowrite säkerhetskopieringar tooan Azure Blob- eller återställningsåtgärden från den. Mer information finns i [autentiseringsuppgifter för SQL Server](https://msdn.microsoft.com/library/ms189522.aspx). |

> [!NOTE]
> Om du väljer toocopy och ladda upp en säkerhetskopia toohello Azure Blob storage-tjänst, måste du använda en sida blob-datatyp som din lagringsalternativ om du planerar toouse den här filen för återställning. ÅTERSTÄLLNING från en block-blob-datatyp misslyckas med felmeddelandet.
> 
> 

## <a name="next-steps"></a>Nästa steg
1. Skapa ett Azure-konto om du inte redan har ett. Om du utvärderar Azure bör hello [kostnadsfri utvärderingsversion](https://azure.microsoft.com/free/).
2. Sedan gå igenom en hello följande självstudier som beskriver hur du skapar ett lagringskonto och utföra en återställning.
   
   * **SQL Server 2014**: [Självstudier: SQL Server 2014-säkerhetskopiering och återställning tooMicrosoft Azure Blob Storage-tjänsten](https://msdn.microsoft.com/library/jj720558\(v=sql.120\).aspx).
   * **SQL Server 2016**: [Självstudier: med SQL Server 2016 databaser hello Microsoft Azure Blob storage-tjänst](https://msdn.microsoft.com/library/dn466438.aspx)
3. Granska ytterligare dokumentation från och med [SQL Server-säkerhetskopiering och återställning med Microsoft Azure Blob Storage-tjänsten](https://msdn.microsoft.com/library/jj919148.aspx).

Om du har några problem, granska hello avsnittet [SQL Server-säkerhetskopiering tooURL bästa praxis och felsöka](https://msdn.microsoft.com/library/jj919149.aspx).

Andra SQL-Server säkerhetskopiera och återställa alternativ, se [säkerhetskopiering och återställning av SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).

