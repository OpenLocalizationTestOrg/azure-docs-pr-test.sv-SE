---
title: "aaaBest praxis för företag som flyttar tooAzure | Microsoft Docs"
description: "Beskriver en kodskelett att företag kan använda tooensure en säker och hanterbar miljö."
services: azure-resource-manager
documentationcenter: na
author: rdendtler
manager: timlt
editor: tysonn
ms.assetid: 8692f37e-4d33-4100-b472-a8da37ce628f
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/31/2017
ms.author: rodend;karlku;tomfitz
ms.openlocfilehash: d1402cf21d0cf740e44c03fc345ecd39a6e1680c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-enterprise-scaffold---prescriptive-subscription-governance"></a>Azure enterprise kodskelett - normativ prenumeration styrning
Företag vidtar allt hello offentligt moln dess rörlighet och flexibilitet. De använder hello Molnets styrkor toogenerate intäkter eller optimera resurser för hello företag. Microsoft Azure tillhandahåller många olika tjänster att företag kan sätta ihop som byggblock tooaddress en mängd olika arbetsbelastningar och program. 

Men om du vet där toobegin är ofta svårt. När du har bestämt toouse Azure uppstå några frågor ofta:

* ”Hur jag uppfylla våra juridiska krav för data suveränitet i vissa länder”?
* ”Hur jag säkerställa att någon inte oavsiktligt ändrar kritiska system”?
* ”Hur vet jag vad varje resurs stöder så jag kan kontot för den och faktura tillbaka korrekt”?

en tom prenumeration med ingen guard spår hello oförmåga är avskräckande. Den här tomt utrymme kan hindra move-tooAzure.

Den här artikeln ger en startpunkt för tekniker tooaddress hello behovet av styrning och jämför den med hello behöver mer flexibel. Det inför hello begreppet en enterprise-kodskelett som hjälper organisationer i implementera och hantera sina Azure-prenumerationer. 

## <a name="need-for-governance"></a>Behovet av att styrning
Du måste lösa hello-avsnittet för styrning tidig tooensure hello lyckad användning av hello molnet inom hello företag när du flyttar tooAzure. Hello tid och bureaucracy för att skapa ett omfattande styrning system du tyvärr innebär vissa affärsgrupper gå direkt toovendors utan att inbegripa företagets IT. Den här metoden kan lämna hello enterprise öppna toovulnerabilities om hello resurser inte som hanteras korrekt. hello-egenskaperna hos offentliga cloud i hello - flexibilitet, flexibilitet och priser för förbrukningsbaserad - är viktiga toobusiness grupper som behöver tooquickly uppfyller hello behoven hos kunder (interna och externa). Men företagets IT måste tooensure att skyddas effektivt data och datorer.

I verkligheten är scaffold-teknik används toocreate hello grunden för hello struktur. Hej kodskelett hjälper hello allmänna mönster och innehåller fästpunkter för permanent system toobe monterade. En enterprise-kodskelett är hello samma: en uppsättning flexibla kontroller och funktioner i Azure som tillhandahåller strukturen toohello miljö och ankare för tjänster som bygger på hello offentligt moln. Det ger hello builders (IT och affärsgrupper) en foundation toocreate och koppla nya tjänster.

Hej kodskelett baseras på praxis som vi har samlats in från många Användarsegmentet med klienter med olika storlekar. Dessa klienter mellan små organisationer utveckling av lösningar i hello molnet tooFortune 500 företag och oberoende programvaruleverantörer som migrerar och utveckling av lösningar i hello molnet. hello enterprise kodskelett är ”specialbyggt” toobe flexibla toosupport både traditionell IT-arbetsbelastningar och flexibel arbetsbelastningar; utvecklare som skapar programvara som en-tjänst (SaaS)-program utifrån t.ex, Azure-funktioner.

hello enterprise kodskelett är avsedda toobe hello grunden för varje ny prenumeration i Azure. Den låter administratörer tooensure arbetsbelastningar uppfyller hello minsta styrningskraven i en organisation utan förhindrar att affärsgrupper och utvecklare snabbt uppfylla sina egna mål.

> [!IMPORTANT]
> Styrning är avgörande toohello framgången för Azure. Den här artikeln riktad mot hello teknisk implementering av en enterprise-kodskelett men bara vidrör på hello bredare process och relationer mellan hello komponenter. Princip för styrning flödar uppifrån hello ned och bestäms av vilka hello företag vill tooachieve. Naturligtvis finns hello skapandet av en modell för styrning för Azure innehåller representanter från IT-avdelningen, men viktigare ska den vara starkt representation från business grupp utfyllnadstecken och säkerhet och riskhantering. I hello slutet är en enterprise-kodskelett om att minimera business risk toofacilitate uppdrag och mål för en organisation.
> 
> 

hello följande bild beskrivs hello komponenter i hello kodskelett. hello foundation förlitar sig på en fast plan för avdelningar, konton och prenumerationer. hello pelare består av Resource Manager principer och starka namngivning standarder. hello resten av hello kodskelett hämtas från grundläggande funktioner i Azure och funktioner att aktivera en säker och hanterbar miljö.

![kodskelett komponenter](./media/resource-manager-subscription-governance/components.png)

> [!NOTE]
> Azure har växt snabbt efter lanseringen 2008. Detta krävs Microsoft engineering team toorethink deras metod för att hantera och distribuera tjänster. hello Azure Resource Manager-modellen introducerades i 2014 och ersätter hello klassiska distributionsmodellen. Resource Manager kan organisationer toomore enkelt distribuera, ordna och styra Azure-resurser. Resource Manager innehåller parallellisering när du skapar resurser för snabbare distribution av lösningar för komplexa, beroende av varandra. Den omfattar även detaljerad åtkomstkontroll och hello möjlighet tootag resurser med metadata. Microsoft rekommenderar att du skapar alla resurser via hello Resource Manager-modellen. hello enterprise kodskelett är uttryckligen utformad för hello Resource Manager-modellen.
> 
> 

## <a name="define-your-hierarchy"></a>Definiera din hierarki
hello grunden för hello kodskelett är hello Azure Enterprise-registrering (och hello Enterprise Portal). Hej företagsregistrering definierar hello form och användning av Azure-tjänster inom ett företag och är hello core styrningsstrukturen. Inom hello enterprise-avtal kan kunder toofurther dela upp hello-miljö till avdelningar, konton och slutligen prenumerationer. En Azure-prenumeration är hello grundläggande där alla resurser som ingår. Den definierar också flera begränsningar i Azure, till exempel antal kärnor, resurser osv.

![Hierarki](./media/resource-manager-subscription-governance/agreement.png)

Alla företag har olika och hello hierarkin i hello föregående bild kan betydande flexibilitet i hur Azure ordnas inom hello företag. Innan du implementerar hello riktlinjer som finns i det här dokumentet, bör du modellen hierarkin och förstå hello inverkan på fakturering, åtkomst till företagsresurser och komplexitet.

hello tre vanliga mönster för Azure-registreringar är:

* Hej **funktionella** mönster
  
    ![funktionella](./media/resource-manager-subscription-governance/functional.png)
* Hej **affärsenhet** mönster 
  
    ![verksamheten](./media/resource-manager-subscription-governance/business.png)
* Hej **geografiska** mönster
  
    ![geografisk](./media/resource-manager-subscription-governance/geographic.png)

Du kan använda hello kodskelett på hello nivå tooextend hello styrning prenumerationskraven av hello enterprise till hello prenumeration.

## <a name="naming-standards"></a>Namnge standarder
hello första hörnsten i hello kodskelett naming standarder. Väl utformad namngivning standarder aktivera tooidentify resurser i hello-portalen på en faktura och i skript. Troligen redan namngivningen standarder för lokal infrastruktur. När du lägger till Azure tooyour miljö bör du utöka de naming standarder tooyour Azure-resurser. Namnge standard underlätta mer effektiv hantering av hello-miljön på alla nivåer.

> [!TIP]
> För namngivningsregler:
> * Granska och vidta om möjligt hello [Patterns and Practices vägledning](../guidance/guidance-naming-conventions.md). Den här vägledningen hjälper dig att bestämma på ett meningsfullt namngivningsstandarden.
> * Använd camelCasing för namnen på de resurser (till exempel myResourceGroup och vnetNetworkName). Obs: Det är vissa resurser, till exempel lagringskonton, där hello endast alternativet toouse gemen (och inga andra specialtecken).
> * Överväg att använda Azure Resource Manager principer (beskrivs i nästa avsnitt om hello) tooenforce naming standarder.
> 
> hello hjälper föregående tips dig att implementera en konsekvent namngivningskonvention.

## <a name="policies-and-auditing"></a>Principer och granskning
hello andra hörnsten i hello kodskelett innebär att du skapar [Azure Resource Manager principer](resource-manager-policy.md) och [granskning hello aktivitetsloggen](resource-group-audit.md). Hanteraren för filserverresurser med får principer du hello möjlighet toomanage risk i Azure. Du kan definiera principer som kontrollera data suveränitet genom att begränsa, framtvinga eller granskning av vissa åtgärder. 

* Principen är en standard **Tillåt** system. Du styr åtgärder genom att definiera och tilldela principer tooresources som neka eller granska åtgärder på resurser.
* Principer beskrivs av principdefinitioner i en princip definition language (if-then villkor).
* Du skapar principer med JSON (Javascript Object Notation) formaterade filer. När du har definierat en princip du tilldelar den tooa viss omfång: prenumerationen, resursgruppen eller resursen.

Principer har flera åtgärder som tillåter för en detaljerad metoden tooyour scenarier. hello åtgärder är:

* **Neka**: block hello resursbegäran
* **Granska**: tillåter hello begäran men lägger till en rad toohello aktivitetsloggen (som kan vara används tooprovide aviseringar eller tootrigger runbooks)
* **Lägg till**: lägger till angivna information toohello resursen. Till exempel om det inte taggen ”CostCenter” på en resurs lägger du till taggen med ett standardvärde.

### <a name="common-uses-of-resource-manager-policies"></a>Vanliga användningsområden för Resource Manager-principer
Principer för Azure Resource Manager är ett kraftfullt verktyg i hello Azure toolkit. Kan du tooavoid oväntat kostnader, tooidentify kostnader för resurser via taggning och tooensure center som kompatibilitet krav är uppfyllda. När principer kombineras med inbyggda granskningsfunktioner som hello fashion du komplexa och flexibla lösningar. Principer tillåter företag tooprovide kontroller för arbetsbelastningar ”traditionell IT” och ”Agile” arbetsbelastningar; som, utveckla kundprogram. hello vanligaste mönster vi finns i principer är:

* **GEO-kompatibilitetsdata/suveränitet** -Azure tillhandahåller regioner över hälsningsmeddelande. Företag vill ofta toocontrol där resurserna skapas (om tooensure data suveränitet eller endast tooensure resurserna skapas nära toohello end konsumenter av hello resurser).
* **Kostnadshanteringen** -Azure-prenumeration kan innehålla resurser av många typer och skala. Företag vill ofta tooensure som standard prenumerationer Undvik onödigt stora resurser, vilket kan kosta hundratals kronor i månaden eller mer.
* **Standard styrning via taggar som krävs** -kräver taggar är en av hello vanligaste och hög önskade funktioner. Använda principer för Azure Resource Manager är företag kan tooensure att en resurs är taggade på rätt sätt. hello vanligaste taggar är: avdelning, resurs-ägare och miljö typ (till exempel - produktion, test och utveckling)

**Exempel**

”Traditionell IT” prenumerationen för line-of-business-program

* Framtvinga avdelning och ägare taggarna på alla resurser
* Begränsa resursen skapas toohello Nordamerika Region
* Begränsa hello möjlighet toocreate G-serien virtuella datorer och HDInsight-kluster

”Flexibel” miljö för en affärsenhet skapar molnprogram

* toomeet data suveränitet krav tillåta hello skapande av resurser endast i en viss region.
* Tillämpa miljötaggen på alla resurser. Om en resurs har skapats utan en tagg, bifoga hello **miljö: Okänt** tagga toohello resurs.
* Granska när resurserna skapas utanför Nordamerika men förhindrar inte.
* Granska när hög kostnad resurser skapas.

> [!TIP]
> hello vanligaste användningen av Resource Manager principer mellan olika organisationer är toocontrol *där* resurser kan skapas och *vad* typer av resurser kan skapas. Dessutom tooproviding kontroller på *där* och *vad*, många företag använda principer tooensure resurser har hello rätt metadata toobill tillbaka för användning. Vi rekommenderar att tillämpa principer på hello prenumerationsnivån för:
> 
> * GEO-kompatibilitetsdata/suveränitet
> * Kostnadshantering
> * Taggar som krävs (bestämd av företagets behov, till exempel BillTo Programägaren)
> 
> Du kan använda ytterligare principer på lägre nivåer i omfånget.
> 
> 

### <a name="audit---what-happened"></a>Gransknings - vad hände?
tooview hur fungerar din miljö, behöver du tooaudit användaraktivitet. De flesta typer av resurser i Azure skapa diagnostikloggar som du kan analysera via ett verktyg i loggen eller i Azure Operations Management Suite. Du kan samla in aktivitetsloggar över flera prenumerationer tooprovide en avdelningsnivå eller enterprise-vy. Granskningsposter är både ett viktigt diagnostiska verktyg och en mekanism för avgörande tootrigger händelser i hello Azure-miljön.

Aktivitetsloggar från Resource Manager distributioner aktivera toodetermine hello **operations** som tog plats och vem som utförde dem. Aktivitetsloggar kan samlas in och sammanställs med verktyg som logganalys.

## <a name="resource-tags"></a>Resurstaggar
Som användare i din organisation lägga till resurser toohello prenumeration, blir allt viktigare tooassociate resurser med hello rätt avdelning, kund och miljö. Du kan koppla metadata tooresources via [taggar](resource-group-using-tags.md). Du kan använda taggar tooprovide information om hello resurs eller hello ägare. Taggar kan du endast sammanställd toonot och gruppera resurser på olika sätt, men använder informationen för hello återbetalning. Du kan tagga resurser med dig too15 nyckel: värde-par. 

Resurstaggarna är flexibla och ska vara anslutna toomost resurser. Exempel på vanliga resurstaggar är:

* BillTo
* Avdelning (eller affärsenhet)
* Miljö (produktion, fas, utveckling)
* Nivån (Webbnivå, program nivå)
* Programmet ägare
* Projektnamn

![tags](./media/resource-manager-subscription-governance/resource-group-tagging.png)

Fler exempel på taggar finns [rekommenderas namnkonventionerna för Azure-resurser](../guidance/guidance-naming-conventions.md).

> [!TIP]
> Överväg att göra en princip som kräver märkning för:
> 
> * Resursgrupper
> * Lagring
> * Virtuella datorer
> * Tjänsten miljöer/web programservrar
> 
> Den här märkning strategin identifierar alla prenumerationer vilka metadata som behövs för hello företag, ekonomi, säkerhet, riskhantering och övergripande hanteringen av hello-miljö. 

## <a name="resource-group"></a>Resursgrupp
Resource Manager kan tooput resurser i meningsfulla grupper för hantering av fakturerings- eller fysisk tillhörighet. Som nämnts tidigare har Azure två distributionsmodeller. Hej var tidigare klassiska modellen, hello grundläggande hanteringsenheten hello prenumeration. Det var svårt toobreak ned resurser inom en prenumeration som ledde toohello skapande av stora mängder prenumerationer. Vi såg hello införandet av resursgrupper med hello Resource Manager-modellen. Resursgrupper är behållare för resurser som har en gemensam livscykel eller dela ett attribut, till exempel ”alla SQL-servrar” eller ”A”.

Resursgrupper kan inte ingå i varandra och resurser kan bara höra tooone resursgruppen. Du kan använda vissa åtgärder på alla resurser i en resursgrupp. Till exempel försvinner tar bort en resursgrupp alla resurser inom hello resursgrupp. Normalt du placera en hela programmet eller relaterade system i hello samma resursgrupp. Till exempel en tre nivåprogram kallas Contoso webbprogram skulle innehålla hello webbservern, programserver och SQLServer i hello samma resursgrupp.

> [!TIP]
> Hur du ska organisera dina resursgrupper kan skilja sig från ”traditionell IT” arbetsbelastningar för ”flexibel IT” arbetsbelastningar:
> 
> * ”Traditionell IT” arbetsbelastningar är oftast grupperade efter objekt inom hello samma livscykel, till exempel ett program. Gruppering av program kan enskilda programhantering.
> * ”Flexibel IT” arbetsbelastningar tenderar toofocus på extern kund-riktade molnprogram. hello resursgrupper bör återspegla hello lager för distributionen (till exempel Webbnivå, App-nivå) och hantering.
> 
> Förstå din arbetsbelastning hjälper dig att utveckla en strategi för gruppen.

## <a name="role-based-access-control"></a>Rollbaserad åtkomstkontroll
Du ber förmodligen dig själv ”som ska ha åtkomst tooresources”? och ”hur kan jag styra åtkomst”? Att tillåta eller inte tillåta åtkomst toohello Azure-portalen och kontrollera åtkomst tooresources hello-portalen är avgörande. 

När Azure ursprungligen hade släppts åtkomst kontroller tooa prenumeration har grundläggande: administratör eller medadministratör. Åtkomst till tooa prenumeration hello klassisk modell underförstådda åtkomst tooall hello resurser i hello-portalen. Den här bristen på finmaskig kontroll vägleds toohello spridning av prenumerationer tooprovide en nivå av rimliga åtkomstkontroll för en Azure-registrering.

Den här spridning av prenumerationer behövs inte längre. Med rollbaserad åtkomstkontroll kan du tilldela användare toostandard roller (till exempel vanliga ”läsare” och ”författare” typer av roller). Du kan också definiera anpassade roller.

> [!TIP]
> tooimplement rollbaserad åtkomstkontroll:
> * Anslut din företagsidentitet store (vanligtvis Active Directory) tooAzure Active Directory med hjälp av hello AD Connect-verktyget.
> * Styra hello Admin/Medadministratör för en prenumeration med hjälp av en hanterad identitet. **Inte** tilldela Admin-/ medadministratör tooa ny prenumeration ägare. Använd i stället RBAC roller tooprovide **ägare** rättigheter tooa grupp eller en person.
> * Lägg till Azure tooa användargrupp (till exempel X Programägarna) i Active Directory. Använda hello synkroniserade grupp tooprovide grupp medlemmar hello rättigheter toomanage hello resursgrupp som innehåller programmet hello.
> * Följ hello principen att bevilja hello **minsta privilegium** krävs toodo hello förväntades arbete. Exempel:
>   * Distributionsgruppen: En grupp som bara kan toodeploy resurser.
>   * : Virtuell dator en hanteringsgrupp som kan toorestart virtuella datorer (för åtgärder)
> 
> De här tipsen hjälpa dig att hantera åtkomst i din prenumeration.

## <a name="azure-resource-locks"></a>Azure-resurslås
När organisationen lägger till core toohello abonnemang, blir allt viktigare tooensure att tjänsterna är tillgängliga tooavoid avbrott i verksamheten. [Resurslås](resource-group-lock-resources.md) aktivera toorestrict åtgärder på värdefulla resurser där ändra eller ta bort dem skulle ha en betydande inverkan på ditt program eller molninfrastruktur. Du kan använda Lås tooa prenumerationen, resursgruppen eller resursen. Normalt kan du använda Lås toofoundational resurser, till exempel virtuella nätverk, gatewayenheter och storage-konton. 

Resurslås för närvarande stöder två värden: CanNotDelete och ReadOnly. CanNotDelete innebär att användare (med lämpliga behörigheter för hello) kan fortfarande läsa eller ändra en resurs men ta bort inte den. ReadOnly innebär att behöriga användare inte kan ta bort eller ändra en resurs.

toocreate eller ta bort lås för hantering, måste du ha tillgång för`Microsoft.Authorization/*` eller `Microsoft.Authorization/locks/*` åtgärder.
I hello inbyggda roller beviljas endast ägare och administratör för användaråtkomst dessa åtgärder.

> [!TIP]
> Nätverksalternativ för kärnor bör skyddas med lås. Oavsiktlig borttagning av en gateway med plats-till-plats VPN är katastrofal tooan Azure-prenumeration. Azure tillåter inte att du toodelete ett virtuellt nätverk som används, men att använda fler begränsningar är en bra försiktighetsåtgärd. 
> 
> * Virtuellt nätverk: CanNotDelete
> * Nätverkssäkerhetsgruppen: CanNotDelete
> * Principer: CanNotDelete
> 
> Principer är också viktigt toohello underhåll av lämpliga kontroller. Vi rekommenderar att du installerar en **CanNotDelete** låsa toopolices som används.

## <a name="core-networking-resources"></a>Core nätverksresurser
Åtkomst tooresources kan vara interna (inom hello företagets nätverk) eller externa (via hello internet). Det är enkelt för användare i din organisation tooinadvertently put-resurser i hello fel plats och öppna dem potentiellt toomalicious åtkomst. Företag måste lägga till lämpliga kontroller tooensure att Azure användare gör hello rätt beslut som lokala enheter. Vi kan identifiera kärnresurser som ger grundläggande kontroll av åtkomst för prenumeration styrning. hello kärnresurser består av:

* **Virtuella nätverk** är behållarobjekt för undernät. Även om det inte är absolut nödvändigt, används ofta när du ansluter program toointernal företagets resurser.
* **Nätverkssäkerhetsgrupper** är liknande tooa brandvägg och ange regler för hur en resurs kan ”prata” hello nätverket. De ger detaljerad kontroll över hur / om ett undernät (eller en virtuell dator) kan ansluta toohello Internet eller andra undernät i hello samma virtuella nätverk.

![Kärnnätverket](./media/resource-manager-subscription-governance/core-network.png)

> [!TIP]
> För nätverk:
> * Skapa virtuella nätverk dedikerad tooexternal riktade arbetsbelastningar och interna arbetsbelastningar. Den här metoden minskar hello chans att oavsiktligt placera virtuella datorer som är avsedda för internt arbetsbelastningar i ett externt Internetriktade utrymme.
> * Konfigurera säkerhet grupper toolimit nätverksåtkomst. Blockera åtkomst toohello minst internet från interna virtuella nätverk och blockera åtkomst toohello företagets nätverk från externa virtuella nätverk.
> 
> De här tipsen hjälper dig att implementera säker nätverksresurser.

### <a name="automation"></a>Automation
Hantera resurser individuellt är både tidskrävande och potentiellt känsliga för vissa åtgärder. Azure tillhandahåller olika automatiseringsfunktionerna inklusive Azure Automation, Logic Apps och Azure Functions. [Azure Automation](../automation/automation-intro.md) kan administratörer toocreate och definiera runbooks toohandle vanliga uppgifter vid hantering av resurser. Du kan skapa runbooks med hjälp av en redigerare för PowerShell eller grafiska redigerare. Du kan skapa komplexa arbetsflöden i flera steg. Azure Automation är ofta använda toohandle vanliga aktiviteter som att stänga av oanvända resurser eller att skapa resurser i svaret tooa utlösare utan mänsklig inblandning.

> [!TIP]
> För automatisering:
> * Skapa ett Azure Automation-konto och granska hello tillgängliga runbooks (både grafiskt och kommandot rad) tillgängliga i hello [Runbook-galleriet](../automation/automation-runbook-gallery.md).
> * Importera och anpassa viktiga runbooks för eget bruk.
> 
> Ett vanligt scenario är hello möjlighet tooStart/avstängning virtuella datorer på ett schema. Det finns exempel runbooks som är tillgängliga i hello galleriet som hanterar det här scenariot och lära dig hur tooexpand den.
> 
> 

## <a name="azure-security-center"></a>Azure Security Center
Kanske har ett hello största blockeringar toocloud införande hello gäller över säkerheten. IT-chefer risk och säkerhet avdelningar måste tooensure att resurser i Azure är säkra. 

Hej [Azure Security Center](../security-center/security-center-intro.md) innehåller en central för hello säkerhetsstatusen för resurserna i hello prenumerationer och ger rekommendationer som kan förhindra angripna resurser. Det kan aktivera mer detaljerad principer (till exempel tillämpa principer toospecific resursgrupper som tillåter hello enterprise tootailor riskerna hållningsdata toohello de adressering). Slutligen är Azure Security Center en öppen plattform som gör Microsoft-partner och oberoende leverantörer toocreate programvara som ansluts till Azure Security Center tooenhance dess funktioner. 

> [!TIP]
> Azure Security Center aktiveras som standard i varje prenumeration. Dock måste du aktivera datainsamling från virtuella datorer tooallow Azure Security Center tooinstall dess agenten och börja samla in data.
> 
> ![Datainsamling](./media/resource-manager-subscription-governance/data-collection.png)
> 
> 

## <a name="next-steps"></a>Nästa steg
* Nu när du har lärt dig om prenumerationen styrning, är det tid toosee rekommendationerna i praktiken. Se [exempel på att implementera Azure-prenumeration styrning](resource-manager-subscription-examples.md).

