---
title: "Använda queue storage (C++) | Microsoft Docs"
description: "Lär dig hur du använder tjänsten queue storage i Azure. Exempel är skrivna i C++."
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
ms.openlocfilehash: 85e4d95549ca5edd375f3b15971634e032a3962a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-queue-storage-from-c"></a><span data-ttu-id="e7cbb-104">Använda Queue Storage från C++</span><span class="sxs-lookup"><span data-stu-id="e7cbb-104">How to use Queue Storage from C++</span></span>
[!INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="e7cbb-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="e7cbb-105">Overview</span></span>
<span data-ttu-id="e7cbb-106">Den här guiden visar hur du utför vanliga scenarier med hjälp av Azure Queue storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-106">This guide will show you how to perform common scenarios using the Azure Queue storage service.</span></span> <span data-ttu-id="e7cbb-107">Exemplen är skrivna i C++ och Använd den [Azure Storage-klientbibliotek för C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="e7cbb-107">The samples are written in C++ and use the [Azure Storage Client Library for C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span></span> <span data-ttu-id="e7cbb-108">Scenarier som tas upp inkluderar **infoga**, **granskning**, **komma**, och **bort** kö meddelanden, samt **skapa och ta bort köer**.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-108">The scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating and deleting queues**.</span></span>

> [!NOTE]
> <span data-ttu-id="e7cbb-109">Den här handboken riktar sig mot Azure Storage-klientbibliotek för C++ version 1.0.0 och senare.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-109">This guide targets the Azure Storage Client Library for C++ version 1.0.0 and above.</span></span> <span data-ttu-id="e7cbb-110">Den rekommenderade versionen är Storage-klientbibliotek 2.2.0, som är tillgängliga via [NuGet](http://www.nuget.org/packages/wastorage) eller [GitHub](http://github.com/Azure/azure-storage-cpp/).</span><span class="sxs-lookup"><span data-stu-id="e7cbb-110">The recommended version is Storage Client Library 2.2.0, which is available via [NuGet](http://www.nuget.org/packages/wastorage) or [GitHub](http://github.com/Azure/azure-storage-cpp/).</span></span>
> 
> 

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a><span data-ttu-id="e7cbb-111">Skapa ett C++-program</span><span class="sxs-lookup"><span data-stu-id="e7cbb-111">Create a C++ application</span></span>
<span data-ttu-id="e7cbb-112">I den här guiden använder lagringsfunktioner som kan köras i ett C++-program.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-112">In this guide, you will use storage features which can be run within a C++ application.</span></span>

<span data-ttu-id="e7cbb-113">Om du vill göra det, behöver du installera Azure Storage-klientbibliotek för C++ och skapa ett Azure storage-konto i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-113">To do so, you will need to install the Azure Storage Client Library for C++ and create an Azure storage account in your Azure subscription.</span></span>

<span data-ttu-id="e7cbb-114">Om du vill installera Azure Storage-klientbibliotek för C++ kan du använda följande metoder:</span><span class="sxs-lookup"><span data-stu-id="e7cbb-114">To install the Azure Storage Client Library for C++, you can use the following methods:</span></span>

* <span data-ttu-id="e7cbb-115">**Linux:** Följ instruktionerna den [Azure Storage-klientbibliotek för C++ viktigt](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) sidan.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-115">**Linux:** Follow the instructions given in the [Azure Storage Client Library for C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) page.</span></span>
* <span data-ttu-id="e7cbb-116">**Windows:** i Visual Studio klickar du på **Verktyg > NuGet Package Manager > Package Manager-konsolen**.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-116">**Windows:** In Visual Studio, click **Tools > NuGet Package Manager > Package Manager Console**.</span></span> <span data-ttu-id="e7cbb-117">Skriv följande kommando i den [NuGet Package Manager-konsolen](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) och tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-117">Type the following command into the [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) and press **ENTER**.</span></span>

```  
Install-Package wastorage
```

## <a name="configure-your-application-to-access-queue-storage"></a><span data-ttu-id="e7cbb-118">Konfigurera ditt program för att komma åt Queue Storage</span><span class="sxs-lookup"><span data-stu-id="e7cbb-118">Configure your application to access Queue Storage</span></span>
<span data-ttu-id="e7cbb-119">Lägga till följande uttryck överst i filen C++ där du vill använda Azure storage API: er för att komma åt köer:</span><span class="sxs-lookup"><span data-stu-id="e7cbb-119">Add the following include statements to the top of the C++ file where you want to use the Azure storage APIs to access queues:</span></span>  

```cpp
#include <was/storage_account.h>
#include <was/queue.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="e7cbb-120">Ställ in en anslutningssträng för Azure storage</span><span class="sxs-lookup"><span data-stu-id="e7cbb-120">Set up an Azure storage connection string</span></span>
<span data-ttu-id="e7cbb-121">Ett Azure storage-klienten använder en anslutningssträng för lagring för att lagra slutpunkter och autentiseringsuppgifter för åtkomst till data management services.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-121">An Azure storage client uses a storage connection string to store endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="e7cbb-122">När den körs i ett klientprogram, du måste ange anslutningssträngen för lagring i följande format, med hjälp av namnet på ditt lagringskonto och åtkomstnyckeln lagring för lagringskontot som anges i den [Azure Portal](https://portal.azure.com) för den *AccountName* och *AccountKey* värden.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-122">When running in a client application, you must provide the storage connection string in the following format, using the name of your storage account and the storage access key for the storage account listed in the [Azure Portal](https://portal.azure.com) for the *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="e7cbb-123">Information om lagringskonton och snabbtangenterna finns [om Azure Lagringskonton](storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="e7cbb-123">For information on storage accounts and access keys, see [About Azure Storage Accounts](storage-create-storage-account.md).</span></span> <span data-ttu-id="e7cbb-124">Det här exemplet visar hur du kan deklarera statiska fält att lagra anslutningssträngen:</span><span class="sxs-lookup"><span data-stu-id="e7cbb-124">This example shows how you can declare a static field to hold the connection string:</span></span>  

```cpp
// Define the connection-string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

<span data-ttu-id="e7cbb-125">Om du vill testa ditt program i din lokala Windows-dator, du kan använda Microsoft Azure [lagringsemulatorn](storage-use-emulator.md) som installeras med den [Azure SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="e7cbb-125">To test your application in your local Windows computer, you can use the Microsoft Azure [storage emulator](storage-use-emulator.md) that is installed with the [Azure SDK](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="e7cbb-126">Storage-emulatorn är ett verktyg som simulerar Blob, köer och tabellen tjänster som är tillgängliga i Azure på utvecklingsdatorn lokala.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-126">The storage emulator is a utility that simulates the Blob, Queue, and Table services available in Azure on your local development machine.</span></span> <span data-ttu-id="e7cbb-127">I följande exempel visas hur du kan deklarera statiska fält för anslutningssträngen till din lokala storage-emulatorn:</span><span class="sxs-lookup"><span data-stu-id="e7cbb-127">The following example shows how you can declare a static field to hold the connection string to your local storage emulator:</span></span>  

```cpp
// Define the connection-string with Azure Storage Emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

<span data-ttu-id="e7cbb-128">Om du vill starta Azure storage-emulatorn, Välj den **starta** knappen eller tryck på den **Windows** nyckel.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-128">To start the Azure storage emulator, select the **Start** button or press the **Windows** key.</span></span> <span data-ttu-id="e7cbb-129">Börja skriva **Azure Storage-emulatorn**, och välj **Microsoft Azure Storage-emulatorn** från listan med program.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-129">Begin typing **Azure Storage Emulator**, and select **Microsoft Azure Storage Emulator** from the list of applications.</span></span>

<span data-ttu-id="e7cbb-130">Följande exempel förutsätter att du har använt ett av dessa två sätt för att hämta anslutningssträngen för lagring.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-130">The following samples assume that you have used one of these two methods to get the storage connection string.</span></span>

## <a name="retrieve-your-connection-string"></a><span data-ttu-id="e7cbb-131">Hämta anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="e7cbb-131">Retrieve your connection string</span></span>
<span data-ttu-id="e7cbb-132">Du kan använda den **cloud_storage_account** klass för att representera kontoinformationen för lagring.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-132">You can use the **cloud_storage_account** class to represent your Storage Account information.</span></span> <span data-ttu-id="e7cbb-133">Du kan använda för att hämta information om ditt lagringskonto från anslutningssträngen för lagring av **parsa** metod.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-133">To retrieve your storage account information from the storage connection string, you can use the **parse** method.</span></span>

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

## <a name="how-to-create-a-queue"></a><span data-ttu-id="e7cbb-134">Så här: skapa en kö</span><span class="sxs-lookup"><span data-stu-id="e7cbb-134">How to: Create a queue</span></span>
<span data-ttu-id="e7cbb-135">En **cloud_queue_client** objekt kan du få referensobjekt för köer.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-135">A **cloud_queue_client** object lets you get reference objects for queues.</span></span> <span data-ttu-id="e7cbb-136">Följande kod skapar en **cloud_queue_client** objekt.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-136">The following code creates a **cloud_queue_client** object.</span></span>

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create a queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();
```

<span data-ttu-id="e7cbb-137">Använd den **cloud_queue_client** objekt för att hämta en referens till den kö som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-137">Use the **cloud_queue_client** object to get a reference to the queue you want to use.</span></span> <span data-ttu-id="e7cbb-138">Du kan skapa kön om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-138">You can create the queue if it doesn't exist.</span></span>

```cpp
// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Create the queue if it doesn't already exist.
 queue.create_if_not_exists();  
```

## <a name="how-to-insert-a-message-into-a-queue"></a><span data-ttu-id="e7cbb-139">Så här: Infoga ett meddelande i en kö</span><span class="sxs-lookup"><span data-stu-id="e7cbb-139">How to: Insert a message into a queue</span></span>
<span data-ttu-id="e7cbb-140">Om du vill infoga ett meddelande i en befintlig kö, först skapa en ny **cloud_queue_message**.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-140">To insert a message into an existing queue, first create a new **cloud_queue_message**.</span></span> <span data-ttu-id="e7cbb-141">Därefter anropar den **add_message** metod.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-141">Next, call the **add_message** method.</span></span> <span data-ttu-id="e7cbb-142">En **cloud_queue_message** kan skapas från antingen en sträng eller en **byte** matris.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-142">A **cloud_queue_message** can be created from either a string or a **byte** array.</span></span> <span data-ttu-id="e7cbb-143">Här är kod som skapar en kö (om den inte finns) och som infogar meddelandet ”Hello World”:</span><span class="sxs-lookup"><span data-stu-id="e7cbb-143">Here is code which creates a queue (if it doesn't exist) and inserts the message 'Hello, World':</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Create the queue if it doesn't already exist.
queue.create_if_not_exists();

// Create a message and add it to the queue.
azure::storage::cloud_queue_message message1(U("Hello, World"));
queue.add_message(message1);  
```

## <a name="how-to-peek-at-the-next-message"></a><span data-ttu-id="e7cbb-144">Så här: granska nästa meddelande</span><span class="sxs-lookup"><span data-stu-id="e7cbb-144">How to: Peek at the next message</span></span>
<span data-ttu-id="e7cbb-145">Du kan kika på meddelandet först i en kö utan att ta bort det från kön genom att anropa den **peek_message** metod.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-145">You can peek at the message in the front of a queue without removing it from the queue by calling the **peek_message** method.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Peek at the next message.
azure::storage::cloud_queue_message peeked_message = queue.peek_message();

// Output the message content.
std::wcout << U("Peeked message content: ") << peeked_message.content_as_string() << std::endl;
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a><span data-ttu-id="e7cbb-146">Så här: ändra innehållet i ett meddelande i kön</span><span class="sxs-lookup"><span data-stu-id="e7cbb-146">How to: Change the contents of a queued message</span></span>
<span data-ttu-id="e7cbb-147">Du kan ändra innehållet i ett meddelande direkt i kön.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-147">You can change the contents of a message in-place in the queue.</span></span> <span data-ttu-id="e7cbb-148">Om meddelandet representerar en arbetsuppgift kan du använda den här funktionen för att uppdatera arbetsuppgiftens status.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-148">If the message represents a work task, you could use this feature to update the status of the work task.</span></span> <span data-ttu-id="e7cbb-149">Följande kod uppdaterar kömeddelandet med nytt innehåll och utökar tidsgränsen för visning med ytterligare 60 sekunder.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-149">The following code updates the queue message with new contents, and sets the visibility timeout to extend another 60 seconds.</span></span> <span data-ttu-id="e7cbb-150">Koden sparar statusen för arbetsuppgiften som associeras med meddelandet och ger klienten ytterligare en minut att fortsätta arbeta med meddelandet.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-150">This saves the state of work associated with the message, and gives the client another minute to continue working on the message.</span></span> <span data-ttu-id="e7cbb-151">Du kan använda den här tekniken för att spåra arbetsflöden med flera steg i kömeddelanden, utan att behöva börja om från början om ett bearbetningssteg misslyckas på grund av maskin- eller programvarufel.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-151">You could use this technique to track multi-step workflows on queue messages, without having to start over from the beginning if a processing step fails due to hardware or software failure.</span></span> <span data-ttu-id="e7cbb-152">Normalt ska du behålla ett återförsöksvärde och om meddelandet försöks mer än n gånger, vill du ta bort den.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-152">Typically, you would keep a retry count as well, and if the message is retried more than n times, you would delete it.</span></span> <span data-ttu-id="e7cbb-153">Detta skyddar mot meddelanden som utlöser ett programfel varje gång de bearbetas.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-153">This protects against a message that triggers an application error each time it is processed.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_conection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Get the message from the queue and update the message contents.
// The visibility timeout "0" means make it visible immediately.
// The visibility timeout "60" means the client can get another minute to continue
// working on the message.
azure::storage::cloud_queue_message changed_message = queue.get_message();

changed_message.set_content(U("Changed message"));
queue.update_message(changed_message, std::chrono::seconds(60), true);

// Output the message content.
std::wcout << U("Changed message content: ") << changed_message.content_as_string() << std::endl;  
```

## <a name="how-to-de-queue-the-next-message"></a><span data-ttu-id="e7cbb-154">Så här: ta bort nästa meddelande från kön</span><span class="sxs-lookup"><span data-stu-id="e7cbb-154">How to: De-queue the next message</span></span>
<span data-ttu-id="e7cbb-155">Koden tar bort ett meddelande från en kö i två steg.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-155">Your code de-queues a message from a queue in two steps.</span></span> <span data-ttu-id="e7cbb-156">När du anropar **get_message**, du får nästa meddelande i en kö.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-156">When you call **get_message**, you get the next message in a queue.</span></span> <span data-ttu-id="e7cbb-157">Ett meddelande som returneras från **get_message** blir osynligt för andra koder som läsa meddelanden från den här kön.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-157">A message returned from **get_message** becomes invisible to any other code reading messages from this queue.</span></span> <span data-ttu-id="e7cbb-158">Om du vill slutföra borttagningen av meddelandet från kön, måste du också anropa **delete_message**.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-158">To finish removing the message from the queue, you must also call **delete_message**.</span></span> <span data-ttu-id="e7cbb-159">Den här tvåstegsprocessen för att ta bort ett meddelande säkerställer att om din kod inte kan bearbeta ett meddelande på grund av ett maskin- eller programvarufel så kan en annan instans av koden hämta samma meddelande och försöka igen.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-159">This two-step process of removing a message assures that if your code fails to process a message due to hardware or software failure, another instance of your code can get the same message and try again.</span></span> <span data-ttu-id="e7cbb-160">Koden anropar **delete_message** direkt efter att meddelandet har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-160">Your code calls **delete_message** right after the message has been processed.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Get the next message.
azure::storage::cloud_queue_message dequeued_message = queue.get_message();
std::wcout << U("Dequeued message: ") << dequeued_message.content_as_string() << std::endl;

// Delete the message.
queue.delete_message(dequeued_message);
```

## <a name="how-to-leverage-additional-options-for-de-queuing-messages"></a><span data-ttu-id="e7cbb-161">Så här: använda fler alternativ för meddelanden ur kön</span><span class="sxs-lookup"><span data-stu-id="e7cbb-161">How to: Leverage additional options for de-queuing messages</span></span>
<span data-ttu-id="e7cbb-162">Det finns två metoder som du kan använda för att anpassa meddelandehämtningen från en kö.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-162">There are two ways you can customize message retrieval from a queue.</span></span> <span data-ttu-id="e7cbb-163">För det första kan du hämta en grupp med meddelanden (upp till 32).</span><span class="sxs-lookup"><span data-stu-id="e7cbb-163">First, you can get a batch of messages (up to 32).</span></span> <span data-ttu-id="e7cbb-164">För det andra kan du ange en längre eller kortare tidsgräns för osynlighet för att ge koden mer eller mindre tid att bearbeta klart varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-164">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time to fully process each message.</span></span> <span data-ttu-id="e7cbb-165">Följande kodexempel används den **get_messages** metod för att hämta 20 meddelanden i ett anrop.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-165">The following code example uses the **get_messages** method to get 20 messages in one call.</span></span> <span data-ttu-id="e7cbb-166">Sedan bearbetas varje meddelande med hjälp av en **för** loop.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-166">Then it processes each message using a **for** loop.</span></span> <span data-ttu-id="e7cbb-167">Koden ställer också in tidsgränsen för osynlighet till fem minuter för varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-167">It also sets the invisibility timeout to five minutes for each message.</span></span> <span data-ttu-id="e7cbb-168">Observera att de fem minuterna startar för alla meddelanden samtidigt, efter 5 minuter har gått sedan anropet till **get_messages**, alla meddelanden som inte har tagits bort kommer att bli synliga igen.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-168">Note that the 5 minutes starts for all messages at the same time, so after 5 minutes have passed since the call to **get_messages**, any messages which have not been deleted will become visible again.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Dequeue some queue messages (maximum 32 at a time) and set their visibility timeout to
// 5 minutes (300 seconds).
azure::storage::queue_request_options options;
azure::storage::operation_context context;

// Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
std::vector<azure::storage::cloud_queue_message> messages = queue.get_messages(20, std::chrono::seconds(300), options, context);

for (auto it = messages.cbegin(); it != messages.cend(); ++it)
{
    // Display the contents of the message.
    std::wcout << U("Get: ") << it->content_as_string() << std::endl;
}
```

## <a name="how-to-get-the-queue-length"></a><span data-ttu-id="e7cbb-169">Så här: hämta kölängden</span><span class="sxs-lookup"><span data-stu-id="e7cbb-169">How to: Get the queue length</span></span>
<span data-ttu-id="e7cbb-170">Du kan hämta en uppskattning av antalet meddelanden i en kö.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-170">You can get an estimate of the number of messages in a queue.</span></span> <span data-ttu-id="e7cbb-171">Den **download_attributes** metoden ber kötjänsten att hämta köattributen, inklusive antalet meddelanden.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-171">The **download_attributes** method asks the Queue service to retrieve the queue attributes, including the message count.</span></span> <span data-ttu-id="e7cbb-172">Den **approximate_message_count** metoden hämtar det ungefärliga antalet meddelanden i kön.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-172">The **approximate_message_count** method gets the approximate number of messages in the queue.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Fetch the queue attributes.
queue.download_attributes();

// Retrieve the cached approximate message count.
int cachedMessageCount = queue.approximate_message_count();

// Display number of messages.
std::wcout << U("Number of messages in queue: ") << cachedMessageCount << std::endl;  
```

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="e7cbb-173">Så här: ta bort en kö</span><span class="sxs-lookup"><span data-stu-id="e7cbb-173">How to: Delete a queue</span></span>
<span data-ttu-id="e7cbb-174">Om du vill ta bort en kö och alla meddelanden som finns i den anropar den **delete_queue_if_exists** metoden för köobjektet.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-174">To delete a queue and all the messages contained in it, call the **delete_queue_if_exists** method on the queue object.</span></span>

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// If the queue exists and delete it.
queue.delete_queue_if_exists();  
```

## <a name="next-steps"></a><span data-ttu-id="e7cbb-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e7cbb-175">Next steps</span></span>
<span data-ttu-id="e7cbb-176">Nu när du har lärt dig grunderna i Queue storage kan du följa dessa länkar om du vill veta mer om Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="e7cbb-176">Now that you've learned the basics of Queue storage, follow these links to learn more about Azure Storage.</span></span>

* [<span data-ttu-id="e7cbb-177">Hur du använder Blob Storage från C++</span><span class="sxs-lookup"><span data-stu-id="e7cbb-177">How to use Blob Storage from C++</span></span>](storage-c-plus-plus-how-to-use-blobs.md)
* [<span data-ttu-id="e7cbb-178">Använda Table Storage från C++</span><span class="sxs-lookup"><span data-stu-id="e7cbb-178">How to use Table Storage from C++</span></span>](storage-c-plus-plus-how-to-use-tables.md)
* [<span data-ttu-id="e7cbb-179">Lista över Azure Storage-resurser i C++</span><span class="sxs-lookup"><span data-stu-id="e7cbb-179">List Azure Storage Resources in C++</span></span>](storage-c-plus-plus-enumeration.md)
* [<span data-ttu-id="e7cbb-180">Storage-klientbibliotek för C++-referens</span><span class="sxs-lookup"><span data-stu-id="e7cbb-180">Storage Client Library for C++ Reference</span></span>](http://azure.github.io/azure-storage-cpp)
* [<span data-ttu-id="e7cbb-181">Azure Storage-dokumentation</span><span class="sxs-lookup"><span data-stu-id="e7cbb-181">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)