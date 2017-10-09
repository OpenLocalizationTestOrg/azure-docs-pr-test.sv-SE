---
title: "aaaCreate en Azure-händelsehubb | Microsoft Docs"
description: "Skapa ett namnområde för Azure Event Hubs och en händelsehubb med hjälp av hello Azure-portalen"
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
ms.openlocfilehash: 9a8b7711e2ca7d112e24be19353d43c365ff6935
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-event-hubs-namespace-and-an-event-hub-using-hello-azure-portal"></a><span data-ttu-id="1095c-103">Skapa ett namnområde för Händelsehubbar och en händelsehubb med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1095c-103">Create an Event Hubs namespace and an event hub using hello Azure portal</span></span>

## <a name="create-an-event-hubs-namespace"></a><span data-ttu-id="1095c-104">Skapa ett namnområde för Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="1095c-104">Create an Event Hubs namespace</span></span>
1. <span data-ttu-id="1095c-105">Logga in toohello [Azure-portalen][Azure portal], och klicka på **ny** på hello upp till vänster i hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="1095c-105">Log on toohello [Azure portal][Azure portal], and click **New** at hello top left of hello screen.</span></span>
1. <span data-ttu-id="1095c-106">Klicka på **Sakernas Internet**, och klicka sedan på **Händelsehubbar**.</span><span class="sxs-lookup"><span data-stu-id="1095c-106">Click **Internet of Things**, and then click **Event Hubs**.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub9.png)
1. <span data-ttu-id="1095c-107">I hello **skapa namnområdet** bladet, ange ett namn för namnområdet.</span><span class="sxs-lookup"><span data-stu-id="1095c-107">In hello **Create namespace** blade, enter a namespace name.</span></span> <span data-ttu-id="1095c-108">hello systemet kontrollerar omedelbart toosee om hello namn är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="1095c-108">hello system immediately checks toosee if hello name is available.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub1.png)
1. <span data-ttu-id="1095c-109">När du gör att hello namnområdesnamnet är tillgänglig, väljer du hello prisnivån (Basic eller Standard).</span><span class="sxs-lookup"><span data-stu-id="1095c-109">After making sure hello namespace name is available, choose hello pricing tier (Basic or Standard).</span></span> <span data-ttu-id="1095c-110">Också välja en Azure-prenumeration, resursgrupp och plats i vilken toocreate hello-resurs.</span><span class="sxs-lookup"><span data-stu-id="1095c-110">Also, choose an Azure subscription, resource group, and location in which toocreate hello resource.</span></span> 
1. <span data-ttu-id="1095c-111">Klicka på **skapa** toocreate hello namnområde.</span><span class="sxs-lookup"><span data-stu-id="1095c-111">Click **Create** toocreate hello namespace.</span></span> <span data-ttu-id="1095c-112">Du kan ha toowait några minuter för hello toofully etablera hello systemresurser.</span><span class="sxs-lookup"><span data-stu-id="1095c-112">You may have toowait a few minutes for hello system toofully provision hello resources.</span></span>
2. <span data-ttu-id="1095c-113">Hello nyskapad namnområdet på hello portal listan namnområden.</span><span class="sxs-lookup"><span data-stu-id="1095c-113">In hello portal list of namespaces, click hello newly created namespace.</span></span>
2. <span data-ttu-id="1095c-114">Klicka på **principer för delad åtkomst**, och klicka sedan på **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="1095c-114">Click **Shared access policies**, and then click **RootManageSharedAccessKey**.</span></span>
    
    ![](./media/event-hubs-create/create-event-hub7.png)

3. <span data-ttu-id="1095c-115">Klicka på hello Kopiera knappen toocopy hello **RootManageSharedAccessKey** anslutning sträng toohello Urklipp.</span><span class="sxs-lookup"><span data-stu-id="1095c-115">Click hello copy button toocopy hello **RootManageSharedAccessKey** connection string toohello clipboard.</span></span> <span data-ttu-id="1095c-116">Spara den här anslutningssträngen i en tillfällig plats, till exempel Anteckningar, toouse senare.</span><span class="sxs-lookup"><span data-stu-id="1095c-116">Save this connection string in a temporary location, such as Notepad, toouse later.</span></span>
    
    ![](./media/event-hubs-create/create-event-hub8.png)

## <a name="create-an-event-hub"></a><span data-ttu-id="1095c-117">Skapa en händelsehubb</span><span class="sxs-lookup"><span data-stu-id="1095c-117">Create an event hub</span></span>

1. <span data-ttu-id="1095c-118">Klicka på hello nyskapad namnområde i hello Händelsehubbar namnområde.</span><span class="sxs-lookup"><span data-stu-id="1095c-118">In hello Event Hubs namespace list, click hello newly created namespace.</span></span>      
   
    ![](./media/event-hubs-create/create-event-hub2.png) 

2. <span data-ttu-id="1095c-119">I hello namnområde bladet, klickar du på **Händelsehubbar**.</span><span class="sxs-lookup"><span data-stu-id="1095c-119">In hello namespace blade, click **Event Hubs**.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub3.png)

1. <span data-ttu-id="1095c-120">Hello överkant hello-bladet, klickar du på **lägga till Händelsehubben**.</span><span class="sxs-lookup"><span data-stu-id="1095c-120">At hello top of hello blade, click **Add Event Hub**.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub4.png)
1. <span data-ttu-id="1095c-121">Skriv ett namn för din händelsehubb, och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="1095c-121">Type a name for your event hub, then click **Create**.</span></span>
   
    ![](./media/event-hubs-create/create-event-hub5.png)

<span data-ttu-id="1095c-122">Din händelsehubb har nu skapats och du har anslutningssträngar för hello du behöver toosend och ta emot händelser.</span><span class="sxs-lookup"><span data-stu-id="1095c-122">Your event hub is now created, and you have hello connection strings you need toosend and receive events.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1095c-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1095c-123">Next steps</span></span>
<span data-ttu-id="1095c-124">toolearn mer information om Händelsehubbar, finns i följande länkar:</span><span class="sxs-lookup"><span data-stu-id="1095c-124">toolearn more about Event Hubs, visit these links:</span></span>

* [<span data-ttu-id="1095c-125">Event Hubs-översikt</span><span class="sxs-lookup"><span data-stu-id="1095c-125">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="1095c-126">Event Hubs API-översikt</span><span class="sxs-lookup"><span data-stu-id="1095c-126">Event Hubs API overview</span></span>](event-hubs-api-overview.md)

[Azure portal]: https://portal.azure.com/