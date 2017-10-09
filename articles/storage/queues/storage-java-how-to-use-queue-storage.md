---
title: "aaaHow toouse Queue storage från Java | Microsoft Docs"
description: "Lär dig hur toouse hello Azure Queue service toocreate och ta bort köer och infoga, hämta och ta bort meddelanden. Exempel skriven i Java."
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
ms.openlocfilehash: 297f89c9d21a38d2b4a5f4346f66f59f9d487010
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-java"></a><span data-ttu-id="510e6-104">Hur toouse Queue storage från Java</span><span class="sxs-lookup"><span data-stu-id="510e6-104">How toouse Queue storage from Java</span></span>
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../../includes/storage-check-out-samples-java.md)]

## <a name="overview"></a><span data-ttu-id="510e6-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="510e6-105">Overview</span></span>
<span data-ttu-id="510e6-106">Den här guiden visar hur tooperform vanliga scenarier med hjälp av hello Azure Queue storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="510e6-106">This guide will show you how tooperform common scenarios using hello Azure Queue storage service.</span></span> <span data-ttu-id="510e6-107">hello exemplen är skrivna i Java och använder hello [Azure Storage SDK för Java][Azure Storage SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="510e6-107">hello samples are written in Java and use hello [Azure Storage SDK for Java][Azure Storage SDK for Java].</span></span> <span data-ttu-id="510e6-108">hello beskrivs scenarier där **infoga**, **granskning**, **komma**, och **bort** kö meddelanden, samt  **Skapa** och **bort** köer.</span><span class="sxs-lookup"><span data-stu-id="510e6-108">hello scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating** and **deleting** queues.</span></span> <span data-ttu-id="510e6-109">Mer information om köer finns hello [nästa steg](#Next-Steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="510e6-109">For more information on queues, see hello [Next steps](#Next-Steps) section.</span></span>

<span data-ttu-id="510e6-110">Obs: En SDK är tillgänglig för utvecklare som använder Azure Storage på Android-enheter.</span><span class="sxs-lookup"><span data-stu-id="510e6-110">Note: An SDK is available for developers who are using Azure Storage on Android devices.</span></span> <span data-ttu-id="510e6-111">Mer information finns i hello [Azure Storage SDK för Android][Azure Storage SDK for Android].</span><span class="sxs-lookup"><span data-stu-id="510e6-111">For more information, see hello [Azure Storage SDK for Android][Azure Storage SDK for Android].</span></span>

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a><span data-ttu-id="510e6-112">Skapa ett Java-program</span><span class="sxs-lookup"><span data-stu-id="510e6-112">Create a Java application</span></span>
<span data-ttu-id="510e6-113">I den här guiden använder lagringsfunktioner som kan köras i en Java-program lokalt eller i kod som körs i en webbroll eller worker-rollen i Azure.</span><span class="sxs-lookup"><span data-stu-id="510e6-113">In this guide, you will use storage features which can be run within a Java application locally, or in code running within a web role or worker role in Azure.</span></span>

<span data-ttu-id="510e6-114">toodo så behöver du tooinstall hello Java Development Kit (JDK) och skapa ett Azure storage-konto i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="510e6-114">toodo so, you will need tooinstall hello Java Development Kit (JDK) and create an Azure storage account in your Azure subscription.</span></span> <span data-ttu-id="510e6-115">När du har gjort det, måste tooverify utvecklingssystemet uppfyller minimikraven för hello och beroenden som är listade i hello [Azure Storage SDK för Java] [ Azure Storage SDK for Java] databasen på GitHub.</span><span class="sxs-lookup"><span data-stu-id="510e6-115">Once you have done so, you will need tooverify that your development system meets hello minimum requirements and dependencies which are listed in hello [Azure Storage SDK for Java][Azure Storage SDK for Java] repository on GitHub.</span></span> <span data-ttu-id="510e6-116">Om datorn uppfyller dessa krav, kan du följa hello instruktioner för att hämta och installera hello Azure Storage-biblioteken för Java på datorn från den lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="510e6-116">If your system meets those requirements, you can follow hello instructions for downloading and installing hello Azure Storage Libraries for Java on your system from that repository.</span></span> <span data-ttu-id="510e6-117">När du har slutfört uppgifterna, kommer du att kunna toocreate ett Java-program som använder hello exemplen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="510e6-117">Once you have completed those tasks, you will be able toocreate a Java application which uses hello examples in this article.</span></span>

## <a name="configure-your-application-tooaccess-queue-storage"></a><span data-ttu-id="510e6-118">Konfigurera programmet tooaccess kön lagringen</span><span class="sxs-lookup"><span data-stu-id="510e6-118">Configure your application tooaccess queue storage</span></span>
<span data-ttu-id="510e6-119">Lägg till följande importera instruktioner toohello överkant hello Java filen där du vill toouse Azure storage-API: er tooaccess köer hello:</span><span class="sxs-lookup"><span data-stu-id="510e6-119">Add hello following import statements toohello top of hello Java file where you want toouse Azure storage APIs tooaccess queues:</span></span>

```java
// Include hello following imports toouse queue APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.queue.*;
```

## <a name="setup-an-azure-storage-connection-string"></a><span data-ttu-id="510e6-120">Ställ in en anslutningssträng för Azure storage</span><span class="sxs-lookup"><span data-stu-id="510e6-120">Setup an Azure storage connection string</span></span>
<span data-ttu-id="510e6-121">Ett Azure storage-klienten använder en lagring sträng toostore slutpunkter och autentiseringsuppgifter för åtkomst till data management services.</span><span class="sxs-lookup"><span data-stu-id="510e6-121">An Azure storage client uses a storage connection string toostore endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="510e6-122">När den körs i ett klientprogram, måste du ange hello lagringsanslutningssträngen i hello följande format, med hello namnet på ditt lagringskonto och hello primärnyckeln för hello storage-konto som anges i hello [Azure Portal](https://portal.azure.com)för hello *AccountName* och *AccountKey* värden.</span><span class="sxs-lookup"><span data-stu-id="510e6-122">When running in a client application, you must provide hello storage connection string in hello following format, using hello name of your storage account and hello Primary access key for hello storage account listed in hello [Azure Portal](https://portal.azure.com) for hello *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="510e6-123">Det här exemplet visar hur du kan deklarera en anslutningssträng för statiska fält toohold hello:</span><span class="sxs-lookup"><span data-stu-id="510e6-123">This example shows how you can declare a static field toohold hello connection string:</span></span>

```java
// Define hello connection-string with your values.
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

<span data-ttu-id="510e6-124">I ett program som körs inom en roll i Microsoft Azure, kan den här strängen lagras i hello tjänstkonfigurationsfilen *ServiceConfiguration.cscfg*, och kan nås med ett samtal toohello  **RoleEnvironment.getConfigurationSettings** metod.</span><span class="sxs-lookup"><span data-stu-id="510e6-124">In an application running within a role in Microsoft Azure, this string can be stored in hello service configuration file, *ServiceConfiguration.cscfg*, and can be accessed with a call toohello **RoleEnvironment.getConfigurationSettings** method.</span></span> <span data-ttu-id="510e6-125">Här är ett exempel på hello anslutningssträngen från en **inställningen** element med namnet *StorageConnectionString* i konfigurationsfilen för hello-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="510e6-125">Here's an example of getting hello connection string from a **Setting** element named *StorageConnectionString* in hello service configuration file:</span></span>

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

<span data-ttu-id="510e6-126">hello förutsätter följande exempel att du har använt någon av dessa två metoder tooget hello lagringsanslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="510e6-126">hello following samples assume that you have used one of these two methods tooget hello storage connection string.</span></span>

## <a name="how-to-create-a-queue"></a><span data-ttu-id="510e6-127">Så här: skapa en kö</span><span class="sxs-lookup"><span data-stu-id="510e6-127">How to: Create a queue</span></span>
<span data-ttu-id="510e6-128">En **CloudQueueClient** objekt kan du få referensobjekt för köer.</span><span class="sxs-lookup"><span data-stu-id="510e6-128">A **CloudQueueClient** object lets you get reference objects for queues.</span></span> <span data-ttu-id="510e6-129">hello följande kod skapar en **CloudQueueClient** objekt.</span><span class="sxs-lookup"><span data-stu-id="510e6-129">hello following code creates a **CloudQueueClient** object.</span></span> <span data-ttu-id="510e6-130">(Observera: det finns andra sätt toocreate **CloudStorageAccount** objekt; mer information finns i **CloudStorageAccount** i hello [referens för Azure Storage Client SDK].)</span><span class="sxs-lookup"><span data-stu-id="510e6-130">(Note: There are additional ways toocreate **CloudStorageAccount** objects; for more information, see **CloudStorageAccount** in hello [Azure Storage Client SDK Reference].)</span></span>

<span data-ttu-id="510e6-131">Använd hello **CloudQueueClient** objekt tooget en referens toohello kö som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="510e6-131">Use hello **CloudQueueClient** object tooget a reference toohello queue you want toouse.</span></span> <span data-ttu-id="510e6-132">Du kan skapa hello kö om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="510e6-132">You can create hello queue if it doesn't exist.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

   // Create hello queue client.
   CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

   // Retrieve a reference tooa queue.
   CloudQueue queue = queueClient.getQueueReference("myqueue");

   // Create hello queue if it doesn't already exist.
   queue.createIfNotExists();
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-add-a-message-tooa-queue"></a><span data-ttu-id="510e6-133">Så här: Lägg till en meddelandekö tooa</span><span class="sxs-lookup"><span data-stu-id="510e6-133">How to: Add a message tooa queue</span></span>
<span data-ttu-id="510e6-134">tooinsert ett meddelande i en befintlig kö först skapa en ny **CloudQueueMessage**.</span><span class="sxs-lookup"><span data-stu-id="510e6-134">tooinsert a message into an existing queue, first create a new **CloudQueueMessage**.</span></span> <span data-ttu-id="510e6-135">Därefter anropar hello **addMessage** metod.</span><span class="sxs-lookup"><span data-stu-id="510e6-135">Next, call hello **addMessage** method.</span></span> <span data-ttu-id="510e6-136">En **CloudQueueMessage** kan skapas från en sträng (i UTF-8-format) eller en byte-matris.</span><span class="sxs-lookup"><span data-stu-id="510e6-136">A **CloudQueueMessage** can be created from either a string (in UTF-8 format) or a byte array.</span></span> <span data-ttu-id="510e6-137">Här är kod som skapar en kö (om den inte finns) och infogningar hello-meddelande ”Hello World”.</span><span class="sxs-lookup"><span data-stu-id="510e6-137">Here is code which creates a queue (if it doesn't exist) and inserts hello message "Hello, World".</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference tooa queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Create hello queue if it doesn't already exist.
    queue.createIfNotExists();

    // Create a message and add it toohello queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    queue.addMessage(message);
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-peek-at-hello-next-message"></a><span data-ttu-id="510e6-138">Så här: granska nästa hello-meddelande</span><span class="sxs-lookup"><span data-stu-id="510e6-138">How to: Peek at hello next message</span></span>
<span data-ttu-id="510e6-139">Du kan kika på hello-meddelande i hello framför en kö utan att ta bort den från hello kö genom att anropa **peekMessage**.</span><span class="sxs-lookup"><span data-stu-id="510e6-139">You can peek at hello message in hello front of a queue without removing it from hello queue by calling **peekMessage**.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference tooa queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Peek at hello next message.
    CloudQueueMessage peekedMessage = queue.peekMessage();

    // Output hello message value.
    if (peekedMessage != null)
    {
      System.out.println(peekedMessage.getMessageContentAsString());
   }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-change-hello-contents-of-a-queued-message"></a><span data-ttu-id="510e6-140">Så här: ändra hello innehållet i ett meddelande i kön</span><span class="sxs-lookup"><span data-stu-id="510e6-140">How to: Change hello contents of a queued message</span></span>
<span data-ttu-id="510e6-141">Du kan ändra hello innehållet i ett meddelande direkt i hello kö.</span><span class="sxs-lookup"><span data-stu-id="510e6-141">You can change hello contents of a message in-place in hello queue.</span></span> <span data-ttu-id="510e6-142">Om hello-meddelande representerar en arbetsuppgift, kan du använda den här funktionen tooupdate hello hello arbete aktivitetens status.</span><span class="sxs-lookup"><span data-stu-id="510e6-142">If hello message represents a work task, you could use this feature tooupdate hello status of hello work task.</span></span> <span data-ttu-id="510e6-143">hello följande kod uppdaterar hello kömeddelandet med nytt innehåll och anger hello synlighet timeout tooextend ytterligare 60 sekunder.</span><span class="sxs-lookup"><span data-stu-id="510e6-143">hello following code updates hello queue message with new contents, and sets hello visibility timeout tooextend another 60 seconds.</span></span> <span data-ttu-id="510e6-144">Detta sparar hello statusen för arbetsuppgiften som associeras med hello-meddelande och ger hello klienten en annan minut toocontinue som arbetar på hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="510e6-144">This saves hello state of work associated with hello message, and gives hello client another minute toocontinue working on hello message.</span></span> <span data-ttu-id="510e6-145">Du kan använda den här tekniken tootrack arbetsflöden med flera steg i Kömeddelanden, utan att behöva toostart över från hello början om ett bearbetningssteg misslyckas på grund av toohardware eller ett programvarufel.</span><span class="sxs-lookup"><span data-stu-id="510e6-145">You could use this technique tootrack multi-step workflows on queue messages, without having toostart over from hello beginning if a processing step fails due toohardware or software failure.</span></span> <span data-ttu-id="510e6-146">Normalt ska du behålla ett återförsöksvärde och om hello meddelandet försöks mer än  *n*  tider, tar du bort den.</span><span class="sxs-lookup"><span data-stu-id="510e6-146">Typically, you would keep a retry count as well, and if hello message is retried more than *n* times, you would delete it.</span></span> <span data-ttu-id="510e6-147">Detta skyddar mot meddelanden som utlöser ett programfel varje gång de bearbetas.</span><span class="sxs-lookup"><span data-stu-id="510e6-147">This protects against a message that triggers an application error each time it is processed.</span></span>

<span data-ttu-id="510e6-148">hello följande kod exempel söker igenom hello meddelandekö, söker efter första hello-meddelande som matchar ”Hello World” för hello innehåll, och sedan ändrar hello-meddelande innehåll och avslutas.</span><span class="sxs-lookup"><span data-stu-id="510e6-148">hello following code sample searches through hello queue of messages, locates hello first message that matches "Hello, World" for hello content, then modifies hello message content and exits.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference tooa queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // hello maximum number of messages that can be retrieved is 32.
    final int MAX_NUMBER_OF_MESSAGES_TO_PEEK = 32;

    // Loop through hello messages in hello queue.
    for (CloudQueueMessage message : queue.retrieveMessages(MAX_NUMBER_OF_MESSAGES_TO_PEEK,1,null,null))
    {
        // Check for a specific string.
        if (message.getMessageContentAsString().equals("Hello, World"))
        {
            // Modify hello content of hello first matching message.
            message.setMessageContent("Updated contents.");
            // Set it toobe visible in 30 seconds.
            EnumSet<MessageUpdateFields> updateFields =
                EnumSet.of(MessageUpdateFields.CONTENT,
                MessageUpdateFields.VISIBILITY);
            // Update hello message.
            queue.updateMessage(message, 30, updateFields, null, null);
            break;
        }
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

<span data-ttu-id="510e6-149">Du kan också uppdateras hello följande kodexempel bara första synliga hälsningsmeddelande hello kön.</span><span class="sxs-lookup"><span data-stu-id="510e6-149">Alternatively, hello following code sample updates just hello first visible message on hello queue.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference tooa queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve hello first visible message in hello queue.
    CloudQueueMessage message = queue.retrieveMessage();

    if (message != null)
    {
        // Modify hello message content.
        message.setMessageContent("Updated contents.");
        // Set it toobe visible in 60 seconds.
        EnumSet<MessageUpdateFields> updateFields =
            EnumSet.of(MessageUpdateFields.CONTENT,
            MessageUpdateFields.VISIBILITY);
        // Update hello message.
        queue.updateMessage(message, 60, updateFields, null, null);
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-get-hello-queue-length"></a><span data-ttu-id="510e6-150">Så här: hämta hello Kölängd</span><span class="sxs-lookup"><span data-stu-id="510e6-150">How to: Get hello queue length</span></span>
<span data-ttu-id="510e6-151">Du kan få en uppskattning av hello antal meddelanden i en kö.</span><span class="sxs-lookup"><span data-stu-id="510e6-151">You can get an estimate of hello number of messages in a queue.</span></span> <span data-ttu-id="510e6-152">Hej **downloadAttributes** metoden ber hello Kötjänsten för flera aktuella värden, inklusive hur många meddelanden som finns i en kö.</span><span class="sxs-lookup"><span data-stu-id="510e6-152">hello **downloadAttributes** method asks hello Queue service for several current values, including a count of how many messages are in a queue.</span></span> <span data-ttu-id="510e6-153">hello returneras endast ungefärliga eftersom meddelanden kan läggas till eller tas bort efter hello kötjänsten svarar tooyour begäran.</span><span class="sxs-lookup"><span data-stu-id="510e6-153">hello count is only approximate because messages can be added or removed after hello Queue service responds tooyour request.</span></span> <span data-ttu-id="510e6-154">Hej **getApproximateMessageCount** metoden returnerar hello sista värdet som hämtas av hello anrop för**downloadAttributes**, utan att kötjänsten hello.</span><span class="sxs-lookup"><span data-stu-id="510e6-154">hello **getApproximateMessageCount** method returns hello last value retrieved by hello call too**downloadAttributes**, without calling hello Queue service.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference tooa queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

   // Download hello approximate message count from hello server.
    queue.downloadAttributes();

    // Retrieve hello newly cached approximate message count.
    long cachedMessageCount = queue.getApproximateMessageCount();

    // Display hello queue length.
    System.out.println(String.format("Queue length: %d", cachedMessageCount));
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-dequeue-hello-next-message"></a><span data-ttu-id="510e6-155">Så här: status Created nästa hello-meddelande</span><span class="sxs-lookup"><span data-stu-id="510e6-155">How to: Dequeue hello next message</span></span>
<span data-ttu-id="510e6-156">Koden dequeues ett meddelande från en kö i två steg.</span><span class="sxs-lookup"><span data-stu-id="510e6-156">Your code dequeues a message from a queue in two steps.</span></span> <span data-ttu-id="510e6-157">När du anropar **retrieveMessage**, du får hello nästa meddelande i en kö.</span><span class="sxs-lookup"><span data-stu-id="510e6-157">When you call **retrieveMessage**, you get hello next message in a queue.</span></span> <span data-ttu-id="510e6-158">Ett meddelande som returneras från **retrieveMessage** blir osynligt tooany annan kod läsa meddelanden från den här kön.</span><span class="sxs-lookup"><span data-stu-id="510e6-158">A message returned from **retrieveMessage** becomes invisible tooany other code reading messages from this queue.</span></span> <span data-ttu-id="510e6-159">Som standard är det här meddelandet osynligt i 30 sekunder.</span><span class="sxs-lookup"><span data-stu-id="510e6-159">By default, this message stays invisible for 30 seconds.</span></span> <span data-ttu-id="510e6-160">toofinish att ta bort hello-meddelande från kön hello, måste du också anropa **deleteMessage**.</span><span class="sxs-lookup"><span data-stu-id="510e6-160">toofinish removing hello message from hello queue, you must also call **deleteMessage**.</span></span> <span data-ttu-id="510e6-161">Den här tvåstegsprocessen för att ta bort ett meddelande säkerställer som om din kod inte tooprocess får ett meddelande på grund av fel toohardware eller programvara, en annan instans av koden hello samma meddelande och försök igen.</span><span class="sxs-lookup"><span data-stu-id="510e6-161">This two-step process of removing a message assures that if your code fails tooprocess a message due toohardware or software failure, another instance of your code can get hello same message and try again.</span></span> <span data-ttu-id="510e6-162">Koden anropar **deleteMessage** direkt efter hello-meddelande har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="510e6-162">Your code calls **deleteMessage** right after hello message has been processed.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference tooa queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve hello first visible message in hello queue.
    CloudQueueMessage retrievedMessage = queue.retrieveMessage();

    if (retrievedMessage != null)
    {
        // Process hello message in less than 30 seconds, and then delete hello message.
        queue.deleteMessage(retrievedMessage);
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="additional-options-for-dequeuing-messages"></a><span data-ttu-id="510e6-163">Ytterligare alternativ för mellan köer meddelanden</span><span class="sxs-lookup"><span data-stu-id="510e6-163">Additional options for dequeuing messages</span></span>
<span data-ttu-id="510e6-164">Det finns två metoder som du kan använda för att anpassa meddelandehämtningen från en kö.</span><span class="sxs-lookup"><span data-stu-id="510e6-164">There are two ways you can customize message retrieval from a queue.</span></span> <span data-ttu-id="510e6-165">Först får du en grupp med meddelanden (upp too32).</span><span class="sxs-lookup"><span data-stu-id="510e6-165">First, you can get a batch of messages (up too32).</span></span> <span data-ttu-id="510e6-166">Andra, du kan ange en tidsgräns för osynlighet längre eller kortare för att ge koden mer eller mindre tid toofully bearbeta varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="510e6-166">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time toofully process each message.</span></span>

<span data-ttu-id="510e6-167">hello följande kodexempel används hello **retrieveMessages** metoden tooget 20 meddelanden i ett anrop.</span><span class="sxs-lookup"><span data-stu-id="510e6-167">hello following code example uses hello **retrieveMessages** method tooget 20 messages in one call.</span></span> <span data-ttu-id="510e6-168">Sedan bearbetas varje meddelande med hjälp av en **för** loop.</span><span class="sxs-lookup"><span data-stu-id="510e6-168">Then it processes each message using a **for** loop.</span></span> <span data-ttu-id="510e6-169">Den anger också hello osynlighet timeout toofive minuter (300 sekunder) för varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="510e6-169">It also sets hello invisibility timeout toofive minutes (300 seconds) for each message.</span></span> <span data-ttu-id="510e6-170">Observera att hello fem minuterna startar för alla meddelanden på hello samma tid, så när fem minuter sedan hello anropet för**retrieveMessages**, alla meddelanden som inte har tagits bort kommer att bli synliga igen.</span><span class="sxs-lookup"><span data-stu-id="510e6-170">Note that hello five minutes starts for all messages at hello same time, so when five minutes have passed since hello call too**retrieveMessages**, any messages which have not been deleted will become visible again.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference tooa queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve 20 messages from hello queue with a visibility timeout of 300 seconds.
    for (CloudQueueMessage message : queue.retrieveMessages(20, 300, null, null)) {
        // Do processing for all messages in less than 5 minutes,
        // deleting each message after processing.
        queue.deleteMessage(message);
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-list-hello-queues"></a><span data-ttu-id="510e6-171">Så här: listan hello köer</span><span class="sxs-lookup"><span data-stu-id="510e6-171">How to: List hello queues</span></span>
<span data-ttu-id="510e6-172">tooobtain en lista över hello aktuella köer, anrop hello **CloudQueueClient.listQueues()** metod som returnerar en mängd **CloudQueue** objekt.</span><span class="sxs-lookup"><span data-stu-id="510e6-172">tooobtain a list of hello current queues, call hello **CloudQueueClient.listQueues()** method, which will return a collection of **CloudQueue** objects.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient =
        storageAccount.createCloudQueueClient();

    // Loop through hello collection of queues.
    for (CloudQueue queue : queueClient.listQueues())
    {
        // Output each queue name.
        System.out.println(queue.getName());
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="510e6-173">Så här: ta bort en kö</span><span class="sxs-lookup"><span data-stu-id="510e6-173">How to: Delete a queue</span></span>
<span data-ttu-id="510e6-174">toodelete en kö och alla hälsningsmeddelande som ingår i den anropet hello **deleteIfExists** metod på hello **CloudQueue** objekt.</span><span class="sxs-lookup"><span data-stu-id="510e6-174">toodelete a queue and all hello messages contained in it, call hello **deleteIfExists** method on hello **CloudQueue** object.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference tooa queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Delete hello queue if it exists.
    queue.deleteIfExists();
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="next-steps"></a><span data-ttu-id="510e6-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="510e6-175">Next steps</span></span>
<span data-ttu-id="510e6-176">Nu när du har lärt dig hello grunderna i queue storage kan du följa dessa länkar toolearn om mer komplexa lagringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="510e6-176">Now that you've learned hello basics of queue storage, follow these links toolearn about more complex storage tasks.</span></span>

* <span data-ttu-id="510e6-177">[Azure Storage SDK för Java][Azure Storage SDK for Java]</span><span class="sxs-lookup"><span data-stu-id="510e6-177">[Azure Storage SDK for Java][Azure Storage SDK for Java]</span></span>
* <span data-ttu-id="510e6-178">[Referens för Azure Storage Client SDK][referens för Azure Storage Client SDK]</span><span class="sxs-lookup"><span data-stu-id="510e6-178">[Azure Storage Client SDK Reference][Azure Storage Client SDK Reference]</span></span>
* <span data-ttu-id="510e6-179">[Azure Storage Services REST API][Azure Storage Services REST API]</span><span class="sxs-lookup"><span data-stu-id="510e6-179">[Azure Storage Services REST API][Azure Storage Services REST API]</span></span>
* <span data-ttu-id="510e6-180">[Azure Storage-teamets blogg][Azure Storage Team Blog]</span><span class="sxs-lookup"><span data-stu-id="510e6-180">[Azure Storage Team Blog][Azure Storage Team Blog]</span></span>

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[referens för Azure Storage Client SDK]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage Services REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
