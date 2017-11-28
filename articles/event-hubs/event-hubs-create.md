---
title: "Skapa en Azure-händelsehubb | Microsoft Docs"
description: "Skapa ett namnområde för Azure Event Hubs och en händelsehubb med hjälp av Azure portal"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: ff99e327-c8db-4354-9040-9c60c51a2191
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2017
ms.author: sethm
ms.openlocfilehash: 816bf1426704d3391550e80c0700f1b011683a94
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-event-hubs-namespace-and-an-event-hub-using-the-azure-portal"></a><span data-ttu-id="484ed-103">Skapa ett namnområde för Händelsehubbar och en händelsehubb med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="484ed-103">Create an Event Hubs namespace and an event hub using the Azure portal</span></span>

## <a name="create-an-event-hubs-namespace"></a><span data-ttu-id="484ed-104">Skapa ett namnområde för Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="484ed-104">Create an Event Hubs namespace</span></span>
1. <span data-ttu-id="484ed-105">Logga in på [Azure Portal][Azure portal] och klicka på **Ny** högst upp till vänster på skärmen.</span><span class="sxs-lookup"><span data-stu-id="484ed-105">Log on to the [Azure portal][Azure portal], and click **New** at the top left of the screen.</span></span>
1. <span data-ttu-id="484ed-106">Klicka på **Sakernas Internet**, och klicka sedan på **Händelsehubbar**.</span><span class="sxs-lookup"><span data-stu-id="484ed-106">Click **Internet of Things**, and then click **Event Hubs**.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub9.png)
1. <span data-ttu-id="484ed-107">I bladet **Skapa namnområde** anger du ett namn för namnområdet.</span><span class="sxs-lookup"><span data-stu-id="484ed-107">In the **Create namespace** blade, enter a namespace name.</span></span> <span data-ttu-id="484ed-108">Systemet kontrollerar omedelbart om namnet är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="484ed-108">The system immediately checks to see if the name is available.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub1.png)
1. <span data-ttu-id="484ed-109">När du har kontrollerat att namnet för namnområdet är tillgängligt, väljer du prisnivå (Basic eller Standard).</span><span class="sxs-lookup"><span data-stu-id="484ed-109">After making sure the namespace name is available, choose the pricing tier (Basic or Standard).</span></span> <span data-ttu-id="484ed-110">Välj även en Azure-prenumeration, resursgrupp och plats där du vill skapa resursen.</span><span class="sxs-lookup"><span data-stu-id="484ed-110">Also, choose an Azure subscription, resource group, and location in which to create the resource.</span></span> 
1. <span data-ttu-id="484ed-111">Klicka på **Skapa** för att skapa namnområdet.</span><span class="sxs-lookup"><span data-stu-id="484ed-111">Click **Create** to create the namespace.</span></span> <span data-ttu-id="484ed-112">Du kan behöva vänta några minuter för systemet att fullständigt etablera resurser.</span><span class="sxs-lookup"><span data-stu-id="484ed-112">You may have to wait a few minutes for the system to fully provision the resources.</span></span>
2. <span data-ttu-id="484ed-113">Klicka på det nyligen skapade namnområden i portalen listan över namnområden.</span><span class="sxs-lookup"><span data-stu-id="484ed-113">In the portal list of namespaces, click the newly created namespace.</span></span>
2. <span data-ttu-id="484ed-114">Klicka på **principer för delad åtkomst**, och klicka sedan på **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="484ed-114">Click **Shared access policies**, and then click **RootManageSharedAccessKey**.</span></span>
    
    ![](./media/event-hubs-create/create-event-hub7.png)

3. <span data-ttu-id="484ed-115">Klicka på kopieringsknappen för att kopiera anslutningssträngen **RootManageSharedAccessKey** till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="484ed-115">Click the copy button to copy the **RootManageSharedAccessKey** connection string to the clipboard.</span></span> <span data-ttu-id="484ed-116">Spara den här anslutningssträngen i en tillfällig plats, till exempel Anteckningar för senare användning.</span><span class="sxs-lookup"><span data-stu-id="484ed-116">Save this connection string in a temporary location, such as Notepad, to use later.</span></span>
    
    ![](./media/event-hubs-create/create-event-hub8.png)

## <a name="create-an-event-hub"></a><span data-ttu-id="484ed-117">Skapa en händelsehubb</span><span class="sxs-lookup"><span data-stu-id="484ed-117">Create an event hub</span></span>

1. <span data-ttu-id="484ed-118">Klicka på det nya namnområdet i listan Händelsehubbar namnområde.</span><span class="sxs-lookup"><span data-stu-id="484ed-118">In the Event Hubs namespace list, click the newly created namespace.</span></span>      
   
    ![](./media/event-hubs-create/create-event-hub2.png) 

2. <span data-ttu-id="484ed-119">Klicka på namnområdesbladet och på **Händelsehubbar**.</span><span class="sxs-lookup"><span data-stu-id="484ed-119">In the namespace blade, click **Event Hubs**.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub3.png)

1. <span data-ttu-id="484ed-120">Klicka på **Lägg till händelsehubb** överst på bladet.</span><span class="sxs-lookup"><span data-stu-id="484ed-120">At the top of the blade, click **Add Event Hub**.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub4.png)
1. <span data-ttu-id="484ed-121">Skriv ett namn för din händelsehubb, och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="484ed-121">Type a name for your event hub, then click **Create**.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub5.png)

<span data-ttu-id="484ed-122">Din händelsehubb har nu skapats och du har anslutningssträngar för måste du skicka och ta emot händelser.</span><span class="sxs-lookup"><span data-stu-id="484ed-122">Your event hub is now created, and you have the connection strings you need to send and receive events.</span></span>

## <a name="next-steps"></a><span data-ttu-id="484ed-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="484ed-123">Next steps</span></span>
<span data-ttu-id="484ed-124">Mer information om Händelsehubbar finns i följande länkar:</span><span class="sxs-lookup"><span data-stu-id="484ed-124">To learn more about Event Hubs, visit these links:</span></span>

* [<span data-ttu-id="484ed-125">Event Hubs-översikt</span><span class="sxs-lookup"><span data-stu-id="484ed-125">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="484ed-126">Event Hubs API-översikt</span><span class="sxs-lookup"><span data-stu-id="484ed-126">Event Hubs API overview</span></span>](event-hubs-api-overview.md)

[Azure portal]: https://portal.azure.com/