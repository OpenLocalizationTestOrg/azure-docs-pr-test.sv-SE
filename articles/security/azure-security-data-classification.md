---
title: "aaaData klassificering för Azure | Microsoft Docs"
description: "Den här artikeln ger en introduktion toohello grunderna i dataklassificering och visar dess värde, särskilt i hello kontexten för cloud computing och använder Microsoft Azure"
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ROBOTS: NOINDEX
redirect_url: https://aka.ms/data-classification-cloud
ms.assetid: 91d4afed-b80f-4a26-b526-b52b9c858cfb
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/18/2017
ms.author: yurid
ms.openlocfilehash: 726da2482beab3bf7b0ac33510f2b523d5074df8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-classification-for-azure"></a>Dataklassificering för Azure
Den här artikeln ger en introduktion toohello grunderna i dataklassificering och visar dess värde, särskilt i hello kontexten för cloud computing och använder Microsoft Azure. 

## <a name="data-classification-fundamentals"></a>Grunderna för klassificering av data
Lyckade dataklassificering i en organisation kräver bred medvetenhet om din organisations behov och goda kunskaper om där dina datatillgångar finns.  

Det finns data i något av tre grundläggande tillstånd: 

* Vilande 
* I processen 
* Under överföring 

Alla tre tillstånd kräver unika tekniska lösningar för dataklassificering men hello tillämpade principer för dataklassificering bör vara hello samma för varje. Data som har klassificerats som konfidentiella måste toostay konfidentiell när den är i vila, i processen och under överföring. 

Data kan också vara strukturerade eller Ostrukturerade. Typiska klassificeringsprocesser för hello strukturerade data hittades i databaser och kalkylblad är mindre komplicerade och tidskrävande toomanage än för Ostrukturerade data, t.ex dokument, källkod och e-post. 

> [!TIP]
> Mer information om funktioner i Azure och bästa praxis för datakryptering [Azure Data kryptering bästa praxis](azure-security-data-encryption-best-practices.md)
> 
> 

I allmänhet organisationer har flera Ostrukturerade data än strukturerade data. Oavsett om data är strukturerade eller Ostrukturerade, är det viktigt att du toomanage data känslighet. Implementeras ordentligt säkerställer dataklassificering känsliga eller konfidentiella data tillgångar hanteras med större tillsyn än datatillgångar som anses vara offentligt eller ledigt toodistribute. 

### <a name="controlling-access-toodata"></a>Kontrollera åtkomst toodata
Autentisering och auktorisering ihop ofta med varandra och deras roller tror många. I verkligheten är de helt annorlunda, enligt följande bild hello.  

![Dataåtkomst och -kontroll](./media/azure-security-data-classification/azure-security-data-classification-fig1.png)

### <a name="authentication"></a>Autentisering
Autentisering består vanligtvis av minst två delar: en användarnamn eller användar-ID tooidentify en användare och en token, till exempel ett lösenord, tooconfirm som hello användarnamn autentiseringsuppgifter är giltiga. hello-processen ger inte hello autentiserade användare med åtkomst tooany objekt eller tjänster. verifierar att hello-användare är den som de säger att de är.   

> [!TIP]
> [Azure Active Directory](../active-directory/active-directory-whatis.md) tillhandahåller molnbaserade identitets-tjänster som du kan tooauthenticate och auktorisera användare. 
> 
> 

### <a name="authorization"></a>Auktorisering
Auktorisering är hello process för att tillhandahålla en autentiserad användare hello möjlighet tooaccess ett program, uppgifter, datafilen eller annat objekt. Tilldela autentiserade användare hello rättigheter toouse, ändra eller ta bort objekt som de kan komma åt kräver uppmärksamhet toodata klassificeringen. 

Auktoriseringen kräver implementering av en mekanism toovalidate enskilda användare måste tooaccess filer och information som baseras på en kombination av rollen, säkerhetsprinciper och risken att tänka på vid. Till exempel data från specifika line-of-business (LOB)-program behöver inte toobe nås av alla anställda och bara en liten del av anställda kommer förmodligen behöver komma åt toohuman resurser (HR) filer. Men för organisationer toocontrol som kan komma åt data som samt när och hur, ett effektivt system för att autentisera användare måste vara på plats. 

> [!TIP]
> Kontrollera att tooleverage rollbaserad åtkomstkontroll (RBAC) toogrant endast hello mängden åtkomst i Microsoft Azure som användare måste tooperform sitt arbete. Läs [använda rollen tilldelningar toomanage åtkomst tooyour Azure Active Directory-resurser](../active-directory/role-based-access-control-configure.md) för mer information. 
> 
> 

### <a name="roles-and-responsibilities-in-cloud-computing"></a>Roller och ansvarsområden i molntjänster
Även om molntjänstleverantörer kan hantera risker, kunder måste tooensure Klassificeringshantering att data och tvingande implementeras ordentligt hello tooprovide rätt nivå av data management services.  

Dataklassificering ansvarsområden varierar baserat på vilken modell för moln är på plats, som visas i följande bild hello. hello tre primära molnet tjänstmodeller är infrastruktur som en tjänst (IaaS), plattform som en tjänst (PaaS) och programvara som en tjänst (SaaS). Implementering av metoder för klassificering av data kan också variera beroende på hello beroendet av och förväntningar hello molntjänstleverantör. 

![Roller](./media/azure-security-data-classification/azure-security-data-classification-fig2.png)

Även om du är ansvarig för att klassificera data bör molntjänstleverantörer göra skriftliga åtaganden om hur de ska skydda och bibehålla sekretessen för hello hello kundinformation som lagras i sina moln.  

* **IaaS-providers** kraven är begränsad tooensuring som hello virtuell miljö kan innehålla funktioner för klassificering och krav på efterlevnad. IaaS-providers har en mindre roll i dataklassificering eftersom de bara behöver tooensure att kundinformation adresser krav på efterlevnad. Leverantörer måste dock fortfarande kontrollera att deras virtuella miljöer adress dataklassificeringskrav bäst i tillägg toosecuring sina datacenter.
* **PaaS-providers** ansvarsområden blandas eftersom hello plattform kan användas i en överlappande tillvägagångssättet tooprovide säkerhet för ett verktyg för klassificering. PaaS-providrar kan ansvara för autentisering och eventuellt vissa auktoriseringsregler och måste ger säkerhet och klassificering funktioner tootheir programmet datalager. Mycket som IaaS-providers måste PaaS-providers tooensure sina-plattformen uppfyller med eventuella krav för klassificering av relevanta data.
* **SaaS-providers** ofta betraktas som en del av en kedja för auktorisering och kommer måste tooensure som hello data som lagras i hello SaaS-program kan styras efter Klassificeringstyp. SaaS-program kan användas för LOB-program och av deras karaktär måste tooprovide hello innebär tooauthenticate och auktorisera data som används och lagras. 

## <a name="classification-process"></a>Av klassificeringsprocessen
Många organisationer som förstå hello måste för dataklassificering och vill tooimplement det står inför ett grundläggande utmaningen: där toobegin?

En effektiv och enkelt sätt tooimplement dataklassificering är toouse hello PLAN, göra, kontrollera, ACT modellen från [MOF](https://technet.microsoft.com/solutionaccelerators/dd320379.aspx). hello följande figur diagram hello uppgifter som är nödvändiga toosuccessfully implementera dataklassificering i den här modellen.  

1. **PLANERA**. Identifiera datatillgångar, ett data skyddar toodeploy hello klassificering program, och utveckla skydd profiler. 
2. **GÖR**. När principer för klassificering av data är överens distribuera programmet hello och implementera-tekniker som behövs för känsliga data.  
3. **KONTROLLERA**. Kontrollera och verifiera tooensure hello verktyg och metoder som används effektivt adressering hello klassificeringsprinciperna rapporter. 
4. **ACT**. Granska hello status för dataåtkomst och granska filer och data som behöver revision med hjälp av en gruppering och revision metoder tooadopt ändringar och tooaddress nya risker.  

![Planera,, kontrollera, fungerar](./media/azure-security-data-classification/azure-security-data-classification-fig3.png)

### <a name="select-a-terminology-model-that-addresses-your-needs"></a>Välj en modell för terminologi som hanterar dina behov
Det finns flera typer av processer för att klassificera data, inklusive manuella processer, platsbaserad processer som klassificerar data baserat på en användares eller systemets plats programbaserade processer, till exempel databasspecifika klassificering och automatisk processer som används av olika teknologier, vilket beskrivs i hello ”skydda känsliga data” senare i den här artikeln.  

Den här artikeln introducerar två generaliserad terminologi modeller som är baserade på väl används och industrin respekteras modeller. Dessa termer modeller som båda har tre nivåer av känslighet för klassificering som visas i hello i den följande tabellen.  

> [!NOTE]
> När klassificera en fil eller en resurs som kombinerar data som vanligtvis skulle klassificeras på olika nivåer, hello högsta nivå för klassificering finns bör fastställa hello övergripande klassificering. Till exempel en fil som innehåller känslig och begränsad information bör klassificeras som begränsade.  
> 
> 

| **Känslighet** | **Terminologi modell 1** | **Terminologi modell 2** |
| --- | --- | --- |
| Hög |Konfidentiellt |Begränsad |
| Medel |Endast för intern användning |Känsliga |
| Låg |Offentligt |Obegränsad |

#### <a name="confidential-restricted"></a>Konfidentiellt (begränsat)
Information som har klassificerats som konfidentiell eller begränsade innehåller data som kan vara allvarligt tooone eller flera personer och/eller organisationer om komprometteras eller tappas bort. Denna information tillhandahålls ofta på grundval av ”måste tooknow” och kan innehålla: 

* Personliga data, inklusive personligt identifierbar information som Social Security eller nationella identifikationsnummer, passport siffror, kreditkortsnummer, drivrutinens licensnummer, medicinska poster och health insurance princip-ID-nummer.  
* Finansiella poster, inklusive finansiella kontonummer, till exempel kontrollerar eller Investeringskontonummer. 
* Företag material, till exempel dokument eller data som är unika eller speciella immateriell egendom.  
* Juridiska data, inklusive eventuella jurist privilegierad material. 
* Autentiseringsdata, inklusive privata kryptografiska nycklar, användarnamn lösenord par eller andra ID-sekvenser, till exempel privata nyckelfilerna biometriska. 

Data som har klassificerats som konfidentiella ofta har regelverk och kompatibilitetskrav för datahantering. 

#### <a name="for-internal-use-only-sensitive"></a>För endast internt bruk (skiftlägeskänsligt)
Information som klassificeras som med medelhög känslighet innehåller filer och data som inte skulle ha en betydande påverkan på en person eller organisation om tappas bort eller förstörs. Sådan information kan innehålla: 

* E-post, de flesta kan tas bort eller distribueras utan att orsaka en kris (exklusive postlådor eller e-post från personer som identifieras i hello konfidentiell klassificering).  
* Dokument och filer som inte innehåller känsliga data.

I allmänhet innehåller klassificeringen sådant som inte är konfidentiella. Denna klassificering kan innehålla de flesta företag data, eftersom de flesta filer som hanteras eller som används för dagliga kan klassificeras som känslig. Med hello undantag av data som publiceras eller är konfidentiell, kan alla data i en företagsorganisation som klassificeras som känslig som standard. 

#### <a name="public-unrestricted"></a>Offentliga (obegränsad)
Information som klassificeras som offentlig innehåller data och filer som inte är kritiska toobusiness behov eller åtgärder. Denna klassificering kan även innehålla data som har avsiktligt utgivna toohello offentliga för deras användning, till exempel marknadsföring material eller tryck på meddelanden. Denna klassificering kan dessutom innehålla data, till exempel skräppost e-postmeddelanden lagras av en e-posttjänst. 

### <a name="define-data-ownership"></a>Definiera dataägarskap
Det är viktigt tooestablish en Rensa fängelsestraff kedja av ägarskapet för alla datatillgångar. hello följande tabell visas olika ägarskap roller i ansträngningar för klassificering av data och deras respektive rättigheter.  

| **Roll** | **Skapa** | **Ändra/ta bort** | **Delegaten** | **Läsa** | **Arkiv/återställning** |
| --- | --- | --- | --- | --- | --- |
| Ägare |X |X |X |X |X |
| Skyddar | | |X | | |
| Administratör | | | | |X |
| Användaren\* | |X | |X | |

**Användare kan beviljas ytterligare rättigheter som redigera och ta bort av skyddar* 

> [!NOTE]
> den här tabellen innehåller inte en fullständig lista över roller och rättigheter, men bara ett representativt urval. 
> 
> 

Hej **data tillgångsägare** är hello ursprungliga skapare av hello data, som kan delegera ägarskap och tilldelar skyddar. När en fil skapas ska hello ägaren vara kan tooassign en klassificering, vilket innebär att de har en ansvar toounderstand vad som måste toobe som klassificerats som konfidentiell baserat på organisationens principer. Alla data som en tillgångsägare för data kan vara automatiskt klassificeras som endast internt bruk (skiftlägeskänsligt) om de inte äger eller skapa konfidentiell (begränsade) datatyper. Hello ägarrollen ändras ofta när hello data klassificeras. Hello ägare kan till exempel skapa en databas sekretessbelagda uppgifter och förlorar sina rättigheter toohello data skyddar.  

> [!NOTE]
> data tillgångens ägare använder ofta en blandning av tjänster, enheter och medier, vilket är personliga och vilket tillhör toohello organisation. Rensa organisationsprincip hjälper dig att se till att användningen av enheter, till exempel bärbara datorer och smarta enheter är i enlighet med riktlinjerna för klassificering av data.  
> 
> 

Hej **data tillgången skyddar** tilldelas av hello tillgångens ägare (eller ombud) toomanage hello tillgången bl.a tooagreements med hello tillgångens ägare eller i enlighet med tillämpliga principkrav. Vi rekommenderar kan hello skyddar roll implementeras på ett automatiserat system. En tillgång skyddar säkerställer att nödvändiga åtkomstkontroller tillhandahålls och ansvarar för att hantera och skydda tillgångar delegerad tootheir försiktighet. hello ansvarsområden hello tillgången skyddar kan vara:  

* Skydda hello tillgången i enlighet med hello tillgångens ägare riktning eller i enlighet med hello tillgångens ägare 
* Se till att klassificeringsprinciperna följs 
* Informera tillgångens ägare av alla ändringar tooagreed-på kontroller och/eller skydd procedurer tidigare toothose ändringarna träder i kraft 
* Reporting toohello tillgångens ägare om ändringar tooor borttagning av hello tillgången skyddar ansvarsområden 
* En **administratör** representerar en användare som ansvarar för att säkerställa att integritet bibehålls, men är inte en data tillgångens ägare, skyddar eller användare. Många administratörsroller utgör i själva verket data behållartjänster utan att ha åtkomst till toohello data. Hej administratör innehåller säkerhetskopiering och återställning av hello data, underhålla poster hello tillgångar och välja, införskaffa och hantera hello enheter och lagring som house hello tillgångar. 
* hello tillgång användare omfattar alla som har beviljats åtkomst toodata eller en fil. Access-tilldelning är ofta delegerade med hello ägare toohello tillgången skyddar.  

### <a name="implementation"></a>Implementering
Hanteringsanmärkningar gäller tooall metoder för klassificering. Dessa förutsättningar måste tooinclude information om vem, vad, var när och varför en datatillgång skulle vara används för åtkomst till, ändras eller tas bort. Alla tillgångshantering måste göras med en förståelse av hur en organisation visar dess risker, men en enkel metod kan användas som definierats i hello klassificering av data. Ytterligare överväganden för dataklassificering omfattar hello införandet av nya program och verktyg och hantera ändringar när en Klassificeringsmetod har implementerats.  

### <a name="reclassification"></a>Gruppering
Gruppera eller ändra hello klassificering tillstånd för en datatillgång måste toobe när användaren eller systemet avgör den hello datatillgång vikten eller riskprofil har ändrats. Den här aktiviteten är viktigt för att säkerställa att hello klassificering status fortsätter toobe aktuella och giltiga. De flesta innehåll som inte har klassificerats manuellt kan klassificeras automatiskt eller baserat på användning av en skyddar data eller dataägare. 

### <a name="manual-data-reclassification"></a>Manuell data gruppering
Den här aktiviteten som vi rekommenderar säkerställer att hello information om ändringen skapas och granskas. hello mest sannolika orsaken till manuell gruppering är på grund av känslighet eller för poster som sparas i dokumentet format eller en krav tooreview data som har ursprungligen felklassificerade. Eftersom det här dokumentet anser dataklassificering och flytta data toohello moln, manuell gruppering ansträngningar kräver uppmärksamhet på grundval av fall och en risk ledningen är perfekt tooaddress klassificeringskraven. I allmänhet skulle sådana arbete överväga hello organisationens policy om vad som måste toobe klassificeras hello klassificering standardläget (alla data och filer som känslig men inte konfidentiell) och ta undantag för hög risk data. 

### <a name="automatic-data-reclassification"></a>Automatisk gruppering
Automatisk gruppering använder hello samma allmänna regel som manuell klassificering. hello undantaget är att automatiserade lösningar kan säkerställa att regler följt och tillämpas efter behov. Dataklassificering kan göras som en del av en tvingande dataklassificeringsprincip, som kan tillämpas när data lagras i användning och under överföring med hjälp av teknik för auktorisering.

* Program-baserade. I vissa program som standard anger en klassificeringsnivån. Till exempel är data från customer relationship management (CRM) programvara, personal och hälsa poster hanteringsverktyg konfidentiell som standard. 
* Plats-baserad. Dataplats kan hjälpa dig att identifiera data känslighet. Data som lagras av en HR eller ekonomiavdelningen är till exempel troligt toobe konfidentiell karaktär.  

### <a name="data-retention-recovery-and-disposal"></a>Datalagring, återställning och avyttring
Återställning av data och avyttring, t.ex gruppering av data, är en viktig aspekt för att hantera datatillgångar. hello principer för återställning av data och avyttring skulle definieras av en databevarandeprincip och startas om hello samma sätt som data gruppering; sådana arbete utförs av hello skyddar och administratör roller som en gemensam aktivitet.  

Fel toohave en databevarandeprincip kan innebära förlust eller fel toocomply med identifiering av regler och juridiska krav. De flesta organisationer som inte har en tydligt definierade databevarandeprincip tenderar toouse en standard ”hålla allt” bevarandeprincip. Men har en bevarandeprincip ytterligare risker i scenarier för tjänster. 

Till exempel betraktas en databevarandeprincip för molntjänst-providers som ”hello varaktighet hello prenumeration” (så länge hello-tjänsten är betalt för hello data sparas). En betala för kvarhållning avtalet kan inte hantera företagets och andra bevarandeprinciper. Definierar en princip för konfidentiella data kan säkerställa att data lagras och tas bort baserat på bästa praxis. Dessutom kan en arkiveringsprincip skapas tooformalize insikt om vilka data som ska tas bort av och när. 

Databevarandeprincip bör adressera hello krävs regelverk och krav på efterlevnad, samt krav på företagets juridiska datalagring. Klassificerad data kan leda till frågor om kvarhållning varaktighet och undantag för data som har lagrats med en leverantör. dessa frågor är mer sannolikt för data som inte har klassificerats korrekt. 

> [!TIP]
> Lär dig mer om bevarandeprinciper för Azure Data och mycket mer genom att läsa hello [prenumerationsavtalet för Microsoft Online](https://azure.microsoft.com/support/legal/subscription-agreement/)
> 
> 

## <a name="protecting-confidential-data"></a>Skydda känsliga data
När data klassificeras blir söka efter och implementera sätt tooprotect konfidentiella data en del av strategin för distribution eventuella data protection. Skydda känsliga data kräver ytterligare uppmärksamhet toohow data lagras och överföras i konventionella arkitekturer samt hello molnet. 

Det här avsnittet innehåller grundläggande information om vissa tekniker som kan automatisera tvingande ansträngningar toohelp skydda data som har klassificerats som konfidentiella. 

Som hello som följande bild visar dessa tekniker kan distribueras som lokala eller molnbaserade lösningar, eller på en hybrid sätt med några av dem distribuerade lokalt och vissa i hello molnet. (Vissa tekniker, till exempel kryptering och rättighetshantering, också utöka toouser enheter.)  

![Tekniker](./media/azure-security-data-classification/azure-security-data-classification-fig4.png)

### <a name="rights-management-software"></a>Rights management-programvara
En lösning för att förhindra förlust av data är rights management-programvara. Till skillnad från metoder som försöker toointerrupt hello flödet av information på Avsluta punkter i en organisation, fungerar rights management-programvaran på djup nivåer inom data lagringstekniker. Dokument krypteras och kontroll över vem som kan dekryptera dem använder åtkomstkontroller som definieras i en lösning för kontroll av autentisering, till exempel en katalogtjänst.  

> [!TIP]
> Du kan använda Azure Rights Management (Azure RMS) som hello information protection lösning tooprotect data i olika scenarier. Läs [vad är Azure Rights Management?](https://docs.microsoft.com/rights-management/understand-explore/what-is-azure-rms) för mer information om den här Azure lösningen.
> 
> 

Några av hello fördelarna med rights management-programvara är: 

* Skyddad känslig information. Användare kan skydda sina data direkt med rights management-aktiverade program. Inga ytterligare åtgärder krävs – redigera dokument, skicka e-post och publicerar data erbjuder en konsekventa data protection upplevelse. 
* Skyddet följer med hello data. Kunder behåller kontrollen över vem som har åtkomst till tootheir data om i hello moln, befintliga IT-infrastruktur, eller vid hello användarens skrivbord. Organisationer kan välja tooencrypt sina data och begränsa åtkomsten enligt tootheir affärsbehov. 
* Standard information protection-principer. Administratörer och användare kan använda standard policys för många vanliga affärsscenarierna, till exempel ”företagets konfidentiellt – Read Only” och ”inte vidarebefordra”. En omfattande uppsättning användningsrättigheter stöds till exempel läsa, kopiera, skriva ut, spara, redigera och framåt tooallow flexibilitet för att definiera anpassade användningsrättigheter. 

> [!TIP]
> Du kan skydda data i Azure Storage med hjälp av [Azure Storage Service-kryptering](../storage/storage-service-encryption.md) för Data i vila. Du kan också använda [Azure Disk Encryption](azure-security-disk-encryption.md) toohelp skydda data på virtuella diskar som används för virtuella datorer i Azure.
> 
> 

### <a name="encryption-gateways"></a>Kryptering gateways
Kryptering gateways fungerar i sina egna lager tooprovide kryptering services av omdirigering alla åtkomst toocloud-data. Den här metoden ska inte förväxlas med som ett virtuellt privat nätverk (VPN). Gatewayer är utformad tooprovide en transparent layer toocloud-baserade lösningar.   

Kryptering gateways kan ge ett sätt toomanage och säkra data som har klassificerats som konfidentiell genom att kryptera hello data under överföring samt data i vila.  

Kryptering gateways placeras i hello dataflödet mellan användarenheter och programdata Centrerar tooprovide kryptering/dekryptering tjänster. Dessa lösningar som VPN, är huvudsakligen lokala lösningar. De är utformad tooprovide tredje part med kontroll över krypteringsnycklar som hjälper dig att minska hello risken för att placera både hello data och hantering av nycklar med en provider. Dessa lösningar är utformade, ungefär som kryptering, toowork sömlöst och transparent mellan användare och hello-tjänsten. 

> [!TIP]
> Du kan använda Azure ExpressRoute tooextend ditt lokala nätverk till hello Microsoft cloud via en dedikerad privata anslutning. Läs [teknisk översikt för ExpressRoute](../expressroute/expressroute-introduction.md) för mer information om den här funktionen. Ett annat alternativ för gränsöverskridande anslutning mellan ditt lokala nätverk och [Azure är en plats-till-plats-VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).
> 
> 

### <a name="data-loss-prevention"></a>Skydd mot dataförlust
Dataförlust (ibland hänvisade tooas dataläckage) är viktigt och hello förhindra förlust av externa data via avsiktliga och oavsiktliga Insider-deltagare är av yttersta vikt för många organisationer.  

Data går förlorade förebygger tekniker hjälper dig att se till att lösningar, till exempel e-tjänster inte överföra data som har klassificerats som konfidentiella. Organisationer kan dra nytta av DLP-funktioner i befintliga produkter toohelp förhindra förlust av data. Sådana funktioner använder principer som du kan enkelt skapa från grunden eller genom att använda en mall som tillhandahålls av leverantören för hello-programvara.  

DLP-tekniker kan utföra djupgående innehållsanalys genom nyckelordsmatchning, ordlistematchning, reguljär uttrycksutvärdering och annat innehåll undersökning toodetect innehåll som strider mot organisationens DLP-principer. Till exempel kan DLP förhindra hello dataförlust hello följande typer av data: 

* Social Security och nationella ID-nummer 
* Bankinformation 
* Kreditkortsnummer  
* IP-adresser 

Vissa DLP-tekniker tillhandahåller också hello möjlighet toooverride hello DLP konfigurationen (till exempel om en organisation behöver tootransmit personnummer information tooa löneuppgifter processor). Dessutom är möjliga tooconfigure DLP så att användarna meddelas innan de försöker även toosend känslig information som inte ska skickas. 

> [!TIP]
> Du kan använda Office 365 DLP funktioner tooprotect dokument. Läs [Office 365 efterlevnad kontroller: förhindra dataförlust](https://blogs.office.com/2013/10/28/office-365-compliance-controls-data-loss-prevention/) för mer information.
> 
> 

## <a name="see-also"></a>Se även
* [Metodtips för Azure Data kryptering](azure-security-data-encryption-best-practices.md)
* [Azure Identitetshantering och åtkomst kontroll säkerhetsmetoder](azure-security-identity-management-best-practices.md)
* [Azure-säkerhetsteamets blogg](http://blogs.msdn.com/b/azuresecurity/)
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx)

