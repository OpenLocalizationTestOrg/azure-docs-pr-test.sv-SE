---
title: "aaaGet igång med Data Lake Analytics med hjälp av REST API | Microsoft Docs"
description: "Använd WebHDFS REST API: er tooperform åtgärder på Data Lake Analytics"
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 5e133d92-baaa-44c9-890c-ab2d85c91122
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/03/2017
ms.author: jgao
ms.openlocfilehash: a0b13d521821fd2d74716cc52485585feb7c51b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-rest-apis"></a><span data-ttu-id="2fab3-103">Kom igång med Azure Data Lake Analytics med hjälp av REST API:er | Azure</span><span class="sxs-lookup"><span data-stu-id="2fab3-103">Get started with Azure Data Lake Analytics using REST APIs</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

<span data-ttu-id="2fab3-104">Lär dig hur toouse WebHDFS REST API: er och Data Lake Analytics REST API: er toomanage Data Lake Analytics-konton, jobb och katalogen.</span><span class="sxs-lookup"><span data-stu-id="2fab3-104">Learn how toouse WebHDFS REST APIs and Data Lake Analytics REST APIs toomanage Data Lake Analytics accounts, jobs, and catalog.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="2fab3-105">Krav</span><span class="sxs-lookup"><span data-stu-id="2fab3-105">Prerequisites</span></span>
* <span data-ttu-id="2fab3-106">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="2fab3-106">**An Azure subscription**.</span></span> <span data-ttu-id="2fab3-107">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2fab3-107">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="2fab3-108">**Skapa ett program i Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="2fab3-108">**Create an Azure Active Directory Application**.</span></span> <span data-ttu-id="2fab3-109">Du kan använda hello Azure AD application tooauthenticate hello Data Lake Analytics-program med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2fab3-109">You use hello Azure AD application tooauthenticate hello Data Lake Analytics application with Azure AD.</span></span> <span data-ttu-id="2fab3-110">Det finns olika sätt tooauthenticate med Azure AD, som är **slutanvändarens autentisering** eller **tjänst-till-tjänst autentisering**.</span><span class="sxs-lookup"><span data-stu-id="2fab3-110">There are different approaches tooauthenticate with Azure AD, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="2fab3-111">Anvisningar och mer information om hur tooauthenticate, se [autentisera med Data Lake Analytics med hjälp av Azure Active Directory](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="2fab3-111">For instructions and more information on how tooauthenticate, see [Authenticate with Data Lake Analytics using Azure Active Directory](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span></span>
* <span data-ttu-id="2fab3-112">[cURL](http://curl.haxx.se/).</span><span class="sxs-lookup"><span data-stu-id="2fab3-112">[cURL](http://curl.haxx.se/).</span></span> <span data-ttu-id="2fab3-113">Den här artikeln använder cURL toodemonstrate hur toomake REST API-anrop mot ett Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="2fab3-113">This article uses cURL toodemonstrate how toomake REST API calls against a Data Lake Analytics account.</span></span>

## <a name="authenticate-with-azure-active-directory"></a><span data-ttu-id="2fab3-114">Autentisera med hjälp av Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2fab3-114">Authenticate with Azure Active Directory</span></span>
<span data-ttu-id="2fab3-115">Det finns två metoder för hur du autentiserar med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2fab3-115">There are two methods for authenticating with Azure Active Directory.</span></span>

### <a name="end-user-authentication-interactive"></a><span data-ttu-id="2fab3-116">Slutanvändarautentisering (interaktiv)</span><span class="sxs-lookup"><span data-stu-id="2fab3-116">End-user authentication (interactive)</span></span>
<span data-ttu-id="2fab3-117">Med den här metoden uppmanar programmet hello användaren toolog i och alla hello åtgärder utförs i hello hello användarens kontext.</span><span class="sxs-lookup"><span data-stu-id="2fab3-117">Using this method, application prompts hello user toolog in and all hello operations are performed in hello context of hello user.</span></span> 

<span data-ttu-id="2fab3-118">Följ de här stegen för interaktiv autentisering:</span><span class="sxs-lookup"><span data-stu-id="2fab3-118">Follow these steps for interactive authentication:</span></span>

1. <span data-ttu-id="2fab3-119">Omdirigera hello användaren toohello följande URL: en via ditt program:</span><span class="sxs-lookup"><span data-stu-id="2fab3-119">Through your application, redirect hello user toohello following URL:</span></span>
   
        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>
   
   > [!NOTE]
   > <span data-ttu-id="2fab3-120">\<OMDIRIGERINGS-URI > måste toobe kodad för användning i en URL.</span><span class="sxs-lookup"><span data-stu-id="2fab3-120">\<REDIRECT-URI> needs toobe encoded for use in a URL.</span></span> <span data-ttu-id="2fab3-121">Alltså ska `https%3A%2F%2Flocalhost` användas för https://localhost)</span><span class="sxs-lookup"><span data-stu-id="2fab3-121">So, for https://localhost, use `https%3A%2F%2Flocalhost`)</span></span>
   > 
   > 
   
    <span data-ttu-id="2fab3-122">För hello syftet med den här självstudiekursen, du ersätta hello platshållarvärdena i URL: en för hello ovan och klistra in den i webbläsarens adressfält.</span><span class="sxs-lookup"><span data-stu-id="2fab3-122">For hello purpose of this tutorial, you can replace hello placeholder values in hello URL above and paste it in a web browser's address bar.</span></span> <span data-ttu-id="2fab3-123">Du kommer att omdirigerade tooauthenticate med Azure-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="2fab3-123">You will be redirected tooauthenticate using your Azure login.</span></span> <span data-ttu-id="2fab3-124">När du har loggat in visas svaret hello i hello webbläsarens adressfält.</span><span class="sxs-lookup"><span data-stu-id="2fab3-124">Once you succesfully log in, hello response is displayed in hello browser's address bar.</span></span> <span data-ttu-id="2fab3-125">hello svaret ska ha hello följande format:</span><span class="sxs-lookup"><span data-stu-id="2fab3-125">hello response will be in hello following format:</span></span>
   
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>
2. <span data-ttu-id="2fab3-126">Avbilda hello Auktoriseringskoden från hello svar.</span><span class="sxs-lookup"><span data-stu-id="2fab3-126">Capture hello authorization code from hello response.</span></span> <span data-ttu-id="2fab3-127">För den här självstudiekursen kan du kopiera hello Auktoriseringskoden från adressfältet i hello av hello webbläsare och skicka den i hello POST-begäran toohello tokenslutpunkten, enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="2fab3-127">For this tutorial, you can copy hello authorization code from hello address bar of hello web browser and pass it in hello POST request toohello token endpoint, as shown below:</span></span>
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>
   
   > [!NOTE]
   > <span data-ttu-id="2fab3-128">I det här fallet hello \<OMDIRIGERINGS-URI > måste inte vara kodad.</span><span class="sxs-lookup"><span data-stu-id="2fab3-128">In this case, hello \<REDIRECT-URI> need not be encoded.</span></span>
   > 
   > 
3. <span data-ttu-id="2fab3-129">hello svaret är ett JSON-objekt som innehåller en åtkomsttoken (t.ex. `"access_token": "<ACCESS_TOKEN>"`) och en uppdateringstoken (t.ex. `"refresh_token": "<REFRESH_TOKEN>"`).</span><span class="sxs-lookup"><span data-stu-id="2fab3-129">hello response is a JSON object that contains an access token (e.g., `"access_token": "<ACCESS_TOKEN>"`) and a refresh token (e.g., `"refresh_token": "<REFRESH_TOKEN>"`).</span></span> <span data-ttu-id="2fab3-130">Programmet använder hello åtkomsttoken vid åtkomst till Azure Data Lake Store och hello uppdatera token tooget en annan åtkomsttoken när en åtkomst-token upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="2fab3-130">Your application uses hello access token when accessing Azure Data Lake Store and hello refresh token tooget another access token when an access token expires.</span></span>
   
        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before":    "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}
4. <span data-ttu-id="2fab3-131">När hello åtkomst-token upphör att gälla, kan du begära en ny åtkomsttoken med hjälp av hello uppdateringstoken som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="2fab3-131">When hello access token expires, you can request a new access token using hello refresh token, as shown below:</span></span>
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
             -F grant_type=refresh_token \
             -F resource=https://management.core.windows.net/ \
             -F client_id=<CLIENT-ID> \
             -F refresh_token=<REFRESH-TOKEN>

<span data-ttu-id="2fab3-132">Mer information om interaktiv användarautentisering finns i [Flöde beviljat med auktoriseringskod](https://msdn.microsoft.com/library/azure/dn645542.aspx).</span><span class="sxs-lookup"><span data-stu-id="2fab3-132">For more information on interactive user authentication, see [Authorization code grant flow](https://msdn.microsoft.com/library/azure/dn645542.aspx).</span></span>

### <a name="service-to-service-authentication-non-interactive"></a><span data-ttu-id="2fab3-133">Tjänst-till-tjänst-autentisering (icke-interaktivt)</span><span class="sxs-lookup"><span data-stu-id="2fab3-133">Service-to-service authentication (non-interactive)</span></span>
<span data-ttu-id="2fab3-134">Programmet tillhandahåller använder den här metoden egna autentiseringsuppgifter tooperform hello åtgärder.</span><span class="sxs-lookup"><span data-stu-id="2fab3-134">Using this method, application provides its own credentials tooperform hello operations.</span></span> <span data-ttu-id="2fab3-135">För att göra detta måste du skicka en POST-begäran som hello som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="2fab3-135">For this, you must issue a POST request like hello one shown below:</span></span> 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

<span data-ttu-id="2fab3-136">hello utdata för begäran innehåller en autentiseringstoken (betecknas med `access-token` i hello utdata nedan) som du sedan skickar med REST API-anrop.</span><span class="sxs-lookup"><span data-stu-id="2fab3-136">hello output of this request will include an authorization token (denoted by `access-token` in hello output below) that you will subsequently pass with your REST API calls.</span></span> <span data-ttu-id="2fab3-137">Spara den här autentiseringstoken i en textfil; du behöver den senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="2fab3-137">Save this authentication token in a text file; you will need this later in this article.</span></span>

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

<span data-ttu-id="2fab3-138">Den här artikeln använder hello **icke-interaktiv** metod.</span><span class="sxs-lookup"><span data-stu-id="2fab3-138">This article uses hello **non-interactive** approach.</span></span> <span data-ttu-id="2fab3-139">Mer information om icke-interaktivt (service to service anrop) finns [tjänsten tooservice anrop med autentiseringsuppgifter](https://msdn.microsoft.com/library/azure/dn645543.aspx).</span><span class="sxs-lookup"><span data-stu-id="2fab3-139">For more information on non-interactive (service-to-service calls), see [Service tooservice calls using credentials](https://msdn.microsoft.com/library/azure/dn645543.aspx).</span></span>

## <a name="create-a-data-lake-analytics-account"></a><span data-ttu-id="2fab3-140">Skapa ett Data Lake Analytics-konto</span><span class="sxs-lookup"><span data-stu-id="2fab3-140">Create a Data Lake Analytics account</span></span>
<span data-ttu-id="2fab3-141">Du måste skapa en Azure-resursgrupp och ett Data Lake Store-konto innan du kan skapa ett Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="2fab3-141">You must create an Azure Resource group, and a Data Lake Store account before you can create a Data Lake Analytics account.</span></span>  <span data-ttu-id="2fab3-142">Se [Skapa ett Data Lake Store-konto](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account).</span><span class="sxs-lookup"><span data-stu-id="2fab3-142">See [Create a Data Lake Store account](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account).</span></span>

<span data-ttu-id="2fab3-143">Hej följande Curl-kommando visar hur toocreate ett konto:</span><span class="sxs-lookup"><span data-stu-id="2fab3-143">hello following Curl command shows how toocreate an account:</span></span>

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<NewAzureDataLakeAnalyticsAccountName>?api-version=2016-11-01 -d@"C:\tutorials\adla\CreateDataLakeAnalyticsAccountRequest.json"

<span data-ttu-id="2fab3-144">Ersätt \< `REDACTED` \> med hello autentiseringstoken \< `AzureSubscriptionID` \> med ditt prenumerations-ID \< `AzureResourceGroupName` \> med en befintlig Azure-resurs Gruppnamn och \< `NewAzureDataLakeAnalyticsAccountName` \> med ett nytt namn för Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="2fab3-144">Replace \<`REDACTED`\> with hello authorization token, \<`AzureSubscriptionID`\> with your subscription ID, \<`AzureResourceGroupName`\> with an existing Azure Resource Group name, and \<`NewAzureDataLakeAnalyticsAccountName`\> with a new Data Lake Analytics Account name.</span></span> <span data-ttu-id="2fab3-145">Hej begärannyttolast för det här kommandot finns i hello **CreateDatalakeAnalyticsAccountRequest.json** fil som har angetts för hello `-d` parametern ovan.</span><span class="sxs-lookup"><span data-stu-id="2fab3-145">hello request payload for this command is contained in hello **CreateDatalakeAnalyticsAccountRequest.json** file that is provided for hello `-d` parameter above.</span></span> <span data-ttu-id="2fab3-146">hello innehållet i input.json-filen för hello likna hello följande:</span><span class="sxs-lookup"><span data-stu-id="2fab3-146">hello contents of hello input.json file resemble hello following:</span></span>

    {  
        "location": "East US 2",  
        "name": "myadla1004",  
        "tags": {},  
        "properties": {  
            "defaultDataLakeStoreAccount": "my1004store",  
            "dataLakeStoreAccounts": [  
                {  
                    "name": "my1004store"  
                }     
            ]
        }  
    }  


## <a name="list-data-lake-analytics-accounts-in-a-subscription"></a><span data-ttu-id="2fab3-147">Lista över Data Lake Analytics-konton i en prenumeration</span><span class="sxs-lookup"><span data-stu-id="2fab3-147">List Data Lake Analytics accounts in a subscription</span></span>
<span data-ttu-id="2fab3-148">hello följande Curl-kommando visar hur toolist konton i en prenumeration:</span><span class="sxs-lookup"><span data-stu-id="2fab3-148">hello following Curl command shows how toolist accounts in a subscription:</span></span>

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/providers/Microsoft.DataLakeAnalytics/Accounts?api-version=2016-11-01

<span data-ttu-id="2fab3-149">Ersätt \< `REDACTED` \> med hello autentiseringstoken \< `AzureSubscriptionID` \> med ditt prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="2fab3-149">Replace \<`REDACTED`\> with hello authorization token, \<`AzureSubscriptionID`\> with your subscription ID.</span></span> <span data-ttu-id="2fab3-150">hello utdata liknar:</span><span class="sxs-lookup"><span data-stu-id="2fab3-150">hello output is similar to:</span></span>

    {
        "value": [
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla0831.azuredatalakeanalytics.net",
                "accountId": "21e74660-0941-4880-ae72-b143c2615ea9",
                "creationTime": "2016-09-01T12:49:12.7451428Z",
                "lastModifiedTime": "2016-09-01T12:49:12.7451428Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla0831rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla0831",
            "name": "myadla0831",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            },
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla1004.azuredatalakeanalytics.net",
                "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
                "creationTime": "2016-10-04T20:46:42.287147Z",
                "lastModifiedTime": "2016-10-04T20:46:42.287147Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
            "name": "myadla1004",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            }
        ]
    }

## <a name="get-information-about-a-data-lake-analytics-account"></a><span data-ttu-id="2fab3-151">Hämta information om ett Data Lake Analytics-konto</span><span class="sxs-lookup"><span data-stu-id="2fab3-151">Get information about a Data Lake Analytics account</span></span>
<span data-ttu-id="2fab3-152">Hej följande Curl-kommando visar hur tooget en kontoinformation:</span><span class="sxs-lookup"><span data-stu-id="2fab3-152">hello following Curl command shows how tooget an account information:</span></span>

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>?api-version=2015-11-01

<span data-ttu-id="2fab3-153">Ersätt \< `REDACTED` \> med hello autentiseringstoken \< `AzureSubscriptionID` \> med ditt prenumerations-ID \< `AzureResourceGroupName` \> med en befintlig Azure-resurs Gruppnamn och \< `DataLakeAnalyticsAccountName` \> med hello namnet på ett befintligt Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="2fab3-153">Replace \<`REDACTED`\> with hello authorization token, \<`AzureSubscriptionID`\> with your subscription ID, \<`AzureResourceGroupName`\> with an existing Azure Resource Group name, and \<`DataLakeAnalyticsAccountName`\> with hello name of an existing Data Lake Analytics Account.</span></span> <span data-ttu-id="2fab3-154">hello utdata liknar:</span><span class="sxs-lookup"><span data-stu-id="2fab3-154">hello output is similar to:</span></span>

    {
        "properties": {
            "defaultDataLakeStoreAccount": "my1004store",
            "dataLakeStoreAccounts": [
            {
                "properties": {
                "suffix": "azuredatalakestore.net"
                },
                "name": "my1004store"
            }
            ],
            "provisioningState": "Creating",
            "state": null,
            "endpoint": null,
            "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
            "creationTime": null,
            "lastModifiedTime": null
        },
        "location": "East US 2",
        "tags": {},
        "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
        "name": "myadla1004",
        "type": "Microsoft.DataLakeAnalytics/accounts"
    }

## <a name="list-data-lake-stores-of-a-data-lake-analytics-account"></a><span data-ttu-id="2fab3-155">Ange Data Lake Stores för ett Data Lake Analytics-konto i en lista</span><span class="sxs-lookup"><span data-stu-id="2fab3-155">List Data Lake Stores of a Data Lake Analytics account</span></span>
<span data-ttu-id="2fab3-156">hello följande Curl-kommando visar hur toolist Data Lake lagrar för ett konto:</span><span class="sxs-lookup"><span data-stu-id="2fab3-156">hello following Curl command shows how toolist Data Lake Stores of an account:</span></span>

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>/DataLakeStoreAccounts/?api-version=2016-11-01

<span data-ttu-id="2fab3-157">Ersätt \< `REDACTED` \> med hello autentiseringstoken \< `AzureSubscriptionID` \> med ditt prenumerations-ID \< `AzureResourceGroupName` \> med en befintlig Azure-resurs Gruppnamn och \< `DataLakeAnalyticsAccountName` \> med hello namnet på ett befintligt Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="2fab3-157">Replace \<`REDACTED`\> with hello authorization token, \<`AzureSubscriptionID`\> with your subscription ID, \<`AzureResourceGroupName`\> with an existing Azure Resource Group name, and \<`DataLakeAnalyticsAccountName`\> with hello name of an existing Data Lake Analytics Account.</span></span> <span data-ttu-id="2fab3-158">hello utdata liknar:</span><span class="sxs-lookup"><span data-stu-id="2fab3-158">hello output is similar to:</span></span>

    {
        "value": [
            {
            "properties": {
                "suffix": "azuredatalakestore.net"
            },
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004/dataLakeStoreAccounts/my1004store",
            "name": "my1004store",
            "type": "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts"
            }
        ]
    }

## <a name="submit-u-sql-jobs"></a><span data-ttu-id="2fab3-159">Skicka U-SQL-jobb</span><span class="sxs-lookup"><span data-stu-id="2fab3-159">Submit U-SQL jobs</span></span>
<span data-ttu-id="2fab3-160">Hej följande Curl-kommando visar hur toosubmit U-SQL-jobb:</span><span class="sxs-lookup"><span data-stu-id="2fab3-160">hello following Curl command shows how toosubmit a U-SQL job:</span></span>

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs/<NewGUID>?api-version=2016-03-20-preview -d@"C:\tutorials\adla\SubmitADLAJob.json"

<span data-ttu-id="2fab3-161">Ersätt \< `REDACTED` \> med hello autentiseringstoken \< `DataLakeAnalyticsAccountName` \> med hello namnet på ett befintligt Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="2fab3-161">Replace \<`REDACTED`\> with hello authorization token, \<`DataLakeAnalyticsAccountName`\> with hello name of an existing Data Lake Analytics Account.</span></span> <span data-ttu-id="2fab3-162">Hej begärannyttolast för det här kommandot finns i hello **SubmitADLAJob.json** fil som har angetts för hello `-d` parametern ovan.</span><span class="sxs-lookup"><span data-stu-id="2fab3-162">hello request payload for this command is contained in hello **SubmitADLAJob.json** file that is provided for hello `-d` parameter above.</span></span> <span data-ttu-id="2fab3-163">hello innehållet i input.json-filen för hello likna hello följande:</span><span class="sxs-lookup"><span data-stu-id="2fab3-163">hello contents of hello input.json file resemble hello following:</span></span>

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "properties": {
            "type": "USql",
            "script": "@searchlog =\n    EXTRACT UserId          int,\n            Start           DateTime,\n            Region          string,\n            Query          
        string,\n            Duration        int?,\n            Urls            string,\n            ClickedUrls     string\n    FROM \"/Samples/Data/SearchLog.tsv\"\n    US
        ING Extractors.Tsv();\n\nOUTPUT @searchlog   \n    too\"/Output/SearchLog-from-Data-Lake.csv\"\nUSING Outputters.Csv();"
        }
    }

<span data-ttu-id="2fab3-164">hello utdata liknar:</span><span class="sxs-lookup"><span data-stu-id="2fab3-164">hello output is similar to:</span></span>

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "myadl@SPI",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "2016-10-05T13:54:59.9871859+00:00",
        "state": "Compiling",
        "result": "Succeeded",
        "stateAuditRecords": [
            {
            "newState": "New",
            "timeStamp": "2016-10-05T13:54:59.9871859+00:00",
            "details": "userName:myadl@SPI;submitMachine:N/A"
            }
        ],
        "properties": {
            "owner": "myadl@SPI",
            "resources": [],
            "runtimeVersion": "default",
            "rootProcessNodeId": "00000000-0000-0000-0000-000000000000",
            "algebraFilePath": "adl://myadls0831.azuredatalakestore.net/system/jobservice/jobs/Usql/2016/10/05/13/54/8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a/algebra.xml",
            "compileMode": "Semantic",
            "errorSource": "Unknown",
            "totalCompilationTime": "PT0S",
            "totalPausedTime": "PT0S",
            "totalQueuedTime": "PT0S",
            "totalRunningTime": "PT0S",
            "type": "USql"
        }
    }


## <a name="list-u-sql-jobs"></a><span data-ttu-id="2fab3-165">Lista U-SQL-jobb</span><span class="sxs-lookup"><span data-stu-id="2fab3-165">List U-SQL jobs</span></span>
<span data-ttu-id="2fab3-166">Hej följande Curl-kommando visar hur toolist U-SQL-jobb:</span><span class="sxs-lookup"><span data-stu-id="2fab3-166">hello following Curl command shows how toolist U-SQL jobs:</span></span>

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs?api-version=2016-11-01 

<span data-ttu-id="2fab3-167">Ersätt \< `REDACTED` \> med hello autentiseringstoken och \< `DataLakeAnalyticsAccountName` \> med hello namnet på ett befintligt Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="2fab3-167">Replace \<`REDACTED`\> with hello authorization token, and \<`DataLakeAnalyticsAccountName`\> with hello name of an existing Data Lake Analytics Account.</span></span> 

<span data-ttu-id="2fab3-168">hello utdata liknar:</span><span class="sxs-lookup"><span data-stu-id="2fab3-168">hello output is similar to:</span></span>

    {
    "value": [
        {
        "jobId": "65cf1691-9dbe-43cd-90ed-1cafbfb406fb",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someone@microsoft.com",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:46:53 GMT",
        "startTime": "Wed, 05 Oct 2016 13:47:33 GMT",
        "endTime": "Wed, 05 Oct 2016 13:48:07 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        },
        {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someoneadl@SPI",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:54:59 GMT",
        "startTime": "Wed, 05 Oct 2016 13:55:43 GMT",
        "endTime": "Wed, 05 Oct 2016 13:56:11 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        }
    ],
    "nextLink": null,
    "count": null
    }


## <a name="get-catalog-items"></a><span data-ttu-id="2fab3-169">Hämta katalogobjekt</span><span class="sxs-lookup"><span data-stu-id="2fab3-169">Get catalog items</span></span>
<span data-ttu-id="2fab3-170">hello följande Curl-kommando visar hur tooget hello databaser från hello katalog:</span><span class="sxs-lookup"><span data-stu-id="2fab3-170">hello following Curl command shows how tooget hello databases from hello catalog:</span></span>

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/catalog/usql/databases?api-version=2016-11-01

<span data-ttu-id="2fab3-171">hello utdata liknar:</span><span class="sxs-lookup"><span data-stu-id="2fab3-171">hello output is similar to:</span></span>

    {
    "@odata.context":"https://myadla0831.azuredatalakeanalytics.net/sqlip/$metadata#databases","value":[
        {
        "computeAccountName":"myadla0831","databaseName":"mytest","version":"f6956327-90b8-4648-ad8b-de3ff09274ea"
        },{
        "computeAccountName":"myadla0831","databaseName":"master","version":"e8bca908-cc73-41a3-9564-e9bcfaa21f4e"
        }
    ]
    }

## <a name="see-also"></a><span data-ttu-id="2fab3-172">Se även</span><span class="sxs-lookup"><span data-stu-id="2fab3-172">See also</span></span>
* <span data-ttu-id="2fab3-173">toosee en mer komplex fråga, se [analysera webbplatsloggar med hjälp av Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="2fab3-173">toosee a more complex query, see [Analyze Website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="2fab3-174">tooget utveckla U-SQL-program, se [utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="2fab3-174">tooget started developing U-SQL applications, see [Develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
* <span data-ttu-id="2fab3-175">toolearn U-SQL finns [Kom igång med Azure Data Lake Analytics U-SQL-språket](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="2fab3-175">toolearn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
* <span data-ttu-id="2fab3-176">Information om hanteringsuppgifter finns i [Hantera Azure Data Lake Analytics med hjälp av Azure Portal](data-lake-analytics-manage-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="2fab3-176">For management tasks, see [Manage Azure Data Lake Analytics using Azure portal](data-lake-analytics-manage-use-portal.md).</span></span>
* <span data-ttu-id="2fab3-177">tooget en översikt över Data Lake Analytics finns [översikt över Azure Data Lake Analytics](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2fab3-177">tooget an overview of Data Lake Analytics, see [Azure Data Lake Analytics overview](data-lake-analytics-overview.md).</span></span>
* <span data-ttu-id="2fab3-178">toosee hello samma självstudier med andra verktyg klickar du på hello flikväljarna överst hello hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="2fab3-178">toosee hello same tutorial using other tools, click hello tab selectors on hello top of hello page.</span></span>

