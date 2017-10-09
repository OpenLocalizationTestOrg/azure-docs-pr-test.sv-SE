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
# <a name="how-toouse-azure-storage-in-windows-store-apps"></a>Hur toouse Azure Storage i Windows Store-appar
## <a name="overview"></a>Översikt
Den här guiden visar hur tooget igång med att utveckla en Windows Store-app som använder Azure Storage.

## <a name="download-required-tools"></a>Hämta verktyg som krävs
* [Visual Studio](https://www.visualstudio.com/downloads/) gör det enkelt toobuild, lokalisera, paketera och distribuera Windows Store-appar. Visual Studio 2012 eller senare krävs.
* Hej [Azure Storage-klientbibliotek](https://www.nuget.org/packages/WindowsAzure.Storage) innehåller en Windows Runtime-klassbiblioteket för att arbeta med Azure Storage.
* [WCF Data Services-verktyg för Windows Store-appar](http://www.microsoft.com/download/details.aspx?id=30714) utökar hello Lägg till tjänstereferens erfarenhet av klientsidan OData-stöd för Windows Store-appar i Visual Studio.

## <a name="develop-apps"></a>Utveckla appar
### <a name="getting-ready"></a>Komma igång
Skapa en ny Windows Store-app-projekt i Visual Studio 2012 eller senare:

![store-appar-lagring – vs-projekt][store-apps-storage-vs-project]

Lägg till en referens toohello Azure Storage-klientbibliotek genom att högerklicka på **referenser**, klicka på **Lägg till referens**, och sedan bläddra toohello lagring klienten biblioteket för Windows Runtime som du ned:

![store-appar-lagring – Välj-bibliotek][store-apps-storage-choose-library]

### <a name="using-hello-library-with-hello-blob-and-queue-services"></a>Med hjälp av hello bibliotek med hello Blob och Queue-tjänster
Appen är nu redo toocall hello Azure Blob- och köegenskaper tjänster. Lägg till följande hello **med** instruktioner så att Azure Storage-typer kan refereras direkt:

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
```

Sedan lägger till en knapp tooyour sida. Lägg till följande kod tooits hello **klickar du på** händelse och ändra din Händelsehanterarmetoden med hello [asynkrona nyckelordet](http://msdn.microsoft.com/library/vstudio/hh156513.aspx):

```csharp
var credentials = new StorageCredentials(accountName, accountKey);
var account = new CloudStorageAccount(credentials, true);
var blobClient = account.CreateCloudBlobClient();
var container = blobClient.GetContainerReference("container1");
await container.CreateIfNotExistsAsync();
```

Den här koden förutsätter att du har två strängvariabler *accountName* och *accountKey*. De representerar hello namnet på ditt konto och hello lagringskontonyckel som är associerad med det kontot.

Skapa och köra programmet hello. Klicka på knappen för hello kontrollerar om en behållare med namnet *container1* finns i ditt konto och sedan skapa den om den inte.

### <a name="using-hello-library-with-hello-table-service"></a>Med hello tabelltjänsten hello-bibliotek
Typer som används toocommunicate med hello Azure Table-tjänsten är beroende av WCF Data Services för hello Windows Store-app-biblioteket. Lägg till en referens toohello krävs WCF-bibliotek med hjälp av hello Package Manager-konsolen:

![store-appar – lagring--Pakethanteraren][store-apps-storage-package-manager]

Använd hello följande kommando toopoint Package Manager toohello plats på datorn:

    Install-Package Microsoft.Data.OData.WindowsStore -Source "C:\Program Files (x86)\Microsoft WCF Data Services\5.0\bin\NuGet"

Detta kommando lägger automatiskt till alla nödvändiga referenser tooyour projekt. Om du inte vill att toouse hello Package Manager-konsolen kan du lägga till hello WCF Data Services NuGet mapp på din lokala dator toohello listan över paket källor och sedan lägga till hello referens med hello UI, enligt beskrivningen i [hantera NuGet-paket med hello dialogrutan](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog).

När du har refererad hello WCF Data Services NuGet-paketet, ändra hello koden i din knappen **klickar du på** händelse:

```csharp
var credentials = new StorageCredentials(accountName, accountKey);
var account = new CloudStorageAccount(credentials, true);
var tableClient = account.CreateCloudTableClient();
var table = tableClient.GetTableReference("table1");
await table.CreateIfNotExistsAsync();
```

Den här koden kontrollerar om det finns en tabell med namnet *tabell1* finns i ditt konto och sedan skapar den om den inte.

Du kan också lägga till en referens tooMicrosoft.WindowsAzure.Storage.Table.dll som finns i samma paket som du hämtade hello. Det här biblioteket innehåller ytterligare funktioner, till exempel reflection-baserade serialisering och allmänna frågor. Observera att det här biblioteket inte har stöd för JavaScript.

[store-apps-storage-vs-project]: ./media/storage-use-store-apps/store-apps-storage-vs-project.png
[store-apps-storage-choose-library]: ./media/storage-use-store-apps/store-apps-storage-choose-library.png
[store-apps-storage-package-manager]: ./media/storage-use-store-apps/store-apps-storage-package-manager.png
