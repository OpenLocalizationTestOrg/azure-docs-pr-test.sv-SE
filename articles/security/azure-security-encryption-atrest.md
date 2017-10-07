---
title: aaaMicrosoft Azure Data kryptering i vila | Microsoft Docs
description: "Den här artikeln innehåller en översikt över Microsoft Azure kryptering av data i vila, hello övergripande funktioner och allmänna överväganden."
services: security
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: TomSh
ms.assetid: 9dcb190e-e534-4787-bf82-8ce73bf47dba
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: yurid
ms.openlocfilehash: d74d103e4fd7585543b4f039877af7d742a6b2e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-encryption-at-rest"></a>Azure Data kryptering i vila
Det finns flera verktyg i Microsoft Azure toosafeguard data enligt tooyour företags behov för säkerhet och efterlevnad. Det här dokumentet fokuserar på hur data skyddas i vila i Microsoft Azure, beskrivs hello olika komponenter som deltar i hello data protection implementering och granskar- och nackdelar med hello olika nyckelhantering skydd metoder. 

Kryptering i vila är vanliga säkerhetskrav. En fördel med Microsoft Azure är att organisationer kan uppnå kryptering i vila utan hello kostnaden för implementering och hantering och hello risken för en anpassad nyckel lösning. Organisationer har hello alternativet för att låta Azure helt hantera kryptering i vila. Dessutom kan har organisationer olika alternativ tooclosely hantera kryptering eller krypteringsnycklar.

## <a name="what-is-encryption-at-rest"></a>Vad är kryptering i vila?
Kryptering i vila refererar toohello kryptografiska kodning (kryptering) av data om det sparas. hello kryptering i vila Designer i Azure använder symmetrisk kryptering tooencrypt och dekryptera stora mängder data snabbt enligt tooa enkel konceptuella modellen:

- En symmetrisk krypteringsnyckel är används tooencrypt data som den sparas 
- Hej samma krypteringsnyckel är används toodecrypt data som den färdigställs för användning i minnet
- Data kan vara partitionerade och olika nycklar kan användas för varje partition
- Nycklar måste lagras på en säker plats med principer för åtkomstkontroll begränsa åtkomst toocertain identiteter och loggning nyckelanvändning. Datakrypteringsnycklar ofta är krypterade med asymmetrisk kryptering toofurther begränsa åtkomsten (beskrivs i hello *nyckel hierarkin*senare i den här artikeln)

hello ovan beskriver hello gemensamma övergripande element kryptering i vila. I praktiken kräver viktiga scenarier för hantering och kontroll, samt tillgänglighet och skala garantier ytterligare konstruktioner. Microsoft Azure-kryptering i Rest-begrepp och komponenter som beskrivs nedan.

## <a name="hello-purpose-of-encryption-at-rest"></a>hello syftet med kryptering i vila
Kryptering i vila är avsedda tooprovide dataskydd för data i vila (enligt beskrivningen ovan.) Attacker mot data i vila omfattar försök tooobtain fysisk åtkomst toohello maskinvara på vilka hello data lagras och sedan röjande hello innehåller data. I sådana angrepp kan hårddisken för en server ha har mishandled under underhåll så att en angripare tooremove hello-hårddisk. Senare hello angriparen placerar hello hårddisken till en dator under deras tooattempt tooaccess hello kontrolldata. 

Kryptering i vila är utformad tooprevent hello angripare från att komma åt hello okrypterade data genom att säkerställa hello krypteras data på disken. Om angripare tooobtain en hårddisk med sådana krypterade data och krypteringsnycklar ingen åtkomst toohello, hello angripare äventyras inte hello data utan bra svårt. I ett sådant scenario angriparen tooattempt attacker mot krypterade data som är mycket mer komplexa och resursen förbrukar än åtkomst till okrypterade data på en hårddisk. Därför kryptering i vila rekommenderas och är ett krav för hög prioritet för många organisationer. 

I vissa fall krävs också kryptering i vila av en organisations behov av åtgärder för styrning och efterlevnad av data. Branschmässiga och statliga förordningar som HIPAA, PCI och FedRAMP och internationella regelkrav utforma skyddsåtgärder via processer och principer om kraven för skydd och kryptering. För många av dessa förordningar är kryptering i vila en obligatorisk åtgärd som krävs för kompatibla hantering och skydd. 

Dessutom toocompliance och regelkrav kryptering i vila vara uppfattas som en funktion för skydd på djupet plattform. Microsoft ger en kompatibel plattform för tjänster, program och data, omfattande anläggning och fysisk säkerhet för dataåtkomst kontroll och granskning, men det är viktigt tooprovide ytterligare ”överlappande” säkerhetsåtgärder om något av hello andra säkerhetsmetoder misslyckas. Kryptering i vila innehåller sådana en ytterligare skydd.

Microsoft är allokerat tooproviding kryptering i vila alternativ för molntjänster och tooprovide kunder lämplig hanterbarhet krypteringsnycklar och åtkomst toologs visar när krypteringsnycklar används. Dessutom samarbetar Microsoft mot hello mål för att göra alla kunddata krypterat i vila som standard.

## <a name="azure-encryption-at-rest-components"></a>Azure kryptering i vila komponenter

Som tidigare nämnts är hello målet för kryptering i vila att data som sparas på disk är krypterad med en kryptering med hemliga nyckel. tooachieve att målet skydda nyckelgenerering, lagring, åtkomstkontroll och hantering av hello kryptering nycklar måste anges. Även om information kan variera, kan Azure services kryptering på Rest-implementeringar beskrivas vad gäller hello nedan begrepp som sedan illustreras i följande diagram hello.

![Komponenter](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig1.png)

### <a name="azure-key-vault"></a>Azure Key Vault

hello lagringsplatsen hello kryptering och access control toothose nycklar är central tooan kryptering i vila modellen. hello nycklar måste toobe hög skyddas men hanteras av angivna användarna och tillgängliga toospecific tjänster. För Azure-tjänster är Azure Key Vault hello rekommenderas viktiga lagringslösning och ger en gemensam hanteringsupplevelse för tjänster. Nycklar lagras och hanteras i nyckelvalv och kan få åtkomst till tooa nyckelvalv toousers eller tjänster. Azure Key Vault stöder kunden skapandet av nycklar eller import av Kundnycklar för användning i kundhanterad kryptering viktiga scenarier.

### <a name="azure-active-directory"></a>Azure Active Directory

Behörigheter toouse hello nycklar lagras i Azure Key Vault toomanage eller tooaccess dem för kryptering i vila kryptering och dekryptering, kan få tooAzure Active Directory-konton. 

### <a name="key-hierarchy"></a>Viktiga hierarki

Vanligtvis mer än en krypteringsnyckel används i en kryptering i vila implementering. Asymmetrisk kryptering är användbart för att upprätta förtroende hello och autentisering som behövs för åtkomst och hantering. Symmetrisk kryptering är effektivare för bulk-kryptering och dekryptering, tillåter starkare kryptering och bättre prestanda. Dessutom kan begränsa hello användning av en enda kryptering viktiga minskar hello risk som hello nyckel ska äventyras och hello kostnaden för omkryptering när en nyckel måste ersättas. tooleverage hello fördelarna med asymmetriska och symmetrisk kryptering och begränsa hello användning och exponering av en enskild nyckel, använder en nyckel hierarki består av följande typer av nycklar hello Azure kryptering i vila modeller:

- **Datakrypteringsnyckeln (DEK)** – en symmetrisk nyckel AES256 används tooencrypt en partition eller datablock.  En enskild resurs kan ha många partitioner och många datakrypteringsnycklar. Kryptera varje datablock med en annan nyckel gör kryptografi analys attacker svårare. Åtkomst tooDEKs krävs av hello resource provider eller program-instans som kryptering och dekryptering av ett visst block. När en DEK ersätts med en ny nyckel måste endast hello data i dess associerade block krypteras igen med hello ny nyckel.
- **Nyckeln kryptering nyckel (KEK)** – en asymmetrisk krypteringsnyckel används tooencrypt hello datakrypteringsnycklar. Med en nyckel krypteringsnyckel kan hello datakrypteringsnycklar själva toobe krypterade och kontrolleras. hello entitet som har åtkomst toohello KEK är annorlunda än hello entitet som kräver hello DEK. På så sätt kan en entitet toobroker åtkomst toohello DEK för hello syftet med begränsad åtkomst för varje DEK toospecific partition. Eftersom hello KEK krävs toodecrypt hello DEKs är hello KEK en enda åtkomstpunkt som DEKs kan raderas effektivt genom borttagning av hello KEK.

hello datakrypteringsnycklar, krypteras med hello nyckeln krypteringsnycklarna lagras separat och endast en entitet med åtkomst toohello kryptering av nyckeln får alla datakrypteringsnycklar krypteras med nyckeln. Olika modeller för lagring av nycklar stöds. Diskuteras varje modell i detalj senare i hello nästa avsnitt.

## <a name="data-encryption-models"></a>Kryptering datamodeller

Förstå hello olika kryptering modeller och deras för- och nackdelar är viktigt för att förstå hur hello olika resursproviders i Azure implementera kryptering i vila. Dessa definitioner är gemensamma för alla providrar i Azure tooensure gemensamt språk och taxonomi. 

Det finns tre scenarier för kryptering på serversidan:

- Kryptering för serversidan med tjänsten hanterade nycklar
    - Azure Resursproviders operationer hello kryptering och dekryptering
    - Microsoft hanterar hello nycklar
    - Funktioner för fullständig moln

- Kryptering för serversidan med kundhanterad nycklar i Azure Key Vault
    - Azure Resursproviders operationer hello kryptering och dekryptering
    - Kunden styr nycklar via Azure Key Vault
    - Funktioner för fullständig moln

- Serversidan kryptering med kundhanterad nycklar på kundens kontrollerade maskinvara
    - Azure Resursproviders operationer hello kryptering och dekryptering
    - Kunden kontroller nycklar på kunden kontrollerade maskinvara
    - Funktioner för fullständig moln

Tänk hello följande för klientsidan kryptering:

- Azure-tjänster inte kan se dekrypterade data
- Kunder hantera och lagra nycklar på lokalt (eller i andra säkra lagrar). Nycklarna är inte tillgängliga tooAzure tjänster
- Minskad molnet funktioner

hello stöds kryptering modeller i Azure som delas upp i två grupper: ”klienten kryptering” och ”serversidan kryptering” som tidigare nämnts. Observera att, oberoende av hello kryptering i vila modellen används, Azure services rekommenderar alltid hello användning av en säker transport, till exempel TLS- eller HTTPS. Därför bör åtgärdas av hello transportprotokollet kryptering i transport och får inte vara en viktig faktor för att fastställa vilka kryptering i vila modellen toouse.

### <a name="client-encryption-model"></a>Modell med klientdator kryptering

Modell med klientdator kryptering refererar tooencryption som utförs utanför hello Resource Provider eller Azure av hello tjänsten eller anropande programmet. hello kryptering kan utföras av hello-tjänstprogram i Azure eller av ett program som körs i hello kunden datacenter. I båda fallen när möjligheterna med den här modellen för kryptering, hello Azure Resource Provider tar emot en krypterad blob av data utan hello möjlighet toodecrypt hello data på något sätt, eller ha åtkomst toohello krypteringsnycklar. I den här modellen hello nyckelhantering utförs av hello anropar/tjänstprogram och är helt ogenomskinlig toohello Azure-tjänsten.

![Client](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig2.png)

### <a name="server-side-encryption-model"></a>Kryptering för serversidan modellen

Serversidan kryptering modeller finns tooencryption som utförs av hello Azure-tjänsten. I den här modellen hello Resursprovidern utför hello kryptera och dekryptera åtgärder. Azure Storage kan ta emot data i klartext åtgärder och utför hello kryptering och dekryptering internt. Hej Resursprovidern kan använda krypteringsnycklar som hanteras av Microsoft eller hello kunden beroende på hello angivna konfigurationen. 

![Server](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig3.png)

### <a name="server-side-encryption-key-management-models"></a>Kryptering för serversidan nyckelhantering modeller

Var och en av hello serversidan kryptering i vila modeller innebär olika egenskaper för hantering av nycklar. Här ingår varifrån och hur krypteringsnycklar skapas och lagras som samt hello Åtkomstmodeller och hello viktiga rotation procedurer. 

#### <a name="server-side-encryption-using-service-managed-keys"></a>Kryptering för serversidan via tjänsten hanterade nycklar

För många kunder är hello grundläggande krav tooensure som hello data krypteras när det är i vila. Serversidan kryptering med nycklar för hanteras av tjänsten kan den här modellen genom så att kunderna toomark hello specifik resurs (Storage-konto, SQL DB o.s.v.) för kryptering och låta alla nyckelhantering aspekter som nyckel utfärdande, rotation och säkerhetskopiering tooMicrosoft. De flesta Azure-tjänster som stöder kryptering i vila vanligtvis stöd för den här modellen för att avlasta hello hantering av hello kryptering nycklar tooAzure. hello Azure-resursprovider skapar hello nycklar, placerar dem i säker lagring och hämtar dem när de behövs. Detta innebär att hello-tjänsten har fullständig åtkomst toohello nycklar och hello-tjänsten har full kontroll över hello Livscykelhantering för autentiseringsuppgifter.

![Hanteras](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig4.png)

Serversidan kryptering med för tjänsten hanteras därför snabbt adresser hello måste toohave kryptering i vila med låg övergripande toohello kunden. När det är tillgängligt öppnas hello Azure-portalen för hello målprenumerationen och resursprovidern vanligtvis en kund och kontrollerar en som anger de vill ha hello data toobe krypteras. I vissa resurshanterare serversidan kryptering med tjänsten hanterade nycklar är aktiverad som standard. 

Kryptering för serversidan med Microsoft-hanterad nycklar innebär hello-tjänsten har fullständig åtkomst toostore och hanterar hello nycklar. Även om vissa kunder vill kanske toomanage hello nycklar eftersom de anser att de kan säkerställa bättre säkerhet, bör hello kostnader och risker som är associerade med en anpassad nyckel lagringslösning övervägas vid utvärdering av den här modellen. I många fall kan en organisation bestämma att resursen begränsningar eller riskerna med en lokal lösning kan större än hello risken för molnhantering av hello kryptering i vila nycklar.  Men den här modellen kan inte vara tillräcklig för organisationer som har krav toocontrol hello skapas eller livscykeln för hello krypteringsnycklar eller toohave olika personer Hantera tjänstens krypteringsnycklar än de som hanterar hello-tjänsten (t.ex. uppdelning av nyckelhantering från hello övergripande Hanteringsmodellen för hello-tjänsten).

##### <a name="key-access"></a>Nyckelåtkomst

När SSI-kryptering med tjänsten hanterade nycklar används hello nyckelgenerering, lagring och tjänsten åtkomst till hanteras av hello-tjänsten. Normalt lagrar hello grundläggande Azure resursproviders hello datakrypteringsnycklar i ett arkiv som är Stäng toohello data och snabbt tillgänglig och kan nås när hello nyckeln krypteringsnycklarna lagras i en säker intern lagring.

**Fördelar**

- Enkel installation
- Microsoft hanterar viktiga rotation, säkerhetskopiering och redundans
- Kunden har hello kostnaden för implementering eller hello risken för ett schema för anpassade nyckelhantering.

**Nackdelar**

- Ingen kund kontroll över hello krypteringsnycklar (nyckelspecifikation, livscykel, återkallade, etc.)
- Ingen möjlighet toosegregate nyckelhantering från övergripande Hanteringsmodellen för hello-tjänsten

#### <a name="server-side-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Kryptering för serversidan med hjälp av kunden hanterade nycklar i Azure Key Vault 

Med Customer hanterade nycklar i Key Vault för scenarier där hello kravet är tooencrypt hello data i vila och kontroll hello kryptering nycklar kunder kan använda kryptering för serversidan. Vissa tjänster kan lagra endast hello rot nyckeln krypteringsnyckeln i Azure Key Vault och store hello krypterade Datakrypteringsnyckeln i en intern närmare toohello platsdata. I att scenariot kunder kan ta sina egna nycklar tooKey valvet (BYOK – ta med din egen nyckel), eller skapa nya, och använder dem tooencrypt hello önskade resurser. Medan hello Resursprovidern utför hello kryptering och dekryptering åtgärder använder hello konfigurerade nyckeln som hello rotnyckeln för alla krypteringsåtgärder för. 

##### <a name="key-access"></a>Nyckelåtkomst

hello omfattar kryptering för serversidan modellen med hanterade Kundnycklar i Azure Key Vault hello tjänsten använder hello nycklar tooencrypt och dekryptera efter behov. Kryptering i vila nycklar görs tillgänglig tooa tjänsten via en princip för åtkomstkontroll beviljar tjänsten identitet åtkomst tooreceive hello nyckeln. Kör ett kopplat prenumerations en Azure-tjänst kan konfigureras med en identitet för tjänsten inom den prenumerationen. hello-tjänsten kan utföra Azure Active Directory-autentisering och ta emot en token för autentisering som identifierar sig själv som den tjänst som agerar på uppdrag av hello prenumeration. Denna token kan sedan presenterades tooKey valvet tooobtain en nyckel att den har fått åtkomst till.

För åtgärder med hjälp av krypteringsnycklar, en tjänstidentitet kan beviljas åtkomst tooany av hello följande åtgärder: dekryptera, kryptera, unwrapKey, wrapKey, verifiera, loggar, hämta, visa, uppdatera, skapa, importera, ta bort, säkerhetskopiera och återställa.

tooobtain en nyckel för användning i för att kryptera eller dekryptera data i vila hello tjänstidentiteten som hello Resource Manager service-instansen kommer att köras som måste ha UnwrapKey (tooget hello-nyckel för dekryptering) och WrapKey (tooinsert en nyckel i nyckelvalvet när du skapar en ny nyckel).


>[!NOTE] 
>Mer information om Key Vault finns auktorisering hello secure nyckelvalv sidan i hello [Azure Key Vault dokumentationen](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault). 

**Fördelar**

- Fullständig kontroll över hello nycklar används – kryptering nycklar hanteras i hello kundens Key Vault under hello kundens kontroll.
- Möjlighet tooencrypt flera tjänster tooone master
- Kan särskilja nyckelhantering från övergripande Hanteringsmodellen för hello-tjänsten
- Definiera tjänsten och nyckelplats över regioner

**Nackdelar**

- Kunden har fullt ansvar för hantering av nycklar
- Kunden har fullt ansvar för hantering av livscykel
- Ytterligare kostnader för installation och konfiguration

#### <a name="server-side-encryption-using-service-managed-keys-in-customer-controlled-hardware"></a>Kryptering för serversidan via tjänsten hanterade nycklar i kundens kontrollerade maskinvara

För scenarier där hello har tooencrypt hello data i vila och hantera hello nycklar i en skyddad databas utanför Microsofts kontroll, aktivera vissa Azure-tjänster hello värden din egen nyckel HYOK-nyckelhantering modellen. I den här modellen hello-tjänsten måste hämta hello nyckel på en extern webbplats och därför garantier för tillgänglighet och prestanda påverkas och konfigurationen är mer komplexa. Dessutom eftersom hello-tjänsten har åtkomst toohello DEK under hello kryptering och dekryptering operations hello övergripande säkerhetsgarantier med den här modellen är liknande toowhen hello nycklar är kund hanteras i Azure Key Vault.  Den här modellen är därför inte lämplig för de flesta organisationer om de inte har särskilda nyckelhantering krav vilket kräver den. På grund av begränsningar i toothese stöder de flesta Azure Services inte kryptering för serversidan med hjälp av server-hanterade nycklar i kundens kontrollerade maskinvara.

##### <a name="key-access"></a>Nyckelåtkomst

När servern kryptering med för tjänsten som hanteras i kundens kontrollerade maskinvara används bevaras hello nycklar på ett system som konfigurerats av hello kunden. Azure-tjänster som stöder den här modellen ger ett sätt att upprätta en säker anslutning tooa kund angivna KeyStore.

**Fördelar**

- Fullständig kontroll över hello rotnyckeln används – kryptering nycklar som hanteras av en kund som store
- Möjlighet tooencrypt flera tjänster tooone master
- Kan särskilja nyckelhantering från övergripande Hanteringsmodellen för hello-tjänsten
- Definiera tjänsten och nyckelplats över regioner

**Nackdelar**

- Fullt ansvar för nyckellagring, säkerhet, prestanda och tillgänglighet
- Fullt ansvar för hantering av nycklar
- Fullt ansvar för hantering av livscykel
- Betydande installation, konfiguration och löpande underhållskostnaderna
- Ökad beroende på nätverkets tillgänglighet mellan hello kunden datacenter och Azure-datacenter.

## <a name="encryption-at-rest-in-microsoft-cloud-services"></a>Kryptering i vila i Microsoft-molntjänster

Microsoft Cloud services används i alla tre modeller: IaaS, PaaS, SaaS. Nedan finner du exempel på hur de får plats på varje modell:

- Programvarutjänster, enligt tooas programvara som en Server eller SaaS som har program som tillhandahålls av hello molntjänster som Office 365.
- Tjänster-plattformen som kunder utnyttja hello molnet i sina program via hello molnet för saker som lagring, analyser och service bus-funktioner.
- Infrastrukturtjänster eller infrastruktur som en tjänst (IaaS) i vilken kund distribuera operativsystem och program som finns i hello moln och utnyttja eventuellt andra molntjänster.

### <a name="encryption-at-rest-for-saas-customers"></a>Kryptering i vila för SaaS-kunder

Programvara som en tjänst (SaaS)-kunder har vanligtvis kryptering i vila aktiverat eller tillgängliga i varje tjänst. Office 365-tjänster har flera alternativ för kunder tooverify eller aktivera kryptering i vila. Information om Office 365-tjänster finns i Data krypteringstekniker för Office 365.

### <a name="encryption-at-rest-for-paas-customers"></a>Kryptering i vila för PaaS-kunder

Plattform som en tjänst (PaaS) kundinformation finns vanligtvis i en miljö för körning av programmet och alla Azure-Resursproviders används toostore kundens data. toosee hello kryptering i vila alternativ tillgängliga tooyou granska hello tabellen nedan för hello lagrings- och plattformar som du använder. Där de stöds, finns länkar tooinstructions om hur du aktiverar kryptering i vila för varje resursprovider. 

### <a name="encryption-at-rest-for-iaas-customers"></a>Kryptering i vila för IaaS-kunder

Infrastruktur som en tjänst (IaaS)-kunder kan ha flera olika tjänster och program används. IaaS-tjänster kan aktivera kryptering i vila i sina Azure värd för virtuella datorer och virtuella hårddiskar med hjälp av Azure Disk Encryption. 

#### <a name="encrypted-storage"></a>Krypterade lagring

Som PaaS, kan IaaS-lösningar utnyttja andra Azure-tjänster som lagrar data krypterat i vila. I dessa fall kan aktivera du hello kryptering på Rest-stöd som tillhandahålls av varje förbrukade Azure-tjänsten. hello tabellen nedan räknar hello större lagring, tjänster och program plattformar och hello modell för kryptering i vila som stöds. Där de stöds, finns länkar tooinstructions om hur du aktiverar kryptering i vila. 

#### <a name="encrypted-compute"></a>Krypterade beräkning

En fullständig kryptering i vila lösning kräver att hello data sparas aldrig i okrypterat format. Som används, på en server läser in hello data i minnet, data kan sparas lokalt på olika sätt, inklusive växlingsfil för Windows hello, en krasch-dump och eventuella loggning hello kan program utföra. den här informationen är krypterad tooensure vilande IaaS-program kan använda Azure Disk Encryption på en virtuell Azure IaaS (Windows eller Linux) och virtuell disk. 

#### <a name="custom-encryption-at-rest"></a>Anpassade kryptering i vila

Vi rekommenderar att om möjligt IaaS program utnyttja Azure Disk Encryption och kryptering i vila alternativen i alla förbrukade Azure-tjänster. I vissa fall som oregelbundna krypteringskrav eller Azure-baserad lagring, en utvecklare av ett IaaS-program behöver tooimplement kryptering i vila sig själva. Utvecklare av IaaS-lösningar kan bättre integrera med Azure hantering och kundens förväntningar genom att använda vissa Azure komponenter. Särskilt bör utvecklare använda hello Azure Key Vault service tooprovide säker lagring av nycklar samt ger sina kunder med alternativ för konsekvent nyckelhantering med som mest Azure-plattformstjänster. Anpassade lösningar bör dessutom använda Azure hanteras Tjänsteidentiteter tooenable service konton tooaccess krypteringsnycklar. Information för utvecklare på Azure Key Vault och hanteras Tjänsteidentiteter finns i deras respektive SDK: er.

## <a name="azure-resource-providers-encryption-model-support"></a>Azure-resurs providers modellen krypteringsstöd

Microsoft Azure Services varje stöd för en eller flera av hello kryptering i vila modeller. För vissa tjänster, men kanske en eller flera av hello kryptering modeller inte gäller. Tjänster kan dessutom släpp stöd för dessa scenarier vid olika scheman. Det här avsnittet beskrivs hello kryptering i rest-stöd när hello detta skrivs för varje hello viktiga data för Azure storage-tjänster.

### <a name="azure-disk-encryption"></a>Azure disk encryption

En kund med Azure-infrastrukturen som en tjänst (IaaS)-funktioner kan uppnå kryptering i vila för sina virtuella IaaS-datorer och diskar via Azure Disk Encryption. Mer information om Azure Disk encryption finns hello [dokumentation för Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).

#### <a name="azure-storage"></a>Azure Storage

Azure Blob och filen har stöd för kryptering i vila för serversidan krypterade scenarier samt kundens krypterade data (kryptering på klientsidan).

- Serversidan: kunder som använder Azure blob storage kan aktivera kryptering i vila på varje resurs Azure storage-konto. En gång aktiverad serversidan datakryptering görs transparent toohello program. Se [Azure Storage Service-kryptering för Data i vila](https://docs.microsoft.com/azure/storage/storage-service-encryption) för mer information.
- På klientsidan: kryptering på klientsidan av Azure BLOB stöds. När du använder klientsidan kryptering kunder kryptera hello data och överför hello data som en krypterad blob. Nyckelhantering görs av hello kunden. Se [kryptering på klientsidan och Azure Key Vault för Microsoft Azure Storage](https://docs.microsoft.com/azure/storage/storage-client-side-encryption) för mer information.


#### <a name="sql-azure"></a>SQL Azure

SQL Azure stöder för närvarande kryptering i vila för Microsoft-hanterad tjänstsidan och kryptering på klientsidan scenarier.

Stöd för sever kryptering anges via hello SQL-funktion som kallas Transparent datakryptering. När en SQL Azure-kund kan skapas och hanteras för dem automatiskt TDE nyckel. Kryptering i vila kan aktiveras på hello databas- och programnivå. Från och med juni 2017 [Transparent Data kryptering (TDE)](https://msdn.microsoft.com/library/bb934049.aspx) aktiveras som standard för nya databaser.

Klientsidan kryptering av data i SQL Azure stöds via hello [Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx) funktion. Krypterad använder alltid en nyckel som skapas och lagras av hello-klienten. Kunder kan lagra hello huvudnyckeln i ett certifikatarkiv för Windows Azure Key Vault eller en lokal maskinvarusäkerhetsmodul. Med hjälp av SQL Server Management Studio, SQL-användare välja vilka viktiga de vill ha toouse tooencrypt vilken kolumn.

|                                  |                |                     | **Kryptering modellen**             |                              |        |
|----------------------------------|----------------|---------------------|------------------------------|------------------------------|--------|
|                                  |                |                     |                              |                              | **Klienten** |
|                                  | **Hantering av nycklar** | **Tjänsten hanteras nyckel** | **Kunden hanteras i Nyckelvalvet** | **Kunden hanteras lokala** |        |
| **Lagring och databaser**            |                |                     |                              |                              |        |
| Disken (IaaS)                      |                | -                   | Ja                          | Ja*                         | -      |
| SQLServer (IaaS)                |                | Ja                 | Ja                          | Ja                          | Ja    |
| Azure SQL (PaaS)                 |                | Ja                 | Förhandsversion                      | -                            | Ja    |
| Azure Storage (blockera/Sidblobbar) |                | Ja                 | Förhandsversion                      | -                            | Ja    |
| Azure Storage (filer)            |                | Ja                 | -                            | -                            | -      |
| Azure Storage (tabeller, köer)   |                | -                   | -                            | -                            | Ja    |
| Cosmos DB (dokumentet DB)          |                | Ja                 | -                            | -                            | -      |
| StorSimple                       |                | Ja                 | -                            | -                            | Ja    |
| Säkerhetskopiering                           |                | -                   | -                            | -                            | Ja    |
| **Tillgångsinformation och analys**       |                |                     |                              |                              |        |
| Azure Data Factory               |                | Ja                 | -                            | -                            | -      |
| Azure Machine Learning           |                | -                   | Förhandsversion                      | -                            | -      |
| Azure Stream Analytics           |                | Ja                 | -                            | -                            | -      |
| HDInsights (Azure Blob Storage)  |                | Ja                 | -                            | -                            | -      |
| HDInsights (Data Lake lagring)   |                | Ja                 | -                            | -                            | -      |
| Azure Data Lake Store            |                | Ja                 | Ja                          | -                            | -      |
| Azure Data Catalog               |                | Ja                 | -                            | -                            | -      |
| Power BI                         |                | Ja                 | -                            | -                            | -      |
| **IoT-tjänster**                     |                |                     |                              |                              |        |
| IoT Hub                          |                | -                   | -                            | -                            | Ja    |
| Service Bus                      |                | -              | -                            | -                            | Ja    |
| Händelsehubbar                       |                | -             | -                            | -                            | -      |


## <a name="conclusion"></a>Slutsats

Skydd av kundinformation som lagras i Azure-tjänster är av avgörande betydelse tooMicrosoft. Alla Azure-värdtjänsten tjänster är allokerat tooproviding kryptering på övriga alternativ. Grundläggande tjänster som Azure Storage, SQL Azure och viktiga analytics och intelligence ge redan kryptering Rest-alternativ. Vissa av dessa tjänster kontrollerade Kundnycklar och kryptering på klientsidan som tjänsten hanterade nycklar och kryptering. Microsoft Azure-tjänster brett förbättra kryptering vid Rest tillgänglighet och nya alternativ planeras för förhandsversionen och allmän tillgänglighet hello kommande månaderna.

