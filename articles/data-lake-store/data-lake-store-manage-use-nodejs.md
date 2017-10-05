---
title: "Kom igång med Azure Data Lake Store med hjälp av Azure SDK för Node.js | Microsoft Docs"
description: "Lär dig använda Node.js för att arbeta med Data Lake Store-konton och filsystemet."
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 2fee173c-69ae-4e1d-8773-48618cda9e16
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/06/2017
ms.author: nitinme
ms.openlocfilehash: 8c7a2e6ca061bbfa077592efb73d592906c3d070
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-data-lake-store-using-azure-sdk-for-nodejs"></a><span data-ttu-id="606dd-103">Kom igång med Azure Data Lake Store med hjälp av Azure SDK för Node.js</span><span class="sxs-lookup"><span data-stu-id="606dd-103">Get started with Azure Data Lake Store using Azure SDK for Node.js</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="606dd-104">Portal</span><span class="sxs-lookup"><span data-stu-id="606dd-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="606dd-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="606dd-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="606dd-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="606dd-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="606dd-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="606dd-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="606dd-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="606dd-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="606dd-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="606dd-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="606dd-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="606dd-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="606dd-111">Python</span><span class="sxs-lookup"><span data-stu-id="606dd-111">Python</span></span>](data-lake-store-get-started-python.md)
>
> 

> [!NOTE]
> <span data-ttu-id="606dd-112">För att ladda upp och hämta stora mängder data (stora filer, ett stort antal filer eller båda), rekommenderar vi att du använder den [Python SDK](data-lake-store-get-started-python.md), [.NET SDK](data-lake-store-get-started-net-sdk.md), [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md), eller [Azure PowerShell](data-lake-store-get-started-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="606dd-112">For uploading and downloading large amount of data (large files, a large number of files, or both), we recommend that you use the [Python SDK](data-lake-store-get-started-python.md), the [.NET SDK](data-lake-store-get-started-net-sdk.md), [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md), or [Azure PowerShell](data-lake-store-get-started-powershell.md).</span></span> <span data-ttu-id="606dd-113">De alternativen har bättre prestanda eftersom de använder flera trådar för att parallellisera dataförflyttningen.</span><span class="sxs-lookup"><span data-stu-id="606dd-113">These options have better performance as they use multiple threads to parallelize the data movement.</span></span>
> 
> 

<span data-ttu-id="606dd-114">Lär dig mer om att använda Azure SDK för Node.js för att skapa ett Azure Data Lake Store-konto och utföra grundläggande åtgärder, till exempel skapa mappar, ladda upp och hämta datafiler, ta bort ditt konto, osv. Mer information om Data Lake Store finns i [Översikt över Data Lake Store](data-lake-store-overview.md).</span><span class="sxs-lookup"><span data-stu-id="606dd-114">Learn how to use the Azure SDK for Node.js to create an Azure Data Lake Store account and perform basic operations such as create folders, upload and download data files, delete your account, etc. For more information about Data Lake Store, see [Overview of Data Lake Store](data-lake-store-overview.md).</span></span> <span data-ttu-id="606dd-115">SDK stöder för närvarande,</span><span class="sxs-lookup"><span data-stu-id="606dd-115">Currently, the SDK supports</span></span>

* <span data-ttu-id="606dd-116">**Node.js-version: 0.10.0 eller högre**</span><span class="sxs-lookup"><span data-stu-id="606dd-116">**Node.js version: 0.10.0 or higher**</span></span>
* <span data-ttu-id="606dd-117">**REST API-version för konto: 2015-10-01-preview**</span><span class="sxs-lookup"><span data-stu-id="606dd-117">**REST API version for Account: 2015-10-01-preview**</span></span>
* <span data-ttu-id="606dd-118">**REST API-version för filsystem: 2015-10-01-preview**</span><span class="sxs-lookup"><span data-stu-id="606dd-118">**REST API version for FileSystem: 2015-10-01-preview**</span></span>

## <a name="prerequisites"></a><span data-ttu-id="606dd-119">Krav</span><span class="sxs-lookup"><span data-stu-id="606dd-119">Prerequisites</span></span>
<span data-ttu-id="606dd-120">Innan du påbörjar den här artikeln måste du ha:</span><span class="sxs-lookup"><span data-stu-id="606dd-120">Before you begin this article, you must have the following:</span></span>

* <span data-ttu-id="606dd-121">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="606dd-121">**An Azure subscription**.</span></span> <span data-ttu-id="606dd-122">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="606dd-122">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="606dd-123">**Skapa ett program i Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="606dd-123">**Create an Azure Active Directory Application**.</span></span> <span data-ttu-id="606dd-124">Du kan använda Azure AD-program för att autentisera Data Lake Store-program med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="606dd-124">You use the Azure AD application to authenticate the Data Lake Store application with Azure AD.</span></span> <span data-ttu-id="606dd-125">Det finns olika sätt att autentisera med Azure AD: **slutanvändarens autentisering** eller **serviceautentisering**.</span><span class="sxs-lookup"><span data-stu-id="606dd-125">There are different approaches to authenticate with Azure AD, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="606dd-126">Instruktioner och mer information om hur du autentiserar finns i [Slutanvändarautentisering](data-lake-store-end-user-authenticate-using-active-directory.md) eller [Tjänst-till-tjänst-autentisering](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="606dd-126">For instructions and more information on how to authenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>

## <a name="how-to-install"></a><span data-ttu-id="606dd-127">Så här installerar du</span><span class="sxs-lookup"><span data-stu-id="606dd-127">How to Install</span></span>
```bash
npm install azure-arm-datalake-store
```

## <a name="authenticate-using-azure-active-directory"></a><span data-ttu-id="606dd-128">Autentisera med hjälp av Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="606dd-128">Authenticate using Azure Active Directory</span></span>
<span data-ttu-id="606dd-129">Fragmenten nedan visar två olika sätt att autentisera med Data Lake Store med hjälp av Azure AD.</span><span class="sxs-lookup"><span data-stu-id="606dd-129">The snippets below show two separate ways of authenticating with Data Lake Store using Azure AD.</span></span> <span data-ttu-id="606dd-130">En detaljerad beskrivning på olika metoder som ska användas för autentisering med Data Lake Store finns i [autentisera med Data Lake Store med Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="606dd-130">For a detailed discussion on various methods to use for authentication with Data Lake Store, see [Authenticate with Data Lake Store using Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).</span></span>

<span data-ttu-id="606dd-131">Kodfragmentet nedan kräver också indata som Azure AD-domännamn, klient-ID för en Azure AD-appar, osv. Dessa uppgifter kan hämtas från en Azure AD-program som du skapade, som ska tas med i länken ovan.</span><span class="sxs-lookup"><span data-stu-id="606dd-131">The snippet below also requires inputs like Azure AD domain name, client ID for an Azure AD app, etc. All these details can be retrieved from an Azure AD application that you must created, the details of which are also included in link above.</span></span>

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-store-clients"></a><span data-ttu-id="606dd-132">Skapa Data Lake Store-klienter</span><span class="sxs-lookup"><span data-stu-id="606dd-132">Create the Data Lake Store Clients</span></span>
```javascript
var adlsManagement = require("azure-arm-datalake-store");
var acccountClient = new adlsManagement.DataLakeStoreAccountClient(credentials, "your-subscription-id");
var filesystemClient = new adlsManagement.DataLakeStoreFileSystemClient(credentials);
```

## <a name="create-a-data-lake-store-account"></a><span data-ttu-id="606dd-133">Skapa ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="606dd-133">Create a Data Lake Store Account</span></span>
```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlsacct';
var location = 'eastus2';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location
};

client.account.create(resourceGroupName, accountName, accountToCreate, function (err, result, request, response) {
  if (err) {
    console.log(err);
    /*err has reference to the actual request and response, so you can see what was sent and received on the wire.
      The structure of err looks like this:
      err: {
        code: 'Error Code',
        message: 'Error Message',
        body: 'The response body if any',
        request: reference to a stripped version of http request
        response: reference to a stripped version of the response
      }
    */
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="create-a-file-with-content"></a><span data-ttu-id="606dd-134">Skapa en fil med innehåll</span><span class="sxs-lookup"><span data-stu-id="606dd-134">Create a file with content</span></span>
```javascript
var util = require('util');
var accountName = 'testadlsacct';
var fileToCreate = '/myfolder/myfile.txt';
var options = {
  streamContents: new Buffer('some string content')
}

filesystemClient.fileSystem.listFileStatus(accountName, fileToCreate, options, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    // no result is returned, only a 201 response for success.
    console.log('response is: ' + util.inspect(response, {depth: null}));
  }
});
```

## <a name="get-a-list-of-files-and-folders"></a><span data-ttu-id="606dd-135">Hämta en lista över filer och mappar</span><span class="sxs-lookup"><span data-stu-id="606dd-135">Get a list of files and folders</span></span>
```javascript
var util = require('util');
var accountName = 'testadlsacct';
var pathToEnumerate = '/myfolder';
filesystemClient.fileSystem.listFileStatus(accountName, pathToEnumerate, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a><span data-ttu-id="606dd-136">Se även</span><span class="sxs-lookup"><span data-stu-id="606dd-136">See also</span></span>
* [<span data-ttu-id="606dd-137">Microsoft Azure SDK för Node.js</span><span class="sxs-lookup"><span data-stu-id="606dd-137">Microsoft Azure SDK for Node.js</span></span>](https://github.com/azure/azure-sdk-for-node)
* [<span data-ttu-id="606dd-138">Microsoft Azure SDK för Node.js – Data Lake Analytics Management</span><span class="sxs-lookup"><span data-stu-id="606dd-138">Microsoft Azure SDK for Node.js - Data Lake Analytics Management</span></span>](https://www.npmjs.com/package/azure-arm-datalake-analytics)

