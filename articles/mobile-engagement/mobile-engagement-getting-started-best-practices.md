---
title: "aaaAzure Mobile Engagement komma igång med bästa praxis"
description: "Komma igång med Azure Mobile Engagement och bästa praxis för integrering"
services: mobile-engagement
documentationcenter: mobile
author: wesmc7777
manager: erikre
editor: 
ms.assetid: dfce1183-6398-466e-aa7e-ed702fb52818
ms.service: mobile-engagement
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 10/04/2016
ms.author: wesmc;ricksal
ms.openlocfilehash: d3f81c34fe74f741a60ca511caa55c312af73b1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement---getting-started-guide-with-best-practices"></a>Azure Mobile Engagement – Komma igång med bästa praxis
## <a name="overview"></a>Översikt
**mobila hello-skärmen är mycket överfulla blanksteg:** i 2013, hitta en undersökning hello genomsnittlig mobilenheter hade 27 appar installerade. Användarna lade i snitt ned 30 timmar i månaden på apparna. Största delen av tiden tillbringades på sociala nätverk och med spel (cirka 20 timmar). År 2014 fanns hello Android-marknaden runt 1,5 miljoner appar för användare toochoose från. hello Apple store innehöll då runt 1,2 miljoner appar. Användningen av mobilappar bara fortsätter att öka och konkurrensen mellan de olika apputvecklarna är stenhård. 

hello genomsnittlig mobilanvändare installerar och avinstallerar appar med hög frekvens beroende på om du ändrar intressen och upplevelser i appen. I ordning toodetermine hello framgångsrik en app blir den viktiga tooknow mer än bara hur många användare som installerar appen. Det är viktigt tooknow hur användbar din app är och om den trendbrott i användningsmönstret. hello följande frågor blir viktiga:

* Användarna börjar toofind appen ointressanta eller föråldrad? 
* Hur många användare har helt slutat använda appen? 
* Ökar eller minskar antalet köp i appen?
* Användare misslyckas toocomplete arbetsflöden på grund av problem med hello app eller bristande intresse? 
* Kan du behålla din app användbarhet och relevans genom att trycka på grundläggande ny innehåll tooyour användare? 
* Nya innehåll skulle bli hello samma för alla användare eller fokuserar på användarsegment baserat på beteende i appen? 

Svaren tooquestions liknande toothese kan hjälpa till att utöka hello livslängd och maximera intäkterna från din app. De kan också vara till god hjälp när du ska definiera och behålla användarbasen. 

Medierelaterade appar har ofta toohave vissa hello högsta kvarhållning av användare. En anledning till detta är de ständigt tillhandahåller ny innehåll toousers. Tidigt införande skicka push-meddelanden dirigeras tooa användarsegment tenderar toohave effekt på kvarhållningen. 

hello Azure Mobile Engagement-programmet är utformad toohelp du utöka hello livslängd och hålla kvar av din app genom att tillhandahålla en metod toogather och analysera detaljerad information om hello använder din app. Det hjälper dig att klassificera din användarbas enligt toobehavior och skapa riktade kampanjer för att leverera push-meddelanden och meddelanden i appen tooidentified användarsegment. Med KPI:er (Key Performance Indicators) mäter du hur aktiva användarna är inom olika områden i appen. Azure Mobile Engagement tillhandahåller metoderna hello toodetermine måste dessa KPI: er. Det ökar hello avkastning på investeringen (ROI) genom att tillhandahålla hello infrastruktur du behöver tooincrease engagement med din mobilapp. 

I ordning tooget hello mesta möjliga av Azure Mobile Engagement behöver du toostart med en väl avvägd plan. Planen hjälper dig identifiera hello detaljerade data som du behöver toobe kan toosegment användaren bas. Ofta baseras detta på användarnas beteende och upplevelser i appen. För att din plan toobe lyckas är det en bästa praxis tooclearly definiera hello KPI: er som mäter hello målen för din app. Med tydliga nyckeltal, du kan enkelt att bädda in hello nödvändiga logiken i appen toocollect bra metataggkontroll data som du ska använda tooanalyze och utvärdera dina KPI: er. Det här avsnittet är en guide för bästa praxis för att definiera hello KPI: er som du vill använda med ingå i planen. 

## <a name="step-1-define-your-kpis-toofit-hello-bet-model"></a>Steg 1: Definiera dina KPI: er toofit hello BET-modellen
Korrekt definiera KPI: er kan vara en svår uppgift toocomplete. Appar för olika branscher har sina egna krav och mål. Tyvärr kan detta tooconfuse ditt arbete. toohelp Undvik detta genom mål och KPI: er klassificeras enligt tre huvudkategorier: **företag**, **Engagement**, och **tekniska**. Det här är vad vi kallar hello **BET-modellen**.

En bra plan har vanligtvis tydliga mål med hello KPI: er som mäter framgången hello i hello följande kategorier av hello BET-modellen.

#### <a name="business-kpis"></a>KPI:er för företag
KPI: er ska vara hello enklaste del toobuild. Du har förmodligen redan definierat sådana i någon form när du planerade din mobilapp. Dessa KPI:er hjälper dig att mäta intäkter och avkastning för appen. hello innehåller följande lista några exempel på KPI: er som kan hjälpa dig när du definierar dina nyckeltal:

* KPI:er för medieföretag
  * Antal annonsklickningar
  * Antal sidbesök per användare
  * Antal prenumeranter för tillfället
* KPI:er för spelföretag
  * Antal köp i app
  * Genomsnittliga intäkter per användare (ARPU)
  * Tid per session
  * Antal speldagar och aktuell nivå i spelet
* KPI:er för e-handel
  * Antal dagar som appen har använts
  * Genomsnittliga intäkter per användare (ARPU)
  * Genomsnittligt belopp per kundvagn i kassan
  * Produktkategori med flest visningar och köp
* KPI:er för banker och försäkringsbolag
  * Antal konton
  * Funktioner som aktiverats
  * Besökta erbjudandesidor
  * Klickade eller aktiverade aviseringar/meddelanden       

#### <a name="engagement-kpis"></a>KPI:er för användarinteraktion
Ett KPI: er är en prestanda indikator toomeasure hello engagerade dina användare. Trender i det här området att avgöra hello kvarhållning för din app. Här är några exempel på nyckeltal för den här typen av KPI:

* Aktiva användare i hello senaste 7 dagarna
* Inaktiva användare under hello senaste 7 dagarna
* Antal användare som inte har använt hello app i 30 dagar  

Vissa uppenbara externa faktorer kan så klart påverka indikationerna inom det här området. Exempelvis kan du toobe en mobil enhet med en användare vid alla tidpunkter. Detta kan dock variera. En spelappar används toohave högre användning runt helgdagar när spelarna mer arbete eller skola.   

Väldefinierade KPI: er i den här kategorin hjälper dig att mäta hello förhållandet mellan din app och dina kunder.

#### <a name="technical-kpis"></a>KPI:er för teknik
Nyckeltal i den här kategorin kan hjälpa dig att avgöra om din app fungerar som den ska eller om den hänger sig eller kraschar. Dessa indikatorer mäta hello hälsotillståndet för din app och identifierar användbarhetsproblem som hindrar användare från att använda hello app. Information som samlas in för den här kategorin kan också innehålla nyckeltal som är relevanta toomarketing team. hello data kan också vara användbar vid felsökning av IT och support team toohelp identifiera okända programfel. 

Här följer några exempel på KPI:er för teknik:

* Antal ohanterade eller hanterade undantag och information om dem 
* Tidsstämpel för senaste krasch
* Senast klickade knapp eller senast besökta sida
* Mängden minne på hello app
* Appens bildfrekvens
* OS-version som hello appen körs på
* Appversion

Definiera dessa KPI: er toohelp mäta appens prestanda och identifiera potentiella programfel. Den här indikatorer ska minska hello gång du toodeliver en korrigering för kunderna. De kan också hjälpa dig att definiera ett användarsegment som har stött på ett specifikt problem. Du kan använda att återställa användaren segmentering toocreate kampanjer toodeliver meddelanden om nya korrigeringsfiler och eventuella kampanjer toohelp nöjda. 

#### <a name="playbook-exercise-1-create-your-kpi-dashboard"></a>Strategibok – övning 1: Skapa en KPI-instrumentpanel
När du definierar en marknadsföringsstrategi ska KPI:erna resultera i en vy för vart och ett av huvudmålen. De bör vara tydligt definierade datapunkter som gör att du toocollect viktig information toomonitor din app och hello beteendet för hello slutanvändaren.

Skapa en KPI-instrumentpanel som innehåller hello informationen nedan

1. Vad är hello KPI: er för hello app?
2. Vilka datapunkter ska jag använda toorepresent varje KPI?
3. Var finns dessa data i appen (skärm, inställningar, system o.s.v.)?
4. Kan jag köra en interaktionssekvens för denna KPI?

Du kan använda hello **KPI Builder** kalkylblad i vår [Media Playbook mallen] [ Media Playbook link] exempel och vägledning.

## <a name="step-2-your-engagement-program"></a>Steg 2: Ett program för användarinteraktion
Ett bra Mobile Engagement-program bör betraktas som en viktig del av din app. Det måste absolut innehålla ett bra välkomstprogram som körs för en användare under hello första dagarna av appanvändningen. Detta brukar toohave en mycket positiv inverkan på engagement och kvarhållning för din app. Studier har visat att hello merparten av användare att sluta använda hello en app några dagar efter installationen. Du vill toostrive toomeet eller överskrida kundens förväntningar med intresseväckande innehåll tidigt när hello användaren fortfarande fokuserar på din app. Kontrollera att du presentera hello värdefullaste funktionerna och fördelarna med din app tooyour kunder. 

![](./media/mobile-engagement-getting-started-best-practices/unsegmented-push-notifications.png)

Push-meddelanden är hello bästa metoden tooearly Användarsegmentet med användare av mobila enheter. Dock måste du vara försiktig när du segmenterar användare för mottagande av push-meddelanden. Om användarna tycker att de får skräppost eller ointressanta meddelanden kan det få allvarliga följder. Några få klickningar kan en användare kan ta bort programmet aldrig tooreturn. hello användaren ska få personanpassat mervärde från appen – i stället för allmänt hållna massutskick.

När användarna har aktiverats och blivit engagerade hjälper ditt program för användarinteraktion enhet andra aspekter av hello app.

Du kan till exempel konfigurera en kampanj som ber aktiva användare-toorate din app. Eftersom detta användarsegment hello mest aktiva och har de flesta erfarenhet av du appen för hello, du då förvänta toogive hello relevanta betyg. När du har höga betyg kan hjälpa enhet hello organisk hämtning av din app som också minskar kostnaderna för din nya kunden att förvärva.

#### <a name="engagement-sequence"></a>Interaktionssekvenser
I varje globalt program för användaraktion ska det finnas interaktionssekvenser. Varje sekvens syftar tooreach flera mål.

###### <a name="life-push-sequence"></a>Sekvens med push-meddelanden vid en viss tidpunkt i livscykeln
hello målen för den här typen av sekvens skiljer sig beroende på hello livscykeln för hello användarens interaktion med hello app. En användare kan vara ny, inaktiv eller mycket aktiv. Under de olika faserna i livscykeln kan användarna få nytt innehåll i hello form av tips eller länkar toodocumentation. 

Till exempel kan en ny användare behöver hjälpa komma objektorienterad tooan app eller dra nytta av en ny stimulansordningen liknande användaren-toohello som är följande hello första gången de starta hello appen...

*”Glad toohave dig att komma igång! Kom ihåg toologin tooget din 1 månad kostnadsfritt ”!*

###### <a name="behavioral-push-sequence"></a>Sekvens med beteendebaserade push-meddelanden
Hej beteendebaserade push sekvens syftar tooincrease användningen och bygger på användarbeteende som samlats in hello app.  

Till exempel en mycket aktiv användare av en fotbollsapp kan ha nytta av att få följande push-meddelande hello...

*”Niklas, du är verkligen en seriös fotbollssupporter! Logga in tooour Allsvenska och vinna tillträde toohello Finalmatchen ”!*

###### <a name="alerting-push-sequence"></a>Sekvens med push-meddelanden i form av aviseringar eller meddelanden
Användarna uppskattar normalt att få nyheter som har med deras intressen att göra. En sekvens med push-meddelanden i form av aviseringar eller meddelanden ökar i regel användarinteraktionen om de bygger på verkliga intressen. Detta kan vara explicit när en användare väljer sina egna intressen i hello app. Det kan också underförstådda intressen baserade på data som samlas in under användarinteraktion med hello app.

Till exempel köper hello användare av en E-handel app regelbundet ett visst kaffemärke med jämna som du har skapat med ett KPI för företag. hello kan följande avisering förbättra den här användarens interaktion med hello app.

*”Hej max en av dina favoritkaffemärken under görs på försäljning 25% rabatt hello första veckan i September 2015. Vi uppskattar du som kund och vill toomake att du var medveten om ”.*

###### <a name="rentention-push-sequence"></a>Sekvens med push-meddelanden för ökad kvarhållning
Den här aktivitetssekvensen mål tooretain användare med hjälp av en återkommande push notification kampanjer toohelp driva en rutinmässig in med hello app. Detta kan hjälpa ökad kvarhållning om hello användaren gillar hello interaktioner. 

Hello användare av en sportapp kan till exempel få hello följande push-meddelande varje vecka utifrån hello användarens favoritlag:

*”Chansen-toowin 200 poäng finns rösta om hello New York Yankees kommer vinner denna vecka mot Stockholmsderbyt blå Jays”!*

#### <a name="hello-3w-approach"></a>hello 3W-metoden
Hantering av hello olika push sekvenser hjälper dig att öka interaktionen med slutanvändare. Behöver du dock fortfarande toouse hello 3W-metoden för att anpassa dina meddelanden. hello 3W-metoden bör hantera vem, vad och när för varje meddelande. Om du kan ge tillfredsställande svar på dessa tre frågor är du redo för interaktion med rätt fokus.

![](./media/mobile-engagement-getting-started-best-practices/who-what-when.png)

###### <a name="who-hello-user-segment-that-will-receive-messages"></a>Som: hello användarsegment som ska ta emot meddelanden
Skicka meddelanden tooyour användare bör övervägas en mycket känslig kommunikationskanal. Kontrollera hello-meddelanden som du riktar toosend tooa användarsegment är väl begränsade toohello intressen att användarsegment. Ett felaktigt irrelevanta har utskick troligen toohave en negativ inverkan på en användare. De kan överväga den skräppost som inledande tooyour appen avinstalleras. 

Använd en kombination av specifika tekniska och beteendebaserade kriterier när du definierar det användarsegment som ska ta emot meddelandet. Ett enkelt exempel definiera ett användarsegment kan vara liknande toohello följande instruktion:

”Alla användare som startade hello ett mobilt program för hello första gången 3 dagar sedan och som har besökt inloggningssidan hello två gånger utan att faktiskt logga in”.

Denna instruktion hjälper dig identifiera hello data behöver du toocollect toosupport ett specifikt scenario.

###### <a name="what-hello-message-that-you-will-send"></a>Vad: hello-meddelande som ska skickas
**Lämplig ton**

Använd en lämplig ton för interaktionen och användarsegmentet i fråga. Detta definitivt är ett bra sätt tooconnect med dina slutanvändare och befordra en användares intresse för din app. 

**Omdirigering**

Ett push-meddelande kan användas för fler än öppnandet av programmet hello. Om hello har meddelandet samband med en nyhet eller ett produkterbjudande kan det här meddelandet kan djuplänk direkt toohello Högerklicka innehåll i programmet hello. toosupport, måste du skapa en URL schemat toolet hello programmet Hantera hello omdirigering. Det här är ett mycket viktigt steg som inte får glömmas bort när du utarbetar interaktionssekvenser.

Omdirigering kan också hanteras för andra system. Till exempel med en åtgärds-URL är det möjligt tooredirect slutanvändare toomany andra system, inklusive hello följande:

* En webbplats
* En e-postlåda som redan har konfigurerats
* En SMS-inkorg
* En fjärrtjänst
* Direkt toohello appbutiken för att betygsätta hello program. 

Detta skapar många tillfällen tooengage slutanvändarna och skapar automatiska regler tooimprove prestanda.

**Format/innehåll**

Olika typer och format för push-meddelanden: 

1. **Meddelanden** : gör toosend reklam meddelanden toousers på olika tag (utanför appen i appen eller när som helst).
2. **Omröstningar** : aktiverat toogather information från slutanvändarna genom att ställa frågor till dem.. Svaren är sedan tillgängliga när du skapar kriterier tootarget slutanvändare.
3. **Data-push** : gör att du toosend en binär fil eller base64-data filen tooupdate hello app. hello informationen i en data-push skickas tooyour programmet toopersonalize hello användarnas upplevelse i din app. Programmet måste toobe utformats toosupport hello data i en data-push.
4. **Paneler (endast Windows Phone)** : aktiverat toouse hello Microsoft Push Notification Service (MPNS) toosend systemspecifika Push-meddelanden med XML-Data (stöds sedan SDK version 0.9.0. hello slutlig nyttolast för panelerna får inte överstiga 32 kB.). hello-meddelande visas direkt på panelen.
5. **Webbvy**: En webbvy är ett popup-fönster med webbinnehåll. Det här popup-fönster visas när hello slutanvändaren har klickat på hello push-meddelande. En webbvy möjliggör toohave mer interaktion med hello slutanvändaren.

> [!NOTE]
> Kontrollera att hello-innehåll som du skickar som push-meddelanden följer hello respektive plattforms (iOS, Android, Windows) riktlinjer för att utveckla appar och skicka push-meddelanden.
> 
> 

###### <a name="when-hello-timing-of-your-campaign"></a>När: hello tidpunkten för kampanjen
När är hello bästa tid tooactivate en kampanj som utlöser push-meddelanden? Ska detta ske manuellt eller automatiskt? Ska det vara återkommande? Bestämma hello rätt tidpunkt eller frekvens är viktigt tooengage användare med hello bästa resultat. För varje interaktionssekvens och scenario måste du ange när blir hello bästa tid toosend push-meddelanden. Här är några exempel:

![](./media/mobile-engagement-getting-started-best-practices/campaign-timing-examples.png)

Om du skickar många meddelanden varje dag måste du ta en allvarlig funderare på om det kan finnas användare som ser dina utskick som skräppost. 

Azure Mobile Engagement tillhandahåller två sätt toohelp undvika att få meddelanden klassade som skräppost. Använd först extremt detaljerad segmentering tooensure du göra inte målet hello samma användare. Azure Mobile Engagement innehåller dessutom en ”kvotfunktion”. Med den funktionen kan du begränsa antalet meddelanden som skickas inom ramen för en och samma kampanj. Till exempel säkerställer ange en standard kvoten too5 per vecka att användare som en del av hello kampanjens användarsegment tar emot mer än 5 meddelanden den veckan.

#### <a name="playbook-exercise-2-create-your-engagement-program"></a>Strategibok, övning 2: Skapa ett program för användarinteraktion
Ägna en stund sammanfatta dina mål och definiera hello kampanjer som du förväntar dig tooplay med specifika sekvenser. Kontrollera att du använder hello 3W-metoden toohello meddelanden i kampanjerna. 

Använd hello **Program för användarinteraktion** kalkylblad i hello [Media Playbook mallen] [ Media Playbook link] exempel och vägledning.

## <a name="step-3-app-integration"></a>Steg 3: Appintegrering
#### <a name="create-a-tag-plan"></a>Skapa en taggplan
toointegrate Azure Mobile Engagement i din app behöver du toocreate en taggplan. Hej taggplanen är hello hörnsten i hello-projekt. Den definierar hello relationen mellan marknadsföringsspecifikationer, hello arbetsflöde av programmet hello och hello verkliga taggdata som samlats in i hello app toomeasure KPI: er. Anger vilka analyser du kunna toosee hello-portalen. Det hjälper dig också definiera användarsegment och skicka riktade push-meddelanden tooengage dina slutanvändare. När du har definierat taggplanen hello är lägger till hello kod toointegrate i din app det enkelt med hello Azure Mobile Engagement SDK.

En taggplan bör inte tagga precis allt i en app. Den ska bara innehålla taggdata som är del av din Mobile Engagement-strategi. Strategierna kan dock skifta mellan de olika apparna. Hej [Media Playbook mallen] [ Media Playbook link] under förutsättning av Azure Mobile Engagement hjälper dig att skapa en egen taggplan med valfri metod. Använd hello **Taggplanen** kalkylbladet som en guide toobuilding din taggen planera.

När du definierar ett taggavsnitt i kalkylbladet för hello vara mycket specifik. Detta är mycket viktigt tooavoid förvirring. Ange detaljerad information om de scenarier som ska utlösa en taggsändning. Inkludera hello namnet på hello aktivitet där olika taggarna är inbäddade. Detta måste inkluderas i hello **Informationsdelen** tillhör hello kalkylblad. hello kalkylbladet med taggplanen ska hello huvudsakliga referensen under testverifieringen. 

I hello **Data toocollect** avsnittet Utvecklingsteamet ska hitta hello typer, namn, värden och extra information nyckel/värde-par som krävs för varje tagg som ska bäddas in hello program.

Vi rekommenderar att du granskar hello taggplanen alla team som är associerade med hello-projekt. Gör alla nödvändiga ändringar och skicka en bekräftelse på att allt är klart till marknadsförings- och utvecklingsavdelningarna.

Hej **specifikation** kalkylblad kan vara används toohelp guida alla inblandade hello-projekt.

#### <a name="data-types"></a>Datatyper
Här följer några vanliga datatyper som stöds av Azure Mobile Engagement.

###### <a name="devices-and-users"></a>Enheter och användare
Azure Mobile Engagement identifierar användare genom att generera ett unikt ID för varje enhet. Detta ID kallas enhets-ID för hello (eller deviceid). Den skapas så att alla program som körs på samma enhet resursen hello samma enhetsidentifierare för.

###### <a name="sessions-and-activities"></a>Sessioner och aktiviteter
En session är en instans av hello appen som körs av en användare. hello sessionen sträcker sig från hello tid hello användaren startar hello app tills det slutar.

En aktivitet är en logisk gruppering av en uppsättning saker hello appen kan göra under en session. Det är vanligtvis en viss skärm i hello app, men det kan vara något definieras av hello logiken för programmet hello. Du bör åtminstone tagga alla skärmar eller aktiviteter i din app. Detta gör att du toounderstand hello användarsökvägen.

###### <a name="events"></a>Händelser
Händelser är används tooreport användarinteraktion med hello app. Det kan till exempel röra sig om omedelbara åtgärder, som att dela innehåll eller spela upp en video. Genom att tagga händelser ger du datasamlingar som visar hur användarna samverkar med hello app. 

###### <a name="jobs"></a>Jobb
Jobb finns används tooreport åtgärder som har en varaktighet. Några exempel:

* Körning av API-anrop
* Visningstid för annonser
* Varaktighet för uppgifter som körs i bakgrunden 
* Varaktighet för köpprocesser
* Visa en video

###### <a name="errors"></a>Fel
Fel är att använda tooreport problem som identifieras av hello app. Exempel på detta är felaktiga användaråtgärder eller misslyckade API-anrop.

###### <a name="application-information"></a>Programinformation
Programinformationen (Appinfon) används tootag data relaterade tooa användarens upplevelse med ett program. Det genereras av användaren interagerar med programmet hello. 

För en viss nyckel Azure Mobile Engagement håller bara reda på hello senaste värdet (ingen historik). Appinfon visar hello status för din app eller dina slutanvändare. Till exempel hello logga in på status eller en användares favoritprodukter.

###### <a name="crash-data"></a>Kraschdata
Kraschdata samlas in automatiskt av hello Mobile Engagement SDK rapporter programfel som inte hanteras av programmet hello. Exempel på detta kan vara ett ohanterat undantag.

###### <a name="extra-data"></a>Extra data
Händelser, fel, aktiviteter och jobb kan göras mer avancerade med parametrar. Det här är en utvecklare kan tillhandahålla som specifika data från programmet hello extra information. Det här är viktigt för att möjliggöra detaljerad segmentering. 

Till exempel kan hello värdet för taggen ”artikel” du toosegment slutanvändare baserat på vilka som läst just den artikeln. Men det räcker kanske inte för att uppnå önskat resultat. I det fallet kan det vara bra om taggen ”artikel” också är försedd med extra information som ”nyhetskategori” inom en viss aktivitet. Detta kan vara användbart toodetermine dynamiskt hello favoritkategorier hello användare. 

Extra information rapporteras i form av ett nyckel-/värdepar. I exemplet hello medieapp är hello extra information för ”nyhetskategori” hello värdet för den kategorin. Exempel på detta är ”sport”, ”ekonomi” och ”politik”.

#### <a name="tag-and-sdk-integration"></a>Taggning och SDK-integration
För steg-för-steg-instruktioner för att integrera hello Azure Mobile Engagement SDK i din app, följ hello [Engagement SDK Integration](mobile-engagement-windows-store-integrate-engagement.md) dokumentation om Azure-webbplatsen. Välj målplattformen hello länkar hello överst på sidan.

Vi rekommenderar att du skapar projekt för två appar som är byggda på Azure Mobile Engagement. En för utveckling och testning och hello andra för produktion. IT-avdelningen kan då gå från test mellanlagring tooproduction när hello användargodkännande lyckas.

#### <a name="user-acceptance-testing-uat"></a>Acceptanstest för användare (UAT)
Acceptanstest för användare (UAT) går ut på att kontrollera så att allt fungerar som avsett. Arbetsflöden ska slutföras och samla in alla nödvändiga data som anges i taggplanen:

* Information märkning ska finnas enligt toodocumented AZME-koncepten
* All information du behöver samlas in (inklusive värden med extra information och appinfo)
* Nomenklaturen matchar bl.a tooyout taggen planera
* Inga dubblettaggar får skickas

Testa alla hello typer av meddelandebeteenden som finns inbäddade i appen

* Meddelanden, undersökningar, data-push både i och utanför appen
* Text-/webbvisningar
* Uppdatering av aktivitetsikon, kategorier

#### <a name="setup"></a>Konfiguration
Det är mycket enkelt att konfigurera Azure Mobile Engagement. All dokumentation för hello relaterade toohello användargränssnittet är tillgängligt på hello Azure Mobile Engagement webbplats [hur toonavigate hello användargränssnittet](mobile-engagement-user-interface-home.md).

Vi rekommenderar att du börjar med att konfigurera hello rätt roller och rollmedlemskap för hello användarna i ditt projekt. Detta hjälper dig att hantera tillgång toohello plattform för alla användare. Rollerna kan vara:

* Administratörer
* Utvecklare
* Läsare 

Därefter:

* Registrera din deviceID tootest på din egen enhet.
* Besök toohello inställningarna för ditt konto och ange hello tidszon toohave diagram och meddelanden leveranstider för din tidszon.
* Gå toohello inställningarna för din App och registrera hello ”App-info” måste tootarget slutanvändaren inom räckvidden.

Mer information om hur toorun din första push-meddelandekampanj, granska [hur tooget igång med att använda och hantera push-meddelanden tooreach ut tooyour slutanvändare](mobile-engagement-how-tos.md).

## <a name="conclusion"></a>Slutsats
Program för användarinteraktion bör köras upprepade gånger. Prova dig fram och förbättra appen allt eftersom. 

Från början, utan skaffa dig erfarenheter med engagement strategier försök inte toobuild en global strategi. En steg-för-steg-metod som identifierar KPI: er och hur tooleverage dem. Strategin är unik för varje app.

När du har fått en viss erfarenhet bör du överväga att lägga till följande tooyour engagement program hello:

* Spårning: Du kan få nya användare och troligtvis även definiera källor för datainsamling. Azure mobile Engagement kan vara länkad toodata samling källor. Det gör att du toomonitor resultaten från de olika källorna. Den här informationen blir intressant toomaximize avkastning på investeringen. 
* A/B-testning: Det här är en viktig del av programmet för användarinteraktion. Varje app har sina egna specifikationer. Med A/B-testning kan du förbättra programmet för användarinteraktion.
* Geografisk placering: Här finns det stora affärsmöjligheter. Tack toothis funktion som du kan nå på hello rätt plats och tid. Vi rekommenderar att du verifierar du har samlat in tillräckligt med beteendedata för slutanvändarna innan du startar toouse geografiska plats.
* Data-push: En data-push är ett osynligt push-meddelande. Med en data-push kan du anpassa appen baserat på slutanvändarnas beteende. Om ett användarsegment ofta tittar högteknologiska produkter, kan hello app ägare skicka en data-push som personanpassar hemsidan med avancerad innehåll.

## <a name="next-steps"></a>Nästa steg
* [Skapa ett Azure Mobile Engagement-konto](mobile-engagement-create.md).
* Besök [definiera din Mobile Engagement-strategi](mobile-engagement-define-your-mobile-engagement-strategy.md) toolearn mer om hur du definierar din Mobile Engagement-strategi.

<!--Image references-->


<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks
