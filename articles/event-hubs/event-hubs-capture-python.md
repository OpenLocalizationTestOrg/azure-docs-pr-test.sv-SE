---
title: "Azure Event Hubs avbilda genomgången | Microsoft Docs"
description: "Exempel som använder Azure Python SDK för att demonstrera funktionen Event Hubs avbilda."
services: event-hubs
documentationcenter: 
author: djrosanova
manager: timlt
editor: 
ms.assetid: bdff820c-5b38-4054-a06a-d1de207f01f6
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: darosa;sethm
ms.openlocfilehash: a764a116755c20f60e92e553bd7c896425272b85
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="event-hubs-capture-walkthrough-python"></a><span data-ttu-id="9da1e-103">Event Hubs avbilda genomgång: Python</span><span class="sxs-lookup"><span data-stu-id="9da1e-103">Event Hubs Capture walkthrough: Python</span></span>

<span data-ttu-id="9da1e-104">Event Hubs avbilda är en funktion i Händelsehubbar som gör att du kan automatiskt leverera strömmande data i din händelsehubb till ett Azure Blob storage-konto valfritt.</span><span class="sxs-lookup"><span data-stu-id="9da1e-104">Event Hubs Capture is a feature of Event Hubs that enables you to automatically deliver the streaming data in your event hub to an Azure Blob storage account of your choice.</span></span> <span data-ttu-id="9da1e-105">Den här funktionen gör det enkelt att utföra batchbearbetning strömning av data i realtid.</span><span class="sxs-lookup"><span data-stu-id="9da1e-105">This capability makes it easy to perform batch processing on real-time streaming data.</span></span> <span data-ttu-id="9da1e-106">Den här artikeln beskriver hur du använder Event Hubs avbilda med Python.</span><span class="sxs-lookup"><span data-stu-id="9da1e-106">This article describes how to use Event Hubs Capture with Python.</span></span> <span data-ttu-id="9da1e-107">Mer information om händelsen hubbar avbilda finns i [översiktsartikel](event-hubs-archive-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9da1e-107">For more information about Event Hubs Capture, see the [overview article](event-hubs-archive-overview.md).</span></span>

<span data-ttu-id="9da1e-108">Det här exemplet använder den [Azure Python SDK](https://azure.microsoft.com/develop/python/) att demonstrera funktionen avbildning.</span><span class="sxs-lookup"><span data-stu-id="9da1e-108">This sample uses the [Azure Python SDK](https://azure.microsoft.com/develop/python/) to demonstrate the Capture feature.</span></span> <span data-ttu-id="9da1e-109">Sender.py skickas simulerad miljö telemetri till Händelsehubbar i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="9da1e-109">The sender.py program sends simulated environmental telemetry to Event Hubs in JSON format.</span></span> <span data-ttu-id="9da1e-110">Händelsehubben är konfigurerad för att använda funktionen avbilda dataskrivning att blob-lagring i batchar.</span><span class="sxs-lookup"><span data-stu-id="9da1e-110">The event hub is configured to use the Capture feature to write this data to blob storage in batches.</span></span> <span data-ttu-id="9da1e-111">Appen capturereader.py sedan läser dessa blobbar och skapar en append-fil per enhet och sedan skriver data till CSV-filer.</span><span class="sxs-lookup"><span data-stu-id="9da1e-111">The capturereader.py app then reads these blobs and creates an append file per device, then writes the data into .csv files.</span></span>

## <a name="what-will-be-accomplished"></a><span data-ttu-id="9da1e-112">Detta kommer att utföras</span><span class="sxs-lookup"><span data-stu-id="9da1e-112">What will be accomplished</span></span>

1. <span data-ttu-id="9da1e-113">Skapa ett Azure Blob Storage-konto och en blob-behållare i den, som använder Azure portal.</span><span class="sxs-lookup"><span data-stu-id="9da1e-113">Create an Azure Blob Storage account and a blob container within it, using the Azure portal.</span></span>
2. <span data-ttu-id="9da1e-114">Skapa ett namnområde som Event Hub med Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9da1e-114">Create an Event Hub namespace, using the Azure portal.</span></span>
3. <span data-ttu-id="9da1e-115">Skapa en händelsehubb med funktionen avbilda aktiverad, med hjälp av Azure portal.</span><span class="sxs-lookup"><span data-stu-id="9da1e-115">Create an event hub with the Capture feature enabled, using the Azure portal.</span></span>
4. <span data-ttu-id="9da1e-116">Skicka data till händelsehubben med Python-skriptet.</span><span class="sxs-lookup"><span data-stu-id="9da1e-116">Send data to the event hub with a Python script.</span></span>
5. <span data-ttu-id="9da1e-117">Läsa filerna från avbildningen och bearbeta dem med en annan Python-skriptet.</span><span class="sxs-lookup"><span data-stu-id="9da1e-117">Read the files from the capture and process them with another Python script.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9da1e-118">Krav</span><span class="sxs-lookup"><span data-stu-id="9da1e-118">Prerequisites</span></span>

- <span data-ttu-id="9da1e-119">Python 2.7.x</span><span class="sxs-lookup"><span data-stu-id="9da1e-119">Python 2.7.x</span></span>
- <span data-ttu-id="9da1e-120">En Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="9da1e-120">An Azure subscription</span></span>
- <span data-ttu-id="9da1e-121">En aktiv [Händelsehubbar namnområde och event hub.](event-hubs-create.md)</span><span class="sxs-lookup"><span data-stu-id="9da1e-121">An active [Event Hubs namespace and event hub.](event-hubs-create.md)</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="9da1e-122">Skapa ett Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="9da1e-122">Create an Azure Storage account</span></span>
1. <span data-ttu-id="9da1e-123">Logga in på [Azure portal][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="9da1e-123">Log on to the [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="9da1e-124">I det vänstra navigeringsfönstret i portalen klickar du på **ny**, klicka på **lagring**, och klicka sedan på **Lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="9da1e-124">In the left navigation pane of the portal, click **New**, then click **Storage**, and then click **Storage Account**.</span></span>
3. <span data-ttu-id="9da1e-125">Fyll i fälten i bladet storage-konto och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="9da1e-125">Complete the fields in the storage account blade and then click **Create**.</span></span>
   
   ![][1]
4. <span data-ttu-id="9da1e-126">Efter att du ser den **distributioner lyckades** klickar du på namnet på det nya lagringskontot och i den **Essentials** bladet, klickar du på **Blobbar**.</span><span class="sxs-lookup"><span data-stu-id="9da1e-126">After you see the **Deployments Succeeded** message, click the name of the new storage account and in the **Essentials** blade, click **Blobs**.</span></span> <span data-ttu-id="9da1e-127">När den **Blob-tjänst** blad öppnas, klickar du på **+ behållare** längst upp.</span><span class="sxs-lookup"><span data-stu-id="9da1e-127">When the **Blob service** blade opens, click **+ Container** at the top.</span></span> <span data-ttu-id="9da1e-128">Namnet på behållaren **avbilda**sedan Stäng den **Blob-tjänst** bladet.</span><span class="sxs-lookup"><span data-stu-id="9da1e-128">Name the container **capture**, then close the **Blob service** blade.</span></span>
5. <span data-ttu-id="9da1e-129">Klicka på **åtkomstnycklar** i den vänstra bladet och kopiera namnet på lagringskontot och värdet för **key1**.</span><span class="sxs-lookup"><span data-stu-id="9da1e-129">Click **Access keys** in the left blade and copy the name of the storage account and the value of **key1**.</span></span> <span data-ttu-id="9da1e-130">Spara dessa värden i anteckningar eller en tillfällig plats.</span><span class="sxs-lookup"><span data-stu-id="9da1e-130">Save these values to Notepad or some other temporary location.</span></span>

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a><span data-ttu-id="9da1e-131">Skapa en Python-skript för att skicka händelser till din event hub</span><span class="sxs-lookup"><span data-stu-id="9da1e-131">Create a Python script to send events to your event hub</span></span>
1. <span data-ttu-id="9da1e-132">Öppna din favorit Python-redigerare, till exempel [Visual Studio Code][Visual Studio Code].</span><span class="sxs-lookup"><span data-stu-id="9da1e-132">Open your favorite Python editor, such as [Visual Studio Code][Visual Studio Code].</span></span>
2. <span data-ttu-id="9da1e-133">Skapa ett skript som heter **sender.py**.</span><span class="sxs-lookup"><span data-stu-id="9da1e-133">Create a script called **sender.py**.</span></span> <span data-ttu-id="9da1e-134">Det här skriptet skickar 200 händelser till din event hub.</span><span class="sxs-lookup"><span data-stu-id="9da1e-134">This script sends 200 events to your event hub.</span></span> <span data-ttu-id="9da1e-135">De är enkla miljön avläsningar skickas i JSON.</span><span class="sxs-lookup"><span data-stu-id="9da1e-135">They are simple environmental readings sent in JSON.</span></span>
3. <span data-ttu-id="9da1e-136">Klistra in följande kod i sender.py:</span><span class="sxs-lookup"><span data-stu-id="9da1e-136">Paste the following code into sender.py:</span></span>
   
  ```python
  import uuid
  import datetime
  import random
  import json
  from azure.servicebus import ServiceBusService
   
  sbs = ServiceBusService(service_namespace='INSERT YOUR NAMESPACE NAME', shared_access_key_name='RootManageSharedAccessKey', shared_access_key_value='INSERT YOUR KEY')
  devices = []
  for x in range(0, 10):
      devices.append(str(uuid.uuid4()))
   
  for y in range(0,20):
      for dev in devices:
          reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
          s = json.dumps(reading)
          sbs.send_event('INSERT YOUR EVENT HUB NAME', s)
      print y
  ```
4. <span data-ttu-id="9da1e-137">Uppdatera föregående kod för att använda din namnområdesnamnet och värdet för nyckeln händelsehubbens namn som du fick när du skapade namnområdet Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="9da1e-137">Update the preceding code to use your namespace name, key value, and event hub name that you obtained when you created the Event Hubs namespace.</span></span>

## <a name="create-a-python-script-to-read-your-capture-files"></a><span data-ttu-id="9da1e-138">Skapa en Python-skript för att läsa Capture-filer</span><span class="sxs-lookup"><span data-stu-id="9da1e-138">Create a Python script to read your Capture files</span></span>

1. <span data-ttu-id="9da1e-139">Fyll i bladet och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="9da1e-139">Fill out the blade and click **Create**.</span></span>
2. <span data-ttu-id="9da1e-140">Skapa ett skript som heter **capturereader.py**.</span><span class="sxs-lookup"><span data-stu-id="9da1e-140">Create a script called **capturereader.py**.</span></span> <span data-ttu-id="9da1e-141">Det här skriptet läser insamlade filer och skapar en fil per enhet för att skriva data endast för enheten.</span><span class="sxs-lookup"><span data-stu-id="9da1e-141">This script reads the captured files and creates a file per device to write the data only for that device.</span></span>
3. <span data-ttu-id="9da1e-142">Klistra in följande kod i capturereader.py:</span><span class="sxs-lookup"><span data-stu-id="9da1e-142">Paste the following code into capturereader.py:</span></span>
   
  ```python
  import os
  import string
  import json
  import avro.schema
  from avro.datafile import DataFileReader, DataFileWriter
  from avro.io import DatumReader, DatumWriter
  from azure.storage.blob import BlockBlobService
   
  def processBlob(filename):
      reader = DataFileReader(open(filename, 'rb'), DatumReader())
      dict = {}
      for reading in reader:
          parsed_json = json.loads(reading["Body"])
          if not 'id' in parsed_json:
              return
          if not dict.has_key(parsed_json['id']):
              list = []
              dict[parsed_json['id']] = list
          else:
              list = dict[parsed_json['id']]
              list.append(parsed_json)
      reader.close()
      for device in dict.keys():
          deviceFile = open(device + '.csv', "a")
          for r in dict[device]:
              deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\n')
   
  def startProcessing(accountName, key, container):
      print 'Processor started using path: ' + os.getcwd()
      block_blob_service = BlockBlobService(account_name=accountName, account_key=key)
      generator = block_blob_service.list_blobs(container)
      for blob in generator:
          if blob.properties.content_length != 0:
              print('Downloaded a non empty blob: ' + blob.name)
              cleanName = string.replace(blob.name, '/', '_')
              block_blob_service.get_blob_to_path(container, blob.name, cleanName)
              processBlob(cleanName)
              os.remove(cleanName)
          block_blob_service.delete_blob(container, blob.name)
  startProcessing('YOUR STORAGE ACCOUNT NAME', 'YOUR KEY', 'capture')
  ```
4. <span data-ttu-id="9da1e-143">Se till att klistra in lämpliga värden för lagringskontonamn och nyckel i anropet till `startProcessing`.</span><span class="sxs-lookup"><span data-stu-id="9da1e-143">Be sure to paste the appropriate values for your storage account name and key in the call to `startProcessing`.</span></span>

## <a name="run-the-scripts"></a><span data-ttu-id="9da1e-144">Kör skripten</span><span class="sxs-lookup"><span data-stu-id="9da1e-144">Run the scripts</span></span>
1. <span data-ttu-id="9da1e-145">Öppna en kommandotolk med Python i dess sökväg och kör sedan följande kommandon för att installera nödvändiga Python-paket:</span><span class="sxs-lookup"><span data-stu-id="9da1e-145">Open a command prompt that has Python in its path, and then run these commands to install Python prerequisite packages:</span></span>
   
  ```
  pip install azure-storage
  pip install azure-servicebus
  pip install avro
  ```
   
  <span data-ttu-id="9da1e-146">Om du har en tidigare version av azure storage eller azure, kan du behöva använda de **--uppgradera** alternativet</span><span class="sxs-lookup"><span data-stu-id="9da1e-146">If you have an earlier version of either azure-storage or azure, you may need to use the **--upgrade** option</span></span>
   
  <span data-ttu-id="9da1e-147">Du kan också behöva kör du följande (inte behövs på de flesta datorer):</span><span class="sxs-lookup"><span data-stu-id="9da1e-147">You might also need to run the following (not necessary on most systems):</span></span>
   
  ```
  pip install cryptography
  ```
2. <span data-ttu-id="9da1e-148">Ändra katalogen till överallt där du sparade sender.py och capturereader.py och kör det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="9da1e-148">Change your directory to wherever you saved sender.py and capturereader.py, and run this command:</span></span>
   
  ```
  start python sender.py
  ```
   
  <span data-ttu-id="9da1e-149">Detta kommando startar en ny Python-process för att köra avsändaren.</span><span class="sxs-lookup"><span data-stu-id="9da1e-149">This command starts a new Python process to run the sender.</span></span>
3. <span data-ttu-id="9da1e-150">Nu ska du vänta några minuter för avbildningen som ska köras.</span><span class="sxs-lookup"><span data-stu-id="9da1e-150">Now wait a few minutes for the capture to run.</span></span> <span data-ttu-id="9da1e-151">Skriv följande kommando i din ursprungliga kommandofönster:</span><span class="sxs-lookup"><span data-stu-id="9da1e-151">Then type the following command into your original command window:</span></span>
   
   ```
   python capturereader.py
   ```

   <span data-ttu-id="9da1e-152">Den här avbildningen processorn använder den lokala katalogen för att hämta alla blobbar från kontot/lagringsbehållaren.</span><span class="sxs-lookup"><span data-stu-id="9da1e-152">This capture processor uses the local directory to download all the blobs from the storage account/container.</span></span> <span data-ttu-id="9da1e-153">Bearbetar någon som inte är tomma och skriver resultatet som CSV-filer till den lokala katalogen.</span><span class="sxs-lookup"><span data-stu-id="9da1e-153">It processes any that are not empty, and writes the results as .csv files into the local directory.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9da1e-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9da1e-154">Next steps</span></span>

<span data-ttu-id="9da1e-155">Du kan lära dig mer om Event Hubs genom att gå till följande länkar:</span><span class="sxs-lookup"><span data-stu-id="9da1e-155">You can learn more about Event Hubs by visiting the following links:</span></span>

* <span data-ttu-id="9da1e-156">[Översikt av Händelsehubbar avbilda][Overview of Event Hubs Capture]</span><span class="sxs-lookup"><span data-stu-id="9da1e-156">[Overview of Event Hubs Capture][Overview of Event Hubs Capture]</span></span>
* <span data-ttu-id="9da1e-157">Ett komplett [exempelprogram som använder händelsehubbar][sample application that uses Event Hubs].</span><span class="sxs-lookup"><span data-stu-id="9da1e-157">A complete [sample application that uses Event Hubs][sample application that uses Event Hubs].</span></span>
* <span data-ttu-id="9da1e-158">Exemplet [Skala ut händelsebearbetning med händelsehubbar][Scale out Event Processing with Event Hubs].</span><span class="sxs-lookup"><span data-stu-id="9da1e-158">The [Scale out Event Processing with Event Hubs][Scale out Event Processing with Event Hubs] sample.</span></span>
* <span data-ttu-id="9da1e-159">[Översikt av händelsehubbar][Event Hubs overview]</span><span class="sxs-lookup"><span data-stu-id="9da1e-159">[Event Hubs overview][Event Hubs overview]</span></span>

[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-archive-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-overview.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
