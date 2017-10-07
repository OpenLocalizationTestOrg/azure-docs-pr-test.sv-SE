---
title: "aaaCortana Intelligence lösning utvärderingsverktyget | Microsoft Docs"
description: "Här är alla hello stegen toofollow toopublish tooAppSource din Cortana Intelligence-lösning som en Microsoft-Partner."
services: machine-learning
documentationcenter: 
author: AnupamMicrosoft
manager: jhubbard
editor: cgronlun
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: anupams;v-bruham;garye
ms.openlocfilehash: 76cde4e2090c121683b7026f3d80f90f64566607
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="cortana-intelligence-solution-evaluation-tool"></a>Utvärderingsverktyg för Cortana Intelligence-lösningar
## <a name="overview"></a>Översikt
Du kan använda hello Cortana Intelligence lösning evaluation tool tooassess avancerade analyser-lösningar för kompatibilitet med Microsoft-rekommenderad bästa praxis. Microsoft är glad över toowork med våra partners (ISV: er / SIs) tooprovide hög kvalitet lösningar för kunder, återförsäljare och implementering. Den här guiden ska gå igenom hello processen för utvärderingsverktyget för hello lösning med din lösning och beskrivs hello specifika metodtips i söker efter.

## <a name="getting-started"></a>Komma igång
Kontrollera [hämta](https://aka.ms/aa-evaluation-tool-download) och installera utvärderingsverktyg för hello Cortana Intelligence-lösning.

Krav:
- Windows 10: [officiella platsen för Windows 10](https://www.microsoft.com/en-us/windows)
- Azure Powershell: [installera och konfigurera Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).

## <a name="identifying-your-app"></a>Identifierar din app
När installationen är klar öppnar hello verktyget och påbörja första utvärderingen.

![Öppna utvärderingsverktyg](./media/cortana-intelligence-appsource-evaluation-tool/1-open-evaluation-tool.png)

Ange information om din lösning.

![Anslut Azure-prenumeration](./media/cortana-intelligence-appsource-evaluation-tool/2-connect-azure-subscription.png)

Anslut tooyour Azure-prenumeration och ange hello resursgruppen som innehåller din app.

![Välj resurser](./media/cortana-intelligence-appsource-evaluation-tool/3-select-resources.png)

När hello resursgruppen har lästs in, markera hello-resurser som ingår i din lösning och identifiera hello tillgänglighet för data resurserna som antingen:
- Införandet
- Förbrukning
- internt

Vi använder informationen för toobetter förstå hur din lösning är att använda olika komponenter och tooensure användarinriktad komponenter stämmer överens med bästa praxis.

### <a name="ingestion"></a>Införandet
Införandet i det här fallet innebär att alla datakällor som används toopull i data från utanför hello lösning eller att inga tjänster utanför hello lösningen använder toopush data till den.

### <a name="consumption"></a>Förbrukning
Förbrukning i det här fallet innebär alla datauppsättningar som används toopush data tooend användare direkt eller indirekt. Exempel:
- Datauppsättningar som används i direktfrågan från PowerBI.
- Datauppsättningar frågas i en WebApp.

>[!NOTE]
Om en viss resurs används för både införandet och förbrukning, Välj **förbrukning**.

### <a name="internal"></a>internt
Används internt för alla dataresurser som används bara i internt programbearbetning.

Sedan kommer du att ange tooprovide giltiga autentiseringsuppgifter för alla databaser som angetts i hello föregående steg:

![Ange test krav](./media/cortana-intelligence-appsource-evaluation-tool/4-set-test-prerequisites.png)

## <a name="solution-test-cases"></a>Lösning testfall
hello lösning verktyget utför en mängd testerna på din lösning.

![Körning av test](./media/cortana-intelligence-appsource-evaluation-tool/5-set-test-execution.png)

När hello testen är klara kan du kommer att tillfrågas tooprovide en förklaring eller en motivering för varför din lösning uppfyller inte kravet på hello.

![Ange en motivering](./media/cortana-intelligence-appsource-evaluation-tool/6-provide-business-justification.png)

Om din lösning publicerar tooAzure SQL DW, publicera hello utvärdering testerna behöver du tooalso tooAzure Analysis Services. 

Din lösning kan använda IaaS virtuella datorer som kör Sql Server Analysis Services i stället för Azure Analysis Services. Detta är en godkänd orsak till felet i hello test.
## <a name="packaging-your-evaluation-results"></a>Paketera din utvärderingsresultat
När du har slutfört hello testfall utvärdering paketet kommer att exporterade tooa zip-filen och du blir ombedd tooprovide feedback på hello utvärderingsverktyg. 

Du behöver tooshare detta test resulterar zip-filen med Microsoft för din lösning toobe utvärderas innan du hämtar godkännande toobe läggs tooAppSource

![Utvärderingsverktyget för klass](./media/cortana-intelligence-appsource-evaluation-tool/7-grade-evaluation-tool.png)

Ovan avsnitt i det här artikeln beskriver olika funktioner i verktyget hello, granska nu typer av bästa praxis som det här verktyget utvärderar Låt oss.

## <a name="security-evaluation-considerations"></a>Säkerhetsaspekter för utvärdering
### <a name="databases-should-use-azure-active-directory-authentication"></a>Databaser ska använda Azure Active Directory-autentisering
Azure SQL- eller Azure SQL DW resurser i hello sloution ska aktiveras med Azure Active Directory (AAD)-autentisering. AAD innehåller en enda plats toomanage alla identiteter och roller.

| Mer information om | Finns den här artikeln |
| --- | --- |
| AAD med SQL Database och SQL Data Warehouse | [Använda Azure Active Directory-autentisering för autentisering med SQL Database eller SQL Data Warehouse](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-aad-authentication) |
| Konfigurera och hantera AAD | [Konfigurera och hantera Azure Active Directory-autentisering med SQL Database eller SQL Data Warehouse](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-aad-authentication-configure) |
| Autentisering med Azure Webbappar | [Autentisering och auktorisering i Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/app-service-authentication-overview) |
| Konfigurera Webbappar i AAD | [Hur tooconfigure din Apptjänst programmet toouse Azure Active Directory-inloggning](https://docs.microsoft.com/en-us/azure/app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication)|

### <a name="datasets-accessible-tooend-users-should-support-role-based-access-control"></a>Datauppsättningar tillgänglig tooend-användarna ska ha stöd för rollbaserad åtkomstkontroll
Vid körning hello utvärderingsverktyg du kommer att tillfrågas toospecify rapportering eller publicera resurser. Det förutsätts att resurserna är avsedda för åtkomst av slutanvändare, inte utvecklare. Dessa resurser bör vara ge rollbaserad åtkomstkontroll (RBAC) i ordning tooensure slutanvändare som endast kan tooaccess behörighet data.

Mer specifikt kan något av följande Azure-resurser hello kan konfigureras med RBAC och anses acceptabla:
- Säker HDInsight, se [en introduktion tooHadoop säkerhet med domänanslutna HDInsight-kluster](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-domain-joined-introduction)
- Azure SQL Se [AAD-autentisering med Azure SQL]( https://docs.microsoft.com/en-us/azure/sql-database/sql-database-aad-authentication)
- Azure Analysis Services, se [hantera databasroller och användare för Azure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-database-users)
- Azure SQL Data Warehouse (Tänk på att eftersom SQL DW stöder RBAC, inte rekommenderas för direkta slutanvändarnas åtkomst.)

Om du använder en annan resurstyp som stöder RBAC kan ange som i hello testfall justering.

### <a name="azure-data-lake-store-should-use-at-rest-encryption"></a>Azure Data Lake Store ska använda kryptering i vila
Azure Data Lake Store (ADLS) stöder kryptering i vila som standard med hjälp av ADLS-hanterade krypteringsnycklar. Du kan också konfigurera kryptering med Azure Key Vault.

Information om hur du anger ADLS krypteringsinställningar [skapa ett Azure Data Lake Store-konto](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-get-started-portal#create-an-azure-data-lake-store-account).

### <a name="azure-sql-and-azure-sql-data-warehouse-should-use-encryption"></a>Azure SQL och Azure SQL Data Warehouse ska använda kryptering
Azure SQL och Azure SQL DW båda stöder Transparent Data kryptering (TDE), vilket ger realtid kryptering och dekryptering av både data och loggfiler.

| Mer information om | Finns den här artikeln |
| --- | --- |
| Transparent datakryptering (TDE) | [Transparent datakryptering](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-tde) |
| Azure SQL Data Warehouse med TDE | [SQL Data Warehouse Encrption TDE TSQL](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-encryption-tde-tsql) |
| Konfigurera Azure SQL med TDE | [Transparent datakryptering med Azure SQL Database](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database) |
| Konfigurera Azure SQL med Always Encrypted | [SQL-databas Encrypted Always Azure Key Vault](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-always-encrypted-azure-key-vault)|

Dessutom tooTDE, Azure SQL också stöder Always Encrypted, en ny krypteringsteknik för data som garanterar data krypteras inte bara i vila och under flytt mellan klienten och servern, utan även när data används när du kör kommandon på hello-servern.

### <a name="any-virtual-machines-must-be-deployed-from-hello-azure-marketplace"></a>Alla virtuella datorer måste distribueras från hello Azure Marketplace
Ordning tooprovide en konsekvent säkerhetsnivå över AppSource kräver att alla virtuella datorer som distribueras som en del av en Cortana Intelligence-lösning som är certifierade och publiceras på hello Azure Marketplace.

toosearch hello aktuella listan över Azure Marketplace-avbildningar, se [Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute).

Mer information om hur toopublish en virtuell dator bild för Azure Marketplace finns [Guide toocreate en avbildning av virtuell dator för hello Azure Marketplace](https://docs.microsoft.com/en-us/azure/marketplace-publishing/marketplace-publishing-vm-image-creation).

## <a name="scalability-evaluation-considerations"></a>Överväganden för utvärdering av skalbarhet
### <a name="cortana-intelligence-solutions-should-include-a-scalable-big-data-platform"></a>Cortana Intelligence-lösningar ska innehålla en skalbar stordataplattform
Cortana Intelligence-lösningar bör skala toovery stora datamängder. Detta innebär i Azure, de ska innehålla en hello två skalning Petabyte data plattformar:
- Azure Data Lake Store
- Azure SQL Data Warehouse

Om din lösning inte behöver support för dessa datamängder eller om du använder en alternativ dataplattform du förklara detta i hello testfall justering.
### <a name="cortana-intelligence-solutions-should-include-dedicated-ingestion-data-environments"></a>Cortana Intelligence-lösningar bör innehålla dedikerade införandet data miljöer
Cortana Intelligence-lösningar Undvik direkt infoga data i relationsdatakällor. Rådata bör i stället lagras i en Ostrukturerade miljö med idempotent infogningar/uppdateringar i alla relationella Arkiv med hjälp av Azure Data Factory.

Mer information om hur du kopierar data med Azure Data Factory [Självstudier: skapa en pipeline med Kopieringsaktiviteten med Visual Studio](https://docs.microsoft.com/en-us/azure/data-factory/data-factory-copy-activity-tutorial-using-visual-studio).

### <a name="azure-sql-data-warehouse-should-use-polybase-for-data-ingestion"></a>Azure SQL Data Warehouse bör använda PolyBase för datapåfyllning
Azure SQL DW stöder PolyBase för mycket skalbar, parallella datapåfyllning. PolyBase låter dig toouse Azure SQL DW tooissue frågor mot externa datauppsättningar som lagras i Azure Blob Storage eller Azure Data Lake Store. Detta ger högre prestanda tooalternative metoder för massuppdateringar.

Anvisningar för att komma igång med PolyBase och Azure SQL DW finns [Läs in data med PolyBase i SQL Data Warehouse](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-get-started-load-with-polybase).

Läs mer om bästa praxis med PolyBase och Azure SQL DW [Guide för att använda PolyBase i SQL Data Warehouse](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-load-polybase-guide).

## <a name="availability-evaluation-considerations"></a>Tillgänglighet för utvärdering

### <a name="datasets-accessible-tooend-users-should-support-a-large-volume-of-concurrent-users"></a>Datauppsättningar tillgänglig tooend användare ska ha stöd för ett stort antal samtidiga användare
Vid körning hello utvärderingsverktyg du kommer att tillfrågas toospecify rapportering eller publicera resurser. Det förutsätts att resurserna är avsedda för åtkomst av slutanvändare, inte utvecklare. Medel stort antal samtidiga användare ska ha stöd för dessa resurser.

Mer specifikt får Azure SQL Data Warehouse inte vara hello enda datakälla tillgängliga tooend användare. Om Azure SQL DW anges som en resurs för Privilegierade användare, ska Azure Analysis Services göras tillgängliga tootypical användare.

Läs mer om Azure SQL DW samtidighet gränser [hantering samtidighet och arbetsbelastning i SQL Data Warehouse](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-develop-concurrency).

Läs mer om Azure Analysis Services [Analysis Services-översikt](https://docs.microsoft.com/azure/analysis-services/analysis-services-overview).

### <a name="azure-sql-resources-should-have-a-read-only-replica-for-failover"></a>Azure SQL-resurser ska ha en skrivskyddad replik för redundans
Azure SQL-databaser stöd för sekundär geo-replikering tooa-instans. Denna instans kan sedan användas som en växling vid fel instans tooprovide hög tillgänglighet program.

Mer information om geo-replikering för Azure SQL-databaser finns [SQL Database GEO-replikering översikt](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-geo-replication-overview).

Anvisningar för hur tooconfigure geo-replikering för SQL Azure, se [konfigurera aktiv geo-replikering för Azure SQL Database med Transact-SQL](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-geo-replication-transact-sql).

### <a name="azure-sql-data-warehouse-should-have-geo-redundant-backups-enabled"></a>Azure SQL Data Warehouse bör ha geo-redundant säkerhetskopior aktiverad
Azure SQL DW stöder dagliga säkerhetskopior toogeo redundant lagring. Den här georeplikering gör att du kan återställa hello datalagret även i situationer där du inte kan komma åt ögonblicksbilder som lagras i din primära region. Den här funktionen är aktiverad som standard och bör inte inaktivera för Cortana Intelligence-lösningar.

Mer information om Azure SQL DW-säkerhetskopiering och återställning finns här [säkerhetskopiering av SQL Data Warehouse](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-backups).

### <a name="virtual-machines-should-be-configured-with-availability-sets"></a>Virtuella datorer ska konfigureras med tillgänglighetsuppsättningar
Virtuella Azure-datorer ska konfigureras i tillgänglighetsuppsättningar i ordning toominimize hello effekten av planerade och oplanerat underhållshändelser.

Läs mer om virtuella Azure-datorn tillgänglighet [hantera hello tillgängligheten för Windows-datorer i Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability).

## <a name="other-evaluation-considerations"></a>Andra överväganden för utvärdering
### <a name="cortana-intelligence-apps-should-use-a-centralized-tool-for-data-orchestration"></a>Cortana Intelligence-appar bör använda ett centraliserat verktyg för data orchestration
Med ett enda verktyg för att hantera och schemaläggning dataförflyttning och omvandling säkerställer konsekvent runt viktiga data. Det ger en tydlig logik runt logik för omprövning, beroende management, aviseringen-loggning och så vidare. Vi rekommenderar hello användning av [Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/data-factory-introduction) för data orchestration i Azure.

Om du använder ett verktyg än Azure Data Factory för data orchestration Beskriv vilka verktyg eller verktyg som du använder.
### <a name="azure-machine-learning-models-should-be-retrained-using-azure-data-factory"></a>Azure Machine Learning-modeller bör retrained med hjälp av Azure Data Factory
Azure Machine Learning (AzureML) tillhandahåller enkel toouse verktyg för hello skapande och distribution av förutsägelsemodellering och maskininlärning pipelines. Det är dock viktigt att Produktionsdistribution av dessa modeller för AzureML inte baseras på en enda fast dataset, men i stället anpassas efter dina behov toohello rörliga dynamics av verkliga fenomen.

Läs mer om hur du skapar omtränings webbtjänster i AzureML [träna om Machine Learning-modeller via programmering](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-retrain-models-programmatically).

Mer information om hur du automatiserar hello modellen utbildning processen med Azure Data Factory finns [uppdaterar Azure Machine Learning-modeller med Uppdateringsresursaktivitet](https://docs.microsoft.com/en-us/azure/data-factory/data-factory-azure-ml-update-resource-activity).

## <a name="existing-documentation"></a>Befintliga dokumentationen
[Microsoft Azure-certifierad toogrow företaget moln](https://azure.microsoft.com/en-us/marketplace/programs/certified/)

[Microsoft Azure certifierad för Cortana Intellignece](https://azure.microsoft.com/en-us/marketplace/programs/certified/cortana/)

