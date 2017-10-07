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
# <a name="change-a-blob-path-from-hello-default-path-private-preview"></a><span data-ttu-id="da620-103">Ändra en blobbsökvägen från hello standardsökvägen (privat förhandsvisning)</span><span class="sxs-lookup"><span data-stu-id="da620-103">Change a blob path from hello default path (private preview)</span></span>

<span data-ttu-id="da620-104">Den här artikeln beskriver hur fungerar tooset upp en Azure toorename en standardsökväg för blob.</span><span class="sxs-lookup"><span data-stu-id="da620-104">This article describes how tooset up an Azure function toorename a default blob file path.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="da620-105">Krav</span><span class="sxs-lookup"><span data-stu-id="da620-105">Prerequisites</span></span>

<span data-ttu-id="da620-106">Se till att du har en jobbdefinition av som har konfigurerats på rätt sätt i en hybrid Dataresurs i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="da620-106">Ensure that you have a job definition that has been correctly configured in a hybrid data resource within a resource group.</span></span>

## <a name="create-an-azure-function"></a><span data-ttu-id="da620-107">Skapa en Azure-funktion</span><span class="sxs-lookup"><span data-stu-id="da620-107">Create an Azure function</span></span>

<span data-ttu-id="da620-108">toocreate en Azure-funktion hello följande:</span><span class="sxs-lookup"><span data-stu-id="da620-108">toocreate an Azure function, do hello following:</span></span>

1. <span data-ttu-id="da620-109">Gå toohello [Azure-portalen](http://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="da620-109">Go toohello [Azure portal](http://portal.azure.com/).</span></span>

2. <span data-ttu-id="da620-110">Hello överkant hello vänster, klickar du på **ny**.</span><span class="sxs-lookup"><span data-stu-id="da620-110">At hello top of hello left pane, click **New**.</span></span> 

3. <span data-ttu-id="da620-111">I hello **Sök** skriver **Funktionsapp**, och tryck sedan på RETUR.</span><span class="sxs-lookup"><span data-stu-id="da620-111">In hello **Search** box, type **Function App**, and then press Enter.</span></span>

    ![Skriv ”Funktionsapp” i sökrutan för hello](./media/storsimple-data-manager-change-default-blob-path/goto-function-app-resource.png)

4. <span data-ttu-id="da620-113">I hello **resultat** klickar du på **Funktionsapp**.</span><span class="sxs-lookup"><span data-stu-id="da620-113">In hello **Results** list, click **Function App**.</span></span>

    ![Välj hello funktionen app resurs i hello resultatlistan](./media/storsimple-data-manager-change-default-blob-path/select-function-app-resource.png)

    <span data-ttu-id="da620-115">Hej **Funktionsapp** öppnas.</span><span class="sxs-lookup"><span data-stu-id="da620-115">hello **Function App** window opens.</span></span>

5. <span data-ttu-id="da620-116">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="da620-116">Click **Create**.</span></span>

    ![Hej Funktionsapp knapp i ”Skapa”](./media/storsimple-data-manager-change-default-blob-path/create-new-function-app.png)

6. <span data-ttu-id="da620-118">På hello **Funktionsapp** configuration bladet hello följande:</span><span class="sxs-lookup"><span data-stu-id="da620-118">On hello **Function App** configuration blade, do hello following:</span></span>

    <span data-ttu-id="da620-119">a.</span><span class="sxs-lookup"><span data-stu-id="da620-119">a.</span></span> <span data-ttu-id="da620-120">I hello **appnamn** rutan, typnamn hello app.</span><span class="sxs-lookup"><span data-stu-id="da620-120">In hello **App name** box, type hello app name.</span></span>
    
    <span data-ttu-id="da620-121">b.</span><span class="sxs-lookup"><span data-stu-id="da620-121">b.</span></span> <span data-ttu-id="da620-122">I hello **prenumeration** rutan, hello-typnamn för hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="da620-122">In hello **Subscription** box, type hello name of hello subscription.</span></span>

    <span data-ttu-id="da620-123">c.</span><span class="sxs-lookup"><span data-stu-id="da620-123">c.</span></span> <span data-ttu-id="da620-124">Under **resursgruppen**, klickar du på **Skapa nytt**, och sedan ange hello namnet på resursgruppen hello.</span><span class="sxs-lookup"><span data-stu-id="da620-124">Under **Resource Group**, click **Create new**, and then type hello name of hello resource group.</span></span>

    <span data-ttu-id="da620-125">d.</span><span class="sxs-lookup"><span data-stu-id="da620-125">d.</span></span> <span data-ttu-id="da620-126">I hello **värd planera** skriver **förbrukning planera**.</span><span class="sxs-lookup"><span data-stu-id="da620-126">In hello **Hosting Plan** box, type **Consumption Plan**.</span></span>

    <span data-ttu-id="da620-127">e.</span><span class="sxs-lookup"><span data-stu-id="da620-127">e.</span></span> <span data-ttu-id="da620-128">I hello **plats** rutan typen hello plats.</span><span class="sxs-lookup"><span data-stu-id="da620-128">In hello **Location** box, type hello location.</span></span>

    <span data-ttu-id="da620-129">f.</span><span class="sxs-lookup"><span data-stu-id="da620-129">f.</span></span> <span data-ttu-id="da620-130">Under **lagringskonto**, Välj ett befintligt lagringskonto eller skapa ett nytt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="da620-130">Under **Storage account**, select an existing storage account or create a new storage account.</span></span> <span data-ttu-id="da620-131">Ett lagringskonto används internt för hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="da620-131">A storage account is used internally for hello function.</span></span>

    ![Ange nya Funktionsapp konfigurationsdata](./media/storsimple-data-manager-change-default-blob-path/enter-new-funcion-app-data.png)

7. <span data-ttu-id="da620-133">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="da620-133">Click **Create**.</span></span>  
    <span data-ttu-id="da620-134">Hej funktionsapp skapas.</span><span class="sxs-lookup"><span data-stu-id="da620-134">hello function app is created.</span></span>

8. <span data-ttu-id="da620-135">Hello vänster klickar du på **fler tjänster**, och sedan hello följande:</span><span class="sxs-lookup"><span data-stu-id="da620-135">In hello left pane, click **More services**, and then do hello following:</span></span>
    
    <span data-ttu-id="da620-136">a.</span><span class="sxs-lookup"><span data-stu-id="da620-136">a.</span></span> <span data-ttu-id="da620-137">I hello **Filter** skriver **apptjänster**.</span><span class="sxs-lookup"><span data-stu-id="da620-137">In hello **Filter** box, type **App services**.</span></span>
    
    <span data-ttu-id="da620-138">b.</span><span class="sxs-lookup"><span data-stu-id="da620-138">b.</span></span> <span data-ttu-id="da620-139">Klicka på **Apptjänster**.</span><span class="sxs-lookup"><span data-stu-id="da620-139">Click **App Services**.</span></span> 

    ![Länken ”Mer tjänster” hello vänster](./media/storsimple-data-manager-change-default-blob-path/more-services.png)

9. <span data-ttu-id="da620-141">Klicka på hello funktionsapp hello namn i hello listan över apptjänster.</span><span class="sxs-lookup"><span data-stu-id="da620-141">In hello list of app services, click hello name of hello function app.</span></span>  
    <span data-ttu-id="da620-142">hello öppnas funktionen appen.</span><span class="sxs-lookup"><span data-stu-id="da620-142">hello function app page opens.</span></span>

10. <span data-ttu-id="da620-143">Hello vänster klickar du på **nya funktionen**, och sedan hello följande:</span><span class="sxs-lookup"><span data-stu-id="da620-143">In hello left pane, click **New Function**, and then do hello following:</span></span> 

    <span data-ttu-id="da620-144">a.</span><span class="sxs-lookup"><span data-stu-id="da620-144">a.</span></span> <span data-ttu-id="da620-145">I hello **språk** väljer **C#**.</span><span class="sxs-lookup"><span data-stu-id="da620-145">In hello **Language** list, select **C#**.</span></span>
    
    <span data-ttu-id="da620-146">b.</span><span class="sxs-lookup"><span data-stu-id="da620-146">b.</span></span> <span data-ttu-id="da620-147">I hello matris av mallen paneler, Välj **QueueTrigger CSharp**.</span><span class="sxs-lookup"><span data-stu-id="da620-147">In hello array of template tiles, select **QueueTrigger-CSharp**.</span></span>

    <span data-ttu-id="da620-148">c.</span><span class="sxs-lookup"><span data-stu-id="da620-148">c.</span></span> <span data-ttu-id="da620-149">I hello **namnge din funktion** Skriv ett namn för din funktion.</span><span class="sxs-lookup"><span data-stu-id="da620-149">In hello **Name your function** box, type a name for your function.</span></span>

    <span data-ttu-id="da620-150">d.</span><span class="sxs-lookup"><span data-stu-id="da620-150">d.</span></span> <span data-ttu-id="da620-151">I hello **könamnet** skriver definition för DTS jobbnamn.</span><span class="sxs-lookup"><span data-stu-id="da620-151">In hello **Queue name** box, type your data-transformation job definition name.</span></span>

    <span data-ttu-id="da620-152">e.</span><span class="sxs-lookup"><span data-stu-id="da620-152">e.</span></span> <span data-ttu-id="da620-153">Under **konto lagringsanslutning**, klickar du på **nya**, och välj hello-konto som motsvarar toohello DTS jobbet.</span><span class="sxs-lookup"><span data-stu-id="da620-153">Under **Storage account connection**, click **new**, and then select hello account that corresponds toohello data-transformation job.</span></span>  
        <span data-ttu-id="da620-154">Anteckna hello anslutningsnamn.</span><span class="sxs-lookup"><span data-stu-id="da620-154">Make a note of hello connection name.</span></span> <span data-ttu-id="da620-155">senare i hello Azure funktionen krävs hello namn.</span><span class="sxs-lookup"><span data-stu-id="da620-155">hello name is required later in hello Azure function.</span></span>

       ![Skapa en ny C#-funktion](./media/storsimple-data-manager-change-default-blob-path/create-new-csharp-function.png)

    <span data-ttu-id="da620-157">f.</span><span class="sxs-lookup"><span data-stu-id="da620-157">f.</span></span> <span data-ttu-id="da620-158">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="da620-158">Click **Create**.</span></span>  
    <span data-ttu-id="da620-159">Hej **funktionen** öppnas.</span><span class="sxs-lookup"><span data-stu-id="da620-159">hello **Function** window opens.</span></span>

11. <span data-ttu-id="da620-160">I hello **funktionen** fönstret kör _.csx_ filen och sedan hello följande:</span><span class="sxs-lookup"><span data-stu-id="da620-160">In hello **Function** window, run _.csx_ file, and then do hello following:</span></span>

    <span data-ttu-id="da620-161">a.</span><span class="sxs-lookup"><span data-stu-id="da620-161">a.</span></span> <span data-ttu-id="da620-162">Klistra in följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="da620-162">Paste hello following code:</span></span>

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

    <span data-ttu-id="da620-163">b.</span><span class="sxs-lookup"><span data-stu-id="da620-163">b.</span></span> <span data-ttu-id="da620-164">Ersätt **STORAGE_CONNECTIONNAME** på rad 11 med anslutningens storage-konto (se punkt 8 c).</span><span class="sxs-lookup"><span data-stu-id="da620-164">Replace **STORAGE_CONNECTIONNAME** on line 11 with your storage account connection (refer point 8c).</span></span>

    <span data-ttu-id="da620-165">c.</span><span class="sxs-lookup"><span data-stu-id="da620-165">c.</span></span> <span data-ttu-id="da620-166">Hello längst upp till vänster, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="da620-166">At hello top left, click **Save**.</span></span>

    ![Spara funktion](./media/storsimple-data-manager-change-default-blob-path/save-function.png)

12. <span data-ttu-id="da620-168">toocomplete Hej funktion, lägga till en mer fil hello följande:</span><span class="sxs-lookup"><span data-stu-id="da620-168">toocomplete hello function, add one more file by doing hello following:</span></span>

    <span data-ttu-id="da620-169">a.</span><span class="sxs-lookup"><span data-stu-id="da620-169">a.</span></span> <span data-ttu-id="da620-170">Klicka på **visa filer**.</span><span class="sxs-lookup"><span data-stu-id="da620-170">Click **View files**.</span></span>

       ![Hej ”visa filer” länk](./media/storsimple-data-manager-change-default-blob-path/view-files.png)

    <span data-ttu-id="da620-172">b.</span><span class="sxs-lookup"><span data-stu-id="da620-172">b.</span></span> <span data-ttu-id="da620-173">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="da620-173">Click **Add**.</span></span>
    
    <span data-ttu-id="da620-174">c.</span><span class="sxs-lookup"><span data-stu-id="da620-174">c.</span></span> <span data-ttu-id="da620-175">Typen **project.json**, och tryck sedan på RETUR.</span><span class="sxs-lookup"><span data-stu-id="da620-175">Type **project.json**, and then press Enter.</span></span>
    
    <span data-ttu-id="da620-176">d.</span><span class="sxs-lookup"><span data-stu-id="da620-176">d.</span></span> <span data-ttu-id="da620-177">I hello **project.json** fil, klistra in följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="da620-177">In hello **project.json** file, paste hello following code:</span></span>

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

    <span data-ttu-id="da620-178">e.</span><span class="sxs-lookup"><span data-stu-id="da620-178">e.</span></span> <span data-ttu-id="da620-179">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="da620-179">Click **Save**.</span></span>

<span data-ttu-id="da620-180">Du har skapat en Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="da620-180">You have created an Azure function.</span></span> <span data-ttu-id="da620-181">Den här funktionen aktiveras varje gång en ny blob genereras av hello DTS jobb.</span><span class="sxs-lookup"><span data-stu-id="da620-181">This function is triggered each time a new blob is generated by hello data-transformation job.</span></span>

## <a name="next-steps"></a><span data-ttu-id="da620-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="da620-182">Next steps</span></span>

[<span data-ttu-id="da620-183">Använda StorSimple Data-hanterarens användargränssnitt tootransform dina data</span><span class="sxs-lookup"><span data-stu-id="da620-183">Use StorSimple Data Manager UI tootransform your data</span></span>](storsimple-data-manager-ui.md)
