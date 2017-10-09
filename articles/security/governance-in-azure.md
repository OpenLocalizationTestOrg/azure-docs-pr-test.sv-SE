---
title: aaaGovernance i Azure | Microsoft Docs
description: "Läs mer om molnbaserad databearbetning tjänster som omfattar ett brett urval av compute-instanser och tjänster som kan skalas upp-och automatiskt toomeet hello behov program-eller enterprise."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/01/2017
ms.author: TomSh
ms.openlocfilehash: 956e82e92f4232c24069bdb79fed5315f1d6486f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="governance-in-azure"></a>Styrning i Azure

Vi vet att säkerheten är jobbet en i hello molnet och det är viktigt att du hitta korrekt och rimlig information om säkerheten i Azure. En av hello bästa orsaker toouse Azure för dina program och tjänster är tootake nytta av dess mängd säkerhetsverktyg och funktioner. Dessa verktyg och funktioner gör det möjligt toocreate säkra lösningar på hello säker Azure-plattformen.

toohelp bättre förstå hello matris med styrning kontroller implementerat i Microsoft Azure från båda hello-kund och Microsoft operations perspektiv, den här artikeln ”styrning i Azure” skrivs som ger en omfattande titt på hello Styrning funktioner med Microsoft Azure.

## <a name="azure-platform"></a>Azure-plattformen

Azure är en offentlig molntjänstplattform som har stöd för ett brett urval av operativsystem, programmeringsspråk, ramverk, verktyg, databaser och enheter. Det kan köras Linux behållare med Dockers integration. skapa appar med JavaScript, Python, .NET, PHP, Java och Node.js; build-servrar för iOS, Android och Windows enheter. Offentliga Azure-molnet och support hello samma tekniker miljoner utvecklare och IT-proffs redan förlitar sig på och litar på.

När du bygger på, eller migrera IT tillgångar, en offentlig molntjänstleverantören måste den organisationens förmåga tooprotect dina program och data med hello tjänster och hello kontroller ger toomanage hello säkerheten för din molnbaserade tillgångar.

Azures infrastruktur har utformats från hello anläggning tooapplications som värd för miljontals kunder samtidigt och det ger en säker grund som företag kan uppfylla sina säkerhetskrav. Dessutom Azure ger dig många säkerhetsalternativ och hello möjlighet toocontrol dem så att du kan anpassa toomeet hello unika säkerhetskrav för distributioner i din organisation.

Det här dokumentet hjälper dig att förstå hur Azure styrning funktioner kan hjälpa dig att uppfylla kraven.

## <a name="abstract"></a>Abstrakt

Microsoft Azure cloud styrning ger en integrerad granskning och rådgivning metod för att granska och ge råd organisationer på deras användning av hello Azure-plattformen. Microsoft Azure cloud styrning refererar toohello beslutsprocesser, villkor och principer som ingår i hello planering, arkitektur, förvärv, distribution, åtgärden och hantering av ett moln datoranvändning.

toocreate en plan för Microsoft Azure cloud styrning behöver du tootake inblick i hello personer, processer och tekniker för närvarande på plats och sedan bygga ramverk som gör det lättare för IT-tooconsistently stöder affärsbehov och ger slut användare med hello flexibilitet toouse hello kraftfulla funktioner i Microsoft Azure.

Det här dokumentet beskriver hur du kan uppnå en förhöjd styrning av din IT-resurser i Microsoft Azure. Det här dokumentet hjälper dig att förstå hello styrning funktioner för säkerhet och inbyggda tooMicrosoft Azure.

hello följande finns huvudsakliga hello styrning problem som beskrivs i det här dokumentet:

- Genomförande av principer, processer och procedurer enligt organisationens mål.

- Säkerhet och kontinuerlig organisation krav uppfylls

- Övervakning och aviseringar

## <a name="implementation-of-policies-processes-and-procedures"></a>Implementering av principer, processer och procedurer 

Management har upprättat roller och ansvarsområden toooversee-implementering av hello information säkerhetsprinciper och fungerar kontinuitet i Azure. Microsoft Azure management är ansvarig för att granska säkerhets- och kontinuiteten rutiner inom sina respektive grupper (inklusive tredje part) och gör det lättare att efterlevnad med principer, processer och standarder.

Här följer hello faktorer utvecklades:

- Kontoetablering

- Prenumerationen kontroller

- Roll baserad åtkomstkontroll

- Resurshantering

- Resursen spårning

- Kritisk resurskontroll

- API-åtkomst tooBilling Information

- Kontroller för nätverk

## <a name="account-provisioning"></a>Kontoetablering

Definiera konto hierarki är ett viktigt steg toouse och struktur Azure-tjänster inom ett företag och hello core styrningsstrukturen. Vid kunder med hello enterprise-avtal kan kunder ytterligare dela upp hello-miljö till avdelningar, konton och slutligen prenumerationer.

![Kontoetablering](./media/governance-in-azure/security-governance-in-azure-fig1.png)

Om du inte har ett enterprise-avtal, bör du använda [Azure taggar](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) på prenumerationen nivå toodefine hierarki. En Azure-prenumeration är hello grundläggande där alla resurser som ingår. Den definierar också flera begränsningar i Azure, till exempel antal kärnor, resurser osv. Prenumerationer kan innehålla [resursgrupper](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), som kan innehålla resurser. [RBAC](https://docs.microsoft.com/azure/api-management/api-management-role-based-access-control) principer som tillämpas på dessa tre nivåer.

Alla företag har olika och hello hierarkin med hjälp av Azure taggar vid Företagskunder kan betydande flexibilitet i hur Azure ordnas inom hello företag. Innan du distribuerar resurserna i Microsoft Azure bör du modellera hierarkin och förstå hello inverkan på fakturering, åtkomst till företagsresurser och komplexitet.

## <a name="subscription-controls"></a>Prenumerationen kontroller

Prenumerationen styr hur resursförbrukning rapporteras och debiteras. Prenumerationer kan konfigureras för separata fakturering och betalning. Vi kan ha flera prenumerationer som nämnts tidigare i en Azure-konto. Prenumerationer kan vara används toodetermine hello Azure resursanvändningen för flera avdelningar i ett företag.

Om exempelvis ett företag har IT-avdelningen, HR och marknadsföring avdelningar och dessa har olika projekt som körs. Baserat på hello användning av Azure-resurser som virtuella datorer av varje avdelning, kan de faktureras därefter. Vi kan styra hello ekonomi för varje avdelning av detta.

Azure-prenumerationer upprätta tre parametrar:

- ett unikt prenumerations-ID

- en fakturering plats

- Uppsättning av tillgängliga resurser

En individ, som inkluderar en Microsoft-konto-ID, ett kreditkort antal och hello fullständig uppsättning Azure services – även om Microsoft tillämpar förbrukning gränser, beroende på prenumerationstypen hello.

Registrering av Azure hierarkier definiera hur tjänster är strukturerade inom ett Enterprise-avtal. hello Enterprise Portal gör att kunder toodivide åtkomst tooAzure resurser som är associerade med ett Enterprise-avtal baserat på flexibla hierarkier anpassningsbara tooan organisations unika behov. hello hierarkin mönstret måste matcha en organisations hantering och geografiska struktur så att hello associeras fakturering och åtkomst till företagsresurser kan redovisas korrekt.

hello tre övergripande mönster är funktionell, affärsenhet och geografiska, med avdelningar som en administrativ konstruktion för kontot grupperingar. Inom varje avdelning kan tilldelas konton prenumerationer som skapar silor för fakturering och flera viktiga begränsningar i Azure (t.ex. antalet virtuella datorer, lagringskonton osv.).

![Prenumerationen kontroller](./media/governance-in-azure/security-governance-in-azure-fig2.png)


För organisationer med ett Enterprise-avtal så en hierarki i fyra nivåer i Azure-prenumerationer:

- registreringen företagsadministratör

- avdelning administratör

- kontoägaren

- Tjänstadministratör

Den här hierarkin styr hello följande:

- Fakturering relation

- Administrationen

- Rollen åtkomstkontroll (RBAC) tooartifacts

- Gränser-gränser

- Gränser

  - Användnings- och fakturering (priskort baserat på erbjudande siffror)

  - Begränsningar

  - Virtual Network

- Ansluten too1 AAD (1 AAD associeras med många prenumerationer)

- Associerade tooan enterprise registrering konto

## <a name="role-based-access-controls"></a>Rollbaserad åtkomstkontroll

När Azure ursprungligen hade släppts åtkomst kontroller tooa prenumeration har grundläggande: administratör eller medadministratör. Åtkomst till tooa prenumeration hello klassisk modell underförstådda åtkomst tooall hello resurser i hello-portalen. Den här bristen på finmaskig kontroll vägleds toohello spridning av prenumerationer tooprovide en nivå av rimliga åtkomstkontroll för en Azure-registrering.

![Rollbaserad åtkomstkontroll](./media/governance-in-azure/security-governance-in-azure-fig3.png)

Den här spridning av prenumerationer behövs inte längre. Med rollbaserad åtkomstkontroll kan du tilldela användare toostandard roller (till exempel vanliga ”läsare” och ”författare” typer av roller). Du kan också definiera anpassade roller.

[Azure rollbaserad åtkomstkontroll (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) Aktivera detaljerad åtkomsthantering för Azure. Med RBAC kan bevilja du endast hello mängd åtkomst att användarna måste tooperform sitt arbete. Säkerhet-inriktade företag bör fokusera på och ger anställda hello exakt behörigheter de behöver. För många behörigheter exponera en konto tooattackers. För få behörigheter innebär att anställda kan få arbetet gjort effektivt. Azure rollbaserad åtkomstkontroll (RBAC) kan du lösa det här problemet genom att erbjuda detaljerad åtkomsthantering för Azure. RBAC kan hjälpa dig toosegregate uppgifter inom ditt team och bevilja endast hello åtkomst toousers de behöver tooperform sitt arbete. Obegränsad istället för att ge alla behörigheter i din Azure-prenumeration eller resurser, kan du tillåta endast vissa åtgärder.

Till exempel använda RBAC toolet en medarbetare hantera virtuella datorer i en prenumeration, medan en annan kan hantera SQL-databaser inom hello samma prenumeration.

Azure RBAC har tre grundläggande roller som gäller tooall resurstyper:

- **Ägare** har fullständig åtkomst tooall resurser inklusive hello rätt toodelegate åtkomst tooothers.

- **Deltagare** kan skapa och hantera alla typer av Azure-resurser, men det går inte att bevilja åtkomst tooothers.

- **Läsaren** kan visa befintliga Azure-resurser.

hello resten av hello RBAC-roller i Azure kan hanteringen av specifika Azure-resurser. Hello virtuella deltagarrollen tillåter hello användaren toocreate och hantera virtuella datorer. Det ger dem åtkomst toohello virtuellt nätverk eller hello undernät som hello virtuella datorn ansluter till.

[Inbyggda RBAC-roller](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) visar hello roller som är tillgängliga i Azure. Det anger hello åtgärder och att varje inbyggda rollen ger toousers omfång.

Bevilja åtkomst genom att tilldela hello lämpliga RBAC rollen toousers, grupper och program för ett visst område. hello omfånget för en rolltilldelning kan vara en prenumeration, resursgrupp eller en enskild resurs. En roll som tilldelats en överordnad omfattning ger också åtkomst toohello underordnade som finns i den.

En användare med åtkomst tooa resursgruppen kan exempelvis hantera alla hello-resurser som den innehåller som webbplatser, virtuella datorer och undernät.

Azure RBAC endast stöder hanteringsåtgärder för hello Azure-resurser i hello Azure-portalen och Azure Resource Manager API: er. Det går inte att tillåta alla data på åtgärder för Azure-resurser. Du kan exempelvis tillåta någon toomanage Storage-konton, men inte toohello blobbar eller tabeller i ett Lagringskonto kan inte. Kan hanteras på samma sätt kan en SQL-databas, men hello inte tabeller i den.

Mer information om hur RBAC kan hjälpa dig att hantera åtkomsten finns i [Vad är rollbaserad åtkomstkontroll?](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is)

Du kan också [skapa en anpassad roll](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles) i rollbaserad åtkomstkontroll (RBAC) om ingen av hello inbyggda roller uppfyller dina specifika åtkomst måste. Anpassade roller kan skapas med [Azure PowerShell](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-powershell), [Azure-kommandoradsgränssnittet (CLI)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-azure-cli), och hello [REST API](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-rest). Precis som inbyggda roller kan anpassade roller tilldelas toousers, grupper och program på prenumerationen, resursgruppen och resursen omfattningar.

Inom varje prenumeration kan du bevilja too2000 rolltilldelningar.

## <a name="resource-management"></a>Resurshantering

Azure som ursprungligen endast hello som klassiska distributionsmodellen. I den här modellen fanns varje resurs självständigt. Det gick inte toogroup relaterade resurser tillsammans. I stället du hade toomanually spåra vilka resurser som består av din lösning eller ditt program och Kom ihåg toomanage dem i ett samordnat sätt.

toodeploy en lösning, hade tooeither skapa varje resurs individuellt via hello klassiska portalen eller skapa ett skript som distribuerats alla hello resurser i hello rätt ordning. toodelete en lösning var du tvungen toodelete varje resurs individuellt. Du kan inte enkelt tillämpa och uppdatera principer för åtkomstkontroll för relaterade resurser. Slutligen kan du inte använda taggar tooresources toolabel dem med villkor som hjälper dig övervaka dina resurser och hantera fakturering.

I 2014 introducerade Azure Resource Manager som läggs till hello konceptet för en resursgrupp. En resursgrupp är en behållare för resurser som delar en gemensam livscykel. hello Resource Manager-modellen har flera fördelar:

- Du kan distribuera, hantera och övervaka alla hello-tjänster för din lösning som en grupp i stället för att hantera dessa tjänster individuellt.

- Du kan upprepade gånger distribuera lösningen under hela dess livscykel och vara säker på att dina resurser distribueras i ett konsekvent tillstånd.

- Du kan använda access control tooall resurser i resursgruppen och dessa principer tillämpas automatiskt när nya resurser läggs toohello resursgruppen.

- Du kan lägga till taggar tooresources toologically ordna alla hello resurser i din prenumeration.

- Du kan använda JavaScript Object Notation (JSON) toodefine hello infrastrukturen för lösningen. hello JSON-fil kallas en Resource Manager-mall.

- Du kan definiera hello beroenden mellan resurser så att de distribueras i rätt ordning för hello.

![Resurshantering](./media/governance-in-azure/security-governance-in-azure-fig4.png)

Resource Manager kan tooput resurser i meningsfulla grupper för hantering av fakturerings- eller fysisk tillhörighet. Som nämnts tidigare har Azure två distributionsmodeller. I tidigare hello [modell](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model), hello grundläggande hanteringsenheten var hello prenumeration. Det var svårt toobreak ned resurser inom en prenumeration som ledde toohello skapande av stora mängder prenumerationer. Vi såg hello införandet av resursgrupper med hello Resource Manager-modellen.

En resursgrupp är en behållare som innehåller relaterade resurser för en Azure-lösning. [hello resursgruppen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) kan innehålla alla hello resurser för hello lösning, eller bara de resurser som du vill toomanage som en grupp. Du bestämma hur du vill att tooallocate resurser tooresource grupper baserat på vad som är hello bäst för din organisation.

Rekommendationer om mallar finns i [Metodtips för att skapa Azure Resource Manager-mallar](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices).

Azure Resource Manager analyserar alla beroenden tooensure resurserna skapas i rätt ordning för hello. Om en resurs bygger på ett värde från en annan resurs (till exempel en virtuell dator som behöver ett lagringskonto för diskar) anger du ett beroende.

>[!Note]
>Mer information finns i [Definiera beroenden i Azure Resource Manager-mallar](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-define-dependencies).

Du kan också använda hello mallen för uppdateringar toohello infrastruktur. Du kan till exempel lägga till en resurs tooyour lösning och lägga till konfigurationsregler för hello resurser som redan har distribuerats. Om hello mallen anger att skapa en resurs, men att resursen finns redan, utför Azure Resource Manager en uppdatering i stället för att skapa en ny tillgång. Azure Resource Manager uppdateringar hello befintliga tillgången toohello samma tillstånd som den skulle ha som ny.

Resource Manager tillhandahåller tillägg för scenarier när du behöver ytterligare åtgärder som att installera programvara som inte ingår i hello-installationen.

## <a name="resource-tracking"></a>Resursen spårning

Som användare i din organisation lägga till resurser toohello prenumeration, blir allt viktigare tooassociate resurser med hello rätt avdelning, kund och miljö. Du kan koppla metadata tooresources via taggar. Du använder [taggar](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) tooprovide information om hello resurs eller hello ägare. Taggar kan du endast sammanställd toonot och gruppera resurser på flera sätt, men använder informationen för hello återbetalning.

Använd taggar när du har en komplex samling resursgrupper och resurser och behöver toovisualize tillgångarna hello sätt som gör hello de flesta meningsfullt tooyou. Du kan till exempel markera resurser som har en liknande roll i organisationen eller toohello tillhör samma avdelning.

Utan taggar, användare i din organisation kan skapa flera resurser som kan vara svårt toolater identifiera och hantera. Du kan till exempel vill toodelete alla hello resurser för ett projekt. Om resurserna inte är märkta för hello projekt, måste du hittar dem manuellt. Taggning kan vara ett bra sätt tooreduce onödiga kostnader i din prenumeration.

Resurser behöver inte tooreside i hello samma resurs grupp tooshare en tagg. Du kan skapa egna taggen taxonomi tooensure att alla användare i din organisation använder vanliga taggar i stället för användare av misstag använder varianter (till exempel ”Avd” i stället för ”avdelning”).

Resursprinciper aktivera toocreate standardregler för din organisation. Du kan skapa principer som kontrollera resurser som är märkta med hello lämpliga värden.

> [!Note]
> Mer information finns i [gäller resursprinciper för taggar](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy-tags).

Du kan också visa taggade resurser via hello Azure-portalen.

Hej [användningsrapporten](https://docs.microsoft.com/azure/billing/billing-understand-your-bill) för din prenumeration omfattar taggnamn och värden, vilket gör att du toobreak ut kostnader efter taggar.

> [!Note]
> Mer information om taggar finns [med hjälp av taggar tooorganize resurserna i Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags).

hello följande begränsningar gäller tootags:

- Varje resurs eller en resursgrupp kan innehålla högst 15 taggen nyckel/värde-par. Den här begränsningen gäller endast tootags tillämpas direkt toohello resursgrupp eller resurs. En resursgrupp kan innehålla många resurser som har 15 taggen nyckel/värde-par.

- hello taggnamn är begränsad too512 tecken.

- Hej Taggvärdet är begränsad too256 tecken.

- Märkningar toohello resursgruppen ärvs inte av hello resurser i resursgruppen.

Om du har mer än 15 värden som du behöver tooassociate med en resurs kan du använda en JSON-sträng för hello taggvärde. hello JSON-strängen kan innehålla flera värden som är viktiga för tillämpade tooa enstaka etikett.

### <a name="tags-and-billing"></a>Taggar och fakturering

Taggar kan du toogroup din faktureringsinformation. Till exempel om du kör flera virtuella datorer i olika organisationer kan använda hello taggar toogroup användning per kostnadsställe. Du kan också använda taggar toocategorize kostnader av körningsmiljön; t.ex, hello fakturering användning för virtuella datorer som körs i produktionsmiljön.

Du kan hämta information om taggar via hello [Azure Resursanvändning och RateCard APIs](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview) eller hello användning fil med kommaavgränsade värden (CSV) fil. Du ladda ned filen för användning av hello från hello [konton i Azure portal](https://account.windowsazure.com/) eller [EA portal](https://ea.azure.com/).

>[!Note]
> Mer information om Programmeringsåtkomst toobilling information finns i [få insikter om dina Microsoft Azure-resursförbrukning](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview). REST-API: et finns [Azure Billing REST API-referens](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).

När du hämtar hello användning CSV för tjänster som stöder taggar med fakturering visas hello taggar i hello taggar kolumn.

## <a name="critical-resource-controls"></a>Kritisk resurskontroller

När organisationen lägger till core toohello abonnemang, blir allt viktigare tooensure att tjänsterna är tillgängliga tooavoid avbrott i verksamheten. [Resurslås](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) aktivera toorestrict åtgärder på värdefulla resurser där ändra eller ta bort dem skulle ha en betydande inverkan på ditt program eller molninfrastruktur. Du kan använda Lås tooa prenumerationen, resursgruppen eller resursen. Normalt kan du använda Lås toofoundational resurser, till exempel virtuella nätverk, gatewayenheter och storage-konton.

Resurslås för närvarande stöder två värden: CanNotDelete och ReadOnly. CanNotDelete innebär att användare (med lämpliga behörigheter för hello) kan fortfarande läsa eller ändra en resurs men ta bort inte den. ReadOnly innebär att behöriga användare inte kan ta bort eller ändra en resurs.

Hanteraren för filserverresurser Lås gäller toooperations som sker i plan för hello, som består av åtgärder som skickas för<https://management.azure.com>. hello Lås begränsar inte hur resurser utföra sina egna funktioner. Resursändringar är begränsad, men resursåtgärder är inte begränsade. Till exempel en ReadOnly-lås på en SQL-databas hindrar dig från att ta bort eller ändra hello-databasen, men det hindrar dig inte från att skapa, uppdatera eller ta bort data i hello-databas.

Tillämpa **ReadOnly** kan leda till toounexpected resultat eftersom vissa åtgärder som ser ut som att läsa ytterligare åtgärder krävs för åtgärderna. Till exempel placera en **ReadOnly** lås på ett lagringskonto som förhindrar att alla användare med hello nycklar. hello lista nycklar operationen hanteras via en POST-begäran eftersom hello returnerade nycklar är tillgängliga för skrivåtgärder.

![Kritisk resurskontroller](./media/governance-in-azure/security-governance-in-azure-fig5.png)

För ett annat exempel är förhindrar att placera en ReadOnly-lås på en App Service-resurs Visual Studio Server Explorer visar filer för hello resursen eftersom den interaktionen kräver skrivåtkomst.

Till skillnad från rollbaserad åtkomstkontroll använder du management Lås tooapply en begränsning för alla användare och roller. toolearn om att ange behörigheter för användare och roller, se [Azure rollbaserad åtkomstkontroll](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).

När du använder ett lås på en överordnad omfattning, alla resurser inom detta scope ärver hello samma Lås. Även resurser som du senare lägger till Ärv hello överordnade hello Lås. hello mest restriktiva Lås i hello arv företräde.

toocreate eller ta bort lås för hantering, måste du ha åtkomst tooMicrosoft.Authorization/ _eller Microsoft.Authorization/locks/_ åtgärder. I hello inbyggda roller, endast **ägare** och **administratör för användaråtkomst** beviljas dessa åtgärder.

## <a name="api-access-toobilling-information"></a>API-åtkomst toobilling information

Använda Azure Billing API: erna toopull användnings- och data till din önskade verktyg för dataanalys. hello Azure Resursanvändning och RateCard APIs kan hjälpa dig korrekt förutsäga och hantera dina kostnader. hello API: er implementeras som en Resource Provider och en del av hello API: er som exponeras av hello Azure Resource Manager.

### <a name="azure-resource-usage-api-preview"></a>Azure Resursanvändning API (förhandsgranskning)

Använd hello Azure [resurs användning API](https://msdn.microsoft.com/library/azure/mt219003) tooget uppskattade Azure förbrukningsdata. hello API innehåller:

- **Azure rollbaserad åtkomstkontroll** -Konfigurera åtkomst principer på hello [Azure-portalen](https://portal.azure.com/) eller via [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview) toospecify vilka användare eller program kan få åtkomst toohello prenumeration användningsdata. Anropare måste använda standard Azure Active Directory-token för autentisering. Lägg till hello anroparen tooeither hello fakturering Reader, Reader, ägare eller deltagare rollen tooget åtkomst toohello användningsdata för en viss Azure-prenumeration.

- **Varje timme och dag** – anropare kan ange om de vill ha sina Azure användningsdata i varje timme tidsgrupper eller dagligen tidsgrupper. hello standard är varje dag.

- **Instansen metadata (inklusive resurstaggar)** – hämta instansen detaljnivåer som hello fullständiga resurs-uri (/subscriptions/ {prenumerations-id} /..), hello resursinformation för gruppen och resurstaggar. Dessa metadata hjälper dig att deterministiskt och allokera programmässigt användning av hello taggar för användningsområden som mellan debitering.

- **Resursmetadata** -resursinformation som hello mätaren namn, mätaren kategori, mätaren underkategori, enhet och region ge hello anroparen en bättre förståelse för vad förbrukades. Vi arbetar också tooalign resurs metadata terminologi över hello Azure-portalen, Azure användning CSV, EA CSV, fakturering och andra offentliga upplevelser, toolet du korrelera data över upplevelser.

- **Användning för alla erbjuder typer** – användningsdata är tillgängliga för alla erbjuder typer som betala per användning, MSDN, Summa, kredit och EA.

**Azure-resurs RateCard API (förhandsgranskning)**

Använd hello Azure Resource RateCard API tooget hello lista över tillgängliga Azure-resurser och uppskattade prisinformation för varje. hello API innehåller:

- **Azure rollbaserad åtkomstkontroll** – konfigurera principer för åtkomst på hello Azure-portalen eller via Azure PowerShell-cmdlets toospecify som användare eller program kan komma åt toohello RateCard data. Anropare måste använda standard Azure Active Directory-token för autentisering. Lägg till hello anroparen tooeither hello Reader, ägare eller deltagare rollen tooget åtkomst toohello användningsdata för en viss Azure-prenumeration.

- **Stöd för betala per användning, MSDN, Summa och kredit erbjuder (EA stöds inte)** -detta API ger Azure erbjudande nivå hastighet information. hello anroparen av denna API måste klara i hello erbjudande information tooget resursinformation och priser. Vi kan för närvarande inte tooprovide EA priser eftersom EA erbjudanden har anpassat kostnader per registrering. Här följer några hello-scenarier som är möjligt med hello kombination av hello användning och hello RateCard APIs:

- **Azure tillbringar under hello månad** -Använd hello kombination av hello användnings- och RateCard APIs tooget bättre insikter om ditt moln tillbringar under hello månad. Du kan analysera hello varje timme och dag buckets för användnings- och kostnad beräkningarna.

- **Ställa in aviseringar** – använda hello användning och hello RateCard APIs tooget uppskattade molnet förbrukning och kostnader och konfigurera resurs- eller monetära baserade aviseringar.

- **Förutsäga faktura** – Get uppskattade användnings- och molnet tillbringar och tillämpa maskininlärning algoritmer toopredict vilka hello faktura skulle vara hello slutet av hello fakturering cykel.

- **Före förbrukning kostnad analysis** – Använd hello RateCard API toopredict hur mycket din faktura är för din förväntade användning när du flyttar dina arbetsbelastningar tooAzure. Om du har befintliga arbetsbelastningar i andra moln eller privata moln, kan du mappa förbrukningen med hello Azure priser tooget ägna en bättre uppfattning på Azure. Denna uppskattning ger du hello möjlighet toopivot på erbjudandet och jämför hello olika erbjudandetyper utöver betala per användning, t.ex. summa och kredit. hello API ger dig även hello möjlighet toosee kostnaden skillnader efter region och låter dig toodo en vad händer om kostnaden analysis toohelp du fattar distributionsbeslut.

- **Konsekvensanalys** -du kan fastställa om det är mer kostnadseffektivt toorun arbetsbelastningar i en annan region, eller på en annan konfiguration av hello Azure-resurs. Azure-resurs kostnaderna kan skilja sig utifrån hello Azure-region som du använder.

- Du kan också bestämma om en annan typ av Azure-erbjudande ger en bättre hastighet på Azure-resurs.

## <a name="networking-controls"></a>Kontroller för nätverk

Åtkomst tooresources kan vara interna (inom hello företagets nätverk) eller externa (via hello internet). Det är enkelt för användare i din organisation tooinadvertently put-resurser i hello fel plats och öppna dem potentiellt toomalicious åtkomst. Precis som med lokalt / enheter, företag måste lägga till lämpliga kontroller tooensure att Azure användare gör hello rätt beslut.

![Kontroller för nätverk](./media/governance-in-azure/security-governance-in-azure-fig6.png)

Vi kan identifiera kärnresurser som ger grundläggande kontroll av åtkomst för prenumeration styrning. hello kärnresurser består av:

### <a name="network-connectivity"></a>Nätverksanslutning

[Virtuella nätverk](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) är behållarobjekt för undernät. Även om det inte är absolut nödvändigt, används ofta när du ansluter program toointernal företagets resurser. hello Azure Virtual Network service aktiverar du toosecurely ansluta Azure-resurser tooeach andra med virtuella nätverk (Vnet).

Ett virtuellt nätverk är en representation av ditt eget nätverk i hello molnet. Ett virtuellt nätverk är en logisk isolering av hello Azure-molnet dedikerad tooyour prenumeration. Du kan också ansluta Vnet tooyour lokalt nätverk.

Följande är funktioner för virtuella Azure-nätverk:

- **Isolering**: Vnet isoleras från varandra. Du kan skapa separata Vnet för utveckling, testning och produktion som Använd hello samma CIDR-Adressblock. Däremot kan du skapa flera virtuella nätverk som använder olika CIDR-Adressblock och ansluta nätverk tillsammans. Du kan dela ett VNet i flera undernät. Azure erbjuder intern namnmatchning för virtuella datorer och molntjänster rollinstanser anslutna tooa VNet. Du kan också konfigurera ett virtuellt nätverk toouse egna DNS-servrar, istället för att använda intern namnmatchning för Azure.

- **Internetanslutning**: alla Azure virtuella datorer (VM) och molntjänster rollinstanser anslutna tooa VNet har åtkomst till toohello Internet, som standard. Du kan också aktivera inkommande åtkomst toospecific resurser efter behov.

- **Azure-resurs anslutningen**: Azure-resurser som molntjänster och virtuella datorer kan vara anslutna toohello samma virtuella nätverk. hello resurser kan ansluta tooeach andra använder privata IP-adresser, även om de finns i olika undernät. Azure tillhandahåller standardroutning mellan undernät, virtuella nätverk och lokala nätverk, så att du inte har tooconfigure och hantera vägar.

- **VNet-anslutningen**: Vnet kan vara anslutna tooeach andra, aktivera resurser anslutna tooany VNet toocommunicate med alla resurser för andra virtuella nätverk.

- **Lokal anslutning**: Vnet kan vara anslutna tooon lokala nätverk via privata nätverksanslutningar mellan ditt nätverk och Azure, eller en plats-till-plats VPN-anslutning via hello Internet.

- **Trafikfiltrering**: molntjänster och Virtuella nätverkstrafik för rollen instanser inkommande och utgående kan filtreras efter källans IP-adress och port, mål-IP-adress och port och protokoll.

- **Routning**: du kan du åsidosätta Azures standard routning genom att konfigurera egna vägar eller använda BGP-vägar via en nätverksgateway.

## <a name="network-access-controls"></a>Åtkomstkontroller för nätverk

[Nätverkssäkerhetsgrupper](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) fungerar som en brandvägg och ange regler för hur en resurs kan ”prata” hello nätverket. De ger detaljerad kontroll över hur / om ett undernät (eller en virtuell dator) kan ansluta toohello Internet eller andra undernät i hello samma virtuella nätverk.

En nätverkssäkerhetsgrupp (NSG) innehåller en lista över säkerhetsregler som tillåter eller nekar nätverket trafik tooresources anslutna tooAzure virtuella nätverk (VNet). NSG: er kan vara associerade toosubnets, enskilda virtuella datorer (klassisk) eller enskilda nätverksgränssnitt (NIC) kopplade tooVMs (Resource Manager).

När en NSG är associerad tooa undernät, tillämpas hello reglerna tooall resurser anslutna toohello undernät. Trafik kan ytterligare begränsas av också koppla en NSG tooa VM eller nätverkskort.

## <a name="security-and-continuous-compliance-with-organizational-standards"></a>Säkerhet och kontinuerlig kompatibilitet med organisationens normer

Alla företag har olika behov och alla företag kan dra nytta av distinkta fördelar från molnlösningar. Kunder för alla typer har fortfarande, hello samma grundläggande frågor om hur du flyttar toohello moln. De vill tooretain kontroll över sina data och de vill att data toobe hålls säker och privat, alla samtidigt genomskinlighet och efterlevnad.

Kunder vill från molntjänstleverantörer är:

- **Skydda våra data** medan bekräftades att hello molnet kan ge ökad säkerhet för data och administrativ kontroll, IT-företag fortfarande gäller att migrera toohello moln lämnar dem mer sårbara toohackers än nuvarande interna lösningar.

- **Skydda våra data** privata molntjänster Generera en unik sekretess utmaningar för företag. Företag, leta toohello moln toosave på kostnader med infrastruktur och förbättra deras flexibilitet de också bry dig om att förlora kontroll över där deras data lagras, vem som åtkomst till den och hur den används.

- **Ge oss kontrollen** även om de dra nytta av hello molnet toodeploy mer innovativa lösningar företag är mycket orolig förlora kontroll över sina data. hello senaste upplysningar hos myndigheter som har åtkomst till kundens data på både juridiska och extra juridiska sätt göra vissa IT-chefer försiktig med att lagra sina data i hello molnet.

- **Befordra genomskinlighet** säkerhet, sekretess och kontroll är viktiga toobusiness beslutsfattare, de också vill hello möjlighet tooindependently kontrollera hur data lagras, ofta och säkra.

- **Upprätthålla överensstämmelse** företag Expandera användningen av molntekniker, hello komplexitet och omfattningen av standarder och föreskrifter fortsätta tooevolve. Företag måste tooknow som deras efterlevnadsstandarder uppfylls och att efterlevnad kommer att utvecklas som föreskrifter ändring över tid.

## <a name="security-configuration-monitoring-and-alerting"></a>Konfiguration, övervakning och avisering

Azure-prenumeranter kan hantera sina molnmiljöer från flera enheter, inklusive hantering av arbetsstationer, utvecklardatorer och även privilegierade slutanvändarens enheter som har uppgiftsspecifika behörigheter. I vissa fall kan utförs administrativa funktioner via webbaserade konsoler, till exempel hello Azure-portalen. I andra fall kan finnas det direkta anslutningar tooAzure från lokala system över virtuella privata nätverk (VPN), Terminal Services, klientprotokoll för program eller (programmässigt) hello Azure Service Management API (SMAPI). Dessutom kan klientslutpunkter vara antingen domänanslutna eller isolerade och ohanterade, till exempel surfplattor eller smartphones.

Flera funktioner för åtkomst och hantering ger en omfattande uppsättning alternativ, kan den här variationen lägga till betydande risk tooa molndistribution. Det kan vara svårt toomanage spåra och granska administrativa åtgärder. Den här variationen kan också innebära säkerhetshot via oreglerad åtkomst tooclient slutpunkter som används för att hantera molntjänster. Med hjälp av allmänna eller personliga arbetsstationer för att utveckla och hantera infrastrukturen öppnas oförutsägbara hotvektorer, till exempel webbsurfning (t.ex. vattenhålsattacker) eller mejl (t.ex. social manipulation och nätfiskewebbplatser).

Övervakning, loggning och granskning ger en grund för att spåra och förstå administrativa aktiviteter, men det inte alltid möjligt tooaudit alla åtgärder i slutföra detalj på grund av toohello mängden data som genereras. Granskning hello effektivitet med principer för hantering av hello är bästa praxis, men.

Azure-säkerhet styrning från AD DS-grupprincipobjekt toocontrol alla hello administratörens Windows-gränssnitt, till exempel fildelning. Inkludera hanteringsdatorer processerna för granskning, övervakning och loggning. Spåra alla administratörers och utvecklares åtkomst och användning.

### <a name="azure-security-center"></a>Azure security center

Hej [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) innehåller en central för hello säkerhetsstatusen för resurserna i hello prenumerationer och ger rekommendationer som kan förhindra angripna resurser. Det kan aktivera mer detaljerad principer (till exempel tillämpa principer toospecific resursgrupper som tillåter hello enterprise tootailor riskerna hållningsdata toohello de adressering).

![Azure Security Center](./media/governance-in-azure/security-governance-in-azure-fig7.png)

Security Center innehåller integrerad säkerhet övervaka och hantera principer för dina Azure-prenumerationer, och upptäcka hot som annars kanske skulle förbli oupptäckta, fungerar tillsammans med ett vittomfattande ekosystem med säkerhetslösningar. När du har aktiverat [säkerhetsprinciper](https://docs.microsoft.com/azure/security-center/security-center-policies) för en prenumeration resurser, Security Center analyserar hello säkerheten för dina resurser tooidentify potentiella säkerhetsproblem. Information om nätverkskonfigurationen är tillgänglig direkt.

Azure Security Center representerar en kombination av bästa praxis analys- och principhantering för alla resurser i en Azure-prenumeration. Verktyget kraftfulla och enkelt toouse kan säkerhet grupper och risker polis tooprevent, upptäcka och åtgärda toosecurity hot som automatiskt samlar in och analyserar säkerhetsdata från Azure-resurser, hello nätverket och partnerlösningarna, till exempel program mot skadlig kod och brandväggar.

Dessutom analysteknik Azure Security Center avancerad, inklusive machine learning och beteendeanalys samtidigt utnyttja globala hotinformation från Microsoft-produkter och tjänster, hello Microsoft Digital Crimes Unit (DCU), hello Microsoft Security Response Center (MSRC) och externa flöden används. [Säkerhet styrning](https://www.credera.com/blog/credera-site/azure-governance-part-4-other-tools-in-the-toolbox/) kan användas brett på hello prenumerationsnivån eller minskar antalet toospecific detaljerade krav tillämpas tooindividual resurser via principdefinitionen.

Slutligen Azure Security Center analyserar resurssäkerhetshälsa baserat på de principerna och använder den här tooprovide insiktsfulla instrumentpaneler och avisering för händelser, t.ex identifiering av skadlig kod eller skadliga IP-anslutning försök.

>[!Note]
> Mer information om hur tooapply rekommendationer läsa [utföra säkerhetsrekommendationerna i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-recommendations).

Security Center samlar in data från dina virtuella datorer tooassess sina säkerhetstillstånd anger säkerhetsrekommendationer och varna dig toothreats. Första gången du öppnar Security Center, är insamling av data aktiverat på alla virtuella datorer i din prenumeration. Insamling av data rekommenderas, men du kan välja bort av [inaktivera datainsamling](https://docs.microsoft.com/azure/security-center/security-center-faq) i hello Security Center-princip.

Slutligen är Azure Security Center en öppen plattform som gör Microsoft-partner och oberoende leverantörer toocreate programvara som ansluts till Azure Security Center tooenhance dess funktioner.

Azure Security Center övervakar hello följande Azure-resurser:

- Virtuella datorer (VM) (inklusive Cloud Services)

- Azure Virtual Networks

- Azure SQL-tjänsten

- Partnerlösningar som är integrerad med din Azure-prenumeration, till exempel en brandvägg för webbaserade program på virtuella datorer och på [Apptjänstmiljö](https://docs.microsoft.com/azure/app-service/app-service-app-service-environments-readme).

### <a name="operations-management-suite"></a>Operations Management Suite

Hej OMS programvaruutveckling och tjänsten grupps informationssäkerhet och [styrning programmet](https://github.com/Microsoft/azure-docs/blob/master/articles/log-analytics/log-analytics-security.md) stöder affärskraven och följer toolaws och -förordningar enligt beskrivningen i [Microsoft Azure Trust Center ](https://azure.microsoft.com/support/trust-center/) och [Microsoft Trust Center kompatibilitet](https://www.microsoft.com/TrustCenter/Compliance/default.aspx). Hur OMS upprätta säkerhetskrav, identifierar säkerhetsåtgärder, hanterar och övervakar risker beskrivs också det. Årligen, vi granska principer, standarder och procedurer som riktlinjer.

Varje medlem i gruppen OMS utveckling får formella säkerhetsutbildning. Internt, använder vi ett system för version för programutveckling. Varje projekt program är skyddat av hello systemet för versionskontroll.

Microsoft har ett team för säkerhet och efterlevnad som övervakar och utvärderar alla tjänster i Microsoft. Information security polis utgör hello team och de är inte kopplat till hello tekniska avdelningar som utvecklar OMS. hello har sina egna management kedjan och utföra oberoende bedömning av produkter och tjänster tooensure säkerhet och efterlevnad.

Operations Management Suite (även kallat OMS) är en uppsättning tjänster som har utformats i hello molnet från hello start. I stället för att distribuera och hantera lokala resurser finns helt OMS-komponenter i Azure. Konfigurationen är minimal, och du kommer igång på bara några minuter.

![Operations Manager Suite](./media/governance-in-azure/security-governance-in-azure-fig8.png)

Bara för att OMS-tjänster körs i betyder hello molnet att de effektivt kan hantera din lokala miljö.

Placera en agent på alla Windows eller Linux-dator i datacentret och den skickar data tooLog Analytics där den kan analyseras tillsammans med alla data som samlas in från molnet eller lokala tjänster. Använda Azure Backup och Azure Site Recovery tooleverage hello moln för säkerhetskopiering och hög tillgänglighet för på lokala resurser.

Runbooks i hello molnet normalt inte kan komma åt lokala resurser, men du kan installera en agent på en eller flera datorer för som ska vara värd för runbooks i ditt datacenter. När du startar en runbook kan ange du bara om du vill att den toorun i hello moln eller på en lokal worker.

hello huvudfunktionerna i OMS tillhandahålls av en uppsättning tjänster som körs i Azure. Varje tjänst innehåller en funktion för specifika hanteringsservern och du kan kombinera olika hanteringsscenarier för tjänster tooachieve.

![Operations Manager Suite](./media/governance-in-azure/security-governance-in-azure-fig9.JPG)

Azure-åtgärden manager överskrider dess funktioner genom att tillhandahålla lösningar för hantering. [Hanteringslösningar](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solutions) är färdigförpackade uppsättningar av logik som implementerar ett hanteringsscenario som utnyttjar en eller flera OMS-tjänster.

![Hantera Azure-åtgärden](./media/governance-in-azure/security-governance-in-azure-fig10.png)

Olika lösningar är tillgängliga från Microsoft och att du kan enkelt lägga till Azure-prenumeration tooyour tooincrease hello värdet på din investering i OMS-partner.

Som partner kan du skapa egna lösningar toosupport dina program och tjänster och ge dem toousers via hello Azure Marketplace eller snabb Start mallar.

## <a name="performance-alerting-and-monitoring"></a>Prestandaaviseringar och övervakning

### <a name="alerting"></a>Aviseringar

Aviseringar är en metod för att övervaka Azure-resurs mått, händelser eller loggar och att meddelas när du anger ett villkor uppfylls.

**Aviseringar i olika Azure-tjänster**

Aviseringar är tillgängliga för olika tjänster, inklusive:

- Application Insights: Gör det möjligt för webbtestet och mått aviseringar.

>[!Note]
> Se [Ställ in aviseringar på Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-alerts) och [övervaka tillgänglighet och svarstider för alla webbplatser](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability).

- Logganalys (Operations Management Suite): Gör hello routning av aktivitet och diagnostikloggar tooLog Analytics. Operations Management Suite kan mått, logg och andra aviseringstyper.

>[!Note]
> Mer information finns i aviseringar i [logganalys](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts).

- Azure övervaka: Gör det möjligt för varningar baserat på både måttvärden och aktivitet logghändelser. Du kan använda hello [REST-API för Azure-Monitor](https://msdn.microsoft.com/library/dn931943.aspx) toomanage aviseringar.

>[!Note]
> Mer information finns i [med hello Azure-portalen, PowerShell eller hello kommandoradsgränssnittet toocreate aviseringar](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal).

### <a name="monitoring"></a>Övervakning

Prestandaproblem i din molnapp kan påverka din verksamhet. Med flera sammankopplade komponenter och ofta versioner kan degradations inträffa när som helst. Och om du utvecklar en app användarna identifiera vanligtvis problem som du inte kan hitta vid testning. Du bör känna till de här problemen omedelbart och har verktyg för att diagnostisera och åtgärda problem med hello. Microsoft Azure har en uppsättning verktyg för att identifiera problemen.

**Hur övervakar jag min Azure-molnappar?**

Det finns ett antal verktyg för övervakning av Azure-program och tjänster. Några av sina funktioner överlappar varandra. Detta är delvis historiska skäl och delvis på grund av toohello oskärpa mellan utveckling och drift av ett program.

Här följer hello huvudsakliga verktyg:

- **Azure-Monitor** är grundläggande verktyg för att övervaka tjänster som körs på Azure. Den ger dig infrastrukturnivå data om hello genomflödet av en tjänst och hello omgivande miljö. Om du hanterar dina appar i Azure, bestämmer om tooscale uppåt eller nedåt resurser sedan Azure-Monitor ger dig vad du använder toostart.

- **Application Insights** kan användas för utveckling och som en produktion övervakningslösning. Det fungerar genom att installera ett paket i din app och så får du en mer interna vy över vad som händer. Data innehåller svarstider för beroenden, undantag spårning, felsökning ögonblicksbilder, körning av profiler. Den innehåller kraftfulla smarta verktyg för analys all telemetri båda toohelp du felsöka en app och toohelp du förstår vad användarna gör med den. Du kan se om en topp i antal svarstider förfaller toosomething i en app eller vissa externa resourcing problemet. Om du använder Visual Studio och hello appen är fel, kan du tas rätt toohello problemet raderna i koden så att du kan åtgärda den.

- **Logga Analytics** för den som behöver tootune prestanda och planera Underhåll på program som körs i produktion. Den är baserad i Azure. Den samlar in och samlar in data från flera källor, men med en fördröjning på 10 minuter för too15. Det ger en heltäckande lösning för IT för Azure, lokalt och från tredje part molnbaserad infrastruktur (till exempel Amazon Web Services). Det ger bättre verktyg tooanalyze data över flera källor, tillåter komplexa frågor över alla loggar och proaktivt kan meddela på angivna villkor. Du kan även samla in anpassade data till den centrala databasen så kan fråga och visualisera den.

- **System Center Operations Manager (SCOM)** är för att hantera och övervaka stora molnet installationer. Du kanske redan känner till den som ett hanteringsverktyg för lokal Windows Sever och baserat Hyper-V-moln, men den kan även integreras med och hantera Azure-appar. Bland annat installera den Application Insights på befintliga live appar. Om en app kraschar, om det i sekunder.


## <a name="next-steps"></a>Nästa steg

- [Bästa praxis för att skapa mallar för Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices).

- [Exempel på att implementera Azure-prenumeration styrning](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-subscription-examples).

- [Microsoft Azure Government](https://docs.microsoft.com/azure/azure-government/).
