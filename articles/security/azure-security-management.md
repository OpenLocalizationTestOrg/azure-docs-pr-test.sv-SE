---
title: "aaaEnhance fjärrhantering säkerhet i Azure | Microsoft Docs"
description: "Den här artikeln beskriver hur du ökar säkerheten för fjärrhantering när du administrerar Microsoft Azure-miljöer, inklusive molntjänster, Virtual Machines och anpassade program."
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 2431feba-3364-4a63-8e66-858926061dd3
ms.service: security
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2017
ms.author: terrylan
ms.openlocfilehash: 9262cfb98bfe51d15fbad8f18997c4573668d9ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="security-management-in-azure"></a>Säkerhetshantering i Azure
Azure-prenumeranter kan hantera sina molnmiljöer från flera enheter, inklusive hantering av arbetsstationer, utvecklardatorer och även privilegierade slutanvändarens enheter som har uppgiftsspecifika behörigheter. I vissa fall kan administrativa funktioner utförs via webbaserade konsoler, till exempel hello [Azure-portalen](https://azure.microsoft.com/features/azure-portal/). I andra fall kan finnas det direkta anslutningar tooAzure från lokala system över virtuella privata nätverk (VPN), Terminal Services, klientprotokoll för program eller (programmässigt) hello Azure Service Management API (SMAPI). Dessutom kan klientslutpunkter vara antingen domänanslutna eller isolerade och ohanterade, till exempel surfplattor eller smartphones.

Flera funktioner för åtkomst och hantering ger en omfattande uppsättning alternativ, kan den här variationen lägga till betydande risk tooa molndistribution. Det kan vara svårt toomanage spåra och granska administrativa åtgärder. Den här variationen kan också innebära säkerhetshot via oreglerad åtkomst tooclient slutpunkter som används för att hantera molntjänster. Med hjälp av allmänna eller personliga arbetsstationer för att utveckla och hantera infrastrukturen öppnas oförutsägbara hotvektorer, till exempel webbsurfning (t.ex. vattenhålsattacker) eller mejl (t.ex. social manipulation och nätfiskewebbplatser).

![][1]

hello potentiella för angrepp ökar i den här typen av miljö eftersom det är svårt att använda tooconstruct säkerhetsprinciper och mekanismer tooappropriately hantera åtkomst tooAzure gränssnitt (till exempel SMAPI) från många olika slutpunkter.

### <a name="remote-management-threats"></a>Hot med fjärrhantering
Angripare försöka ofta toogain privilegierad åtkomst genom att kompromettera autentiseringsuppgifter (till exempel via lösenord, nätfiske och hämta autentiseringsuppgifter) eller genom att lura användare till att köra skadlig kod (till exempel från skadliga webbplatser med enhet med hämtningar eller från skadliga mejlbilagor). I en fjärradministrerade molnmiljö konto överträdelser kan leda tooan ökad risk på grund av tooanywhere, när som helst åtkomst.

Även om nära kontroller i primära administratörskontona kan användarkonton på lägre nivå vara används tooexploit svaga sidorna i Säkerhetsstrategin. Brist på lämplig säkerhetsutbildning kan också leda toobreaches via kontoinformation avslöjas eller exponeras på kontoinformation.

När en användares arbetsstation också används för administrativa uppgifter, kan den vara hotad på många olika punkter. Om en användare surfar hello webben, verktygen parts 3 eller öppen källkod eller öppnar en skadlig dokumentfil innehåller som en trojan.

De flesta riktade attacker som kan vara resultatet dataöverträdelser spåras i allmänhet toobrowser kryphål plugin-program (till exempel Flash, PDF, Java) och nätfiske (e-post) på stationära datorer. Dessa datorer kan ha administrativ nivå eller tjänstnivå behörigheter tooaccess live servrar eller nätverksenheter för åtgärder när den används för utveckling och hantering av andra tillgångar.

### <a name="operational-security-fundamentals"></a>Grunderna om operativ säkerhet
Du kan minska angreppsytan för en klient genom att minska hello antalet möjliga startpunkter för säker hantering och åtgärder. Det kan göras med hjälp av säkerhetsprinciper: ”uppdelning av uppgifter” och ”ansvarsfördelning av miljöer”.

Isolera känsliga funktioner från varandra toodecrease hello sannolikheten att ett fel på en nivå leder tooa intrång i en annan. Exempel:

* Administrativa uppgifter bör inte kombineras med aktiviteter som kan leda till röjande tooa (till exempel att skadlig kod i en administratörs mejl som sedan infekterar en Infrastrukturserver).
* En arbetsstation som används för åtgärder med hög känslighet bör inte vara hello samma system som används för risk, till exempel webbläsarens hello Internet.

Minska risken för angrepp hello system genom att ta bort onödiga program. Exempel:

* Standard administrativa, support eller utveckling bör inte kräva installation av en e-postklient eller andra produktivitetsprogram installerade om hello Huvudsyftet är toomanage molntjänster.

Klientdatorer som måste administratören åtkomst tooinfrastructure komponenter bör vara föremål toohello strikta principer som möjligt för tooreduce säkerhetsrisker. Exempel:

* Säkerhetsprinciperna kan innehålla grupprincipinställningar som nekar öppen Internet-åtkomst från hello enheten och användning av en restriktiv brandväggskonfiguration.
* Använd IPsec (Internet Protocol security) VPN om direktåtkomst behövs.
* Konfigurera separata Active Directory-domäner för hantering och utveckling.
* Isolera och filtrera nätverkstrafiken på hanteringsdatorn.
* Använd program mot skadlig kod.
* Implementera multifaktorautentisering tooreduce hello risken för stulna autentiseringsuppgifter.

Du kan också förenkla hanteringsuppgifter genom att konsolidera och ta bort ohanterade slutpunkter.

### <a name="providing-security-for-azure-remote-management"></a>Skapa säker Azure-fjärrhantering
Azure tillhandahåller säkerhet mekanismer tooaid administratörer som hanterar Azure-molntjänster och virtuella datorer. Dessa mekanismer är:

* Autentisering och [rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md).
* Övervakning, loggning och granskning.
* Certifikat och krypterad kommunikation.
* En webbhanteringsportal.
* Filtrering av nätverkspaket.

Med säkerhetskonfiguration på klientsidan och datacenterdistribution av en management gateway är det möjligt toorestrict och övervaka administratör åtkomst toocloud program och data.

> [!NOTE]
> Vissa rekommendationerna i den här artikeln kan leda till ökad användning av data, nätverk eller beräkningsresurser och öka kostnaderna för din licens eller prenumeration.
>
>

## <a name="hardened-workstation-for-management"></a>Förstärkt dator för hantering
hello syftet med att förstärka en dator är tooeliminate alla men hello mest kritiska funktioner som krävs för att den toooperate, vilket gör hello potentiella attacker så mycket som möjligt. Systemförstärkning handlar bland annat minimera hello antalet installerade tjänster och program, begränsa programkörning, begränsa network access tooonly vad som behövs och alltid hålla toodate hello system. Dessutom separeras administrativa verktyg och aktiviteter från andra slutanvändaraktiviteter när man arbetar med en förstärkt dator.

Du kan begränsa hello risken för angrepp på den fysiska infrastrukturen via dedikerade hanteringsnätverk, serverrum med kortåtkomst och datorer som körs på skyddade delar av hello nätverk i en lokal företagsmiljö. I ett moln eller hybridbaserad IT-modell kan kan et säker hantering vara mer komplexa på grund av hello bristande fysisk åtkomst tooIT resurser. Det krävs noggrann programvarukonfiguration, säkerhetsfokuserade processer och omfattande principer för att implementera skyddslösningar.

Med en minsta-programvara med lägsta behörighet i en låst dator för molnhantering av, och för programutveckling – kan minska hello risken för säkerhetsincidenter genom att standardisera hello fjärransluten hantering och utveckling miljöer. En förstärkt Datorkonfiguration kan förhindra hello röjande av konton som används toomanage kritiska molnresurser genom att stänga många vanliga vägar som används av skadlig kod och trojaner. Använd särskilt [Windows AppLocker](http://technet.microsoft.com/library/dd759117.aspx) och Hyper-V-teknik toocontrol och isolera klientsystembeteende och minska hotrisken, inklusive e-post eller surfa på Internet.

Hej administratör kör ett vanligt användarkonto (som blockerar körning på administrativ nivå) på en förstärkt dator och associerade program styrs av en lista över tillåtna. hello grundelementen i en förstärkt dator är följande:

* Aktiv genomsökning och korrigering. Distribuera program mot skadlig kod, utföra regelbundna sårbarhetsgenomsökningar och uppdatera alla datorer med hjälp av hello senaste säkerhetsuppdateringarna inom rimlig tid.
* Begränsad funktionalitet. Avinstallera alla program som inte behövs och inaktivera onödiga (start-) tjänster.
* Förstärka nätverket. Använd Windows-brandväggen regler tooallow endast giltiga IP-adresser, portar och URL: er relaterade tooAzure hantering. Se till att inkommande fjärranslutningar toohello arbetsstation också blockeras.
* Begränsning av körning. Tillåt endast en uppsättning fördefinierade körbara filer som behövs för hantering av toorun (kallas tooas ”standardneka”). Som standard bör användare nekas behörighet toorun de program såvida inte definieras uttryckligen i hello listan över tillåtna.
* Lägsta behörighet. Användare av hanteringsdatorn bör inte ha administrativ behörighet på hello själva lokala datorn. Det här sättet kan inte ändra systemkonfigurationen hello eller hello systemfiler, oavsiktligt eller avsiktligt.

Du kan göra allt detta med hjälp av [grupprincipobjekt](https://www.microsoft.com/download/details.aspx?id=2612) (GPO) i Active Directory Domain Services (AD DS) och tillämpa dem i din (lokala) management-tooall management domänkonton.

### <a name="managing-services-applications-and-data"></a>Hantera tjänster, program och data
Azure cloud services-konfiguration utförs via hello Azure portal eller SMAPI, via kommandoradsgränssnittet Windows PowerShell för hello eller ett specialbyggt program som utnyttjar dessa RESTful-gränssnitt. Produkter som använder dessa mekanismer är bland annat Azure AD (Active Directory Azure), Azure Storage, Azure Websites och Azure Virtual Network.

Program som distribueras på virtuella datorer tillhandahåller egna klientverktyg och gränssnitt efter behov, till exempel hello Microsoft Management Console (MMC), en konsol för företagshantering (till exempel Microsoft System Center eller Windows Intune) eller en annan management program, Microsoft SQL Server Management Studio, t.ex. Verktygen finns oftast i en företagsmiljö eller i ett klientnätverk. De kan vara beroende av specifika nätverksprotokoll, till exempel Remote Desktop Protocol (RDP), som kräver direkta och tillståndskänsliga anslutningar. Vissa kan ha webbaktiverade gränssnitt som inte ska vara publicerade eller tillgängliga via hello Internet öppet.

Du kan begränsa åtkomst tooinfrastructure och plattform tjänster för hantering i Azure med hjälp av [multifaktorautentisering](../multi-factor-authentication/multi-factor-authentication.md), [X.509-hanteringscertifikat](https://blogs.msdn.microsoft.com/azuresecurity/2015/07/13/certificate-management-in-azure-dos-and-donts/), och brandväggsregler. hello Azure portal och SMAPI kräver Transport Layer Security (TLS). Tjänster och program som du distribuerar till Azure måste du dock tootake skyddsåtgärder som är lämpliga för ditt program. Dessa mekanismer kan ofta aktiveras enklare via en standardiserad förstärkt datorkonfiguration.

### <a name="management-gateway"></a>Management Gateway
toocentralize alla administrativ åtkomst och förenkla övervakning och loggning kan du distribuera ett dedikerat [fjärrskrivbordsgateway](https://technet.microsoft.com/library/dd560672) (RD Gateway)-server i ditt lokala nätverk, anslutna tooyour Azure-miljön.

En  fjärrskrivbordsgateway  är en principbaserad RDP-proxytjänst som tillämpar säkerhetskrav. Genom att implementera RD Gateway tillsammans med Windows Server NAP (Network Access Protection) kan du se till att bara klienter som uppfyller de specifika hälsokriterier som upprättats av grupprincipobjekt för AD DS (Active Directory Domain Services) kan ansluta. Följande gäller också:

* Etablera en [Azure-hanteringscertifikatet](http://msdn.microsoft.com/library/azure/gg551722.aspx) på hello RD Gateway så att den är hello endast värden tillåts tooaccess hello Azure-portalen.
* Anslut hello RD Gateway toohello samma [hanteringsdomän](http://technet.microsoft.com/library/bb727085.aspx) som hello administratörsdatorerna. Detta är nödvändigt när du använder en plats-till-plats IPsec VPN eller ExpressRoute i en domän som har ett enkelriktat förtroende tooAzure AD, eller om du federerar autentiseringsuppgifter mellan din lokala AD DS-instans och Azure AD.
* Konfigurera en [auktoriseringsprincip för klientanslutning](http://technet.microsoft.com/library/cc753324.aspx) toolet hello RD Gateway kontrollerar du att hello klientdatorns namn är giltigt (domänanslutet) och tillåtna tooaccess hello Azure-portalen.
* Använd IPsec för [Azure VPN](https://azure.microsoft.com/documentation/services/vpn-gateway/) toofurther skydda hanteringstrafiken från tjuvlyssnande och token-stöld eller överväga en isolerad Internet-anslutning via [Azure ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/).
* Aktivera multifaktorautentisering (via [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md)) eller smartkortsautentisering för administratörer som loggar in via RD Gateway.
* Konfigurera källa [IP-adressbegränsningar](http://azure.microsoft.com/blog/2013/08/27/confirming-dynamic-ip-address-restrictions-in-windows-azure-web-sites/) eller [Nätverkssäkerhetsgrupper](../virtual-network/virtual-networks-nsg.md) i Azure toominimize hello antalet tillåtna hanteringsslutpunkter.

## <a name="security-guidelines"></a>Riktlinjer för säkerhet
I allmänhet hjälpa toosecure administratörsdatorerna för användning med hello molnet är liknande toohello metoder som används för alla övriga lokala – till exempel minimera byggnadsbehörigheterna och de restriktiva behörigheterna. Vissa unika aspekter av molnhantering är mer akin tooremote eller enterprise out-of-band-hantering. Dessa inkluderar hello använda och granska autentiseringsuppgifter, fjärråtkomst med ökad säkerhet och hotidentifiering och -svar.

### <a name="authentication"></a>Autentisering
Du kan använda Azure inloggning begränsningar tooconstrain källans IP-adresser för att komma åt administrativa verktyg och granska förfrågningar. toohelp Azure identifiera hanteringsklienter (datorer och/eller program), du kan konfigurera både SMAPI (via kundutvecklade verktyg som Windows PowerShell-cmdlets) och hello Azure portal toorequire hantering av klientens certifikat toobe installerat dessutom tooSSL certifikat. Vi rekommenderar också att administratörsåtkomst kräver multifaktorautentisering.

Vissa program eller tjänster som du distribuerar i Azure kan ha sina egna autentiseringsmekanismer för både slutanvändare och administratörer, medan andra utnyttjar Azure AD fullt ut. Beroende på om du federerar autentiseringsuppgifter via Active Directory Federation Services (AD FS), via katalogsynkronisering eller underhåll av användarkonton enbart i hello molnet, med [Microsoft Identity Manager](https://technet.microsoft.com/library/mt218776.aspx) (del av Azure AD Premium) hjälper dig att hantera identitetslivscykler mellan hello resurser.

### <a name="connectivity"></a>Anslutning
Mekanismer är tillgängliga toohelp skydda anslutningar tooyour virtuella Azure-nätverk. Två av dessa mekanismer [plats-till-plats VPN](https://channel9.msdn.com/series/Azure-Site-to-Site-VPN) (S2S) och [punkt-till-plats VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) (P2S), aktivera hello användning av branschstandarden standard IPsec (S2S) eller hello [Secure Socket Tunneling Protocol](https://technet.microsoft.com/magazine/2007.06.cableguy.aspx)(SSTP) (P2S) för kryptering och tunnlar. När Azure ansluter toopublic Azure riktade hantering, till exempel hello Azure-portalen, kräver Azure Hypertext Transfer Protocol Secure (HTTPS).

En fristående strikt dator som inte ansluter tooAzure via en RD Gateway ska använda hello SSTP-baserade punkt-till-plats VPN-toocreate hello första anslutningen toohello Azure Virtual Network och därefter ska RDP-anslutning tooindividual virtuella datorer från hello VPN-tunnel.

### <a name="management-auditing-vs-policy-enforcement"></a>Hantering av granskning jämfört med tillämpning av principer
Vanligtvis det finns två tillvägagångssätt för att hjälpa toosecure hanteringsprocesser: gransknings- och principtillämpning. Om du gör både och får du omfattande kontroller, men det kanske inte är möjligt i alla situationer. Dessutom har varje metod olika nivåer av risk, kostnad och arbete som är kopplat till att hantera säkerhet, särskilt när det gäller toohello andelen förtroende som placerats i både enskilda och systemarkitekturer.

Övervakning, loggning och granskning ger en grund för att spåra och förstå administrativa aktiviteter, men det inte alltid möjligt tooaudit alla åtgärder i slutföra detalj på grund av toohello mängden data som genereras. Granskning hello effektivitet med principer för hantering av hello är bästa praxis, men.

Principtillämpning som innehåller strikta åtkomstkontroller skapar programmässiga mekanismer som kan styra administratörsåtgärder, och det säkerställer att alla möjliga skyddsåtgärder används. Loggning ger bevis på efterlevnad i tillägg tooa register över vem som gjorde vad, från var och när. Loggning också kan du tooaudit och dubbelkontrollera information om hur administratörer följer principerna, och det ger aktivitetsbevis

## <a name="client-configuration"></a>Klientkonfiguration
Vi rekommenderar tre primära konfigurationer för en förstärkt dator. hello största skillnaderna mellan dem är kostnad, användbarhet och tillgänglighet, samtidigt som en liknande säkerhetsprofil för alla alternativ. hello följande tabell ger en kort analys av hello fördelar och riskerna tooeach. (Observera att ”företagets dator” hänvisar tooa stationär dator med standardkonfiguration som distribueras för alla domänanvändare, oavsett roller.)

| Konfiguration | Fördelar | Nackdelar |
| --- | --- | --- |
| Fristående förstärkt dator |Strikt kontrollerad dator |högre kostnader för dedikerade stationära datorer |
| - | Minskad risk för programtrojaner |Ökat hanteringsarbete |
| - | Tydlig uppdelning av uppgifter | - |
| Företagets dator som virtuell dator |Minskade maskinvarukostnader | - |
| - | Uppdelning av roll och program | - |
| Windows toogo med BitLocker-diskkryptering |Kompatibilitet med de flesta datorer |Tillgångar |
| - | Kostnadseffektivitet och portabilitet | - |
| - | Isolerad hanteringsmiljö |- |

Det är viktigt att hello förstärkt hello värden och inte hello Gäst, utan något mellan hello är värd för operativsystemet och hello maskinvara. Följande hello ”principen om ren källa” innebär (även kallat ”säkert ursprung”) hello värden ska vara hello mest härdat. Annars är hello förstärkta datorn (Gäst) ämne tooattacks på hello system som är värd.

Du kan särskilja administrativa funktioner via dedikerade systemavbildningar för varje strikt dator som har endast hello verktyg ytterligare krävs behörighet för att hantera välja och Azure molnprogram med specifika lokala AD DS-grupprincipobjekt för hello uppgifter som krävs.

För IT-miljöer som har någon lokal infrastruktur (till exempel ingen åtkomst tooa lokal AD DS-instans för grupprincipobjekt eftersom alla servrar i hello molnet), en tjänst som [Microsoft Intune](https://technet.microsoft.com/library/jj676587.aspx) kan förenkla distribution och underhåll datorkonfigurationer.

### <a name="stand-alone-hardened-workstation-for-management"></a>Fristående strikt dator för hantering
Med en fristående strikt dator  har administratörer en stationär eller bärbar dator som de använder för administrativa uppgifter och en annan separat stationär eller bärbar dator för icke-administrativa uppgifter. En arbetsstation som är dedikerad toomanaging Azure-tjänster behöver inte andra program installerade. Dessutom hjälper datorer som stöder en [Trusted Platform Module](https://technet.microsoft.com/library/cc766159) (TPM) eller liknande krypteringsteknik på maskinvarunivå till med att autentisera enheter och skydda från vissa angrepp. TPM kan också använda fullständig volym skydd av hello systemenhet genom att använda [BitLocker-diskkryptering](https://technet.microsoft.com/library/cc732774.aspx).

I hello fristående strikt dator scenario (se nedan) hello lokal instans av Windows-brandväggen (eller en klient för icke-Microsoft-brandvägg) är konfigurerade tooblock inkommande anslutningar, till exempel RDP. Hej administratör kan logga in toohello förstärkt och starta en RDP-session som ansluter tooAzure när du har etablerat en VPN-anslutning ansluta med ett Azure Virtual Network, men kan inte logga in tooa företagets dator och använda RDP tooconnect toohello härdat arbetsstation själva.

![][2]

### <a name="corporate-pc-as-virtual-machine"></a>Företagets dator som virtuell dator
I fall där en separat fristående strikt dator kostar kostnadshindrande eller olämplig, hello förstärkt dator kan vara värd för en virtuell dator tooperform icke-administrativa uppgifter.

![][3]

tooavoid flera säkerhetsrisker som kan uppstå om du använder en dator för systemhantering och andra dagliga arbetsuppgifter kan du distribuera en Windows Hyper-V virtuell dator toohello härdat arbetsstation. Den här virtuella datorn kan användas som hello företagets dator. hello kan företagets förbli isolerad från hello värddatorn, vilket minskar dess angreppsyta och tar bort hello användarens dagliga aktiviteter (till exempel e-post) från tillsammans med känsliga administrativa uppgifter.

hello företagets virtuella dator körs i ett skyddat utrymme och tillhandahåller användarprogram. hello-värden är en ”ren källa” och tillämpar strikta nätverksprinciper i hello rotoperativsystemet (till exempel blockera RDP-åtkomst från hello virtuell dator).

### <a name="windows-toogo"></a>Windows tooGo
Ett annat alternativ toorequiring en fristående strikt dator är toouse en [Windows tooGo](https://technet.microsoft.com/library/hh831833.aspx) enhet, en funktion som har stöd för en funktion för USB-Start på klientsidan. Windows tooGo gör det möjligt för användare tooboot en kompatibel dator tooan isolerad systemavbildning som körs från en krypterad USB-enhet. De ger ytterligare kontroller för slutpunkter för fjärradministration eftersom hello avbildningen kan hanteras helt av företagets IT-grupp, med strikta säkerhetsprinciper, en minimal OS-version och TP-stöd.

I hello bilden nedan är hello bärbara avbildningen ett domänanslutet system som är förkonfigurerad tooconnect endast tooAzure, kräva multifaktorautentisering och blockerar all trafik för icke-management. Om en användare startas hello samma PC toohello företagets standardavbildning och försöker komma åt RD Gateway för Azure-hanteringsverktyg, blockeras hello-sessionen. Windows tooGo blir operativsystemet för hello rotnivå och inga ytterligare lager är krävs (värdoperativsystem system, hypervisor, virtuell dator) som kan vara mer sårbara toooutside attacker.

![][4]

Det är viktigt toonote att USB-flash-enheter försvinner lättare än en genomsnittlig stationär dator. Använder BitLocker tooencrypt hello hela volymen, tillsammans med ett starkt lösenord, blir det mindre troligt att en angripare kan använda hello enhetsavbildningen i skadliga syften. Dessutom, om hello USB-flash-enhet tappas bort, återkalla och [utfärda ett nytt certifikat](https://technet.microsoft.com/library/hh831574.aspx) tillsammans med ett lösenord för snabb återställning kan du minska exponeringen. Administrativa granskningsloggar i Azure, inte på hello-klienten ytterligare reducera potentiella dataförluster.

## <a name="best-practices"></a>Bästa praxis
Överväg att hello följande riktlinjer när du hanterar program och data i Azure.

### <a name="dos-and-donts"></a>Att tänka på
Inte förutsätt att eftersom en arbetsstation har låsts som andra vanliga säkerhetskrav inte behöver toobe uppfylls. hello risken är högre på grund av upphöjda åtkomstnivåerna som administratörskonton normalt har. Exempel på risker och deras alternativa metoder för säker visas i hello nedan.

| Gör inte följande | Gör följande |
| --- | --- |
| Skicka inte autentiseringsuppgifter för administratörsåtkomst eller andra hemligheter via mejl (t.ex. SSL eller hanteringscertifikat) |Upprätthåll sekretessen genom att lämna ut kontonamn och -lösenord via telefon (men lagrar dem inte i röstmeddelanden), utför en fjärrinstallation av klient-/servercertifikat (via en krypterad session), ladda ned från en skyddad nätverksresurs eller distribuera manuellt via flyttbara medier. |
| - | Hantera hanteringscertifikatets livscyklar proaktivt. |
| Lagra inte lösenord okrypterade eller icke-hashformaterade i programlagring (till exempel i kalkylblad, SharePoint-webbplatser eller filresurser). |Fastställa säkerhetsprinciper och principer för systemhärdning och Använd dem tooyour utvecklingsmiljö. |
| - | Använd [Enhanced Mitigation Experience Toolkit 5.5](https://technet.microsoft.com/security/jj653751) certifikat fästning regler tooensure tillgång tooAzure SSL/TLS-webbplatser. |
| Använd inte samma konton och lösenord för flera administratörer. Återanvänd inte heller lösenord i flera användarkonton eller -tjänster, särskilt de för sociala medier eller andra icke-administrativa aktiviteter. |Skapa en dedikerad Microsoft-konto toomanage din Azure-prenumeration, ett konto som inte används för personliga e-post. |
| Skicka inte konfigurationsfiler via mejl. |Konfigurationsfiler och profiler ska installeras från en betrodd källa (till exempel en krypterad USB-flash-enhet), inte från en mekanism som lätt kan äventyras, till exempel mejl. |
| Använd inte svaga eller enkla lösenord för inloggning. |Tillämpa principer för starka lösenord, förfallotider (ändra vid första användning), tidsgränser för konsolen och automatiska kontolåsningar. Använd ett hanteringssystem för klientlösenord med multifaktorautentisering för valvåtkomst med lösenord. |
| Exponera inte management portar toohello Internet. |Låsa Azure-portar och IP-adresser toorestrict åtkomst. Mer information finns i hello [nätverkssäkerhet för Azure](http://download.microsoft.com/download/4/3/9/43902EC9-410E-4875-8800-0788BE146A3D/Windows%20Azure%20Network%20Security%20Whitepaper%20-%20FINAL.docx) vitboken. |
| - | Använd brandväggar, VPN:er och NAP för alla hanteringsanslutningar. |

## <a name="azure-operations"></a>Åtgärder i Azure
Inom Microsofts Azure-åtgärder använder operationstekniker och supportpersonal som har åtkomst till Azures produktionssystem  [härdade datorer med virtuella datorer](#stand-alone-hardened-workstation-for-management) etablerade på dem för intern åtkomst till företagsnätverk och -program (till exempel mejl, intranät o.s.v.). Alla hanteringsdatorer har TPM: er, hello värdens startenheten är krypterad med BitLocker och de är domänanslutna tooa särskild organisationsenhet (OU) i Microsofts primära företagsdomän.

Systemhärdning tillämpas via en grupprincip med centraliserad uppdatering av programvara. Granskning och analys är händelseloggar (till exempel säkerhet och AppLocker) in från hanteringsdatorer och sparas tooa central plats.

Dessutom är dedikerade jumpbox-datorer i Microsofts nätverk som kräver tvåfaktorsautentisering används tooconnect tooAzure produktionsnätverk.

## <a name="azure-security-checklist"></a>Checklista för Azure-säkerhet
Minimera hello antalet uppgifter som administratörer kan utföra på en förstärkt dator bidrar till att minimera hello risken för angrepp i utvecklings- och hanteringsmiljö. Använd hello följande tekniker toohelp skydda din härdade dator:

* IE-härdning. hello webbläsaren Internet Explorer (eller valfri webbläsare att) är en viktig ingångspunkt för skadlig kod på grund av tooits omfattande interaktioner med externa servrar. Granska dina klientprinciper och se till att webbläsaren körs i skyddat läge genom att inaktivera tillägg, inaktivera filnedladdningar och använda [Microsoft SmartScreen](https://technet.microsoft.com/library/jj618329.aspx)-filtrering. Kontrollera att säkerhetsvarningar visas. Dra nytta av Internetzoner och skapa en lista över betrodda platser som du har konfigurerat rimliga härdning för. Blockera alla andra webbplatser och kod i webbläsaren, till exempel ActiveX och Java.
* Standardanvändare. Körs som en vanlig användare får ett antal fördelar, är hello största att stjäla administratörers autentiseringsuppgifter via skadlig kod blir svårare. Dessutom kan ett vanligt användarkonto har inte utökade privilegier på rotoperativsystemet hello och många konfigurationsalternativ och API: er är låsta som standard.
* AppLocker. Du kan använda [AppLocker](http://technet.microsoft.com/library/ee619725.aspx) toorestrict hello program och skript som användarna kan köra. Du kan köra AppLocker i granskningsläge eller tvingande läge. Som standard har AppLocker en Tillåt-regel som låter användare som har en token admin-toorun all kod på hello-klienten. Den här regeln finns tooprevent administratörer låser sig ut och den gäller endast tooelevated token. Se även Kodintegritet som en del av Windows Servers [grundläggande säkerhet](http://technet.microsoft.com/library/dd348705.aspx).
* Kodsignering. Att kodsignera alla verktyg och skript som används av administratörer ger en hanterbar mekanism för att distribuera principer för programlåsning. Hashvärden skalas inte med snabba kodändringar toohello kod och filsökvägar ger inte en hög säkerhetsnivå. Du bör kombinera AppLocker-regler med en PowerShell [körningsprincip](http://technet.microsoft.com/library/ee176961.aspx) som endast tillåter att viss signerad kod och skript toobe [utförs](http://technet.microsoft.com/library/hh849812.aspx).
* Grupprincip. Skapa en global administrativ princip som är kopplade tooany Domändatorer som används för hantering (och blockera åtkomst från alla andra) och toouser konton autentiseras på datorerna.
* Säkrare etablering. Skydda Baslinjens härdade datoravbildning toohelp skyddas mot manipulation. Använd säkerhetsåtgärder som kryptering och isolering toostore avbildningar, virtuella datorer och skript och begränsa åtkomsten (kanske använda en granskningsbar-i/utcheckning process).
* Korrigering. Underhåll en konsekvent avbildning (eller ha separata avbildningar för utveckling, åtgärder och andra administrativa uppgifter), söka efter ändringar och skadlig kod regelbundet, hålla hello byggs upp toodate och aktivera bara datorer när de behövs.
* Kryptering. Kontrollera att hanteringsdatorerna har en TPM-toomore på ett säkert sätt aktivera [Krypterande filsystem](https://technet.microsoft.com/library/cc700811.aspx) (EFS) och BitLocker. Om du använder Windows tooGo, använder du bara krypterade USB-nycklar tillsammans med BitLocker.
* Styrning. Använda AD DS-grupprincipobjekt toocontrol alla hello administratörens Windows-gränssnitt, till exempel fildelning. Inkludera hanteringsdatorer processerna för granskning, övervakning och loggning. Spåra alla administratörers och utvecklares åtkomst och användning.

## <a name="summary"></a>Sammanfattning
Genom att använda en härdad datorkonfiguration för administrering av dina Azure-molntjänster, virtuella datorer och program kan du undvika flera risker och hot som kan uppstå när du fjärrhanterar kritiska IT-infrastrukturer. Både Azure och Windows erbjuder mekanismer för att du kan använda toohelp skydda och styr beteendet för kommunikation, autentisering och klienten.

## <a name="next-steps"></a>Nästa steg
hello följande resurser finns tillgängliga tooprovide mer allmän information om Azure och relaterade Microsoft-tjänster i tillägg toospecific objekt som refereras till i det här dokumentet:

* [Skydda privilegierad åtkomst](https://technet.microsoft.com/library/mt631194.aspx) – hämta teknisk information om hello för att designa och skapa en säker administrativ dator för hantering av Azure
* [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/Security/AzureSecurity) – Lär dig mer om funktionerna i Azure-plattformen som skyddar hello Azure-strukturen och hello arbetsbelastningar som körs på Azure
* [Microsoft Security Response Center](http://www.microsoft.com/security/msrc/default.aspx) --Microsoft säkerhetsrisker, inklusive problem med Azure, kan rapporteras eller via e-post för[secure@microsoft.com](mailto:secure@microsoft.com)
* [Azures Säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) – Håll dig toodate på hello senaste inom Azure-säkerhet

<!--Image references-->
[1]: ./media/azure-security-management/typical-management-network-topology.png
[2]: ./media/azure-security-management/stand-alone-hardened-workstation-topology.png
[3]: ./media/azure-security-management/hardened-workstation-enabled-with-hyper-v.png
[4]: ./media/azure-security-management/hardened-workstation-using-windows-to-go-on-a-usb-flash-drive.png
