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
# <a name="resource-manager-rest-apis"></a><span data-ttu-id="c0b6b-103">REST API:er för Resurshanteraren</span><span class="sxs-lookup"><span data-stu-id="c0b6b-103">Resource Manager REST APIs</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c0b6b-104">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c0b6b-104">Azure PowerShell</span></span>](powershell-azure-resource-manager.md)
> * [<span data-ttu-id="c0b6b-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c0b6b-105">Azure CLI</span></span>](xplat-cli-azure-resource-manager.md)
> * [<span data-ttu-id="c0b6b-106">Portal</span><span class="sxs-lookup"><span data-stu-id="c0b6b-106">Portal</span></span>](resource-group-portal.md) 
> * [<span data-ttu-id="c0b6b-107">REST-API</span><span class="sxs-lookup"><span data-stu-id="c0b6b-107">REST API</span></span>](resource-manager-rest-api.md)
> 
> 

<span data-ttu-id="c0b6b-108">Bakom varje anrop tooAzure Resource Manager bakom alla distribuerade mallen bakom alla konfigurerade storage-konto finns en eller flera anrop toohello Azure Resource Manager-RESTful-API.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-108">Behind every call tooAzure Resource Manager, behind every deployed template, behind every configured storage account there are one or more calls toohello Azure Resource Manager's RESTful API.</span></span> <span data-ttu-id="c0b6b-109">Det här avsnittet är avsedda toothose API: er och hur du kan anropa dem utan att använda alla SDK alls.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-109">This topic is devoted toothose APIs and how you can call them without using any SDK at all.</span></span> <span data-ttu-id="c0b6b-110">Den här metoden är användbar om du vill ha fullständig kontroll över begäranden tooAzure, eller om hello SDK för ditt språk är inte tillgänglig eller inte stöder hello-åtgärder som du behöver.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-110">This approach is useful if you want full control of requests tooAzure, or if hello SDK for your preferred language is not available or doesn't support hello operations you need.</span></span>

<span data-ttu-id="c0b6b-111">Den här artikeln går inte via varje API som exponeras i Azure, men i stället använder vissa åtgärder som exempel på hur du ansluter toothem.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-111">This article does not go through every API that is exposed in Azure, but rather uses some operations as examples of how you connect toothem.</span></span> <span data-ttu-id="c0b6b-112">När du förstår grunderna hello, kan du läsa hello [Azure Resource Manager REST API Reference](https://docs.microsoft.com/rest/api/resources/) toofind detaljerad information om hur toouse hello resten av hello API: er.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-112">After you understand hello basics, you can read hello [Azure Resource Manager REST API Reference](https://docs.microsoft.com/rest/api/resources/) toofind detailed information on how toouse hello rest of hello APIs.</span></span>

## <a name="authentication"></a><span data-ttu-id="c0b6b-113">Autentisering</span><span class="sxs-lookup"><span data-stu-id="c0b6b-113">Authentication</span></span>
<span data-ttu-id="c0b6b-114">Autentisering för Resource Manager hanteras av Azure Active Directory (AD).</span><span class="sxs-lookup"><span data-stu-id="c0b6b-114">Authentication for Resource Manager is handled by Azure Active Directory (AD).</span></span> <span data-ttu-id="c0b6b-115">tooconnect tooany API, måste du först tooauthenticate med Azure AD tooreceive någon autentiseringstoken som du kan skicka på tooevery begäran.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-115">tooconnect tooany API, you first need tooauthenticate with Azure AD tooreceive an authentication token that you can pass on tooevery request.</span></span> <span data-ttu-id="c0b6b-116">Som vi beskriver ett rent anrop direkt toohello REST API: er, antar vi att du inte vill tooauthenticate av du blir ombedd att ange ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-116">As we are describing a pure call directly toohello REST APIs, we assume that you don't want tooauthenticate by being prompted for a username and password.</span></span> <span data-ttu-id="c0b6b-117">Vi förutsätter också att du inte använder två faktor autentiseringsmekanismer.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-117">We also assume you are not using two factor authentication mechanisms.</span></span> <span data-ttu-id="c0b6b-118">Därför kan skapa vi något som kallas en Azure AD-program och ett huvudnamn för tjänsten som används toolog i.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-118">Therefore, we create what is called an Azure AD Application and a service principal that are used toolog in.</span></span> <span data-ttu-id="c0b6b-119">Men kom ihåg att Azure AD stöder flera autentiseringsprocedurer och alla kunde använda tooretrieve den autentiseringstoken som behövs för efterföljande API-begäranden.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-119">But remember that Azure AD support several authentication procedures and all of them could be used tooretrieve that authentication token that we need for subsequent API requests.</span></span>
<span data-ttu-id="c0b6b-120">Följ [skapa Azure AD-program och tjänstens huvudnamn](resource-group-create-service-principal-portal.md) steg-för-steg-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-120">Follow [Create Azure AD Application and Service Principal](resource-group-create-service-principal-portal.md) for step by step instructions.</span></span>

### <a name="generating-an-access-token"></a><span data-ttu-id="c0b6b-121">Generera en åtkomst-Token</span><span class="sxs-lookup"><span data-stu-id="c0b6b-121">Generating an Access Token</span></span>
<span data-ttu-id="c0b6b-122">Autentisering mot Azure AD gör du genom att nämna tooAzure AD, som finns i login.microsoftonline.com. tooauthenticate, behöver du toohave hello följande information:</span><span class="sxs-lookup"><span data-stu-id="c0b6b-122">Authentication against Azure AD is done by calling out tooAzure AD, located at login.microsoftonline.com. tooauthenticate, you need toohave hello following information:</span></span>

* <span data-ttu-id="c0b6b-123">Azure AD-klient-ID (hello namnet på den Azure AD som du använder toolog i, ofta hello inte nödvändiga men samma som ditt företag)</span><span class="sxs-lookup"><span data-stu-id="c0b6b-123">Azure AD Tenant ID (hello name of that Azure AD you are using toolog in, often hello same as your company but not necessary)</span></span>
* <span data-ttu-id="c0b6b-124">Program-ID (tas under steg i att skapa programmet hello Azure AD)</span><span class="sxs-lookup"><span data-stu-id="c0b6b-124">Application ID (taken during hello Azure AD application creation step)</span></span>
* <span data-ttu-id="c0b6b-125">Lösenord (som du valde när du skapar hello Azure AD-program)</span><span class="sxs-lookup"><span data-stu-id="c0b6b-125">Password (that you selected while creating hello Azure AD Application)</span></span>

<span data-ttu-id="c0b6b-126">Se till att tooreplace ”Azure AD-klient-ID”, ”program-ID” och ”Password” med hello rätt värden i hello efter HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-126">In hello following HTTP request, make sure tooreplace "Azure AD Tenant ID", "Application ID" and "Password" with hello correct values.</span></span>

<span data-ttu-id="c0b6b-127">**Allmän HTTP-begäran:**</span><span class="sxs-lookup"><span data-stu-id="c0b6b-127">**Generic HTTP Request:**</span></span>

```HTTP
POST /<Azure AD Tenant ID>/oauth2/token?api-version=1.0 HTTP/1.1 HTTP/1.1
Host: login.microsoftonline.com
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&resource=https%3A%2F%2Fmanagement.core.windows.net%2F&client_id=<Application ID>&client_secret=<Password>
```

<span data-ttu-id="c0b6b-128">... kommer (om autentiseringen lyckas) resulterar i ett svar liknande toohello efter svar:</span><span class="sxs-lookup"><span data-stu-id="c0b6b-128">... will (if authentication succeeds) result in a response similar toohello following response:</span></span>

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
<span data-ttu-id="c0b6b-129">(hello access_token i hello före svar har kortare tooincrease läsbarhet)</span><span class="sxs-lookup"><span data-stu-id="c0b6b-129">(hello access_token in hello preceding response have been shortened tooincrease readability)</span></span>

<span data-ttu-id="c0b6b-130">**Generera åtkomsttoken med hjälp av Bash:**</span><span class="sxs-lookup"><span data-stu-id="c0b6b-130">**Generating access token using Bash:**</span></span>

```console
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=client_credentials&resource=https://management.core.windows.net/&client_id=<application id>&client_secret=<password you selected for authentication>" https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0
```

<span data-ttu-id="c0b6b-131">**Generera åtkomsttoken med hjälp av PowerShell:**</span><span class="sxs-lookup"><span data-stu-id="c0b6b-131">**Generating access token using PowerShell:**</span></span>

```powershell
Invoke-RestMethod -Uri https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0 -Method Post
 -Body @{"grant_type" = "client_credentials"; "resource" = "https://management.core.windows.net/"; "client_id" = "<application id>"; "client_secret" = "<password you selected for authentication>" }
```

<span data-ttu-id="c0b6b-132">hello svaret innehåller ett åtkomsttoken, information om hur lång tid att token är giltig och information om vilken resurs som du kan använda den token för.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-132">hello response contains an access token, information about how long that token is valid, and information about what resource you can use that token for.</span></span>
<span data-ttu-id="c0b6b-133">hello åtkomst-token som du fick i hello tidigare http-anrop måste överföras för alla begäran toohello Resource Manager API.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-133">hello access token you received in hello previous HTTP call must be passed in for all request toohello Resource Manager API.</span></span> <span data-ttu-id="c0b6b-134">Du skickar det som ett värde med ett huvud med namnet ”tillstånd” med hello värdet ”ägar YOUR_ACCESS_TOKEN”.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-134">You pass it as a header value named "Authorization" with hello value "Bearer YOUR_ACCESS_TOKEN".</span></span> <span data-ttu-id="c0b6b-135">Observera hello blanksteg mellan ”ägar” och åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-135">Notice hello space between "Bearer" and your access token.</span></span>

<span data-ttu-id="c0b6b-136">Som du ser i hello ovan HTTP resultatet är hello token giltig för en specifik tidsperiod under vilken du bör cachelagra och återanvända samma säkerhetstoken.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-136">As you can see from hello above HTTP Result, hello token is valid for a specific period of time during which you should cache and reuse that same token.</span></span> <span data-ttu-id="c0b6b-137">Även om det är möjligt tooauthenticate mot Azure AD för varje API-anrop, skulle det vara mycket ineffektiv.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-137">Even if it is possible tooauthenticate against Azure AD for each API call, it would be highly inefficient.</span></span>

## <a name="calling-resource-manager-rest-apis"></a><span data-ttu-id="c0b6b-138">Anropar Resource Manager REST API: er</span><span class="sxs-lookup"><span data-stu-id="c0b6b-138">Calling Resource Manager REST APIs</span></span>
<span data-ttu-id="c0b6b-139">Det här avsnittet använder endast några API: er tooexplain hello grundläggande användning av hello REST-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-139">This topic only uses a few APIs tooexplain hello basic usage of hello REST operations.</span></span> <span data-ttu-id="c0b6b-140">Information om alla åtgärder för hello finns [Azure Resource Manager REST API: er](https://docs.microsoft.com/rest/api/resources/).</span><span class="sxs-lookup"><span data-stu-id="c0b6b-140">For information about all hello operations, see [Azure Resource Manager REST APIs](https://docs.microsoft.com/rest/api/resources/).</span></span>

### <a name="list-all-subscriptions"></a><span data-ttu-id="c0b6b-141">Visa en lista över alla prenumerationer</span><span class="sxs-lookup"><span data-stu-id="c0b6b-141">List all subscriptions</span></span>
<span data-ttu-id="c0b6b-142">En av hello enklaste åtgärder du kan göra är toolist hello tillgängliga prenumerationer som du har åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-142">One of hello simplest operations you can do is toolist hello available subscriptions that you can access.</span></span> <span data-ttu-id="c0b6b-143">På hello på begäran, ser du hur hello åtkomst-token som skickas som en rubrik:</span><span class="sxs-lookup"><span data-stu-id="c0b6b-143">In hello following request, you see how hello access token is passed in as a header:</span></span>

<span data-ttu-id="c0b6b-144">(Ersätt YOUR_ACCESS_TOKEN med ditt faktiska åtkomst-Token.)</span><span class="sxs-lookup"><span data-stu-id="c0b6b-144">(Replace YOUR_ACCESS_TOKEN with your actual Access Token.)</span></span>

```HTTP
GET /subscriptions?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

<span data-ttu-id="c0b6b-145">... och därför kan hämta en lista över prenumerationer som den här tjänstens huvudnamn får tooaccess</span><span class="sxs-lookup"><span data-stu-id="c0b6b-145">... and as a result, you get a list of subscriptions that this service principal is allowed tooaccess</span></span>

<span data-ttu-id="c0b6b-146">(Prenumerations-ID: N har minskats för läsbarhet)</span><span class="sxs-lookup"><span data-stu-id="c0b6b-146">(Subscription IDs have been shortened for readability)</span></span>

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

### <a name="list-all-resource-groups-in-a-specific-subscription"></a><span data-ttu-id="c0b6b-147">Visa en lista över alla resursgrupper i en viss prenumeration</span><span class="sxs-lookup"><span data-stu-id="c0b6b-147">List all resource groups in a specific subscription</span></span>
<span data-ttu-id="c0b6b-148">Alla resurser som är tillgängliga med hello Resource Manager API: er är kapslad i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-148">All resources available with hello Resource Manager APIs are nested inside a resource group.</span></span> <span data-ttu-id="c0b6b-149">Du kan fråga Resource Manager för befintliga resursgrupper i din prenumeration med hjälp av hello följande HTTP GET-begäran.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-149">You can query Resource Manager for existing resource groups in your subscription using hello following HTTP GET request.</span></span> <span data-ttu-id="c0b6b-150">Observera hur hello prenumerations-ID skickas som en del av hello URL nu.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-150">Notice how hello subscription ID is passed in as part of hello URL this time.</span></span>

<span data-ttu-id="c0b6b-151">(Ersätt YOUR_ACCESS_TOKEN och PRENUMERATIONSID med ditt faktiska åtkomst-token och prenumerations-ID)</span><span class="sxs-lookup"><span data-stu-id="c0b6b-151">(Replace YOUR_ACCESS_TOKEN and SUBSCRIPTION_ID with your actual access token and subscription ID)</span></span>

```HTTP
GET /subscriptions/SUBSCRIPTION_ID/resourcegroups?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

<span data-ttu-id="c0b6b-152">hello svar du får beror på om du har alla resursgrupper som har definierats och i så fall, hur många.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-152">hello response you get depends on whether you have any resource groups defined and if so, how many.</span></span>

<span data-ttu-id="c0b6b-153">(Prenumerations-ID: N har minskats för läsbarhet)</span><span class="sxs-lookup"><span data-stu-id="c0b6b-153">(Subscription IDs have been shortened for readability)</span></span>

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

### <a name="create-a-resource-group"></a><span data-ttu-id="c0b6b-154">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="c0b6b-154">Create a resource group</span></span>
<span data-ttu-id="c0b6b-155">Hittills har har vi endast frågar hello Resource Manager API: er för information.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-155">So far, we've only been querying hello Resource Manager APIs for information.</span></span> <span data-ttu-id="c0b6b-156">Det är dags vi skapa vissa resurser, och låt oss börja med hello enklaste av dem alla, en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-156">It's time we create some resources, and let's start by hello simplest of them all, a resource group.</span></span> <span data-ttu-id="c0b6b-157">hello följande HTTP-begäran skapar en resursgrupp i en region/plats och lägger till en tagg tooit.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-157">hello following HTTP request creates a resource group in a region/location of your choice, and adds a tag tooit.</span></span>

<span data-ttu-id="c0b6b-158">(Ersätta YOUR_ACCESS_TOKEN, PRENUMERATIONSID, RESOURCE_GROUP_NAME med faktiska åtkomst-Token, prenumerations-ID och namnet på hello resursgrupp toocreate)</span><span class="sxs-lookup"><span data-stu-id="c0b6b-158">(Replace YOUR_ACCESS_TOKEN, SUBSCRIPTION_ID, RESOURCE_GROUP_NAME with your actual Access Token, Subscription ID and name of hello Resource Group you want toocreate)</span></span>

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

<span data-ttu-id="c0b6b-159">Om det lyckas, får du ett svar som är liknande toohello efter svar:</span><span class="sxs-lookup"><span data-stu-id="c0b6b-159">If successful, you get a response that is similar toohello following response:</span></span>

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

<span data-ttu-id="c0b6b-160">Du har skapat en resursgrupp i Azure.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-160">You've successfully created a resource group in Azure.</span></span> <span data-ttu-id="c0b6b-161">Grattis!</span><span class="sxs-lookup"><span data-stu-id="c0b6b-161">Congratulations!</span></span>

### <a name="deploy-resources-tooa-resource-group-using-a-resource-manager-template"></a><span data-ttu-id="c0b6b-162">Distribuera resurser tooa resursgrupp med en Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="c0b6b-162">Deploy resources tooa resource group using a Resource Manager Template</span></span>
<span data-ttu-id="c0b6b-163">Med Resource Manager kan du distribuera dina resurser med hjälp av mallar.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-163">With Resource Manager, you can deploy your resources using templates.</span></span> <span data-ttu-id="c0b6b-164">En mall definierar flera resurser och deras beroenden.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-164">A template defines several resources and their dependencies.</span></span> <span data-ttu-id="c0b6b-165">För det här avsnittet förutsätter vi att du är bekant med Resource Manager-mallar och vi bara att visa hur toomake hello API anropa toostart distribution.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-165">For this section, we assume you are familiar with Resource Manager templates, and we just show you how toomake hello API call toostart deployment.</span></span> <span data-ttu-id="c0b6b-166">Mer information om hur du skapar mallar finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="c0b6b-166">For more information about constructing templates, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="c0b6b-167">Distribution av en mall inte skilja sig mycket toohow du anropa andra API: er.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-167">Deployment of a template doesn't differ much toohow you call other APIs.</span></span> <span data-ttu-id="c0b6b-168">En viktig aspekt är att distributionen av en mall kan ta lång tid.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-168">One important aspect is that deployment of a template can take quite a long time.</span></span> <span data-ttu-id="c0b6b-169">hello API-anrop returnerar bara och är det upp tooyou som utvecklare tooquery status för hello distribution toofind ut när hello distributionen är klar.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-169">hello API call just returns, and it's up tooyou as developer tooquery for status of hello deployment toofind out when hello deployment is done.</span></span> <span data-ttu-id="c0b6b-170">Mer information finns i [spåra asynkrona åtgärder i Azure](resource-manager-async-operations.md).</span><span class="sxs-lookup"><span data-stu-id="c0b6b-170">For more information, see [Track asynchronous Azure operations](resource-manager-async-operations.md).</span></span>

<span data-ttu-id="c0b6b-171">I det här exemplet använder vi ett offentligt exponerade mallen som finns på [GitHub](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="c0b6b-171">For this example, we use a publicly exposed template available on [GitHub](https://github.com/Azure/azure-quickstart-templates).</span></span> <span data-ttu-id="c0b6b-172">hello-mallen använder vi distribuerar en Linux VM toohello västra USA region.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-172">hello template we use deploys a Linux VM toohello West US region.</span></span> <span data-ttu-id="c0b6b-173">Även om det här exemplet används en mall som är tillgängliga i en offentlig databas som GitHub, kan du i stället skickas hello fullständig mallen i hello begäran.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-173">Even though this example uses a template available in a public repository like GitHub, you can instead pass hello full template as part of hello request.</span></span> <span data-ttu-id="c0b6b-174">Observera att vi tillhandahåller parametervärden i hello begäran som används i hello distribuera mallen.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-174">Note that we provide parameter values in hello request that are used inside hello deployed template.</span></span>

<span data-ttu-id="c0b6b-175">(Ersätta PRENUMERATIONSID, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, lösenord och DNS_NAME_FOR_PUBLIC_IP toovalues som är lämpliga för din begäran)</span><span class="sxs-lookup"><span data-stu-id="c0b6b-175">(Replace SUBSCRIPTION_ID, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD and DNS_NAME_FOR_PUBLIC_IP toovalues appropriate for your request)</span></span>

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

<span data-ttu-id="c0b6b-176">hello lång JSON-svar för denna begäran har utelämnat tooimprove läsbarhet i den här dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-176">hello long JSON response for this request has been omitted tooimprove readability of this documentation.</span></span> <span data-ttu-id="c0b6b-177">hello svaret innehåller information om mallbaserat hello-distribution som du skapade.</span><span class="sxs-lookup"><span data-stu-id="c0b6b-177">hello response contains information about hello templated deployment that you created.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c0b6b-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c0b6b-178">Next steps</span></span>

- <span data-ttu-id="c0b6b-179">toolearn om hantering av asynkrona REST-åtgärder, se [spåra asynkrona åtgärder i Azure](resource-manager-async-operations.md).</span><span class="sxs-lookup"><span data-stu-id="c0b6b-179">toolearn about handling asynchronous REST operations, see [Track asynchronous Azure operations](resource-manager-async-operations.md).</span></span>
