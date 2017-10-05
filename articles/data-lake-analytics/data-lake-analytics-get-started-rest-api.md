---
title: "Kom igång med Data Lake Analytics med hjälp av REST API| Microsoft Docs"
description: "Använd WebHDFS REST API:er för att utföra åtgärder på Data Lake Analytics"
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
ms.openlocfilehash: 332d7af2539eea8890745005104ac5b0921c2b7f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-rest-apis"></a><span data-ttu-id="6139b-103">Kom igång med Azure Data Lake Analytics med hjälp av REST API:er | Azure</span><span class="sxs-lookup"><span data-stu-id="6139b-103">Get started with Azure Data Lake Analytics using REST APIs</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

<span data-ttu-id="6139b-104">Ta reda på hur du använder WebHDFS REST API: er och Data Lake Analytics REST API: er för att hantera Data Lake Analytics-konton, -jobb och -kataloger.</span><span class="sxs-lookup"><span data-stu-id="6139b-104">Learn how to use WebHDFS REST APIs and Data Lake Analytics REST APIs to manage Data Lake Analytics accounts, jobs, and catalog.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="6139b-105">Krav</span><span class="sxs-lookup"><span data-stu-id="6139b-105">Prerequisites</span></span>
* <span data-ttu-id="6139b-106">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="6139b-106">**An Azure subscription**.</span></span> <span data-ttu-id="6139b-107">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6139b-107">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="6139b-108">**Skapa ett program i Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="6139b-108">**Create an Azure Active Directory Application**.</span></span> <span data-ttu-id="6139b-109">Du kan använda Azure AD-programmet för att autentisera Data Lake Analytics-programmet med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6139b-109">You use the Azure AD application to authenticate the Data Lake Analytics application with Azure AD.</span></span> <span data-ttu-id="6139b-110">Det finns olika sätt att autentisera med Azure AD: **slutanvändarens autentisering** eller **serviceautentisering**.</span><span class="sxs-lookup"><span data-stu-id="6139b-110">There are different approaches to authenticate with Azure AD, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="6139b-111">Instruktioner och mer information om hur du autentiserar finns i [Autentisera med Data Lake Analytics med Azure Active Directory](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="6139b-111">For instructions and more information on how to authenticate, see [Authenticate with Data Lake Analytics using Azure Active Directory](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).</span></span>
* <span data-ttu-id="6139b-112">[cURL](http://curl.haxx.se/).</span><span class="sxs-lookup"><span data-stu-id="6139b-112">[cURL](http://curl.haxx.se/).</span></span> <span data-ttu-id="6139b-113">Den här artikeln använder cURL för att demonstrera hur du gör REST API-anrop mot ett Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="6139b-113">This article uses cURL to demonstrate how to make REST API calls against a Data Lake Analytics account.</span></span>

## <a name="authenticate-with-azure-active-directory"></a><span data-ttu-id="6139b-114">Autentisera med hjälp av Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6139b-114">Authenticate with Azure Active Directory</span></span>
<span data-ttu-id="6139b-115">Det finns två metoder för hur du autentiserar med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6139b-115">There are two methods for authenticating with Azure Active Directory.</span></span>

### <a name="end-user-authentication-interactive"></a><span data-ttu-id="6139b-116">Slutanvändarautentisering (interaktiv)</span><span class="sxs-lookup"><span data-stu-id="6139b-116">End-user authentication (interactive)</span></span>
<span data-ttu-id="6139b-117">När du använder den här metoden blir användaren uppmanad att logga in och alla åtgärder utförs i användarkontexten.</span><span class="sxs-lookup"><span data-stu-id="6139b-117">Using this method, application prompts the user to log in and all the operations are performed in the context of the user.</span></span> 

<span data-ttu-id="6139b-118">Följ de här stegen för interaktiv autentisering:</span><span class="sxs-lookup"><span data-stu-id="6139b-118">Follow these steps for interactive authentication:</span></span>

1. <span data-ttu-id="6139b-119">Omdirigera användaren via ditt program till följande URL:</span><span class="sxs-lookup"><span data-stu-id="6139b-119">Through your application, redirect the user to the following URL:</span></span>
   
        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>
   
   > [!NOTE]
   > <span data-ttu-id="6139b-120">\<REDIRECT-URI> måste kodas för användning i en URL.</span><span class="sxs-lookup"><span data-stu-id="6139b-120">\<REDIRECT-URI> needs to be encoded for use in a URL.</span></span> <span data-ttu-id="6139b-121">Alltså ska `https%3A%2F%2Flocalhost` användas för https://localhost)</span><span class="sxs-lookup"><span data-stu-id="6139b-121">So, for https://localhost, use `https%3A%2F%2Flocalhost`)</span></span>
   > 
   > 
   
    <span data-ttu-id="6139b-122">För självstudierna kan du ersätta platshållarvärdena i URL-adressen ovan och klistra in den i webbläsarens adressfält.</span><span class="sxs-lookup"><span data-stu-id="6139b-122">For the purpose of this tutorial, you can replace the placeholder values in the URL above and paste it in a web browser's address bar.</span></span> <span data-ttu-id="6139b-123">Du omdirigeras för att autentisera med Azure-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="6139b-123">You will be redirected to authenticate using your Azure login.</span></span> <span data-ttu-id="6139b-124">När du har loggat in visas svaret i webbläsarens adressfält.</span><span class="sxs-lookup"><span data-stu-id="6139b-124">Once you succesfully log in, the response is displayed in the browser's address bar.</span></span> <span data-ttu-id="6139b-125">Svaret ska ha följande format:</span><span class="sxs-lookup"><span data-stu-id="6139b-125">The response will be in the following format:</span></span>
   
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>
2. <span data-ttu-id="6139b-126">Avbilda auktoriseringskoden från svaret.</span><span class="sxs-lookup"><span data-stu-id="6139b-126">Capture the authorization code from the response.</span></span> <span data-ttu-id="6139b-127">För de här självstudierna kan du kopiera auktoriseringskoden från adressfältet i webbläsaren och skicka den i POST-begäran till tokenslutpunkten, enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="6139b-127">For this tutorial, you can copy the authorization code from the address bar of the web browser and pass it in the POST request to the token endpoint, as shown below:</span></span>
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>
   
   > [!NOTE]
   > <span data-ttu-id="6139b-128">I det här fallet behöver \<REDIRECT-URI> inte vara kodad.</span><span class="sxs-lookup"><span data-stu-id="6139b-128">In this case, the \<REDIRECT-URI> need not be encoded.</span></span>
   > 
   > 
3. <span data-ttu-id="6139b-129">Svaret är ett JSON-objekt som innehåller en åtkomsttoken (t.ex. `"access_token": "<ACCESS_TOKEN>"`) och en uppdateringstoken (t.ex. `"refresh_token": "<REFRESH_TOKEN>"`).</span><span class="sxs-lookup"><span data-stu-id="6139b-129">The response is a JSON object that contains an access token (e.g., `"access_token": "<ACCESS_TOKEN>"`) and a refresh token (e.g., `"refresh_token": "<REFRESH_TOKEN>"`).</span></span> <span data-ttu-id="6139b-130">Programmet använder åtkomsttoken vid åtkomst till Azure Data Lake Store och uppdateringstoken för att få en annan åtkomsttoken när en åtkomst-token upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="6139b-130">Your application uses the access token when accessing Azure Data Lake Store and the refresh token to get another access token when an access token expires.</span></span>
   
        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before":    "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}
4. <span data-ttu-id="6139b-131">När åtkomsttoken upphört att gälla kan du begära en ny åtkomsttoken med hjälp av den uppdateringstoken som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="6139b-131">When the access token expires, you can request a new access token using the refresh token, as shown below:</span></span>
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
             -F grant_type=refresh_token \
             -F resource=https://management.core.windows.net/ \
             -F client_id=<CLIENT-ID> \
             -F refresh_token=<REFRESH-TOKEN>

<span data-ttu-id="6139b-132">Mer information om interaktiv användarautentisering finns i [Flöde beviljat med auktoriseringskod](https://msdn.microsoft.com/library/azure/dn645542.aspx).</span><span class="sxs-lookup"><span data-stu-id="6139b-132">For more information on interactive user authentication, see [Authorization code grant flow](https://msdn.microsoft.com/library/azure/dn645542.aspx).</span></span>

### <a name="service-to-service-authentication-non-interactive"></a><span data-ttu-id="6139b-133">Tjänst-till-tjänst-autentisering (icke-interaktivt)</span><span class="sxs-lookup"><span data-stu-id="6139b-133">Service-to-service authentication (non-interactive)</span></span>
<span data-ttu-id="6139b-134">När du använder den här metoden tillhandahåller programmet sina egna autentiseringsuppgifter för att utföra åtgärderna.</span><span class="sxs-lookup"><span data-stu-id="6139b-134">Using this method, application provides its own credentials to perform the operations.</span></span> <span data-ttu-id="6139b-135">När du väljer den här metoden måste du skicka en POST-begäran som den som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="6139b-135">For this, you must issue a POST request like the one shown below:</span></span> 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

<span data-ttu-id="6139b-136">Utdata för begäran innehåller en autentiseringstoken (betecknas med `access-token` i utdata nedan) som du sedan skickar med REST API-anrop.</span><span class="sxs-lookup"><span data-stu-id="6139b-136">The output of this request will include an authorization token (denoted by `access-token` in the output below) that you will subsequently pass with your REST API calls.</span></span> <span data-ttu-id="6139b-137">Spara den här autentiseringstoken i en textfil; du behöver den senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="6139b-137">Save this authentication token in a text file; you will need this later in this article.</span></span>

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

<span data-ttu-id="6139b-138">I den här artikeln används metoden **icke-interaktivt**.</span><span class="sxs-lookup"><span data-stu-id="6139b-138">This article uses the **non-interactive** approach.</span></span> <span data-ttu-id="6139b-139">Mer information om icke-interaktivt (serviceartikel) finns i [Serviceartikelanrop med autentiseringsuppgifter](https://msdn.microsoft.com/library/azure/dn645543.aspx).</span><span class="sxs-lookup"><span data-stu-id="6139b-139">For more information on non-interactive (service-to-service calls), see [Service to service calls using credentials](https://msdn.microsoft.com/library/azure/dn645543.aspx).</span></span>

## <a name="create-a-data-lake-analytics-account"></a><span data-ttu-id="6139b-140">Skapa ett Data Lake Analytics-konto</span><span class="sxs-lookup"><span data-stu-id="6139b-140">Create a Data Lake Analytics account</span></span>
<span data-ttu-id="6139b-141">Du måste skapa en Azure-resursgrupp och ett Data Lake Store-konto innan du kan skapa ett Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="6139b-141">You must create an Azure Resource group, and a Data Lake Store account before you can create a Data Lake Analytics account.</span></span>  <span data-ttu-id="6139b-142">Se [Skapa ett Data Lake Store-konto](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account).</span><span class="sxs-lookup"><span data-stu-id="6139b-142">See [Create a Data Lake Store account](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account).</span></span>

<span data-ttu-id="6139b-143">Följande Curl-kommando visar hur du skapar ett konto:</span><span class="sxs-lookup"><span data-stu-id="6139b-143">The following Curl command shows how to create an account:</span></span>

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<NewAzureDataLakeAnalyticsAccountName>?api-version=2016-11-01 -d@"C:\tutorials\adla\CreateDataLakeAnalyticsAccountRequest.json"

<span data-ttu-id="6139b-144">Ersätt \<`REDACTED`\> med autentiseringstoken, \<`AzureSubscriptionID`\> med ditt prenumerations-ID, \<`AzureResourceGroupName`\> med ett befintligt namn på en Azure-resursgrupp och \<`NewAzureDataLakeAnalyticsAccountName`\> med ett nytt namn för Data Lake Analytics-kontot.</span><span class="sxs-lookup"><span data-stu-id="6139b-144">Replace \<`REDACTED`\> with the authorization token, \<`AzureSubscriptionID`\> with your subscription ID, \<`AzureResourceGroupName`\> with an existing Azure Resource Group name, and \<`NewAzureDataLakeAnalyticsAccountName`\> with a new Data Lake Analytics Account name.</span></span> <span data-ttu-id="6139b-145">Nyttolasten för begäran för det här kommandot finns i den **CreateDatalakeAnalyticsAccountRequest.json**-fil som har angetts för parametern `-d` ovan.</span><span class="sxs-lookup"><span data-stu-id="6139b-145">The request payload for this command is contained in the **CreateDatalakeAnalyticsAccountRequest.json** file that is provided for the `-d` parameter above.</span></span> <span data-ttu-id="6139b-146">Innehållet i input.json-filen liknar följande:</span><span class="sxs-lookup"><span data-stu-id="6139b-146">The contents of the input.json file resemble the following:</span></span>

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


## <a name="list-data-lake-analytics-accounts-in-a-subscription"></a><span data-ttu-id="6139b-147">Lista över Data Lake Analytics-konton i en prenumeration</span><span class="sxs-lookup"><span data-stu-id="6139b-147">List Data Lake Analytics accounts in a subscription</span></span>
<span data-ttu-id="6139b-148">Följande Curl-kommando visar hur du skapar en lista med konton i en prenumeration:</span><span class="sxs-lookup"><span data-stu-id="6139b-148">The following Curl command shows how to list accounts in a subscription:</span></span>

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/providers/Microsoft.DataLakeAnalytics/Accounts?api-version=2016-11-01

<span data-ttu-id="6139b-149">Ersätt \<`REDACTED`\> med autentiseringstoken, \<`AzureSubscriptionID`\> med ditt prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="6139b-149">Replace \<`REDACTED`\> with the authorization token, \<`AzureSubscriptionID`\> with your subscription ID.</span></span> <span data-ttu-id="6139b-150">Utdatan liknar följande:</span><span class="sxs-lookup"><span data-stu-id="6139b-150">The output is similar to:</span></span>

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

## <a name="get-information-about-a-data-lake-analytics-account"></a><span data-ttu-id="6139b-151">Hämta information om ett Data Lake Analytics-konto</span><span class="sxs-lookup"><span data-stu-id="6139b-151">Get information about a Data Lake Analytics account</span></span>
<span data-ttu-id="6139b-152">Följande Curl-kommando visar hur du hämtar kontoinformation:</span><span class="sxs-lookup"><span data-stu-id="6139b-152">The following Curl command shows how to get an account information:</span></span>

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>?api-version=2015-11-01

<span data-ttu-id="6139b-153">Ersätt \<`REDACTED`\> med autentiseringstoken, \<`AzureSubscriptionID`\> med ditt prenumerations-ID \<`AzureResourceGroupName`\> med ett befintligt namn på en Azure-resursgrupp och \<`DataLakeAnalyticsAccountName`\> med ett nytt namn för Data Lake Analytics-kontot.</span><span class="sxs-lookup"><span data-stu-id="6139b-153">Replace \<`REDACTED`\> with the authorization token, \<`AzureSubscriptionID`\> with your subscription ID, \<`AzureResourceGroupName`\> with an existing Azure Resource Group name, and \<`DataLakeAnalyticsAccountName`\> with the name of an existing Data Lake Analytics Account.</span></span> <span data-ttu-id="6139b-154">Utdatan liknar följande:</span><span class="sxs-lookup"><span data-stu-id="6139b-154">The output is similar to:</span></span>

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

## <a name="list-data-lake-stores-of-a-data-lake-analytics-account"></a><span data-ttu-id="6139b-155">Ange Data Lake Stores för ett Data Lake Analytics-konto i en lista</span><span class="sxs-lookup"><span data-stu-id="6139b-155">List Data Lake Stores of a Data Lake Analytics account</span></span>
<span data-ttu-id="6139b-156">Följande Curl-kommando visar hur du anger Data Lake Stores för ett konto:</span><span class="sxs-lookup"><span data-stu-id="6139b-156">The following Curl command shows how to list Data Lake Stores of an account:</span></span>

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>/DataLakeStoreAccounts/?api-version=2016-11-01

<span data-ttu-id="6139b-157">Ersätt \<`REDACTED`\> med autentiseringstoken, \<`AzureSubscriptionID`\> med ditt prenumerations-ID \<`AzureResourceGroupName`\> med ett befintligt namn på en Azure-resursgrupp och \<`DataLakeAnalyticsAccountName`\> med ett nytt namn för Data Lake Analytics-kontot.</span><span class="sxs-lookup"><span data-stu-id="6139b-157">Replace \<`REDACTED`\> with the authorization token, \<`AzureSubscriptionID`\> with your subscription ID, \<`AzureResourceGroupName`\> with an existing Azure Resource Group name, and \<`DataLakeAnalyticsAccountName`\> with the name of an existing Data Lake Analytics Account.</span></span> <span data-ttu-id="6139b-158">Utdatan liknar följande:</span><span class="sxs-lookup"><span data-stu-id="6139b-158">The output is similar to:</span></span>

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

## <a name="submit-u-sql-jobs"></a><span data-ttu-id="6139b-159">Skicka U-SQL-jobb</span><span class="sxs-lookup"><span data-stu-id="6139b-159">Submit U-SQL jobs</span></span>
<span data-ttu-id="6139b-160">Följande Curl-kommando visar hur du skapar ett U-SQL-jobb:</span><span class="sxs-lookup"><span data-stu-id="6139b-160">The following Curl command shows how to submit a U-SQL job:</span></span>

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs/<NewGUID>?api-version=2016-03-20-preview -d@"C:\tutorials\adla\SubmitADLAJob.json"

<span data-ttu-id="6139b-161">Ersätt \<`REDACTED`\> med autentiseringstoken, \<`DataLakeAnalyticsAccountName`\> med namnet på ett befintligt Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="6139b-161">Replace \<`REDACTED`\> with the authorization token, \<`DataLakeAnalyticsAccountName`\> with the name of an existing Data Lake Analytics Account.</span></span> <span data-ttu-id="6139b-162">Nyttolasten i begäran för det här kommandot finns i den **SubmitADLAJob.json**-fil som har angetts för parametern `-d` ovan.</span><span class="sxs-lookup"><span data-stu-id="6139b-162">The request payload for this command is contained in the **SubmitADLAJob.json** file that is provided for the `-d` parameter above.</span></span> <span data-ttu-id="6139b-163">Innehållet i input.json-filen liknar följande:</span><span class="sxs-lookup"><span data-stu-id="6139b-163">The contents of the input.json file resemble the following:</span></span>

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
        ING Extractors.Tsv();\n\nOUTPUT @searchlog   \n    TO \"/Output/SearchLog-from-Data-Lake.csv\"\nUSING Outputters.Csv();"
        }
    }

<span data-ttu-id="6139b-164">Utdatan liknar följande:</span><span class="sxs-lookup"><span data-stu-id="6139b-164">The output is similar to:</span></span>

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


## <a name="list-u-sql-jobs"></a><span data-ttu-id="6139b-165">Lista U-SQL-jobb</span><span class="sxs-lookup"><span data-stu-id="6139b-165">List U-SQL jobs</span></span>
<span data-ttu-id="6139b-166">Följande Curl-kommando visar hur du anger U-SQL-jobb i en lista:</span><span class="sxs-lookup"><span data-stu-id="6139b-166">The following Curl command shows how to list U-SQL jobs:</span></span>

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs?api-version=2016-11-01 

<span data-ttu-id="6139b-167">Ersätt \<`REDACTED`\> med autentiseringstoken, och \<`DataLakeAnalyticsAccountName`\> med namnet på ett befintligt Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="6139b-167">Replace \<`REDACTED`\> with the authorization token, and \<`DataLakeAnalyticsAccountName`\> with the name of an existing Data Lake Analytics Account.</span></span> 

<span data-ttu-id="6139b-168">Utdatan liknar följande:</span><span class="sxs-lookup"><span data-stu-id="6139b-168">The output is similar to:</span></span>

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


## <a name="get-catalog-items"></a><span data-ttu-id="6139b-169">Hämta katalogobjekt</span><span class="sxs-lookup"><span data-stu-id="6139b-169">Get catalog items</span></span>
<span data-ttu-id="6139b-170">Följande Curl-kommando visar hur du hämtar databaserna från katalogen:</span><span class="sxs-lookup"><span data-stu-id="6139b-170">The following Curl command shows how to get the databases from the catalog:</span></span>

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/catalog/usql/databases?api-version=2016-11-01

<span data-ttu-id="6139b-171">Utdatan liknar följande:</span><span class="sxs-lookup"><span data-stu-id="6139b-171">The output is similar to:</span></span>

    {
    "@odata.context":"https://myadla0831.azuredatalakeanalytics.net/sqlip/$metadata#databases","value":[
        {
        "computeAccountName":"myadla0831","databaseName":"mytest","version":"f6956327-90b8-4648-ad8b-de3ff09274ea"
        },{
        "computeAccountName":"myadla0831","databaseName":"master","version":"e8bca908-cc73-41a3-9564-e9bcfaa21f4e"
        }
    ]
    }

## <a name="see-also"></a><span data-ttu-id="6139b-172">Se även</span><span class="sxs-lookup"><span data-stu-id="6139b-172">See also</span></span>
* <span data-ttu-id="6139b-173">Om du vill se en mer komplex fråga, se [Analysera webbplatsloggar med hjälp av Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="6139b-173">To see a more complex query, see [Analyze Website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="6139b-174">Information om att utveckla U-SQL-program finns i [Utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="6139b-174">To get started developing U-SQL applications, see [Develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
* <span data-ttu-id="6139b-175">Information om U-SQL finns i [Kom igång med U-SQL-språk i Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="6139b-175">To learn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
* <span data-ttu-id="6139b-176">Information om hanteringsuppgifter finns i [Hantera Azure Data Lake Analytics med hjälp av Azure Portal](data-lake-analytics-manage-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6139b-176">For management tasks, see [Manage Azure Data Lake Analytics using Azure portal](data-lake-analytics-manage-use-portal.md).</span></span>
* <span data-ttu-id="6139b-177">Om du vill få en översikt över Data Lake Analytics, se [Översikt över Azure Data Lake Analytics](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6139b-177">To get an overview of Data Lake Analytics, see [Azure Data Lake Analytics overview](data-lake-analytics-overview.md).</span></span>
* <span data-ttu-id="6139b-178">Klicka på flikväljarna överst på sidan om du vill se samma självstudier med andra verktyg.</span><span class="sxs-lookup"><span data-stu-id="6139b-178">To see the same tutorial using other tools, click the tab selectors on the top of the page.</span></span>

