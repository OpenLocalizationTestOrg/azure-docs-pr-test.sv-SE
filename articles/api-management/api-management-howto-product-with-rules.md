---
title: aaaProtect din API med Azure API Management | Microsoft Docs
description: "Lär dig hur tooprotect din API med kvoter och begränsning (hastighetsbegränsning) principer."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 450dc368-d005-401d-ae64-3e1a2229b12f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 3113fd277d434da0c051b8b90fd629a102bf4867
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="protect-your-api-with-rate-limits-using-azure-api-management"></a>Skydda ditt API med frekvensbegränsningar med hjälp av Azure API Management
Den här guiden visar hur lätt det är tooadd skydd för din serverdel API genom att konfigurera hastighet gränsen och kvot principer med Azure API Management.

I den här självstudiekursen skapar du en ”kostnadsfri utvärderingsversion” API-produkt som gör att utvecklare toomake too10 anrop per minut och in tooa högst 200 anrop per vecka tooyour API: et med hello [gränsen anropet frekvensen per prenumeration](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) och [ Ange kvot per prenumeration](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) principer. Du kommer sedan publicerar hello API och testa hello hastighet gränsen principen.

För mer avancerade scenarier med hjälp av hello begränsning [hastighet gränsen av nyckeln](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) och [kvoten av nyckeln](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) principer, se [avancerade begäran begränsning med Azure API Management](api-management-sample-flexible-throttling.md).

## <a name="create-product"></a>toocreate en produkt
I det här steget ska du skapa en produkt för en kostnadsfri utvärdering som inte kräver prenumerationsgodkännande.

> [!NOTE]
> Om du redan har en produkt som har konfigurerats och vill toouse den för den här självstudiekursen, kan du hoppa vidare för[konfigurera anropa hastighet principer för begränsning av och kvot] [ Configure call rate limit and quota policies] och följ hello kursen därifrån med hjälp av produkten i stället för hello kostnadsfri utvärderingsversion av produkten.
> 
> 

tooget har startats klickar du på **Publisher portal** i hello Azure-portalen för API Management-tjänsten.

![Utgivarportalen][api-management-management-console]

> Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [hantera din första API i Azure API Management] [ Manage your first API in Azure API Management] kursen.
> 
> 

Klicka på **produkter** i hello **API Management** menyn på hello vänstra toodisplay hello **produkter** sidan.

![Lägg till produkt][api-management-add-product]

Klicka på **Lägg till produkten** toodisplay hello **Lägg till ny produkt** dialogrutan.

![Lägg till ny produkt][api-management-new-product-window]

I hello **rubrik** skriver **kostnadsfri utvärderingsversion**.

I hello **beskrivning** rutan, typen hello följande text: **prenumeranter kommer att kunna toorun 10 anrop per minut in tooa högst 200 anrop i veckan då åtkomst nekas.**

Produkter i API Management kan skyddas eller vara öppna. Skyddade produkter måste vara prenumererade toobefore som de kan användas. Öppna produkter kan användas utan en prenumeration. Se till att **kräver prenumeration** är valda toocreate en skyddad produkt som kräver en prenumeration. Det här är standardinställningen för hello.

Om du vill att en administratör tooreview och godkänna eller avvisa prenumeration försöker toothis produkten, Välj **kräver godkännande för prenumerationen**. Om hello inte är markerad, att prenumeration försök automatiskt godkänd. I det här exemplet godkänns automatiskt prenumerationer, så att inte markerar hello.

tooallow developer konton toosubscribe flera gånger toohello ny produkt, Välj hello **tillåter flera samtidiga prenumerationer** kryssrutan. I den här självstudiekursen ska vi inte använda flera samtidiga prenumerationer och lämnar därför kryssrutan avmarkerad.

När alla värden anges, klickar du på **spara** toocreate hello produkten.

![Produkten läggs till][api-management-product-added]

Som standard är nya produkter synliga toousers i hello **administratörer** grupp. Vi tooadd hello **utvecklare** grupp. Klicka på **kostnadsfri utvärderingsversion**, och klicka sedan på hello **synlighet** fliken.

> Grupper finns i API Management används toomanage hello synligheten för produkter toodevelopers. Produkter ge synlighet toogroups och utvecklare kan visa och prenumerera toohello produkter som är synliga toohello grupper som de tillhör. Mer information finns i [hur toocreate och använda grupper i Azure API Management][How toocreate and use groups in Azure API Management].
> 
> 

![Lägga till en utvecklargrupp][api-management-add-developers-group]

Välj hello **utvecklare** kryssrutan och klicka sedan på **spara**.

## <a name="add-api"></a>tooadd API toohello produkten
I det här steget i självstudiekursen hello vi lägga till hello Echo API toohello nya kostnadsfri utvärderingsversion av produkten.

> Varje instans för API Management-tjänsten har redan konfigurerats med en Echo-API som kan använda tooexperiment med och lär dig mer om API-hantering. Mer information finns i [Hantera ditt första API i Azure API Management][Manage your first API in Azure API Management].
> 
> 

Klicka på **produkter** från hello **API Management** menyn på hello vänster och klicka sedan på **kostnadsfri utvärderingsversion** tooconfigure hello produkten.

![Konfigurera produkten][api-management-configure-product]

Klicka på **lägga till API tooproduct**.

![Lägg till API tooproduct][api-management-add-api]

Välj **Echo API** och klicka på **Spara**.

![Lägg till Echo API][api-management-add-echo-api]

## <a name="policies"></a>tooconfigure anropa hastighet principer för begränsning av och kvot
Hastighetsbegränsningar och kvoter konfigureras i hello redigeraren. hello två principer som vi kommer att lägga till i den här kursen är hello [gränsen anropet frekvensen per prenumeration](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) och [Set kvot per prenumeration](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) principer. Dessa principer tillämpas hello produkten definitionsområdet.

Klicka på **principer** under hello **API Management** menyn hello vänster. I hello **produkten** klickar du på **kostnadsfri utvärderingsversion**.

![Princip för produkt][api-management-product-policy]

Klicka på **Lägg till princip** tooimport hello Principmall och börja skapa hello hastighet gränsen och kvot principer.

![Lägg till princip][api-management-add-policy]

Hastigheten med vilken gränsen och kvot principer är inkommande principer, så position hello markören i hello inkommande element.

![Principredigerare][api-management-policy-editor-inbound]

Rulla igenom hello lista över principer och hitta hello **gränsen anropet frekvensen per prenumeration** post i principen.

![Principrapporter][api-management-limit-policies]

Efter hello markören är placerad i hello **inkommande** principelement, klicka på hello pilen bredvid **gränsen anropet frekvensen per prenumeration** tooinsert mall för Grupprincip.

```xml
<rate-limit calls="number" renewal-period="seconds">
<api name="name" calls="number">
<operation name="name" calls="number" />
</api>
</rate-limit>
```

Hello principen kan inställningsgränser för hello produkten API: er och åtgärder som du kan se från kodutdrag hello. I den här självstudiekursen kommer vi inte använda den här funktionen, så ta bort hello **api** och **åtgärden** element från hello **gräns för överföringshastigheten** elementet så att endast hello yttre **gräns för överföringshastigheten** förblir element som visas i följande exempel hello.

```xml
<rate-limit calls="number" renewal-period="seconds">
</rate-limit>
```

I hello kostnadsfri utvärderingsversion produkten hello högsta tillåtna anropet frekvensen är 10 anrop per minut, så Skriv **10** som hello värde hello **anrop** attribut, och **60** för hello **förnyelseperioden** attribut.

```xml
<rate-limit calls="10" renewal-period="60">
</rate-limit>
```

tooconfigure hello **Set kvot per prenumeration** princip, position markören direkt under hello nyligen tillagda **gräns för överföringshastigheten** element i hello **inkommande** element, leta upp och klickar på hello pilen toohello till vänster i **Set kvot per prenumeration**.

```xml
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
<api name="name" calls="number" bandwidth="kilobytes">
<operation name="name" calls="number" bandwidth="kilobytes" />
</api>
</quota>
```

På liknande sätt toohello **Set kvot per prenumeration** princip, **Set kvot per prenumeration** principen kan ange versaler för för hello produkten API: er och åtgärder. I den här självstudiekursen kommer vi inte använda den här funktionen, så ta bort hello **api** och **åtgärden** element från hello **kvot** element, som visas i följande exempel hello.

```xml
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
</quota>
```

Kvoter kan baseras på hello antal anrop per intervall, bandbredd eller båda. I den här självstudiekursen vi inte begränsa utifrån bandbredd, så ta bort hello **bandbredd** attribut.

```xml
<quota calls="number" renewal-period="seconds">
</quota>
```

I hello kostnadsfri utvärderingsversion produkten är hello kvoten 200 anrop per vecka. Ange **200** som hello värde hello **anrop** attributet och sedan ange **604800** som hello värde hello **förnyelseperioden** attribut.

```xml
<quota calls="200" renewal-period="604800">
</quota>
```

> Principintervall anges i sekunder. toocalculate hello-intervall för en vecka, multiplicera hello antal dagar (7) av hello antal timmar under en dag (24) av hello antalet minuter under en timme (60) av hello antal sekunder per minut (60): 7 * 24 * 60 * 60 = 604800.
> 
> 

När du har konfigurerat hello princip måste den matcha hello följande exempel.

```xml
<policies>
    <inbound>
        <rate-limit calls="10" renewal-period="60">
        </rate-limit>
        <quota calls="200" renewal-period="604800">
        </quota>
        <base />

</inbound>
<outbound>

    <base />

    </outbound>
</policies>
```

När hello önskad principer som har konfigurerats, klickar du på **spara**.

![Spara en princip][api-management-policy-save]

## <a name="publish-product"></a> toopublish hello produkten
Nu när hello hello API: er läggs till och hello principerna är konfigurerade publiceras hello produkten så att den kan användas av utvecklare. Klicka på **produkter** från hello **API Management** menyn på hello vänster och klicka sedan på **kostnadsfri utvärderingsversion** tooconfigure hello produkten.

![Konfigurera produkten][api-management-configure-product]

Klicka på **publicera**, och klicka sedan på **Ja, publicera den** tooconfirm.

![Publicera produkten][api-management-publish-product]

## <a name="subscribe-account"></a>toosubscribe en utvecklare konto toohello produkt
Nu hello produkten publiceras, är tillgängliga toobe prenumererar tooand användas av utvecklare.

> Administratörer av en API Management-instans är automatiskt prenumererade tooevery produkten. I den här självstudiekursen steg ska vi prenumerera på något av hello-administratör developer konton toohello kostnadsfri utvärderingsversion produkt. Om ditt utvecklarkonto ingår i hello administratörsrollen kan följa du tillsammans med det här steget, även om du redan prenumererar.
> 
> 

Klicka på **användare** på hello **API Management** menyn på hello vänster och klicka sedan på hello namnet på ditt utvecklarkonto. I det här exemplet använder vi hello **Clayton Gragg** utvecklarkonto.

![Konfigurera en utvecklare][api-management-configure-developer]

Klicka på **Lägg till prenumeration**.

![Lägg till en prenumeration][api-management-add-subscription-menu]

Välj **Kostnadsfri utvärdering** och klicka sedan på **Prenumerera**.

![Lägg till en prenumeration][api-management-add-subscription]

> [!NOTE]
> I den här självstudiekursen aktiveras inte flera samtidiga prenumerationer för hello kostnadsfri utvärderingsversion av produkten. Om de vore kommer du att tillfrågas tooname hello prenumeration, som visas i följande exempel hello.
> 
> 

![Lägg till en prenumeration][api-management-add-subscription-multiple]

När du klickar på **prenumerera**, hello produkten visas i hello **prenumeration** listan för hello användaren.

![Prenumerationen har lagts till][api-management-subscription-added]

## <a name="test-rate-limit"></a>toocall en åtgärd och testa hello hastighetsbegränsning
Nu när hello kostnadsfri utvärderingsversion produkten har konfigurerats och publicerade, vi anropa vissa åtgärder och testa hello hastighet gränsen principen.
Växeln toohello developer-portalen genom att klicka på **utvecklarportalen** hello övre högra menyn.

![Utvecklarportalen][api-management-developer-portal-menu]

Klicka på **API: er** i hello översta menyn och klicka sedan på **Echo API**.

![Utvecklarportalen][api-management-developer-portal-api-menu]

Klicka på **GET Resource** och klicka sedan på **Prova**.

![Öppna konsolen][api-management-open-console]

Behåller hello standardvärdet parametervärden och välj sedan din prenumeration nyckel för hello kostnadsfri utvärderingsversion av produkten.

![Prenumerationsnyckel][api-management-select-key]

> [!NOTE]
> Om du har flera prenumerationer kan vara säker på att tooselect hello nyckel för **kostnadsfri utvärderingsversion**, eller annan hello principer som konfigurerades i hello föregående steg inte gäller.
> 
> 

Klicka på **skicka**, och sedan visa hello svar. Obs hello **svarsstatusen** av **200 OK**.

![Åtgärdsresultat][api-management-http-get-results]

Klicka på **skicka** med en hastighet som är större än hello hastighet gränsen princip av 10 anrop per minut. När hello hastighet gränsen princip överskrids svarsstatusen **429 för många begäranden** returneras.

![Åtgärdsresultat][api-management-http-get-429]

Hej **svar innehåll** anger hello återstående intervallet innan försök kommer att lyckas.

När hello hastighet gränsen princip av 10 anrop per minut har aktiverats kan efterföljande anrop misslyckas tills 60 sekunder har förflutit från hello första produktens hello 10 antal samtal toohello innan hello hastighetsbegränsning har överskridits. I det här exemplet är hello återstående intervall 54 sekunder.

## <a name="next-steps"> </a>Nästa steg
* Titta på en demonstration av inställningen hastighetsbegränsningar och kvoter i hello följande video.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Rate-Limits-and-Quotas/player]
> 
> 

[api-management-management-console]: ./media/api-management-howto-product-with-rules/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-product-with-rules/api-management-add-product.png
[api-management-new-product-window]: ./media/api-management-howto-product-with-rules/api-management-new-product-window.png
[api-management-product-added]: ./media/api-management-howto-product-with-rules/api-management-product-added.png
[api-management-add-policy]: ./media/api-management-howto-product-with-rules/api-management-add-policy.png
[api-management-policy-editor-inbound]: ./media/api-management-howto-product-with-rules/api-management-policy-editor-inbound.png
[api-management-limit-policies]: ./media/api-management-howto-product-with-rules/api-management-limit-policies.png
[api-management-policy-save]: ./media/api-management-howto-product-with-rules/api-management-policy-save.png
[api-management-configure-product]: ./media/api-management-howto-product-with-rules/api-management-configure-product.png
[api-management-add-api]: ./media/api-management-howto-product-with-rules/api-management-add-api.png
[api-management-add-echo-api]: ./media/api-management-howto-product-with-rules/api-management-add-echo-api.png
[api-management-developer-portal-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-menu.png
[api-management-publish-product]: ./media/api-management-howto-product-with-rules/api-management-publish-product.png
[api-management-configure-developer]: ./media/api-management-howto-product-with-rules/api-management-configure-developer.png
[api-management-add-subscription-menu]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-menu.png
[api-management-add-subscription]: ./media/api-management-howto-product-with-rules/api-management-add-subscription.png
[api-management-developer-portal-api-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-api-menu.png
[api-management-open-console]: ./media/api-management-howto-product-with-rules/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-product-with-rules/api-management-http-get.png
[api-management-http-get-results]: ./media/api-management-howto-product-with-rules/api-management-http-get-results.png
[api-management-http-get-429]: ./media/api-management-howto-product-with-rules/api-management-http-get-429.png
[api-management-product-policy]: ./media/api-management-howto-product-with-rules/api-management-product-policy.png
[api-management-add-developers-group]: ./media/api-management-howto-product-with-rules/api-management-add-developers-group.png
[api-management-select-key]: ./media/api-management-howto-product-with-rules/api-management-select-key.png
[api-management-subscription-added]: ./media/api-management-howto-product-with-rules/api-management-subscription-added.png
[api-management-add-subscription-multiple]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-multiple.png

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How tooadd and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs tooa product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Manage your first API in Azure API Management]: api-management-get-started.md
[How toocreate and use groups in Azure API Management]: api-management-howto-create-groups.md
[View subscribers tooa product]: api-management-howto-add-products.md#view-subscribers
[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps

[Create a product]: #create-product
[Configure call rate limit and quota policies]: #policies
[Add an API toohello product]: #add-api
[Publish hello product]: #publish-product
[Subscribe a developer account toohello product]: #subscribe-account
[Call an operation and test hello rate limit]: #test-rate-limit

[Limit call rate]: https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate
[Set usage quota]: https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota
