---
title: "aaaDesign Azure mallar för sammansatta lösningar | Microsoft Docs"
description: "Visar bästa praxis för att utforma Azure Resource Manager-mallar för avancerade scenarier"
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ce1141d6-ece7-4976-acea-1db1f775409e
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: tomfitz
ms.openlocfilehash: aa45e9a46d79a6336b696cff8fd37f65fa449f11
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="design-patterns-for-azure-resource-manager-templates-when-deploying-complex-solutions"></a>Designmönster för Azure Resource Manager-mallar när du distribuerar komplexa lösningar
Med ett flexibelt sätt baserat på Azure Resource Manager-mallar kan distribuera du komplexa topologier snabbt och konsekvent. Du kan anpassa dessa distributioner enkelt som core erbjudanden utvecklas eller tooaccommodate varianter för avvikare scenarier eller kunder.

Det här avsnittet är en del av en större vitbok. Hämta tooread hello fullständig papper [World klassen Azure Resource Manager-mallar överväganden och beprövade metoder](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).

Mallar kombinera hello fördelarna med hello underliggande Azure Resource Manager med hello förmåga och läsbarhet för JavaScript Object Notation (JSON). Mallar kan du:

* Distribuera topologier och deras arbetsbelastning konsekvent.
* Hantera alla dina resurser i ett program tillsammans med resursgrupper.
* Använda rollbaserad åtkomstkontroll (RBAC) toogrant åtkomstbehörighet toousers, grupper och tjänster.
* Använd märkning kopplingarna toostreamline aktiviteter, till exempel fakturering Rollup-beräkningar.

Den här artikeln innehåller information om förbrukningen scenarier, arkitektur och implementering mönster identifieras vid våra design sessioner och verkliga mallen implementeringar med Azure Customer Advisory Team (AzureCAT) kunder. Dessa metoder är är långt från akademisk, beprövade metoder informera hello utvecklingen av mallar för 12 i hello upp Linux-baserade OSS tekniker, bland annat: Apache Kafka, Apache Spark, Cloudera, Couchbase, Hortonworks HDP, DataStax Enterprise drivs av Apache Cassandra, Elasticsearch, Jenkins, MongoDB, PostgreSQL, Redis och Nagios. 

Den här artikeln delar dessa beprövade metoder toohelp du skapa world klassen Azure Resource Manager-mallar.  

Vi har hittat flera förbrukning upplevelser för Resource Manager-mall i företag, System dietetisk (SI) s och CSV: er i vårt arbete med kunder. hello följande avsnitt innehåller en översikt över vanliga scenarier och mönster för annan kundtyper.

## <a name="enterprises-and-system-integrators"></a>Företag och systemintegrerare.
I stora organisationer ofta visas två konsumenter av Resource Manager-mallar: interna programvaran utvecklingsgrupper och företagets IT. Vi har hittat som hello scenarier för hello SIs mappar toohello scenarier för företag, så hello samma beaktanden.

### <a name="internal-software-development-teams"></a>Interna programvaran utvecklingsgrupper
Om din grupp utvecklar programvara toosupport ditt företag, tillhandahåller mallar ett enkelt sätt tooquickly distribuera teknik för företag-specifika lösningar. Du kan också använda mallar toorapidly skapar utbildning miljöer som aktiverar team medlemmar toogain nödvändiga kunskaper.

Du kan använda mallar som-är eller utöka eller skriva dem tooaccommodate dina behov. Med taggar inom mallar, kan du ange fakturering sammanfattning med olika vyer som team, projekt, individuella och utbildning.

Företag vill ofta programvaruutveckling team toocreate en mall för konsekvent distribution av en lösning. hello mallen underlättar begränsningar så vissa objekt i den miljön som är fasta och inte kan åsidosättas. En bank kan till exempel kräva en mall tooinclude RBAC så programmerare inte kan redigera en bank lösning toosend data tooa personliga storage-konto.

### <a name="corporate-it"></a>Företagets IT
Företagets IT-organisationer använder vanligtvis mallar för att leverera molnet kapaciteten och molnbaserade funktioner.

#### <a name="cloud-capacity"></a>Kapacitet
Det är ett vanligt sätt för företagets IT grupper tooprovide med kapacitet för team med ”t-shirt storlekar”, som är standard erbjudande storlekar, till exempel små, medelstora och stora. hello t-shirt storlek erbjudanden kan blanda olika resurstyper och kvantiteter och ger en nivå av standardisering som gör det möjligt toouse mallar. hello mallar ge kapacitet på ett konsekvent sätt som tillämpar företagsprinciper och använder märkning tooprovide återbetalning tooconsuming organisationer.

Du kan till exempel måste tooprovide utvecklings-, test- eller produktionsmiljöer inom vilken hello programvara utvecklingsgrupper kan distribuera lösningar. hello miljön innehåller en fördefinierad nätverkstopologi och element som hello programvara utvecklingsgrupper kan inte ändra, till exempel regler för åtkomst toohello internet och paket allmänheten. Du kan också ha organisation-specifika rollerna för dessa miljöer med distinkta åtkomstbehörigheter för hello-miljö.

#### <a name="cloud-hosted-capabilities"></a>Molnbaserade funktioner
Du kan använda mallar toosupport molnbaserade funktioner, inklusive enskilda programvarupaket eller sammansatta erbjudanden som erbjuds toointernal raderna i företag. Ett exempel på ett erbjudande för sammansatta är analytics as a service – analytics, visualisering och andra tekniker, levereras i en optimerad, anslutna konfiguration på en fördefinierad nätverkstopologi.

Molnbaserade funktioner som påverkas av hello säkerhets- och rollen överväganden som upprättas av hello kapacitet erbjuder på som de är inbyggt. Dessa funktioner erbjuds som de är eller som en hanterad tjänst. För hello senare, begränsad åtkomst krävs tooenable åtkomst till hello miljö för hantering.

## <a name="cloud-service-vendors"></a>Cloud service leverantörer
Vi identifierat pratar toomany CSV: er flera metoder som du kan använda toodeploy tjänster för kunder och associerade kraven.

### <a name="csv-hosted-offering"></a>CSV-värdbaserad erbjudande
Om du har ditt erbjudande i din egen Azure-prenumeration två värd sätt är vanliga: distribuera en distinkta distribution för alla kunder eller distribuera skalningsenheter som ligger till grund för en delad infrastruktur som används för alla kunder.

* **Distinkta distributioner för varje kund.** Distinkta distributioner per kund kräver fast topologier för olika kända konfigurationer. Dessa distributioner kan ha storlekar för olika virtuella datorer (VM), varierande antal noder och olika mängder associerad lagring. Taggning av distributioner används för sammanslagning fakturering för varje kund. RBAC kan vara aktiverad tooallow kunder åtkomst tooaspects av deras molnmiljö.
* **Skalningsenheter i miljöer med delade flera innehavare.** En mall kan representera en skalningsenhet för miljöer med flera innehavare. I det här fallet hello samma infrastruktur är används toosupport alla kunder. hello distributioner representerar en grupp med resurser som levererar en nivå av kapacitet för hello finns erbjuder, till exempel antalet användare och antal transaktioner. Dessa skalenheter ökas eller minskas begäran krävs.

### <a name="csv-offering-injected-into-customer-subscription"></a>CSV-erbjudande injekteras i kundprenumerationen
Du kanske vill toodeploy programvaran till prenumerationer som ägs av slutanvändare och kunder. Du kan använda mallar toodeploy distinkta distributioner i Azure-konto för en kund.

Dessa distributioner använda RBAC så att du kan uppdatera och hantera hello distribution inom hello kundens konto.

### <a name="azure-marketplace"></a>Azure Marketplace
tooadvertise och sälja dina erbjudanden via en marketplace, till exempel Azure Marketplace, kan du utveckla mallar toodeliver olika typer av distributioner som körs i Azure-konto för en kund. Dessa distinkta distributioner kan beskrivas vanligtvis som en t-shirt storlek (liten, medel, stora), produkt/målgruppen typ (community, developer, enterprise) eller funktionstyp (grundläggande hög tillgänglighet).  I vissa fall kan dessa typer kan du toospecify vissa attribut för hello distribution, till exempel VM eller antal diskar.

## <a name="oss-projects"></a>OSS projekt
I projekt med öppen källkod aktivera Resource Manager-mallar en community-toodeploy en lösning som snabbt med hjälp av beprövade metoder. Du kan lagra mallar i GitHub-lagringsplatsen så hello gemenskapen kan ändra dem i framtiden. Användare kan distribuera dessa mallar i sina egna Azure-prenumerationer.

hello visas följande avsnitt hello saker du måste tooconsider innan du utformar din lösning.

## <a name="identifying-what-is-outside-and-inside-a-vm"></a>Identifiera vad är utanför och inuti en virtuell dator
När du utformar din mall är användbara toolook på hello krav enligt vad som är utanför och inom hello virtuella maskiner (VMs):

* Utanför innebär hello virtuella datorer och andra resurser för din distribution, till exempel hello nätverkets topologi, märkning referenser toohello certifikat/hemligheter och rollbaserad åtkomstkontroll. Dessa resurser är en del av din mall.
* Inuti innebär hello installerad programvara och vill övergripande tillstånd konfiguration. Andra mekanismer som VM-tillägg eller skript, används helt eller delvis. Dessa mekanismer kan identifieras och körs av hello mallen men är inte i den.

Vanliga exempel aktiviteter som du vill göra ”inuti hello box”-  

* Installera eller ta bort serverroller och funktioner
* Installera och konfigurera programvara på hello nod eller kluster
* Distribuera webbplatser på en webbserver
* Distribuera databasen scheman
* Hantera registret eller andra typer av konfigurationsinställningar
* Hantera filer och kataloger
* Starta, stoppa och hantera processer och tjänster
* Hantera lokala grupper och användarkonton
* Installera och hantera paket (.msi, .exe, yum, etc.)
* Hantera miljövariabler
* Köra interna skript (Windows PowerShell, bash, osv.)

### <a name="desired-state-configuration-dsc"></a>Önskad tillståndskonfiguration DSC)
Tänker hello interna tillståndet för dina virtuella datorer efter distributionen, vill du toomake till den här distributionen inte ”driva” från hello-konfiguration som du har definierat och kontrolleras till källkontroll. Den här metoden gör utvecklarna eller driftspersonal gör inte ad hoc-ändringar tooan miljö som inte kontrollerat, testas eller som registrerats i källkontroll. Den här kontrollen är viktig, eftersom hello manuella ändringar inte ingår i källkontroll. De också inte en del av hello standard distribution och påverkar framtida automatiserad distribution av hello programvara.

Önskad tillståndskonfiguration är också viktigt från ett säkerhetsperspektiv utöver din internt anställda. Hackare försöker regelbundet toocompromise och utnyttja programvarusystem. När lyckas är vanliga tooinstall filer och ändra annars hello tillståndet för en komprometterad system. Med önskad tillståndskonfiguration kan du identifiera går mellan hello önskad och aktuell status och återställa en känd konfiguration.

Det finns resurstillägg för hello populäraste mekanismer för DSC - PowerShell DSC, Chef och Puppet. Var och en av dessa tillägg kan distribuera hello ursprungligt tillstånd för den virtuella datorn och också används toomake att hello önskad tillstånd upprätthålls.

## <a name="common-template-scopes"></a>Vanliga mallen scope
Vi har sett tre viktiga lösning mallar scope framkommer i vår erfarenhet. Dessa tre scope – kapacitet, kapaciteten och slutpunkt till slutpunkt lösning – beskrivs i följande avsnitt hello.

### <a name="capacity-scope"></a>Kapacitet omfång
Ett omfång kapacitet ger en uppsättning resurser i en standard-topologi som är förkonfigurerad toobe efterlever principer och förordningar. hello vanligaste exempel distribuerar en standard utvecklingsmiljö i ett scenario med företagets IT- eller SI.

### <a name="capability-scope"></a>Funktionen omfång
Ett omfång kapaciteten fokuserar på att distribuera och konfigurera en topologi för en viss teknik. Vanliga scenarier inklusive tekniker, till exempel SQL Server, Cassandra, Hadoop.

### <a name="end-to-end-solution-scope"></a>Slutpunkt till slutpunkt lösningen omfattning
En slutpunkt till slutpunkt lösningen omfattning mål utöver en enskild funktion och i stället fokuserar på att leverera en end tooend-lösning som består av flera funktioner.  

En lösning som omfattar mallen ska omfatta visar sig som en uppsättning av en eller flera omfång kapaciteten mallar med lösningen-specifika resurser, logik och tillstånd. Ett exempel på en lösning som omfattar mall är en end tooend data pipeline lösningsmall. hello mall kan blanda Lösningsspecifika topologi och tillstånd med flera omfång kapaciteten lösningsmallar som Kafka, Storm och Hadoop.

## <a name="choosing-free-form-vs-known-configurations"></a>Att välja Friform kontra kända konfigurationer
Du tror först en mall bör ge konsumenterna hello största flexibilitet, men många saker påverka hello välja om toouse Friform konfigurationer kontra kända konfigurationer. Det här avsnittet identifierar hello viktig kund och tekniska överväganden som formats hello metoden delas i det här dokumentet.

### <a name="free-form-configurations"></a>Kostnadsfri-konfigurationer
Hello yta ljud Friform konfigurationer perfekt. De kan du tooselect en VM-typ och innehåller ett godtyckligt antal noder och anslutna diskar för dessa noder, och göra det som parametrar tooa mall. Den här metoden är inte idealiskt för vissa scenarier.

I [storlekar för virtuella datorer](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)hello VM av olika typer och storlekar som kan identifieras och var och en av hello antal beständiga diskar (2, 4, 8, 16 eller 32) som kan bifogas. Varje ansluten disk innehåller 500 IOPS och multiplar av dessa diskar kan vara pooler för en multiplikator som antalet IOPS. Till exempel 16 diskar kan vara grupperade tooprovide 8 000 IOPS. Poolning är klar med konfigurationen i hello operativsystem, använder Microsoft Windows lagringsutrymmen eller redundant array of prisvärda diskar (RAID) i Linux.

En konfiguration kan hello markeringen flera VM-instanser olika VM typer och storlekar för dessa instanser olika diskar för hello VM-typ, och en eller flera skript tooconfigure hello VM innehållet.

Det är vanligt att en distribution kan ha flera typer av noder, till exempel master- och datanoder, så den här flexibiliteten ofta har angetts för varje nodtyp.

När du börjar toodeploy kluster alla signifikanta börja toowork med dessa avancerade scenarier. Om du distribuerar ett Hadoop-kluster, till exempel skulle med 8 överordnade noder 200 datanoder och pool 4 anslutna diskar på varje nod och grupperade 16 anslutna diskar per datanod du ha 208 virtuella datorer och 3,232 diskar toomanage.

Storage-konto kommer begränsning begäranden ovan dess identifierade 20 000 transaktioner per sekund begränsa, så du bör titta på lagring konto partitionering och beräkningar toodetermine hello lämpligt antal storage-konton tooaccommodate den här topologin. Angivna hello mängden kombinationer som stöds av hello Friform metoden dynamiska beräkningar är obligatoriska toodetermine hello partitionering lämpliga. hello Azure Resource Manager-mall språk tillhandahåller för närvarande inte matematiska funktioner, så du måste utföra dessa beräkningar i koden genereras en unik, hårdkodade mall med hello rätt information.

I företagets IT och SI scenarier någon underhålla hello mallar och stöder hello distribueras topologier för en eller flera organisationer. Den här ytterligare kostnader – olika konfigurationer och mallar för varje kund, är ett långt önskvärt.

Du kan använda dessa mallar toodeploy miljöer i kundens Azure-prenumeration, men både företagets IT-team och CSV: er vanligtvis distribuera dem till sina egna prenumerationer med hjälp av en återbetalning funktionen toobill sina kunder. I dessa scenarier hello målet är toodeploy kapacitet för flera kunder i en pool av prenumerationer och Behåll distributioner tätt fyllts i hello prenumerationer toominimize prenumeration svällande – det vill säga flera prenumerationer toomanage. Med dynamisk verkligen distributionsstorlekarna kan uppnå den här typen av densitet krävs noggrann planering och ytterligare utveckling för scaffold-teknik för arbete uppdrag hello organisation.

Dessutom kan inte skapa prenumerationer via ett API-anrop men måste göra det manuellt via hello-portalen. När hello antalet prenumerationer ökar alla resulterande prenumeration svällande kräver mänsklig inblandning – kan automatiseras. Med så mycket variationer i hello storlekar för distributioner av du skulle ha toopre etablera ett antal prenumerationer manuellt tooensure prenumerationer som är tillgängliga.

Med tanke på sådana faktorer är en helt fritt format konfiguration mindre tilltalande än Min första tanke.

### <a name="known-configurations--hello-t-shirt-sizing-approach"></a>Kända konfigurationer – hello t-shirt storlek metod
Snarare än erbjuder en mall som ger totalt flexibilitet och oräkneliga variationer i vår erfarenhet vanliga mönstret är tooprovide hello möjlighet tooselect kända konfigurationer – i praktiken standard t-shirt storlekar, till exempel sandbox små, medelstora och stora. Andra exempel på t-shirt storlekar är produkterbjudanden, till exempel community edition eller enterprise edition.  I andra fall kan det vara belastningsspecifikt konfigurationer av en teknik – som kartan minska eller utan sql.

Många företagets IT organisationer, OSS leverantörer och SIs gör sina erbjudanden just nu på detta sätt i lokal virtualiserade miljöer (företag) eller programvara som en tjänst (SaaS)-erbjudanden (CSV: er och OSVs).

Den här metoden ger bra, kända konfigurationer i olika storlekar förkonfigurerade för kunder. Utan kända konfigurationer måste slutkunder fastställa klustret storlek på egen hand, factor plattform resurs begränsningar och gör matematiska tooidentify hello resulterande partitionering av lagringskonton och andra resurser (förfaller toocluster storlek och resurs begränsningar). Kända konfigurationer aktivera kunder tooeasily väljer hello rätt t-shirt storlek – det vill säga en viss distribution. Dessutom toomaking en bättre upplevelse för hello kund ett litet antal kända konfigurationer är enklare toosupport och kan hjälpa dig att leverera en högre säkerhetsnivå för densitet.

En metod för kända konfiguration som fokuserar på t-shirt storlekar kan också ha olika antal noder i en storlek. Exempelvis kanske liten t-shirt mellan 3 och 10 noder.  hello t-shirt storlek skulle vara utformad tooaccommodate in too10 noder och ger hello konsumenten hello möjlighet toomake Friform val in toohello maximala storlek.  

En t-shirt baserat på arbetsbelastning, kan vara mer Friform till sin natur vad gäller hello antalet noder som kan distribueras, men har distinkta nod arbetsbelastningsstorlek och konfigurationen av hello programvara på hello nod.

T-shirt storlek baserat på produkterbjudanden, t.ex. community- eller Enterprise, kan ha olika resurstyper och maximalt antal noder som kan distribueras, knutna vanligtvis toolicensing överväganden eller funktionstillgänglighet över hello olika erbjudanden.

Du kan också anpassa kunder med unika varianter med hjälp av hello JSON-baserade mallar. När du hanterar avvikare, kan du införliva hello lämplig planering och överväganden för utveckling, support och kostnadshantering.

Baserat på hello kunden mallen förbrukning scenarier och krav hello början av det här dokumentet kan identifiera vi ett mönster för mallen uppdelning.

## <a name="capacity-and-capability-scoped-solution-templates"></a>Kapacitet och kapaciteten omfång lösningsmallar
Uppdelning ger en modulär metod tootemplate utveckling som stöder återanvändning utökningsbarhet, testning och tooling. Det här avsnittet innehåller information om hur en uppdelning metod kan vara tillämpade tootemplates med en kapacitet omfattning.

I den här metoden en Huvudmall tar emot parametervärden från en mall för konsument sedan länkar tooseveral typer av mallar och skript nedströms enligt nedan. Parametrar, statiska variabler och genererade variabler är används tooprovide värden till och från hello länkade mallar.

![Mallparametrar](./media/best-practices-resource-manager-design-templates/template-parameters.png)

**Parametrar skickas tooa Huvudmall sedan toolinked mallar**

följande avsnitt fokus på hello typer av mallar och skript som en mall delas i hello. hello avsnitt beskrivs metoder för att skicka statusinformation mellan hello mallar. Varje mall och hello skript typer hello bild beskrivs tillsammans med exempel. Kontextuella exempel finns i ”sätta ihop: ett exempel på implementering” senare i det här dokumentet.

### <a name="template-metadata"></a>Mallens metadata
Mallens metadata (hello metadata.json-fil) innehåller nyckel/värde-par som beskriver en mall i JSON som kan läsas av människor och programvarusystem.

![Mallens metadata](./media/best-practices-resource-manager-design-templates/template-metadata.png)

**Mallens metadata beskrivs i hello metadata.json fil**

Programvaruagenter kan hämta hello metadata.json filen och publicera hello information och en länk toohello mall i en webbsida eller katalog. Delar är *itemDisplayName*, *beskrivning*, *sammanfattning*, *githubUsername*, och *dateUpdated*.

Nedan visas en exempelfil i sin helhet.

    {
        "itemDisplayName": "PostgreSQL 9.3 on Ubuntu VMs",
        "description": "This template creates a PostgreSQL streaming-replication between a master and one or more slave servers each with 2 striped data disks. hello database servers are deployed into a private-only subnet with one publicly accessible jumpbox VM in a DMZ subnet with public IP.",
        "summary": "PostgreSQL stream-replication with multiple slave servers and a publicly accessible jumpbox VM",
        "githubUsername": "arsenvlad",
        "dateUpdated": "2015-04-24"
    }

### <a name="main-template"></a>Huvudmall
hello Huvudmall tar emot parametrar från en användare använder denna information toopopulate komplexa objektvariabler och kör hello länkade mallar.

![Huvudmall](./media/best-practices-resource-manager-design-templates/main-template.png)

**hello Huvudmall tar emot parametrar från en användare**

En parameter som har angetts är en känd konfiguration kallas även hello t-shirt storleksparameter på grund av dess standardiserade värden, till exempel små, medel eller stor. Du kan använda den här parametern på flera olika sätt i praktiken. Mer information finns i ”kända configuration resurser mall” senare i det här dokumentet.

Vissa resurser har distribuerats oavsett hello kända konfiguration som angetts av en användare-parameter. Dessa resurser etableras med en enda delad resurs-mall och delas med andra mallar så hello delad resurs mallen körs första.

Vissa resurser har distribuerats eventuellt oavsett hello angivna kända konfiguration.

### <a name="shared-resources-template"></a>Mall för delade resurser
Den här mallen innehåller resurser som är gemensamma för alla kända konfigurationer. Den innehåller hello virtuellt nätverk, tillgänglighetsuppsättningar och andra resurser som krävs oavsett hello kända konfigurationsmall som har distribuerats.

![För Mallresurser](./media/best-practices-resource-manager-design-templates/template-resources.png)

**Mall för delade resurser**

Resursnamn, till exempel hello virtuella nätverksnamnet baseras på hello Huvudmall. Du kan ange dem som en variabel i mallen eller tas emot som en parameter från hello användaren som krävs av din organisation.

### <a name="optional-resources-template"></a>Valfria resurser mall
mallen för hello valfria resurser innehåller resurser som distribueras via programmering baserat på hello-värdet för en parameter eller variabel.

![Valfria resurser](./media/best-practices-resource-manager-design-templates/optional-resources.png)

**Valfria resurser mall**

Du kan till exempel använda en valfri resurser mallen tooconfigure en jumpbox som möjliggör indirekt åtkomst tooa distribuerad miljö från hello offentliga Internet. Du vill använda en parameter eller variabel tooidentify om hello jumpbox ska aktiveras och hello *concat* funktionsnamn toobuild hello mål för hello mall som *jumpbox_enabled.json*. Länka mallen använder hello resulterande variabeln tooinstall hello jumpbox.

Du kan länka hello valfria resurser mall från flera platser:

* När tillämpliga tooevery distribution, skapa en länk med parametern-driven hello delade resurser mallen.
* När tillämpliga tooselect kända konfigurationer, till exempel bara installera på stora distributioner – skapa en länk som drivs av parametern eller variabeln-driven från hello kända konfigurationsmall.

Om en viss resurs är valfria kan inte styrs av hello mallen konsument utan hello mall-providern. Du kanske till exempel måste toosatisfy krav för en viss produkt eller produkt-tillägg (common för CSV: er) eller tooenforce principer (common för SIs och företagets IT grupper). I dessa fall kan använda du variabeln tooidentify om hello resursen ska distribueras.

### <a name="known-configuration-resources-template"></a>Konfigurationen för kända resurser mall
En parameter kan vara exponerade tooallow hello mallen konsumenten toospecify en önskad konfiguration för kända toodeploy i hello huvudsakliga mallen. Den här kända konfigurationen använder ofta en t-shirt storlek metod med en uppsättning fast configuration storlekar, till exempel sandbox, small, medium, och stora.

![Konfigurationen för kända resurser](./media/best-practices-resource-manager-design-templates/known-config.png)

**Konfigurationen för kända resurser mall**

hello t-shirt storlek metod används ofta men hello parametrar kan representera en uppsättning kända konfigurationer. Du kan till exempel ange en uppsättning miljöer för ett företagsprogram som utveckling, testa och produkt. Du kan också använda den för en cloud service toorepresent olika skala enheter, produktversioner eller produktkonfigurationer, till exempel Community, Developer eller Enterprise.

Precis som med hello delad resurs mallen skickas variabler toohello kända konfigurationer mallen från antingen:

* En användare – det vill säga hello parametrar skickas toohello Huvudmall.
* En organisation, det vill säga hello variabler i hello Huvudmall som representerar interna krav eller principer.

### <a name="member-resources-template"></a>Medlemmen resurser mall
I en konfiguration för kända ingår ofta en eller flera noder medlemstyper. Till exempel med Hadoop har du överordnade noder och datanoder. Om du installerar MongoDB, har du datanoder och ett avgörandet. Om du distribuerar DataStax har datanoder och en virtuell dator med OpsCenter installerad.

![Medlemmar resurser](./media/best-practices-resource-manager-design-templates/member-resources.png)

**Medlemmen resurser mall**

Varje typ av noder kan ha olika storlekar för virtuella datorer, antal anslutna diskar, skript tooinstall och Ställ in hello noder, portkonfigurationer för hello virtuella datorer, antal instanser och annan information. Så att varje nodtyp hämtar sin egen information för att distribuera och konfigurera en infrastruktur som kör skript toodeploy medlem resurs mallen, som innehåller hello och konfigurera programvara inom hello VM.

För virtuella datorer, vanligtvis är två typer av skript används ofta återanvändbara och anpassade skript.

### <a name="widely-reusable-scripts"></a>Allmänt återanvändbara skript
Allmänt återanvändbara skript kan användas för flera typer av mallar. En av hello bättre exempel på dessa ofta återanvändbara skript ställer in RAID på Linux toopool diskar och få ett större antal IOPS. Oavsett hello programvara installeras i hello VM, ger det här skriptet återanvändning av beprövade metoder för vanliga scenarier.

![Återanvändbara skript](./media/best-practices-resource-manager-design-templates/reusable-scripts.png)

**Medlemmen resurser mallar kan anropa ofta återanvändbara skript**

### <a name="custom-scripts"></a>Anpassade skript
Mallar anropa ofta en eller flera skript som installerar och konfigurerar programvara på virtuella datorer. Ett vanligt mönster visas med stora topologier där flera instanser av en eller flera medlemstyper har distribuerats. Ett installationsskript startas för varje virtuell dator som kan köras parallellt, följt av ett installationsskript som anropas efter att alla virtuella datorer (eller alla virtuella datorer i en given Medlemstyp) har distribuerats.

![Anpassade skript](./media/best-practices-resource-manager-design-templates/custom-scripts.png)

**Medlemmen resurser mallar kan anropa skript för ett visst syfte, till exempel VM-konfiguration**

## <a name="capability-scoped-solution-template-example---redis"></a>Funktionen omfång lösning mallen exempel – Redis
tooshow hur en implementering fungerar ska vi titta på ett praktiskt exempel för att skapa en mall som underlättar hello distributionen och konfigurationen av Redis i standard t-shirt storlekar.  

Hello-distribution finns det en uppsättning delade resurser (virtuella nätverk, storage-konto, tillgänglighetsuppsättningar) och en valfri resurs (jumpbox). Det finns flera kända konfigurationer som t-shirt storlekar (liten, medel, stora) men med en nod skriva. Det finns även två specifika ändamål skript (installation, konfiguration).

### <a name="creating-hello-template-files"></a>Skapa hello mallfilerna
Du skapar en Main-mall med namnet azuredeploy.json.

Du skapar den delade resurser mallen med namnet delade resources.json

Du skapar en valfri resurs mallen tooenable hello distribution av en jumpbox med namnet jumpbox_enabled.json

Redis använder bara en enskild nodtyp, så du skapar en enda medlem resursmall med namnet nod resources.json.

Du vill tooinstall varje enskild nod med Redis och sedan konfigurera hello kluster.  Du har skript tooaccommodate hello installationen och konfigurera redis-kluster-install.sh och redis-kluster-setup.sh.

### <a name="linking-hello-templates"></a>Länka hello mallar
Med hjälp av mallen länka hello Huvudmall som länkar ut toohello delade resurser mall som etablerar hello virtuellt nätverk.

Logik har lagts till i hello Huvudmall tooenable konsumenter av hello mallen toospecify om en jumpbox ska distribueras. En *aktiverat* värde för hello *EnableJumpbox* parametern anger att hello kunden toodeploy en jumpbox. Om det här värdet anges hello mall sammanfogar *_enabled* som suffix tooa grundläggande mallnamn hello jumpbox kapaciteten.

hello Huvudmall gäller hello *stora* parametern värdet som ett suffix tooa grundläggande mall-namn för t-shirt storlekar och använder sedan detta värde i en mall länk ut för*technology_on_os_large.json*.

hello topologi liknar den här bilden.

![Redis-mall](./media/best-practices-resource-manager-design-templates/redis-template.png)

**Mallstruktur för ett Redis-mall**

### <a name="configuring-state"></a>Konfigurera tillstånd
Det finns två steg tooconfiguring hello tillstånd för hello noder i klustret hello, både representeras av syfte specifika skript.  Redis-installerar ”redis-kluster-install.sh” och ”redis-kluster-setup.sh” konfigurerar hello klustret.

### <a name="supporting-different-size-deployments"></a>Stöder olika storlekar distributioner
Inuti variabler, hello t-shirt storlek mall anger hello antalet noder för varje typ av toodeploy för hello anges storleken (*stora*). Distribuerar sedan det antalet VM-instanser som använder resurs slingor, tillhandahåller tooresources unika namn genom att lägga till ett nodnamn med ett numeriskt sekvensnummer från *copyIndex()*. Den gör de här stegen för båda frekvent och Varm zonen virtuella datorer som definierats i mallen för hello t-shirt namn

## <a name="decomposition-and-end-to-end-solution-scoped-templates"></a>Lösning för slutpunkt till slutpunkt och uppdelning omfång mallar
En lösningsmall med en omfattning för slutpunkt till slutpunkt lösningen fokuserar på att leverera en slutpunkt till slutpunkt-lösning.  Den här metoden är vanligtvis en sammansättning av flera omfång kapaciteten mallar med ytterligare resurser, logik och tillstånd.

Som är markerade i hello bilden nedan, är hello samma modell som används för funktionen omfång mallar utökat för mallar med en omfattning för slutpunkt till slutpunkt-lösning.

En mall för delade resurser och valfria resurser mallar fungera hello samma funktion som hello kapacitet och kapaciteten omfång mallen metoder, men omfattas av hello end tooend-lösning.

End-lösning för tooend omfång mallar kan också vanligtvis ha t-shirt storlekar, hello fungerande konfiguration resurser mall visar vad som krävs för en viss kända konfiguration av hello lösning.

hello fungerande konfiguration resurser mallen länkar tooone eller mer kapacitet omfattas lösningsmallar som är relevanta toohello end-lösning för tooend samt hello medlem resurs mallar som krävs för hello end tooend-lösning.

Eftersom hello t-shirt storleken på hello-lösning kan skilja sig hello enskilda kapaciteten omfång mallen, är variabler inom hello fungerande konfiguration resurser mallen används tooprovide hello lämpliga värden för underordnade kapaciteten omfång lösning mallar toodeploy hello lämplig t-shirt storlek.

![Slutpunkt till slutpunkt](./media/best-practices-resource-manager-design-templates/end-to-end.png)

**hello modellen används för kapacitet eller kapaciteten omfång lösningsmallar lätt kan utökas för end tooend lösning mallen scope**

## <a name="preparing-templates-for-hello-marketplace"></a>Förbereda mallar för hello Marketplace
hello föregående metoden enkelt kan hantera scenarier där företag, SIs och CSV: er vill tooeither distribuera hello-mallarna eller aktivera sina kunder toodeploy på egen hand.

Ett annat önskade scenario distribuerar en mall via hello marketplace.  Den här uppdelning-metoden fungerar för hello marketplace, med några mindre ändringar.

Som tidigare nämnts kan mallar vara används toooffer olika distributionstyper för försäljning i hello marketplace. Distinkta distributionstyper kanske t-shirt storlekar (liten, medel, stora), produkt/målgruppen typ (community, developer, enterprise) eller funktionstyp (grundläggande hög tillgänglighet).

Värdsystemet toolist hello kända konfigurationerna i hello marketplace som visas nedan, hello befintliga slutet tooend lösning eller begränsade mallar kan vara lätt-funktioner.

hello parametrar toohello Huvudmall först ändras tooremove hello ingående parameter med namnet tshirtSize.

Medan hello distinkta distributionstyper mappas toohello fungerande konfiguration resurser mall, måste de också hello gemensamma resurser och konfiguration finns i hello delade resurser mallen och potentiellt dem i valfri resurs mallar.

Om du vill toopublish din mall toohello marketplace, upprätta olika kopior av mallen Main som ersätter hello tidigare tillgängliga inkommande parametern för tshirtSize tooa variabel som är inbäddad i hello mallen.

![Marketplace](./media/best-practices-resource-manager-design-templates/marketplace.png)

**Anpassning av en lösning omfång mall för hello marketplace**

## <a name="next-steps"></a>Nästa steg
* toolearn om att dela tillstånd till och från mallar, se [dela tillstånd i Azure Resource Manager-mallar](best-practices-resource-manager-state.md).
* Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).

