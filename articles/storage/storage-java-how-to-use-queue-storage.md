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
ms.openlocfilehash: c2d5211ec5b6454f7dbc126aad4ba9950df13661
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-java"></a>Hur toouse Queue storage från Java
[!INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="overview"></a>Översikt
Den här guiden visar hur tooperform vanliga scenarier med hjälp av hello Azure Queue storage-tjänst. hello exemplen är skrivna i Java och använder hello [Azure Storage SDK för Java][Azure Storage SDK for Java]. hello beskrivs scenarier där **infoga**, **granskning**, **komma**, och **bort** kö meddelanden, samt  **Skapa** och **bort** köer. Mer information om köer finns hello [nästa steg](#Next-Steps) avsnitt.

Obs: En SDK är tillgänglig för utvecklare som använder Azure Storage på Android-enheter. Mer information finns i hello [Azure Storage SDK för Android][Azure Storage SDK for Android].

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Skapa ett Java-program
I den här guiden använder lagringsfunktioner som kan köras i en Java-program lokalt eller i kod som körs i en webbroll eller worker-rollen i Azure.

toodo så behöver du tooinstall hello Java Development Kit (JDK) och skapa ett Azure storage-konto i din Azure-prenumeration. När du har gjort det, måste tooverify utvecklingssystemet uppfyller minimikraven för hello och beroenden som är listade i hello [Azure Storage SDK för Java] [ Azure Storage SDK for Java] databasen på GitHub. Om datorn uppfyller dessa krav, kan du följa hello instruktioner för att hämta och installera hello Azure Storage-biblioteken för Java på datorn från den lagringsplatsen. När du har slutfört uppgifterna, kommer du att kunna toocreate ett Java-program som använder hello exemplen i den här artikeln.

## <a name="configure-your-application-tooaccess-queue-storage"></a>Konfigurera programmet tooaccess kön lagringen
Lägg till följande importera instruktioner toohello överkant hello Java filen där du vill toouse Azure storage-API: er tooaccess köer hello:

```java
// Include hello following imports toouse queue APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.queue.*;
```

## <a name="setup-an-azure-storage-connection-string"></a>Ställ in en anslutningssträng för Azure storage
Ett Azure storage-klienten använder en lagring sträng toostore slutpunkter och autentiseringsuppgifter för åtkomst till data management services. När den körs i ett klientprogram, måste du ange hello lagringsanslutningssträngen i hello följande format, med hello namnet på ditt lagringskonto och hello primärnyckeln för hello storage-konto som anges i hello [Azure Portal](https://portal.azure.com)för hello *AccountName* och *AccountKey* värden. Det här exemplet visar hur du kan deklarera en anslutningssträng för statiska fält toohold hello:

```java
// Define hello connection-string with your values.
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

I ett program som körs inom en roll i Microsoft Azure, kan den här strängen lagras i hello tjänstkonfigurationsfilen *ServiceConfiguration.cscfg*, och kan nås med ett samtal toohello  **RoleEnvironment.getConfigurationSettings** metod. Här är ett exempel på hello anslutningssträngen från en **inställningen** element med namnet *StorageConnectionString* i konfigurationsfilen för hello-tjänsten:

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

hello förutsätter följande exempel att du har använt någon av dessa två metoder tooget hello lagringsanslutningssträng.

## <a name="how-to-create-a-queue"></a>Så här: skapa en kö
En **CloudQueueClient** objekt kan du få referensobjekt för köer. hello följande kod skapar en **CloudQueueClient** objekt. (Observera: det finns andra sätt toocreate **CloudStorageAccount** objekt; mer information finns i **CloudStorageAccount** i hello [referens för Azure Storage Client SDK].)

Använd hello **CloudQueueClient** objekt tooget en referens toohello kö som du vill toouse. Du kan skapa hello kö om den inte finns.

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

## <a name="how-to-add-a-message-tooa-queue"></a>Så här: Lägg till en meddelandekö tooa
tooinsert ett meddelande i en befintlig kö först skapa en ny **CloudQueueMessage**. Därefter anropar hello **addMessage** metod. En **CloudQueueMessage** kan skapas från en sträng (i UTF-8-format) eller en byte-matris. Här är kod som skapar en kö (om den inte finns) och infogningar hello-meddelande ”Hello World”.

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

## <a name="how-to-peek-at-hello-next-message"></a>Så här: granska nästa hello-meddelande
Du kan kika på hello-meddelande i hello framför en kö utan att ta bort den från hello kö genom att anropa **peekMessage**.

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

## <a name="how-to-change-hello-contents-of-a-queued-message"></a>Så här: ändra hello innehållet i ett meddelande i kön
Du kan ändra hello innehållet i ett meddelande direkt i hello kö. Om hello-meddelande representerar en arbetsuppgift, kan du använda den här funktionen tooupdate hello hello arbete aktivitetens status. hello följande kod uppdaterar hello kömeddelandet med nytt innehåll och anger hello synlighet timeout tooextend ytterligare 60 sekunder. Detta sparar hello statusen för arbetsuppgiften som associeras med hello-meddelande och ger hello klienten en annan minut toocontinue som arbetar på hello-meddelande. Du kan använda den här tekniken tootrack arbetsflöden med flera steg i Kömeddelanden, utan att behöva toostart över från hello början om ett bearbetningssteg misslyckas på grund av toohardware eller ett programvarufel. Normalt ska du behålla ett återförsöksvärde och om hello meddelandet försöks mer än  *n*  tider, tar du bort den. Detta skyddar mot meddelanden som utlöser ett programfel varje gång de bearbetas.

hello följande kod exempel söker igenom hello meddelandekö, söker efter första hello-meddelande som matchar ”Hello World” för hello innehåll, och sedan ändrar hello-meddelande innehåll och avslutas.

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

Du kan också uppdateras hello följande kodexempel bara första synliga hälsningsmeddelande hello kön.

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

## <a name="how-to-get-hello-queue-length"></a>Så här: hämta hello Kölängd
Du kan få en uppskattning av hello antal meddelanden i en kö. Hej **downloadAttributes** metoden ber hello Kötjänsten för flera aktuella värden, inklusive hur många meddelanden som finns i en kö. hello returneras endast ungefärliga eftersom meddelanden kan läggas till eller tas bort efter hello kötjänsten svarar tooyour begäran. Hej **getApproximateMessageCount** metoden returnerar hello sista värdet som hämtas av hello anrop för**downloadAttributes**, utan att kötjänsten hello.

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

## <a name="how-to-dequeue-hello-next-message"></a>Så här: status Created nästa hello-meddelande
Koden dequeues ett meddelande från en kö i två steg. När du anropar **retrieveMessage**, du får hello nästa meddelande i en kö. Ett meddelande som returneras från **retrieveMessage** blir osynligt tooany annan kod läsa meddelanden från den här kön. Som standard är det här meddelandet osynligt i 30 sekunder. toofinish att ta bort hello-meddelande från kön hello, måste du också anropa **deleteMessage**. Den här tvåstegsprocessen för att ta bort ett meddelande säkerställer som om din kod inte tooprocess får ett meddelande på grund av fel toohardware eller programvara, en annan instans av koden hello samma meddelande och försök igen. Koden anropar **deleteMessage** direkt efter hello-meddelande har bearbetats.

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

## <a name="additional-options-for-dequeuing-messages"></a>Ytterligare alternativ för mellan köer meddelanden
Det finns två metoder som du kan använda för att anpassa meddelandehämtningen från en kö. Först får du en grupp med meddelanden (upp too32). Andra, du kan ange en tidsgräns för osynlighet längre eller kortare för att ge koden mer eller mindre tid toofully bearbeta varje meddelande.

hello följande kodexempel används hello **retrieveMessages** metoden tooget 20 meddelanden i ett anrop. Sedan bearbetas varje meddelande med hjälp av en **för** loop. Den anger också hello osynlighet timeout toofive minuter (300 sekunder) för varje meddelande. Observera att hello fem minuterna startar för alla meddelanden på hello samma tid, så när fem minuter sedan hello anropet för**retrieveMessages**, alla meddelanden som inte har tagits bort kommer att bli synliga igen.

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

## <a name="how-to-list-hello-queues"></a>Så här: listan hello köer
tooobtain en lista över hello aktuella köer, anrop hello **CloudQueueClient.listQueues()** metod som returnerar en mängd **CloudQueue** objekt.

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

## <a name="how-to-delete-a-queue"></a>Så här: ta bort en kö
toodelete en kö och alla hälsningsmeddelande som ingår i den anropet hello **deleteIfExists** metod på hello **CloudQueue** objekt.

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

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig hello grunderna i queue storage kan du följa dessa länkar toolearn om mer komplexa lagringsuppgifter.

* [Azure Storage SDK för Java][Azure Storage SDK for Java]
* [Referens för Azure Storage Client SDK][referens för Azure Storage Client SDK]
* [Azure Storage Services REST API][Azure Storage Services REST API]
* [Azure Storage-teamets blogg][Azure Storage Team Blog]

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[referens för Azure Storage Client SDK]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage Services REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
