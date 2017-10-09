---
title: "aaaUse hello REST API tooget igång med Data Lake Store | Microsoft Docs"
description: "Använd WebHDFS REST API: er tooperform åtgärder på Data Lake Store"
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
ms.openlocfilehash: 62fce8293dfee730a61f2a3d37fc138ce7c3afdf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-rest-apis"></a><span data-ttu-id="732c9-103">Kom igång med Azure Data Lake Store med hjälp av REST API:er</span><span class="sxs-lookup"><span data-stu-id="732c9-103">Get started with Azure Data Lake Store using REST APIs</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="732c9-104">Portalen</span><span class="sxs-lookup"><span data-stu-id="732c9-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="732c9-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="732c9-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="732c9-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="732c9-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="732c9-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="732c9-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="732c9-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="732c9-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="732c9-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="732c9-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="732c9-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="732c9-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="732c9-111">Python</span><span class="sxs-lookup"><span data-stu-id="732c9-111">Python</span></span>](data-lake-store-get-started-python.md)
>
> 

<span data-ttu-id="732c9-112">I den här artikeln får du lära dig hur toouse WebHDFS REST API: er och Data Lake Store REST API: er tooperform konto management samt filsystemsåtgärder på Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="732c9-112">In this article, you will learn how toouse WebHDFS REST APIs and Data Lake Store REST APIs tooperform account management as well as filesystem operations on Azure Data Lake Store.</span></span> <span data-ttu-id="732c9-113">Azure Data Lake Store visar sina egna REST API:er för kontohanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="732c9-113">Azure Data Lake Store exposes its own REST APIs for account management operations.</span></span> <span data-ttu-id="732c9-114">Men eftersom Data Lake Store är kompatibelt med HDFS och Hadoop-ekosystemet, stöder det användning av WebHDFS REST API:er för filsystemsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="732c9-114">However, because Data Lake Store is compatible with HDFS and Hadoop ecosystem, it supports using WebHDFS REST APIs for filesystem operations.</span></span>

> [!NOTE]
> <span data-ttu-id="732c9-115">Detaljerad information om hello REST API-stöd för Data Lake Store finns [Azure Data Lake Store REST API-referens](https://msdn.microsoft.com/library/mt693424.aspx).</span><span class="sxs-lookup"><span data-stu-id="732c9-115">For detailed information on hello REST API support for Data Lake Store, see [Azure Data Lake Store REST API Reference](https://msdn.microsoft.com/library/mt693424.aspx).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="732c9-116">Krav</span><span class="sxs-lookup"><span data-stu-id="732c9-116">Prerequisites</span></span>
* <span data-ttu-id="732c9-117">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="732c9-117">**An Azure subscription**.</span></span> <span data-ttu-id="732c9-118">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="732c9-118">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="732c9-119">**Skapa ett program i Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="732c9-119">**Create an Azure Active Directory Application**.</span></span> <span data-ttu-id="732c9-120">Du kan använda hello Azure AD application tooauthenticate hello Data Lake Store-program med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="732c9-120">You use hello Azure AD application tooauthenticate hello Data Lake Store application with Azure AD.</span></span> <span data-ttu-id="732c9-121">Det finns olika sätt tooauthenticate med Azure AD, som är **slutanvändarens autentisering** eller **tjänst-till-tjänst autentisering**.</span><span class="sxs-lookup"><span data-stu-id="732c9-121">There are different approaches tooauthenticate with Azure AD, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="732c9-122">Anvisningar och mer information om hur tooauthenticate, se [slutanvändarens autentisering](data-lake-store-end-user-authenticate-using-active-directory.md) eller [tjänst-till-tjänst autentisering](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="732c9-122">For instructions and more information on how tooauthenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>
* <span data-ttu-id="732c9-123">[cURL](http://curl.haxx.se/).</span><span class="sxs-lookup"><span data-stu-id="732c9-123">[cURL](http://curl.haxx.se/).</span></span> <span data-ttu-id="732c9-124">Den här artikeln använder cURL toodemonstrate hur toomake REST API-anrop mot ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="732c9-124">This article uses cURL toodemonstrate how toomake REST API calls against a Data Lake Store account.</span></span>

## <a name="how-do-i-authenticate-using-azure-active-directory"></a><span data-ttu-id="732c9-125">Hur autentiserar jag med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="732c9-125">How do I authenticate using Azure Active Directory?</span></span>
<span data-ttu-id="732c9-126">Du kan använda två metoder tooauthenticate med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="732c9-126">You can use two approaches tooauthenticate using Azure Active Directory.</span></span>

### <a name="end-user-authentication-interactive"></a><span data-ttu-id="732c9-127">Slutanvändarautentisering (interaktiv)</span><span class="sxs-lookup"><span data-stu-id="732c9-127">End-user authentication (interactive)</span></span>
<span data-ttu-id="732c9-128">I det här scenariot uppmanar hello programmet hello användaren toolog i och alla hello åtgärder utförs i hello hello användarens kontext.</span><span class="sxs-lookup"><span data-stu-id="732c9-128">In this scenario, hello application prompts hello user toolog in and all hello operations are performed in hello context of hello user.</span></span> <span data-ttu-id="732c9-129">Utför följande steg för interaktiv autentisering hello.</span><span class="sxs-lookup"><span data-stu-id="732c9-129">Perform hello following steps for interactive authentication.</span></span>

1. <span data-ttu-id="732c9-130">Omdirigera hello användaren toohello följande URL: en via ditt program:</span><span class="sxs-lookup"><span data-stu-id="732c9-130">Through your application, redirect hello user toohello following URL:</span></span>
   
        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<APPLICATION-ID>&response_type=code&redirect_uri=<REDIRECT-URI>
   
   > [!NOTE]
   > <span data-ttu-id="732c9-131">\<OMDIRIGERINGS-URI > måste toobe kodad för användning i en URL.</span><span class="sxs-lookup"><span data-stu-id="732c9-131">\<REDIRECT-URI> needs toobe encoded for use in a URL.</span></span> <span data-ttu-id="732c9-132">Alltså ska `https%3A%2F%2Flocalhost` användas för https://localhost)</span><span class="sxs-lookup"><span data-stu-id="732c9-132">So, for https://localhost, use `https%3A%2F%2Flocalhost`)</span></span>
   > 
   > 
   
    <span data-ttu-id="732c9-133">För hello syftet med den här självstudiekursen, du ersätta hello platshållarvärdena i URL: en för hello ovan och klistra in den i webbläsarens adressfält.</span><span class="sxs-lookup"><span data-stu-id="732c9-133">For hello purpose of this tutorial, you can replace hello placeholder values in hello URL above and paste it in a web browser's address bar.</span></span> <span data-ttu-id="732c9-134">Du kommer att omdirigerade tooauthenticate med Azure-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="732c9-134">You will be redirected tooauthenticate using your Azure login.</span></span> <span data-ttu-id="732c9-135">När du har loggar in visas svaret hello i hello webbläsarens adressfält.</span><span class="sxs-lookup"><span data-stu-id="732c9-135">Once you successfully log in, hello response is displayed in hello browser's address bar.</span></span> <span data-ttu-id="732c9-136">hello svaret ska ha hello följande format:</span><span class="sxs-lookup"><span data-stu-id="732c9-136">hello response will be in hello following format:</span></span>
   
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>
2. <span data-ttu-id="732c9-137">Avbilda hello Auktoriseringskoden från hello svar.</span><span class="sxs-lookup"><span data-stu-id="732c9-137">Capture hello authorization code from hello response.</span></span> <span data-ttu-id="732c9-138">För den här självstudiekursen kan du kopiera hello Auktoriseringskoden från adressfältet i hello av hello webbläsare och skicka den i hello POST-begäran toohello tokenslutpunkten, enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="732c9-138">For this tutorial, you can copy hello authorization code from hello address bar of hello web browser and pass it in hello POST request toohello token endpoint, as shown below:</span></span>
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<APPLICATION-ID> \
        -F code=<AUTHORIZATION-CODE>
   
   > [!NOTE]
   > <span data-ttu-id="732c9-139">I det här fallet hello \<OMDIRIGERINGS-URI > måste inte vara kodad.</span><span class="sxs-lookup"><span data-stu-id="732c9-139">In this case, hello \<REDIRECT-URI> need not be encoded.</span></span>
   > 
   > 
3. <span data-ttu-id="732c9-140">hello svaret är ett JSON-objekt som innehåller en åtkomsttoken (t.ex. `"access_token": "<ACCESS_TOKEN>"`) och en uppdateringstoken (t.ex. `"refresh_token": "<REFRESH_TOKEN>"`).</span><span class="sxs-lookup"><span data-stu-id="732c9-140">hello response is a JSON object that contains an access token (e.g., `"access_token": "<ACCESS_TOKEN>"`) and a refresh token (e.g., `"refresh_token": "<REFRESH_TOKEN>"`).</span></span> <span data-ttu-id="732c9-141">Programmet använder hello åtkomsttoken vid åtkomst till Azure Data Lake Store och hello uppdatera token tooget en annan åtkomsttoken när en åtkomst-token upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="732c9-141">Your application uses hello access token when accessing Azure Data Lake Store and hello refresh token tooget another access token when an access token expires.</span></span>
   
        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before":    "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}
4. <span data-ttu-id="732c9-142">När hello åtkomst-token upphör att gälla, kan du begära en ny åtkomsttoken med hjälp av hello uppdateringstoken som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="732c9-142">When hello access token expires, you can request a new access token using hello refresh token, as shown below:</span></span>
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
             -F grant_type=refresh_token \
             -F resource=https://management.core.windows.net/ \
             -F client_id=<APPLICATION-ID> \
             -F refresh_token=<REFRESH-TOKEN>

<span data-ttu-id="732c9-143">Mer information om interaktiv användarautentisering finns i [Flöde beviljat med auktoriseringskod](https://msdn.microsoft.com/library/azure/dn645542.aspx).</span><span class="sxs-lookup"><span data-stu-id="732c9-143">For more information on interactive user authentication, see [Authorization code grant flow](https://msdn.microsoft.com/library/azure/dn645542.aspx).</span></span>

### <a name="service-to-service-authentication-non-interactive"></a><span data-ttu-id="732c9-144">Tjänst-till-tjänst-autentisering (icke-interaktivt)</span><span class="sxs-lookup"><span data-stu-id="732c9-144">Service-to-service authentication (non-interactive)</span></span>
<span data-ttu-id="732c9-145">I det här scenariot hello hello programmet tillhandahåller egna autentiseringsuppgifter tooperform hello åtgärder.</span><span class="sxs-lookup"><span data-stu-id="732c9-145">In this scenario, hello hello application provides its own credentials tooperform hello operations.</span></span> <span data-ttu-id="732c9-146">För att göra detta måste du skicka en POST-begäran som hello som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="732c9-146">For this, you must issue a POST request like hello one shown below.</span></span> 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

<span data-ttu-id="732c9-147">hello utdata för begäran innehåller en autentiseringstoken (betecknas med `access-token` i hello utdata nedan) som du sedan skickar med REST API-anrop.</span><span class="sxs-lookup"><span data-stu-id="732c9-147">hello output of this request will include an authorization token (denoted by `access-token` in hello output below) that you will subsequently pass with your REST API calls.</span></span> <span data-ttu-id="732c9-148">Spara den här autentiseringstoken i en textfil; du behöver den senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="732c9-148">Save this authentication token in a text file; you will need this later in this article.</span></span>

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

<span data-ttu-id="732c9-149">Den här artikeln använder hello **icke-interaktiv** metod.</span><span class="sxs-lookup"><span data-stu-id="732c9-149">This article uses hello **non-interactive** approach.</span></span> <span data-ttu-id="732c9-150">Mer information om icke-interaktivt (service to service anrop) finns [tjänsten tooservice anrop med autentiseringsuppgifter](https://msdn.microsoft.com/library/azure/dn645543.aspx).</span><span class="sxs-lookup"><span data-stu-id="732c9-150">For more information on non-interactive (service-to-service calls), see [Service tooservice calls using credentials](https://msdn.microsoft.com/library/azure/dn645543.aspx).</span></span>

## <a name="create-a-data-lake-store-account"></a><span data-ttu-id="732c9-151">Skapa ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="732c9-151">Create a Data Lake Store account</span></span>
<span data-ttu-id="732c9-152">Den här åtgärden är baserad på hello REST API-anrop som definierats [här](https://msdn.microsoft.com/library/mt694078.aspx).</span><span class="sxs-lookup"><span data-stu-id="732c9-152">This operation is based on hello REST API call defined [here](https://msdn.microsoft.com/library/mt694078.aspx).</span></span>

<span data-ttu-id="732c9-153">Använd hello följande cURL-kommando.</span><span class="sxs-lookup"><span data-stu-id="732c9-153">Use hello following cURL command.</span></span> <span data-ttu-id="732c9-154">Ersätt **\<yourstorename>** med ditt Data Lake Store-namn.</span><span class="sxs-lookup"><span data-stu-id="732c9-154">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

<span data-ttu-id="732c9-155">I hello ovanstående kommando ersätter \< `REDACTED` \> med hello autentiseringstoken som hämtats tidigare.</span><span class="sxs-lookup"><span data-stu-id="732c9-155">In hello above command, replace \<`REDACTED`\> with hello authorization token you retrieved earlier.</span></span> <span data-ttu-id="732c9-156">Hej begärannyttolast för det här kommandot finns i hello **input.json** fil som har angetts för hello `-d` parametern ovan.</span><span class="sxs-lookup"><span data-stu-id="732c9-156">hello request payload for this command is contained in hello **input.json** file that is provided for hello `-d` parameter above.</span></span> <span data-ttu-id="732c9-157">hello innehållet i input.json-filen för hello likna hello följande:</span><span class="sxs-lookup"><span data-stu-id="732c9-157">hello contents of hello input.json file resemble hello following:</span></span>

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }    

## <a name="create-folders-in-a-data-lake-store-account"></a><span data-ttu-id="732c9-158">Skapa mappar i ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="732c9-158">Create folders in a Data Lake Store account</span></span>
<span data-ttu-id="732c9-159">Den här åtgärden är baserad på hello WebHDFS REST API-anrop som definierats [här](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).</span><span class="sxs-lookup"><span data-stu-id="732c9-159">This operation is based on hello WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).</span></span>

<span data-ttu-id="732c9-160">Använd hello följande cURL-kommando.</span><span class="sxs-lookup"><span data-stu-id="732c9-160">Use hello following cURL command.</span></span> <span data-ttu-id="732c9-161">Ersätt **\<yourstorename>** med ditt Data Lake Store-namn.</span><span class="sxs-lookup"><span data-stu-id="732c9-161">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS'

<span data-ttu-id="732c9-162">I hello ovanstående kommando ersätter \< `REDACTED` \> med hello autentiseringstoken som hämtats tidigare.</span><span class="sxs-lookup"><span data-stu-id="732c9-162">In hello above command, replace \<`REDACTED`\> with hello authorization token you retrieved earlier.</span></span> <span data-ttu-id="732c9-163">Det här kommandot skapar en katalog med namnet **mytempdir** under hello rotmappen för ditt Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="732c9-163">This command creates a directory called **mytempdir** under hello root folder of your Data Lake Store account.</span></span>

<span data-ttu-id="732c9-164">Om hello åtgärden slutförs bör du se ett svar som detta:</span><span class="sxs-lookup"><span data-stu-id="732c9-164">You should see a response like this if hello operation completes successfully:</span></span>

    {"boolean":true}

## <a name="list-folders-in-a-data-lake-store-account"></a><span data-ttu-id="732c9-165">Lista mappar i ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="732c9-165">List folders in a Data Lake Store account</span></span>
<span data-ttu-id="732c9-166">Den här åtgärden är baserad på hello WebHDFS REST API-anrop som definierats [här](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).</span><span class="sxs-lookup"><span data-stu-id="732c9-166">This operation is based on hello WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).</span></span>

<span data-ttu-id="732c9-167">Använd hello följande cURL-kommando.</span><span class="sxs-lookup"><span data-stu-id="732c9-167">Use hello following cURL command.</span></span> <span data-ttu-id="732c9-168">Ersätt **\<yourstorename>** med ditt Data Lake Store-namn.</span><span class="sxs-lookup"><span data-stu-id="732c9-168">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS'

<span data-ttu-id="732c9-169">I hello ovanstående kommando ersätter \< `REDACTED` \> med hello autentiseringstoken som hämtats tidigare.</span><span class="sxs-lookup"><span data-stu-id="732c9-169">In hello above command, replace \<`REDACTED`\> with hello authorization token you retrieved earlier.</span></span>

<span data-ttu-id="732c9-170">Om hello åtgärden slutförs bör du se ett svar som detta:</span><span class="sxs-lookup"><span data-stu-id="732c9-170">You should see a response like this if hello operation completes successfully:</span></span>

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

## <a name="upload-data-into-a-data-lake-store-account"></a><span data-ttu-id="732c9-171">Ladda upp data till ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="732c9-171">Upload data into a Data Lake Store account</span></span>
<span data-ttu-id="732c9-172">Den här åtgärden är baserad på hello WebHDFS REST API-anrop som definierats [här](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).</span><span class="sxs-lookup"><span data-stu-id="732c9-172">This operation is based on hello WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).</span></span>

<span data-ttu-id="732c9-173">Använd hello följande cURL-kommando.</span><span class="sxs-lookup"><span data-stu-id="732c9-173">Use hello following cURL command.</span></span> <span data-ttu-id="732c9-174">Ersätt **\<yourstorename>** med ditt Data Lake Store-namn.</span><span class="sxs-lookup"><span data-stu-id="732c9-174">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X PUT -L -T 'C:\temp\list.txt' -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE'

<span data-ttu-id="732c9-175">I hello ovan syntax **-T** parametern är hello platsen för hello-filen som du överför.</span><span class="sxs-lookup"><span data-stu-id="732c9-175">In hello above syntax **-T** parameter is hello location of hello file you are uploading.</span></span>

<span data-ttu-id="732c9-176">hello-utdata är liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="732c9-176">hello output is similar toohello following:</span></span>
   
    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE&write=true
    ...
    Content-Length: 0

    HTTP/1.1 100 Continue

    HTTP/1.1 201 Created
    ...

## <a name="read-data-from-a-data-lake-store-account"></a><span data-ttu-id="732c9-177">Läs data från ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="732c9-177">Read data from a Data Lake Store account</span></span>
<span data-ttu-id="732c9-178">Den här åtgärden är baserad på hello WebHDFS REST API-anrop som definierats [här](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).</span><span class="sxs-lookup"><span data-stu-id="732c9-178">This operation is based on hello WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).</span></span>

<span data-ttu-id="732c9-179">Att läsa data från ett Data Lake Store-konto är en tvåstegsprocess.</span><span class="sxs-lookup"><span data-stu-id="732c9-179">Reading data from a Data Lake Store account is a two-step process.</span></span>

* <span data-ttu-id="732c9-180">Du skickar först en GET-begäran mot slutpunkten hello `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`.</span><span class="sxs-lookup"><span data-stu-id="732c9-180">You first submit a GET request against hello endpoint `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`.</span></span> <span data-ttu-id="732c9-181">Detta returnerar en plats toosubmit hello nästa GET-begäran.</span><span class="sxs-lookup"><span data-stu-id="732c9-181">This will return a location toosubmit hello next GET request to.</span></span>
* <span data-ttu-id="732c9-182">Du skickar hello GET-begäran mot slutpunkten hello `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`.</span><span class="sxs-lookup"><span data-stu-id="732c9-182">You then submit hello GET request against hello endpoint `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`.</span></span> <span data-ttu-id="732c9-183">Detta visar hello innehållet i hello-fil.</span><span class="sxs-lookup"><span data-stu-id="732c9-183">This will display hello contents of hello file.</span></span>

<span data-ttu-id="732c9-184">Men eftersom det inte finns någon skillnad i hello indataparametrarna mellan hello först och hello andra steget kan du använda hello `-L` parametern toosubmit hello första begäran.</span><span class="sxs-lookup"><span data-stu-id="732c9-184">However, because there is no difference in hello input parameters between hello first and hello second step, you can use hello `-L` parameter toosubmit hello first request.</span></span> <span data-ttu-id="732c9-185">`-L`alternativet kombinerar två begäranden till ett i princip och gör att cURL gör om hello begäran på hello ny plats.</span><span class="sxs-lookup"><span data-stu-id="732c9-185">`-L` option essentially combines two requests into one and will make cURL redo hello request on hello new location.</span></span> <span data-ttu-id="732c9-186">Slutligen hello utdata från alla anrop för hello begäran visas som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="732c9-186">Finally, hello output from all hello request calls is displayed, like shown below.</span></span> <span data-ttu-id="732c9-187">Ersätt **\<yourstorename>** med ditt Data Lake Store-namn.</span><span class="sxs-lookup"><span data-stu-id="732c9-187">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN'

<span data-ttu-id="732c9-188">Du bör se en utdata liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="732c9-188">You should see an output similar toohello following:</span></span>

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...

    HTTP/1.1 200 OK
    ...

    Hello, Data Lake Store user!

## <a name="rename-a-file-in-a-data-lake-store-account"></a><span data-ttu-id="732c9-189">Byt namn på en fil i ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="732c9-189">Rename a file in a Data Lake Store account</span></span>
<span data-ttu-id="732c9-190">Den här åtgärden är baserad på hello WebHDFS REST API-anrop som definierats [här](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).</span><span class="sxs-lookup"><span data-stu-id="732c9-190">This operation is based on hello WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).</span></span>

<span data-ttu-id="732c9-191">Använd hello följande cURL-kommando toorename en fil.</span><span class="sxs-lookup"><span data-stu-id="732c9-191">Use hello following cURL command toorename a file.</span></span> <span data-ttu-id="732c9-192">Ersätt **\<yourstorename>** med ditt Data Lake Store-namn.</span><span class="sxs-lookup"><span data-stu-id="732c9-192">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt'

<span data-ttu-id="732c9-193">Du bör se en utdata liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="732c9-193">You should see an output similar toohello following:</span></span>

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="delete-a-file-from-a-data-lake-store-account"></a><span data-ttu-id="732c9-194">Ta bort en fil från ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="732c9-194">Delete a file from a Data Lake Store account</span></span>
<span data-ttu-id="732c9-195">Den här åtgärden är baserad på hello WebHDFS REST API-anrop som definierats [här](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).</span><span class="sxs-lookup"><span data-stu-id="732c9-195">This operation is based on hello WebHDFS REST API call defined [here](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).</span></span>

<span data-ttu-id="732c9-196">Använd hello följande cURL-kommando toodelete en fil.</span><span class="sxs-lookup"><span data-stu-id="732c9-196">Use hello following cURL command toodelete a file.</span></span> <span data-ttu-id="732c9-197">Ersätt **\<yourstorename>** med ditt Data Lake Store-namn.</span><span class="sxs-lookup"><span data-stu-id="732c9-197">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE'

<span data-ttu-id="732c9-198">Du bör se utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="732c9-198">You should see an output like hello following:</span></span>

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="delete-a-data-lake-store-account"></a><span data-ttu-id="732c9-199">Ta bort ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="732c9-199">Delete a Data Lake Store account</span></span>
<span data-ttu-id="732c9-200">Den här åtgärden är baserad på hello REST API-anrop som definierats [här](https://msdn.microsoft.com/library/mt694075.aspx).</span><span class="sxs-lookup"><span data-stu-id="732c9-200">This operation is based on hello REST API call defined [here](https://msdn.microsoft.com/library/mt694075.aspx).</span></span>

<span data-ttu-id="732c9-201">Använd hello följande cURL-kommando toodelete ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="732c9-201">Use hello following cURL command toodelete a Data Lake Store account.</span></span> <span data-ttu-id="732c9-202">Ersätt **\<yourstorename>** med ditt Data Lake Store-namn.</span><span class="sxs-lookup"><span data-stu-id="732c9-202">Replace **\<yourstorename>** with your Data Lake Store name.</span></span>

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

<span data-ttu-id="732c9-203">Du bör se utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="732c9-203">You should see an output like hello following:</span></span>

    HTTP/1.1 200 OK
    ...
    ...

## <a name="see-also"></a><span data-ttu-id="732c9-204">Se även</span><span class="sxs-lookup"><span data-stu-id="732c9-204">See also</span></span>
* [<span data-ttu-id="732c9-205">Stordataprogram med öppen källkod som är kompatibla med Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="732c9-205">Open Source Big Data applications compatible with Azure Data Lake Store</span></span>](data-lake-store-compatible-oss-other-applications.md)

