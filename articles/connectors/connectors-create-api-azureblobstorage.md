---
title: "Lägg till Azure blob storage Connector i dina Logic Apps | Microsoft Docs"
description: "Komma igång och konfigurera Azure blob storage-kopplingen i en logikapp"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b5dc3f75-6bea-420b-b250-183668d2848d
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/02/2017
ms.author: mandia; ladocs
ms.openlocfilehash: bc7908868828bd1628633cf9e57f8c44f8000827
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-azure-blob-storage-connector-in-a-logic-app"></a><span data-ttu-id="a5b50-103">Använda Azure blob storage-kopplingen i en logikapp</span><span class="sxs-lookup"><span data-stu-id="a5b50-103">Use the Azure blob storage connector in a logic app</span></span>
<span data-ttu-id="a5b50-104">Använda Azure Blob storage anslutningen för att ladda upp, uppdatera, hämta och ta bort blobbar i ditt lagringskonto alla inom en logikapp.</span><span class="sxs-lookup"><span data-stu-id="a5b50-104">Use the Azure Blob storage connector to upload, update, get, and delete blobs in your storage account, all within a logic app.</span></span>  

<span data-ttu-id="a5b50-105">Med Azure blob storage kan du:</span><span class="sxs-lookup"><span data-stu-id="a5b50-105">With Azure blob storage, you:</span></span>

* <span data-ttu-id="a5b50-106">Skapa ditt arbetsflöde genom att ladda upp nya projekt eller hämta filer som nyligen har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="a5b50-106">Build your workflow by uploading new projects, or getting files that have been recently updated.</span></span>
* <span data-ttu-id="a5b50-107">Använd åtgärder för att hämta filens metadata, ta bort en fil och kopiera filer.</span><span class="sxs-lookup"><span data-stu-id="a5b50-107">Use actions to get file metadata, delete a file, copy files, and more.</span></span> <span data-ttu-id="a5b50-108">Till exempel när ett verktyg uppdateras i en Azure-webbplats (en utlösare), sedan uppdatera en fil i blob storage (en åtgärd).</span><span class="sxs-lookup"><span data-stu-id="a5b50-108">For example, when a tool is updated in an Azure web site (a trigger), then update a file in blob storage (an action).</span></span> 

<span data-ttu-id="a5b50-109">Det här avsnittet visar hur du använder blob storage-kopplingen i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="a5b50-109">This topic shows you how to use the blob storage connector in a logic app.</span></span>

<span data-ttu-id="a5b50-110">Läs mer om Logic Apps i [vad är logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="a5b50-110">To learn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

<span data-ttu-id="a5b50-111">Läs mer om Logic Apps i [vad är logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="a5b50-111">To learn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-azure-blob-storage"></a><span data-ttu-id="a5b50-112">Ansluta till Azure blob storage</span><span class="sxs-lookup"><span data-stu-id="a5b50-112">Connect to Azure blob storage</span></span>
<span data-ttu-id="a5b50-113">Innan din logikapp får åtkomst till alla tjänster måste du först skapa en *anslutning* till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a5b50-113">Before your logic app can access any service, you first create a *connection* to the service.</span></span> <span data-ttu-id="a5b50-114">En anslutning kan du ansluta en logikapp och en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="a5b50-114">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="a5b50-115">Till exempel för att ansluta till ett lagringskonto måste du först skapa en blob-lagring *anslutning*.</span><span class="sxs-lookup"><span data-stu-id="a5b50-115">For example, to connect to a storage account, you first create a blob storage *connection*.</span></span> <span data-ttu-id="a5b50-116">Ange de autentiseringsuppgifter som du vanligtvis använder för att få åtkomst till tjänsten som du ansluter till om du vill skapa en anslutning.</span><span class="sxs-lookup"><span data-stu-id="a5b50-116">To create a connection, enter the credentials you normally use to access the service you are connecting to.</span></span> <span data-ttu-id="a5b50-117">Så med Azure storage kan du ange autentiseringsuppgifterna till ditt lagringskonto för att skapa anslutningen.</span><span class="sxs-lookup"><span data-stu-id="a5b50-117">So with Azure storage, enter the credentials to your storage account to create the connection.</span></span> 

#### <a name="create-the-connection"></a><span data-ttu-id="a5b50-118">Skapa anslutningen</span><span class="sxs-lookup"><span data-stu-id="a5b50-118">Create the connection</span></span>
> [!INCLUDE [Create a connection to Azure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]

## <a name="use-a-trigger"></a><span data-ttu-id="a5b50-119">Använda en utlösare</span><span class="sxs-lookup"><span data-stu-id="a5b50-119">Use a trigger</span></span>
<span data-ttu-id="a5b50-120">Den här anslutningen har inte utlösare.</span><span class="sxs-lookup"><span data-stu-id="a5b50-120">This connector does not have any triggers.</span></span> <span data-ttu-id="a5b50-121">Använd andra utlösare för att starta logikapp, till exempel en upprepning utlösare, en HTTP-Webhook-utlösare, utlösare som är tillgängliga med övriga kopplingar och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="a5b50-121">Use other triggers to start the logic app, such as a Recurrence trigger, an HTTP Webhook trigger, triggers available with other connectors, and more.</span></span> <span data-ttu-id="a5b50-122">[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md) innehåller ett exempel.</span><span class="sxs-lookup"><span data-stu-id="a5b50-122">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md) provides an example.</span></span>

## <a name="use-an-action"></a><span data-ttu-id="a5b50-123">Använda en åtgärd</span><span class="sxs-lookup"><span data-stu-id="a5b50-123">Use an action</span></span>
<span data-ttu-id="a5b50-124">En åtgärd är en åtgärd som utförs av arbetsflödet som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="a5b50-124">An action is an operation carried out by the workflow defined in a logic app.</span></span>

1. <span data-ttu-id="a5b50-125">Klicka på plustecknet.</span><span class="sxs-lookup"><span data-stu-id="a5b50-125">Select the plus sign.</span></span> <span data-ttu-id="a5b50-126">Du ser flera alternativ: **lägga till en åtgärd**, **Lägg till ett villkor**, eller en av de **mer** alternativ.</span><span class="sxs-lookup"><span data-stu-id="a5b50-126">You see several choices: **Add an action**, **Add a condition**, or one of the **More** options.</span></span>
   
    ![](./media/connectors-create-api-azureblobstorage/add-action.png)
2. <span data-ttu-id="a5b50-127">Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="a5b50-127">Choose **Add an action**.</span></span>
3. <span data-ttu-id="a5b50-128">Skriv ”blob” om du vill hämta en lista över alla tillgängliga åtgärder i textrutan.</span><span class="sxs-lookup"><span data-stu-id="a5b50-128">In the text box, type “blob” to get a list of all the available actions.</span></span>
   
    ![](./media/connectors-create-api-azureblobstorage/actions.png) 
4. <span data-ttu-id="a5b50-129">I vårt exempel väljer **AzureBlob - Get filens metadata med sökvägen**.</span><span class="sxs-lookup"><span data-stu-id="a5b50-129">In our example, choose **AzureBlob - Get file metadata using path**.</span></span> <span data-ttu-id="a5b50-130">Om det finns redan en anslutning, väljer du den **...** (Visa Väljaren) för att välja en fil.</span><span class="sxs-lookup"><span data-stu-id="a5b50-130">If a connection already exists, then select the **...** (Show Picker) button to select a file.</span></span>
   
    ![](./media/connectors-create-api-azureblobstorage/sample-file.png)
   
    <span data-ttu-id="a5b50-131">Om du uppmanas att ange anslutningsinformation anger du information för att skapa anslutningen.</span><span class="sxs-lookup"><span data-stu-id="a5b50-131">If you are prompted for the connection information, then enter the details to create the connection.</span></span> <span data-ttu-id="a5b50-132">[Skapa anslutningen](connectors-create-api-azureblobstorage.md#create-the-connection) i det här avsnittet beskriver dessa egenskaper.</span><span class="sxs-lookup"><span data-stu-id="a5b50-132">[Create the connection](connectors-create-api-azureblobstorage.md#create-the-connection) in this topic describes these properties.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="a5b50-133">I det här exemplet kan vi hämta metadata för en fil.</span><span class="sxs-lookup"><span data-stu-id="a5b50-133">In this example, we get the metadata of a file.</span></span> <span data-ttu-id="a5b50-134">Lägg till en annan åtgärd som skapar en ny fil med en annan koppling om du vill se metadata.</span><span class="sxs-lookup"><span data-stu-id="a5b50-134">To see the metadata, add another action that creates a new file using another connector.</span></span> <span data-ttu-id="a5b50-135">Till exempel lägga till en OneDrive-åtgärd som skapar en ny ”test” fil baserat på metadata.</span><span class="sxs-lookup"><span data-stu-id="a5b50-135">For example, add a OneDrive action that creates a new "test" file based on the metadata.</span></span> 


5. <span data-ttu-id="a5b50-136">**Spara** dina ändringar (övre vänstra hörnet i verktygsfältet).</span><span class="sxs-lookup"><span data-stu-id="a5b50-136">**Save** your changes (top left corner of the toolbar).</span></span> <span data-ttu-id="a5b50-137">Din logikapp sparas och aktiveras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="a5b50-137">Your logic app is saved and may be automatically enabled.</span></span>

> [!TIP]
> <span data-ttu-id="a5b50-138">[Lagringsutforskaren](http://storageexplorer.com/) är ett bra verktyg för att hantera flera lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="a5b50-138">[Storage Explorer](http://storageexplorer.com/) is a great tool to  manage multiple storage accounts.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="a5b50-139">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="a5b50-139">Connector-specific details</span></span>

<span data-ttu-id="a5b50-140">Visa alla utlösare och åtgärder som definierats i swagger och även se några gränser i den [connector information](/connectors/azureblobconnector/).</span><span class="sxs-lookup"><span data-stu-id="a5b50-140">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/azureblobconnector/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="a5b50-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a5b50-141">Next steps</span></span>
<span data-ttu-id="a5b50-142">[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="a5b50-142">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="a5b50-143">Utforska andra tillgängliga kopplingar i Logic Apps på vår [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="a5b50-143">Explore the other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>

