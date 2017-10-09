---
title: "Översikt över aaaAzure kryptering | Microsoft Docs"
description: "Lär dig mer om olika krypteringsalternativen i Azure"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 08/18/2017
ms.author: barclayn
ms.openlocfilehash: ef9ab46de32b857e99e8fe628a61386b95cf197f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-encryption-overview"></a>Översikt över Azure kryptering

Den här artikeln innehåller en översikt över hur kryptering används i Microsoft Azure. Den omfattar hello huvudområden krypteringstyp, inklusive kryptering i vila, kryptering i svarta och hantering av nycklar med Key Vault. Varje avsnitt innehåller länkar till mer detaljerad information.

## <a name="encryption-of-data-at-rest"></a>Kryptering av vilande data

Vilande data innehåller information som finns i permanent lagringsutrymme på fysiska media digitala formatet. Detta inkluderar filer på magnetiska eller optical media arkiverade data och säkerhetskopiering av data. Microsoft Azure erbjuder en mängd olika data lagring lösningar toomeet olika behov, inklusive fil-, disk-, blob- och tabellagring. Microsoft tillhandahåller också kryptering tooprotect [Azure SQL Database](../sql-database/sql-database-technical-overview.md), [CosmosDB](../cosmos-db/introduction.md), och Azure Data Lake.

Datakryptering i viloläge är tillgänglig för tjänster över hello Azure programvara som en-tjänst (SaaS), plattform som en-tjänst (PaaS), och infrastruktur-som-en-tjänst (IaaS) molnet modeller. Det här dokumentet sammanfattar och ger resurser toohelp du använda Azures krypteringsalternativen.

Mer detaljerad beskrivning av hur krypteras data i vila i Azure finns hello dokumentet [Azure Data kryptering i vila](azure-security-encryption-atrest.md)

## <a name="azure-encryption-models"></a>Azure kryptering modeller

Azure stöder olika kryptering modeller, inklusive kryptering för serversidan via service-hanterade nycklar, med kundhanterad nycklar i Azure Key Vault, eller använder kundhanterad nycklar på kund-kontrollerade maskinvara. Kryptering på klientsidan kan du toomanage och lagra nycklar lokalt eller i en annan säker plats.

### <a name="client-side-encryption"></a>Kryptering av klientsidan

Kryptering på klientsidan utförs utanför Azure. Kryptering på klientsidan innehåller:

- Data som krypterats av ett program som körs i hello kundens Datacenter eller av ett tjänstprogram
- Data som redan är krypterad när den tas emot av Azure.

Med kryptering på klientsidan hello molntjänstleverantören har inte åtkomst toohello krypteringsnycklar och det går inte att dekryptera dessa data. Du kan behålla fullständig kontroll över hello nycklar.

### <a name="server-side-encryption"></a>Kryptering på serversidan

hello tre kryptering för serversidan modeller har olika nyckelhantering egenskaper som kan väljas per dina krav.

- **Service-hanterade nycklar** ge en kombination av kontrollen och bekvämlighet låg belastning

- **Kundhanterad nycklar** ger dig kontroll över hello nycklar, inklusive hello möjlighet toobring egna nycklar (BYOK) eller toogenerate nya.

- **Service-hanterade nycklar i ccustomer controlledhardware** kan du toomanage nycklar i egna databasen som ligger utanför Microsofts kontroll. Detta kallas värden din egen nyckel (HYOK). Dock konfiguration är komplex och mest Azure services stöder inte den här modellen.

### <a name="azure-disk-encryption"></a>Azure Disk Encryption

Windows- och Linux virtuella datorer kan skyddas med [Azure Disk Encryption](azure-security-disk-encryption.md), som använder hello [Windows BitLocker](https://technet.microsoft.com/library/cc766295(v=ws.10).aspx) teknik- och Linux [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) tooprotect både operativsystemet diskar och datadiskar med fullständig volymkryptering.

Krypteringsnycklar och hemligheter skyddas i din [Azure Key Vault](../key-vault/key-vault-whatis.md) prenumeration. Du kan säkerhetskopiera och återställa krypterade virtuella datorer som är krypterade med hello KEK konfigurationen med hjälp av hello Azure Backup-tjänsten.

### <a name="azure-storage-service-encryption"></a>Azure Storage service-kryptering

Vilande data i Azure storage (Blob och filen) vara krypterad på klientsidan och serversidan scenarier.

[Azure Storage Service-kryptering](../storage/storage-service-encryption.md) (SSE) kan automatiskt att kryptera data innan de lagras och dekrypterar dem automatiskt när du hämtar, gör hello bearbeta helt transparent för användarna. Lagringstjänstens kryptering använder 256-bitars [AES-kryptering](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), vilket är en av hello starkaste block chiffer som är tillgängliga och hanterar kryptering, dekryptering och hantering av nycklar på ett transparent sätt.

### <a name="client-side-encryption-of-azure-blobs"></a>Kryptering på klientsidan av Azure-blobbar

Kryptering på klientsidan av Azure-blobbar kan utföras på olika sätt.

Du kan använda hello Azure Storage-klientbibliotek för .NET-NuGet-paketet tooencrypt data i din tidigare toouploading program för klienten den tooAzure lagring.

Mer om toolearn och hämta hello Azure Storage-klientbibliotek för .NET-NuGet-paketet finns hello dokumentet [Windows Azure Storage 8.3.0](https://www.nuget.org/packages/WindowsAzure.Storage)

När du använder kryptering på klientsidan med Azure Key Vault krypteras data med en enstaka symmetriska innehåll kryptering nyckel (CEK) som genereras av hello Azure Storage client SDK. Hej CEK har krypterats med en nyckel kryptering nyckel (KEK) som kan vara en symmetrisk nyckel eller ett asymmetriskt nyckelpar. Du kan hantera lokalt eller lagra den i Azure Key Vault. hello krypterade data har sedan överföra tooAzure Storage-tjänst.

Mer om kryptering på klientsidan med Azure Key Vault toolearn och kom igång med hur tooinstructions, se dokumentet hello [Självstudier: kryptera och dekryptera blobbar i Microsoft Azure Storage med hjälp av Azure Key Vault](../storage/storage-encrypt-decrypt-blobs-key-vault.md)

Slutligen kan du också använda hello Azure Storage-klientbibliotek för Java tooperform klientsidan kryptering innan du laddar upp data tooAzure lagring och toodecrypt hello data när du hämtar den toohello klienten. Det här biblioteket stöder även integrering med [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) storage-konto för hantering av nycklar.

### <a name="encryption-of-data-at-rest-with-azure-sql-database"></a>Kryptering av data i vila med Azure SQL-databas

[Azure SQL Database](../sql-database/sql-database-technical-overview.md) är en generell relationsdatabastjänst i Microsoft Azure som har stöd för strukturer som relationsdata, JSON, spatial, och XML. Azure SQL stöder både kryptering för serversidan via hello Transparent Data kryptering (TDE)-funktionen och klientsidan kryptering via hello Always Encrypted-funktionen.

#### <a name="transparent-data-encryption"></a>Transparent datakryptering

[TDE Transparent datakryptering](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) är används tooencrypt [SQL Server](https://www.microsoft.com/sql-server/sql-server-2016), [Azure SQL Database](../sql-database/sql-database-technical-overview.md), och [Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) datafiler i realtid med hjälp av en databaskrypteringsnyckel (DEK), som lagras i hello Start databaspost för tillgänglighet under återställningen.

TDE skyddar data och loggfiler med hjälp av AES och 3DES krypteringsalgoritmer. Kryptering av hello databasfilen utförs på hello sidan nivå. hello sidor i en krypterad databas måste krypteras innan de skrivs toodisk och dekrypteras när de har lästs in i minnet. TDE är nu aktiverad som standard för nya Azure SQL-databaser.

#### <a name="always-encrypted"></a>Alltid krypterad

Hej [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) funktion i Azure SQL gör du tooencrypt data i klienten program tidigare toostoring i Azure SQL Database och att du tooenable delegering av lokal databas administration toothird parter och underhålla åtskillnad mellan dem som äger och kan se hello data och de som hanterar den men inte ska ha åtkomst tooit.

#### <a name="cellcolumn-level-encryption"></a>Kryptering på cellen/kolumn

Azure SQL-databas kan du tooapply symmetrisk kryptering tooa kolumn data med hjälp av Transact-SQL. Detta kallas [cell kryptering eller kryptering på kolumnen](https://docs.microsoft.com/sql/relational-databases/security/encryption/encrypt-a-column-of-data) (r), eftersom du kan använda den tooencrypt specifika kolumner eller med specifika dataceller med olika krypteringsnycklar. Det ger mer detaljerade krypteringsfunktioner än TDE som krypterar data i sidor.

Radera har inbyggda funktioner som du kan använda tooencrypt data med hjälp av antingen symmetriskt eller asymmetriskt nycklar, hello offentliga nyckeln för ett certifikat eller med en lösenfras med hjälp av 3DES.

### <a name="cosmos-db-database-encryption"></a>Databaskryptering cosmos DB

[Azure Cosmos-DB](../cosmos-db/database-encryption-at-rest.md) är Microsofts globalt distribuerade och flera olika modeller databas. Data som lagras i Cosmos-DB i beständigt minne (solid-state-hårddiskar) som är krypterad som standard. Det finns inga kontroller tooturn eller inaktiverar. Kryptering i vila implementeras med hjälp av ett antal säkerhetstekniker, inklusive system för säker lagring av nycklar, krypterade nätverk och kryptografiska API: er. Krypteringsnycklar hanteras av Microsoft och roteras per Microsofts interna riktlinjer.

### <a name="at-rest-encryption-in-azure-data-lake"></a>Kryptering i vila i Azure Data Lake

[Azure Data Lake](../data-lake-store/data-lake-store-encryption.md) är en företagsomfattande lagringsplats för varje typ av data samlas in i en enda plats tidigare tooany formell definition av krav eller schema. Har stöd för Azure Data Lake Store ”på som standard” transparent kryptering av data i vila, som ställs in under hello skapandet av ditt konto. Som standard hanterar Data Lake Store hello nycklar för dig, men du måste hello alternativet toomanage dem själv.

Tre typer av nycklar som används i kryptera och dekryptera data: hello Master kryptering nyckel (MEK), Datakrypteringsnyckeln (DEK) och blockera kryptering nyckel (BEK). hello MEK används tooencrypt hello DEK som lagras på beständiga media och hello BEK härleds från hello DEK och hello datablock. Du kan rotera hello MEK om du hanterar dina egna nycklar.

## <a name="encryption-of-data-in-transit"></a>Kryptering av data under överföring

Azure erbjuder många metoder för att behålla data privata som flyttas från en plats tooanother.

### <a name="tlsssl-encryption-in-azure"></a>TLS/SSL-kryptering i Azure

Microsoft använder hello [Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) protokollet tooprotect data när reser mellan hello molntjänster och kunder. Microsofts datacenter förhandla om en TLS-anslutning med klientdatorer som ansluter tooAzure tjänster. TLS innehåller stark autentisering, meddelandesekretess, och integritet (aktivera identifiering av meddelandet manipulation avlyssning och förfalskning), samverkan, flexibla algoritmer, enkel distribution och användning.

[Perfekt Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) (Secrecy) är skyddar anslutningar mellan kunders klientsystem och Microsofts molntjänster med unika nycklar. Anslutningar använder även RSA-baserade 2 048-bitarskryptering nyckellängder. Den här kombinationen gör det svårt för toointercept och komma åt data som är under överföring.

### <a name="azure-storage-transactions"></a>Azure Storage-transaktioner

När du kommunicerar med Azure Storage via hello Azure-portalen för alla transaktioner äga rum via HTTPS. Du kan också använda hello Storage REST API över HTTPS toointeract med Azure Storage. Du kan tillämpa hello HTTPS används när du anropar hello REST API: er tooaccess objekt i storage-konton genom att aktivera säker överföring krävs för hello storage-konto.

Signaturer för delad åtkomst ([SAS](../storage/storage-dotnet-shared-access-signature-part-1.md)), som kan vara används toodelegate komma åt tooAzure lagringsobjekt får innehålla en alternativet toospecify som endast hello HTTPS-protokollet kan användas när du använder signaturer för delad åtkomst. Detta säkerställer att vem som helst skicka länkar med SAS-token använder hello rätt protokoll.

[SMB 3.0](https://technet.microsoft.com/library/dn551363(v=ws.11).aspx#BKMK_SMBEncryption) används tooaccess Azure-filresurser stöder kryptering, och det är tillgängligt i Windows Server 2012 R2, Windows 8, Windows 8.1 och Windows 10, cross-region åtkomst och även komma åt hello skrivbordet.

Klientsidans kryptering krypteras hello data innan den skickade tooAzure lagring, så att den krypteras över hello nätverk.

### <a name="smb-encryption-over-azure-virtual-networks"></a>SMB-kryptering över virtuella Azure-nätverk 

[SMB 3.0](https://support.microsoft.com/help/2709568/new-smb-3-0-features-in-the-windows-server-2012-file-server) i Azure virtuella datorer som kör Windows Server 2012 och senare ger du hello möjlighet toomake data överföringar säker genom att kryptera data under överföring via Azure Virtual Networks tooprotect mot manipulation och avlyssning attacker. Administratörer kan aktivera SMB-kryptering för hello hela servern eller bara vissa resurser.

Som standard när SMB-kryptering är aktiverat för en resurs eller en server, är SMB-3-klienter tillåtna tooaccess hello krypterade resurser.

## <a name="in-transit-encryption-in-azure-virtual-machines"></a>Kryptering under överföring i virtuella Azure-datorer

Krypteras data under överföringen till, från och mellan virtuella Azure-datorer med Windows på flera olika sätt beroende på hello uppbyggnad hello anslutning.

### <a name="rdp-sessions"></a>RDP-sessioner

Du kan ansluta och logga in tooan Azure VM som använder hello [Remote Desktop Protocol](https://msdn.microsoft.com/library/aa383015(v=vs.85).aspx) (RDP) från en Windows-klientdator eller från en Mac med RDP-klienten installerad. Du kan skydda data under överföring över hello nätverk i RDP-sessioner av TLS.

Du kan också använda Fjärrskrivbord tooconnect tooa Linux VM i Azure.

### <a name="secure-access-toolinux-vms-with-ssh"></a>Säker åtkomst tooLinux virtuella datorer med SSH

Du kan använda [Secure Shell](../virtual-machines/linux/ssh-from-windows.md) (SSH) tooconnect tooLinux virtuella datorer körs i Azure för fjärrhantering. SSH är en krypterad anslutningsprotokoll som tillåter säker inloggning via oskyddat anslutningar. Det är hello anslutning standardprotokollet för virtuella Linux-datorer finns i Azure. Ta bort hello behovet av att lösenord toolog i med hjälp av SSH-nycklar för autentisering. SSH använder en offentlig/privat nyckelpar (asymmetrisk kryptering) för autentisering.

## <a name="azure-vpn-encryption"></a>Azure VPN-kryptering

Du kan ansluta tooAzure via ett virtuellt privat nätverk som skapar en säker tunnel tooprotect hello sekretessen för hello data som skickas över hello nätverk.

### <a name="azure-vpn-gateway"></a>Azure VPN Gateway

[Azure VPN-gateway](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md) kan vara används toosend krypterad trafik mellan det virtuella nätverket och din lokala plats via en offentlig eller toosend trafik mellan virtuella nätverk.

Plats-till-plats VPN använder [IPsec](https://en.wikipedia.org/wiki/IPsec) för transport-kryptering. Azure VPN-gatewayer Använd standard förslag. Du kan konfigurera Azure VPN-gatewayer toouse en anpassad IPsec/IKE-princip med särskilda kryptografiska algoritmer och viktiga styrkor i stället hello Azure standard principen anger.

### <a name="point-to-site-vpn"></a>Punkt-till-plats VPN

Punkt-till-plats VPN att enskilda klienter åtkomst till tooan Azure Virtual Network. [hello Secure Socket Tunneling Protocol](https://technet.microsoft.com/library/2007.06.cableguy.aspx) (SSTP) är används toocreate hello VPN-tunnel och kan bläddra brandväggar (hello tunnel visas som en HTTPS-anslutning). Du kan använda din egen interna PKI rotcertifikatutfärdaren för punkt-till-plats-anslutning.

Du kan konfigurera en punkt-till-plats VPN-anslutningen tooa virtuellt nätverk med hello Azure-portalen med certifikatautentisering eller PowerShell.

toolearn mer om punkt-till-plats VPN-anslutningar tooAzure Vnet, se: [konfigurerar en punkt-till-plats-anslutning tooa VNet med hjälp av certifikatutfärdare autentisering: Azure-portalen](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md) och

[Konfigurera en punkt-till-plats-anslutning tooa VNet med hjälp av autentisering med datorcertifikat: PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="site-to-site-vpn"></a>Plats-till-plats VPN 

En plats-till-plats VPN-gateway-anslutningen har använt tooconnect ditt lokala nätverk tooan virtuella Azure-nätverket via en IPsec/IKE (IKEv1 eller IKEv2) VPN-tunnel. Den här typen av anslutning kräver en VPN-enhet som finns lokalt som har ett externt Internetriktade offentliga IP-adress som tilldelats tooit.

Du kan konfigurera en plats-till-plats VPN-anslutningen tooa virtuellt nätverk med hello Azure-portalen, PowerShell eller hello Azure-kommandoradsgränssnittet (CLI).

Läs igenom det här för mer information:

[Skapa en plats-till-plats-anslutning i hello Azure-portalen](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

[Skapa en plats-till-plats-anslutning](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

[Skapa ett virtuellt nätverk med en plats-till-plats VPN-anslutning med hjälp av CLI](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="in-transit-encryption-in-azure-data-lake"></a>Kryptering under överföring i Azure Data Lake

Data under överföring (även kallat data i rörelse) krypteras också alltid i Data Lake Store. Dessutom tooencrypting data tidigare toostoring toopersistent media hello data alltid skyddas under överföringen med hjälp av HTTPS. HTTPS är hello enda protokoll som stöds för hello Data Lake Store REST-gränssnitt.

toolearn mer om kryptering av data under överföring i Azure Data Lake finns hello dokumentet [kryptering av data i Azure Data Lake Store.](../data-lake-store/data-lake-store-encryption.md)

## <a name="key-management-with-azure-key-vault"></a>Hantering av nycklar med Azure Key Vault

Utan tillräckligt skydd och hantering av nycklar hello återges kryptering oanvändbar. Azure Key Vault är Microsofts rekommenderade lösningen för att hantera och kontrollera åtkomst tooencryption nycklar som används av molntjänster. Behörigheter tooaccess nycklar kan tilldelas tooservices eller toousers via Azure Active Directory-konton.

Azure Key Vault besparar organisationer av hello måste tooconfigure korrigera och underhålla Maskinvarusäkerhetsmoduler (HSM) och programvara för hantering av nycklar. Med Azure Key Vault Microsoft ser aldrig dina nycklar och program har inte direkt åtkomst toothem; du behåller kontrollen. Du kan också importera eller generera nycklar i HSM.

## <a name="next-steps"></a>Nästa steg

- [Översikt över Azure-säkerhet](security-get-started-overview.md)
- [Översikt över säkerheten i Azure-nätverk](security-network-overview.md)
- [Översikt över säkerheten i Azure-databas](azure-database-security-overview.md)
- [Översikt över säkerheten i virtuella Azure-datorer](security-virtual-machines-overview.md)
- [Datakryptering i vila](azure-security-encryption-atrest.md)
- [Metodtips för datasäkerhet och kryptering](azure-security-data-encryption-best-practices.md)
