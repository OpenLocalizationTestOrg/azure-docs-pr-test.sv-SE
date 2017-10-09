---
title: aaaPolicies i Azure API Management | Microsoft Docs
description: "Lär dig hur toocreate, redigera och konfigurera principer i API-hantering."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 537e5caf-708b-430e-a83f-72b70af28aa9
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 9ab0f884a655004cb10c05085034df1795f512e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="policies-in-azure-api-management"></a>Principer i Azure API Management
I Azure API Management är-principer en kraftfull funktion hello system som tillåter hello publisher toochange hello funktionssätt hello API via konfiguration. Principer är en samling av instruktioner som utförs i tur och ordning på hello begäran eller svar på en API. Populära instruktioner med Formatkonvertering från XML-tooJSON och anropa hastighet begränsa toorestrict hello mängden inkommande samtal från en utvecklare. Det finns många fler principer out of box hello.

Se hello [principreferens] [ Policy Reference] för en fullständig lista över principrapporter och deras inställningar.

Principerna tillämpas i hello-gateway som placeras mellan hello API konsumenten och hello hanterade API. hello gateway tar emot alla förfrågningar och vanligtvis vidarebefordrar dem utan ändringar toohello underliggande API. En princip kan dock använda ändringar tooboth hello inkommande begäran och utgående svar.

Principen uttryck kan användas som attributvärden eller textvärden i någon av hello API Management-principer, såvida inte hello principen anger annars. Vissa principer, till exempel hello [Åtkomstkontrollflödet] [ Control flow] och [ange variabel] [ Set variable] principer är baserade på principuttrycken. Mer information finns i [avancerade principer] [ Advanced policies] och [principuttrycken][Policy expressions].

## <a name="scopes"></a>Hur tooconfigure principer
Principer kan konfigureras globalt eller hello omfånget för en [produkten][Product], [API] [ API] eller [åtgärden] [Operation]. tooconfigure en princip, navigera toohello principer redigeraren i hello publisher-portalen.

![Principer-menyn][policies-menu]

Redigeraren för hello-principer består av tre huvudavsnitt: hello princip omfång (överst) hello principdefinitionen hello instruktioner lista över (höger) och där principer redigeras (vänster):

![Redigeraren för principer][policies-editor]

toobegin konfigurerar en princip måste du först välja hello scope på vilka hello principen ska gälla. I hello skärmbilden nedan hello **Starter** produkt har valts. Observera att hello kvadratisk symbolen nästa toohello principnamn anger princip används redan på den här nivån.

![Omfång][policies-scope]

Eftersom en princip har redan tillämpats, visas hello-konfiguration i hello definition vyn.

![Konfigurera][policies-configure]

hello principen visas skrivskyddad först. I ordning tooedit hello definition klickar du på hello **konfigurera principen för** åtgärd.

![Redigera][policies-edit]

hello principdefinitionen är ett enkelt XML-dokument som beskriver en sekvens av inkommande och utgående rapporter. hello XML kan redigeras direkt i hello definition fönster. En lista över rapporter som är direkt toohello och instruktioner gäller toohello aktuella omfattningen är aktiverat markerat; som framgår av hello **gränsen anropa hastighet** instruktionen i hello skärmbilden ovan.

Klicka på en aktiverad instruktion läggs hello lämplig XML som hello platsen för hello markören i hello definition vy. 

> [!NOTE]
> Om hello som du vill tooadd inte är aktiverad, kan du kontrollera att du är i hello rätt omfattning för grupprincipobjekt. Varje Principframställning är avsedd för användning i vissa scope och principen avsnitt. tooreview hello princip avsnitt och scope för en princip, kontrollera hello **användning** avsnitt av principen i hello [principreferens][Policy Reference].
> 
> 

En fullständig lista över principrapporter och deras inställningar är tillgängliga i hello [principreferens][Policy Reference].

Till exempel tooadd en ny instruktionen toorestrict inkommande begäranden toospecified IP-adresser, markören hello innanför hello innehållet i hello `inbound` XML-elementet och klicka på hello **begränsa anroparen IP-adresser** instruktionen.

![Principer för begränsning av][policies-restrict]

Detta lägger till en XML-fragment toohello `inbound` element som innehåller information om hur tooconfigure hello instruktionen.

```xml
<ip-filter action="allow | forbid">
    <address>address</address>
    <address-range from="address" to="address"/>
</ip-filter>
```

toolimit inkommande begäranden och accepterar endast de från IP-adressen 1.2.3.4 ändra hello XML på följande sätt:

```xml
<ip-filter action="allow">
    <address>1.2.3.4</address>
</ip-filter>
```

![Spara][policies-save]

När du är färdig konfigurera hello instruktioner för hello principen klickar du på **spara** och hello ändringar kommer att spridda toohello API Management gateway omedelbart.

## <a name="sections"></a>Förstå principkonfiguration
En princip är en serie som utförs för en begäran och ett svar. hello-konfigurationen är korrekt indelat i `inbound`, `backend`, `outbound`, och `on-error` avsnitten enligt hello efter konfiguration.

```xml
<policies>
  <inbound>
    <!-- statements toobe applied toohello request go here -->
  </inbound>
  <backend>
    <!-- statements toobe applied before hello request is forwarded too
         hello backend service go here -->
  </backend>
  <outbound>
    <!-- statements toobe applied toohello response go here -->
  </outbound>
  <on-error>
    <!-- statements toobe applied if there is an error condition go here -->
  </on-error>
</policies> 
```

Om det finns ett fel under hello bearbetningen av en begäran, alla eventuella resterande steg i hello `inbound`, `backend`, eller `outbound` avsnitt hoppas över och körningen hoppar toohello instruktioner i hello `on-error` avsnitt. Placera principrapporter i hello `on-error` avsnitt som du kan granska hello fel med hjälp av hello `context.LastError` egenskapen inspektera och anpassa hello felsvar med hello `set-body` princip, och konfigurera vad händer om ett fel inträffar. Det finns felkoder för inbyggda steg och fel som kan uppstå under hello behandling av princip för instruktioner. Mer information finns i [felhantering i API Management-principer](https://msdn.microsoft.com/library/azure/mt629506.aspx).

Eftersom principer kan anges på olika nivåer (global, produkt, api och åtgärden) kan hello konfiguration du toospecify hello ordning där uttryck hello principdefinition köra med avseende toohello överordnade principen. 

Princip för scope utvärderas i ordning hello.

1. Globalt scope
2. Produkten omfång
3. API-scope
4. Åtgärden omfång

hello instruktioner i dem utvärderas enligt toohello placering av hello `base` element, om den finns. Globala principen har ingen överordnad och använder hello `<base>` element i den har ingen effekt.

Till exempel om du har en princip på global nivå i hello och en princip som konfigurerats för ett API som tillämpas sedan varje gång det specifika API används båda principerna. API Management gör deterministiska sorteringen av kombinerade hanteringsprinciper via hello baselementet. 

```xml
<policies>
    <inbound>
        <cross-domain />
        <base />
        <find-and-replace from="xyz" to="abc" />
    </inbound>
</policies>
```

I hello exempel principdefinitionen ovan, hello `cross-domain` instruktionen skulle köras innan de högre principer som i sin tur följas av hello `find-and-replace` princip. 

toosee hello principer i hello aktuella omfattning i hello Redigeraren för grupprincipobjekt, klickar du på **beräkna om princip för valda omfång**.

## <a name="next-steps"></a>Nästa steg
Gå igenom följande video på principuttrycken.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

[Policy Reference]: api-management-policy-reference.md
[Product]: api-management-howto-add-products.md
[API]: api-management-howto-add-products.md#add-apis 
[Operation]: api-management-howto-add-operations.md

[Advanced policies]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Policy expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-menu]: ./media/api-management-howto-policies/api-management-policies-menu.png
[policies-editor]: ./media/api-management-howto-policies/api-management-policies-editor.png
[policies-scope]: ./media/api-management-howto-policies/api-management-policies-scope.png
[policies-configure]: ./media/api-management-howto-policies/api-management-policies-configure.png
[policies-edit]: ./media/api-management-howto-policies/api-management-policies-edit.png
[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
[policies-save]: ./media/api-management-howto-policies/api-management-policies-save.png
