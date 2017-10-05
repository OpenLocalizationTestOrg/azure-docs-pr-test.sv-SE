---
title: "Använda Queue storage från Java | Microsoft Docs"
description: "Lär dig hur du använder Azure-Kötjänsten för att skapa och ta bort köer, infoga, hämta och ta bort meddelanden. Exempel skriven i Java."
services: storage
documentationcenter: java
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 68cecc8e-38c9-4a24-99e8-cb722bc63cf9
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: 03faea986221453d1862ff0f0d6d1ec21f92f3bb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-queue-storage-from-java"></a><span data-ttu-id="0edfe-104">Använda Queue Storage från Java</span><span class="sxs-lookup"><span data-stu-id="0edfe-104">How to use Queue storage from Java</span></span>
[!INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="overview"></a><span data-ttu-id="0edfe-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="0edfe-105">Overview</span></span>
<span data-ttu-id="0edfe-106">Den här guiden visar hur du utför vanliga scenarier med hjälp av Azure Queue storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="0edfe-106">This guide will show you how to perform common scenarios using the Azure Queue storage service.</span></span> <span data-ttu-id="0edfe-107">Exemplen är skrivna i Java och Använd den [Azure Storage SDK för Java][Azure Storage SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="0edfe-107">The samples are written in Java and use the [Azure Storage SDK for Java][Azure Storage SDK for Java].</span></span> <span data-ttu-id="0edfe-108">Scenarier som tas upp inkluderar **infoga**, **granskning**, **komma**, och **bort** kö meddelanden, samt  **Skapa** och **bort** köer.</span><span class="sxs-lookup"><span data-stu-id="0edfe-108">The scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating** and **deleting** queues.</span></span> <span data-ttu-id="0edfe-109">Mer information om köer finns i [nästa steg](#Next-Steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0edfe-109">For more information on queues, see the [Next steps](#Next-Steps) section.</span></span>

<span data-ttu-id="0edfe-110">Obs: En SDK är tillgänglig för utvecklare som använder Azure Storage på Android-enheter.</span><span class="sxs-lookup"><span data-stu-id="0edfe-110">Note: An SDK is available for developers who are using Azure Storage on Android devices.</span></span> <span data-ttu-id="0edfe-111">Mer information finns i [Azure Storage SDK för Android][Azure Storage SDK for Android].</span><span class="sxs-lookup"><span data-stu-id="0edfe-111">For more information, see the [Azure Storage SDK for Android][Azure Storage SDK for Android].</span></span>

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a><span data-ttu-id="0edfe-112">Skapa ett Java-program</span><span class="sxs-lookup"><span data-stu-id="0edfe-112">Create a Java application</span></span>
<span data-ttu-id="0edfe-113">I den här guiden använder lagringsfunktioner som kan köras i en Java-program lokalt eller i kod som körs i en webbroll eller worker-rollen i Azure.</span><span class="sxs-lookup"><span data-stu-id="0edfe-113">In this guide, you will use storage features which can be run within a Java application locally, or in code running within a web role or worker role in Azure.</span></span>

<span data-ttu-id="0edfe-114">Om du vill göra det, behöver du installera Java Development Kit (JDK) och skapa ett Azure storage-konto i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0edfe-114">To do so, you will need to install the Java Development Kit (JDK) and create an Azure storage account in your Azure subscription.</span></span> <span data-ttu-id="0edfe-115">När du har gjort det, behöver du kontrollera att utvecklingssystemet uppfyller minsta krav och beroenden som visas i den [Azure Storage SDK för Java] [ Azure Storage SDK for Java] databasen på GitHub.</span><span class="sxs-lookup"><span data-stu-id="0edfe-115">Once you have done so, you will need to verify that your development system meets the minimum requirements and dependencies which are listed in the [Azure Storage SDK for Java][Azure Storage SDK for Java] repository on GitHub.</span></span> <span data-ttu-id="0edfe-116">Om datorn uppfyller dessa krav, kan du följa instruktionerna för att hämta och installera Azure Storage-biblioteken för Java på datorn från den lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="0edfe-116">If your system meets those requirements, you can follow the instructions for downloading and installing the Azure Storage Libraries for Java on your system from that repository.</span></span> <span data-ttu-id="0edfe-117">När du har slutfört uppgifterna, kommer du att kunna skapa ett Java-program som använder exemplen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="0edfe-117">Once you have completed those tasks, you will be able to create a Java application which uses the examples in this article.</span></span>

## <a name="configure-your-application-to-access-queue-storage"></a><span data-ttu-id="0edfe-118">Konfigurera ditt program åtkomst till kön lagring</span><span class="sxs-lookup"><span data-stu-id="0edfe-118">Configure your application to access queue storage</span></span>
<span data-ttu-id="0edfe-119">Lägg till följande importuttryck överst i filen Java där du vill använda Azure storage API: er för att komma åt köer:</span><span class="sxs-lookup"><span data-stu-id="0edfe-119">Add the following import statements to the top of the Java file where you want to use Azure storage APIs to access queues:</span></span>

```java
// Include the following imports to use queue APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.queue.*;
```

## <a name="setup-an-azure-storage-connection-string"></a><span data-ttu-id="0edfe-120">Ställ in en anslutningssträng för Azure storage</span><span class="sxs-lookup"><span data-stu-id="0edfe-120">Setup an Azure storage connection string</span></span>
<span data-ttu-id="0edfe-121">Ett Azure storage-klienten använder en anslutningssträng för lagring för att lagra slutpunkter och autentiseringsuppgifter för åtkomst till data management services.</span><span class="sxs-lookup"><span data-stu-id="0edfe-121">An Azure storage client uses a storage connection string to store endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="0edfe-122">När den körs i ett klientprogram, du måste ange anslutningssträngen för lagring i följande format, med hjälp av namnet på ditt lagringskonto och den primära åtkomstnyckeln för lagringskontot som anges i den [Azure Portal](https://portal.azure.com) för den *AccountName* och *AccountKey* värden.</span><span class="sxs-lookup"><span data-stu-id="0edfe-122">When running in a client application, you must provide the storage connection string in the following format, using the name of your storage account and the Primary access key for the storage account listed in the [Azure Portal](https://portal.azure.com) for the *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="0edfe-123">Det här exemplet visar hur du kan deklarera statiska fält att lagra anslutningssträngen:</span><span class="sxs-lookup"><span data-stu-id="0edfe-123">This example shows how you can declare a static field to hold the connection string:</span></span>

```java
// Define the connection-string with your values.
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

<span data-ttu-id="0edfe-124">I ett program som körs inom en roll i Microsoft Azure, kan den här strängen lagras i konfigurationsfilen service *ServiceConfiguration.cscfg*, och kan nås med ett anrop till den **RoleEnvironment.getConfigurationSettings** metod.</span><span class="sxs-lookup"><span data-stu-id="0edfe-124">In an application running within a role in Microsoft Azure, this string can be stored in the service configuration file, *ServiceConfiguration.cscfg*, and can be accessed with a call to the **RoleEnvironment.getConfigurationSettings** method.</span></span> <span data-ttu-id="0edfe-125">Här är ett exempel för att få anslutningssträng från en **inställningen** element med namnet *StorageConnectionString* i tjänstkonfigurationsfilen:</span><span class="sxs-lookup"><span data-stu-id="0edfe-125">Here's an example of getting the connection string from a **Setting** element named *StorageConnectionString* in the service configuration file:</span></span>

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

<span data-ttu-id="0edfe-126">Följande exempel förutsätter att du har använt ett av dessa två sätt för att hämta anslutningssträngen för lagring.</span><span class="sxs-lookup"><span data-stu-id="0edfe-126">The following samples assume that you have used one of these two methods to get the storage connection string.</span></span>

## <a name="how-to-create-a-queue"></a><span data-ttu-id="0edfe-127">Så här: skapa en kö</span><span class="sxs-lookup"><span data-stu-id="0edfe-127">How to: Create a queue</span></span>
<span data-ttu-id="0edfe-128">En **CloudQueueClient** objekt kan du få referensobjekt för köer.</span><span class="sxs-lookup"><span data-stu-id="0edfe-128">A **CloudQueueClient** object lets you get reference objects for queues.</span></span> <span data-ttu-id="0edfe-129">Följande kod skapar en **CloudQueueClient** objekt.</span><span class="sxs-lookup"><span data-stu-id="0edfe-129">The following code creates a **CloudQueueClient** object.</span></span> <span data-ttu-id="0edfe-130">(Observera: det finns flera sätt att skapa **CloudStorageAccount** objekt; mer information finns **CloudStorageAccount** i den [referens för Azure Storage Client SDK].)</span><span class="sxs-lookup"><span data-stu-id="0edfe-130">(Note: There are additional ways to create **CloudStorageAccount** objects; for more information, see **CloudStorageAccount** in the [Azure Storage Client SDK Reference].)</span></span>

<span data-ttu-id="0edfe-131">Använd den **CloudQueueClient** objekt för att hämta en referens till den kö som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="0edfe-131">Use the **CloudQueueClient** object to get a reference to the queue you want to use.</span></span> <span data-ttu-id="0edfe-132">Du kan skapa kön om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="0edfe-132">You can create the queue if it doesn't exist.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

   // Create the queue client.
   CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

   // Retrieve a reference to a queue.
   CloudQueue queue = queueClient.getQueueReference("myqueue");

   // Create the queue if it doesn't already exist.
   queue.createIfNotExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-add-a-message-to-a-queue"></a><span data-ttu-id="0edfe-133">Så här: lägga till ett meddelande till en kö</span><span class="sxs-lookup"><span data-stu-id="0edfe-133">How to: Add a message to a queue</span></span>
<span data-ttu-id="0edfe-134">Om du vill infoga ett meddelande i en befintlig kö börjar du med att skapa ett nytt **CloudQueueMessage**.</span><span class="sxs-lookup"><span data-stu-id="0edfe-134">To insert a message into an existing queue, first create a new **CloudQueueMessage**.</span></span> <span data-ttu-id="0edfe-135">Därefter anropar den **addMessage** metod.</span><span class="sxs-lookup"><span data-stu-id="0edfe-135">Next, call the **addMessage** method.</span></span> <span data-ttu-id="0edfe-136">En **CloudQueueMessage** kan skapas från en sträng (i UTF-8-format) eller en byte-matris.</span><span class="sxs-lookup"><span data-stu-id="0edfe-136">A **CloudQueueMessage** can be created from either a string (in UTF-8 format) or a byte array.</span></span> <span data-ttu-id="0edfe-137">Här är kod som skapar en kö (om den inte finns) och som infogar meddelandet ”Hello, World”.</span><span class="sxs-lookup"><span data-stu-id="0edfe-137">Here is code which creates a queue (if it doesn't exist) and inserts the message "Hello, World".</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Create the queue if it doesn't already exist.
    queue.createIfNotExists();

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    queue.addMessage(message);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-peek-at-the-next-message"></a><span data-ttu-id="0edfe-138">Så här: granska nästa meddelande</span><span class="sxs-lookup"><span data-stu-id="0edfe-138">How to: Peek at the next message</span></span>
<span data-ttu-id="0edfe-139">Du kan kika på meddelandet först i en kö utan att ta bort det från kön genom att anropa **peekMessage**.</span><span class="sxs-lookup"><span data-stu-id="0edfe-139">You can peek at the message in the front of a queue without removing it from the queue by calling **peekMessage**.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Peek at the next message.
    CloudQueueMessage peekedMessage = queue.peekMessage();

    // Output the message value.
    if (peekedMessage != null)
    {
      System.out.println(peekedMessage.getMessageContentAsString());
   }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a><span data-ttu-id="0edfe-140">Så här: ändra innehållet i ett meddelande i kön</span><span class="sxs-lookup"><span data-stu-id="0edfe-140">How to: Change the contents of a queued message</span></span>
<span data-ttu-id="0edfe-141">Du kan ändra innehållet i ett meddelande direkt i kön.</span><span class="sxs-lookup"><span data-stu-id="0edfe-141">You can change the contents of a message in-place in the queue.</span></span> <span data-ttu-id="0edfe-142">Om meddelandet representerar en arbetsuppgift kan du använda den här funktionen för att uppdatera arbetsuppgiftens status.</span><span class="sxs-lookup"><span data-stu-id="0edfe-142">If the message represents a work task, you could use this feature to update the status of the work task.</span></span> <span data-ttu-id="0edfe-143">Följande kod uppdaterar kömeddelandet med nytt innehåll och utökar tidsgränsen för visning med ytterligare 60 sekunder.</span><span class="sxs-lookup"><span data-stu-id="0edfe-143">The following code updates the queue message with new contents, and sets the visibility timeout to extend another 60 seconds.</span></span> <span data-ttu-id="0edfe-144">Koden sparar statusen för arbetsuppgiften som associeras med meddelandet och ger klienten ytterligare en minut att fortsätta arbeta med meddelandet.</span><span class="sxs-lookup"><span data-stu-id="0edfe-144">This saves the state of work associated with the message, and gives the client another minute to continue working on the message.</span></span> <span data-ttu-id="0edfe-145">Du kan använda den här tekniken för att spåra arbetsflöden med flera steg i kömeddelanden, utan att behöva börja om från början om ett bearbetningssteg misslyckas på grund av maskin- eller programvarufel.</span><span class="sxs-lookup"><span data-stu-id="0edfe-145">You could use this technique to track multi-step workflows on queue messages, without having to start over from the beginning if a processing step fails due to hardware or software failure.</span></span> <span data-ttu-id="0edfe-146">Normalt räknar du även antalet omförsök och tar bort meddelandet om fler än *n* försök misslyckas.</span><span class="sxs-lookup"><span data-stu-id="0edfe-146">Typically, you would keep a retry count as well, and if the message is retried more than *n* times, you would delete it.</span></span> <span data-ttu-id="0edfe-147">Detta skyddar mot meddelanden som utlöser ett programfel varje gång de bearbetas.</span><span class="sxs-lookup"><span data-stu-id="0edfe-147">This protects against a message that triggers an application error each time it is processed.</span></span>

<span data-ttu-id="0edfe-148">Följande kod exempel söker igenom kön av meddelanden, söker efter det första meddelandet som matchar ”Hello World” för innehåll, och sedan ändrar meddelandet innehåll och avslutas.</span><span class="sxs-lookup"><span data-stu-id="0edfe-148">The following code sample searches through the queue of messages, locates the first message that matches "Hello, World" for the content, then modifies the message content and exits.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // The maximum number of messages that can be retrieved is 32.
    final int MAX_NUMBER_OF_MESSAGES_TO_PEEK = 32;

    // Loop through the messages in the queue.
    for (CloudQueueMessage message : queue.retrieveMessages(MAX_NUMBER_OF_MESSAGES_TO_PEEK,1,null,null))
    {
        // Check for a specific string.
        if (message.getMessageContentAsString().equals("Hello, World"))
        {
            // Modify the content of the first matching message.
            message.setMessageContent("Updated contents.");
            // Set it to be visible in 30 seconds.
            EnumSet<MessageUpdateFields> updateFields =
                EnumSet.of(MessageUpdateFields.CONTENT,
                MessageUpdateFields.VISIBILITY);
            // Update the message.
            queue.updateMessage(message, 30, updateFields, null, null);
            break;
        }
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

<span data-ttu-id="0edfe-149">Du kan också uppdateras följande kodexempel bara det första synliga meddelandet i kön.</span><span class="sxs-lookup"><span data-stu-id="0edfe-149">Alternatively, the following code sample updates just the first visible message on the queue.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve the first visible message in the queue.
    CloudQueueMessage message = queue.retrieveMessage();

    if (message != null)
    {
        // Modify the message content.
        message.setMessageContent("Updated contents.");
        // Set it to be visible in 60 seconds.
        EnumSet<MessageUpdateFields> updateFields =
            EnumSet.of(MessageUpdateFields.CONTENT,
            MessageUpdateFields.VISIBILITY);
        // Update the message.
        queue.updateMessage(message, 60, updateFields, null, null);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-get-the-queue-length"></a><span data-ttu-id="0edfe-150">Så här: hämta kölängden</span><span class="sxs-lookup"><span data-stu-id="0edfe-150">How to: Get the queue length</span></span>
<span data-ttu-id="0edfe-151">Du kan hämta en uppskattning av antalet meddelanden i en kö.</span><span class="sxs-lookup"><span data-stu-id="0edfe-151">You can get an estimate of the number of messages in a queue.</span></span> <span data-ttu-id="0edfe-152">Den **downloadAttributes** metoden ber Kötjänsten för flera aktuella värden, inklusive hur många meddelanden som finns i en kö.</span><span class="sxs-lookup"><span data-stu-id="0edfe-152">The **downloadAttributes** method asks the Queue service for several current values, including a count of how many messages are in a queue.</span></span> <span data-ttu-id="0edfe-153">Antalet är ungefärliga eftersom meddelanden kan läggas till eller tas bort efter kötjänsten svarar på begäran.</span><span class="sxs-lookup"><span data-stu-id="0edfe-153">The count is only approximate because messages can be added or removed after the Queue service responds to your request.</span></span> <span data-ttu-id="0edfe-154">Den **getApproximateMessageCount** metoden returnerar det sista värdet som hämtas av anrop till **downloadAttributes**, utan att kötjänsten.</span><span class="sxs-lookup"><span data-stu-id="0edfe-154">The **getApproximateMessageCount** method returns the last value retrieved by the call to **downloadAttributes**, without calling the Queue service.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

   // Download the approximate message count from the server.
    queue.downloadAttributes();

    // Retrieve the newly cached approximate message count.
    long cachedMessageCount = queue.getApproximateMessageCount();

    // Display the queue length.
    System.out.println(String.format("Queue length: %d", cachedMessageCount));
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-dequeue-the-next-message"></a><span data-ttu-id="0edfe-155">Så här: status Created nästa meddelande</span><span class="sxs-lookup"><span data-stu-id="0edfe-155">How to: Dequeue the next message</span></span>
<span data-ttu-id="0edfe-156">Koden dequeues ett meddelande från en kö i två steg.</span><span class="sxs-lookup"><span data-stu-id="0edfe-156">Your code dequeues a message from a queue in two steps.</span></span> <span data-ttu-id="0edfe-157">När du anropar **retrieveMessage**, du får nästa meddelande i en kö.</span><span class="sxs-lookup"><span data-stu-id="0edfe-157">When you call **retrieveMessage**, you get the next message in a queue.</span></span> <span data-ttu-id="0edfe-158">Ett meddelande som returneras från **retrieveMessage** blir osynligt för andra koder som läsa meddelanden från den här kön.</span><span class="sxs-lookup"><span data-stu-id="0edfe-158">A message returned from **retrieveMessage** becomes invisible to any other code reading messages from this queue.</span></span> <span data-ttu-id="0edfe-159">Som standard är det här meddelandet osynligt i 30 sekunder.</span><span class="sxs-lookup"><span data-stu-id="0edfe-159">By default, this message stays invisible for 30 seconds.</span></span> <span data-ttu-id="0edfe-160">Om du vill slutföra borttagningen av meddelandet från kön, måste du också anropa **deleteMessage**.</span><span class="sxs-lookup"><span data-stu-id="0edfe-160">To finish removing the message from the queue, you must also call **deleteMessage**.</span></span> <span data-ttu-id="0edfe-161">Den här tvåstegsprocessen för att ta bort ett meddelande säkerställer att om din kod inte kan bearbeta ett meddelande på grund av ett maskin- eller programvarufel så kan en annan instans av koden hämta samma meddelande och försöka igen.</span><span class="sxs-lookup"><span data-stu-id="0edfe-161">This two-step process of removing a message assures that if your code fails to process a message due to hardware or software failure, another instance of your code can get the same message and try again.</span></span> <span data-ttu-id="0edfe-162">Koden anropar **deleteMessage** direkt efter att meddelandet har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="0edfe-162">Your code calls **deleteMessage** right after the message has been processed.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve the first visible message in the queue.
    CloudQueueMessage retrievedMessage = queue.retrieveMessage();

    if (retrievedMessage != null)
    {
        // Process the message in less than 30 seconds, and then delete the message.
        queue.deleteMessage(retrievedMessage);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="additional-options-for-dequeuing-messages"></a><span data-ttu-id="0edfe-163">Ytterligare alternativ för mellan köer meddelanden</span><span class="sxs-lookup"><span data-stu-id="0edfe-163">Additional options for dequeuing messages</span></span>
<span data-ttu-id="0edfe-164">Det finns två metoder som du kan använda för att anpassa meddelandehämtningen från en kö.</span><span class="sxs-lookup"><span data-stu-id="0edfe-164">There are two ways you can customize message retrieval from a queue.</span></span> <span data-ttu-id="0edfe-165">För det första kan du hämta en grupp med meddelanden (upp till 32).</span><span class="sxs-lookup"><span data-stu-id="0edfe-165">First, you can get a batch of messages (up to 32).</span></span> <span data-ttu-id="0edfe-166">För det andra kan du ange en längre eller kortare tidsgräns för osynlighet för att ge koden mer eller mindre tid att bearbeta klart varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="0edfe-166">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time to fully process each message.</span></span>

<span data-ttu-id="0edfe-167">Följande kodexempel används den **retrieveMessages** metod för att hämta 20 meddelanden i ett anrop.</span><span class="sxs-lookup"><span data-stu-id="0edfe-167">The following code example uses the **retrieveMessages** method to get 20 messages in one call.</span></span> <span data-ttu-id="0edfe-168">Sedan bearbetas varje meddelande med hjälp av en **för** loop.</span><span class="sxs-lookup"><span data-stu-id="0edfe-168">Then it processes each message using a **for** loop.</span></span> <span data-ttu-id="0edfe-169">Den anger också tidsgränsen för osynlighet till fem minuter (300 sekunder) för varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="0edfe-169">It also sets the invisibility timeout to five minutes (300 seconds) for each message.</span></span> <span data-ttu-id="0edfe-170">Observera att de fem minuterna startar för alla meddelanden samtidigt, så när fem minuter sedan anropet till **retrieveMessages**, alla meddelanden som inte har tagits bort kommer att bli synliga igen.</span><span class="sxs-lookup"><span data-stu-id="0edfe-170">Note that the five minutes starts for all messages at the same time, so when five minutes have passed since the call to **retrieveMessages**, any messages which have not been deleted will become visible again.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
    for (CloudQueueMessage message : queue.retrieveMessages(20, 300, null, null)) {
        // Do processing for all messages in less than 5 minutes,
        // deleting each message after processing.
        queue.deleteMessage(message);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-list-the-queues"></a><span data-ttu-id="0edfe-171">Så här: visa en lista över köerna</span><span class="sxs-lookup"><span data-stu-id="0edfe-171">How to: List the queues</span></span>
<span data-ttu-id="0edfe-172">Om du vill hämta en lista över de aktuella köerna anropa den **CloudQueueClient.listQueues()** metod som returnerar en mängd **CloudQueue** objekt.</span><span class="sxs-lookup"><span data-stu-id="0edfe-172">To obtain a list of the current queues, call the **CloudQueueClient.listQueues()** method, which will return a collection of **CloudQueue** objects.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient =
        storageAccount.createCloudQueueClient();

    // Loop through the collection of queues.
    for (CloudQueue queue : queueClient.listQueues())
    {
        // Output each queue name.
        System.out.println(queue.getName());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="0edfe-173">Så här: ta bort en kö</span><span class="sxs-lookup"><span data-stu-id="0edfe-173">How to: Delete a queue</span></span>
<span data-ttu-id="0edfe-174">Om du vill ta bort en kö och alla meddelanden som finns i den anropar den **deleteIfExists** -metoden i den **CloudQueue** objekt.</span><span class="sxs-lookup"><span data-stu-id="0edfe-174">To delete a queue and all the messages contained in it, call the **deleteIfExists** method on the **CloudQueue** object.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Delete the queue if it exists.
    queue.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="next-steps"></a><span data-ttu-id="0edfe-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0edfe-175">Next steps</span></span>
<span data-ttu-id="0edfe-176">Nu när du har lärt dig grunderna i queue storage följa dessa länkar för att lära dig mer komplexa lagringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="0edfe-176">Now that you've learned the basics of queue storage, follow these links to learn about more complex storage tasks.</span></span>

* <span data-ttu-id="0edfe-177">[Azure Storage SDK för Java][Azure Storage SDK for Java]</span><span class="sxs-lookup"><span data-stu-id="0edfe-177">[Azure Storage SDK for Java][Azure Storage SDK for Java]</span></span>
* <span data-ttu-id="0edfe-178">[Referens för Azure Storage Client SDK][referens för Azure Storage Client SDK]</span><span class="sxs-lookup"><span data-stu-id="0edfe-178">[Azure Storage Client SDK Reference][Azure Storage Client SDK Reference]</span></span>
* <span data-ttu-id="0edfe-179">[Azure Storage Services REST API][Azure Storage Services REST API]</span><span class="sxs-lookup"><span data-stu-id="0edfe-179">[Azure Storage Services REST API][Azure Storage Services REST API]</span></span>
* <span data-ttu-id="0edfe-180">[Azure Storage-teamets blogg][Azure Storage Team Blog]</span><span class="sxs-lookup"><span data-stu-id="0edfe-180">[Azure Storage Team Blog][Azure Storage Team Blog]</span></span>

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
<span data-ttu-id="0edfe-181">[referens för Azure Storage Client SDK]: http://dl.windowsazure.com/storage/javadoc/</span><span class="sxs-lookup"><span data-stu-id="0edfe-181">[Azure Storage Client SDK Reference]: http://dl.windowsazure.com/storage/javadoc/</span></span>
[Azure Storage Services REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
