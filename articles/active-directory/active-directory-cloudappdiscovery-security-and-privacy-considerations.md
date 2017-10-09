---
title: "aaaCloud App Discovery säkerhets- och överväganden för sekretess | Microsoft Docs"
description: "Det här avsnittet beskriver hello säkerhet och sekretess överväganden relaterade tooCloud App Discovery."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 2fce5c82-d3de-4097-808f-40214768df9e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 33659e85bd2cf4294e443512e69a85401f7c53f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-app-discovery-security-and-privacy-considerations"></a>Cloud App Discovery säkerhets- och överväganden för sekretess
Microsoft allokerat tooprotecting din integritet och skydda dina data, samtidigt som de levererar programvaror och tjänster som hjälper dig att hantera hello säkerheten för din organisation.  
Vi förstår att när du ge dina data tooothers förtroendet kräver omfattande säkerhet tekniska investeringar och kunskaper tooback den.
Microsoft följer toostrict efterlevnad och riktlinjer för säkerhet från säkra software development lifecycle praxis toooperating en tjänst.  
Säkra och skydda data är högsta prioritet på Microsoft.

Det här avsnittet beskrivs hur data samlas in, bearbetas och skyddas i Azure Active Directory Cloud App Discovery

## <a name="overview"></a>Översikt
Cloud App Discovery ingår i Azure AD och finns i Microsoft Azure.  
hello Cloud App Discovery endpoint agent är används toocollect programmet identifieringsdata från IT hanterade datorer.  
hello skickas insamlade data på ett säkert sätt via en krypterad kanal toohello Azure AD Cloud App Discovery-tjänsten.  
hello Cloud App Discovery-data för en organisation visas sedan i hello Azure-portalen. 

![Så här fungerar Cloud App Discovery](./media/active-directory-cloudappdiscovery-security-and-privacy-considerations/cad01.png) 

hello avsnitten följer hello flödet av information och beskriver hur den skyddas som flyttas från din organisation toohello Cloud App Discovery-tjänsten och slutligen toohello Cloud App Discovery-portalen.

## <a name="collecting-data-from-your-organization"></a>Samla in data från din organisation
I ordning toouse Azure Active Directorys Cloud App discovery funktionen tooget insikter om hello-program som används av anställda i din organisation, behöver du toofirst distribuera hello Azure AD Cloud App Discovery endpoint agent toomachines i din organisation.

Administratörer av hello Azure Active Directory-klient (eller ombud) kan hämta installationspaketet för hello agent från hello Azure-portalen. hello-agenten kan antingen manuellt installerats eller installeras på flera datorer i hello organisationen med hjälp av SCCM eller en Grupprincip.

Mer information om distributionsalternativ finns [Cloud App Discovery grupp princip Deployment Guide](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx).


### <a name="data-collected-by-hello-agent"></a>Data som samlas in av hello-agenten
hello samlas beskrivs i nedanstående lista hello in av hello agent när en anslutning görs tooa webbprogram. hello information samlas endast in för dessa program att hello-administratör har konfigurerat för identifiering.  
Du kan redigera hello lista över molnappar som hello agenten övervakar via hello Cloud App Discovery-bladet i hello Microsoft [Azure-portalen](https://portal.azure.com/)under **inställningar**->**Data Samlingen**->**App Samlingslista**. Mer information finns i [komma igång med Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)


**Information kategori**: användarinformation  
**Beskrivning**:  
hello Windows processens användarnamn hello som gjorde begäran toohello target webbprogram (t.ex.: DOMÄN\användarnamn) samt hello Windows säkerhetsidentifierare (SID) för hello användare.

**Information kategori**: processinformation  
**Beskrivning**:  
hello namnet på hello process som gjorts hello begäran toohello målwebbprogram (t.ex.: ”iexplore.exe”)

**Information kategori**: datorn information  
**Beskrivning**:  
hello maskinens NetBIOS-namn på vilka hello-agenten är installerad.

**Information kategori**: App trafikinformation  
**Beskrivning**: 

följande anslutningsinformationen hello:

* hello källa (lokal dator) och mål-IP-adresser och portnummer
* hello offentliga IP-adress hello organisation genom vilken hello begäran går ut.
* hello tid för hello begäran
* hello mängden trafik som skickas och tas emot
* hello IP-version (4 eller 6)
* För TLS-anslutningar: hello målvärddator från hello Servernamnsindikation tillägg eller hello servercertifikat.

Hej efter HTTP-information:

* Metod (GET, POST, etc.)
* Protokoll (HTTP/1.1, etc.)
* Användaragentsträngen
* Värdnamn
* Mål-URI (exklusive frågesträngen)
* Information om innehållstyp
* Referent URL-information (exklusive frågesträngen)

> [!NOTE]
> hello HTTP ovanstående information samlas in för alla icke-krypterade anslutningar.
> För TLS-anslutningar fångas endast den här informationen när hello 'djupinspektion-inställningen är aktiverad i hello portal. hello standardinställningen är ”ON”.
> Mer information finns nedan, och [komma igång med Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)
> 
> 

Dessutom toohello hello agenten samlar in data om hello nätverksaktivitet, samlar även in anonym information om hello konfiguration av programvara och maskinvara, felrapporter och information om hur hello agent används.


### <a name="how-hello-agent-works"></a>Så här fungerar hello-agent
hello agentinstallation innehåller två komponenter:

* En komponent i användarläge
* En komponent som drivrutinen kernel-läge (Windows Filtering Platform drivrutin)

När hello agent installeras första gången ett betrott certifikat datorspecifik lagras på hello datorn som den använder sedan tooestablish en säker anslutning med hello Cloud App Discovery-tjänsten.  
hello hämtar regelbundet agenten principkonfigurationen från hello Cloud App Discovery-tjänsten via den här säker anslutning.  
hello-policy inkluderar information om vilka program toomonitor för molnet och om automatisk uppdatering måste vara aktiverad, bland annat.

Internet-trafik skickas och tas emot på hello datorn från Internet Explorer och Chrome, hello Cloud App Discovery-agenten analyserar hello trafik och extrakt hello relevanta metadata (se hello **Data som samlas in av hello agenten** avsnitt ovan).  
Varje minut filöverföringar hello agent hello insamlade metadata toohello Cloud App Discovery-tjänsten via en krypterad kanal.

hello drivrutinen komponenten fångar upp hello krypterad trafik och infogas i hello krypterad dataström. Mer information finns i hello **fånga upp data från krypterade anslutningar (djupinspektion)** nedan.

### <a name="respecting-user-privacy"></a>Respektera användarnas integritet
Vårt mål är tooprovide administratörer hello verktyg tooset hello balans mellan detaljerad optik till programmet användnings- och sekretess för organisationen. toothat end vi tillhandahåller hello följande rattar i hello inställningssidan i hello Portal:

* **Datainsamling**: Administratörer kan välja toospecify vilka program eller programkategorier som de vill tooget identifieringsdata på.
* **Djupinspektion**: Administratörer kan välja toospecify om hello agenten samlar in HTTP-trafik för SSL/TLS-anslutningar (aka **'Djupinspektion'**). Mer om detta i nästa avsnitt om hello.
* **Medgivande alternativ**: Administratörer kan använda hello Cloud App Discovery portal toochoose om toonotify användare av hello insamling av hello-agenten och huruvida toorequire användaren godkänna hello agenten börjar samla in användardata.

hello Cloud App Discovery endpoint agent samlar endast in hello informationen som beskrivs i hello **Data som samlas in av hello agenten** ovan.

### <a name="intercepting-data-from-encrypted-connections-deep-inspection"></a>Fånga upp data från krypterade anslutningar (djupinspektion)
Administratörer kan konfigurera hello agent toomonitor data från krypterade anslutningar (djup kontroll) som vi nämnt tidigare. TLS ([Transport Layer Security](https://msdn.microsoft.com/library/windows/desktop/aa380516%28v=vs.85%29.aspx)) är en av hello vanligaste protokollen används på hello Internet idag. Genom att kryptera kommunikationen med TLS, kan en klient upprättar en säker och privat kommunikationskanal med en webbserver. TLS innehåller grundläggande skydd för att skicka autentiseringsuppgifter och förhindra hello avslöjande av känslig information.

Hello slutpunkt till slutpunkt säker krypterad kanal som tillhandahålls av TLS kan viktiga säkerhet och sekretess, missbrukat hello protokollet ofta för skadliga eller nefarious ändamål. Faktum är kallas att TLS är ofta så mycket så tooas hello ”universal-kringgående för brandväggar protokoll”. hello rot hello problemet är att de flesta brandväggar tooinspect TLS kommunikation eftersom hello programnivå data krypteras med SSL. Den här kunskapen kan använda angripare ofta TLS toodeliver skadliga nyttolaster tooa användaren säker på att även hello mest intelligent programnivå brandväggar är helt hemlig tooTLS och måste bara vidarebefordra TLS kommunikation mellan värdar. Slutanvändare utnyttja ofta TLS toobypass åtkomstkontroller tillämpas av deras företagets brandväggar och proxyservrar, använder den tooconnect toopublic proxyservrar och för icke-TLS-tunnelprotokoll hello-brandväggen som annars kan blockeras av en princip.

Djupinspektion tillåter hello Cloud App Discovery-agenten tooact som en betrodd man-in-the-middle. När en klientbegäran görs tooaccess en HTTPS-skyddad resurs, hello Endpoint Agent drivrutinen fångar upp hello anslutning och upprättar en ny anslutning toohello mål server tooretrieves dess SSL-certifikat för hello-klient. hello agent verifierar sedan att hello certifikatet kan vara betrott (genom att kontrollera att den inte har återkallats och utföra andra kontroller för certifikat) och om dessa klarar hello Endpoint Agent och sedan kopieras hello informationen från hello servercertifikat och skapar en egen servercertifikat – kallas även en avlyssning certifikat--med hjälp av informationen. hello avlyssning certifikatet är signerade på direkt av hello endpoint agent med ett rotcertifikat som är installerat i hello Windows betrodda certifikatarkivet. Den här självsignerade rotcertifikat markeras inte kan exporteras och är ACL hade tooadministrators. Den är avsedd toonever lämna hello dator har skapats. När klientprogrammet hello slutanvändarens får hello avlyssning certifikatet, den litar på den eftersom det kan har Validera hello certifikatkedja alla hello sätt toohello rotcertifikat. Den här processen är främst transparent från en slutanvändares synsätt med några varningar som beskrivs nedan.

Genom att aktivera djupinspektion hello Cloud App Discovery Endpoint Agent dekryptera och inspektera TLS krypterad kommunikation, så att hello service tooreduce brus och ger inblick i hello användning av hello krypterade molnappar.

#### <a name="a-word-of-caution"></a>En liten varning
Innan du aktiverar på djupinspektion, vi rekommenderar att du meddela dina avsikter tooyour juridiska och HR avdelningar och få sitt samtycke. Kontrollera slutanvändarens privata krypterad kommunikation kan vara känslig ämne, uppenbara. Innan en produktion lansering av djupinspektion, kontrollera att din företagets säkerhetsprinciper och principer för godkänd användning har uppdaterats ska tooindicate som krypterad kommunikation kontrolleras. Användarmeddelande och undantag för webbplatser som anses vara känsliga (t.ex. banker och medicinska webbplatser) kan också vara nödvändigt om du konfigurerar Cloud App Discovery toomonitor dem. Som nämnts ovan är administratörer kan använda hello Cloud App Discovery portal toochoose om toonotify användare av hello insamling av hello-agenten och huruvida toorequire användaren godkänna hello agenten börjar samla in användardata.

### <a name="known-issues-and-drawbacks"></a>Kända problem och nackdelar
Det finns några fall där TLS avlyssning kan påverka hello slutanvändarupplevelse:

* Certifikat för utökad (Validation) återge hello adressfält hello web webbläsare grön tooact som se att du besöker en webbplats som betrodd. TLS-kontroll kan inte duplicera EV i hello certifikat den utfärdar toohello klienten, så att webbplatser som använder EV-certifikat fungerar normalt men hello adressfältet visas inte grönt.  
* Offentlig nyckel fästning (även kallat certifikat fästning) är utformade toohelp skyddar användare från man-in-the-middle-attacker och obehöriga certifikatutfärdare. När hello rotcertifikatet för en fast plats inte matchar någon av hello kända fungerande Certifikatutfärdare, avvisar hello webbläsare hello anslutning med ett fel. Eftersom TLS avlyssning är faktiskt en man-in-the-middle, dessa anslutningar kommer att misslyckas.
* Om användare klickar på hello låsikonen i hello webbläsare adress fältet webbläsare tooinspect hello platsinformation kan se de inte en kedja som slutar på hello certifikatutfärdare används toosign hello webbplatscertifikat, men i stället en certifikatkedja som slutar med hello Windows betrodda certifikatarkivet.

tooreduce hello förekomster av de här problemen, vi håller reda på molntjänster och klientprogram kända toouse utökad validering eller offentliga nyckel fästning och instruera hello Endpoint Agent tooavoid avlyssna berörda anslutningar. Även i dessa fall kan dock får du ändå rapporter om hello användning av dessa molnappar och hello mängden data som överförs men eftersom de inte djupgående kontrolleras ingen information om hur hello appar användes blir tillgängliga.

## <a name="sending-data-toocloud-app-discovery"></a>Skicka data tooCloud App Discovery
När metadata har samlats in av hello-agenten, cachelagras på hello datorn upp tooone minut eller tills hello cachelagrade data når en storlek på 5MB. Den sedan komprimeras och skickas via en säker anslutning toohello Cloud App Discovery-tjänsten.

Om hello-agenten är toocommunicate med hello Cloud App Discovery-tjänsten av någon anledning, lagras hello insamlade metadata i en lokal fil-cache som bara kan användas av Privilegierade användare på hello datorn (till exempel hello administratörsgruppen).  
hello agent automatiskt försök tooresend hello cachelagrade metadata förrän den har tagits emot av hello Cloud App Discovery-tjänsten.

## <a name="receiving-hello-data-at-hello-service-end"></a>Ta emot hello data hello service slutet
hello agenter autentisera toohello Cloud App Discovery-tjänsten använder hello datorn specifika certifikat för klientautentisering anges ovan och vidarebefordrar data via en krypterad kanal.  
hello Cloud App Discovery service analytics försäljningsförlopp processer metadata för varje kund separat per logiskt resurspartitionering den alla led i hello analytics pipeline.
hello analyseras metadata enheter hello olika rapporter i hello-portalen.

Hej obearbetat metadata och hello analyseras metadata lagras för in too180 dagar. Kunder kan dessutom välja toocapture hello analyseras metadata i ett Azure blob storage-konto de önskar.
Detta är användbart för offlineanalys av metadata samt längre kvarhållning av hello data.

## <a name="accessing-hello-data-using-hello-azure-portal"></a>Komma åt hello data med hello Azure-portalen
I en ansträngning tookeep hello metadata som samlas in säker, som standard endast globala administratörer hello-klient har åtkomst toohello Cloud App Discovery-funktionen i hello Azure-portalen.  
Administratörer kan dock välja toodelegate denna åtkomst tooother användare eller grupper.

> [!NOTE]
> Mer information finns i [komma igång med Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)
> 
> 


Åtkomst till hello användardata i hello-portalen måste licensieras med en Azure AD Premium-licens.

## <a name="additional-resources"></a>Ytterligare resurser
* [Hur kan identifiera ej sanktionerade molnappar som används inom organisationen](active-directory-cloudappdiscovery-whatis.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)

