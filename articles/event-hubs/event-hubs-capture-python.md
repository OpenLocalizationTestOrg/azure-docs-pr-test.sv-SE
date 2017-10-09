---
title: "aaaAzure Event Hubs avbilda genomgången | Microsoft Docs"
description: "Exempel som använder hello Azure Python SDK toodemonstrate funktionen hello Event Hubs avbilda."
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
ms.openlocfilehash: 1737dcca283711d863aa970db0e80ae71814e666
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-capture-walkthrough-python"></a><span data-ttu-id="676e1-103">Event Hubs avbilda genomgång: Python</span><span class="sxs-lookup"><span data-stu-id="676e1-103">Event Hubs Capture walkthrough: Python</span></span>

<span data-ttu-id="676e1-104">Event Hubs avbilda är en funktion i Händelsehubbar som gör att du tooautomatically leverera hello strömmande data i din event hub tooan Azure Blob storage-konto valfritt.</span><span class="sxs-lookup"><span data-stu-id="676e1-104">Event Hubs Capture is a feature of Event Hubs that enables you tooautomatically deliver hello streaming data in your event hub tooan Azure Blob storage account of your choice.</span></span> <span data-ttu-id="676e1-105">Den här funktionen gör det enkelt tooperform batchbearbetning strömning av data i realtid.</span><span class="sxs-lookup"><span data-stu-id="676e1-105">This capability makes it easy tooperform batch processing on real-time streaming data.</span></span> <span data-ttu-id="676e1-106">Den här artikeln beskriver hur toouse Händelsehubbar avbilda med Python.</span><span class="sxs-lookup"><span data-stu-id="676e1-106">This article describes how toouse Event Hubs Capture with Python.</span></span> <span data-ttu-id="676e1-107">Mer information om händelsen hubbar avbilda finns hello [översiktsartikel](event-hubs-archive-overview.md).</span><span class="sxs-lookup"><span data-stu-id="676e1-107">For more information about Event Hubs Capture, see hello [overview article](event-hubs-archive-overview.md).</span></span>

<span data-ttu-id="676e1-108">Det här exemplet använder hello [Azure Python SDK](https://azure.microsoft.com/develop/python/) toodemonstrate hello avbilda funktionen.</span><span class="sxs-lookup"><span data-stu-id="676e1-108">This sample uses hello [Azure Python SDK](https://azure.microsoft.com/develop/python/) toodemonstrate hello Capture feature.</span></span> <span data-ttu-id="676e1-109">Hej skickas sender.py simulerad miljö telemetri tooEvent hubbar i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="676e1-109">hello sender.py program sends simulated environmental telemetry tooEvent Hubs in JSON format.</span></span> <span data-ttu-id="676e1-110">Hej händelsehubben har konfigurerats toouse hello avbilda funktion toowrite denna tooblob lagring av data i batchar.</span><span class="sxs-lookup"><span data-stu-id="676e1-110">hello event hub is configured toouse hello Capture feature toowrite this data tooblob storage in batches.</span></span> <span data-ttu-id="676e1-111">Hej capturereader.py app sedan läser dessa blobbar och skapar en append-fil per enhet och sedan skriver hello data till CSV-filer.</span><span class="sxs-lookup"><span data-stu-id="676e1-111">hello capturereader.py app then reads these blobs and creates an append file per device, then writes hello data into .csv files.</span></span>

## <a name="what-will-be-accomplished"></a><span data-ttu-id="676e1-112">Detta kommer att utföras</span><span class="sxs-lookup"><span data-stu-id="676e1-112">What will be accomplished</span></span>

1. <span data-ttu-id="676e1-113">Skapa ett Azure Blob Storage-konto och en blob-behållare i denna med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="676e1-113">Create an Azure Blob Storage account and a blob container within it, using hello Azure portal.</span></span>
2. <span data-ttu-id="676e1-114">Skapa ett namnområde som Event Hub med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="676e1-114">Create an Event Hub namespace, using hello Azure portal.</span></span>
3. <span data-ttu-id="676e1-115">Skapa en händelsehubb hello avbilda funktionen är aktiverad, med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="676e1-115">Create an event hub with hello Capture feature enabled, using hello Azure portal.</span></span>
4. <span data-ttu-id="676e1-116">Skicka data toohello event hub med Python-skriptet.</span><span class="sxs-lookup"><span data-stu-id="676e1-116">Send data toohello event hub with a Python script.</span></span>
5. <span data-ttu-id="676e1-117">Läs hello filer från hello avbilda och bearbeta dem med en annan Python-skriptet.</span><span class="sxs-lookup"><span data-stu-id="676e1-117">Read hello files from hello capture and process them with another Python script.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="676e1-118">Krav</span><span class="sxs-lookup"><span data-stu-id="676e1-118">Prerequisites</span></span>

- <span data-ttu-id="676e1-119">Python 2.7.x</span><span class="sxs-lookup"><span data-stu-id="676e1-119">Python 2.7.x</span></span>
- <span data-ttu-id="676e1-120">En Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="676e1-120">An Azure subscription</span></span>
- <span data-ttu-id="676e1-121">En aktiv [Händelsehubbar namnområde och event hub.](event-hubs-create.md)</span><span class="sxs-lookup"><span data-stu-id="676e1-121">An active [Event Hubs namespace and event hub.](event-hubs-create.md)</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="676e1-122">Skapa ett Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="676e1-122">Create an Azure Storage account</span></span>
1. <span data-ttu-id="676e1-123">Logga in toohello [Azure-portalen][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="676e1-123">Log on toohello [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="676e1-124">Hello vänstra navigeringsfönstret hello-portalen, klicka på **ny**, klicka på **lagring**, och klicka sedan på **Lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="676e1-124">In hello left navigation pane of hello portal, click **New**, then click **Storage**, and then click **Storage Account**.</span></span>
3. <span data-ttu-id="676e1-125">Slutför hello fälten i hello lagring-bladet för kontot och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="676e1-125">Complete hello fields in hello storage account blade and then click **Create**.</span></span>
   
   ![][1]
4. <span data-ttu-id="676e1-126">Efter att du ser hello **distributioner lyckades** klickar du på hello namnet på hello nytt lagringskonto och i hello **Essentials** bladet, klickar du på **Blobbar**.</span><span class="sxs-lookup"><span data-stu-id="676e1-126">After you see hello **Deployments Succeeded** message, click hello name of hello new storage account and in hello **Essentials** blade, click **Blobs**.</span></span> <span data-ttu-id="676e1-127">När hello **Blob-tjänst** blad öppnas, klickar du på **+ behållare** hello överst.</span><span class="sxs-lookup"><span data-stu-id="676e1-127">When hello **Blob service** blade opens, click **+ Container** at hello top.</span></span> <span data-ttu-id="676e1-128">Namnet hello behållaren **avbilda**sedan Stäng hello **Blob-tjänst** bladet.</span><span class="sxs-lookup"><span data-stu-id="676e1-128">Name hello container **capture**, then close hello **Blob service** blade.</span></span>
5. <span data-ttu-id="676e1-129">Klicka på **åtkomstnycklar** i hello vänstra bladet och kopiera hello namn hello storage-konto och hello värdet av **key1**.</span><span class="sxs-lookup"><span data-stu-id="676e1-129">Click **Access keys** in hello left blade and copy hello name of hello storage account and hello value of **key1**.</span></span> <span data-ttu-id="676e1-130">Spara dessa värden tooNotepad eller en tillfällig plats.</span><span class="sxs-lookup"><span data-stu-id="676e1-130">Save these values tooNotepad or some other temporary location.</span></span>

## <a name="create-a-python-script-toosend-events-tooyour-event-hub"></a><span data-ttu-id="676e1-131">Skapa en Python-skriptet toosend händelser tooyour händelsehubb</span><span class="sxs-lookup"><span data-stu-id="676e1-131">Create a Python script toosend events tooyour event hub</span></span>
1. <span data-ttu-id="676e1-132">Öppna din favorit Python-redigerare, till exempel [Visual Studio Code][Visual Studio Code].</span><span class="sxs-lookup"><span data-stu-id="676e1-132">Open your favorite Python editor, such as [Visual Studio Code][Visual Studio Code].</span></span>
2. <span data-ttu-id="676e1-133">Skapa ett skript som heter **sender.py**.</span><span class="sxs-lookup"><span data-stu-id="676e1-133">Create a script called **sender.py**.</span></span> <span data-ttu-id="676e1-134">Det här skriptet skickar 200 händelser tooyour händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="676e1-134">This script sends 200 events tooyour event hub.</span></span> <span data-ttu-id="676e1-135">De är enkla miljön avläsningar skickas i JSON.</span><span class="sxs-lookup"><span data-stu-id="676e1-135">They are simple environmental readings sent in JSON.</span></span>
3. <span data-ttu-id="676e1-136">Klistra in följande kod i sender.py hello:</span><span class="sxs-lookup"><span data-stu-id="676e1-136">Paste hello following code into sender.py:</span></span>
   
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
4. <span data-ttu-id="676e1-137">Uppdatera hello föregående kod toouse din namnområdesnamnet och värdet för nyckeln händelsehubbens namn som du fick när du skapade hello Händelsehubbar namnområde.</span><span class="sxs-lookup"><span data-stu-id="676e1-137">Update hello preceding code toouse your namespace name, key value, and event hub name that you obtained when you created hello Event Hubs namespace.</span></span>

## <a name="create-a-python-script-tooread-your-capture-files"></a><span data-ttu-id="676e1-138">Skapa en Python-skriptet tooread avbilda filer</span><span class="sxs-lookup"><span data-stu-id="676e1-138">Create a Python script tooread your Capture files</span></span>

1. <span data-ttu-id="676e1-139">Fyll i hello bladet och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="676e1-139">Fill out hello blade and click **Create**.</span></span>
2. <span data-ttu-id="676e1-140">Skapa ett skript som heter **capturereader.py**.</span><span class="sxs-lookup"><span data-stu-id="676e1-140">Create a script called **capturereader.py**.</span></span> <span data-ttu-id="676e1-141">Det här skriptet läser hello avbildas filer och skapar en fil per enhet toowrite hello data endast för enheten.</span><span class="sxs-lookup"><span data-stu-id="676e1-141">This script reads hello captured files and creates a file per device toowrite hello data only for that device.</span></span>
3. <span data-ttu-id="676e1-142">Klistra in följande kod i capturereader.py hello:</span><span class="sxs-lookup"><span data-stu-id="676e1-142">Paste hello following code into capturereader.py:</span></span>
   
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
4. <span data-ttu-id="676e1-143">Vara säker på att toopaste hello lämpliga värden för din lagringskontonamn och nyckel i hello anrop för`startProcessing`.</span><span class="sxs-lookup"><span data-stu-id="676e1-143">Be sure toopaste hello appropriate values for your storage account name and key in hello call too`startProcessing`.</span></span>

## <a name="run-hello-scripts"></a><span data-ttu-id="676e1-144">Köra hello skript</span><span class="sxs-lookup"><span data-stu-id="676e1-144">Run hello scripts</span></span>
1. <span data-ttu-id="676e1-145">Öppna en kommandotolk med Python i dess sökväg och kör sedan följande kommandon nödvändiga tooinstall Python-paket:</span><span class="sxs-lookup"><span data-stu-id="676e1-145">Open a command prompt that has Python in its path, and then run these commands tooinstall Python prerequisite packages:</span></span>
   
  ```
  pip install azure-storage
  pip install azure-servicebus
  pip install avro
  ```
   
  <span data-ttu-id="676e1-146">Om du har en tidigare version av azure storage eller azure kan du behöva toouse hello **--uppgradera** alternativet</span><span class="sxs-lookup"><span data-stu-id="676e1-146">If you have an earlier version of either azure-storage or azure, you may need toouse hello **--upgrade** option</span></span>
   
  <span data-ttu-id="676e1-147">Du kan även behöva toorun hello följande (inte behövs på de flesta datorer):</span><span class="sxs-lookup"><span data-stu-id="676e1-147">You might also need toorun hello following (not necessary on most systems):</span></span>
   
  ```
  pip install cryptography
  ```
2. <span data-ttu-id="676e1-148">Ändra din katalog toowherever som du sparade sender.py och capturereader.py och kör det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="676e1-148">Change your directory toowherever you saved sender.py and capturereader.py, and run this command:</span></span>
   
  ```
  start python sender.py
  ```
   
  <span data-ttu-id="676e1-149">Detta kommando startar en ny Python processen toorun hello-avsändare.</span><span class="sxs-lookup"><span data-stu-id="676e1-149">This command starts a new Python process toorun hello sender.</span></span>
3. <span data-ttu-id="676e1-150">Nu ska du vänta några minuter för hello avbilda toorun.</span><span class="sxs-lookup"><span data-stu-id="676e1-150">Now wait a few minutes for hello capture toorun.</span></span> <span data-ttu-id="676e1-151">Skriv följande kommando i din ursprungliga kommandofönstret hello:</span><span class="sxs-lookup"><span data-stu-id="676e1-151">Then type hello following command into your original command window:</span></span>
   
   ```
   python capturereader.py
   ```

   <span data-ttu-id="676e1-152">Den här avbildningen processorn använder hello lokal katalog toodownload alla hello BLOB från hello konto/lagringsbehållaren.</span><span class="sxs-lookup"><span data-stu-id="676e1-152">This capture processor uses hello local directory toodownload all hello blobs from hello storage account/container.</span></span> <span data-ttu-id="676e1-153">Bearbetar någon som inte är tomma och skriver hello resultat som CSV-filer till hello lokal katalog.</span><span class="sxs-lookup"><span data-stu-id="676e1-153">It processes any that are not empty, and writes hello results as .csv files into hello local directory.</span></span>

## <a name="next-steps"></a><span data-ttu-id="676e1-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="676e1-154">Next steps</span></span>

<span data-ttu-id="676e1-155">Mer information om Händelsehubbar genom att besöka hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="676e1-155">You can learn more about Event Hubs by visiting hello following links:</span></span>

* <span data-ttu-id="676e1-156">[Översikt av Händelsehubbar avbilda][Overview of Event Hubs Capture]</span><span class="sxs-lookup"><span data-stu-id="676e1-156">[Overview of Event Hubs Capture][Overview of Event Hubs Capture]</span></span>
* <span data-ttu-id="676e1-157">Ett komplett [exempelprogram som använder händelsehubbar][sample application that uses Event Hubs].</span><span class="sxs-lookup"><span data-stu-id="676e1-157">A complete [sample application that uses Event Hubs][sample application that uses Event Hubs].</span></span>
* <span data-ttu-id="676e1-158">Hej [skala ut händelsebearbetning med Händelsehubbar] [ Scale out Event Processing with Event Hubs] exempel.</span><span class="sxs-lookup"><span data-stu-id="676e1-158">hello [Scale out Event Processing with Event Hubs][Scale out Event Processing with Event Hubs] sample.</span></span>
* <span data-ttu-id="676e1-159">[Översikt av händelsehubbar][Event Hubs overview]</span><span class="sxs-lookup"><span data-stu-id="676e1-159">[Event Hubs overview][Event Hubs overview]</span></span>

[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-archive-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-overview.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
