---
title: "aaaGuide toocreating en datatjänst för hello Marketplace | Microsoft Docs"
description: "Detaljerade anvisningar hur toocreate, certifiera och distribuera en Data-tjänst för att köpa på hello Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 96194198-6991-43b4-918e-ee337e250339
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 0220d357ae0ec89e7d4f6399605850e57c646f73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-service-publishing-guide-for-hello-azure-marketplace"></a>Data Service-publiceringsguide för hello Azure Marketplace
> [!IMPORTANT]
> **Just nu vi inte längre onboarding några nya Data Service-utgivare. Nya dataservices kommer inte godkännas för lista.** Om du har ett SaaS-affärsprogram som toopublish på AppSource hittar du mer information [här](https://appsource.microsoft.com/partners). Om du har en IaaS-program eller utvecklare tjänst du skulle t.ex toopublish på Azure Marketplace hittar du mer information [här](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

När du har slutfört steg 1, för hello [konto skapas och registrering](marketplace-publishing-accounts-creation-registration.md), vi Interaktiv du via hello [allmänna icke-tekniska](marketplace-publishing-pre-requisites.md) och [tekniska krav](marketplace-publishing-data-service-creation-prerequisites.md) av en datatjänst erbjuder på Azure Marketplace. Nu går du igenom hello steg för att skapa ett Data tjänsterbjudande på hello [Publiceringsportal] [ link-pubportal] för hello Azure Marketplace.

## <a name="1----login-toohello-publishing-portal"></a>1.    Inloggningen toohello Publishing Portal.
Gå för[https://publish.windowsazure.com](https://publish.windowsazure.com.)

**För första gången inloggningen tooPublishing Portal, använder du hello samma konto som företagets säljare profilen har registrerats i Developer Center.**  (Senare du kan lägga till anställda på företaget som medadministratör i hello Publiceringsportal).

Klicka på hello **publicera en datatjänster** panelen om det här är hello första inloggningen till hello publishing portal.

## <a name="2----choose-data-services-in-hello-navigation-menu-on-hello-left-side"></a>2.    Välj **datatjänster** i hello navigationsmenyn hello vänster sida.
  ![Rita](media/marketplace-publishing-data-service-creation/pubportal-main-nav.png)

## <a name="3----create-a-new-data-service"></a>3.    Skapa en ny datatjänst
Fyll i hello rubrik för din nya erbjuder för Data-tjänsten och klicka på ”+” på hello rätt.

  ![Rita](media/marketplace-publishing-data-service-creation/step-3.png)

## <a name="4----review-hello-sub-menu-under-hello-newly-created-data-service-in-hello-navigation-menu"></a>4.    Granska hello undermeny under hello nyskapad datatjänst i hello navigeringsmenyn.
Klicka på hello **genomgången** och granska alla nödvändiga steg behövs toopublish korrekt hello datatjänst på hello Azure Marketplace.

> [!TIP]
> Du kan alltid klickar du på hello länkar i hello ”genomgången” sidan eller Använd flikarna på hello datatjänst erbjudande undermeny hello vänster.
> 
> 

## <a name="5----create-a-new-plan"></a>5.    Skapa en ny Plan.
### <a name="offers-plans-transactions"></a>Erbjudanden planer, transaktioner.
Varje erbjudande kan ha flera planer, men måste ha minst en (1) Plan. När slutanvändarna prenumererar tooyour erbjudande prenumerera de för en hello erbjudande Plan. Varje plan definierar hur slutanvändarna kommer att kunna toouse din tjänst.

För närvarande månatlig prenumeration transaktion baserat modellen stöd för Azure Marketplace för Data Services, d.v.s. slutanvändare betalar månadsavgift enligt toohello priset för hello specifik plan de prenumererar tooand kommer att kunna tooconsume varje månad antal transaktionen som definieras av hello plan.

Varje transaktion som vanligtvis definieras som antalet poster som returnerar Data Service baserat på hello fråga skickas toohello Service. hello standardvärdet är 100. Antal transaktioner returnerade tooeach frågan kommer att antalet poster dividerat med 100 och avrundat toohello närmaste heltal.

Det är Azure Marketplace-tjänsten layer ansvar toomonitor (mätaren) antal transaktioner som används av varje fråga.

> [!IMPORTANT]
> Slutanvändare som nått gränsen för hello transaktion under hello månad kommer att blockeras från fortsätter toouse hello service till slutet av deras månatlig prenumeration cykel.
> 
> hello plan eller någon av hello planer kan (men måste inte) innehåller obegränsat antal transaktioner.
> 
> 

### <a name="create-a-plan"></a>Skapa en plan.
1. Klicka på **”+”** nästa toohello ”Lägg till en ny plan”.
2. Välj ett av alternativen hello: **obegränsad** eller **begränsad** användning för den här planen.  Om begränsad ange hello antalet transaktion hello planen kan tooconsume i en månad.
   
    ![Rita](media/marketplace-publishing-data-service-creation/step-5.1.png)  
   
    Publicera Portal kommer också föreslå ”identifieraren för Cacheuppdateringsplanen”, som kommer att använda toocommunicate toohello slutanvändare hello namnet på hello plan i hello UI och används också av hello marknadsplatsen Service tooidentify hello Plan. Du kan ändra hello ”planera identifierare” om du vill.
   
   > [!NOTE]
   > Hej ”planera identifierare” måste vara unika inom hello omfånget för varje erbjudande. Så många andra identifierare som används i hello Publishing Portal planera identifierare låses efter hello första publishing tooproduction och du inte kommer att kunna toochange den här identifieraren.
   > 
   > 
3. Klicka på tooaccept valet.
4. Du kommer att få ytterligare några frågor om nyskapade planen.
   
    ![Rita](media/marketplace-publishing-data-service-creation/step-5.2.png)

| Fråga | Betydelse |
| --- | --- |
| **Den här planen är ledig och tillgänglig över hela världen?** |Du kan skapa en plan för helt kostnadsfri-för-kostnad. Om dess hello bara planera för det här erbjudandet – innebär det att du publicerar ”kostnadsfritt erbjuda” i hello Marketplace. Om det är bara för ett (få) Plan, hello den ger dig en alternativet toooffer slutanvändare toolearn mer om din tjänst med ett relativt litet antal transaktioner per månad.  Om hello svaret är ”Ja”, och inga ytterligare frågor. |

> [!NOTE]
> Slutanvändare kan alltid uppgradera toohello betald planer.
> 
> 

| Fråga | Betydelse |
| --- | --- |
| **Finns kostnadsfri utvärderingsversion?** |Du kan välja mellan ”Nej utvärderingsversion” alls eller ge en alternativ toouse din Plan för ”en månad”. Utgivare som toouse det här alternativet tooprovide slutanvändare hello möjligheten toounderstand hello fördelarna med hello kostnadsfritt erbjuda i en månad. |

> [!IMPORTANT]
> Slutanvändare ska kunna toopurchase en kostnadsfri utvärderingsversion om de har etablerat betalningsalternativ t.ex. kreditkort, enterprise-avtal.
> 
> Efter en månad för kostnadsfri utvärderingsversion av hello kommer Azure Marketplace startas debitering kunder hello priset för hello datum för hello prenumerationen om hello kunden initierade hello prenumeration annullering. Inget särskilt meddelande anges toohello slutanvändare.
> 
> 

| Fråga | Betydelse |
| --- | --- |
| **Den här planen kräver en befordran kod toopurchase?** |Utgivare har en alternativet toolimit åtkomst tootheir serviceplaner genom att tillhandahålla en särskild kod som kallas ”A Promocode” toospecific kunder. Slutanvändare som har den här Promocode kommer att kunna toosubscribe toohello Plan. Om du väljer ”Nej”, så att du godkänner alla från hello region där hello erbjuder är tillgänglig (se [Marketplace Marketing Content guiden](marketplace-publishing-push-to-staging.md) för mer information) kommer att kunna toosubscribe toothis plan. Inga ytterligare frågor blir ombedd. |
| **Dessutom dölja den här planen som inte har en giltig befordran koden?** |Om hello svaret toohello föregående fråga är ”Yes” har hello utgivare en alternativet toocompletely ta bort den här planen från som visas i hello Användargränssnittet för hello Marketplace. Det innebär kunder inte ser den här planen hello erbjudande information på sidan. Slutanvändare som tar emot en promocode toopurchase, kommer att kunna toosubscribe tooit med hjälp av den här promocode. |

## <a name="6----create-your-marketplace-marketing-content"></a>6.    Skapa din Marketplace marknadsföring innehåll
För hur tooprovide information krävs i **marknadsföring, prissättning, Support och kategorier** flikar besök [Marketplace Marketing Content guiden](marketplace-publishing-push-to-staging.md) som är vanliga tooall artefakter som publicerats i hello Azure Marketplace.  

## <a name="7----connect-your-offer-tooyour-service-sql-azure-based-or-web-service-based"></a>7.    Anslut din erbjudande tooyour (SQL Azure-baserat eller webbtjänst baserat).
Klicka på hello **datatjänster** undermeny.

På hello övre hälften av hello sidan blir du ombedd tooprovide hello erbjudande **Namespace**.  

  ![Rita](media/marketplace-publishing-data-service-creation/step-7.png)

hello nedan fråga definierar hur hello Publisher kommer tooexpose nyskapad erbjudande tooAzure Marketplace. (Mer information finns hello [Data Services tekniska nödvändiga Guide](marketplace-publishing-data-service-creation-prerequisites.md)).

  ![Rita](media/marketplace-publishing-data-service-creation/step-7.2.png)

**Publicera hello baserat databasen service**

Klicka på **databasen**. hello följande sida visas:

  ![Rita](media/marketplace-publishing-data-service-creation/step-7.3.png)

toocreate en CSDL-mappning för hello Dataset baserat på hello SQL Azure DB:

  ![Rita](media/marketplace-publishing-data-service-creation/step-7.4.png)

Och sedan för varje tabell

  ![Rita](media/marketplace-publishing-data-service-creation/step-7.5.png)

  ![Rita](media/marketplace-publishing-data-service-creation/step-7.6.png)

Om webbtjänsten

  ![Rita](media/marketplace-publishing-data-service-creation/step-7.7.png)

> [!IMPORTANT]
> Läs [mappning av en befintlig web service tooOData via CSDL](marketplace-publishing-data-service-creation-odata-mapping.md) detaljerade anvisningar och exempel för att skapa en CSDL-webbtjänst.
> 
> 

## <a name="next-steps"></a>Nästa steg
Nu när du har skapat ditt Data tjänsterbjudande, se till att du har slutfört hello instruktionerna i hello [Marketplace Marketing Content guiden](marketplace-publishing-push-to-staging.md) innan du går framåt för[testa Data Service i Förproduktion](marketplace-publishing-data-service-test-in-staging.md).

## <a name="see-also"></a>Se även
* [Komma igång: Hur toopublish ett erbjudande toohello Azure Marketplace](marketplace-publishing-getting-started.md)
* Om du vill förstå hello övergripande processen för OData-mappning och ändamål, den här artikeln [mappning för OData-tjänsten](marketplace-publishing-data-service-creation-odata-mapping.md) tooreview definitioner, strukturer och instruktioner.
* Om du är intresserad av utbildning och förstå hello specifika noder och deras parametrar kan den här artikeln [mappa Data Service OData-noder](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) definitioner och förklaringar, exempel och Använd case kontext.
* Om du vill granska exempel den här artikeln [mappa Data Service OData-exempel](marketplace-publishing-data-service-creation-odata-mapping-examples.md) toosee exempelkoden och förstå kodsyntax och kontext.

[link-pubportal]:https://publish.windowsazure.com
