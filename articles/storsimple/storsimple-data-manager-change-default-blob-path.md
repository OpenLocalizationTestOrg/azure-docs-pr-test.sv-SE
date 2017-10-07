---
title: "aaaChange blobbsökvägen från hello standard | Microsoft Docs"
description: "Lär dig hur fungerar tooset upp en Azure toorename en blob-sökväg (privat förhandsvisning)"
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/16/2017
ms.author: vidarmsft
ms.openlocfilehash: 2c414603514223c701ab1a3bd0b81ee18f1af666
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="change-a-blob-path-from-hello-default-path-private-preview"></a>Ändra en blobbsökvägen från hello standardsökvägen (privat förhandsvisning)

Den här artikeln beskriver hur fungerar tooset upp en Azure toorename en standardsökväg för blob. 

## <a name="prerequisites"></a>Krav

Se till att du har en jobbdefinition av som har konfigurerats på rätt sätt i en hybrid Dataresurs i en resursgrupp.

## <a name="create-an-azure-function"></a>Skapa en Azure-funktion

toocreate en Azure-funktion hello följande:

1. Gå toohello [Azure-portalen](http://portal.azure.com/).

2. Hello överkant hello vänster, klickar du på **ny**. 

3. I hello **Sök** skriver **Funktionsapp**, och tryck sedan på RETUR.

    ![Skriv ”Funktionsapp” i sökrutan för hello](./media/storsimple-data-manager-change-default-blob-path/goto-function-app-resource.png)

4. I hello **resultat** klickar du på **Funktionsapp**.

    ![Välj hello funktionen app resurs i hello resultatlistan](./media/storsimple-data-manager-change-default-blob-path/select-function-app-resource.png)

    Hej **Funktionsapp** öppnas.

5. Klicka på **Skapa**.

    ![Hej Funktionsapp knapp i ”Skapa”](./media/storsimple-data-manager-change-default-blob-path/create-new-function-app.png)

6. På hello **Funktionsapp** configuration bladet hello följande:

    a. I hello **appnamn** rutan, typnamn hello app.
    
    b. I hello **prenumeration** rutan, hello-typnamn för hello prenumeration.

    c. Under **resursgruppen**, klickar du på **Skapa nytt**, och sedan ange hello namnet på resursgruppen hello.

    d. I hello **värd planera** skriver **förbrukning planera**.

    e. I hello **plats** rutan typen hello plats.

    f. Under **lagringskonto**, Välj ett befintligt lagringskonto eller skapa ett nytt lagringskonto. Ett lagringskonto används internt för hello-funktionen.

    ![Ange nya Funktionsapp konfigurationsdata](./media/storsimple-data-manager-change-default-blob-path/enter-new-funcion-app-data.png)

7. Klicka på **Skapa**.  
    Hej funktionsapp skapas.

8. Hello vänster klickar du på **fler tjänster**, och sedan hello följande:
    
    a. I hello **Filter** skriver **apptjänster**.
    
    b. Klicka på **Apptjänster**. 

    ![Länken ”Mer tjänster” hello vänster](./media/storsimple-data-manager-change-default-blob-path/more-services.png)

9. Klicka på hello funktionsapp hello namn i hello listan över apptjänster.  
    hello öppnas funktionen appen.

10. Hello vänster klickar du på **nya funktionen**, och sedan hello följande: 

    a. I hello **språk** väljer **C#**.
    
    b. I hello matris av mallen paneler, Välj **QueueTrigger CSharp**.

    c. I hello **namnge din funktion** Skriv ett namn för din funktion.

    d. I hello **könamnet** skriver definition för DTS jobbnamn.

    e. Under **konto lagringsanslutning**, klickar du på **nya**, och välj hello-konto som motsvarar toohello DTS jobbet.  
        Anteckna hello anslutningsnamn. senare i hello Azure funktionen krävs hello namn.

       ![Skapa en ny C#-funktion](./media/storsimple-data-manager-change-default-blob-path/create-new-csharp-function.png)

    f. Klicka på **Skapa**.  
    Hej **funktionen** öppnas.

11. I hello **funktionen** fönstret kör _.csx_ filen och sedan hello följande:

    a. Klistra in följande kod hello:

    ```
    using System;
    using System.Configuration;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.WindowsAzure.Storage.Queue;
    using Microsoft.WindowsAzure.Storage;
    using System.Collections.Generic;
    using System.Linq;

    public static void Run(QueueItem myQueueItem, TraceWriter log)
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConfigurationManager.AppSettings["STORAGE_CONNECTIONNAME"]);

        string storageAccUriEndswith = "windows.net/";
        string uri = myQueueItem.TargetLocation.Replace("%20", " ");
        log.Info($"Blob Uri: {uri}");

        // Remove storage account uri string
        uri = uri.Substring(uri.IndexOf(storageAccUriEndswith) + storageAccUriEndswith.Length);

        string containerName = uri.Substring(0, uri.IndexOf("/")); 

        // Remove container name string
        uri = uri.Substring(containerName.Length + 1);

        // Current blob path
        string blobName = uri; 

        string volumeName = uri.Substring(containerName.Length + 1);
        volumeName = uri.Substring(0, uri.IndexOf("/"));

        // Remove volume name string
        uri = uri.Substring(volumeName.Length + 1);

        string newContainerName = uri.Substring(0, uri.IndexOf("/")).ToLower();
        string newBlobName = uri.Substring(newContainerName.Length + 1);

        log.Info($"Container name: {containerName}");
        log.Info($"Volume name: {volumeName}");
        log.Info($"New container name: {newContainerName}");

        log.Info($"Blob name: {blobName}");
        log.Info($"New blob name: {newBlobName}");

        // Create hello blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Container reference
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlobContainer newContainer = blobClient.GetContainerReference(newContainerName);
        newContainer.CreateIfNotExists();

        if(!container.Exists())
        {
            log.Info($"Container - {containerName} not exists");
            return;
        }

        if(!newContainer.Exists())
        {
            log.Info($"Container - {newContainerName} not exists");
            return;
        }

        CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
        if (!blob.Exists())
        {
            // Skip toocopy hello blob toonew container, if source blob doesn't exist
            log.Info($"hello specified blob does not exist.");
            log.Info($"Blob Uri: {blob.Uri}");
            return;
        }

        CloudBlockBlob blobCopy = newContainer.GetBlockBlobReference(newBlobName);
        if (!blobCopy.Exists())
        {
            blobCopy.StartCopy(blob);
            // Delete old blob, after copy toonew container
            blob.DeleteIfExists();
            log.Info($"Blob file path renamed completed successfully");
        }
        else
        {
            log.Info($"Blob file path renamed already done");
            // Delete old blob, if already exists.
            blob.DeleteIfExists();
        }
    }

    public class QueueItem
    {
        public string SourceLocation {get;set;}
        public long SizeInBytes {get;set;}
        public string Status {get;set;}
        public string JobID {get;set;}
        public string TargetLocation {get; set;}
    }

    ```

    b. Ersätt **STORAGE_CONNECTIONNAME** på rad 11 med anslutningens storage-konto (se punkt 8 c).

    c. Hello längst upp till vänster, klickar du på **spara**.

    ![Spara funktion](./media/storsimple-data-manager-change-default-blob-path/save-function.png)

12. toocomplete Hej funktion, lägga till en mer fil hello följande:

    a. Klicka på **visa filer**.

       ![Hej ”visa filer” länk](./media/storsimple-data-manager-change-default-blob-path/view-files.png)

    b. Klicka på **Lägg till**.
    
    c. Typen **project.json**, och tryck sedan på RETUR.
    
    d. I hello **project.json** fil, klistra in följande kod hello:

    ```
    {
    "frameworks": {
        "net46":{
        "dependencies": {
            "windowsazure.storage": "8.1.1"
        }
        }
    }
    }

    ```

    e. Klicka på **Spara**.

Du har skapat en Azure-funktion. Den här funktionen aktiveras varje gång en ny blob genereras av hello DTS jobb.

## <a name="next-steps"></a>Nästa steg

[Använda StorSimple Data-hanterarens användargränssnitt tootransform dina data](storsimple-data-manager-ui.md)
