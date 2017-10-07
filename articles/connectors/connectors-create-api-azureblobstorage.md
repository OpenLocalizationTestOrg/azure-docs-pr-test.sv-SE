---
title: aaaAdd hello Azure blob storage-anslutningen i dina Logic Apps | Microsoft Docs
description: "Komma igång och konfigurera hello Azure blob storage-anslutningen i en logikapp"
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
ms.openlocfilehash: add61287ef1b2228ef9d3f54ce082807bad6858b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-blob-storage-connector-in-a-logic-app"></a><span data-ttu-id="d1553-103">Använd hello Azure blob storage-anslutningen i en logikapp</span><span class="sxs-lookup"><span data-stu-id="d1553-103">Use hello Azure blob storage connector in a logic app</span></span>
<span data-ttu-id="d1553-104">Använd hello Azure Blob storage connector tooupload, uppdatera, hämta och ta bort blobbar i ditt lagringskonto alla inom en logikapp.</span><span class="sxs-lookup"><span data-stu-id="d1553-104">Use hello Azure Blob storage connector tooupload, update, get, and delete blobs in your storage account, all within a logic app.</span></span>  

<span data-ttu-id="d1553-105">Med Azure blob storage kan du:</span><span class="sxs-lookup"><span data-stu-id="d1553-105">With Azure blob storage, you:</span></span>

* <span data-ttu-id="d1553-106">Skapa ditt arbetsflöde genom att ladda upp nya projekt eller hämta filer som nyligen har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="d1553-106">Build your workflow by uploading new projects, or getting files that have been recently updated.</span></span>
* <span data-ttu-id="d1553-107">Använda åtgärder tooget filmetadata, ta bort en fil och kopiera filer.</span><span class="sxs-lookup"><span data-stu-id="d1553-107">Use actions tooget file metadata, delete a file, copy files, and more.</span></span> <span data-ttu-id="d1553-108">Till exempel när ett verktyg uppdateras i en Azure-webbplats (en utlösare), sedan uppdatera en fil i blob storage (en åtgärd).</span><span class="sxs-lookup"><span data-stu-id="d1553-108">For example, when a tool is updated in an Azure web site (a trigger), then update a file in blob storage (an action).</span></span> 

<span data-ttu-id="d1553-109">Det här avsnittet visar hur toouse hello blob storage-kopplingen i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="d1553-109">This topic shows you how toouse hello blob storage connector in a logic app.</span></span>

<span data-ttu-id="d1553-110">toolearn mer om Logic Apps finns [vad är logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="d1553-110">toolearn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

<span data-ttu-id="d1553-111">toolearn mer om Logic Apps finns [vad är logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="d1553-111">toolearn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-tooazure-blob-storage"></a><span data-ttu-id="d1553-112">Ansluta tooAzure blob-lagring</span><span class="sxs-lookup"><span data-stu-id="d1553-112">Connect tooAzure blob storage</span></span>
<span data-ttu-id="d1553-113">Innan din logikapp får åtkomst till alla tjänster måste du först skapa en *anslutning* toohello service.</span><span class="sxs-lookup"><span data-stu-id="d1553-113">Before your logic app can access any service, you first create a *connection* toohello service.</span></span> <span data-ttu-id="d1553-114">En anslutning kan du ansluta en logikapp och en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="d1553-114">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="d1553-115">Till exempel tooconnect tooa storage-konto du först skapa en blob-lagring *anslutning*.</span><span class="sxs-lookup"><span data-stu-id="d1553-115">For example, tooconnect tooa storage account, you first create a blob storage *connection*.</span></span> <span data-ttu-id="d1553-116">toocreate en anslutning, ange hello autentiseringsuppgifter som du vanligtvis använder tooaccess hello-tjänsten som du ansluter till.</span><span class="sxs-lookup"><span data-stu-id="d1553-116">toocreate a connection, enter hello credentials you normally use tooaccess hello service you are connecting to.</span></span> <span data-ttu-id="d1553-117">Ange så hello autentiseringsuppgifter tooyour storage-konto toocreate hello anslutning med Azure storage.</span><span class="sxs-lookup"><span data-stu-id="d1553-117">So with Azure storage, enter hello credentials tooyour storage account toocreate hello connection.</span></span> 

#### <a name="create-hello-connection"></a><span data-ttu-id="d1553-118">Skapa hello-anslutning</span><span class="sxs-lookup"><span data-stu-id="d1553-118">Create hello connection</span></span>
> [!INCLUDE [Create a connection tooAzure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]

## <a name="use-a-trigger"></a><span data-ttu-id="d1553-119">Använda en utlösare</span><span class="sxs-lookup"><span data-stu-id="d1553-119">Use a trigger</span></span>
<span data-ttu-id="d1553-120">Den här anslutningen har inte utlösare.</span><span class="sxs-lookup"><span data-stu-id="d1553-120">This connector does not have any triggers.</span></span> <span data-ttu-id="d1553-121">Använda andra utlösare toostart hello logikapp, till exempel en upprepning utlösare, en HTTP-Webhook-utlösare, utlösare som är tillgängliga med övriga kopplingar och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="d1553-121">Use other triggers toostart hello logic app, such as a Recurrence trigger, an HTTP Webhook trigger, triggers available with other connectors, and more.</span></span> <span data-ttu-id="d1553-122">[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md) innehåller ett exempel.</span><span class="sxs-lookup"><span data-stu-id="d1553-122">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md) provides an example.</span></span>

## <a name="use-an-action"></a><span data-ttu-id="d1553-123">Använda en åtgärd</span><span class="sxs-lookup"><span data-stu-id="d1553-123">Use an action</span></span>
<span data-ttu-id="d1553-124">En åtgärd är en åtgärd som utförs av hello arbetsflöde som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="d1553-124">An action is an operation carried out by hello workflow defined in a logic app.</span></span>

1. <span data-ttu-id="d1553-125">Välj hello plustecken.</span><span class="sxs-lookup"><span data-stu-id="d1553-125">Select hello plus sign.</span></span> <span data-ttu-id="d1553-126">Du ser flera alternativ: **lägga till en åtgärd**, **Lägg till ett villkor**, eller en av hello **mer** alternativ.</span><span class="sxs-lookup"><span data-stu-id="d1553-126">You see several choices: **Add an action**, **Add a condition**, or one of hello **More** options.</span></span>
   
    ![](./media/connectors-create-api-azureblobstorage/add-action.png)
2. <span data-ttu-id="d1553-127">Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="d1553-127">Choose **Add an action**.</span></span>
3. <span data-ttu-id="d1553-128">Skriv ”blob” tooget en lista över alla tillgängliga åtgärder för hello hello i textrutan.</span><span class="sxs-lookup"><span data-stu-id="d1553-128">In hello text box, type “blob” tooget a list of all hello available actions.</span></span>
   
    ![](./media/connectors-create-api-azureblobstorage/actions.png) 
4. <span data-ttu-id="d1553-129">I vårt exempel väljer **AzureBlob - Get filens metadata med sökvägen**.</span><span class="sxs-lookup"><span data-stu-id="d1553-129">In our example, choose **AzureBlob - Get file metadata using path**.</span></span> <span data-ttu-id="d1553-130">Om det finns redan en anslutning, och välj sedan hello **...** (Visa Väljaren) knappen tooselect en fil.</span><span class="sxs-lookup"><span data-stu-id="d1553-130">If a connection already exists, then select hello **...** (Show Picker) button tooselect a file.</span></span>
   
    ![](./media/connectors-create-api-azureblobstorage/sample-file.png)
   
    <span data-ttu-id="d1553-131">Om du uppmanas att ange anslutningsinformation för hello ange hello information toocreate hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="d1553-131">If you are prompted for hello connection information, then enter hello details toocreate hello connection.</span></span> <span data-ttu-id="d1553-132">[Skapa hello anslutning](connectors-create-api-azureblobstorage.md#create-the-connection) i det här avsnittet beskriver dessa egenskaper.</span><span class="sxs-lookup"><span data-stu-id="d1553-132">[Create hello connection](connectors-create-api-azureblobstorage.md#create-the-connection) in this topic describes these properties.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="d1553-133">I det här exemplet kan vi hämta hello metadata för en fil.</span><span class="sxs-lookup"><span data-stu-id="d1553-133">In this example, we get hello metadata of a file.</span></span> <span data-ttu-id="d1553-134">toosee hello metadata, Lägg till en annan åtgärd som skapar en ny fil med en annan koppling.</span><span class="sxs-lookup"><span data-stu-id="d1553-134">toosee hello metadata, add another action that creates a new file using another connector.</span></span> <span data-ttu-id="d1553-135">Till exempel lägga till en OneDrive-åtgärd som skapar en ny ”test” fil baserat på hello metadata.</span><span class="sxs-lookup"><span data-stu-id="d1553-135">For example, add a OneDrive action that creates a new "test" file based on hello metadata.</span></span> 


5. <span data-ttu-id="d1553-136">**Spara** dina ändringar (övre vänstra hörnet av hello verktygsfältet).</span><span class="sxs-lookup"><span data-stu-id="d1553-136">**Save** your changes (top left corner of hello toolbar).</span></span> <span data-ttu-id="d1553-137">Din logikapp sparas och aktiveras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="d1553-137">Your logic app is saved and may be automatically enabled.</span></span>

> [!TIP]
> <span data-ttu-id="d1553-138">[Lagringsutforskaren](http://storageexplorer.com/) är ett bra verktyg för att hantera flera lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="d1553-138">[Storage Explorer](http://storageexplorer.com/) is a great tool too manage multiple storage accounts.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="d1553-139">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="d1553-139">Connector-specific details</span></span>

<span data-ttu-id="d1553-140">Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/azureblobconnector/).</span><span class="sxs-lookup"><span data-stu-id="d1553-140">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/azureblobconnector/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="d1553-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d1553-141">Next steps</span></span>
<span data-ttu-id="d1553-142">[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="d1553-142">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="d1553-143">Utforska hello andra tillgängliga kopplingar i Logic Apps på vår [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="d1553-143">Explore hello other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>

