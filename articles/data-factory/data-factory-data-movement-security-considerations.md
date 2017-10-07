---
title: "aaaSecurity överväganden för flytt av data i Azure Data Factory | Microsoft Docs"
description: "Lär dig mer om hur du skyddar flytt av data i Azure Data Factory."
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: abnarain
ms.openlocfilehash: 4bfce8884df14ad5b94e28ad3dfcf7025e2130a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory---security-considerations-for-data-movement"></a>Azure Data Factory - säkerhetsaspekter för dataflyttning
## <a name="introduction"></a>Introduktion
Den här artikeln beskriver grundläggande infrastruktur att data movement tjänster i Azure Data Factory använder toosecure dina data. Azure Data Factory-hanteringsresurser bygger på Azure säkerhetsinfrastruktur och Använd alla möjliga skyddsåtgärder som Azure erbjuder.

I en Data Factory-lösning skapar du en eller flera data[pipelines](data-factory-create-pipelines.md). En pipeline är en logisk gruppering aktiviteter som tillsammans utför en uppgift. Dessa pipelines finns i hello region där hello datafabriken har skapats. 

Även om Data Factory finns bara på **västra USA**, **östra USA**, och **Nordeuropa** regioner, hello data movement service är tillgänglig [globalt i flera regioner](data-factory-data-movement-activities.md#global). Data Factory-tjänsten garanterar att data inte lämna ett geografiskt område / region om du uttryckligen instruerar hello service toouse en annan region om hello data movement service inte har distribuerats toothat region. 

Azure Data Factory själva lagrar inte alla data utom länkade tjänsten autentiseringsuppgifter för molnet datalager som krypteras med certifikat. Gör det möjligt att skapa datadrivna arbetsflöden tooorchestrate flytt av data mellan [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) och bearbetning av data med hjälp av [compute services](data-factory-compute-linked-services.md) i andra regioner eller i ett lokalt miljö. Du kan också för[övervaka och hantera arbetsflöden](data-factory-monitor-manage-pipelines.md) använder både programmässiga och UI-metoder.

Flytt av data med hjälp av Azure Data Factory har **certifierade** för:
-   [HIPAA/HITECH](https://www.microsoft.com/en-us/trustcenter/Compliance/HIPAA)  
-   [ISO/IEC 27001](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27001)  
-   [ISO/IEC 27018](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27018) 
-   [CSA STJÄRNA](https://www.microsoft.com/en-us/trustcenter/Compliance/CSA-STAR-Certification)
     
Om du är intresserad av Azure efterlevnad och hur Azure skyddar sin egen infrastruktur finns hello [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/default.aspx). 

Vi går igenom säkerhetsaspekter i hello följande två scenarion för flytt av data i den här artikeln: 

- **Scenariot**– i det här scenariot både käll- och mål som är offentligt tillgänglig via internet. Dessa inkluderar hanterade lagring molntjänster som Azure Storage, Azure SQL Data Warehouse, Azure SQL Database, Azure Data Lake Store, Amazon S3, Amazon Redshift SaaS-tjänster, till exempel Salesforce och webbprotokoll, till exempel FTP och OData. Du hittar en fullständig lista över datakällor som stöds [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats).
- **Hybridscenario**– i det här scenariot källan eller målet finns bakom en brandvägg eller i ett lokalt nätverk eller hello företagsdata arkivet är i ett privat nätverk eller virtuella nätverk (oftast hello källa) och inte är allmänt tillgänglig . Database-servrar som är värd för virtuella datorer också omfattas av det här scenariot.

## <a name="cloud-scenarios"></a>Scenarier för
###<a name="securing-data-store-credentials"></a>Att säkra autentiseringsuppgifterna för datalager
Azure Data Factory skyddar autentiseringsuppgifterna för ditt datalager av **kryptera** dem med hjälp av **certifikat ska hanteras av Microsoft**. Dessa certifikat roteras varje **två år** (som inkluderar förnyelse av certifikat och migrering av autentiseringsuppgifter). Dessa krypterade autentiseringsuppgifter lagras på ett säkert sätt i en **Azure Storage hanteras av Azure Data Factory hanteringstjänster**. Mer information om säkerhet i Azure Storage [översikt över säkerheten i Azure Storage](../security/security-storage-overview.md).

### <a name="data-encryption-in-transit"></a>Datakryptering under överföring
Om hello molnet datalagret stöder HTTPS- eller TLS, alla data som överförs mellan flytt datatjänster i Data Factory och ett datalager i molnet är via en säker kanal HTTPS- eller TLS.

> [!NOTE]
> Alla anslutningar för**Azure SQL Database** och **Azure SQL Data Warehouse** alltid kräva kryptering (SSL/TLS) när data är i överföringen tooand från hello-databasen. När du redigerar en pipeline som använder en JSON-redigerare, lägger du till hello **kryptering** egenskapen och ställa in också**SANT** i hello **anslutningssträngen**. När du använder hello [guiden Kopiera](data-factory-azure-copy-wizard.md), hello guiden anger den här egenskapen som standard. För **Azure Storage**, kan du använda **HTTPS** i hello anslutningssträngen.

### <a name="data-encryption-at-rest"></a>Datakryptering i vila
Vissa data lagras stöd för kryptering av data i vila. Vi rekommenderar att du aktiverar data krypteringsmekanism för dessa databaser. 

#### <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse
Transparent Data kryptering (TDE) i Azure SQL Data Warehouse hjälper med att skydda mot hello hotet att skadlig aktivitet genom att utföra realtid kryptering och dekryptering av data i vila. Det här beteendet är transparent toohello klient. Mer information finns i [skydda en databas i SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md).

#### <a name="azure-sql-database"></a>Azure SQL Database
Azure SQL Database stöder också transparent datakryptering (TDE), vilket hjälper dig med att skydda mot hello hotet att skadlig aktivitet genom att utföra realtid kryptering och dekryptering av hello data utan ändringar toohello program. Det här beteendet är transparent toohello klient. Mer information finns i [Transparent datakryptering med Azure SQL Database](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database). 

#### <a name="azure-data-lake-store"></a>Azure Data Lake Store
Azure Data Lake store ger också kryptering för data som lagras i hello-konto. När aktiverat, krypteras data innan beständighet automatiskt Data Lake store och dekrypterar före hämtning, vilket gör det transparent toohello klientens hello dataåtkomst. Mer information finns i [säkerhet i Azure Data Lake Store](../data-lake-store/data-lake-store-security-overview.md). 

#### <a name="azure-blob-storage-and-azure-table-storage"></a>Azure Blob Storage och Azure-tabellagring
Azure Blob Storage och Azure Table storage stöder Storage Service kryptering (SSE), som automatiskt krypterar dina data innan bestående toostorage och dekrypterar före hämtning. Mer information finns i [Azure Storage Service-kryptering för Data i vila](../storage/common/storage-service-encryption.md).

#### <a name="amazon-s3"></a>Amazon S3
Amazon S3 stöder både klienten och servern kryptering av vilande data. Mer information finns i [skydda Data med hjälp av kryptering](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingEncryption.html). Data Factory stöder för närvarande inte Amazon S3 inuti en virtuell privat moln (Virtual PC).

#### <a name="amazon-redshift"></a>Amazon Redshift
Amazon Redshift stöder kluster kryptering för data i vila. Mer information finns i [Amazon Redshift databaskryptering](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-db-encryption.html). Data Factory stöder för närvarande inte Amazon Redshift inuti en Virtual PC. 

#### <a name="salesforce"></a>Salesforce
Salesforce stöder Shield plattform kryptering som har stöd för kryptering av filer, bifogade filer, anpassade fält. Mer information finns i [förstå hello Web Server OAuth-autentisering flöda](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_web_server_oauth_flow.htm).  

## <a name="hybrid-scenarios-using-data-management-gateway"></a>Hybridmoln (med Data Management Gateway)
Hybridscenarier kräver Data Management Gateway toobe installeras i ett lokalt nätverk eller i ett virtuellt nätverk (Azure) eller ett virtuellt privat moln (Amazon). hello gateway måste vara kan tooaccess hello lokala datalager. Mer information om hello gateway finns [Data Management Gateway](data-factory-data-management-gateway.md). 

![Data Management Gateway kanaler](media/data-factory-data-movement-security-considerations/data-management-gateway-channels.png)

Hej **kommandokanal** tillåter kommunikation mellan flytt datatjänster i Data Factory och Data Management Gateway. hello-meddelande innehåller information relaterad toohello aktivitet. hello datakanal används för att överföra data mellan lokala datalager och molnet dataarkiv.    

### <a name="on-premises-data-store-credentials"></a>Lokala autentiseringsuppgifterna för datalager
hello autentiseringsuppgifterna för ditt lokala datalager lagras lokalt (inte i hello moln). De kan ställas in i tre olika sätt. 

- Med hjälp av **oformaterad text** (mindre säkert) via HTTPS från Azure-portalen / guiden Kopiera. hello autentiseringsuppgifter skickas i klartext toohello lokala gateway.
- Med hjälp av **kryptering med JavaScript-bibliotek från guiden Kopiera**.
- Med hjälp av **klickar du på-när baserat autentiseringsuppgifter manager app**. Klicka på hello-när programmet körs på hello lokal dator som har åtkomst toohello gateway och anger autentiseringsuppgifterna för hello-datalagret. Det här alternativet och hello nästa är hello säkraste alternativen. hello autentiseringsuppgifter manager app använder som standard hello port 8050 på hello datorn med gateway för säker kommunikation.  
- Använd [ny AzureRmDataFactoryEncryptValue](/powershell/module/azurerm.datafactories/New-AzureRmDataFactoryEncryptValue) PowerShell cmdlet tooencrypt autentiseringsuppgifter. hello cmdlet använder hello certifikat som gatewayen är konfigurerad toouse tooencrypt hello autentiseringsuppgifter. Du kan använda hello krypterade autentiseringsuppgifter som returneras av denna cmdlet och lägga till för**EncryptedCredential** element av hello **connectionString** i hello JSON-fil som du använder med hello [ Nya AzureRmDataFactoryLinkedService](/powershell/module/azurerm.datafactories/new-azurermdatafactorylinkedservice) cmdlet eller hello JSON kodutdrag i hello Data Factory-redigeraren i hello-portalen. Genom att välja alternativet och hello-när programmet hello säkraste alternativen. 

#### <a name="javascript-cryptography-library-based-encryption"></a>JavaScript kryptografi biblioteket-baserad kryptering
Du kan kryptera autentiseringsuppgifterna för datalager med [kryptering med JavaScript-bibliotek](https://www.microsoft.com/download/details.aspx?id=52439) från hello [guiden Kopiera](data-factory-copy-wizard.md). När du väljer det här alternativet hello guiden Kopiera hämtar hello offentliga nyckeln för gateway och använder den tooencrypt hello autentiseringsuppgifterna för datalager. hello autentiseringsuppgifter dekrypteras av hello gateway-datorn och skyddas av Windows [DPAPI](https://msdn.microsoft.com/library/ms995355.aspx).

**Webbläsare som stöds:** IE8, IE9, IE10, IE11, Microsoft Edge och senaste Firefox, Chrome, Opera Safari webbläsare. 

#### <a name="click-once-credentials-manager-app"></a>Klicka på – en gång autentiseringsuppgifter manager-appen
Du kan starta hello Klicka-när baserat autentiseringsuppgifter manager-appen från Azure portal/kopiera guiden när du redigerar pipelines. Det här programmet säkerställer att autentiseringsuppgifter inte överförs i klartext över hello-överföring. Som standard används den hello port **8050** på hello datorn med gateway för säker kommunikation. Om det behövs kan du ändra den här porten.  
  
![HTTPS-port för hello-gateway](media/data-factory-data-movement-security-considerations/https-port-for-gateway.png)

Data Management Gateway används för närvarande, en enda **certifikat**. Det här certifikatet har skapats under hello gateway-installationen (gäller tooData Management Gateway som skapats efter November 2016 och version 2.4.xxxx.x eller senare). Du kan ersätta det här certifikatet med din egen SSL/TLS-certifikat. Det här certifikatet används genom att klicka på hello-när autentiseringsuppgifter manager programmet toosecurely ansluta toohello gateway-datorn för att ange autentiseringsuppgifterna för datalager. Data sparas autentiseringsuppgifterna för datalager på ett säkert sätt lokalt med hjälp av Windows hello [DPAPI](https://msdn.microsoft.com/library/ms995355.aspx) på hello-dator med en gateway. 

> [!NOTE]
> Äldre gateways som har installerats före November 2016 eller version 2.3.xxxx.x fortsätta toouse autentiseringsuppgifterna krypteras och lagras i molnet. Även om du uppgraderar hello gateway toohello senaste versionen är hello autentiseringsuppgifterna inte migrerade tooan lokal dator    
  
| Gateway-versionen (under generering) | Autentiseringsuppgifterna lagrades | Autentiseringsuppgifter kryptering / säkerhet | 
| --------------------------------- | ------------------ | --------- |  
| < = 2.3.xxxx.x | I molnet | Krypteras med certifikat (som skiljer sig från hello som används av autentiseringsuppgifter manager app) | 
| > = 2.4.xxxx.x | Lokalt | Skyddad via DPAPI | 
  

### <a name="encryption-in-transit"></a>Kryptering under överföring
Alla dataöverföringar är via en säker kanal **HTTPS** och **TLS via TCP** tooprevent man-in-the-middle-attacker under kommunikationen med Azure-tjänster.
 
Du kan också använda [IPSec VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md) eller [Express Route](../expressroute/expressroute-introduction.md) toofurther säker hello kommunikationskanalen mellan ditt lokala nätverk och Azure.

Virtuellt nätverk är en logisk representation av nätverket i hello molnet. Du kan ansluta ett virtuellt Azure-nätverk (VNet) med lokala nätverk tooyour genom att skapa IPSec-VPN (plats-till-plats) eller Express Route (privat Peering)       

hello sammanfattas följande tabell hello nätverks- och gateway rekommendationer baserat på olika kombinationer av käll- och målplatserna för hybrid dataförflyttning.

| Källa | Mål | Nätverkskonfiguration | Installationsprogram för gateway |
| ------ | ----------- | --------------------- | ------------- | 
| Lokal | Virtuella datorer och molntjänster som är distribuerad i virtuella nätverk | IPSec-VPN (punkt-till-plats eller plats-till-plats) | Gateway kan installeras antingen lokalt eller på en virtuell Azure dator (VM) i virtuella nätverk | 
| Lokal | Virtuella datorer och molntjänster som är distribuerad i virtuella nätverk | ExpressRoute (privat Peering) | Gateway kan installeras antingen lokalt eller på en Azure VM i VNet | 
| Lokal | Azure-baserade tjänster som har en offentlig slutpunkt | ExpressRoute (offentlig Peering) | Gateway måste vara installerad lokalt | 

hello följande bilder visar hello användning av Data Management Gateway för att flytta data mellan en lokal databas och Azure-tjänster med hjälp av expressroute- och IPSec-VPN (med virtuella nätverk):

**Expressroute:**
 
![Använda Expressroute med gateway](media/data-factory-data-movement-security-considerations/express-route-for-gateway.png) 

**IPSec-VPN:**

![IPSec VPN med gateway](media/data-factory-data-movement-security-considerations/ipsec-vpn-for-gateway.png)

### <a name="firewall-configurations-and-whitelisting-ip-address-of-gateway"></a>Brandväggskonfigurationer och vitlistning av IP-adressen för gateway

#### <a name="firewall-requirements-for-on-premisesprivate-network"></a>Krav för brandväggen för för lokala/privat nätverk  
Företag kan en **företagsbrandvägg** körs på hello centrala routern hello organisation. Och, **Windows-brandväggen** körs som en daemon på hello lokala dator på vilken hello gatewayen har installerats. 

hello följande tabell innehåller **utgående port** och domän för hello **företagsbrandvägg**.

| Domännamn | Utgående portar | Beskrivning |
| ------------ | -------------- | ----------- | 
| `*.servicebus.windows.net` | 443, 80 | Krävs av hello gateway tooconnect toodata flytt tjänster i Data Factory |
| `*.core.windows.net` | 443 | Används av hello gateway tooconnect tooAzure Storage-konto när du använder hello [mellanlagrad kopiera](data-factory-copy-activity-performance.md#staged-copy) funktion. | 
| `*.frontend.clouddatahub.net` | 443 | Krävs av hello gateway tooconnect toohello Azure Data Factory-tjänsten. | 
| `*.database.windows.net` | 1433   | (Valfritt) krävs när målet är Azure SQL Database / Azure SQL Data Warehouse. Använd hello mellanlagrad kopiera funktionen toocopy data tooAzure SQL Database-/ Azure SQL Data Warehouse utan att öppna hello-port 1433. | 
| `*.azuredatalakestore.net` | 443 | (Valfritt) krävs när målet är Azure Data Lake store | 

> [!NOTE] 
> Du kan ha toomanage portar / vitlistning domäner på hello företagsbrandvägg nivå som krävs av respektive datakällor. Den här tabellen använder bara Azure SQL Database, Azure SQL Data Warehouse, Azure Data Lake Store som exempel.   

hello följande tabell innehåller **inkommande port** krav för hello **windows-brandväggen**.

| Ingående portar | Beskrivning | 
| ------------- | ----------- | 
| 8050 (TCP) | Krävs av hello autentiseringsuppgifter manager programmet toosecurely ange autentiseringsuppgifter för lokala datalager på hello gateway. | 

![Krav för gateway-port](media\data-factory-data-movement-security-considerations/gateway-port-requirements.png) 

#### <a name="ip-configurations-whitelisting-in-data-store"></a>IP-konfigurationer / vitlistning i data store
Vitlistning av IP-adressen för hello datorn åtkomst till dem kräver dessutom att vissa datalager i hello molnet. Se till att hello IP-adressen för gateway-datorn hello är godkända / konfigurerad i brandväggen.

hello kräver följande molntjänster datalager vitlistning av IP-adressen för hello gateway-datorn. Vissa av dessa datalager som standard får inte kräva vitlistning av hello IP-adress. 

- [Azure SQL Database](../sql-database/sql-database-firewall-configure.md) 
- [Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md#create-a-server-level-firewall-rule-in-the-azure-portal)
- [Azure Data Lake Store](../data-lake-store/data-lake-store-secure-data.md#set-ip-address-range-for-data-access)
- [Azure Cosmos DB](../documentdb/documentdb-firewall-support.md)
- [Amazon Redshift](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) 

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar

**Fråga:** kan hello Gateway delas mellan olika datafabriker?
**Svar:** vi stöder inte den här funktionen ännu. Vi arbetar aktivt på den.

**Fråga:** vilka är kraven för hello-port för hello gateway toowork?
**Svar:** Gateway gör HTTP-baserade anslutningar tooopen internet. Hej **utgående portarna 443 och 80** måste vara öppna för gateway-toomake den här anslutningen. Öppna **inkommande Port 8050** endast på hello datornivån (inte på företagets brandvägg nivån) för Autentiseringshanteraren program. Om Azure SQL Database eller Azure SQL Data Warehouse används som källa / mål och du behöver tooopen **1433** samt port. Mer information finns i [konfigurationer och vitlistning av IP-adresser i brandväggen](#firewall-configurations-and-whitelisting-ip-address-of gateway) avsnitt. 

**Fråga:** vilka är certifikatkraven för Gateway?
**Svar:** aktuella gatewayen kräver ett certifikat som används av hello autentiseringsuppgifter manager-program för att ange autentiseringsuppgifterna för datalager på ett säkert sätt. Det här certifikatet är ett självsignerat certifikat som skapas och konfigureras av hello installationsprogram för gateway. Du kan använda din egen TLS / SSL-certifikat i stället. Mer information finns i [klickar du på-manager-program på en gång autentiseringsuppgifter](#click-once-credentials-manager-app) avsnitt. 

## <a name="next-steps"></a>Nästa steg
Information om prestanda för kopieringsaktiviteten finns [kopiera aktivitet prestanda och prestandajustering guiden](data-factory-copy-activity-performance.md).

 
