---
title: 'aaaAuthenticate med Mobile Engagement REST API: er'
description: 'Beskriver hur tooauthenticate med Azure Mobile Engagement REST API: er'
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: da82cb36-957a-4e19-a805-b44733cf6597
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 10/05/2016
ms.author: wesmc;ricksal
ms.openlocfilehash: 9b54aa5ec3da4bcf55ffe5b7e8d1759095d0c486
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-with-mobile-engagement-rest-apis"></a><span data-ttu-id="7cfeb-103">Autentisera med Mobile Engagement REST API: er</span><span class="sxs-lookup"><span data-stu-id="7cfeb-103">Authenticate with Mobile Engagement REST APIs</span></span>
## <a name="overview"></a><span data-ttu-id="7cfeb-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="7cfeb-104">Overview</span></span>
<span data-ttu-id="7cfeb-105">Det här dokumentet beskriver hur tooget en giltig AAD-Oauth token tooauthenticate med hello Mobile Engagement REST API: er.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-105">This document describes how tooget a valid AAD Oauth token tooauthenticate with hello Mobile Engagement REST APIs.</span></span> 

<span data-ttu-id="7cfeb-106">Det förutsätts att du har en giltig Azure-prenumeration och du har skapat en Mobile Engagement-app med en av våra [Developer självstudier](mobile-engagement-windows-store-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="7cfeb-106">It is assumed that you have a valid Azure subscription and you have created a Mobile Engagement app using one of our [Developer Tutorials](mobile-engagement-windows-store-dotnet-get-started.md).</span></span>

## <a name="authentication"></a><span data-ttu-id="7cfeb-107">Autentisering</span><span class="sxs-lookup"><span data-stu-id="7cfeb-107">Authentication</span></span>
<span data-ttu-id="7cfeb-108">Microsoft Azure Active Directory baserat OAuth-token används för autentisering.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-108">A Microsoft Azure Active Directory based OAuth token is used for authentication.</span></span> 

<span data-ttu-id="7cfeb-109">Ordning tooauthentication en API-begäran, måste ett authorization-huvud läggas tooevery begäran som är av hello följande format:</span><span class="sxs-lookup"><span data-stu-id="7cfeb-109">In order tooauthentication an API request, an authorization header must be added tooevery request which is of hello following form:</span></span>

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

> [!NOTE]
> <span data-ttu-id="7cfeb-110">Azure Active Directory-token upphör gälla inom en timme.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-110">Azure Active Directory tokens expire in 1 hour.</span></span>
> 
> 

<span data-ttu-id="7cfeb-111">Det finns flera sätt tooget en token.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-111">There are several ways tooget a token.</span></span> <span data-ttu-id="7cfeb-112">Eftersom hello API: er kallas vanligtvis från en molnbaserad tjänst, vill du toouse en API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-112">Since hello APIs are generally called from a cloud service, you want toouse an API key.</span></span> <span data-ttu-id="7cfeb-113">En API-nyckel i Azure-terminologi kallas Service principal lösenord.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-113">An API key in Azure terminology is called a Service principal password.</span></span> <span data-ttu-id="7cfeb-114">hello följande procedur beskriver ett sätt toosetting den upp manuellt.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-114">hello following procedure describes one way toosetting it up manually.</span></span>

### <a name="one-time-setup-using-script"></a><span data-ttu-id="7cfeb-115">Enstaka installationen (med hjälp av skript)</span><span class="sxs-lookup"><span data-stu-id="7cfeb-115">One-time setup (using script)</span></span>
<span data-ttu-id="7cfeb-116">Du bör följa hello uppsättningen anvisningar nedan tooperform hello installationen med hjälp av ett PowerShell-skript som tar hello Minimitiden för installationen, men använder hello mest tillåtna standardvärden.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-116">You should follow hello set of instructions below tooperform hello setup using a PowerShell script which takes hello minimum time for setup but uses hello most permissible defaults.</span></span> <span data-ttu-id="7cfeb-117">Alternativt kan du kan även följa hello instruktionerna i hello [manuell installation](mobile-engagement-api-authentication-manual.md) för att göra detta från hello Azure-portalen direkt och vill ha bättre konfiguration.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-117">Optionally, you can also follow hello instructions in hello [manual setup](mobile-engagement-api-authentication-manual.md) for doing this from hello Azure portal directly and do finer configuration.</span></span> 

1. <span data-ttu-id="7cfeb-118">Hämta hello senaste versionen av Azure PowerShell från [här](http://aka.ms/webpi-azps).</span><span class="sxs-lookup"><span data-stu-id="7cfeb-118">Get hello latest version of Azure PowerShell from [here](http://aka.ms/webpi-azps).</span></span> <span data-ttu-id="7cfeb-119">Mer information om hello Hämtningsinstruktioner kan du se detta [länk](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7cfeb-119">For more information on hello download instructions, you can see this [link](/powershell/azure/overview).</span></span>  
2. <span data-ttu-id="7cfeb-120">När du har installerat Azure PowerShell Använd hello följande kommandon för att du har hello tooensure **Azure-modulen** installerad:</span><span class="sxs-lookup"><span data-stu-id="7cfeb-120">Once Azure PowerShell is installed, use hello following commands tooensure that you have hello **Azure module** installed:</span></span>
   
    <span data-ttu-id="7cfeb-121">a.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-121">a.</span></span> <span data-ttu-id="7cfeb-122">Kontrollera att hello Azure PowerShell-modulen är tillgängliga i hello listan över tillgängliga moduler.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-122">Make sure hello Azure PowerShell module is available in hello list of available modules.</span></span> 
   
        Get-Module –ListAvailable 
   
    ![Tillgängliga Azure moduler][1]
   
    <span data-ttu-id="7cfeb-124">b.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-124">b.</span></span> <span data-ttu-id="7cfeb-125">Om du inte hittar hello Azure PowerShell-modulen i hello ovanför listan måste toorun hello följande:</span><span class="sxs-lookup"><span data-stu-id="7cfeb-125">If you do not find hello Azure PowerShell module in hello above list then you need toorun hello following:</span></span>
   
        Import-Module Azure 
3. <span data-ttu-id="7cfeb-126">Inloggningen toohello Azure Resource Manager från PowerShell genom att köra hello följande kommando och ange ditt användarnamn och lösenord för kontot:</span><span class="sxs-lookup"><span data-stu-id="7cfeb-126">Login toohello Azure Resource Manager from PowerShell by running hello following command and providing your user name and password for your Azure account:</span></span> 
   
        Login-AzureRmAccount
4. <span data-ttu-id="7cfeb-127">Om du har flera prenumerationer bör du köra hello följande:</span><span class="sxs-lookup"><span data-stu-id="7cfeb-127">If you have multiple subscriptions then you should run hello following:</span></span>
   
    <span data-ttu-id="7cfeb-128">a.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-128">a.</span></span> <span data-ttu-id="7cfeb-129">Hämta en lista över alla prenumerationer och kopiera hello prenumerations-ID för hello-prenumeration som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-129">Get a list of all your subscriptions and copy hello SubscriptionId of hello subscription you want toouse.</span></span> <span data-ttu-id="7cfeb-130">Kontrollera att den här prenumerationen är hello samma som har hello Mobile Engagement-App som du kommer toointeract med hello API: er.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-130">Make sure this subscription is hello same one which has hello Mobile Engagement App which you are going toointeract with using hello APIs.</span></span> 
   
        Get-AzureRmSubscription
   
    <span data-ttu-id="7cfeb-131">b.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-131">b.</span></span> <span data-ttu-id="7cfeb-132">Hello kör följande kommando med hello SubscriptionId tooconfigure hello prenumeration toobe används.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-132">Run hello following command providing hello SubscriptionId tooconfigure hello subscription toobe used.</span></span>
   
        Select-AzureRmSubscription –SubscriptionId <subscriptionId>
5. <span data-ttu-id="7cfeb-133">Kopiera hello text för hello [ny AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) skript tooyour lokala dator och spara den som en PowerShell-cmdlet (t.ex. `APIAuth.ps1`) och kör den `.\APIAuth.ps1`.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-133">Copy hello text for hello [New-AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) script tooyour local machine and save it as a PowerShell cmdlet (e.g. `APIAuth.ps1`) and execute it `.\APIAuth.ps1`.</span></span> 
6. <span data-ttu-id="7cfeb-134">hello skript ber dig tooprovide en indata för **huvudkontot**.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-134">hello script will ask you tooprovide an input for **principalName**.</span></span> <span data-ttu-id="7cfeb-135">Ange ett lämpligt namn som du vill toouse toocreate Active Directory-program (t.ex. APIAuth).</span><span class="sxs-lookup"><span data-stu-id="7cfeb-135">Provide a suitable name here that you want toouse toocreate your Active Directory application (e.g. APIAuth).</span></span> 
7. <span data-ttu-id="7cfeb-136">När hello skriptet har slutförts visas följande fyra värden som du behöver hello tooauthenticate programmässigt med AD så se till att toocopy dem.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-136">After hello script completes, it will display hello following four values that you will need tooauthenticate programmatically with AD so make sure toocopy them.</span></span> 
   
    <span data-ttu-id="7cfeb-137">**TenantId**, **SubscriptionId**, **ApplicationId**, och **hemlighet**.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-137">**TenantId**, **SubscriptionId**, **ApplicationId**, and **Secret**.</span></span>
   
    <span data-ttu-id="7cfeb-138">Du kommer att använda TenantId som `{TENANT_ID}`, ApplicationId som `{CLIENT_ID}` och hemliga som `{CLIENT_SECRET}`.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-138">You will use TenantId as `{TENANT_ID}`, ApplicationId as `{CLIENT_ID}` and Secret as `{CLIENT_SECRET}`.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="7cfeb-139">Säkerhetsprinciperna standard kan blockeras från att köra ett PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-139">Your default security policy may block you from running a PowerShell scripts.</span></span> <span data-ttu-id="7cfeb-140">I så fall, konfigurera tillfälligt din körning princip tooallow skriptkörningen med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7cfeb-140">If so, you temporarily configure your execution policy tooallow script execution using hello following command:</span></span>
   > 
   > <span data-ttu-id="7cfeb-141">Set-ExecutionPolicy RemoteSigned</span><span class="sxs-lookup"><span data-stu-id="7cfeb-141">Set-ExecutionPolicy RemoteSigned</span></span>
   > 
   > 
8. <span data-ttu-id="7cfeb-142">Här är hur hello uppsättning PS-cmdlets skulle se ut.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-142">Here is how hello set of PS cmdlets would look like.</span></span> 
   
    ![][3]
9. <span data-ttu-id="7cfeb-143">Kontrollera i hello Azure Management portal att ett nytt AD-program har skapats med hello namn du angivna toohello skript kallas **Principalnamnet** under **Visa program som företaget äger**.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-143">Check in hello Azure Management portal that a new AD application was created with hello name you provided toohello script called **principalName** under **Show Applications my company owns**.</span></span>
   
    ![][4]

#### <a name="steps-tooget-a-valid-token"></a><span data-ttu-id="7cfeb-144">Steg tooget en giltig token</span><span class="sxs-lookup"><span data-stu-id="7cfeb-144">Steps tooget a valid token</span></span>
1. <span data-ttu-id="7cfeb-145">Anropa API för hello med hello följande parametrar och se till att tooreplace hello klient\_, klient-ID\_-ID och klienten\_HEMLIGHET:</span><span class="sxs-lookup"><span data-stu-id="7cfeb-145">Call hello API with hello following parameters and make sure tooreplace hello TENANT\_ID, CLIENT\_ID and CLIENT\_SECRET:</span></span>
   
   * <span data-ttu-id="7cfeb-146">**URL-begäran** som *https://login.microsoftonline.com/ {klient\_ID} / oauth2/token*</span><span class="sxs-lookup"><span data-stu-id="7cfeb-146">**Request URL** as *https://login.microsoftonline.com/{TENANT\_ID}/oauth2/token*</span></span>
   * <span data-ttu-id="7cfeb-147">**HTTP-innhållstyphuvud** som *program/x-www-formuläret-urlencoded*</span><span class="sxs-lookup"><span data-stu-id="7cfeb-147">**HTTP Content-Type header** as *application/x-www-form-urlencoded*</span></span>
   * <span data-ttu-id="7cfeb-148">**Brödtext i HTTP-begäran** som *bevilja\_typ = klient\_autentiseringsuppgifter & client_id = {klienten\_ID} & client_secret = {klienten\_HEMLIGHET} & resource=https%3A%2F%2Fmanagement.core.windows.net%2F*</span><span class="sxs-lookup"><span data-stu-id="7cfeb-148">**HTTP Request Body** as *grant\_type=client\_credentials&client_id={CLIENT\_ID}&client_secret={CLIENT\_SECRET}&resource=https%3A%2F%2Fmanagement.core.windows.net%2F*</span></span>
     
     <span data-ttu-id="7cfeb-149">hello följande är en exempelbegäran:</span><span class="sxs-lookup"><span data-stu-id="7cfeb-149">hello following is an example request:</span></span>
     
       <span data-ttu-id="7cfeb-150">POST / {TENANT_ID} / oauth2/token HTTP/1.1 värden: login.microsoftonline.com Content-Type: program/x-www-formuläret-urlencoded grant_type = client_credentials & client_id = {CLIENT_ID} & client_secret = {CLIENT_SECRET} & sammanstä spänningskälla = https % 3A % 2F%2Fmanagement.Core.Windows.NET%2F</span><span class="sxs-lookup"><span data-stu-id="7cfeb-150">POST /{TENANT_ID}/oauth2/token HTTP/1.1   Host: login.microsoftonline.com   Content-Type: application/x-www-form-urlencoded   grant_type=client_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&reso   urce=https%3A%2F%2Fmanagement.core.windows.net%2F</span></span>
     
     <span data-ttu-id="7cfeb-151">Här är ett exempelsvar:</span><span class="sxs-lookup"><span data-stu-id="7cfeb-151">Here is an example response:</span></span>
     
       <span data-ttu-id="7cfeb-152">HTTP/1.1 200 OK Content-Type: application/json; charset = utf-8 Content-Length: 1234</span><span class="sxs-lookup"><span data-stu-id="7cfeb-152">HTTP/1.1 200 OK   Content-Type: application/json; charset=utf-8   Content-Length: 1234</span></span>
     
       <span data-ttu-id="7cfeb-153">{”token_type”: ”ägar”, ”expires_in”: ”3599”, ”expires_on”: ”1445395811”, ”not_before” ”: 144 5391911” ”, resurs”: ”https://management.core.windows.net/”, ”access_token”: {ACCESS_TOKEN}}</span><span class="sxs-lookup"><span data-stu-id="7cfeb-153">{"token_type":"Bearer","expires_in":"3599","expires_on":"1445395811","not_before":"144   5391911","resource":"https://management.core.windows.net/","access_token":{ACCESS_TOKEN}}</span></span>
     
     <span data-ttu-id="7cfeb-154">Det här exemplet ingår URL-kodning av hello POST-parametrar, `resource` värdet är `https://management.core.windows.net/`.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-154">This example included URL encoding of hello POST parameters, `resource` value is actually `https://management.core.windows.net/`.</span></span> <span data-ttu-id="7cfeb-155">Var noga med att koda tooalso URL `{CLIENT_SECRET}` som den kan innehålla specialtecken.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-155">Be careful tooalso URL encode `{CLIENT_SECRET}` as it may contain special characters.</span></span>
     
     > [!NOTE]
     > <span data-ttu-id="7cfeb-156">För att testa, kan du använda en HTTP-klientverktyg som [Fiddler](http://www.telerik.com/fiddler) eller [Chrome Postman tillägg](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop)</span><span class="sxs-lookup"><span data-stu-id="7cfeb-156">For testing, you can use an HTTP client tool like [Fiddler](http://www.telerik.com/fiddler) or [Chrome Postman extension](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop)</span></span> 
     > 
     > 
2. <span data-ttu-id="7cfeb-157">I varje API-anrop som innehåller nu hello authorization-huvud för begäran:</span><span class="sxs-lookup"><span data-stu-id="7cfeb-157">Now in every API call, include hello authorization request header:</span></span>
   
        Authorization: Bearer {ACCESS_TOKEN}
   
    <span data-ttu-id="7cfeb-158">Om du får ett 401 returnerade statuskoden, kontrollera svarstexten hello, kan den informera hello token har upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-158">If you get a 401 status code returned, check hello response body, it might tell you hello token is expired.</span></span> <span data-ttu-id="7cfeb-159">I så fall får en ny token.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-159">In that case, get a new token.</span></span>

## <a name="using-hello-apis"></a><span data-ttu-id="7cfeb-160">Med hjälp av hello API: er</span><span class="sxs-lookup"><span data-stu-id="7cfeb-160">Using hello APIs</span></span>
<span data-ttu-id="7cfeb-161">Nu när du har en giltig token är klar toomake hello API-anrop.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-161">Now that you have a valid token, you are ready toomake hello API calls.</span></span>

1. <span data-ttu-id="7cfeb-162">I varje API-begäran måste toopass en giltiga token som du fick i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-162">In each API request, you will need toopass a valid, unexpired token which you obtained in hello previous section.</span></span>
2. <span data-ttu-id="7cfeb-163">Du behöver tooplug i vissa parametrar i hello Begärd URI som identifierar ditt program.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-163">You will need tooplug in some parameters into hello request URI which identifies your application.</span></span> <span data-ttu-id="7cfeb-164">hello URI-begäran som ser ut som följande hello</span><span class="sxs-lookup"><span data-stu-id="7cfeb-164">hello request URI looks like hello following</span></span>
   
        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/
   
    <span data-ttu-id="7cfeb-165">tooget hello parametrar, klicka på programmets namn och klicka på instrumentpanelen och visas en sida som hello följande med alla hello 3 parametrar.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-165">tooget hello parameters, click on your application name and click Dashboard and you will see a page like hello following with all hello 3 parameters.</span></span>
   
   * <span data-ttu-id="7cfeb-166">**1** `{subscription-id}`</span><span class="sxs-lookup"><span data-stu-id="7cfeb-166">**1** `{subscription-id}`</span></span>
   * <span data-ttu-id="7cfeb-167">**2** `{app-collection}`</span><span class="sxs-lookup"><span data-stu-id="7cfeb-167">**2** `{app-collection}`</span></span>
   * <span data-ttu-id="7cfeb-168">**3** `{app-resource-name}`</span><span class="sxs-lookup"><span data-stu-id="7cfeb-168">**3** `{app-resource-name}`</span></span>
   * <span data-ttu-id="7cfeb-169">**4** din resursgruppens namn kommer toobe **MobileEngagement** om du har skapat en ny.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-169">**4** Your Resource Group name is going toobe **MobileEngagement** unless you created a new one.</span></span> 
     
     ![Mobile Engagement API URI-parametrar][2]

> [!NOTE]
> <br/>
> 
> 1. <span data-ttu-id="7cfeb-171">Ignorera hello API rot-adress som det var för hello tidigare API: er.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-171">Ignore hello API Root Address as this was for hello previous APIs.</span></span><br/>
> 2. <span data-ttu-id="7cfeb-172">Om du har skapat hello-app med den klassiska Azure-portalen måste toouse hello programresursen namn som skiljer sig från hello programnamn sig själv.</span><span class="sxs-lookup"><span data-stu-id="7cfeb-172">If you created hello app using Azure Classic portal then you need toouse hello Application Resource name which is different than hello Application name itself.</span></span> <span data-ttu-id="7cfeb-173">Om du har skapat hello app i hello Azure Portal bör du använda hello Appnamn sig själv (det finns ingen skillnad mellan programmets resursnamn och programnamn för appar som har skapats i hello nya portal).</span><span class="sxs-lookup"><span data-stu-id="7cfeb-173">If you created hello app in hello Azure Portal then you should use hello App Name itself (there is no differentiation between Application Resource Name and App Name for apps created in hello new portal).</span></span>  
> 
> 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/ad-app-creation.png



