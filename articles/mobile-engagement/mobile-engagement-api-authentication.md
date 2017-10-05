---
title: 'Autentisera med Mobile Engagement REST API: er'
description: 'Beskriver hur du autentiserar med Azure Mobile Engagement REST API: er'
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
ms.openlocfilehash: b05181d9252c0a804648e01b4058019278ae5abe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="authenticate-with-mobile-engagement-rest-apis"></a><span data-ttu-id="af9d5-103">Autentisera med Mobile Engagement REST API: er</span><span class="sxs-lookup"><span data-stu-id="af9d5-103">Authenticate with Mobile Engagement REST APIs</span></span>
## <a name="overview"></a><span data-ttu-id="af9d5-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="af9d5-104">Overview</span></span>
<span data-ttu-id="af9d5-105">Det här dokumentet beskriver hur du skaffa en giltig AAD-Oauth-token för autentisering med Mobile Engagement REST-API: er.</span><span class="sxs-lookup"><span data-stu-id="af9d5-105">This document describes how to get a valid AAD Oauth token to authenticate with the Mobile Engagement REST APIs.</span></span> 

<span data-ttu-id="af9d5-106">Det förutsätts att du har en giltig Azure-prenumeration och du har skapat en Mobile Engagement-app med en av våra [Developer självstudier](mobile-engagement-windows-store-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="af9d5-106">It is assumed that you have a valid Azure subscription and you have created a Mobile Engagement app using one of our [Developer Tutorials](mobile-engagement-windows-store-dotnet-get-started.md).</span></span>

## <a name="authentication"></a><span data-ttu-id="af9d5-107">Autentisering</span><span class="sxs-lookup"><span data-stu-id="af9d5-107">Authentication</span></span>
<span data-ttu-id="af9d5-108">Microsoft Azure Active Directory baserat OAuth-token används för autentisering.</span><span class="sxs-lookup"><span data-stu-id="af9d5-108">A Microsoft Azure Active Directory based OAuth token is used for authentication.</span></span> 

<span data-ttu-id="af9d5-109">För att en API autentiseringsbegäran måste ett authorization-huvud läggas till varje begäran som har följande format:</span><span class="sxs-lookup"><span data-stu-id="af9d5-109">In order to authentication an API request, an authorization header must be added to every request which is of the following form:</span></span>

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

> [!NOTE]
> <span data-ttu-id="af9d5-110">Azure Active Directory-token upphör gälla inom en timme.</span><span class="sxs-lookup"><span data-stu-id="af9d5-110">Azure Active Directory tokens expire in 1 hour.</span></span>
> 
> 

<span data-ttu-id="af9d5-111">Det finns flera sätt att hämta en token.</span><span class="sxs-lookup"><span data-stu-id="af9d5-111">There are several ways to get a token.</span></span> <span data-ttu-id="af9d5-112">Eftersom API: erna kallas vanligtvis från en molnbaserad tjänst, som du vill använda en API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="af9d5-112">Since the APIs are generally called from a cloud service, you want to use an API key.</span></span> <span data-ttu-id="af9d5-113">En API-nyckel i Azure-terminologi kallas Service principal lösenord.</span><span class="sxs-lookup"><span data-stu-id="af9d5-113">An API key in Azure terminology is called a Service principal password.</span></span> <span data-ttu-id="af9d5-114">Följande procedur beskriver ett sätt att ställa in manuellt.</span><span class="sxs-lookup"><span data-stu-id="af9d5-114">The following procedure describes one way to setting it up manually.</span></span>

### <a name="one-time-setup-using-script"></a><span data-ttu-id="af9d5-115">Enstaka installationen (med hjälp av skript)</span><span class="sxs-lookup"><span data-stu-id="af9d5-115">One-time setup (using script)</span></span>
<span data-ttu-id="af9d5-116">Du bör följa anvisningarna nedan för att utföra installationen med hjälp av ett PowerShell-skript som tar Minimitiden för installationen, men använder mest tillåtna standardvärdena uppsättning.</span><span class="sxs-lookup"><span data-stu-id="af9d5-116">You should follow the set of instructions below to perform the setup using a PowerShell script which takes the minimum time for setup but uses the most permissible defaults.</span></span> <span data-ttu-id="af9d5-117">Alternativt kan du också följa instruktionerna i den [manuell installation](mobile-engagement-api-authentication-manual.md) för att göra detta från Azure-portalen direkt och vill ha bättre konfiguration.</span><span class="sxs-lookup"><span data-stu-id="af9d5-117">Optionally, you can also follow the instructions in the [manual setup](mobile-engagement-api-authentication-manual.md) for doing this from the Azure portal directly and do finer configuration.</span></span> 

1. <span data-ttu-id="af9d5-118">Hämta den senaste versionen av Azure PowerShell från [här](http://aka.ms/webpi-azps).</span><span class="sxs-lookup"><span data-stu-id="af9d5-118">Get the latest version of Azure PowerShell from [here](http://aka.ms/webpi-azps).</span></span> <span data-ttu-id="af9d5-119">Mer information om anvisningarna kan se detta [länk](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="af9d5-119">For more information on the download instructions, you can see this [link](/powershell/azure/overview).</span></span>  
2. <span data-ttu-id="af9d5-120">När Azure PowerShell har installerats kan använda följande kommandon för att säkerställa att du har den **Azure-modulen** installerad:</span><span class="sxs-lookup"><span data-stu-id="af9d5-120">Once Azure PowerShell is installed, use the following commands to ensure that you have the **Azure module** installed:</span></span>
   
    <span data-ttu-id="af9d5-121">a.</span><span class="sxs-lookup"><span data-stu-id="af9d5-121">a.</span></span> <span data-ttu-id="af9d5-122">Kontrollera att Azure PowerShell-modulen finns i listan över tillgängliga moduler.</span><span class="sxs-lookup"><span data-stu-id="af9d5-122">Make sure the Azure PowerShell module is available in the list of available modules.</span></span> 
   
        Get-Module –ListAvailable 
   
    ![Tillgängliga Azure moduler][1]
   
    <span data-ttu-id="af9d5-124">b.</span><span class="sxs-lookup"><span data-stu-id="af9d5-124">b.</span></span> <span data-ttu-id="af9d5-125">Om du inte hittar Azure PowerShell-modulen i listan ovan måste du köra följande:</span><span class="sxs-lookup"><span data-stu-id="af9d5-125">If you do not find the Azure PowerShell module in the above list then you need to run the following:</span></span>
   
        Import-Module Azure 
3. <span data-ttu-id="af9d5-126">Logga in till Azure Resource Manager från PowerShell genom att köra följande kommando och ditt användarnamn och lösenord för kontot:</span><span class="sxs-lookup"><span data-stu-id="af9d5-126">Login to the Azure Resource Manager from PowerShell by running the following command and providing your user name and password for your Azure account:</span></span> 
   
        Login-AzureRmAccount
4. <span data-ttu-id="af9d5-127">Om du har flera prenumerationer bör du köra följande:</span><span class="sxs-lookup"><span data-stu-id="af9d5-127">If you have multiple subscriptions then you should run the following:</span></span>
   
    <span data-ttu-id="af9d5-128">a.</span><span class="sxs-lookup"><span data-stu-id="af9d5-128">a.</span></span> <span data-ttu-id="af9d5-129">Hämta en lista över alla prenumerationer och kopiera prenumerations-ID för den prenumeration som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="af9d5-129">Get a list of all your subscriptions and copy the SubscriptionId of the subscription you want to use.</span></span> <span data-ttu-id="af9d5-130">Kontrollera att den här prenumerationen är samma som har Mobile Engagement-App som du kommer att interagera med hjälp av API: erna.</span><span class="sxs-lookup"><span data-stu-id="af9d5-130">Make sure this subscription is the same one which has the Mobile Engagement App which you are going to interact with using the APIs.</span></span> 
   
        Get-AzureRmSubscription
   
    <span data-ttu-id="af9d5-131">b.</span><span class="sxs-lookup"><span data-stu-id="af9d5-131">b.</span></span> <span data-ttu-id="af9d5-132">Kör följande kommando för att tillhandahålla SubscriptionId att konfigurera prenumerationen som ska användas.</span><span class="sxs-lookup"><span data-stu-id="af9d5-132">Run the following command providing the SubscriptionId to configure the subscription to be used.</span></span>
   
        Select-AzureRmSubscription –SubscriptionId <subscriptionId>
5. <span data-ttu-id="af9d5-133">Kopiera texten för den [ny AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) skript till din lokala dator och spara den som en PowerShell-cmdlet (t.ex. `APIAuth.ps1`) och kör den `.\APIAuth.ps1`.</span><span class="sxs-lookup"><span data-stu-id="af9d5-133">Copy the text for the [New-AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) script to your local machine and save it as a PowerShell cmdlet (e.g. `APIAuth.ps1`) and execute it `.\APIAuth.ps1`.</span></span> 
6. <span data-ttu-id="af9d5-134">Skriptet blir du ombedd att ange indata för **huvudkontot**.</span><span class="sxs-lookup"><span data-stu-id="af9d5-134">The script will ask you to provide an input for **principalName**.</span></span> <span data-ttu-id="af9d5-135">Ange ett lämpligt namn som du vill använda för att skapa Active Directory-program (t.ex. APIAuth).</span><span class="sxs-lookup"><span data-stu-id="af9d5-135">Provide a suitable name here that you want to use to create your Active Directory application (e.g. APIAuth).</span></span> 
7. <span data-ttu-id="af9d5-136">När skriptet har slutförts visas följande fyra värden som du behöver för att autentisera programmässigt med AD så se till att kopiera dem.</span><span class="sxs-lookup"><span data-stu-id="af9d5-136">After the script completes, it will display the following four values that you will need to authenticate programmatically with AD so make sure to copy them.</span></span> 
   
    <span data-ttu-id="af9d5-137">**TenantId**, **SubscriptionId**, **ApplicationId**, och **hemlighet**.</span><span class="sxs-lookup"><span data-stu-id="af9d5-137">**TenantId**, **SubscriptionId**, **ApplicationId**, and **Secret**.</span></span>
   
    <span data-ttu-id="af9d5-138">Du kommer att använda TenantId som `{TENANT_ID}`, ApplicationId som `{CLIENT_ID}` och hemliga som `{CLIENT_SECRET}`.</span><span class="sxs-lookup"><span data-stu-id="af9d5-138">You will use TenantId as `{TENANT_ID}`, ApplicationId as `{CLIENT_ID}` and Secret as `{CLIENT_SECRET}`.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="af9d5-139">Säkerhetsprinciperna standard kan blockeras från att köra ett PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="af9d5-139">Your default security policy may block you from running a PowerShell scripts.</span></span> <span data-ttu-id="af9d5-140">I så fall, konfigurerar du tillfälligt din körningsprincipen för att tillåta körning av skript med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="af9d5-140">If so, you temporarily configure your execution policy to allow script execution using the following command:</span></span>
   > 
   > <span data-ttu-id="af9d5-141">Set-ExecutionPolicy RemoteSigned</span><span class="sxs-lookup"><span data-stu-id="af9d5-141">Set-ExecutionPolicy RemoteSigned</span></span>
   > 
   > 
8. <span data-ttu-id="af9d5-142">Här är hur uppsättning PS-cmdlets skulle se ut.</span><span class="sxs-lookup"><span data-stu-id="af9d5-142">Here is how the set of PS cmdlets would look like.</span></span> 
   
    ![][3]
9. <span data-ttu-id="af9d5-143">Kontrollera i Azure-hanteringsportalen att ett nytt AD-program har skapats med det namn du angav i skriptet som kallas **Principalnamnet** under **Visa program som företaget äger**.</span><span class="sxs-lookup"><span data-stu-id="af9d5-143">Check in the Azure Management portal that a new AD application was created with the name you provided to the script called **principalName** under **Show Applications my company owns**.</span></span>
   
    ![][4]

#### <a name="steps-to-get-a-valid-token"></a><span data-ttu-id="af9d5-144">Steg för att hämta en giltig token</span><span class="sxs-lookup"><span data-stu-id="af9d5-144">Steps to get a valid token</span></span>
1. <span data-ttu-id="af9d5-145">Anropa API: et med följande parametrar och Ersätt INNEHAVAREN\_, klient-ID\_-ID och klienten\_HEMLIGHET:</span><span class="sxs-lookup"><span data-stu-id="af9d5-145">Call the API with the following parameters and make sure to replace the TENANT\_ID, CLIENT\_ID and CLIENT\_SECRET:</span></span>
   
   * <span data-ttu-id="af9d5-146">**URL-begäran** som *https://login.microsoftonline.com/ {klient\_ID} / oauth2/token*</span><span class="sxs-lookup"><span data-stu-id="af9d5-146">**Request URL** as *https://login.microsoftonline.com/{TENANT\_ID}/oauth2/token*</span></span>
   * <span data-ttu-id="af9d5-147">**HTTP-innhållstyphuvud** som *program/x-www-formuläret-urlencoded*</span><span class="sxs-lookup"><span data-stu-id="af9d5-147">**HTTP Content-Type header** as *application/x-www-form-urlencoded*</span></span>
   * <span data-ttu-id="af9d5-148">**Brödtext i HTTP-begäran** som *bevilja\_typ = klient\_autentiseringsuppgifter & client_id = {klienten\_ID} & client_secret = {klienten\_HEMLIGHET} & resource=https%3A%2F%2Fmanagement.core.windows.net%2F*</span><span class="sxs-lookup"><span data-stu-id="af9d5-148">**HTTP Request Body** as *grant\_type=client\_credentials&client_id={CLIENT\_ID}&client_secret={CLIENT\_SECRET}&resource=https%3A%2F%2Fmanagement.core.windows.net%2F*</span></span>
     
     <span data-ttu-id="af9d5-149">Följande är en exempelbegäran:</span><span class="sxs-lookup"><span data-stu-id="af9d5-149">The following is an example request:</span></span>
     
       <span data-ttu-id="af9d5-150">POST / {TENANT_ID} / oauth2/token HTTP/1.1 värden: login.microsoftonline.com Content-Type: program/x-www-formuläret-urlencoded grant_type = client_credentials & client_id = {CLIENT_ID} & client_secret = {CLIENT_SECRET} & sammanstä spänningskälla = https % 3A % 2F%2Fmanagement.Core.Windows.NET%2F</span><span class="sxs-lookup"><span data-stu-id="af9d5-150">POST /{TENANT_ID}/oauth2/token HTTP/1.1   Host: login.microsoftonline.com   Content-Type: application/x-www-form-urlencoded   grant_type=client_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&reso   urce=https%3A%2F%2Fmanagement.core.windows.net%2F</span></span>
     
     <span data-ttu-id="af9d5-151">Här är ett exempelsvar:</span><span class="sxs-lookup"><span data-stu-id="af9d5-151">Here is an example response:</span></span>
     
       <span data-ttu-id="af9d5-152">HTTP/1.1 200 OK Content-Type: application/json; charset = utf-8 Content-Length: 1234</span><span class="sxs-lookup"><span data-stu-id="af9d5-152">HTTP/1.1 200 OK   Content-Type: application/json; charset=utf-8   Content-Length: 1234</span></span>
     
       <span data-ttu-id="af9d5-153">{”token_type”: ”ägar”, ”expires_in”: ”3599”, ”expires_on”: ”1445395811”, ”not_before” ”: 144 5391911” ”, resurs”: ”https://management.core.windows.net/”, ”access_token”: {ACCESS_TOKEN}}</span><span class="sxs-lookup"><span data-stu-id="af9d5-153">{"token_type":"Bearer","expires_in":"3599","expires_on":"1445395811","not_before":"144   5391911","resource":"https://management.core.windows.net/","access_token":{ACCESS_TOKEN}}</span></span>
     
     <span data-ttu-id="af9d5-154">Det här exemplet ingår URL-kodning av parametrarna efter `resource` värdet är `https://management.core.windows.net/`.</span><span class="sxs-lookup"><span data-stu-id="af9d5-154">This example included URL encoding of the POST parameters, `resource` value is actually `https://management.core.windows.net/`.</span></span> <span data-ttu-id="af9d5-155">Var noga med att även URL koda `{CLIENT_SECRET}` som den kan innehålla specialtecken.</span><span class="sxs-lookup"><span data-stu-id="af9d5-155">Be careful to also URL encode `{CLIENT_SECRET}` as it may contain special characters.</span></span>
     
     > [!NOTE]
     > <span data-ttu-id="af9d5-156">För att testa, kan du använda en HTTP-klientverktyg som [Fiddler](http://www.telerik.com/fiddler) eller [Chrome Postman tillägg](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop)</span><span class="sxs-lookup"><span data-stu-id="af9d5-156">For testing, you can use an HTTP client tool like [Fiddler](http://www.telerik.com/fiddler) or [Chrome Postman extension](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop)</span></span> 
     > 
     > 
2. <span data-ttu-id="af9d5-157">Innehåller nu begärandehuvudet auktorisering i varje API-anrop:</span><span class="sxs-lookup"><span data-stu-id="af9d5-157">Now in every API call, include the authorization request header:</span></span>
   
        Authorization: Bearer {ACCESS_TOKEN}
   
    <span data-ttu-id="af9d5-158">Om du får ett 401 statuskoden Kontrollera svarstexten, den kan informera om token har upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="af9d5-158">If you get a 401 status code returned, check the response body, it might tell you the token is expired.</span></span> <span data-ttu-id="af9d5-159">I så fall får en ny token.</span><span class="sxs-lookup"><span data-stu-id="af9d5-159">In that case, get a new token.</span></span>

## <a name="using-the-apis"></a><span data-ttu-id="af9d5-160">Med hjälp av API: er</span><span class="sxs-lookup"><span data-stu-id="af9d5-160">Using the APIs</span></span>
<span data-ttu-id="af9d5-161">Nu när du har en giltig token är du redo att göra API-anrop.</span><span class="sxs-lookup"><span data-stu-id="af9d5-161">Now that you have a valid token, you are ready to make the API calls.</span></span>

1. <span data-ttu-id="af9d5-162">I varje API-begäran behöver du ange en giltiga token som du hämtade i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="af9d5-162">In each API request, you will need to pass a valid, unexpired token which you obtained in the previous section.</span></span>
2. <span data-ttu-id="af9d5-163">Behöver du ansluter vissa parametrar i URI som identifierar ditt program.</span><span class="sxs-lookup"><span data-stu-id="af9d5-163">You will need to plug in some parameters into the request URI which identifies your application.</span></span> <span data-ttu-id="af9d5-164">Förfrågan URI ser ut ungefär så här</span><span class="sxs-lookup"><span data-stu-id="af9d5-164">The request URI looks like the following</span></span>
   
        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/
   
    <span data-ttu-id="af9d5-165">Hämta parametrar, klicka på programmets namn och klicka på instrumentpanelen och du ser en sida som följande med parametrarna som 3.</span><span class="sxs-lookup"><span data-stu-id="af9d5-165">To get the parameters, click on your application name and click Dashboard and you will see a page like the following with all the 3 parameters.</span></span>
   
   * <span data-ttu-id="af9d5-166">**1** `{subscription-id}`</span><span class="sxs-lookup"><span data-stu-id="af9d5-166">**1** `{subscription-id}`</span></span>
   * <span data-ttu-id="af9d5-167">**2** `{app-collection}`</span><span class="sxs-lookup"><span data-stu-id="af9d5-167">**2** `{app-collection}`</span></span>
   * <span data-ttu-id="af9d5-168">**3** `{app-resource-name}`</span><span class="sxs-lookup"><span data-stu-id="af9d5-168">**3** `{app-resource-name}`</span></span>
   * <span data-ttu-id="af9d5-169">**4** din resursgruppens namn kommer att vara **MobileEngagement** om du har skapat en ny.</span><span class="sxs-lookup"><span data-stu-id="af9d5-169">**4** Your Resource Group name is going to be **MobileEngagement** unless you created a new one.</span></span> 
     
     ![Mobile Engagement API URI-parametrar][2]

> [!NOTE]
> <br/>
> 
> 1. <span data-ttu-id="af9d5-171">Ignorera API rot-adress som det var för tidigare API: er.</span><span class="sxs-lookup"><span data-stu-id="af9d5-171">Ignore the API Root Address as this was for the previous APIs.</span></span><br/>
> 2. <span data-ttu-id="af9d5-172">Om du har skapat programmet med den klassiska Azure-portalen måste du använda programmet resursnamnet som skiljer sig från namnet på programmet sig själv.</span><span class="sxs-lookup"><span data-stu-id="af9d5-172">If you created the app using Azure Classic portal then you need to use the Application Resource name which is different than the Application name itself.</span></span> <span data-ttu-id="af9d5-173">Om du skapade appen i Azure Portal bör du använda namnet på appen sig själv (det finns ingen skillnad mellan programmets resursnamn och programnamn för appar som har skapats i den nya portalen).</span><span class="sxs-lookup"><span data-stu-id="af9d5-173">If you created the app in the Azure Portal then you should use the App Name itself (there is no differentiation between Application Resource Name and App Name for apps created in the new portal).</span></span>  
> 
> 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/ad-app-creation.png



