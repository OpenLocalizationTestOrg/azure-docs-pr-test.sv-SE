---
title: 'Machine Learning rekommendationer: JavaScript Integration | Microsoft Docs'
description: "Azure Machine Learning rekommendationer - integrering för JavaScript - dokumentation"
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: bbbb5bb6-489d-4a62-a2ae-f36237e9e2e1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 03/31/2017
ms.author: luisca
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: TRUE
ms.openlocfilehash: 8f27962d097bffc2a03de80244ae41d6573a4bf3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-machine-learning-recommendations---javascript-integration"></a>Azure Machine Learning-rekommendationer – JavaScript-integrering
> [!NOTE]
> Du bör börja använda kognitiva Rekommendationstjänsten API i stället för den här versionen. Kognitiva Rekommendationstjänsten ersätter den här tjänsten och de nya funktionerna det kommer fram. Den har nya funktioner som batchbearbetning support, bättre API Explorer, en tydligare API underlag, mer konsekvent signup/fakturerings experience och så vidare.
> Lär dig mer om [migrera till den nya kognitiva tjänsten](http://aka.ms/recomigrate)
> 
> 

Det här dokumentet beskriver hur du integrerar din webbplats med JavaScript. JavaScript kan du skicka händelser för datainsamling och använda rekommendationer när du skapar en rekommendation modell. Alla åtgärder via JS kan även göras från serversidan.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="1-general-overview"></a>1. Allmänt: översikt
Integrera din webbplats med Azure ML rekommendationer består på 2 faser:

1. Skicka händelser till Azure ML rekommendationer. Detta gör att du skapar en rekommendation modell.
2. Använda rekommendationer. När modellen har skapats kan du använda rekommendationer. (Det här dokumentet inte beskriver hur du skapar en modell, Läs snabbstartsguiden vill ha mer information om hur).

<ins>Fas I</ins>

I den första fasen in html-sidor en liten JavaScript-bibliotek som gör att sidan för att skicka händelser när de inträffar på HTML-sidan i Azure ML rekommendationer servrar (via Data marknaden):

![Drawing1][1]

<ins>Fas II</ins>

I den andra fasen när du vill visa rekommendationerna på sidan Välj något av följande alternativ:

1. servern (på i vilken fas av rendering på sidan) anropar Azure ML rekommendationer Server (via Data marknaden) för att få rekommendationer. Resultatet innehåller en lista med objekt-ID: t. Servern behöver förbättra resultatet med objekt metadata (t.ex. bilder, beskrivning) och skicka sidan som du har skapat till webbläsaren.

![Drawing2][2]

2. det andra alternativet är att använda små JavaScript-filen från den första fasen för att hämta en enkel lista över rekommenderade objekt. Mottagna här data är smidigare än det i det första alternativet.

![Drawing3][3]

## <a name="2-prerequisites"></a>2. Krav
1. Skapa en ny modell som använder API: erna. Om hur du gör finns i snabbstartsguiden.
2. Koda din &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; med base64. (Detta ska användas för grundläggande autentisering för att aktivera JS-kod för att anropa API: er).

## <a name="3-send-data-acquisition-events-using-javascript"></a>3. Skicka datainsamling händelser med hjälp av JavaScript
Följande steg underlätta skicka händelser:

1. Inkludera JQuery biblioteket i din kod. Du kan ladda ned det från nuget i följande URL.
   
     http://www.nuget.org/Packages/jQuery/1.8.2
2. Innehåller rekommendationer JavaScript-bibliotek från följande URL: http://aka.ms/RecoJSLib1
3. Starta Azure ML rekommendationer biblioteket med lämpliga parametrar.
   
     <script>AzureMLRecommendationsStart (”<base64encoding of username:key>”, ”< model_id >”); </script> 
4. Skicka en händelse. Detaljerad avsnittet nedan på alla typer av händelser (exempel på Välj händelsen) <script> om (typeof AzureMLRecommendationsEvent == ”Odefinierad”) {         
                     AzureMLRecommendationsEvent = []; } AzureMLRecommendationsEvent.push({event: "click", item: "18321116"});</script>

### <a name="31----limitations-and-browser-support"></a>3.1.    Begränsningar och stöd för webbläsare
Detta är en referensimplementering och ges eftersom. Det ska ha stöd för alla större webbläsare.

### <a name="32----type-of-events"></a>3.2.    Typ av händelser
Det finns 5 typer av händelser som har stöd för biblioteket: på, rekommendation klickar på Lägg till till köp vagnen tas bort från köp kundvagn och inköp. Det finns ytterligare en händelse som används för att ange användarkontext kallas inloggningen.

#### <a name="321-click-event"></a>3.2.1. Klicka på händelse
Den här händelsen ska användas när en användare klickade på ett objekt. Vanligtvis när användaren klickar på ett objekt öppnas en ny sida med detaljer om objektet; den här händelsen ska utlösas på den här sidan.

Parametrar:

* händelse (sträng, obligatoriska) ”Klicka”
* objekt (sträng, obligatoriska) - Unik identifierare för objektet
* itemName (sträng, valfritt) - namnet på objektet
* itemDescription (sträng, valfritt) – beskrivning av artikeln
* itemCategory (sträng, valfritt) - kategorin för objektet
  
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "click", item: "3111718" });
        </script>

Eller med valfria data:

        <script>
            if (typeof AzureMLRecommendationsEvent === "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "click", item: "3111718", itemName: "Plane", itemDescription: "It is a big plane", itemCategory: "Aviation"});
        </script>


#### <a name="322-recommendation-click-event"></a>3.2.2. Rekommendation klickar du på händelse
Den här händelsen ska användas när en användare klickade på ett objekt som tagits emot från Azure ML rekommendationer som ett rekommenderade objekt. Vanligtvis när användaren klickar på ett objekt öppnas en ny sida med detaljer om objektet; den här händelsen ska utlösas på den här sidan.

Parametrar:

* händelse (sträng, obligatoriska) ”recommendationclick”
* objekt (sträng, obligatoriska) - Unik identifierare för objektet
* itemName (sträng, valfritt) - namnet på objektet
* itemDescription (sträng, valfritt) – beskrivning av artikeln
* itemCategory (sträng, valfritt) - kategorin för objektet
* Initierar (Strängmatrisen, valfritt) - frö som genererade rekommendation frågan.
* recoList (Strängmatrisen, valfritt) - resultatet av rekommendation begäran som skapade det objekt som användes.
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "recommendationclick", item: "18899918" });
        </script>

Eller med valfria data:

        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: eventName, item: "198", itemName: "Plane2", itemDescription: "It is a big plane2", itemCategory: "Default2", seeds: ["Seed1", "Seed2"], recoList: ["199", "198", "197"]                 });
        </script>


#### <a name="323-add-shopping-cart-event"></a>3.2.3. Lägg till i kundvagn-händelse
Den här händelsen ska användas när användaren lägger till ett objekt i kundvagnen.
Parametrar:

* händelse (sträng, obligatoriska) ”addshopcart”
* objekt (sträng, obligatoriska) - Unik identifierare för objektet
* itemName (sträng, valfritt) - namnet på objektet
* itemDescription (sträng, valfritt) – beskrivning av artikeln
* itemCategory (sträng, valfritt) - kategorin för objektet
  
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "addshopcart", item: "13221118" });
        </script>

#### <a name="324-remove-shopping-cart-event"></a>3.2.4. Ta bort Shopping kundvagn händelse
Den här händelsen ska användas när användaren tar bort ett objekt i kundvagnen.

Parametrar:

* händelse (sträng, obligatoriska) ”removeshopcart”
* objekt (sträng, obligatoriska) - Unik identifierare för objektet
* itemName (sträng, valfritt) - namnet på objektet
* itemDescription (sträng, valfritt) – beskrivning av artikeln
* itemCategory (sträng, valfritt) - kategorin för objektet
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "removeshopcart", item: "111118" });
        </script>

#### <a name="325-purchase-event"></a>3.2.5. Köp händelse
Den här händelsen ska användas när användaren har köpt hans kundvagn.

Parametrar:

* händelse (sträng) ”köpa”
* objekt (inköpta []) - matris som innehåller en post för varje objekt har köpt.<br><br>
  Köpta format:
  * objekt (sträng) - Unik identifierare för objektet.
  * Antal (int eller string) - objekt som har köpts.
  * pris (flyttal eller sträng) - valfria fält - priset för artikeln.

Exemplet nedan visar inköp av 3 objekt (33, 34, 35), två med alla fält (artikel, antal, pris) och en (artikel 34) utan ett pris.

        <script>
            if ( typeof AzureMLRecommendationsEvent == "undefined"){ AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "purchase", items: [{ item: "33", count: "1", price: "10" }, { item: "34", count: "2" }, { item: "35", count: "1", price: "210" }] });
        </script>

#### <a name="326-user-login-event"></a>3.2.6. Användaren logga in händelsen
Azure ML rekommendationer händelse biblioteket skapar och använder en cookie för att identifiera händelser som kommer från samma webbläsare. För att kunna förbättra modellen resultaten Azure ML rekommendationer kan du ange en unik identifiering för användaren som åsidosätter cookie-användning.

Den här händelsen ska användas när du loggar in som användare till din webbplats.

Parametrar:

* händelse (sträng) ”userlogin”
* användare (sträng) unik identifiering av användaren.
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "userlogin", user: “ABCD10AA” });
        </script>

## <a name="4-consume-recommendations-via-javascript"></a>4. Använda rekommendationer via JavaScript
Den kod som förbrukar rekommendationen utlöses av vissa JavaScript-händelse av klientens webbsidan. Rekommendation svaret innehåller rekommenderade objekt-ID, namn och deras klassificeringar. Det är bäst att använda det här alternativet endast för en lista visas de rekommenderade objekten - mer komplexa hantering (till exempel att lägga till objektets metadata) ska göras på server-sida-integration.

### <a name="41-consume-recommendations"></a>4.1 använda rekommendationer
Om du vill använda innehåller rekommendationer som du behöver JavaScript-bibliotek som krävs på sidan och för att anropa AzureMLRecommendationsStart. Avsnittet 2.

Att använda rekommendationer för ett eller flera objekt som du behöver anropa en metod som kallas: AzureMLRecommendationsGetI2IRecommendation.

Parametrar:

* objekt (strängmatris) - en eller flera objekt att få rekommendationer för. Om du använder en Fbt-version kan du ange här bara ett objekt.
* numberOfResults (int) - antal resultat som krävs.
* includeMetadata (boolean, valfritt) - om inställd på 'true' innebär att metadata-fältet måste ha fyllts i resultatet.
* Bearbetning av funktionen - en funktion som hanterar rekommendationerna som returneras. Data returneras en matris med:
  * Artikel - objektet unikt id
  * Name - objektet namn (om det finns i katalogen)
  * klassificeringen - rekommendation klassificering
  * metadata - en sträng som representerar metadata för objektet

Exempel: Följande kod begär 8 rekommendationer för objektet ”64f6eb0d-947a-4c18-a16c-888da9e228ba” (och inte anger includeMetadata - implicit står det krävs inga metadata), sammanfoga sedan resultaten i en buffert.

        <script>
             var reco = AzureMLRecommendationsGetI2IRecommendation(["64f6eb0d-947a-4c18-a16c-888da9e228ba"], 8, false, function (reco) {
                 var buff = "";
                 for (var ii = 0; ii < reco.length; ii++) {
                       buff += reco[ii].item + "," + reco[ii].name + "," + reco[ii].rating + "\n";
                 }
                 alert(buff);
            });
        </script>


[1]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing1.png
[2]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing2.png
[3]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing3.png
