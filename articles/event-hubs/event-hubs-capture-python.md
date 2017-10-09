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
# <a name="event-hubs-capture-walkthrough-python"></a>Event Hubs avbilda genomgång: Python

Event Hubs avbilda är en funktion i Händelsehubbar som gör att du tooautomatically leverera hello strömmande data i din event hub tooan Azure Blob storage-konto valfritt. Den här funktionen gör det enkelt tooperform batchbearbetning strömning av data i realtid. Den här artikeln beskriver hur toouse Händelsehubbar avbilda med Python. Mer information om händelsen hubbar avbilda finns hello [översiktsartikel](event-hubs-archive-overview.md).

Det här exemplet använder hello [Azure Python SDK](https://azure.microsoft.com/develop/python/) toodemonstrate hello avbilda funktionen. Hej skickas sender.py simulerad miljö telemetri tooEvent hubbar i JSON-format. Hej händelsehubben har konfigurerats toouse hello avbilda funktion toowrite denna tooblob lagring av data i batchar. Hej capturereader.py app sedan läser dessa blobbar och skapar en append-fil per enhet och sedan skriver hello data till CSV-filer.

## <a name="what-will-be-accomplished"></a>Detta kommer att utföras

1. Skapa ett Azure Blob Storage-konto och en blob-behållare i denna med hello Azure-portalen.
2. Skapa ett namnområde som Event Hub med hello Azure-portalen.
3. Skapa en händelsehubb hello avbilda funktionen är aktiverad, med hjälp av hello Azure-portalen.
4. Skicka data toohello event hub med Python-skriptet.
5. Läs hello filer från hello avbilda och bearbeta dem med en annan Python-skriptet.

## <a name="prerequisites"></a>Krav

- Python 2.7.x
- En Azure-prenumeration
- En aktiv [Händelsehubbar namnområde och event hub.](event-hubs-create.md)

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-storage-account"></a>Skapa ett Azure Storage-konto
1. Logga in toohello [Azure-portalen][Azure portal].
2. Hello vänstra navigeringsfönstret hello-portalen, klicka på **ny**, klicka på **lagring**, och klicka sedan på **Lagringskonto**.
3. Slutför hello fälten i hello lagring-bladet för kontot och klicka sedan på **skapa**.
   
   ![][1]
4. Efter att du ser hello **distributioner lyckades** klickar du på hello namnet på hello nytt lagringskonto och i hello **Essentials** bladet, klickar du på **Blobbar**. När hello **Blob-tjänst** blad öppnas, klickar du på **+ behållare** hello överst. Namnet hello behållaren **avbilda**sedan Stäng hello **Blob-tjänst** bladet.
5. Klicka på **åtkomstnycklar** i hello vänstra bladet och kopiera hello namn hello storage-konto och hello värdet av **key1**. Spara dessa värden tooNotepad eller en tillfällig plats.

## <a name="create-a-python-script-toosend-events-tooyour-event-hub"></a>Skapa en Python-skriptet toosend händelser tooyour händelsehubb
1. Öppna din favorit Python-redigerare, till exempel [Visual Studio Code][Visual Studio Code].
2. Skapa ett skript som heter **sender.py**. Det här skriptet skickar 200 händelser tooyour händelsehubb. De är enkla miljön avläsningar skickas i JSON.
3. Klistra in följande kod i sender.py hello:
   
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
4. Uppdatera hello föregående kod toouse din namnområdesnamnet och värdet för nyckeln händelsehubbens namn som du fick när du skapade hello Händelsehubbar namnområde.

## <a name="create-a-python-script-tooread-your-capture-files"></a>Skapa en Python-skriptet tooread avbilda filer

1. Fyll i hello bladet och klicka på **skapa**.
2. Skapa ett skript som heter **capturereader.py**. Det här skriptet läser hello avbildas filer och skapar en fil per enhet toowrite hello data endast för enheten.
3. Klistra in följande kod i capturereader.py hello:
   
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
4. Vara säker på att toopaste hello lämpliga värden för din lagringskontonamn och nyckel i hello anrop för`startProcessing`.

## <a name="run-hello-scripts"></a>Köra hello skript
1. Öppna en kommandotolk med Python i dess sökväg och kör sedan följande kommandon nödvändiga tooinstall Python-paket:
   
  ```
  pip install azure-storage
  pip install azure-servicebus
  pip install avro
  ```
   
  Om du har en tidigare version av azure storage eller azure kan du behöva toouse hello **--uppgradera** alternativet
   
  Du kan även behöva toorun hello följande (inte behövs på de flesta datorer):
   
  ```
  pip install cryptography
  ```
2. Ändra din katalog toowherever som du sparade sender.py och capturereader.py och kör det här kommandot:
   
  ```
  start python sender.py
  ```
   
  Detta kommando startar en ny Python processen toorun hello-avsändare.
3. Nu ska du vänta några minuter för hello avbilda toorun. Skriv följande kommando i din ursprungliga kommandofönstret hello:
   
   ```
   python capturereader.py
   ```

   Den här avbildningen processorn använder hello lokal katalog toodownload alla hello BLOB från hello konto/lagringsbehållaren. Bearbetar någon som inte är tomma och skriver hello resultat som CSV-filer till hello lokal katalog.

## <a name="next-steps"></a>Nästa steg

Mer information om Händelsehubbar genom att besöka hello följande länkar:

* [Översikt av Händelsehubbar avbilda][Overview of Event Hubs Capture]
* Ett komplett [exempelprogram som använder händelsehubbar][sample application that uses Event Hubs].
* Hej [skala ut händelsebearbetning med Händelsehubbar] [ Scale out Event Processing with Event Hubs] exempel.
* [Översikt av händelsehubbar][Event Hubs overview]

[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-archive-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-overview.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
