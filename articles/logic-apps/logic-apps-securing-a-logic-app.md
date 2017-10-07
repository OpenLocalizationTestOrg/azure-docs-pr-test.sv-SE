---
title: "aaaSecure komma åt tooAzure Logic Apps | Microsoft Docs"
description: "Lägger till säkerhet för att skydda åtkomst tootriggers indata och utdata, åtgärdsparametrar och tjänster som används med arbetsflöden i Azure Logic Apps."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 9fab1050-cfbc-4a8b-b1b3-5531bee92856
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 11/22/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: abda2179e4cc2d2295cd8332ec017c848a456264
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="secure-access-tooyour-logic-apps"></a>Säker åtkomst tooyour logikappar

Det finns många verktyg-tillgängliga toohelp du skydda din logikapp.

* Skydda åtkomst tootrigger en logikapp (http-begäran utlösare)
* Skydda åtkomst toomanage, redigera eller läsa en logikapp
* Skydda åtkomst toocontents indata och utdata för en körning
* Att säkra parametrar eller indata i åtgärder i ett arbetsflöde
* Skydda åtkomst tooservices som tar emot förfrågningar från ett arbetsflöde

## <a name="secure-access-tootrigger"></a>Säker åtkomst tootrigger

När du arbetar med en logikapp som utlöses på en HTTP-begäran ([begära](../connectors/connectors-native-reqres.md) eller [Webhook](../connectors/connectors-native-webhook.md)), kan du begränsa åtkomsten så att endast auktoriserade klienter kan utlösas hello logikapp. Alla förfrågningar till en logikapp krypteras och skyddas via SSL.

### <a name="shared-access-signature"></a>Signatur för delad åtkomst

Varje begäran slutpunkt för en logikapp innehåller en [delade signatur åtkomst (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) som en del av hello-URL. Varje URL innehåller en `sp`, `sv`, och `sig` Frågeparametern. Behörigheter som anges av `sp`, och motsvarar tooHTTP metoder tillåts, `sv` är hello version som används för toogenerate och `sig` är används tooauthenticate åtkomst tootrigger. hello signatur skapas med en hemlig nyckel på alla hello URL-sökvägar och egenskaper hello SHA256-algoritmen. hello hemlig nyckel aldrig exponeras och publiceras, och hålls krypterade och lagras som en del av hello logikapp. Din logikapp tillåter endast utlösare som innehåller en giltig signatur som skapats med hello hemlig nyckel.

#### <a name="regenerate-access-keys"></a>Återskapa åtkomstnycklar

Du kan återskapa en ny säker nyckel vid när som helst via hello REST API- eller Azure-portalen. Alla aktuella URL: er som har skapats tidigare med hello gamla nyckeln är inaktuella och inte längre auktoriserade toofire hello logikapp.

1. Öppna hello logikappen som du vill tooregenerate en nyckel i hello Azure-portalen
1. Klicka på hello **åtkomstnycklar** menyalternativet **inställningar**
1. Välj hello viktiga tooregenerate och fullständig hello-processen

URL: er som du hämtar efter återskapandet är signerade med hello nya snabbtangent.

#### <a name="creating-callback-urls-with-an-expiration-date"></a>Skapa återanrop URL: er med ett förfallodatum

Om du delar hello URL med andra parter kan generera du URL: er med specifika nycklar och förfallodatum efter behov. Du kan sedan sömlöst Återställ nycklar eller kontrollera åtkomst toofire en app är begränsad tooa vissa timespan. Du kan ange ett sista giltighetsdatum för en URL via hello [logikappar REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers):

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

Ta hello-egenskapen i hello brödtext `NotAfter` som en JSON-datumsträng som returnerar en motringning-URL som är endast giltig förrän hello `NotAfter` datum och tid.

#### <a name="creating-urls-with-primary-or-secondary-secret-key"></a>Skapa URL: er med primär eller sekundär hemlig nyckel

När du generera eller listan återanrop URL: er för frågebaserad utlösare kan ange du också vilka viktiga toouse toosign hello-URL.  Du kan generera en URL som signerats av en viss nyckel via hello [logikappar REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers) på följande sätt:

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

Ta hello-egenskapen i hello brödtext `KeyType` som antingen `Primary` eller `Secondary`.  Detta returnerar en URL som signerats av hello säker nyckel har angetts.

### <a name="restrict-incoming-ip-addresses"></a>Begränsa inkommande IP-adresser

Dessutom toohello signatur för delad åtkomst gärna toorestrict som anropar en logikapp endast från specifika klienter.  Om du hanterar din slutpunkt via Azure API Management kan du till exempel begränsa hello logik app tooonly Godkänn begäran om hello när hello begäran kommer från hello API Management instans-IP-adress.

Den här inställningen kan konfigureras i hello logik app-inställningar:

1. Öppna i hello Azure-portalen, hello logikappen som du vill tooadd IP-adressbegränsningar
1. Klicka på hello **konfiguration för behörighetskontroll** menyalternativet **inställningar**
1. Ange hello lista över IP-adress adressintervall toobe accepteras av hello utlösare

En giltig IP-adressintervall tar hello format `192.168.1.1/255`. Om du vill hello logik app tooonly brand som en kapslad logikapp, väljer hello **andra logikappar** alternativet. Det här alternativet skriver en tom matris toohello resurs, vilket innebär att endast anrop från hello-tjänsten (överordnade logikappar) brand har.

> [!NOTE]
> Du kan fortfarande köra en logikapp med en utlösare för begäran via hello REST API / Management `/triggers/{triggerName}/run` oavsett IP. Det här scenariot kräver autentisering mot hello Azure REST-API och alla händelser som visas i hello Azure granskningslogg. Ange åtkomstkontroll principer därefter.

#### <a name="setting-ip-ranges-on-hello-resource-definition"></a>Ange IP-intervall på hello resursdefinitionen

Om du använder en [Distributionsmall](logic-apps-create-deploy-template.md) tooautomate dina distributioner hello IP-adressintervall inställningar kan konfigureras på hello resource-mall.  

``` json
{
    "properties": {
        "definition": {
        },
        "parameters": {},
        "accessControl": {
            "triggers": {
                "allowedCallerIpAddresses": [
                    {
                        "addressRange": "192.168.12.0/23"
                    },
                    {
                        "addressRange": "2001:0db8::/64"
                    }
                ]
            }
        }
    },
    "type": "Microsoft.Logic/workflows"
}

```

### <a name="adding-azure-active-directory-oauth-or-other-security"></a>Lägga till Azure Active Directory, OAuth eller annan säkerhet

tooadd mer auktorisering protokoll ovanpå en logikapp [Azure API Management](https://azure.microsoft.com/services/api-management/) erbjuder omfattande övervakning, säkerhet, principer och dokumentation för valfri slutpunkt med hello kapaciteten tooexpose en logikapp som en API. Azure API Management kan exponera en offentlig eller privat slutpunkt för hello logikapp, som kan använda Azure Active Directory, certifikat, OAuth eller andra säkerhetskrav. När en begäran tas emot, vidarebefordrar Azure API Management hello begäran toohello logikapp (utför alla nödvändiga transformationer eller begränsningar relä). Du kan använda hello inkommande IP-adressintervall med inställningarna på hello logik app tooonly kan hello logik app toobe utlöses från API-hantering.

## <a name="secure-access-toomanage-or-edit-logic-apps"></a>Säker åtkomst toomanage eller redigera logikappar

Du kan begränsa åtkomst toomanagement åtgärder på en logikapp så att bara vissa användare eller grupper kan tooperform åtgärder på hello resurs. Logikappar använda hello Azure [rollbaserad åtkomstkontroll (RBAC)](../active-directory/role-based-access-control-configure.md) funktion och kan anpassas med hello samma verktyg.  Det finns några inbyggda roller som du kan tilldela medlemmar i din prenumeration tooas också:

* **Logik App deltagare** – ger åtkomst tooview, redigera och uppdatera en logikapp.  Det går inte att ta bort hello resurs eller utföra administrativa åtgärder.
* **Logik App operatorn** – kan visa hello logikapp och kör historik och aktivera/inaktivera.  Det går inte att redigera eller uppdatera hello definition.

Du kan också använda [Azure Resource Lås](../azure-resource-manager/resource-group-lock-resources.md) tooprevent ändrar eller tar bort logikappar. Den här funktionen är värdefullt tooprevent produktionsresurser från ändringar eller borttagningar.

## <a name="secure-access-toocontents-of-hello-run-history"></a>Säker åtkomst toocontents av hello kör historik

Du kan begränsa åtkomst toocontents av in- eller utdata från tidigare körs toospecific IP-adressintervall.  

Alla data i ett arbetsflöde kör krypteras under överföring och i vila. När en toorun samtalshistorik görs hello tjänsten autentiserar hello begäran och ger länkar toohello begäran och svar indata och utdata. Den här länken kan vara skyddad så att endast begäranden tooview innehåll från ett IP-adressintervall returnera hello innehållet. Du kan använda den här funktionen för ytterligare behörighet. Du kan även ange en IP-adress som `0.0.0.0` så ingen kan komma åt indata/utdata. Endast användare med administratörsbehörigheter kan ta bort den här begränsningen tillhandahåller hello möjligheten för 'just-in-time-åtkomst tooworkflow innehållet.

Den här inställningen kan konfigureras i hello resursinställningar för hello Azure-portalen:

1. Öppna i hello Azure-portalen, hello logikappen som du vill tooadd IP-adressbegränsningar
1. Klicka på hello **konfiguration för behörighetskontroll** menyalternativet **inställningar**
1. Ange hello lista över IP-adressintervall för åtkomst toocontent

#### <a name="setting-ip-ranges-on-hello-resource-definition"></a>Ange IP-intervall på hello resursdefinitionen

Om du använder en [Distributionsmall](logic-apps-create-deploy-template.md) tooautomate dina distributioner hello IP-adressintervall inställningar kan konfigureras på hello resource-mall.  

``` json
{
    "properties": {
        "definition": {
        },
        "parameters": {},
        "accessControl": {
            "contents": {
                "allowedCallerIpAddresses": [
                    {
                        "addressRange": "192.168.12.0/23"
                    },
                    {
                        "addressRange": "2001:0db8::/64"
                    }
                ]
            }
        }
    },
    "type": "Microsoft.Logic/workflows"
}
```

## <a name="secure-parameters-and-inputs-within-a-workflow"></a>Säker parametrar och indata i ett arbetsflöde

Du kanske vill tooparameterize vissa aspekter av en arbetsflödesdefinition för distribution i miljöer. Vissa parametrar kan också vara säkra parametrar som du inte vill tooappear när du redigerar ett arbetsflöde, till exempel en klient-ID och klienthemlighet för [Azure Active Directory-autentisering](../connectors/connectors-native-http.md#authentication) av en HTTP-åtgärd.

### <a name="using-parameters-and-secure-parameters"></a>Med hjälp av parametrarna och säker

tooaccess hello värdet för en resurs parameter vid körning, hello [språk i arbetsflödesdefinitionen](http://aka.ms/logicappsdocs) ger en `@parameters()` igen. Du kan också [ange parametrar i hello Distributionsmall för resource](../azure-resource-manager/resource-group-authoring-templates.md#parameters). Men om du anger hello parametertypen som `securestring`, hello parametern inte returneras med hello resten av hello resursdefinitionen och inte är tillgängliga genom att visa hello resurs efter distributionen.

> [!NOTE]
> Om parametern används i hello sidhuvud eller en begäran om kanske hello parametern visas genom att öppna hello kör historik och utgående HTTP-begäran. Se till att tooset principer för åtkomst till innehåll i enlighet med detta.
> Auktorisering huvuden visas aldrig via in- eller utdata. Så om hello hemlighet används det är hello hemlighet inte strängfält.

#### <a name="resource-deployment-template-with-secrets"></a>Resurs-Distributionsmall med hemligheter

hello följande exempel visas en distribution som refererar till en säker parameter av `secret` vid körning. Du kan ange hello värdet för hello i en separat parameterfilen `secret`, eller Använd [Azure Resource Manager KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) tooretrieve hemligheter på distribuera tid.

``` json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "secretDeploymentParam": {
      "type": "securestring"
    }
  },
  "variables": {},
  "resources": [
    {
      "name": "secret-deploy",
      "type": "Microsoft.Logic/workflows",
      "location": "westus",
      "tags": {
        "displayName": "LogicApp"
      },
      "apiVersion": "2016-06-01",
      "properties": {
        "definition": {
          "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "actions": {
            "Call_External_API": {
              "type": "http",
              "inputs": {
                "headers": {
                  "Authorization": "@parameters('secret')"
                },
                "body": "This is hello request"
              },
              "runAfter": {}
            }
          },
          "parameters": {
            "secret": {
              "type": "SecureString"
            }
          },
          "triggers": {
            "manual": {
              "type": "Request",
              "kind": "Http",
              "inputs": {
                "schema": {}
              }
            }
          },
          "contentVersion": "1.0.0.0",
          "outputs": {}
        },
        "parameters": {
          "secret": {
            "value": "[parameters('secretDeploymentParam')]"
          }
        }
      }
    }
  ],
  "outputs": {}
}
```

## <a name="secure-access-tooservices-receiving-requests-from-a-workflow"></a>Säker åtkomst tooservices tar emot förfrågningar från ett arbetsflöde

Det finns många sätt toohelp säker logikapp någon slutpunkt hello måste tooaccess.

### <a name="using-authentication-on-outbound-requests"></a>Med autentisering för utgående förfrågningar

När du arbetar med en HTTP-, http- + Swagger (Öppna API) eller Webhook åtgärd kan du lägga till toohello autentiseringsbegäran skickas. Du kan inkludera grundläggande autentisering eller autentisering med datorcertifikat Azure Active Directory-autentisering. Information om hur tooconfigure den här autentiseringen kan hittas [i den här artikeln](../connectors/connectors-native-http.md#authentication).

### <a name="restricting-access-toologic-app-ip-addresses"></a>Begränsa åtkomst toologic app IP-adresser

Alla anrop från logikappar komma från en specifik uppsättning IP-adresser per region. Du kan lägga till ytterligare filtrering tooonly godkänna begäranden från de som anges av IP-adresser. En lista över de IP-adresserna, se [logik app gränser och konfiguration](logic-apps-limits-and-config.md#configuration).

### <a name="on-premises-connectivity"></a>Lokala anslutningsmöjligheter

Logikappar ange integrering med flera tjänster tooprovide säkert och tillförlitligt lokalt kommunikation.

#### <a name="on-premises-data-gateway"></a>Lokal datagateway

Många hanterade anslutningar för logic apps ger säker anslutning tooon lokalt system, inklusive filsystem, SQL, SharePoint, DB2 och mycket mer. hello gateway vidarebefordrar data från lokala datakällor på krypterade kanaler via hello Azure Service Bus. All trafik som kommer från som säkra utgående trafik från hello gateway agent. Lär dig mer om [hur hello datagateway fungerar](logic-apps-gateway-install.md#gateway-cloud-service).

#### <a name="azure-api-management"></a>Azure API Management

[Azure API Management](https://azure.microsoft.com/services/api-management/) har lokala anslutningsmöjligheter, inklusive plats-till-plats VPN- och ExpressRoute-integrering för säker kommunikation för proxy och tooon lokala system. Du kan snabbt markera en API exponeras från Azure API Management i ett arbetsflöde, vilket ger snabb åtkomst tooon lokala system i hello logik App Designer.

#### <a name="hybrid-connections-from-azure-app-service"></a>Hybridanslutningar från Azure App Service

Du kan använda hello lokalt hybrid anslutning funktionen för Azure API och Web apps toocommunicate lokalt.  Information om hybridanslutningar och hur du kan hitta tooconfigure [i den här artikeln](../app-service-web/web-sites-hybrid-connection-get-started.md).

## <a name="next-steps"></a>Nästa steg
[Skapa en Distributionsmall för](logic-apps-create-deploy-template.md)  
[Undantagshantering](logic-apps-exception-handling.md)  
[Övervaka dina logikappar](logic-apps-monitor-your-logic-apps.md)  
[Diagnostisera logik app fel och problem](logic-apps-diagnosing-failures.md)  
