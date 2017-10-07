---
title: aaaAuditing i Azure SQL Data Warehouse | Microsoft Docs
description: "Kom igång med granskning i Azure SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: 0e6af148-b218-4b43-bb5f-907917d20330
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 08/21/2017
ms.author: rortloff;barbkess
ms.openlocfilehash: 948de74fa052ef206cf1aa65c0d81f084b18cb00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="auditing-in-azure-sql-data-warehouse"></a>Granskning i Azure SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Granskning](sql-data-warehouse-auditing-overview.md)
> * [Hotidentifiering](sql-data-warehouse-security-threat-detection.md)
> 
> 

SQL Data Warehouse granskning kan du toorecord händelser i din databas tooan granskningslogg i ditt Azure Storage-konto. Granskning kan hjälpa dig att upprätthålla regelefterlevnad, Förstå Databasaktivitet och få insyn i avvikelser och fel som kan tyda på affärsproblem eller potentiella säkerhetsöverträdelser. SQL Data Warehouse granskning är integrerat med Microsoft Power BI för nedåt rapportering och analys.

Granskningsverktyg aktivera och underlätta följer toocompliance standarder men garantera inte efterlevnad. Mer information om Azure-program som stöd för överensstämmelse med standarder, finns hello <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Azure Säkerhetscenter</a>.

* [Grunderna för granskning av databasen]
* [Konfigurera granskning för databasen]
* [Analysera granskningsloggar och rapporter]

## <a id="subheading-1"></a>Azure SQL Data Warehouse Database Auditing grunderna
Granskning av SQL Data Warehouse-databas kan du:

* **Behåll** redovisningsspårning markerade händelser. Du kan definiera kategorier av databasen åtgärder toobe granskas.
* **Rapporten** på Databasaktivitet. Du kan använda förkonfigurerade rapporter och en instrumentpanel tooget snabbt igång med aktivitet och rapportera händelser.
* **Analysera** rapporter. Du kan hitta misstänkta händelser, ovanliga aktiviteter och trender.

Du kan konfigurera granskning för hello följande kategorier:

**Vanlig SQL** och **parametriserade SQL** för vilka hello insamlade granskningsloggar klassificeras som  

* **Åtkomst toodata**
* **Schemaändringar (DDL)**
* **Dataändringar (DML)**
* **Konton, roller och behörigheter (DCL)**
* **Lagrade proceduren**, **inloggning** och **transaktionshantering**.

För varje händelsekategori granskning av **lyckade** och **fel** åtgärder har konfigurerats separat.

Mer information om hello aktiviteter och händelser som granskas finns hello <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">referens till granskningslogg Format (doc Filhämtning)</a>.

Granskningsloggar lagras i Azure storage-konto. Du kan definiera en Granska loggen Bevarandeperiod.

En granskningsprincip kan definieras för en viss databas eller som en standardprincip för servern. Granskningsprincip för servern som ska användas som standard tillämpar tooall databaser på en server som inte har en specifik åsidosättning databasen granskning princip definierad.

Innan du konfigurerar audit granskning kontrollera om du använder en [”meddelandeklienter”.](sql-data-warehouse-auditing-downlevel-clients.md)

## <a id="subheading-2"></a>Konfigurera granskning för databasen
1. Starta hello <a href="https://portal.azure.com" target="_blank">Azure-portalen</a>.
2. Gå toohello **inställningar** bladet för hello SQL Data Warehouse som du vill tooaudit. I hello **inställningar** bladet väljer **Auditing & Threat detection**.
   
    ![][1]
3. Därefter aktiverar granskning genom att klicka på hello **ON** knappen.
   
    ![][3]
4. Välj i hello granskning configuration-bladet, **LAGRINGSINFORMATION** tooopen hello granska loggarna lagring-bladet. Välj hello Azure storage-konto där loggar sparas och hello Bevarandeperiod. 
>[!TIP]
>Använd hello samma lagringskonto för alla granskad databaser tooget hello mesta av hello förkonfigurerade rapporter mallar.
   
    ![][4]
5. Klicka på hello **OK** information för knappen toosave hello lagringskonfiguration.
6. Under **loggning av HÄNDELSEN**, klickar du på **lyckade** och **fel** toolog alla händelser eller välja enskilda kategorier.
7. Om du konfigurerar granskning för en databas, kanske du måste tooalter hello anslutningssträngen för din klient tooensure data granskning korrekt avbildas. Kontrollera hello [ändra Server FDQN i anslutningssträngen för hello](sql-data-warehouse-auditing-downlevel-clients.md) avsnittet för äldre klientanslutningar.
8. Klicka på **OK**.

## <a id="subheading-3"></a>Analysera granskningsloggar och rapporter
Granskningsloggar aggregeras i en samling av Store-tabeller med en **SQLDBAuditLogs** prefix i hello Azure storage-konto som du valde under installationen. Du kan visa loggfiler med hjälp av ett verktyg som <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure Lagringsutforskaren</a>.

Rapportmall förkonfigurerade instrumentpanelen är tillgänglig som en <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">nedladdningsbara Excel-kalkylblad</a> toohelp snabbt analysera loggdata. toouse hello mallen på din granskningsloggar, behöver du Excel 2013 eller senare och Power Query, som du kan hämta <a href="http://www.microsoft.com/download/details.aspx?id=39379">här</a>.

hello mallen har fiktiva exempeldata i den och du kan ställa in Power Query tooimport din granskningsloggen direkt från Azure storage-konto.

## <a id="subheading-4"></a>Sessionsnycklar för lagring
I produktion är förmodligen toorefresh lagringen nycklar med jämna mellanrum. När du uppdaterar dina nycklar måste toosave hello princip. hello-processen är som följer:

1. Växla hello i hello granskning configuration bladet (beskrivs ovan i hello installationsprogrammet granskning avsnitt) **Lagringsåtkomstnyckel** från *primära* för*sekundära* och  **Spara**.

   ![][4]
2. Gå toohello lagring configuration-bladet och **återskapa** hello *primära åtkomstnyckeln*.
3. Gå tillbaka toohello granskning configuration bladet växeln hello **Lagringsåtkomstnyckel** från *sekundära* för*primära* och tryck på **spara**.
4. Gå tillbaka toohello lagring UI och **återskapa** hello *sekundära åtkomstnyckel* (som förberedelse för hello uppdatera nycklar för nästa cykel.

## <a id="subheading-5"></a>Automation (PowerShell/REST API)
Du kan också konfigurera granskning i Azure SQL Data Warehouse med hjälp av följande verktyg för automatisering hello:

* **PowerShell-cmdlets**:

   * [Get-AzureRMSqlDatabaseAuditingPolicy][101]
   * [Get-AzureRMSqlServerAuditingPolicy][102]
   * [Ta bort AzureRMSqlDatabaseAuditing][103]
   * [Ta bort AzureRMSqlServerAuditing][104]
   * [Ange AzureRMSqlDatabaseAuditingPolicy][105]
   * [Ange AzureRMSqlServerAuditingPolicy][106]
   * [Använd AzureRMSqlServerAuditingPolicy][107]

<!--Anchors-->
[Grunderna för granskning av databasen]: #subheading-1
[Konfigurera granskning för databasen]: #subheading-2
[Analysera granskningsloggar och rapporter]: #subheading-3


<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-dashboard.png


<!--Link references-->
[101]: /powershell/module/azurerm.sql/get-azurermsqldatabaseauditingpolicy
[102]: /powershell/module/azurerm.sql/Get-AzureRMSqlServerAuditingPolicy
[103]: /powershell/module/azurerm.sql/Remove-AzureRMSqlDatabaseAuditing
[104]: /powershell/module/azurerm.sql/Remove-AzureRMSqlServerAuditing
[105]: /powershell/module/azurerm.sql/Set-AzureRMSqlDatabaseAuditingPolicy
[106]: /powershell/module/azurerm.sql/Set-AzureRMSqlServerAuditingPolicy
[107]: /powershell/module/azurerm.sql/Use-AzureRMSqlServerAuditingPolicy