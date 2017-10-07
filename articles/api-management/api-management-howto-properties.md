---
title: aaaHow toouse egenskaper i Azure API Management-principer
description: "Lär dig hur toouse egenskaper i Azure API Management-principer."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 6f39b00f-cf6e-4cef-9bf2-1f89202c0bc0
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 1ff096deeb97543b48dcf1f40be9dbfcbcd09542
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-properties-in-azure-api-management-policies"></a>Hur toouse egenskaper i Azure API Management-principer
API Management-principer är en kraftfull funktion hello system som tillåter hello publisher toochange hello funktionssätt hello API via konfiguration. Principer är en samling av instruktioner som utförs i tur och ordning på hello begäran eller svar på en API. Principrapporter kan konstrueras med literal textvärden, principuttrycken och egenskaper. 

Varje instans för API Management-tjänsten har en egenskapssamling av nyckel/värde-par som är global toohello tjänstinstansen. De här egenskaperna kan vara används toomanage konstanta strängvärden för alla API-konfiguration och principer. Varje egenskap har hello följande attribut.

| Attribut | Typ | Beskrivning |
| --- | --- | --- |
| Namn |Sträng |hello namnet på hello-egenskap. Den kan innehålla endast bokstäver, siffror, period, bindestreck och understreck. |
| Värde |Sträng |hello värdet för hello-egenskapen. Kan inte vara tomt eller endast bestå av blanksteg. |
| Hemlighet |Booleskt värde |Anger om hello värde är en hemlighet och ska krypteras eller inte. |
| Taggar |Strängmatris |Valfritt taggar som när det ges kan vara används toofilter hello egenskapslistan. |

Egenskaper som har konfigurerats i hello publisher portal på hello **egenskaper** fliken. I följande exempel hello, konfigureras egenskaper.

![Egenskaper][api-management-properties]

Egenskapsvärden kan innehålla teckensträngar och [principuttrycken](https://msdn.microsoft.com/library/azure/dn910913.aspx). hello följande tabell visar hello föregående tre exempel egenskaper och deras attribut. Hej värdet för `ExpressionProperty` är ett principuttryck som returnerar en sträng som innehåller hello aktuellt datum och tid. Hej egenskapen `ContosoHeaderValue` är markerad som en hemlighet, så dess värde inte visas.

| Namn | Värde | Hemlighet | Taggar |
| --- | --- | --- | --- |
| ContosoHeader |trackingId |False |Contoso |
| ContosoHeaderValue |•••••••••••••••••••••• |True |Contoso |
| ExpressionProperty |@(DateTime.Now.ToString()) |False | |

## <a name="toouse-a-property"></a>toouse en egenskap
toouse en egenskap i en princip, plats hello egenskapsnamn i paret dubbla klammerparenteser som `{{ContosoHeader}}`som visas i följande exempel hello.

```xml
<set-header name="{{ContosoHeader}}" exists-action="override">
  <value>{{ContosoHeaderValue}}</value>
</set-header>
```

I det här exemplet `ContosoHeader` används som hello namnet på en rubrik i en `set-header` principen och `ContosoHeaderValue` används som hello värdet för det huvudet. När den här principen utvärderas under en begäran eller svar toohello API Management gateway, `{{ContosoHeader}}` och `{{ContosoHeaderValue}}` ersättas med deras respektive egenskapsvärden.

Egenskaper som kan användas som slutförts attribut eller elementvärden enligt hello föregående exempel, men de kan också läggs till eller kombineras med en del av ett uttryck med exakt text som visas i följande exempel hello:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

Egenskaper kan också innehålla princip uttryck. I följande exempel hello, hello `ExpressionProperty` används.

```xml
<set-header name="CustomHeader" exists-action="override">
    <value>{{ExpressionProperty}}</value>
</set-header>
```

När den här principen utvärderas `{{ExpressionProperty}}` ersätts med värdet: `@(DateTime.Now.ToString())`. Eftersom hello-värdet är ett principuttryck, hello evalueras och hello princip fortsätter med körningen.

Du kan testa detta ut i hello developer-portalen genom att anropa en åtgärd som har en princip med egenskaper i sitt omfång. I följande exempel hello, kallas en åtgärd med hello två föregående exempel `set-header` principer med egenskaper. Observera att hello svaret innehåller två anpassade huvuden som har konfigurerats med principer med egenskaper.

![Utvecklarportalen][api-management-send-results]

Om du tittar på hello [API Inspector trace](api-management-howto-api-inspector.md) för samtal som innehåller hello två föregående exempel principer med egenskaper, kan du se hello två `set-header` principer med hello egenskapsvärden infogas samt hello principuttryck utvärdering för hello-egenskap som innehöll hello principuttryck.

![API-Inspector spårning][api-management-api-inspector-trace]

Observera att egenskapsvärden inte kan innehålla andra egenskaper medan egenskapsvärden kan innehålla principuttrycken. Om text som innehåller en referens som används för ett egenskapsvärde `Property value text {{MyProperty}}`, att egenskapsreferens inte ersättas och inkluderas som en del av hello egenskapsvärde.

## <a name="toocreate-a-property"></a>toocreate en egenskap
toocreate en egenskap, klickar du på **Lägg till egenskap** på hello **egenskaper** fliken.

![Lägg till egenskap][api-management-properties-add-property-menu]

**Namnet** och **värdet** är värden som krävs. Kontrollera om det här egenskapsvärdet är en hemlighet hello **detta är en hemlighet** kryssrutan. Ange en eller flera valfria taggar toohelp med ordna dina egenskaper och klicka på **spara**.

![Lägg till egenskap][api-management-properties-add-property]

När en ny egenskap sparas hello **söka egenskapen** textruta fylls med hello namnet på hello ny egenskap och hello nya egenskapen visas. toodisplay alla egenskaper, avmarkera hello **söka egenskapen** textrutan och tryck på RETUR.

![Egenskaper][api-management-properties-property-saved]

Information om hur du skapar en egenskap med hello REST-API finns [skapa en egenskap med hello REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).

## <a name="tooedit-a-property"></a>tooedit en egenskap
tooedit en egenskap, klickar du på **redigera** bredvid hello egenskapen tooedit.

![Ändra egenskapen][api-management-properties-edit]

Gör eventuella ändringar och klicka på **spara**. Om du ändrar hello egenskapsnamn är de principer som refererar till den egenskapen uppdateras automatiskt toouse hello nytt namn.

![Ändra egenskapen][api-management-properties-edit-property]

Information om hur du redigerar en egenskap med hello REST-API finns [redigera en egenskap med hello REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).

## <a name="toodelete-a-property"></a>toodelete en egenskap
toodelete en egenskap, klickar du på **ta bort** bredvid hello egenskapen toodelete.

![Ta bort egenskapen][api-management-properties-delete]

Klicka på **Ja, ta bort den** tooconfirm.

![Bekräfta borttagning][api-management-delete-confirm]

> [!IMPORTANT]
> Om egenskapen hello refereras av principerna som du inte toosuccessfully ta bort den tills du tar bort hello egenskap från alla principer som använder den.
> 
> 

Mer information om du tar bort en egenskap med hello REST-API finns [ta bort en egenskap med hello REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).

## <a name="toosearch-and-filter-properties"></a>toosearch och filtrera egenskaper
Hej **egenskaper** innehåller sökning och filtrering funktioner toohelp som du hanterar dina egenskaper. toofilter hello egenskapslistan av egenskapsnamn, ange ett sökvillkor i hello **söka egenskapen** textruta. toodisplay alla egenskaper, avmarkera hello **söka egenskapen** textrutan och tryck på RETUR.

![Söka][api-management-properties-search]

toofilter hello egenskapslistan av värden, ange en eller flera taggar i hello **filtrera efter taggar** textruta. toodisplay alla egenskaper, avmarkera hello **filtrera efter taggar** textrutan och tryck på RETUR.

![Filter][api-management-properties-filter]

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om hur du arbetar med principer
  * [Principer för i API-hantering](api-management-howto-policies.md)
  * [Principreferens](https://msdn.microsoft.com/library/azure/dn894081.aspx)
  * [Principuttryck](https://msdn.microsoft.com/library/azure/dn910913.aspx)

## <a name="watch-a-video-overview"></a>Titta på en videoöversikt
> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Use-Properties-in-Policies/player]
> 
> 

[api-management-properties]: ./media/api-management-howto-properties/api-management-properties.png
[api-management-properties-add-property]: ./media/api-management-howto-properties/api-management-properties-add-property.png
[api-management-properties-edit-property]: ./media/api-management-howto-properties/api-management-properties-edit-property.png
[api-management-properties-add-property-menu]: ./media/api-management-howto-properties/api-management-properties-add-property-menu.png
[api-management-properties-property-saved]: ./media/api-management-howto-properties/api-management-properties-property-saved.png
[api-management-properties-delete]: ./media/api-management-howto-properties/api-management-properties-delete.png
[api-management-properties-edit]: ./media/api-management-howto-properties/api-management-properties-edit.png
[api-management-delete-confirm]: ./media/api-management-howto-properties/api-management-delete-confirm.png
[api-management-properties-search]: ./media/api-management-howto-properties/api-management-properties-search.png
[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

