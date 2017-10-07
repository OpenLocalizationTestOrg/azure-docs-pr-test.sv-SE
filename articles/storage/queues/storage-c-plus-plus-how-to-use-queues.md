---
title: aaaHow toouse queue storage (C++) | Microsoft Docs
description: "Lär dig hur toouse hello queue storage-tjänst i Azure. Exempel är skrivna i C++."
services: storage
documentationcenter: .net
author: cbrooksmsft
manager: jahogg
editor: tysonn
ms.assetid: c8a36365-29f6-404d-8fd1-858a7f33b50a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: cpp
ms.topic: article
ms.date: 05/11/2017
ms.author: cbrooksmsft
ms.openlocfilehash: b0cddf017878e9fab87f47d24b2906e40c9f4ad5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-c"></a><span data-ttu-id="7d9a7-104">Hur toouse Queue Storage från C++</span><span class="sxs-lookup"><span data-stu-id="7d9a7-104">How toouse Queue Storage from C++</span></span>
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="7d9a7-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="7d9a7-105">Overview</span></span>
<span data-ttu-id="7d9a7-106">Den här guiden visar hur tooperform vanliga scenarier med hjälp av hello Azure Queue storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-106">This guide will show you how tooperform common scenarios using hello Azure Queue storage service.</span></span> <span data-ttu-id="7d9a7-107">hello exemplen är skrivna i C++ och använda hello [Azure Storage-klientbibliotek för C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="7d9a7-107">hello samples are written in C++ and use hello [Azure Storage Client Library for C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span></span> <span data-ttu-id="7d9a7-108">hello beskrivs scenarier där **infoga**, **granskning**, **komma**, och **bort** kö meddelanden, samt  **Skapa och ta bort köer**.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-108">hello scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating and deleting queues**.</span></span>

> [!NOTE]
> <span data-ttu-id="7d9a7-109">Den här guiden riktad mot hello Azure Storage-klientbibliotek för C++ version 1.0.0 och senare.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-109">This guide targets hello Azure Storage Client Library for C++ version 1.0.0 and above.</span></span> <span data-ttu-id="7d9a7-110">hello rekommenderas version är Storage-klientbibliotek 2.2.0, som är tillgängliga via [NuGet](http://www.nuget.org/packages/wastorage) eller [GitHub](http://github.com/Azure/azure-storage-cpp/).</span><span class="sxs-lookup"><span data-stu-id="7d9a7-110">hello recommended version is Storage Client Library 2.2.0, which is available via [NuGet](http://www.nuget.org/packages/wastorage) or [GitHub](http://github.com/Azure/azure-storage-cpp/).</span></span>
> 
> 

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a><span data-ttu-id="7d9a7-111">Skapa ett C++-program</span><span class="sxs-lookup"><span data-stu-id="7d9a7-111">Create a C++ application</span></span>
<span data-ttu-id="7d9a7-112">I den här guiden använder lagringsfunktioner som kan köras i ett C++-program.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-112">In this guide, you will use storage features which can be run within a C++ application.</span></span>

<span data-ttu-id="7d9a7-113">toodo så behöver du tooinstall hello Azure Storage-klientbibliotek för C++ och skapa ett Azure storage-konto i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-113">toodo so, you will need tooinstall hello Azure Storage Client Library for C++ and create an Azure storage account in your Azure subscription.</span></span>

<span data-ttu-id="7d9a7-114">tooinstall hello Azure Storage-klientbibliotek för C++ kan du använda hello följande metoder:</span><span class="sxs-lookup"><span data-stu-id="7d9a7-114">tooinstall hello Azure Storage Client Library for C++, you can use hello following methods:</span></span>

* <span data-ttu-id="7d9a7-115">**Linux:** Följ instruktionerna i hello hello [Azure Storage-klientbibliotek för C++ viktigt](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) sidan.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-115">**Linux:** Follow hello instructions given in hello [Azure Storage Client Library for C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) page.</span></span>
* <span data-ttu-id="7d9a7-116">**Windows:** i Visual Studio klickar du på **Verktyg > NuGet Package Manager > Package Manager-konsolen**.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-116">**Windows:** In Visual Studio, click **Tools > NuGet Package Manager > Package Manager Console**.</span></span> <span data-ttu-id="7d9a7-117">Typen hello följande kommando i hello [NuGet Package Manager-konsolen](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) och tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-117">Type hello following command into hello [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) and press **ENTER**.</span></span>

```  
Install-Package wastorage
```

## <a name="configure-your-application-tooaccess-queue-storage"></a><span data-ttu-id="7d9a7-118">Konfigurera ditt program tooaccess Queue Storage</span><span class="sxs-lookup"><span data-stu-id="7d9a7-118">Configure your application tooaccess Queue Storage</span></span>
<span data-ttu-id="7d9a7-119">Lägga till följande hello innehålla instruktioner toohello överkant hello C++ filen där du vill toouse hello Azure storage-API: er tooaccess köer:</span><span class="sxs-lookup"><span data-stu-id="7d9a7-119">Add hello following include statements toohello top of hello C++ file where you want toouse hello Azure storage APIs tooaccess queues:</span></span>  

```cpp
#include <was/storage_account.h>
#include <was/queue.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="7d9a7-120">Ställ in en anslutningssträng för Azure storage</span><span class="sxs-lookup"><span data-stu-id="7d9a7-120">Set up an Azure storage connection string</span></span>
<span data-ttu-id="7d9a7-121">Ett Azure storage-klienten använder en lagring sträng toostore slutpunkter och autentiseringsuppgifter för åtkomst till data management services.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-121">An Azure storage client uses a storage connection string toostore endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="7d9a7-122">När den körs i ett klientprogram, du måste ange hello lagringsanslutningssträngen i hello följande format, med hello namnet på ditt konto och hello lagring lagringsåtkomstnyckel för hello storage-konto som anges i hello [Azure Portal](https://portal.azure.com)för hello *AccountName* och *AccountKey* värden.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-122">When running in a client application, you must provide hello storage connection string in hello following format, using hello name of your storage account and hello storage access key for hello storage account listed in hello [Azure Portal](https://portal.azure.com) for hello *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="7d9a7-123">Information om lagringskonton och snabbtangenterna finns [om Azure Lagringskonton](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7d9a7-123">For information on storage accounts and access keys, see [About Azure Storage Accounts](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).</span></span> <span data-ttu-id="7d9a7-124">Det här exemplet visar hur du kan deklarera en anslutningssträng för statiska fält toohold hello:</span><span class="sxs-lookup"><span data-stu-id="7d9a7-124">This example shows how you can declare a static field toohold hello connection string:</span></span>  

```cpp
// Define hello connection-string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

<span data-ttu-id="7d9a7-125">tootest ditt program i din lokala Windows-dator kan du använda hello Microsoft Azure [lagringsemulatorn](../common/storage-use-emulator.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) som installeras med hello [Azure SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="7d9a7-125">tootest your application in your local Windows computer, you can use hello Microsoft Azure [storage emulator](../common/storage-use-emulator.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) that is installed with hello [Azure SDK](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="7d9a7-126">hello storage-emulatorn är ett verktyg som simulerar hello Blob, köer och tabellen tjänster som är tillgängliga i Azure på utvecklingsdatorn lokala.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-126">hello storage emulator is a utility that simulates hello Blob, Queue, and Table services available in Azure on your local development machine.</span></span> <span data-ttu-id="7d9a7-127">hello följande exempel visas hur du deklarera ett statiskt fält toohold hello anslutning sträng tooyour lokala storage-emulatorn:</span><span class="sxs-lookup"><span data-stu-id="7d9a7-127">hello following example shows how you can declare a static field toohold hello connection string tooyour local storage emulator:</span></span>  

```cpp
// Define hello connection-string with Azure Storage Emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

<span data-ttu-id="7d9a7-128">toostart hello Azure storage-emulatorn väljer hello **starta** eller tryck på knappen hello **Windows** nyckel.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-128">toostart hello Azure storage emulator, select hello **Start** button or press hello **Windows** key.</span></span> <span data-ttu-id="7d9a7-129">Börja skriva **Azure Storage-emulatorn**, och välj **Microsoft Azure Storage-emulatorn** hello listan med program.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-129">Begin typing **Azure Storage Emulator**, and select **Microsoft Azure Storage Emulator** from hello list of applications.</span></span>

<span data-ttu-id="7d9a7-130">hello förutsätter följande exempel att du har använt någon av dessa två metoder tooget hello lagringsanslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-130">hello following samples assume that you have used one of these two methods tooget hello storage connection string.</span></span>

## <a name="retrieve-your-connection-string"></a><span data-ttu-id="7d9a7-131">Hämta anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="7d9a7-131">Retrieve your connection string</span></span>
<span data-ttu-id="7d9a7-132">Du kan använda hello **cloud_storage_account** klassen toorepresent kontoinformationen för lagring.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-132">You can use hello **cloud_storage_account** class toorepresent your Storage Account information.</span></span> <span data-ttu-id="7d9a7-133">tooretrieve lagringen kontoinformation från hello anslutningssträngen för lagring kan du använda hello **parsa** metod.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-133">tooretrieve your storage account information from hello storage connection string, you can use hello **parse** method.</span></span>

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

## <a name="how-to-create-a-queue"></a><span data-ttu-id="7d9a7-134">Så här: skapa en kö</span><span class="sxs-lookup"><span data-stu-id="7d9a7-134">How to: Create a queue</span></span>
<span data-ttu-id="7d9a7-135">En **cloud_queue_client** objekt kan du få referensobjekt för köer.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-135">A **cloud_queue_client** object lets you get reference objects for queues.</span></span> <span data-ttu-id="7d9a7-136">hello följande kod skapar en **cloud_queue_client** objekt.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-136">hello following code creates a **cloud_queue_client** object.</span></span>

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create a queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();
```

<span data-ttu-id="7d9a7-137">Använd hello **cloud_queue_client** objekt tooget en referens toohello kö som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-137">Use hello **cloud_queue_client** object tooget a reference toohello queue you want toouse.</span></span> <span data-ttu-id="7d9a7-138">Du kan skapa hello kö om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-138">You can create hello queue if it doesn't exist.</span></span>

```cpp
// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Create hello queue if it doesn't already exist.
 queue.create_if_not_exists();  
```

## <a name="how-to-insert-a-message-into-a-queue"></a><span data-ttu-id="7d9a7-139">Så här: Infoga ett meddelande i en kö</span><span class="sxs-lookup"><span data-stu-id="7d9a7-139">How to: Insert a message into a queue</span></span>
<span data-ttu-id="7d9a7-140">tooinsert ett meddelande i en befintlig kö först skapa en ny **cloud_queue_message**.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-140">tooinsert a message into an existing queue, first create a new **cloud_queue_message**.</span></span> <span data-ttu-id="7d9a7-141">Därefter anropar hello **add_message** metod.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-141">Next, call hello **add_message** method.</span></span> <span data-ttu-id="7d9a7-142">En **cloud_queue_message** kan skapas från antingen en sträng eller en **byte** matris.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-142">A **cloud_queue_message** can be created from either a string or a **byte** array.</span></span> <span data-ttu-id="7d9a7-143">Här är kod som skapar en kö (om den inte finns) och infogningar hello-meddelande ”Hello World”:</span><span class="sxs-lookup"><span data-stu-id="7d9a7-143">Here is code which creates a queue (if it doesn't exist) and inserts hello message 'Hello, World':</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Create hello queue if it doesn't already exist.
queue.create_if_not_exists();

// Create a message and add it toohello queue.
azure::storage::cloud_queue_message message1(U("Hello, World"));
queue.add_message(message1);  
```

## <a name="how-to-peek-at-hello-next-message"></a><span data-ttu-id="7d9a7-144">Så här: granska nästa hello-meddelande</span><span class="sxs-lookup"><span data-stu-id="7d9a7-144">How to: Peek at hello next message</span></span>
<span data-ttu-id="7d9a7-145">Du kan kika på hello-meddelande i hello framför en kö utan att ta bort den från hello kö genom att anropa hello **peek_message** metod.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-145">You can peek at hello message in hello front of a queue without removing it from hello queue by calling hello **peek_message** method.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Peek at hello next message.
azure::storage::cloud_queue_message peeked_message = queue.peek_message();

// Output hello message content.
std::wcout << U("Peeked message content: ") << peeked_message.content_as_string() << std::endl;
```

## <a name="how-to-change-hello-contents-of-a-queued-message"></a><span data-ttu-id="7d9a7-146">Så här: ändra hello innehållet i ett meddelande i kön</span><span class="sxs-lookup"><span data-stu-id="7d9a7-146">How to: Change hello contents of a queued message</span></span>
<span data-ttu-id="7d9a7-147">Du kan ändra hello innehållet i ett meddelande direkt i hello kö.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-147">You can change hello contents of a message in-place in hello queue.</span></span> <span data-ttu-id="7d9a7-148">Om hello-meddelande representerar en arbetsuppgift, kan du använda den här funktionen tooupdate hello hello arbete aktivitetens status.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-148">If hello message represents a work task, you could use this feature tooupdate hello status of hello work task.</span></span> <span data-ttu-id="7d9a7-149">hello följande kod uppdaterar hello kömeddelandet med nytt innehåll och anger hello synlighet timeout tooextend ytterligare 60 sekunder.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-149">hello following code updates hello queue message with new contents, and sets hello visibility timeout tooextend another 60 seconds.</span></span> <span data-ttu-id="7d9a7-150">Detta sparar hello statusen för arbetsuppgiften som associeras med hello-meddelande och ger hello klienten en annan minut toocontinue som arbetar på hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-150">This saves hello state of work associated with hello message, and gives hello client another minute toocontinue working on hello message.</span></span> <span data-ttu-id="7d9a7-151">Du kan använda den här tekniken tootrack arbetsflöden med flera steg i Kömeddelanden, utan att behöva toostart över från hello början om ett bearbetningssteg misslyckas på grund av toohardware eller ett programvarufel.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-151">You could use this technique tootrack multi-step workflows on queue messages, without having toostart over from hello beginning if a processing step fails due toohardware or software failure.</span></span> <span data-ttu-id="7d9a7-152">Normalt ska du behålla ett återförsöksvärde och om hello-meddelande försöks mer än n gånger, vill du ta bort den.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-152">Typically, you would keep a retry count as well, and if hello message is retried more than n times, you would delete it.</span></span> <span data-ttu-id="7d9a7-153">Detta skyddar mot meddelanden som utlöser ett programfel varje gång de bearbetas.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-153">This protects against a message that triggers an application error each time it is processed.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_conection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Get hello message from hello queue and update hello message contents.
// hello visibility timeout "0" means make it visible immediately.
// hello visibility timeout "60" means hello client can get another minute toocontinue
// working on hello message.
azure::storage::cloud_queue_message changed_message = queue.get_message();

changed_message.set_content(U("Changed message"));
queue.update_message(changed_message, std::chrono::seconds(60), true);

// Output hello message content.
std::wcout << U("Changed message content: ") << changed_message.content_as_string() << std::endl;  
```

## <a name="how-to-de-queue-hello-next-message"></a><span data-ttu-id="7d9a7-154">Så här: Frigör kö nästa hello-meddelande</span><span class="sxs-lookup"><span data-stu-id="7d9a7-154">How to: De-queue hello next message</span></span>
<span data-ttu-id="7d9a7-155">Koden tar bort ett meddelande från en kö i två steg.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-155">Your code de-queues a message from a queue in two steps.</span></span> <span data-ttu-id="7d9a7-156">När du anropar **get_message**, du får hello nästa meddelande i en kö.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-156">When you call **get_message**, you get hello next message in a queue.</span></span> <span data-ttu-id="7d9a7-157">Ett meddelande som returneras från **get_message** blir osynligt tooany annan kod läsa meddelanden från den här kön.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-157">A message returned from **get_message** becomes invisible tooany other code reading messages from this queue.</span></span> <span data-ttu-id="7d9a7-158">toofinish att ta bort hello-meddelande från kön hello, måste du också anropa **delete_message**.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-158">toofinish removing hello message from hello queue, you must also call **delete_message**.</span></span> <span data-ttu-id="7d9a7-159">Den här tvåstegsprocessen för att ta bort ett meddelande säkerställer som om din kod inte tooprocess får ett meddelande på grund av fel toohardware eller programvara, en annan instans av koden hello samma meddelande och försök igen.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-159">This two-step process of removing a message assures that if your code fails tooprocess a message due toohardware or software failure, another instance of your code can get hello same message and try again.</span></span> <span data-ttu-id="7d9a7-160">Koden anropar **delete_message** direkt efter hello-meddelande har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-160">Your code calls **delete_message** right after hello message has been processed.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Get hello next message.
azure::storage::cloud_queue_message dequeued_message = queue.get_message();
std::wcout << U("Dequeued message: ") << dequeued_message.content_as_string() << std::endl;

// Delete hello message.
queue.delete_message(dequeued_message);
```

## <a name="how-to-leverage-additional-options-for-de-queuing-messages"></a><span data-ttu-id="7d9a7-161">Så här: använda fler alternativ för meddelanden ur kön</span><span class="sxs-lookup"><span data-stu-id="7d9a7-161">How to: Leverage additional options for de-queuing messages</span></span>
<span data-ttu-id="7d9a7-162">Det finns två metoder som du kan använda för att anpassa meddelandehämtningen från en kö.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-162">There are two ways you can customize message retrieval from a queue.</span></span> <span data-ttu-id="7d9a7-163">Först får du en grupp med meddelanden (upp too32).</span><span class="sxs-lookup"><span data-stu-id="7d9a7-163">First, you can get a batch of messages (up too32).</span></span> <span data-ttu-id="7d9a7-164">Andra, du kan ange en tidsgräns för osynlighet längre eller kortare för att ge koden mer eller mindre tid toofully bearbeta varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-164">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time toofully process each message.</span></span> <span data-ttu-id="7d9a7-165">hello följande kodexempel används hello **get_messages** metoden tooget 20 meddelanden i ett anrop.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-165">hello following code example uses hello **get_messages** method tooget 20 messages in one call.</span></span> <span data-ttu-id="7d9a7-166">Sedan bearbetas varje meddelande med hjälp av en **för** loop.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-166">Then it processes each message using a **for** loop.</span></span> <span data-ttu-id="7d9a7-167">Den anger också hello osynlighet timeout toofive minuter för varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-167">It also sets hello invisibility timeout toofive minutes for each message.</span></span> <span data-ttu-id="7d9a7-168">Observera att hello 5 minuter startar för alla meddelanden med hello samma tid, så efter 5 minuter har gått sedan hello anropet för**get_messages**, alla meddelanden som inte har tagits bort kommer att bli synliga igen.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-168">Note that hello 5 minutes starts for all messages at hello same time, so after 5 minutes have passed since hello call too**get_messages**, any messages which have not been deleted will become visible again.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Dequeue some queue messages (maximum 32 at a time) and set their visibility timeout to
// 5 minutes (300 seconds).
azure::storage::queue_request_options options;
azure::storage::operation_context context;

// Retrieve 20 messages from hello queue with a visibility timeout of 300 seconds.
std::vector<azure::storage::cloud_queue_message> messages = queue.get_messages(20, std::chrono::seconds(300), options, context);

for (auto it = messages.cbegin(); it != messages.cend(); ++it)
{
    // Display hello contents of hello message.
    std::wcout << U("Get: ") << it->content_as_string() << std::endl;
}
```

## <a name="how-to-get-hello-queue-length"></a><span data-ttu-id="7d9a7-169">Så här: hämta hello Kölängd</span><span class="sxs-lookup"><span data-stu-id="7d9a7-169">How to: Get hello queue length</span></span>
<span data-ttu-id="7d9a7-170">Du kan få en uppskattning av hello antal meddelanden i en kö.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-170">You can get an estimate of hello number of messages in a queue.</span></span> <span data-ttu-id="7d9a7-171">Hej **download_attributes** metoden ber hello kön service tooretrieve hello köattributen, inklusive antal för hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-171">hello **download_attributes** method asks hello Queue service tooretrieve hello queue attributes, including hello message count.</span></span> <span data-ttu-id="7d9a7-172">Hej **approximate_message_count** metoden hämtar hello Ungefärligt antal meddelanden i kö för hello.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-172">hello **approximate_message_count** method gets hello approximate number of messages in hello queue.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Fetch hello queue attributes.
queue.download_attributes();

// Retrieve hello cached approximate message count.
int cachedMessageCount = queue.approximate_message_count();

// Display number of messages.
std::wcout << U("Number of messages in queue: ") << cachedMessageCount << std::endl;  
```

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="7d9a7-173">Så här: ta bort en kö</span><span class="sxs-lookup"><span data-stu-id="7d9a7-173">How to: Delete a queue</span></span>
<span data-ttu-id="7d9a7-174">en kö och alla hälsningsmeddelande som ingår i den anropet hello toodelete **delete_queue_if_exists** metod för hello köobjekt.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-174">toodelete a queue and all hello messages contained in it, call hello **delete_queue_if_exists** method on hello queue object.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// If hello queue exists and delete it.
queue.delete_queue_if_exists();  
```

## <a name="next-steps"></a><span data-ttu-id="7d9a7-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7d9a7-175">Next steps</span></span>
<span data-ttu-id="7d9a7-176">Nu när du har lärt dig hello grunderna i Queue storage kan du följa dessa länkar toolearn mer om Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="7d9a7-176">Now that you've learned hello basics of Queue storage, follow these links toolearn more about Azure Storage.</span></span>

* [<span data-ttu-id="7d9a7-177">Hur toouse Blob Storage från C++</span><span class="sxs-lookup"><span data-stu-id="7d9a7-177">How toouse Blob Storage from C++</span></span>](../blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [<span data-ttu-id="7d9a7-178">Hur toouse Table Storage från C++</span><span class="sxs-lookup"><span data-stu-id="7d9a7-178">How toouse Table Storage from C++</span></span>](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [<span data-ttu-id="7d9a7-179">Lista över Azure Storage-resurser i C++</span><span class="sxs-lookup"><span data-stu-id="7d9a7-179">List Azure Storage Resources in C++</span></span>](../common/storage-c-plus-plus-enumeration.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [<span data-ttu-id="7d9a7-180">Storage-klientbibliotek för C++-referens</span><span class="sxs-lookup"><span data-stu-id="7d9a7-180">Storage Client Library for C++ Reference</span></span>](http://azure.github.io/azure-storage-cpp)
* [<span data-ttu-id="7d9a7-181">Azure Storage-dokumentation</span><span class="sxs-lookup"><span data-stu-id="7d9a7-181">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)