---
title: "aaaConfigure din API Management-tjänsten med Git - Azure | Microsoft Docs"
description: "Lär dig hur toosave och konfigurera din API Management service configuration med Git."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: mattfarm
ms.assetid: 364cd53e-88fb-4301-a093-f132fa1f88f5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: ef7d4c18f2ea3f5c9b86403349a83aef240f979b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosave-and-configure-your-api-management-service-configuration-using-git"></a>Hur toosave och konfigurera din API Management service configuration med Git
> 
> 

Varje instans för API Management-tjänsten har en databas som innehåller information om hello konfiguration och metadata för hello tjänstinstansen. Ändringar kan göras toohello service-instans genom att ändra en inställning i hello publisher portal, med hjälp av en PowerShell-cmdlet eller göra ett REST API-anrop. Dessutom toothese metoder du kan också hantera konfigurationen för service-instans med Git, aktivera tjänsten hanteringsscenarier som:

* Konfigurationen versionshantering - ladda ned och lagra olika versioner av tjänstkonfigurationen av
* Massredigera konfigurationsändringar – ändrar toomultiple delar av din tjänstkonfiguration i din lokala databas och integrera hello ändringar tillbaka toohello server med en enda åtgärd
* Välkända Git toolchain och arbetsflöde – använda hello Git verktygsuppsättning och arbetsflöden som du redan är bekant med

hello följande diagram visar en översikt över hello olika sätt tooconfigure din API Management service-instans.

![Konfigurera Git][api-management-git-configure]

När du gör ändringar tooyour tjänst med hello publisher portal, PowerShell-cmdlets eller hello REST API du hanterar din konfigurationsdatabas med hello `https://{name}.management.azure-api.net` slutpunkt, enligt hello höger på hello diagram. hello vänster sida av hello diagram illustrerar hur du kan hantera tjänstkonfigurationen av med Git och Git-lagringsplats för tjänsten finns på `https://{name}.scm.azure-api.net`.

hello följande ger en översikt över hantering av din API Management service-instans med Git.

1. Konfiguration av Git i din tjänst
2. Spara service configuration database tooyour Git-lagringsplatsen
3. Klona hello Git repo tooyour lokal dator
4. Hämta hello senaste lagringsplatsen ned tooyour lokala datorn och genomförande och push ändringar tillbaka tooyour lagringsplatsen
5. Distribuera hello ändringar från din lagringsplats i din konfigurationsdatabas

Den här artikeln beskriver hur tooenable använder Git toomanage tjänstkonfigurationen av och innehåller en referens för hello filer och mappar i hello Git-lagringsplats.

## <a name="access-git-configuration-in-your-service"></a>Konfiguration av Git i din tjänst
Du kan snabbt visa hello status för Git-konfiguration genom att visa hello Git ikon i hello övre högra hörnet av hello publisher portal. I det här exemplet hello statusmeddelande som anger att det finns ändringar som inte sparats toohello databasen. Det beror på att konfigurationsdatabasen för hello API Management-tjänsten inte har ännu har sparats toohello databasen.

![Git-status][api-management-git-icon-enable]

tooview och konfigurera inställningarna i Git, kan du antingen klicka hello Git ikonen eller klicka på hello **säkerhet** menyn och navigera toohello **Configuration databasen** fliken.

![Aktivera GIT][api-management-enable-git]

> [!IMPORTANT]
> Alla hemligheter som inte har definierats som egenskaper kommer att lagras i databasen hello och kommer att finnas kvar i historiken tills du inaktivera och aktivera Git-åtkomst. Egenskaper som innehåller en säker plats toomanage konstanta strängvärden, inklusive hemligheter, för alla API-konfiguration och principer, så du inte behöver toostore dem direkt i din principrapporter. Mer information finns i [hur toouse egenskaper i Azure API Management-principer](api-management-howto-properties.md).
> 
> 

Mer information om att aktivera eller inaktivera Git-åtkomst med hjälp av hello REST-API finns [aktivera eller inaktivera Git-åtkomst med hjälp av hello REST API](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).

## <a name="toosave-hello-service-configuration-toohello-git-repository"></a>toosave hello service configuration toohello Git-lagringsplats
hello första steget innan kloning hello databasen är toosave hello aktuell status för hello service configuration toohello databasen. Klicka på **spara konfigurationen toorepository**.

![Spara konfigurationen][api-management-save-configuration]

Gör eventuella ändringar på hello bekräftelseskärm och klicka på **Ok** toosave.

![Spara konfigurationen][api-management-save-configuration-confirm]

Efter en liten stund hello konfiguration sparas och hello Konfigurationsstatus för hello lagringsplats visas, inklusive hello datum och tidpunkt för senaste ändring av hello och hello senaste synkronisering mellan hello tjänstkonfiguration och hello databasen.

![Konfigurationsstatus][api-management-configuration-status]

När hello konfiguration sparas toohello databasen kan klonas den.

Information om hur du utför den här åtgärden med hjälp av hello REST-API finns [Commit-konfiguration med verktyget hello REST API](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).

## <a name="tooclone-hello-repository-tooyour-local-machine"></a>tooclone hello databasen tooyour lokal dator
tooclone en databas måste hello URL tooyour databasen, ett användarnamn och ett lösenord. hello användarnamn och URL visas hello övre delen av hello **Configuration databasen** fliken.

![Git-klonen][api-management-configuration-git-clone]

hello lösenord genereras längst hello hello **Configuration databasen** fliken.

![Generera lösenord][api-management-generate-password]

toogenerate ett lösenord, kontrollera först att hello **upphör att gälla** är korrekt toohello önskad utgångsdatum och utgångstid och klicka sedan på **generera Token**.

![Lösenord][api-management-password]

> [!IMPORTANT]
> Anteckna det här lösenordet. När du lämnar kommer den här sidan hello lösenord inte att visas igen.
> 
> 

följande exempel används hello Git Bash hello verktyget från [Git för Windows](http://www.git-scm.com/downloads) men du kan använda alla Git-verktyg som du är bekant med.

Öppna Git-verktyget i hello önskade mappen och kör hello efter kommandot tooclone hello git-lagringsplatsen tooyour lokala datorn, med hello-kommando som tillhandahålls av hello publisher-portalen.

```
git clone https://bugbashdev4.scm.azure-api.net/
```

Ange hello användarnamn och lösenord när du uppmanas.

Om du får inga fel, försök att ändra din `git clone` kommandot tooinclude hello användarnamn och lösenord, som visas i följande exempel hello.

```
git clone https://username:password@bugbashdev4.scm.azure-api.net/
```

Försök URL kodning hello delar av hello kommandot lösenord om det innehåller ett fel. Ett snabbt sätt toodo är tooopen Visual Studio och problemet hello följande kommando i hello **kommandofönstret**. tooopen hello **kommandofönstret**, öppna någon lösning eller ett projekt i Visual Studio (eller skapa en ny tom konsolapp), och välj **Windows**, **Immediate** från Hej **felsöka** menyn.

```
?System.NetWebUtility.UrlEncode("password from publisher portal")
```

Använda hello kodade lösenordet tillsammans med ditt namn och databasen plats tooconstruct hello git användarkommando.

```
git clone https://username:url encoded password@bugbashdev4.scm.azure-api.net/
```

Du kan visa och arbeta med det i det lokala filsystemet när hello databasen klonas. Mer information finns i [fil- och struktur referens för lokal Git-lagringsplats](#file-and-folder-structure-reference-of-local-git-repository).

## <a name="tooupdate-your-local-repository-with-hello-most-current-service-instance-configuration"></a>tooupdate lokala databasen med hello aktuell instans tjänstkonfiguration
Om du gör ändringar tooyour API Management service-instans i hello publisher portalen eller med hjälp av hello REST-API, måste du spara dessa ändringar toohello databasen innan du kan uppdatera din lokal databas med hello senaste ändringarna. toodo, genom att välja **spara konfigurationen toorepository** på hello **Configuration databasen** i hello publisher portal och utfärda hello följande kommando i en lokal databas.

```
git pull
```

Innan du kör `git pull` så att du är i hello-mappen för din lokala databas. Om du har precis slutfört hello `git clone` kommandot, du måste ändra lagringsplatsen för hello directory tooyour genom att köra ett kommando som hello nedan.

```
cd bugbashdev4.scm.azure-api.net/
```

## <a name="toopush-changes-from-your-local-repo-toohello-server-repo"></a>toopush ändras från din lokala lagringsplatsen toohello server lagringsplatsen
toopush ändras från din lokala toohello server databasen måste du spara ändringarna och sedan push-installera dem toohello server-databasen. toocommit ändringarna, öppna ditt Git-kommandoradsverktyg, växel toohello katalog med din lokala databasen och problemet hello följande kommandon.

```
git add --all
git commit -m "Description of your changes"
```

toopush alla hello genomför toohello servern, kör hello följande kommando.

```
git push
```

## <a name="toodeploy-any-service-configuration-changes-toohello-api-management-service-instance"></a>toodeploy alla tjänstens konfiguration ändringar toohello API Management tjänstinstans
När din lokala ändringar är bekräftade och intryckt toohello server-databasen kan distribuera du dem tooyour API Management service-instans.

![Distribuera][api-management-configuration-deploy]

Information om hur du utför den här åtgärden med hjälp av hello REST-API finns [distribuera Git ändras tooconfiguration databasen med hjälp av hello REST API](https://docs.microsoft.com/en-us/rest/api/apimanagement/tenantconfiguration).

## <a name="file-and-folder-structure-reference-of-local-git-repository"></a>Fil- och structure-referens för lokal Git-lagringsplats
hello innehåller filer och mappar i hello lokal git-lagringsplatsen hello konfigurationsinformation om hello tjänstinstansen.

| Objekt | Beskrivning |
| --- | --- |
| Rotmapp för api management |Innehåller översta konfiguration för hello tjänstinstans |
| API: er för mappen |Innehåller hello konfiguration för hello API: er i hello tjänstinstans |
| mappen grupper |Innehåller hello konfiguration för hello grupper i hello tjänstinstans |
| för principmappen |Innehåller hello principer i hello tjänstinstans |
| portalStyles mapp |Innehåller hello konfiguration för hello developer portal anpassningar i hello tjänstinstans |
| produkter mapp |Innehåller hello konfiguration för hello produkter i hello tjänstinstans |
| mallmappen |Innehåller hello konfiguration för hello e-mallar i hello tjänstinstans |

Varje mapp kan innehålla en eller flera filer och i vissa fall en eller flera mappar, till exempel en mapp för varje API, produkt eller grupp. hello-filer i mappen är specifika för hello entitetstypen beskrivs av hello mappnamn.

| Filtyp | Syfte |
| --- | --- |
| JSON |Konfigurationsinformation om hello respektive enhet |
| HTML |Beskrivningar om hello-enhet, ofta visas i hello developer-portalen |
| xml |Principrapporter |
| CSS |Formatmallar för developer portal anpassning |

Dessa filer kan skapas, tas bort, redigera och hanteras inte i det lokala filsystemet och hello ändringarna distribueras tillbaka toohello din API Management service-instans.

> [!NOTE]
> hello följande enheter finns inte i hello Git-lagringsplats och kan inte konfigureras med Git.
> 
> * Användare
> * Prenumerationer
> * Egenskaper
> * Developer portal enheter än format
> 
> 

### <a name="root-api-management-folder"></a>Rotmapp för api management
hello rot `api-management` mappen innehåller en `configuration.json` -fil som innehåller översta information om hello tjänstinstansen i hello följande format.

```json
{
  "settings": {
    "RegistrationEnabled": "True",
    "UserRegistrationTerms": null,
    "UserRegistrationTermsEnabled": "False",
    "UserRegistrationTermsConsentRequired": "False",
    "DelegationEnabled": "False",
    "DelegationUrl": "",
    "DelegatedSubscriptionEnabled": "False",
    "DelegationValidationKey": ""
  },
  "$ref-policy": "api-management/policies/global.xml"
}
```

Hej första fyra inställningar (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, och `UserRegistrationTermsConsentRequired`) mappa toohello följande inställningar på hello **identiteter** fliken i hello **säkerhet** avsnitt.

| Identity-inställning | Mappar för|
| --- | --- |
| RegistrationEnabled |**Omdirigeringssida anonyma användare toosign i** kryssruta |
| UserRegistrationTerms |**Villkor för användning på användaren signup** textruta |
| UserRegistrationTermsEnabled |**Visa användningsvillkoren på inloggningssidan** kryssruta |
| UserRegistrationTermsConsentRequired |**Kräv godkännande** kryssruta |

![Identitetsinställningar][api-management-identity-settings]

Hej följande fyra inställningar (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, och `DelegationValidationKey`) mappa toohello följande inställningar på hello **delegering** fliken i hello **säkerhet** avsnitt.

| Delegeringsinställningen | Mappar för|
| --- | --- |
| DelegationEnabled |**Delegera inloggning & anmälan** kryssruta |
| DelegationUrl |**Delegering slutpunkts-URL** textruta |
| DelegatedSubscriptionEnabled |**Delegera produkten prenumeration** kryssruta |
| DelegationValidationKey |**Delegera valideringsnyckel** textruta |

![Inställningarna för standarddelegering][api-management-delegation-settings]

Hej sista inställningen `$ref-policy`, mappar toohello globala instruktioner principfil för hello tjänstinstansen.

### <a name="apis-folder"></a>API: er för mappen
Hej `apis` mappen innehåller en mapp för varje API i hello service-instans som innehåller hello följande objekt.

* `apis\<api name>\configuration.json`-Detta är hello konfigurationen för hello API och innehåller information om hello backend-tjänstens URL och hello åtgärder. Detta är hello samma information som returneras om du toocall [få en specifika API: et](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) med `export=true` i `application/json` format.
* `apis\<api name>\api.description.html`-Detta är hello beskrivning av hello API som motsvarar toohello `description` -egenskapen för hello [API entiteten](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties).
* `apis\<api name>\operations\`-den här mappen innehåller `<operation name>.description.html` filer som mappas toohello åtgärder i hello API. Varje fil innehåller hello beskrivning av en enda åtgärd i hello API som mappar toohello `description` -egenskapen för hello [åtgärden entiteten](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) i hello REST API.

### <a name="groups-folder"></a>mappen grupper
Hej `groups` mappen innehåller en mapp för varje grupp som definierats i hello tjänstinstansen.

* `groups\<group name>\configuration.json`-Detta är hello konfiguration för hello grupp. Detta är hello samma information som returneras om du toocall hello [få en specifik grupp](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) igen.
* `groups\<group name>\description.html`-Detta är hello beskrivning av hello grupp som motsvarar toohello `description` -egenskapen för hello [gruppen entiteten](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties).

### <a name="policies-folder"></a>för principmappen
Hej `policies` mappen innehåller hello hanteringsprinciper för service-instans.

* `policies\global.xml`-innehåller principer som definierats vid globalt scope för service-instans.
* `policies\apis\<api name>\`-Om du har några definierade principer på API-scope, de ingår i den här mappen.
* `policies\apis\<api name>\<operation name>\`mapp - om du har några definierade principer på åtgärdsomfånget de ingår i den här mappen i `<operation name>.xml` filer som mappas toohello principrapporter för varje åtgärd.
* `policies\products\`-Om du har några principer som definierats i produktomfattningen de ingår i den här mappen som innehåller `<product name>.xml` filer som mappas toohello hanteringsprinciper för varje produkt.

### <a name="portalstyles-folder"></a>portalStyles mapp
Hej `portalStyles` mappen innehåller konfigurations- och blad för developer portal anpassningar för hello tjänstinstansen.

* `portalStyles\configuration.json`-innehåller hello namnen på hello formatmallar som används av hello developer-portalen
* `portalStyles\<style name>.css`-varje `<style name>.css` filen innehåller format för hello developer-portalen (`Preview.css` och `Production.css` som standard).

### <a name="products-folder"></a>produkter mapp
Hej `products` mappen innehåller en mapp för varje produkt som definierats i hello tjänstinstansen.

* `products\<product name>\configuration.json`-Detta är hello konfiguration för hello produkten. Detta är hello samma information som returneras om du toocall hello [få en specifik produkt](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) igen.
* `products\<product name>\product.description.html`-Detta är hello beskrivning av hello produkt som motsvarar toohello `description` -egenskapen för hello [entiteten för produkt](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) i hello REST API.

### <a name="templates"></a>mallar
Hej `templates` mappen innehåller konfiguration för hello [e-mallar](api-management-howto-configure-notifications.md) av hello tjänstinstansen.

* `<template name>\configuration.json`-Detta är hello konfiguration för hello e-postmall.
* `<template name>\body.html`-Detta är hello texten för hello e-postmallen.

## <a name="next-steps"></a>Nästa steg
Mer information om andra sätt toomanage service-instans finns:

* Hantera din service-instans med hello följande PowerShell-cmdlets
  * [Tjänstdistributionen PowerShell cmdlet-referens](https://msdn.microsoft.com/library/azure/mt619282.aspx)
  * [Service management PowerShell-cmdlet-referens](https://msdn.microsoft.com/library/azure/mt613507.aspx)
* Hantera service-instans i hello publisher portal
  * [Hantera ditt första API](api-management-get-started.md)
* Hantera din service-instans med hello REST API
  * [API Management REST API-referens](https://msdn.microsoft.com/library/azure/dn776326.aspx)

## <a name="watch-a-video-overview"></a>Titta på en videoöversikt
> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Configuration-over-Git/player]
> 
> 

[api-management-enable-git]: ./media/api-management-configuration-repository-git/api-management-enable-git.png
[api-management-git-enabled]: ./media/api-management-configuration-repository-git/api-management-git-enabled.png
[api-management-save-configuration]: ./media/api-management-configuration-repository-git/api-management-save-configuration.png
[api-management-save-configuration-confirm]: ./media/api-management-configuration-repository-git/api-management-save-configuration-confirm.png
[api-management-configuration-status]: ./media/api-management-configuration-repository-git/api-management-configuration-status.png
[api-management-configuration-git-clone]: ./media/api-management-configuration-repository-git/api-management-configuration-git-clone.png
[api-management-generate-password]: ./media/api-management-configuration-repository-git/api-management-generate-password.png
[api-management-password]: ./media/api-management-configuration-repository-git/api-management-password.png
[api-management-git-configure]: ./media/api-management-configuration-repository-git/api-management-git-configure.png
[api-management-configuration-deploy]: ./media/api-management-configuration-repository-git/api-management-configuration-deploy.png
[api-management-identity-settings]: ./media/api-management-configuration-repository-git/api-management-identity-settings.png
[api-management-delegation-settings]: ./media/api-management-configuration-repository-git/api-management-delegation-settings.png
[api-management-git-icon-enable]: ./media/api-management-configuration-repository-git/api-management-git-icon-enable.png




