---
title: "aaaAzure SQL-databasen automatiskt, geo-redundant säkerhetskopieringar | Microsoft Docs"
description: "SQL-databasen och skapar en lokal databassäkerhetskopia med några minuters mellanrum automatiskt och använder Azure geo-redundant lagring med läsbehörighet för geo-redundans."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 3ee3d49d-16fa-47cf-a3ab-7b22aa491a8d
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/05/2017
ms.author: carlrab
ms.openlocfilehash: 8aff5356e8142707dd7cd2533a4aa5ea8fec866d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="learn-about-automatic-sql-database-backups"></a>Lär dig mer om automatisk säkerhetskopiering i SQL-databas

SQL-databasen skapar databassäkerhetskopior automatiskt och använder Azure-geo-redundant lagring med läsbehörighet (RA-GRS) tooprovide geo-redundans. Dessa säkerhetskopior skapas automatiskt och utan extra kostnad. Du behöver inte toodo något annat toomake dem inträffa. Säkerhetskopiorna av databasen är en viktig del av alla disaster recovery strategi för affärskontinuitet och eftersom de skyddar dina data från oavsiktliga skador eller tas bort. Du kan konfigurera en princip för långsiktig lagring av säkerhetskopior om du vill tookeep säkerhetskopieringar i din egen lagringsbehållaren. Mer information finns i avsnittet om [långsiktig kvarhållning](sql-database-long-term-retention.md).

## <a name="what-is-a-sql-database-backup"></a>Vad är en säkerhetskopia av SQL-databasen?

SQL-databasen använder SQL Server-teknik toocreate [fullständig](https://msdn.microsoft.com/library/ms186289.aspx), [differentiella](https://msdn.microsoft.com/library/ms175526.aspx), och [transaktionsloggen](https://msdn.microsoft.com/library/ms191429.aspx) säkerhetskopior. hello säkerhetskopieringar av transaktionsloggen inträffa normalt var 5-10: e minut med hello frekvens baserat på hello prestandanivå och mycket Databasaktivitet. Säkerhetskopiering av transaktionsloggar, med fullständig och differentiell säkerhetskopiering kan du toorestore en specifik tidpunkt i toohello för en databas tooa samma server som är värd för hello-databasen. När du återställer en databas hello service figurerna reda på vilka full, differentiella och säkerhetskopieringar av transaktionsloggen måste toobe återställs.


Du kan använda dessa säkerhetskopieringar till:

* Återställa en databas tooa i tidpunkt inom hello Bevarandeperiod. Den här åtgärden skapar en ny databas i hello samma server som hello originaldatabasen.
* Återställa en borttagen databas toohello tid som det har tagits bort eller inom hello Bevarandeperiod. hello bort databasen kan endast återställas i hello samma server där hello ursprungliga databasen har skapats.
* Återställa en databas tooanother geografisk region. Detta ger dig toorecover en geografisk katastrofåterställning när du inte kommer åt din server och databas. En ny databas skapas i en befintlig server var som helst i hälsningsmeddelande. 
* Återställa en databas från en specifik säkerhetskopia som lagras i Azure Recovery Services-valvet. Detta ger dig toorestore en gammal version av hello databasen toosatisfy en begäran om kompatibilitet eller toorun en gammal version av programmet hello. Se [långsiktig kvarhållning](sql-database-long-term-retention.md).
* tooperform en återställning, se [återställa databasen från säkerhetskopior](sql-database-recovery-using-backups.md).

> [!NOTE]
> I Azure storage hello termen *replikering* refererar toocopying filer från en plats tooanother. SQL- *databasreplikering* refererar tookeeping toomultiple sekundära databaser synkroniseras med en primär databas. 
> 

## <a name="how-much-backup-storage-is-included-at-no-cost"></a>Hur mycket lagring för säkerhetskopiering ingår kostnadsfritt?
SQL-databasen innehåller too200% av dina högsta etablerade databaslagring säkerhetskopieringslagring utan extra kostnad. Om du har en Standard DB-instans med en etablerade DB storlek på 250 GB, har du till exempel 500 GB lagring för säkerhetskopiering utan extra kostnad. Om din databas överskrider hello tillhandahålls lagring för säkerhetskopiering, kan du välja tooreduce hello kvarhållningsperiod genom att kontakta Azure-supporten. Ett annat alternativ är toopay för lagring av säkerhetskopior extra som faktureras med hello-geografiskt Redundant lagring med läsbehörighet (RA-GRS) normalpris. 

## <a name="how-often-do-backups-happen"></a>Hur ofta hända säkerhetskopieringar?
Fullständiga databassäkerhetskopieringar inträffa varje vecka, differentiella säkerhetskopieringar sker vanligtvis var några timmar och transaktionsloggen säkerhetskopieringar göras normalt var 5-10: e minut. hello första komplett säkerhetskopiering schemaläggs omedelbart när en databas har skapats. Det slutförs vanligtvis inom 30 minuter, men det kan ta längre tid när hello-databasen är av betydande storlek. Hello första säkerhetskopian kan till exempel ta längre tid på en återställd databas eller en databaskopia. Efter hello första fullständig säkerhetskopiering, alla ytterligare säkerhetskopieringar är schemalagda automatiskt och hanteras tyst i bakgrunden hello. hello exakt timing alla för säkerhetskopiering av databaser bestäms av hello SQL Database-tjänsten som det. balanserar hello övergripande belastningen på systemet. 

Hej säkerhetskopieringslagring geo-replikering inträffar baserat på hello Azure Storage replikeringsschema.

## <a name="how-long-do-you-keep-my-backups"></a>Hur länge håller du Mina säkerhetskopior?
Varje säkerhetskopiering i SQL-databas har en kvarhållningsperiod som baseras på hello [tjänstnivå](sql-database-service-tiers.md) av hello-databasen. hello kvarhållningsperiod för en databas i den:


* Grundläggande tjänstnivå är 7 dagar.
* Standard-tjänstnivå är 35 dagar.
* Premiumnivån är 35 dagar.

Om du nedgradera databasen från hello Standard eller Premium-tjänsten nivåer tooBasic sparas hello säkerhetskopieringar i sju dagar. Alla befintliga säkerhetskopior som är äldre än sju dagar är inte längre tillgängliga. 

Om du uppgraderar databasen från hello grundläggande service tier tooStandard eller Premium behåller SQL Database befintliga säkerhetskopior tills de 35 dagar gammal. Den bevarar nya säkerhetskopior allteftersom de sker för 35 dagar.

Om du tar bort en databas, SQL Database behåller hello säkerhetskopieringar i hello samma sätt som för en databas online. Anta att du tar bort en grundläggande databas som har en kvarhållningsperiod på sju dagar. En säkerhetskopia som är fyra dagar gamla sparas för tre dagar.

> [!IMPORTANT]
> Om du tar bort hello Azure SQL-server som är värd för SQL-databaser, tas även bort alla databaser som tillhör toohello server och kan inte återställas. Du kan inte återställa en server som har tagits bort.
> 

## <a name="how-tooextend-hello-backup-retention-period"></a>Hur säkerhetskopierar tooextend hello kvarhållningsperiod?
Om ditt program kräver att hello säkerhetskopior är tillgängliga under längre tid kan du kan utöka hello inbyggda kvarhållningsperiod genom att konfigurera hello långsiktig säkerhetskopiering bevarandeprincip för enskilda databaser (LTR princip). Detta ger dig tooextend hello inbyggda it kvarhållningsperiod från 35 dagar tooup too10 år. Mer information finns i avsnittet om [långsiktig kvarhållning](sql-database-long-term-retention.md).

När du har lagt till hello LTR princip tooa databasen med hjälp av Azure-portalen eller API hello veckovisa fullständiga databassäkerhetskopieringar blir automatiskt kopierade tooyour äger tjänsten Azure Backup-valvet. Om databasen är krypterad med krypteras automatiskt TDE hello säkerhetskopieringar i vila.  säkerhetskopiorna har upphört att gälla, baserat på deras tidsstämpel och hello LTR princip tas bort automatiskt i hello Services-valvet.  Så att du inte behöver toomanage hello Säkerhetskopieringsschemat eller oroa hello rensning av hello gamla filer. hello återställning API stöder säkerhetskopior som lagras i hello valvet så länge hello valvet är i hello samma prenumeration som din SQL-databas. Du kan använda hello Azure-portalen eller PowerShell tooaccess dessa säkerhetskopior.

> [!TIP]
> En hur tooguide finns [konfigurera och återställning från Azure SQL Database långsiktig lagring av säkerhetskopior.](sql-database-long-term-backup-retention-configure.md)
>

## <a name="are-backups-encrypted"></a>Krypteras säkerhetskopieringar

När TDE är aktiverat för en Azure SQL database, är säkerhetskopior också krypterade. Alla nya Azure SQL-databaser har konfigurerats med TDE aktiverad som standard. Mer information om TDE finns [Transparent datakryptering med Azure SQL Database](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database).

## <a name="next-steps"></a>Nästa steg

- Säkerhetskopiorna av databasen är en viktig del av alla disaster recovery strategi för affärskontinuitet och eftersom de skyddar dina data från oavsiktliga skador eller tas bort. toolearn om Hej andra kontinuitet företagslösningar Azure SQL Database, se [översikt över verksamhetskontinuitet](sql-database-business-continuity.md).
- toorestore tooa tidpunkt med hello Azure-portalen finns [återställningspunkt på databas-tooa tidpunkt med hjälp av hello Azure-portalen](sql-database-recovery-using-backups.md).
- toorestore tooa tidpunkt med hjälp av PowerShell, se [återställningspunkt på databas-tooa tidpunkt med hjälp av PowerShell](scripts/sql-database-restore-database-powershell.md).
- tooconfigure, hantera och återställa från långsiktig kvarhållning av automatiska säkerhetskopieringar i ett Azure Recovery Services-valv med hello Azure portal, se [hantera långsiktig lagring av säkerhetskopior med hello Azure-portalen](sql-database-long-term-backup-retention-configure.md).
- tooconfigure, hantera, och Återställ från långsiktig kvarhållning av automatiska säkerhetskopieringar i ett Azure Recovery Services-valv med hjälp av PowerShell, se [hantera långsiktig lagring av säkerhetskopior med hjälp av PowerShell](sql-database-long-term-backup-retention-configure.md).
