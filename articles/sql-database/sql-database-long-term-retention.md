---
title: "aaaStore Azure SQL Database säkerhetskopieringar för too10 år | Microsoft Docs"
description: "Lär dig hur Azure SQL Database stöder lagra säkerhetskopior för too10 år."
keywords: 
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: 
ms.assetid: 66fdb8b8-5903-4d3a-802e-af08d204566e
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/22/2016
ms.author: sashan
ms.openlocfilehash: 5825ebd4e3bd66b59b13aea603d377ef814a1df3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="store-azure-sql-database-backups-for-up-too10-years"></a>Säkerhetskopieringar Azure SQL Database för too10 år
Många program har regelverk, kompatibilitet eller andra företag syfte som kräver att du tooretain databassäkerhetskopieringar utöver hello 7-35 dagar som tillhandahålls av Azure SQL Database [automatiska säkerhetskopieringar](sql-database-automated-backups.md). Med funktionen hello långsiktig lagring av säkerhetskopior kan lagra du säkerhetskopiorna SQL-databasen i ett Azure Recovery Services-valv för too10 år. Du kan lagra upp too1 000 databaser per valvet. Du kan sedan välja någon säkerhetskopia i hello valvet toorestore den som en ny databas.

> [!IMPORTANT]
> Långsiktig lagring av säkerhetskopior är för närvarande under förhandsgranskning och finns tillgänglig i hello följande områden: Östra Australien, sydost, södra, centrala USA, östra Asien, östra USA, östra USA 2, centrala Indien, södra Indien, östra, västra Japan, norra centrala USA, Nord Europa, södra centrala USA, Sydostasien, västra Europa och västra USA.
>

> [!NOTE]
> Du kan aktivera in too200 databaser per valvet under en 24-timmarsperiod. Vi rekommenderar att du använder ett separat valv för varje server toominimize hello effekten av den här gränsen. 
> 

## <a name="how-sql-database-long-term-backup-retention-works"></a>Så här fungerar SQL Database långsiktig lagring av säkerhetskopior.

Du kan associera en SQL-databasserver med långsiktig säkerhetskopiering kvarhållning med Azure Recovery Services-valvet. 

* Måste du skapa hello valvet i hello samma Azure-prenumeration som skapade hello SQLServer och hello i samma geografiska region och resurs-grupp. 
* Sedan kan du konfigurera en bevarandeprincip för alla databaser. hello princip orsaker hello veckovisa fullständiga databasen säkerhetskopieringar toobe kopieras toohello Recovery Services-valvet och bevaras under hello angivna kvarhållningsperiod (upp too10 år). 
* Du kan sedan återställa hello databasen från någon av dessa säkerhetskopieringar tooa ny databas i någon server i hello prenumeration. Azure storage skapar en kopia av befintliga säkerhetskopior och hello kopiera påverkar inte prestanda hello befintlig databas.

> [!TIP]
> En hur tooguide finns [konfigurera och återställning från Azure SQL Database långsiktig lagring av säkerhetskopior](sql-database-long-term-backup-retention-configure.md).

## <a name="enable-long-term-backup-retention"></a>Aktivera långsiktig lagring av säkerhetskopior.

tooconfigure långsiktig lagring av säkerhetskopior för en databas:

1. Skapa ett Azure Recovery Services-valv i hello samma region, prenumeration och resurs grupp som SQL-databasservern. 
2. Registrera hello server toohello valvet.
3. Skapa en princip för Azure Recovery Services-skydd.
4. Tillämpa hello skydd princip toohello databaser som kräver långsiktig säkerhetskopiering kvarhållning.

tooconfigure, hantera, och återställa en databas från långsiktig säkerhetskopiering kvarhållning av automatiska säkerhetskopieringar i ett Azure Recovery Services-valv, gör du något av följande hello:

* Med hjälp av hello Azure-portalen: Klicka på **långsiktig lagring av säkerhetskopior**, väljer du en databas och klicka sedan på **konfigurera**. 

   ![Välj en databas för långsiktig lagring av säkerhetskopior.](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

* Med hjälp av PowerShell: Gå för[konfigurera och återställning från Azure SQL Database långsiktig lagring av säkerhetskopior](sql-database-long-term-backup-retention-configure.md).

## <a name="restore-a-database-thats-stored-with-hello-long-term-backup-retention-feature"></a>Återställa en databas som lagras med hello funktionen för långsiktig lagring av säkerhetskopior.

toorecover från en säkerhetskopia för långsiktig lagring av säkerhetskopior.:

1. Lista hello valvet där hello säkerhetskopian lagras.
2. Lista hello behållare som är mappade tooyour logiska server.
3. Lista hello datakälla inom hello valvet som är mappade tooyour databas.
4. Lista hello Återställningspunkter som är tillgängliga toorestore.
5. Återställa hello databas från återställning hello punkt toohello målservern i din prenumeration.

tooconfigure, hantera, och återställa en databas från långsiktig säkerhetskopiering kvarhållning av automatiska säkerhetskopieringar i ett Azure Recovery Services-valv, gör du något av följande hello:

* Med hjälp av hello Azure-portalen: gå för[hantera långsiktig lagring av säkerhetskopior med hello Azure-portalen](sql-database-long-term-backup-retention-configure.md). 

* Med hjälp av PowerShell: Gå för[hantera långsiktig lagring av säkerhetskopior med hjälp av PowerShell](sql-database-long-term-backup-retention-configure.md).

## <a name="get-pricing-for-long-term-backup-retention"></a>Hämta priser för långsiktig lagring av säkerhetskopior.

Långsiktig säkerhetskopiering kvarhållning av en SQL-databas debiteras enligt toohello [Azure-säkerhetskopiering tjänster priser priser](https://azure.microsoft.com/pricing/details/backup/).

När hello SQL database-server är registrerad toohello valvet, debiteras du för hello totalt lagringsutrymme som används av hello veckovisa säkerhetskopior lagras i hello-valvet.

## <a name="view-available-backups-that-are-stored-in-long-term-backup-retention"></a>Visa tillgängliga säkerhetskopior som lagras i långsiktig lagring av säkerhetskopior.

tooconfigure, hantera, och återställa en databas från långsiktig säkerhetskopiering kvarhållning av automatisk säkerhetskopiering i Azure Recovery Services-valvet via hello Azure-portalen, gör du något av följande hello:

* Med hjälp av hello Azure-portalen: gå för[hantera långsiktig lagring av säkerhetskopior med hello Azure-portalen](sql-database-long-term-backup-retention-configure.md). 

* Med hjälp av PowerShell: Gå för[hantera långsiktig lagring av säkerhetskopior med hjälp av PowerShell](sql-database-long-term-backup-retention-configure.md).

## <a name="disable-long-term-retention"></a>Inaktivera långsiktig kvarhållning

hello återställningstjänsten hanterar automatiskt hello rensning av säkerhetskopieringar utifrån hello tillhandahålls bevarandeprincip. 

toostop skicka hello säkerhetskopieringar för en viss databas toohello valvet, ta bort hello bevarandeprincip för den här databasen.
  
```
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName 'RG1' -ServerName 'Server1' -DatabaseName 'DB1' -State 'Disabled' -ResourceId $policy.Id
```

> [!NOTE]
> hello säkerhetskopieringar som redan finns i hello valvet påverkas inte. De tas bort automatiskt av hello recovery-tjänsten när deras kvarhållningsperiod upphör att gälla.

## <a name="long-term-backup-retention-faq"></a>Långsiktig lagring av säkerhetskopior vanliga frågor och svar

**Kan jag manuellt ta bort specifika säkerhetskopior i hello valvet?**

För närvarande inte. hello valvet rensas automatiskt säkerhetskopior när hello kvarhållningsperiod har upphört att gälla.

**Kan jag registrera Min server toostore säkerhetskopieringar toomore än en valvet?**

Nej, kan du för närvarande lagra säkerhetskopior tooonly ett valv i taget.

**Kan jag ha ett valv och server för olika prenumerationer?**

Nej, för närvarande hello valvet och servern måste inneha hello samma prenumeration och resurs-grupp.

**Kan jag använda ett valv som skapats i en region som skiljer sig från Min server region?**

Nej, hello valvet och servern måste vara i hello samma region toominimize kopiera tid och undvika trafik debiteringar.

**Hur många databaser kan lagra i ett valv?**

Vi stöder för närvarande in too1 000 databaser per valvet. 

**Hur många valv kan skapa per prenumeration?**

Du kan skapa upp too25 valv per prenumeration.

**Hur många databaser kan konfigurera per dag per valvet?**

Du kan ställa in 200 databaser per dag per valvet.

**Fungerar långsiktig lagring av säkerhetskopior med elastiska pooler?**

Ja. Alla databaser i poolen hello kan konfigureras med hello bevarandeprincip.

**Kan jag välja hello tid då hello säkerhetskopian?**

Nej, SQL-databas kontrollerar hello Säkerhetskopieringsschemat toominimize hello inverkan på prestanda på dina databaser.

**Jag har transparent datakryptering är aktiverat för databasen. Kan jag använda den med hello valvet?** 

Ja, transparent datakryptering stöds. Du kan återställa hello databasen från hello valvet även om hello ursprungliga databasen finns inte längre.

**Vad händer med hello säkerhetskopieringar i hello valvet om min prenumeration har inaktiverats?** 

Om din prenumeration har inaktiverats behålla vi hello befintliga databaser och säkerhetskopieringar. Nya säkerhetskopior är inte kopierade toohello valvet. När du återaktivera hello prenumeration återupptas hello service kopiera säkerhetskopieringar toohello valvet. Ditt valv blir tillgänglig toohello återställningsåtgärder med hjälp av hello säkerhetskopior som har kopierats det innan hello prenumeration pausades. 

**Kan jag få åtkomst till toohello SQL-databasens säkerhetskopierade filer så att jag kan hämta eller återställa dem toohello SQLServer?**

Nej, inte för närvarande.

**Är det möjligt toohave flera schemalägger (varje dag, varje vecka, månad, varje år) i en SQL-bevarandeprincip.**

Ingen, flera scheman är endast tillgänglig för säkerhetskopiering för virtuella datorer.

**Vad händer om vi konfigurerar långsiktig lagring av säkerhetskopior för en databas som är en databasdagar för sekundär aktiv geo-replikering?**

Eftersom vi inte göra säkerhetskopior på repliker, finns det inget alternativ för långsiktig lagring av säkerhetskopior på sekundära databaser. Men är det viktigt för användare tooset upp långsiktig lagring av säkerhetskopior på en databasdagar för sekundär aktiv geo-replikering för följande:
* När en växling sker och hello databasen blir en primär databas, tar vi en fullständig säkerhetskopiering, vilket är överförda toovault.
* Det finns inga extra kostnad toohello kund för att ställa in långsiktig lagring av säkerhetskopior på en sekundär databas.

## <a name="next-steps"></a>Nästa steg
Eftersom databassäkerhetskopieringar skydda data från oavsiktliga skador eller tas bort, är de en viktig del av alla affärskontinuitet och haveriberedskap. toolearn om Hej andra SQL-databas kontinuitet för företag-lösningar, se [översikt över verksamhetskontinuitet](sql-database-business-continuity.md).
