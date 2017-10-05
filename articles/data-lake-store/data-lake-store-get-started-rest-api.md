---
title: "Använda REST-API:et för att komma igång med Data Lake Store | Microsoft Docs"
description: "Använd WebHDFS REST API:er för att utföra åtgärder på Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 57ac6501-cb71-4f75-82c2-acc07c562889
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: nitinme
ms.openlocfilehash: dc2c8f58e0a2faf1b00f4903148328a5141a8637
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-data-lake-store-using-rest-apis"></a><span data-ttu-id="e0b62-103">Kom igång med Azure Data Lake Store med hjälp av REST API:er</span><span class="sxs-lookup"><span data-stu-id="e0b62-103">Get started with Azure Data Lake Store using REST APIs</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e0b62-104">Portalen</span><span class="sxs-lookup"><span data-stu-id="e0b62-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="e0b62-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e0b62-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="e0b62-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="e0b62-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="e0b62-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="e0b62-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="e0b62-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="e0b62-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="e0b62-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e0b62-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="e0b62-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="e0b62-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="e0b62-111">Python</span><span class="sxs-lookup"><span data-stu-id="e0b62-111">Python</span></span>](data-lake-store-get-started-python.md)
>
> 

<span data-ttu-id="e0b62-112">I den här artikeln får du lära dig hur du använder WebHDFS REST API:er och Data Lake Store REST API:er för kontohantering samt filsystemsåtgärder på Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="e0b62-112">In this article, you will learn how to use WebHDFS REST APIs and Data Lake Store REST APIs to perform account management as well as filesystem operations on Azure Data Lake Store.</span></span> <span data-ttu-id="e0b62-113">Azure Data Lake Store visar sina egna REST API:er för kontohanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="e0b62-113">Azure Data Lake Store exposes its own REST APIs for account management operations.</span></span> <span data-ttu-id="e0b62-114">Men eftersom Data Lake Store är kompatibelt med HDFS och Hadoop-ekosystemet, stöder det användning av WebHDFS REST API:er för filsystemsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="e0b62-114">However, because Data Lake Store is compatible with HDFS and Hadoop ecosystem, it supports using WebHDFS REST APIs for filesystem operations.</span></span>

> [!NOTE]
> <span data-ttu-id="e0b62-115">Mer detaljerad information om REST API-stöd för Data Lake Store finns i [Azure Data Lake Store REST API-referens](https://msdn.microsoft.com/library/mt693424.aspx).</span><span class="sxs-lookup"><span data-stu-id="e0b62-115">For detailed information on the REST API support for Data Lake Store, see [Azure Data Lake Store REST API Reference](https://msdn.microsoft.com/library/mt693424.aspx).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="e0b62-116">Krav</span><span class="sxs-lookup"><span data-stu-id="e0b62-116">Prerequisites</span></span>
* <span data-ttu-id="e0b62-117">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="e0b62-117">**An Azure subscription**.</span></span> <span data-ttu-id="e0b62-118">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e0b62-118">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="e0b62-119">**Skapa ett program i Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e0b62-119">**Create an Azure Active Directory Application**.</span></span> <span data-ttu-id="e0b62-120">Du kan använda Azure AD-program för att autentisera Data Lake Store-program med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e0b62-120">You use the Azure AD application to authenticate the Data Lake Store application with Azure AD.</span></span> <span data-ttu-id="e0b62-121">Det finns olika sätt att autentisera med Azure AD: **slutanvändarens autentisering** eller **serviceautentisering**.</span><span class="sxs-lookup"><span data-stu-id="e0b62-121">There are different approaches to authenticate with Azure AD, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="e0b62-122">Instruktioner och mer information om hur du autentiserar finns i [Slutanvändarautentisering](data-lake-store-end-user-authenticate-using-active-directory.md) eller [Tjänst-till-tjänst-autentisering](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="e0b62-122">For instructions and more information on how to authenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>
* <span data-ttu-id="e0b62-123">[cURL](http://curl.haxx.se/).</span><span class="sxs-lookup"><span data-stu-id="e0b62-123">[cURL](http://curl.haxx.se/).</span></span> <span data-ttu-id="e0b62-124">Den här artikeln använder cURL för att demonstrera hur du gör REST API-anrop mot ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="e0b62-124">This article uses cURL to demonstrate how to make REST API calls against a Data Lake Store account.</span></span>

## <a name="how-do-i-authenticate-using-azure-active-directory"></a><span data-ttu-id="e0b62-125">Hur autentiserar jag med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e0b62-125">How do I authenticate using Azure Active Directory?</span></span>
<span data-ttu-id="e0b62-126">Du kan använda två sätt för att autentisera med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e0b62-126">You can use two approaches to authenticate using Azure Active Directory.</span></span>

### <a name="end-user-authentication-interactive"></a><span data-ttu-id="e0b62-127">Slutanvändarautentisering (interaktiv)</span><span class="sxs-lookup"><span data-stu-id="e0b62-127">End-user authentication (interactive)</span></span>
<span data-ttu-id="e0b62-128">I det här scenariot uppmanar programmet användaren att logga in och alla åtgärder utförs i kontexten för användaren.</span><span class="sxs-lookup"><span data-stu-id="e0b62-128">In this scenario, the application prompts the user to log in and all the operations are performed in the context of the user.</span></span> <span data-ttu-id="e0b62-129">Utför följande steg för interaktiv autentisering.</span><span class="sxs-lookup"><span data-stu-id="e0b62-129">Perform the following steps for interactive authentication.</span></span>

1. <span data-ttu-id="e0b62-130">Omdirigera användaren via ditt program till följande URL:</span><span class="sxs-lookup"><span data-stu-id="e0b62-130">Through your application, redirect the user to the following URL:</span></span>
   
        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<APPLICATION-ID>&response_type=code&redirect_uri=<REDIRECT-URI>
   
   > [!NOTE]
   > <span data-ttu-id="e0b62-131">\<REDIRECT-URI> måste kodas för användning i en URL.</span><span class="sxs-lookup"><span data-stu-id="e0b62-131">\<REDIRECT-URI> needs to be encoded for use in a URL.</span></span> <span data-ttu-id="e0b62-132">Alltså ska `https%3A%2F%2Flocalhost` användas för https://localhost)</span><span class="sxs-lookup"><span data-stu-id="e0b62-132">So, for https://localhost, use `https%3A%2F%2Flocalhost`)</span></span>
   > 
   > 
   
    <span data-ttu-id="e0b62-133">För självstudierna kan du ersätta platshållarvärdena i URL-adressen ovan och klistra in den i webbläsarens adressfält.</span><span class="sxs-lookup"><span data-stu-id="e0b62-133">For the purpose of this tutorial, you can replace the placeholder values in the URL above and paste it in a web browser's address bar.</span></span> <span data-ttu-id="e0b62-134">Du omdirigeras för att autentisera med Azure-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="e0b62-134">You will be redirected to authenticate using your Azure login.</span></span> <span data-ttu-id="e0b62-135">När du har loggat in visas svaret i webbläsarens adressfält.</span><span class="sxs-lookup"><span data-stu-id="e0b62-135">Once you successfully log in, the response is displayed in the browser's address bar.</span></span> <span data-ttu-id="e0b62-136">Svaret ska ha följande format:</span><span class="sxs-lookup"><span data-stu-id="e0b62-136">The response will be in the following format:</span></span>
   
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>
2. <span data-ttu-id="e0b62-137">Avbilda auktoriseringskoden från svaret.</span><span class="sxs-lookup"><span data-stu-id="e0b62-137">Capture the authorization code from the response.</span></span> <span data-ttu-id="e0b62-138">För de här självstudierna kan du kopiera auktoriseringskoden från adressfältet i webbläsaren och skicka den i POST-begäran till tokenslutpunkten, enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="e0b62-138">For this tutorial, you can copy the authorization code from the address bar of the web browser and pass it in the POST request to the token endpoint, as shown below:</span></span>
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<APPLICATION-ID> \
        -F code=<AUTHORIZATION-CODE>
   
   > [!NOTE]
   > <span data-ttu-id="e0b62-139">I det här fallet behöver \<REDIRECT-URI> inte vara kodad.</span><span class="sxs-lookup"><span data-stu-id="e0b62-139">In this case, the \<REDIRECT-URI> need not be encoded.</span></span>
   > 
   > 
3. <span data-ttu-id="e0b62-140">Svaret är ett JSON-objekt som innehåller en åtkomsttoken (t.ex. `"access_token": "<ACCESS_TOKEN>"`) och en uppdateringstoken (t.ex. `"refresh_token": "<REFRESH_TOKEN>"`).</span><span class="sxs-lookup"><span data-stu-id="e0b62-140">The response is a JSON object that contains an access token (e.g., `"access_token": "<ACCESS_TOKEN>"`) and a refresh token (e.g., `"refresh_token": "<REFRESH_TOKEN>"`).</span></span> <span data-ttu-id="e0b62-141">Programmet använder åtkomsttoken vid åtkomst till Azure Data Lake Store och uppdateringstoken för att få en annan åtkomsttoken när en åtkomst-token upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="e0b62-141">Your application uses the access token when accessing Azure Data Lake Store and the refresh token to get another access token when an access token expires.</span></span>
   
        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before":    "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}
4. <span data-ttu-id="e0b62-142">När åtkomsttoken upphört att gälla kan du begära en ny åtkomsttoken med hjälp av den uppdateringstoken som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="e0b62-142">When the access token expires, you can request a new access token using the refresh token, as shown below:</span></span>
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
             -F grant_type=refresh_token \
             -F resource=https://management.core.windows.net/ \
             -F client_id=<APPLICATION-ID> \
             -F refresh_token=<REFRESH-TOKEN>

<span data-ttu-id="e0b62-143">Mer information om interaktiv användarautentisering finns i [Flöde beviljat med auktoriseringskod](https://msdn.microsoft.com/library/azure/dn645542.aspx).</span><span class="sxs-lookup"><span data-stu-id="e0b62-143">For more information on interactive user authentication, see [Authorization code grant flow](https://msdn.microsoft.com/library/azure/dn645542.aspx).</span></span>

### <a name="service-to-service-authentication-non-interactive"></a><span data-ttu-id="e0b62-144">Tjänst-till-tjänst-autentisering (icke-interaktivt)</span><span class="sxs-lookup"><span data-stu-id="e0b62-144">Service-to-service authentication (non-interactive)</span></span>
<span data-ttu-id="e0b62-145">I det här scenariot ger programmet sina egna autentiseringsuppgifter för att utföra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="e0b62-145">In this scenario, the the application provides its own credentials to perform the operations.</span></span> <span data-ttu-id="e0b62-146">För detta måste du skicka en POST-begäran såsom visas nedan.</span><span class="sxs-lookup"><span data-stu-id="e0b62-146">For this, you must issue a POST request like the one shown below.</span></span> 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

<span data-ttu-id="e0b62-147">Utdata för begäran innehåller en autentiseringstoken (betecknas med `access-token` i utdata nedan) som du sedan skickar med REST API-anrop.</span><span class="sxs-lookup"><span data-stu-id="e0b62-147">The output of this request will include an authorization token (denoted by `access-token` in the output below) that you will subsequently pass with your REST API calls.</span></span> <span data-ttu-id="e0b62-148">Spara den här autentiseringstoken i en textfil; du behöver den senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="e0b62-148">Save this authentication token in a text file; you will need this later in this article.</span></span>

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

<span data-ttu-id="e0b62-149">I den här artikeln används metoden **icke-interaktivt**.</span><span class="sxs-lookup"><span data-stu-id="e0b62-149">This article uses the **non-interactive** approach.</span></span> <span data-ttu-id="e0b62-150">Mer information om icke-interaktivt (serviceartikel) finns i [Serviceartikelanrop med autentiseringsuppgifter](https://msdn.microsoft.com/library/azure/dn645543.aspx).</span><span class="sxs-lookup"><span data-stu-id="e0b62-150">For more information on non-interactive (service-to-service calls), see [Service to service calls using credentials](https://msdn.microsoft.com/library/azure/dn645543.aspx).</span></span>

## <a name="create-a-data-lake-store-account"></a><span data-ttu-id="e0b62-151">Skapa ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="e0b62-151">Create a Data Lake Store account</span></span>
<span data-ttu-id="e0b62-152">Den här åtgärden är baserad på det REST API-anrop som definierats [här](https://msdn.microsoft.com/library/mt694078.aspx).</span><span class="sxs-lookup"><span data-stu-id="e0b62-152">This operation is based on the REST API call defined [here](https://msdn.microsoft.com/library/mt694078.aspx).</span></span>

<span data-ttu-id="e0b62-153">Använd följande cURL-kommando.</span><span class="sxs-lookup"><span data-stu-id="e0b62-153">Use the following cURL command.</span></span> <span data-ttu-id="e0b62-154">Ersätt **\<yourstorename>** med ditt Data Lake Store-namn.</span><span class="sxs-lookup"><span data-stu-id="e0b62-154">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

<span data-ttu-id="e0b62-155">I kommandot ovan ersätter du \<`REDACTED`\> med den autentiseringstoken som hämtades tidigare.</span><span class="sxs-lookup"><span data-stu-id="e0b62-155">In the above command, replace \<`REDACTED`\> with the authorization token you retrieved earlier.</span></span> <span data-ttu-id="e0b62-156">Nyttolasten i begäran för det här kommandot finns i den **input.json**-fil som har angetts för parameter `-d` ovan.</span><span class="sxs-lookup"><span data-stu-id="e0b62-156">The request payload for this command is contained in the **input.json** file that is provided for the `-d` parameter above.</span></span> <span data-ttu-id="e0b62-157">Innehållet i input.json-filen liknar följande:</span><span class="sxs-lookup"><span data-stu-id="e0b62-157">The contents of the input.json file resemble the following:</span></span>

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }    

## <a name="create-folders-in-a-data-lake-store-account"></a><span data-ttu-id="e0b62-158">Skapa mappar i ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="e0b62-158">Create folders in a Data Lake Store account</span></span>
<span data-ttu-id="e0b62-159">Den här åtgärden är baserad på det WebHDFS REST API-anrop som definierats [här](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).</span><span class="sxs-lookup"><span data-stu-id="e0b62-159">This operation is based on the WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).</span></span>

<span data-ttu-id="e0b62-160">Använd följande cURL-kommando.</span><span class="sxs-lookup"><span data-stu-id="e0b62-160">Use the following cURL command.</span></span> <span data-ttu-id="e0b62-161">Ersätt **\<yourstorename>** med ditt Data Lake Store-namn.</span><span class="sxs-lookup"><span data-stu-id="e0b62-161">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS'

<span data-ttu-id="e0b62-162">I kommandot ovan ersätter du \<`REDACTED`\> med den autentiseringstoken som hämtades tidigare.</span><span class="sxs-lookup"><span data-stu-id="e0b62-162">In the above command, replace \<`REDACTED`\> with the authorization token you retrieved earlier.</span></span> <span data-ttu-id="e0b62-163">Det här kommandot skapar en katalog med namnet **mytempdir** under rotmappen för ditt Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="e0b62-163">This command creates a directory called **mytempdir** under the root folder of your Data Lake Store account.</span></span>

<span data-ttu-id="e0b62-164">Om åtgärden slutförs bör du se ett svar som detta:</span><span class="sxs-lookup"><span data-stu-id="e0b62-164">You should see a response like this if the operation completes successfully:</span></span>

    {"boolean":true}

## <a name="list-folders-in-a-data-lake-store-account"></a><span data-ttu-id="e0b62-165">Lista mappar i ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="e0b62-165">List folders in a Data Lake Store account</span></span>
<span data-ttu-id="e0b62-166">Den här åtgärden är baserad på det WebHDFS REST API-anrop som definierats [här](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).</span><span class="sxs-lookup"><span data-stu-id="e0b62-166">This operation is based on the WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).</span></span>

<span data-ttu-id="e0b62-167">Använd följande cURL-kommando.</span><span class="sxs-lookup"><span data-stu-id="e0b62-167">Use the following cURL command.</span></span> <span data-ttu-id="e0b62-168">Ersätt **\<yourstorename>** med ditt Data Lake Store-namn.</span><span class="sxs-lookup"><span data-stu-id="e0b62-168">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS'

<span data-ttu-id="e0b62-169">I kommandot ovan ersätter du \<`REDACTED`\> med den autentiseringstoken som hämtades tidigare.</span><span class="sxs-lookup"><span data-stu-id="e0b62-169">In the above command, replace \<`REDACTED`\> with the authorization token you retrieved earlier.</span></span>

<span data-ttu-id="e0b62-170">Om åtgärden slutförs bör du se ett svar som detta:</span><span class="sxs-lookup"><span data-stu-id="e0b62-170">You should see a response like this if the operation completes successfully:</span></span>

    {
    "FileStatuses": {
        "FileStatus": [{
            "length": 0,
            "pathSuffix": "mytempdir",
            "type": "DIRECTORY",
            "blockSize": 268435456,
            "accessTime": 1458324719512,
            "modificationTime": 1458324719512,
            "replication": 0,
            "permission": "777",
            "owner": "NotSupportYet",
            "group": "NotSupportYet"
        }]
    }
    }

## <a name="upload-data-into-a-data-lake-store-account"></a><span data-ttu-id="e0b62-171">Ladda upp data till ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="e0b62-171">Upload data into a Data Lake Store account</span></span>
<span data-ttu-id="e0b62-172">Den här åtgärden är baserad på det WebHDFS REST API-anrop som definierats [här](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).</span><span class="sxs-lookup"><span data-stu-id="e0b62-172">This operation is based on the WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).</span></span>

<span data-ttu-id="e0b62-173">Använd följande cURL-kommando.</span><span class="sxs-lookup"><span data-stu-id="e0b62-173">Use the following cURL command.</span></span> <span data-ttu-id="e0b62-174">Ersätt **\<yourstorename>** med ditt Data Lake Store-namn.</span><span class="sxs-lookup"><span data-stu-id="e0b62-174">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X PUT -L -T 'C:\temp\list.txt' -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE'

<span data-ttu-id="e0b62-175">I ovanstående syntax är parametern **-T** platsen för den fil som du överför.</span><span class="sxs-lookup"><span data-stu-id="e0b62-175">In the above syntax **-T** parameter is the location of the file you are uploading.</span></span>

<span data-ttu-id="e0b62-176">De utdata som genereras liknar följande:</span><span class="sxs-lookup"><span data-stu-id="e0b62-176">The output is similar to the following:</span></span>
   
    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE&write=true
    ...
    Content-Length: 0

    HTTP/1.1 100 Continue

    HTTP/1.1 201 Created
    ...

## <a name="read-data-from-a-data-lake-store-account"></a><span data-ttu-id="e0b62-177">Läs data från ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="e0b62-177">Read data from a Data Lake Store account</span></span>
<span data-ttu-id="e0b62-178">Den här åtgärden är baserad på det WebHDFS REST API-anrop som definierats [här](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).</span><span class="sxs-lookup"><span data-stu-id="e0b62-178">This operation is based on the WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).</span></span>

<span data-ttu-id="e0b62-179">Att läsa data från ett Data Lake Store-konto är en tvåstegsprocess.</span><span class="sxs-lookup"><span data-stu-id="e0b62-179">Reading data from a Data Lake Store account is a two-step process.</span></span>

* <span data-ttu-id="e0b62-180">Du skickar först en GET-begäran mot slutpunkten `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`.</span><span class="sxs-lookup"><span data-stu-id="e0b62-180">You first submit a GET request against the endpoint `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`.</span></span> <span data-ttu-id="e0b62-181">Detta returnerar en plats dit du skickar nästa GET-begäran.</span><span class="sxs-lookup"><span data-stu-id="e0b62-181">This will return a location to submit the next GET request to.</span></span>
* <span data-ttu-id="e0b62-182">Du skickar sedan GET-begäran mot slutpunkten `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`.</span><span class="sxs-lookup"><span data-stu-id="e0b62-182">You then submit the GET request against the endpoint `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`.</span></span> <span data-ttu-id="e0b62-183">Detta visar innehållet i filen.</span><span class="sxs-lookup"><span data-stu-id="e0b62-183">This will display the contents of the file.</span></span>

<span data-ttu-id="e0b62-184">Eftersom det inte finns någon skillnad i indataparametrarna mellan det första och andra steget, kan du använda parameter `-L` för att skicka den första begäran.</span><span class="sxs-lookup"><span data-stu-id="e0b62-184">However, because there is no difference in the input parameters between the first and the second step, you can use the `-L` parameter to submit the first request.</span></span> <span data-ttu-id="e0b62-185">`-L`-alternativet kombinerar i stort sett två begärande till ett och gör att cURL gör om begäran på den nya platsen.</span><span class="sxs-lookup"><span data-stu-id="e0b62-185">`-L` option essentially combines two requests into one and will make cURL redo the request on the new location.</span></span> <span data-ttu-id="e0b62-186">Slutligen visas utdata från alla anrop för begäran såsom visas nedan.</span><span class="sxs-lookup"><span data-stu-id="e0b62-186">Finally, the output from all the request calls is displayed, like shown below.</span></span> <span data-ttu-id="e0b62-187">Ersätt **\<yourstorename>** med ditt Data Lake Store-namn.</span><span class="sxs-lookup"><span data-stu-id="e0b62-187">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN'

<span data-ttu-id="e0b62-188">Du bör se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="e0b62-188">You should see an output similar to the following:</span></span>

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...

    HTTP/1.1 200 OK
    ...

    Hello, Data Lake Store user!

## <a name="rename-a-file-in-a-data-lake-store-account"></a><span data-ttu-id="e0b62-189">Byt namn på en fil i ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="e0b62-189">Rename a file in a Data Lake Store account</span></span>
<span data-ttu-id="e0b62-190">Den här åtgärden är baserad på det WebHDFS REST API-anrop som definierats [här](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).</span><span class="sxs-lookup"><span data-stu-id="e0b62-190">This operation is based on the WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).</span></span>

<span data-ttu-id="e0b62-191">Använd kommandot cURL för att byta namn på en fil.</span><span class="sxs-lookup"><span data-stu-id="e0b62-191">Use the following cURL command to rename a file.</span></span> <span data-ttu-id="e0b62-192">Ersätt **\<yourstorename>** med ditt Data Lake Store-namn.</span><span class="sxs-lookup"><span data-stu-id="e0b62-192">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt'

<span data-ttu-id="e0b62-193">Du bör se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="e0b62-193">You should see an output similar to the following:</span></span>

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="delete-a-file-from-a-data-lake-store-account"></a><span data-ttu-id="e0b62-194">Ta bort en fil från ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="e0b62-194">Delete a file from a Data Lake Store account</span></span>
<span data-ttu-id="e0b62-195">Den här åtgärden är baserad på det WebHDFS REST API-anrop som definierats [här](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).</span><span class="sxs-lookup"><span data-stu-id="e0b62-195">This operation is based on the WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).</span></span>

<span data-ttu-id="e0b62-196">Använd kommandot cURL för att ta bort en fil.</span><span class="sxs-lookup"><span data-stu-id="e0b62-196">Use the following cURL command to delete a file.</span></span> <span data-ttu-id="e0b62-197">Ersätt **\<yourstorename>** med ditt Data Lake Store-namn.</span><span class="sxs-lookup"><span data-stu-id="e0b62-197">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE'

<span data-ttu-id="e0b62-198">Du bör se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="e0b62-198">You should see an output like the following:</span></span>

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="delete-a-data-lake-store-account"></a><span data-ttu-id="e0b62-199">Ta bort ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="e0b62-199">Delete a Data Lake Store account</span></span>
<span data-ttu-id="e0b62-200">Den här åtgärden är baserad på det REST API-anrop som definierats [här](https://msdn.microsoft.com/library/mt694075.aspx).</span><span class="sxs-lookup"><span data-stu-id="e0b62-200">This operation is based on the REST API call defined [here](https://msdn.microsoft.com/library/mt694075.aspx).</span></span>

<span data-ttu-id="e0b62-201">Använd kommandot cURL för att ta bort ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="e0b62-201">Use the following cURL command to delete a Data Lake Store account.</span></span> <span data-ttu-id="e0b62-202">Ersätt **\<yourstorename>** med ditt Data Lake Store-namn.</span><span class="sxs-lookup"><span data-stu-id="e0b62-202">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

<span data-ttu-id="e0b62-203">Du bör se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="e0b62-203">You should see an output like the following:</span></span>

    HTTP/1.1 200 OK
    ...
    ...

## <a name="see-also"></a><span data-ttu-id="e0b62-204">Se även</span><span class="sxs-lookup"><span data-stu-id="e0b62-204">See also</span></span>
* [<span data-ttu-id="e0b62-205">Stordataprogram med öppen källkod som är kompatibla med Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="e0b62-205">Open Source Big Data applications compatible with Azure Data Lake Store</span></span>](data-lake-store-compatible-oss-other-applications.md)

