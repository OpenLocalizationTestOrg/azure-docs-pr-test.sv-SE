---
title: 'aaaResource Manager REST API: er | Microsoft Docs'
description: "En översikt över hello Resource Manager REST API: er autentisering och exempel på användning"
services: azure-resource-manager
documentationcenter: na
author: navalev
manager: timlt
editor: 
ms.assetid: e8d7a1d2-1e82-4212-8288-8697341408c5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/13/2017
ms.author: navale;tomfitz;
ms.openlocfilehash: 3ccc3575c5e06c41f2fdc5317711980fc6a2f649
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="resource-manager-rest-apis"></a>REST API:er för Resurshanteraren
> [!div class="op_single_selector"]
> * [Azure PowerShell](powershell-azure-resource-manager.md)
> * [Azure CLI](xplat-cli-azure-resource-manager.md)
> * [Portal](resource-group-portal.md) 
> * [REST-API](resource-manager-rest-api.md)
> 
> 

Bakom varje anrop tooAzure Resource Manager bakom alla distribuerade mallen bakom alla konfigurerade storage-konto finns en eller flera anrop toohello Azure Resource Manager-RESTful-API. Det här avsnittet är avsedda toothose API: er och hur du kan anropa dem utan att använda alla SDK alls. Den här metoden är användbar om du vill ha fullständig kontroll över begäranden tooAzure, eller om hello SDK för ditt språk är inte tillgänglig eller inte stöder hello-åtgärder som du behöver.

Den här artikeln går inte via varje API som exponeras i Azure, men i stället använder vissa åtgärder som exempel på hur du ansluter toothem. När du förstår grunderna hello, kan du läsa hello [Azure Resource Manager REST API Reference](https://docs.microsoft.com/rest/api/resources/) toofind detaljerad information om hur toouse hello resten av hello API: er.

## <a name="authentication"></a>Autentisering
Autentisering för Resource Manager hanteras av Azure Active Directory (AD). tooconnect tooany API, måste du först tooauthenticate med Azure AD tooreceive någon autentiseringstoken som du kan skicka på tooevery begäran. Som vi beskriver ett rent anrop direkt toohello REST API: er, antar vi att du inte vill tooauthenticate av du blir ombedd att ange ett användarnamn och lösenord. Vi förutsätter också att du inte använder två faktor autentiseringsmekanismer. Därför kan skapa vi något som kallas en Azure AD-program och ett huvudnamn för tjänsten som används toolog i. Men kom ihåg att Azure AD stöder flera autentiseringsprocedurer och alla kunde använda tooretrieve den autentiseringstoken som behövs för efterföljande API-begäranden.
Följ [skapa Azure AD-program och tjänstens huvudnamn](resource-group-create-service-principal-portal.md) steg-för-steg-instruktioner.

### <a name="generating-an-access-token"></a>Generera en åtkomst-Token
Autentisering mot Azure AD gör du genom att nämna tooAzure AD, som finns i login.microsoftonline.com. tooauthenticate, behöver du toohave hello följande information:

* Azure AD-klient-ID (hello namnet på den Azure AD som du använder toolog i, ofta hello inte nödvändiga men samma som ditt företag)
* Program-ID (tas under steg i att skapa programmet hello Azure AD)
* Lösenord (som du valde när du skapar hello Azure AD-program)

Se till att tooreplace ”Azure AD-klient-ID”, ”program-ID” och ”Password” med hello rätt värden i hello efter HTTP-begäran.

**Allmän HTTP-begäran:**

```HTTP
POST /<Azure AD Tenant ID>/oauth2/token?api-version=1.0 HTTP/1.1 HTTP/1.1
Host: login.microsoftonline.com
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&resource=https%3A%2F%2Fmanagement.core.windows.net%2F&client_id=<Application ID>&client_secret=<Password>
```

... kommer (om autentiseringen lyckas) resulterar i ett svar liknande toohello efter svar:

```json
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1448199959",
  "not_before": "1448196059",
  "resource": "https://management.core.windows.net/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhb...86U3JI_0InPUk_lZqWvKiEWsayA"
}
```
(hello access_token i hello före svar har kortare tooincrease läsbarhet)

**Generera åtkomsttoken med hjälp av Bash:**

```console
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=client_credentials&resource=https://management.core.windows.net/&client_id=<application id>&client_secret=<password you selected for authentication>" https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0
```

**Generera åtkomsttoken med hjälp av PowerShell:**

```powershell
Invoke-RestMethod -Uri https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0 -Method Post
 -Body @{"grant_type" = "client_credentials"; "resource" = "https://management.core.windows.net/"; "client_id" = "<application id>"; "client_secret" = "<password you selected for authentication>" }
```

hello svaret innehåller ett åtkomsttoken, information om hur lång tid att token är giltig och information om vilken resurs som du kan använda den token för.
hello åtkomst-token som du fick i hello tidigare http-anrop måste överföras för alla begäran toohello Resource Manager API. Du skickar det som ett värde med ett huvud med namnet ”tillstånd” med hello värdet ”ägar YOUR_ACCESS_TOKEN”. Observera hello blanksteg mellan ”ägar” och åtkomst-token.

Som du ser i hello ovan HTTP resultatet är hello token giltig för en specifik tidsperiod under vilken du bör cachelagra och återanvända samma säkerhetstoken. Även om det är möjligt tooauthenticate mot Azure AD för varje API-anrop, skulle det vara mycket ineffektiv.

## <a name="calling-resource-manager-rest-apis"></a>Anropar Resource Manager REST API: er
Det här avsnittet använder endast några API: er tooexplain hello grundläggande användning av hello REST-åtgärder. Information om alla åtgärder för hello finns [Azure Resource Manager REST API: er](https://docs.microsoft.com/rest/api/resources/).

### <a name="list-all-subscriptions"></a>Visa en lista över alla prenumerationer
En av hello enklaste åtgärder du kan göra är toolist hello tillgängliga prenumerationer som du har åtkomst till. På hello på begäran, ser du hur hello åtkomst-token som skickas som en rubrik:

(Ersätt YOUR_ACCESS_TOKEN med ditt faktiska åtkomst-Token.)

```HTTP
GET /subscriptions?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

... och därför kan hämta en lista över prenumerationer som den här tjänstens huvudnamn får tooaccess

(Prenumerations-ID: N har minskats för läsbarhet)

```json
{
  "value": [
    {
      "id": "/subscriptions/3a8555...555995",
      "subscriptionId": "3a8555...555995",
      "displayName": "Azure Subscription",
      "state": "Enabled",
      "subscriptionPolicies": {
        "locationPlacementId": "Internal_2014-09-01",
        "quotaId": "Internal_2014-09-01"
      }
    }
  ]
}
```

### <a name="list-all-resource-groups-in-a-specific-subscription"></a>Visa en lista över alla resursgrupper i en viss prenumeration
Alla resurser som är tillgängliga med hello Resource Manager API: er är kapslad i en resursgrupp. Du kan fråga Resource Manager för befintliga resursgrupper i din prenumeration med hjälp av hello följande HTTP GET-begäran. Observera hur hello prenumerations-ID skickas som en del av hello URL nu.

(Ersätt YOUR_ACCESS_TOKEN och PRENUMERATIONSID med ditt faktiska åtkomst-token och prenumerations-ID)

```HTTP
GET /subscriptions/SUBSCRIPTION_ID/resourcegroups?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

hello svar du får beror på om du har alla resursgrupper som har definierats och i så fall, hur många.

(Prenumerations-ID: N har minskats för läsbarhet)

```json
{
    "value": [
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/myfirstresourcegroup",
            "name": "myfirstresourcegroup",
            "location": "eastus",
            "properties": {
                "provisioningState": "Succeeded"
            }
        },
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/mysecondresourcegroup",
            "name": "mysecondresourcegroup",
            "location": "northeurope",
            "tags": {
                "tagname1": "My first tag"
            },
            "properties": {
                "provisioningState": "Succeeded"
            }
        }
    ]
}
```

### <a name="create-a-resource-group"></a>Skapa en resursgrupp
Hittills har har vi endast frågar hello Resource Manager API: er för information. Det är dags vi skapa vissa resurser, och låt oss börja med hello enklaste av dem alla, en resursgrupp. hello följande HTTP-begäran skapar en resursgrupp i en region/plats och lägger till en tagg tooit.

(Ersätta YOUR_ACCESS_TOKEN, PRENUMERATIONSID, RESOURCE_GROUP_NAME med faktiska åtkomst-Token, prenumerations-ID och namnet på hello resursgrupp toocreate)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  }
}
```

Om det lyckas, får du ett svar som är liknande toohello efter svar:

```json
{
  "id": "/subscriptions/3a8555...555995/resourceGroups/RESOURCE_GROUP_NAME",
  "name": "RESOURCE_GROUP_NAME",
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  },
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

Du har skapat en resursgrupp i Azure. Grattis!

### <a name="deploy-resources-tooa-resource-group-using-a-resource-manager-template"></a>Distribuera resurser tooa resursgrupp med en Resource Manager-mall
Med Resource Manager kan du distribuera dina resurser med hjälp av mallar. En mall definierar flera resurser och deras beroenden. För det här avsnittet förutsätter vi att du är bekant med Resource Manager-mallar och vi bara att visa hur toomake hello API anropa toostart distribution. Mer information om hur du skapar mallar finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).

Distribution av en mall inte skilja sig mycket toohow du anropa andra API: er. En viktig aspekt är att distributionen av en mall kan ta lång tid. hello API-anrop returnerar bara och är det upp tooyou som utvecklare tooquery status för hello distribution toofind ut när hello distributionen är klar. Mer information finns i [spåra asynkrona åtgärder i Azure](resource-manager-async-operations.md).

I det här exemplet använder vi ett offentligt exponerade mallen som finns på [GitHub](https://github.com/Azure/azure-quickstart-templates). hello-mallen använder vi distribuerar en Linux VM toohello västra USA region. Även om det här exemplet används en mall som är tillgängliga i en offentlig databas som GitHub, kan du i stället skickas hello fullständig mallen i hello begäran. Observera att vi tillhandahåller parametervärden i hello begäran som används i hello distribuera mallen.

(Ersätta PRENUMERATIONSID, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, lösenord och DNS_NAME_FOR_PUBLIC_IP toovalues som är lämpliga för din begäran)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME/providers/microsoft.resources/deployments/DEPLOYMENT_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "properties": {
    "templateLink": {
      "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json",
      "contentVersion": "1.0.0.0",
    },
    "mode": "Incremental",
    "parameters": {
        "newStorageAccountName": {
          "value": "GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME"
        },
        "adminUsername": {
          "value": "ADMIN_USER_NAME"
        },
        "adminPassword": {
          "value": "ADMIN_PASSWORD"
        },
        "dnsNameForPublicIP": {
          "value": "DNS_NAME_FOR_PUBLIC_IP"
        },
        "ubuntuOSVersion": {
          "value": "15.04"
        }
    }
  }
}
```

hello lång JSON-svar för denna begäran har utelämnat tooimprove läsbarhet i den här dokumentationen. hello svaret innehåller information om mallbaserat hello-distribution som du skapade.

## <a name="next-steps"></a>Nästa steg

- toolearn om hantering av asynkrona REST-åtgärder, se [spåra asynkrona åtgärder i Azure](resource-manager-async-operations.md).
