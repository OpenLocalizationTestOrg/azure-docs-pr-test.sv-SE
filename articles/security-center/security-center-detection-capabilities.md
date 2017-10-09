---
title: aaaDetection funktioner i Azure Security Center | Microsoft Docs
description: "Det här dokumentet hjälper dig att toounderstand hur funktionerna för identifiering av Azure Security Center fungerar."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 4c5599cc-99a1-430f-895f-601615ef12a0
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: d8001cc2acdd0026bd9b3596bbdfec56f8874513
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-detection-capabilities"></a>Identifieringsfunktioner i Azure Security Center
Det här dokumentet beskriver funktioner för Azure Security Center avancerad identifiering, vilket hjälper till att identifiera active hot målobjekt för Microsoft Azure-resurser och ger dig med hello insikter behövs toorespond snabbt.

> [!NOTE]
> Avancerade identifieringar finns i hello Standard nivå av Azure Security Center. En kostnadsfri 60-dagars utvärderingsversion är tillgänglig. Du kan uppgradera från hello prisnivån val i hello [säkerhetsprincip](security-center-policies.md). Besök [Security Center sidan](https://azure.microsoft.com/pricing/details/security-center/) toolearn mer om prissättning. 
> 
> 

## <a name="responding-tootodays-threats"></a>Svara tootoday's hot
Det har betydande förändringar i hello hotbild över hello senaste 20 år. Hello tidigare hade företag vanligtvis bara tooworry om webbplatsen defacement enskilda användare som har mest intresserad av att se ”vad de kan göra”. Dagens angripare är mycket mer sofistikerade och organiserade. De har ofta specifika ekonomiska och strategiska mål. De har också mer resurser tillgängliga toothem, som de kan grundval av landet eller i organiserad crime.

Den här metoden har lett tooan oöverträffad nivå ge i hello angripare rangordningar. Dagens angripare är inte intresserade av att bara vanställa en webbplats. De är nu intresserad av att stjäla information, finansiella konton och privata data – som de kan använda toogenerate kontanta hello öppna marknaden eller tooleverage ett visst företag, politiska eller militära position. Ännu mer om än de angriparna med ett mål för finansiella hello angripare som bryter mot nätverk toodo skada tooinfrastructure och personer.

Organisationerna distribuerar ofta olika punktlösningar som fokuserar på försvara hello enterprise perimeternätverket eller slutpunkter genom att söka efter kända attacker signaturer som svar. Dessa lösningar tenderar toogenerate en stor volym med låg återgivning aviseringar som kräver en analytiker tootriage för säkerhet och undersöka. De flesta organisationer saknar hello tid och kunskaper som krävs för toorespond toothese aviseringar – så många går oåtgärdade.  Under tiden kan angripare har utvecklats sina metoder toosubvert många signatur-baserade försvar och [anpassa toocloud miljöer](https://azure.microsoft.com/blog/detecting-threats-with-azure-security-center/). Nya metoder är obligatoriska toomore snabbt identifiera nya hot och påskynda identifiering och svar. 

## <a name="how-azure-security-center-detects-and-responds-toothreats"></a>Hur Azure Security Center identifierar och svarar toothreats
Microsofts ständigt på hello Håll utkik efter hot. De har åtkomst tooan omfattande uppsättning telemetri erfarenheterna från Microsofts global närvaro i hello molnet och lokalt. Wide når och skilda samlingen av datauppsättningar kan Microsoft toodiscover nya angreppsmönster och trender i dess lokala konsument- och företagsinriktade produkter samt online-tjänster. Detta gör i sin tur att identifieringsalgoritmerna i Security Center snabbt kan uppdateras allt eftersom angripare hittar nya och alltmer sofistikerade sätt att utnyttja systemen. Och för dig som kund innebär det att du kan hålla jämna steg med dagens snabbt föränderliga hotmiljö. 

Hotidentifiering Security Center fungerar genom att automatiskt samla in säkerhetsinformation från Azure-resurser, hello nätverket och anslutna partnerlösningar. Den här informationen kan ofta korrelerar information från flera källor, tooidentify hot analyseras. Säkerhetsaviseringar prioriteras i Security Center tillsammans med rekommendationer om hur tooremediate hello hot.

![Datainsamling och datapresentation i Security Center](./media/security-center-detection-capabilities/security-center-detection-capabilities-fig1.png)

Security Center använder avancerade säkerhetsanalyser, som går mycket längre än signaturbaserade lösningar. Genombrott i stordata och [maskininlärning](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) tekniker är balanserad tooevaluate händelser över hello hela molninfrastrukturresurs – upptäcka hot som skulle vara omöjligt tooidentify med manuella metoder och förutsäga hello utvecklingen av attacker. Dessa säkerhetsanalyser omfattar: 

* **Integrerad hotinformation**: ser ut för kända obehöriga genom att utnyttja globala hotinformation från Microsoftprodukter och tjänster, hello Microsoft Digital Crimes Unit (DCU), hello Microsoft Security Response Center (MSRC) och externa feeds.
* **Beteendeanalys**: gäller kända mönster toodiscover skadligt beteende. 
* **Avvikelseidentifiering**: använder statistiska toobuild utgångspunkt historisk profilering. Varnar på avvikelser från etablerade baslinjer som följer tooa potentiella angrepp.

### <a name="threat-intelligence"></a>Hotinformation
Microsoft har tillgång till en enorm mängd global hotinformation. Telemetri flödar i från flera källor, till exempel Azure, Office 365, Microsoft CRM online, Microsoft Dynamics AX, outlook.com, MSN.com, hello Microsoft Digital Crimes Unit (DCU) och Microsoft Security Response Center (MSRC). Forskare får även information om hot från som delas med större molntjänst-providers och prenumererar toothreat intelligence feeds från tredje part. Azure Security Center kan använda denna information tooalert du toothreats från kända obehöriga. Några exempel är:

* **Utgående kommunikation tooa skadliga IP-adressen**: utgående trafik tooa kända botnät eller darknet sannolikt tyder på att resursen har komprometterats angriparen den försöker tooexecute kommandon på dessa system eller exfiltrate data. Azure Security Center jämför nätverket trafik tooMicrosoft globala hot databasen och varnar dig om kommunikation tooa skadliga IP-adress.

## <a name="behavioral-analytics"></a>Beteendeanalys
Beteendeanalys är en teknik som analyserar och jämför tooa insamling av kända mönster. Dessa mönster är dock inte enkla signaturer. De är definieras via komplexa maskininlärningsalgoritmer som är kopplade toomassive datauppsättningar. och av expertanalytiker genom noggrann analys av skadliga beteenden. Azure Security Center kan använda beteendeanalys tooidentify komprometteras resurser baserat på analys av loggar för virtuell dator, virtuella nätverk enhetsloggar, fabric loggar, krascher Dumpar och andra källor. 

Det finns dessutom korrelation med andra signaler toocheck för underlag för en omfattande kampanj. Den här korrelation hjälper tooidentify händelser som är konsekventa med etablerade indikatorer för angrepp. Några exempel är:

* **Körningen av misstänkt processen**: angripare använda flera tekniker tooexecute skadlig programvara utan identifiering. Till exempel att en angripare kan ge skadlig kod hello samma namn som legitima systemfiler men placera dessa filer på en annan plats, Använd ett namn som är mycket lik tooa ofarlig fil eller mask hello true filtillägget. Bearbeta körningar toodetect avvikare som dessa Security Center modeller processer beteenden och Övervakare.  
* **Dold skadlig kod och utnyttjande försök**: avancerad skadlig kod är kan tooevade traditionella antivirusprodukter genom att antingen skriva toodisk aldrig eller kryptera programvarukomponenter som lagras på disk.  Dock kan sådana skadlig kod identifieras med hjälp av minnesanalys som hello skadlig kod måste lämna spårningar i minnet i ordning toofunction. När programmet kraschar avbildar en krasch-dump en del av minnet när hello hello krascher.  Genom att analysera hello minne i hello kraschdump Azure Security Center kan identifiera metoder som används för tooexploit sårbarheter i program, komma åt känsliga data och smyg sparas med-i en komprometterad dator utan att påverka hello prestanda din dator.
* **Lateral förflyttning och interna rekognosering**: toopersist i en komprometterad nätverks- och leta upp/skörd värdefulla data angripare försöka ofta toomove sidled från hello komprometteras datorn tooothers inom hello samma nätverk. Security Center övervakar processen och logga in aktiviteter i ordning toodiscover försöker tooexpand en angripare fot inom hello nätverk, till exempel fjärrkommandot körning nätverksåtkomst, och kontouppräkning.
* **PowerShell-skript**: PowerShell som används av angripare tooexecute skadlig kod på virtuella datorer som mål för en rad olika syften. Security Center inspekterar PowerShell-aktivitet för att hitta tecken på misstänkt aktivitet. 
* **Utgående attacker**: angripare ofta mål molnresurser med hello målet med hjälp av dessa resurser toomount ytterligare attacker. Angripna virtuella datorer, till exempel kan vara används toolaunch brute force-attacker mot andra virtuella datorer, skicka skräppost eller skanna öppna portar och andra enheter i hello internet. Genom att använda machine learning toonetwork trafik Security Center kan identifiera när utgående nätverkskommunikation överskrider hello normen. Hello gäller skräppost, Security Center också korrelerar ovanliga e-trafik med information från Office 365 toodetermine om hello e-post är sannolikt nefarious eller hello resultatet av en giltig e-kampanj.  

### <a name="anomaly-detection"></a>Avvikelseidentifiering
Azure Security Center använder också avvikelseidentifiering identifiering tooidentify hot. I kontrast toobehavioral analytics (som beror på kända mönster som härletts från stora datamängder), avvikelseidentifiering är mer ”anpassad” och fokuserar på baslinjer som är specifika tooyour distributioner. Machine learning är tillämpade toodetermine normal aktivitet för dina distributioner och sedan regler är genererade toodefine avvikare förhållanden som kan representera en säkerhetshändelse. Här är ett exempel:

* **Inkommande råstyrkeattacker med RDP/SSH**: Dina distributioner kan ha upptagna virtuella datorer med ett stort antal inloggningar varje dag och andra virtuella datorer som bara har ett fåtal om ens någon inloggning. Azure Security Center kan fastställa baslinjen inloggningsaktivitet för dessa virtuella datorer och använda machine learning toodefine vad är utanför normal inloggningsaktivitet. Om hello antal inloggningar eller hello tid på dagen då hello inloggningar eller hello plats från vilken hello inloggningar krävs eller andra inloggnings-relaterade egenskaper skiljer sig avsevärt från hello baslinje, kan en avisering genereras. Maskininlärningen avgör vad som är viktigt.

## <a name="continuous-threat-intelligence-monitoring"></a>Kontinuerlig övervakning av hotinformation
Azure Security Center fungerar säkerhet forskning vetenskap team som kontinuerligt övervakar ändringar i hello hotbild. Detta omfattar följande initiativ hello:

* **Övervakning av hotinformation**: Hotinformation inbegriper mekanismer, indikatorer, effekter och rekommendationer relaterade till befintliga eller nya hot. Den här informationen delas i hello säkerhet community och Microsoft övervakar kontinuerligt hot intelligence feeds från interna och externa källor.
* **Informationsdelning**: Säkerhetsteamen som arbetar med Microsofts breda utbud av molnbaserade och lokala tjänster, servrar och enheter för klientslutpunkter delar och analyserar sina insikter. 
* **Microsofts säkerhetsexperter**: Kontinuerligt arbete i team inom hela Microsoft som arbetar inom specialiserade säkerhetsområden, exempelvis datautredning och identifiering av webbattacker.
* **Justera identifiering**: algoritmer körs mot verkliga kunden datauppsättningar och säkerhet forskare arbeta med kunder toovalidate hello resultat. SANT och FALSKT positiva identifieringar kan använda toorefine maskininlärningsalgoritmer.

Dessa kombinerade ansträngningar avslutas nya och förbättrade identifieringar som du kan dra nytta av omedelbart – åtgärd ingen för du tootake.

## <a name="see-also"></a>Se även
I det här dokumentet du har lärt dig hur tooAzure Security Center identifiering funktioner fungerar. toolearn mer om Security Center finns hello följande:

* [Planerings- och bruksanvisning för Azure Security Center](security-center-planning-and-operations-guide.md)
* [Hantera och åtgärda toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Säkerhetsaviseringar per typ i Azure Security Center](security-center-alerts-type.md)
* [Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.
* [Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.
* [Vanliga frågor om Azure Security Center](security-center-faq.md) – finns vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) – Här hittar du blogginlägg om säkerhet och regelefterlevnad i Azure.

