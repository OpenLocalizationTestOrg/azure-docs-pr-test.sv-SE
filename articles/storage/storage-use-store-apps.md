---
title: aaaUse Azure storage i Windows Store-appar | Microsoft Docs
description: "Lär dig hur toocreate en Windows Store-appar som använder Azure Blob, kön, tabell eller filen."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 63c4b29d-b2f2-4d7c-b164-a0d38f4d14f6
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: ac21b0695c0d77c1d81c1b921718fb929945d45e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-storage-in-windows-store-apps"></a><span data-ttu-id="46162-103">Hur toouse Azure Storage i Windows Store-appar</span><span class="sxs-lookup"><span data-stu-id="46162-103">How toouse Azure Storage in Windows Store apps</span></span>
## <a name="overview"></a><span data-ttu-id="46162-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="46162-104">Overview</span></span>
<span data-ttu-id="46162-105">Den här guiden visar hur tooget igång med att utveckla en Windows Store-app som använder Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="46162-105">This guide shows how tooget started with developing a Windows Store app that makes use of Azure Storage.</span></span>

## <a name="download-required-tools"></a><span data-ttu-id="46162-106">Hämta verktyg som krävs</span><span class="sxs-lookup"><span data-stu-id="46162-106">Download required tools</span></span>
* <span data-ttu-id="46162-107">[Visual Studio](https://www.visualstudio.com/downloads/) gör det enkelt toobuild, lokalisera, paketera och distribuera Windows Store-appar.</span><span class="sxs-lookup"><span data-stu-id="46162-107">[Visual Studio](https://www.visualstudio.com/downloads/) makes it easy toobuild, debug, localize, package, and deploy Windows Store apps.</span></span> <span data-ttu-id="46162-108">Visual Studio 2012 eller senare krävs.</span><span class="sxs-lookup"><span data-stu-id="46162-108">Visual Studio 2012 or later is required.</span></span>
* <span data-ttu-id="46162-109">Hej [Azure Storage-klientbibliotek](https://www.nuget.org/packages/WindowsAzure.Storage) innehåller en Windows Runtime-klassbiblioteket för att arbeta med Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="46162-109">hello [Azure Storage Client Library](https://www.nuget.org/packages/WindowsAzure.Storage) provides a Windows Runtime class library for working with Azure Storage.</span></span>
* <span data-ttu-id="46162-110">[WCF Data Services-verktyg för Windows Store-appar](http://www.microsoft.com/download/details.aspx?id=30714) utökar hello Lägg till tjänstereferens erfarenhet av klientsidan OData-stöd för Windows Store-appar i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="46162-110">[WCF Data Services Tools for Windows Store Apps](http://www.microsoft.com/download/details.aspx?id=30714) extends hello Add Service Reference experience with client-side OData support for Windows Store apps in Visual Studio.</span></span>

## <a name="develop-apps"></a><span data-ttu-id="46162-111">Utveckla appar</span><span class="sxs-lookup"><span data-stu-id="46162-111">Develop apps</span></span>
### <a name="getting-ready"></a><span data-ttu-id="46162-112">Komma igång</span><span class="sxs-lookup"><span data-stu-id="46162-112">Getting ready</span></span>
<span data-ttu-id="46162-113">Skapa en ny Windows Store-app-projekt i Visual Studio 2012 eller senare:</span><span class="sxs-lookup"><span data-stu-id="46162-113">Create a new Windows Store app project in Visual Studio 2012 or later:</span></span>

![store-appar-lagring – vs-projekt][store-apps-storage-vs-project]

<span data-ttu-id="46162-115">Lägg till en referens toohello Azure Storage-klientbibliotek genom att högerklicka på **referenser**, klicka på **Lägg till referens**, och sedan bläddra toohello lagring klienten biblioteket för Windows Runtime som du ned:</span><span class="sxs-lookup"><span data-stu-id="46162-115">Next, add a reference toohello Azure Storage Client Library by right-clicking **References**, clicking **Add Reference**, and then browsing toohello Storage Client Library for Windows Runtime that you downloaded:</span></span>

![store-appar-lagring – Välj-bibliotek][store-apps-storage-choose-library]

### <a name="using-hello-library-with-hello-blob-and-queue-services"></a><span data-ttu-id="46162-117">Med hjälp av hello bibliotek med hello Blob och Queue-tjänster</span><span class="sxs-lookup"><span data-stu-id="46162-117">Using hello library with hello Blob and Queue services</span></span>
<span data-ttu-id="46162-118">Appen är nu redo toocall hello Azure Blob- och köegenskaper tjänster.</span><span class="sxs-lookup"><span data-stu-id="46162-118">At this point, your app is ready toocall hello Azure Blob and Queue services.</span></span> <span data-ttu-id="46162-119">Lägg till följande hello **med** instruktioner så att Azure Storage-typer kan refereras direkt:</span><span class="sxs-lookup"><span data-stu-id="46162-119">Add hello following **using** statements so that Azure Storage types can be referenced directly:</span></span>

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
```

<span data-ttu-id="46162-120">Sedan lägger till en knapp tooyour sida.</span><span class="sxs-lookup"><span data-stu-id="46162-120">Next, add a button tooyour page.</span></span> <span data-ttu-id="46162-121">Lägg till följande kod tooits hello **klickar du på** händelse och ändra din Händelsehanterarmetoden med hello [asynkrona nyckelordet](http://msdn.microsoft.com/library/vstudio/hh156513.aspx):</span><span class="sxs-lookup"><span data-stu-id="46162-121">Add hello following code tooits **Click** event and modify your event handler method by using hello [async keyword](http://msdn.microsoft.com/library/vstudio/hh156513.aspx):</span></span>

```csharp
var credentials = new StorageCredentials(accountName, accountKey);
var account = new CloudStorageAccount(credentials, true);
var blobClient = account.CreateCloudBlobClient();
var container = blobClient.GetContainerReference("container1");
await container.CreateIfNotExistsAsync();
```

<span data-ttu-id="46162-122">Den här koden förutsätter att du har två strängvariabler *accountName* och *accountKey*.</span><span class="sxs-lookup"><span data-stu-id="46162-122">This code assumes that you have two string variables, *accountName* and *accountKey*.</span></span> <span data-ttu-id="46162-123">De representerar hello namnet på ditt konto och hello lagringskontonyckel som är associerad med det kontot.</span><span class="sxs-lookup"><span data-stu-id="46162-123">They represent hello name of your storage account and hello account key that is associated with that account.</span></span>

<span data-ttu-id="46162-124">Skapa och köra programmet hello.</span><span class="sxs-lookup"><span data-stu-id="46162-124">Build and run hello application.</span></span> <span data-ttu-id="46162-125">Klicka på knappen för hello kontrollerar om en behållare med namnet *container1* finns i ditt konto och sedan skapa den om den inte.</span><span class="sxs-lookup"><span data-stu-id="46162-125">Clicking hello button will check whether a container named *container1* exists in your account and then create it if not.</span></span>

### <a name="using-hello-library-with-hello-table-service"></a><span data-ttu-id="46162-126">Med hello tabelltjänsten hello-bibliotek</span><span class="sxs-lookup"><span data-stu-id="46162-126">Using hello library with hello Table service</span></span>
<span data-ttu-id="46162-127">Typer som används toocommunicate med hello Azure Table-tjänsten är beroende av WCF Data Services för hello Windows Store-app-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="46162-127">Types that are used toocommunicate with hello Azure Table service depend on WCF Data Services for hello Windows Store app library.</span></span> <span data-ttu-id="46162-128">Lägg till en referens toohello krävs WCF-bibliotek med hjälp av hello Package Manager-konsolen:</span><span class="sxs-lookup"><span data-stu-id="46162-128">Next, add a reference toohello required WCF libraries by using hello Package Manager Console:</span></span>

![store-appar – lagring--Pakethanteraren][store-apps-storage-package-manager]

<span data-ttu-id="46162-130">Använd hello följande kommando toopoint Package Manager toohello plats på datorn:</span><span class="sxs-lookup"><span data-stu-id="46162-130">Use hello following command toopoint Package Manager toohello location on your machine:</span></span>

    Install-Package Microsoft.Data.OData.WindowsStore -Source "C:\Program Files (x86)\Microsoft WCF Data Services\5.0\bin\NuGet"

<span data-ttu-id="46162-131">Detta kommando lägger automatiskt till alla nödvändiga referenser tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="46162-131">This command will automatically add all required references tooyour project.</span></span> <span data-ttu-id="46162-132">Om du inte vill att toouse hello Package Manager-konsolen kan du lägga till hello WCF Data Services NuGet mapp på din lokala dator toohello listan över paket källor och sedan lägga till hello referens med hello UI, enligt beskrivningen i [hantera NuGet-paket med hello dialogrutan](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog).</span><span class="sxs-lookup"><span data-stu-id="46162-132">If you do not want toouse hello Package Manager Console, you can add hello WCF Data Services NuGet folder on your local machine toohello list of package sources and then add hello reference through hello UI, as described in [Managing NuGet Packages Using hello Dialog](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog).</span></span>

<span data-ttu-id="46162-133">När du har refererad hello WCF Data Services NuGet-paketet, ändra hello koden i din knappen **klickar du på** händelse:</span><span class="sxs-lookup"><span data-stu-id="46162-133">When you have referenced hello WCF Data Services NuGet package, change hello code in your button's **Click** event:</span></span>

```csharp
var credentials = new StorageCredentials(accountName, accountKey);
var account = new CloudStorageAccount(credentials, true);
var tableClient = account.CreateCloudTableClient();
var table = tableClient.GetTableReference("table1");
await table.CreateIfNotExistsAsync();
```

<span data-ttu-id="46162-134">Den här koden kontrollerar om det finns en tabell med namnet *tabell1* finns i ditt konto och sedan skapar den om den inte.</span><span class="sxs-lookup"><span data-stu-id="46162-134">This code checks whether a table named *table1* exists in your account, and then creates it if not.</span></span>

<span data-ttu-id="46162-135">Du kan också lägga till en referens tooMicrosoft.WindowsAzure.Storage.Table.dll som finns i samma paket som du hämtade hello.</span><span class="sxs-lookup"><span data-stu-id="46162-135">You can also add a reference tooMicrosoft.WindowsAzure.Storage.Table.dll, which is available in hello same package that you downloaded.</span></span> <span data-ttu-id="46162-136">Det här biblioteket innehåller ytterligare funktioner, till exempel reflection-baserade serialisering och allmänna frågor.</span><span class="sxs-lookup"><span data-stu-id="46162-136">This library contains additional functionality, such as reflection-based serialization and generic queries.</span></span> <span data-ttu-id="46162-137">Observera att det här biblioteket inte har stöd för JavaScript.</span><span class="sxs-lookup"><span data-stu-id="46162-137">Note that this library does not support JavaScript.</span></span>

[store-apps-storage-vs-project]: ./media/storage-use-store-apps/store-apps-storage-vs-project.png
[store-apps-storage-choose-library]: ./media/storage-use-store-apps/store-apps-storage-choose-library.png
[store-apps-storage-package-manager]: ./media/storage-use-store-apps/store-apps-storage-package-manager.png
