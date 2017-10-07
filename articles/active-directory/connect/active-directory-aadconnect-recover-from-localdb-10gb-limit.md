---
title: "Azure AD Connect: Hur toorecover från LocalDB 10GB begränsa problemet | Microsoft Docs"
description: "Det här avsnittet beskrivs hur toorecover Azure AD Connect-synkronisering-tjänsten när den stöter på LocalDB 10GB begränsa problemet."
services: active-directory
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: 41d081af-ed89-4e17-be34-14f7e80ae358
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: billmath
ms.openlocfilehash: 7b8ce6e19b68837639017bb0315eda4b924d525a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-how-toorecover-from-localdb-10-gb-limit"></a>Azure AD Connect: Hur toorecover från LocalDB 10 GB gränsen
Azure AD Connect kräver en SQL Server-databasen toostore identity-data. Du kan använda hello standard SQL Server 2012 Express LocalDB installerad med Azure AD Connect, eller så kan du använda din egen fullständig SQL. SQL Server Express har en storleksgräns på 10 GB. När du använder LocalDB och gränsen har uppnåtts kan synkroniseringstjänsten för Azure AD Connect inte längre starta eller synkronisera korrekt. Den här artikeln innehåller steg för återställning av hello.

## <a name="symptoms"></a>Symtom
Det finns två vanliga problem:

* Azure AD Connect-synkroniseringstjänsten **körs** men inte toosynchronize med *”stoppats databas-disk-fullständiga”* fel.

* Azure AD Connect-synkroniseringstjänsten **är toostart**. När du försöker toostart hello-tjänsten misslyckas med händelse 6323 och felmeddelandet *”hello servern påträffade ett fel eftersom SQLServer är slut på diskutrymme”.*

## <a name="short-term-recovery-steps"></a>Kortsiktig steg för återställning
Det här avsnittet innehåller hello steg tooreclaim DB-utrymme som krävs för Azure AD Connect-synkroniseringstjänsten tooresume igen. hello stegen innefattar:
1. [Identifiera hello synkroniseringstjänsten status](#determine-the-synchronization-service-status)
2. [Minska hello-databas](#shrink-the-database)
3. [Ta bort kör historikdata](#delete-run-history-data)
4. [Ange ett kortare kvarhållningsperiod för körningshistorik data](#shorten-retention-period-for-run-history-data)

### <a name="determine-hello-synchronization-service-status"></a>Identifiera hello synkroniseringstjänsten status
Bestäm först om hello-synkroniseringstjänsten körs fortfarande eller inte:

1. Logga in tooyour Azure AD Connect-servern som administratör.

2. Gå för**Service Control Manager**.

3. Kontrollera hello status **Microsoft Azure AD Sync**.


4. Om den körs inte stoppa eller starta om hello-tjänsten. Hoppa över [förminskas hello databasen](#shrink-the-database) steget och gå för[ta bort kör historikdata](#delete-run-history-data) steg.

5. Om den inte körs försöker du toostart hello service. Om hello-tjänsten startar hoppa över [förminskas hello databasen](#shrink-the-database) steget och gå för[ta bort kör historikdata](#delete-run-history-data) steg. Annars fortsätter du med [förminskas hello databasen](#shrink-the-database) steg.

### <a name="shrink-hello-database"></a>Minska hello-databas
Använd hello förminskas åtgärden toofree in tillräckligt med DB utrymme toostart hello synkroniseringstjänsten. Det Frigör DB-utrymme genom att ta bort blanksteg i hello-databasen. Det här steget är bästa eftersom det inte är säkert att du alltid återställa utrymme. toolearn mer om komprimeringsåtgärden igen den här artikeln [krympa en databas](https://msdn.microsoft.com/library/ms189035.aspx).

> [!IMPORTANT]
> Hoppa över det här steget om du kan hämta hello synkroniseringstjänsten toorun. Det rekommenderas inte tooshrink hello SQL-databas som den kan leda toopoor prestanda på grund av fragmentering av tooincreased.

hello hello-databas som skapats för Azure AD Connect heter **ADSync**. tooperform en komprimeringsåtgärden, måste du logga antingen som hello sysadmin eller DBO hello-databasen. Under installationen av Azure AD Connect har hello följande konton beviljats sysadmin-behörighet:
* Lokala administratörer
* hello-användarkonto som har använt toorun Azure AD Connect-installationen.
* hello Sync-tjänstkontot som används som hello operativsystem kontexten för Azure AD Connect-synkroniseringstjänsten.
* hello gruppen ADSyncAdmins som skapades under installationen.

1. Säkerhetskopiera hello databasen genom att kopiera **ADSync.mdf** och **ADSync_log.ldf** filer som finns `%ProgramFiles%\program files\Microsoft Azure AD Sync\Data` tooa säker plats.

2. Starta en ny PowerShell-session.

3. Navigera toofolder `%ProgramFiles%\Program Files\Microsoft SQL Server\110\Tools\Binn`.

4. Starta **sqlcmd** utility genom att köra kommandot hello `./SQLCMD.EXE -S “(localdb)\.\ADSync” -U <Username> -P <Password>`, med hjälp av hello autentiseringsuppgifter i en sysadmin eller hello databasen DBO.

5. tooshrink hello databasen på hello sqlcmd-kommandotolk (1 >), ange `DBCC Shrinkdatabase(ADSync,1);`, följt av `GO` hello nästa rad.

6. Försök igen toostart hello synkroniseringstjänsten om hello åtgärden lyckas. Om du kan starta hello synkroniseringstjänsten gå för[ta bort kör historikdata](#delete-run-history-data) steg. Om inte, kontakta supporten.

### <a name="delete-run-history-data"></a>Ta bort kör historikdata
Som standard behåller Azure AD Connect upp tooseven dagars kör historikdata. I det här steget vi med att ta bort hello kör historik datautrymme tooreclaim DB så att Azure AD Connect-synkroniseringstjänsten kan starta synkronisera igen.

1.  Starta **Synchronization Service Manager** genom att gå tooSTART → synkroniseringstjänsten.

2.  Gå toohello **Operations** fliken.

3.  Under **åtgärder**väljer **Rensa körs**...

4.  Du kan antingen välja **avmarkera alla körningar** eller **Rensa körs innan... <date>**  alternativet. Vi rekommenderar att du börjar genom att avmarkera kör historikdata som är äldre än två dagar. Om du fortsätter toorun i DB storlek problemet och välj sedan hello **avmarkera alla körningar** alternativet.

### <a name="shorten-retention-period-for-run-history-data"></a>Ange ett kortare kvarhållningsperiod för körningshistorik data
Det här steget är tooreduce hello sannolikheten för att köra i hello 10 GB gränsen problemet efter flera synkroniseringscyklerna.

1. Öppna en ny PowerShell-session.

2. Kör `Get-ADSyncScheduler` och anteckna hello PurgeRunHistoryInterval-egenskap som anger hello aktuella Bevarandeperiod.

3. Kör `Set-ADSyncScheduler -PurgeRunHistoryInterval 2.00:00:00` tooset hello kvarhållning period tootwo dagar. Justera hello kvarhållningsperiod efter behov.

## <a name="long-term-solution--migrate-toofull-sql"></a>Långsiktig lösning – migrera toofull SQL
I allmänhet hello problemet är jämförbar 10 GB-databasens storlek är inte längre tillräckliga för Azure AD Connect toosynchronize din lokala Active Directory tooAzure AD. Vi rekommenderar att du växla toousing hello fullständig version av SQLServer. Du kan inte direkt ersätta hello LocalDB av en befintlig distribution av Azure AD Connect med hello-databas med hello fullständig version av SQL Server. I stället måste du distribuera en ny Azure AD Connect-server med hello fullständig version av SQL Server. Vi rekommenderar att du gör en öppning migrering där hello ny Azure AD Connect-server (med SQL DB) distribueras som en fristående server, nästa toohello befintlig Azure AD Connect-server (med LocalDB). 
* Anvisningar om hur tooconfigure fjärr-SQL med Azure AD Connect finns tooarticle [anpassad installation av Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-custom).
* Anvisningar för migrering av öppning för uppgraderingen av Azure AD Connect finns tooarticle [Azure AD Connect: uppgradera från en tidigare version toohello senaste](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#swing-migration).

## <a name="next-steps"></a>Nästa steg
Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).
