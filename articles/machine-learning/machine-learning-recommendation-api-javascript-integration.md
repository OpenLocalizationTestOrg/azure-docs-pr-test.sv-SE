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
redirect_document_id: True
ms.openlocfilehash: 4c5f0eee4aa04ce823321d52985374c52850f0d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-machine-learning-recommendations---javascript-integration"></a>Azure Machine Learning-rekommendationer – JavaScript-integrering
> [!NOTE]
> Du bör börja använda hello kognitiva rekommendationer API-tjänsten i stället för den här versionen. hello kognitiva Rekommendationstjänsten ersätter den här tjänsten och alla nya funktioner för hello det kommer fram. Den har nya funktioner som batchbearbetning support, bättre API Explorer, en tydligare API underlag, mer konsekvent signup/fakturerings experience och så vidare.
> Lär dig mer om [migrera toohello ny kognitiva tjänst](http://aka.ms/recomigrate)
> 
> 

Det här dokumentet beskriver hur toointegrate webbplatsen med hjälp av JavaScript. hello JavaScript kan du toosend datainsamling händelser och tooconsume rekommendationer när du skapar en rekommendation modell. Alla åtgärder via JS kan även göras från serversidan.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="1-general-overview"></a>1. Allmänt: översikt
Integrera din webbplats med Azure ML rekommendationer består på 2 faser:

1. Skicka händelser till Azure ML rekommendationer. Detta aktiverar toobuild en rekommendation modell.
2. Använda hello rekommendationer. När hello modellen har skapats kan du använda hello rekommendationer. (Det här dokumentet beskriver inte hur hello toobuild en modell, läsa Snabbstart guiden tooget mer information om hur).

<ins>Fas I</ins>

I hello hello första fasen i en liten JavaScript-bibliotek som gör att infoga i HTML-sidor toosend händelser när de inträffar på hello HTML-sida i Azure ML rekommendationer servrar (via Data marknaden):

![Drawing1][1]

<ins>Fas II</ins>

I hello välja andra fasen när du vill tooshow hello rekommendationer om hello sidan du något av följande alternativ för hello:

1. servern (på hello fas av rendering på sidan) anropar Azure ML rekommendationer Server (via Data marknaden) tooget rekommendationer. hello resultatet innehåller en lista med objekt-ID: t. Servern måste tooenrich hello resultat med hello objekt metadata (t.ex. bilder, beskrivning) och skicka hello skapade sidan toohello webbläsare.

![Drawing2][2]

2. hello är andra alternativet toouse hello små JavaScript-fil från fas en tooget en enkel lista över rekommenderade objekt. mottagna här hello data är smidigare än hello något i hello första alternativet.

![Drawing3][3]

## <a name="2-prerequisites"></a>2. Krav
1. Skapa en ny modell med hello API: er. Om hur finns hello snabbstartsguiden toodo den.
2. Koda din &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; med base64. (Detta används för hello grundläggande autentisering tooenable hello JS kod toocall hello API: er).

## <a name="3-send-data-acquisition-events-using-javascript"></a>3. Skicka datainsamling händelser med hjälp av JavaScript
hello följande underlätta skicka händelser:

1. Inkludera JQuery biblioteket i din kod. Du kan ladda ned det från nuget i hello följande URL: en.
   
     http://www.nuget.org/Packages/jQuery/1.8.2
2. Inkludera hello rekommendationer JavaScript-bibliotek från hello följande URL: http://aka.ms/RecoJSLib1
3. Initiera Azure ML rekommendationer bibliotek med hello lämpliga parametrar.
   
     <script>AzureMLRecommendationsStart (”<base64encoding of username:key>”, ”< model_id >”); </script> 
4. Skicka hello händelse. Detaljerad avsnittet nedan på alla typer av händelser (exempel på Välj händelsen) <script> om (typeof AzureMLRecommendationsEvent == ”Odefinierad”) {         
                     AzureMLRecommendationsEvent = []; } AzureMLRecommendationsEvent.push({event: "click", item: "18321116"});</script>

### <a name="31----limitations-and-browser-support"></a>3.1.    Begränsningar och stöd för webbläsare
Detta är en referensimplementering och ges eftersom. Det ska ha stöd för alla större webbläsare.

### <a name="32----type-of-events"></a>3.2.    Typ av händelser
Det finns 5 typer av händelser som hello biblioteket stöder: klickar du på, rekommendation klickar på Lägg till tooShop kundvagn, ta bort från köp kundvagn och inköp. Det finns ytterligare en händelse som används tooset hello användarkontext kallas inloggningen.

#### <a name="321-click-event"></a>3.2.1. Klicka på händelse
Den här händelsen ska användas när en användare klickade på ett objekt. Vanligtvis när användaren klickar på ett objekt öppnas en ny sida med detaljer om objektet hello; den här händelsen ska utlösas på den här sidan.

Parametrar:

* händelse (sträng, obligatoriska) ”Klicka”
* objekt (sträng, obligatoriska) - Unik identifierare för hello objekt
* itemName (sträng, valfritt) - hello namnet på hello objekt
* itemDescription (sträng, valfritt) - hello beskrivning av hello objektet
* itemCategory (sträng, valfritt) - hello kategori hello objekt
  
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
Den här händelsen ska användas när en användare klickade på ett objekt som tagits emot från Azure ML rekommendationer som ett rekommenderade objekt. Vanligtvis när användaren klickar på ett objekt öppnas en ny sida med detaljer om objektet hello; den här händelsen ska utlösas på den här sidan.

Parametrar:

* händelse (sträng, obligatoriska) ”recommendationclick”
* objekt (sträng, obligatoriska) - Unik identifierare för hello objekt
* itemName (sträng, valfritt) - hello namnet på hello objekt
* itemDescription (sträng, valfritt) - hello beskrivning av hello objektet
* itemCategory (sträng, valfritt) - hello kategori hello objekt
* frö (Strängmatrisen, valfritt) - hello frö som genererade hello rekommendation frågan.
* recoList (Strängmatrisen, valfritt) - hello resultatet av hello rekommendation begäran som genererade hello-objekt som du har klickat på.
  
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
Den här händelsen ska användas när hello användare lägger till ett objekt toohello kundvagn.
Parametrar:

* händelse (sträng, obligatoriska) ”addshopcart”
* objekt (sträng, obligatoriska) - Unik identifierare för hello objekt
* itemName (sträng, valfritt) - hello namnet på hello objekt
* itemDescription (sträng, valfritt) - hello beskrivning av hello objektet
* itemCategory (sträng, valfritt) - hello kategori hello objekt
  
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "addshopcart", item: "13221118" });
        </script>

#### <a name="324-remove-shopping-cart-event"></a>3.2.4. Ta bort Shopping kundvagn händelse
Den här händelsen ska användas när hello användaren tar bort en artikel toohello kundvagn.

Parametrar:

* händelse (sträng, obligatoriska) ”removeshopcart”
* objekt (sträng, obligatoriska) - Unik identifierare för hello objekt
* itemName (sträng, valfritt) - hello namnet på hello objekt
* itemDescription (sträng, valfritt) - hello beskrivning av hello objektet
* itemCategory (sträng, valfritt) - hello kategori hello objekt
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "removeshopcart", item: "111118" });
        </script>

#### <a name="325-purchase-event"></a>3.2.5. Köp händelse
Den här händelsen ska användas när användaren hello köpte hans kundvagn.

Parametrar:

* händelse (sträng) ”köpa”
* objekt (inköpta []) - matris som innehåller en post för varje objekt har köpt.<br><br>
  Köpta format:
  * objekt (sträng) - Unik identifierare för hello-objektet.
  * Antal (int eller string) - objekt som har köpts.
  * pris (flyttal eller sträng) - Fritextfält - hello priset hello-objektet.

hello exemplet nedan visar inköp av 3 objekt (33, 34, 35), två med alla fält (artikel, antal, pris) och en (artikel 34) utan ett pris.

        <script>
            if ( typeof AzureMLRecommendationsEvent == "undefined"){ AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "purchase", items: [{ item: "33", count: "1", price: "10" }, { item: "34", count: "2" }, { item: "35", count: "1", price: "210" }] });
        </script>

#### <a name="326-user-login-event"></a>3.2.6. Användaren logga in händelsen
Azure ML rekommendationer händelse biblioteket skapar och Använd en cookie i ordning tooidentify händelser som kommer från hello samma webbläsare. I ordning tooimprove hello modell kan resultat Azure ML rekommendationer tooset en unik identifiering för användaren som åsidosätter hello cookie användning.

Den här händelsen ska användas efter hello användaren logga in tooyour.

Parametrar:

* händelse (sträng) ”userlogin”
* användare (sträng) unik identifiering av hello användare.
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "userlogin", user: “ABCD10AA” });
        </script>

## <a name="4-consume-recommendations-via-javascript"></a>4. Använda rekommendationer via JavaScript
hello-kod som förbrukar hello rekommendation utlöses av vissa JavaScript-händelse av webbsidan hello-klienten. hello rekommendation svaret innehåller hello rekommenderade objekt-ID: N, deras namn och deras klassificering. Det är bästa toouse det här alternativet endast för en lista över visning av hello rekommenderade objekt - mer komplexa hantering (till exempel att lägga till hello objektmetadata) ska göras på hello server sida-integration.

### <a name="41-consume-recommendations"></a>4.1 använda rekommendationer
tooconsume rekommendationer som du behöver tooinclude hello krävs JavaScript-bibliotek på sidan och toocall AzureMLRecommendationsStart. Avsnittet 2.

tooconsume rekommendationer för ett eller flera objekt som du behöver toocall anropade en metod: AzureMLRecommendationsGetI2IRecommendation.

Parametrar:

* objekt (strängmatris) - en eller flera objekt tooget rekommendationer för. Om du använder en Fbt-version kan du ange här bara ett objekt.
* numberOfResults (int) - antal resultat som krävs.
* includeMetadata (boolean, valfritt) - om ange too'true' anger hello metadata fältet måste ha fyllts i hello resultat.
* Bearbetning av funktionen - en funktion som hanterar hello rekommendationer returneras. hello data returneras en matris med:
  * Artikel - objektet unikt id
  * Name - objektet namn (om det finns i katalogen)
  * klassificeringen - rekommendation klassificering
  * metadata - en sträng som representerar hello metadata för hello objekt

Exempel: följande kod hello begär 8 rekommendationer för objektet ”64f6eb0d-947a-4c18-a16c-888da9e228ba” (och inte anger includeMetadata - implicit står det krävs inga metadata), sammanfoga sedan hello resultat i en buffert.

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
