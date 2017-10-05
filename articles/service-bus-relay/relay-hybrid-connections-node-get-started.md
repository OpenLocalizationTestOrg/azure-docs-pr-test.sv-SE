---
title: "Komma igång med Azure Relay-hybridanslutningar i Node | Microsoft Docs"
description: "Skriv ett Node.js-konsolprogram för Azure Relay-hybridanslutningar."
services: service-bus-relay
documentationcenter: node
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e44e4867-3cf3-46be-8f8a-7671e2013bc4
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: node
ms.workload: na
ms.date: 07/07/2017
ms.author: sethm
ms.openlocfilehash: c3bfc45969f250059988129f532edd12dfe3dcfe
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-relay-hybrid-connections"></a><span data-ttu-id="e6b38-103">Kom igång med Relay hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="e6b38-103">Get started with Relay Hybrid Connections</span></span>

[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

<span data-ttu-id="e6b38-104">I den här självstudien ger dig en introduktion till [Azure Relay-hybridanslutningar](relay-what-is-it.md#hybrid-connections) och visar hur du använder Node.js för att skapa ett klientprogram som skickar meddelanden till motsvarande lyssnarprogram.</span><span class="sxs-lookup"><span data-stu-id="e6b38-104">This tutorial provides an introduction to [Azure Relay Hybrid Connections](relay-what-is-it.md#hybrid-connections), and shows how to use Node.js to create a client application that sends messages to a corresponding listener application.</span></span> 

## <a name="what-will-be-accomplished"></a><span data-ttu-id="e6b38-105">Detta kommer att utföras</span><span class="sxs-lookup"><span data-stu-id="e6b38-105">What will be accomplished</span></span>

<span data-ttu-id="e6b38-106">Eftersom hybridanslutningar kräver både en klient och en serverkomponent, kommer vi att skapa två konsolprogram i den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="e6b38-106">Because Hybrid Connections requires both a client and a server component, we will create two console applications in this tutorial.</span></span> <span data-ttu-id="e6b38-107">Här är stegen:</span><span class="sxs-lookup"><span data-stu-id="e6b38-107">Here are the steps:</span></span>

1. <span data-ttu-id="e6b38-108">Skapa ett Relay-namnområde med Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="e6b38-108">Create a Relay namespace, using the Azure portal.</span></span>
2. <span data-ttu-id="e6b38-109">Skapa en hybridanslutning med Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="e6b38-109">Create a hybrid connection, using the Azure portal.</span></span>
3. <span data-ttu-id="e6b38-110">Skriv ett serverkonsolprogram för att ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="e6b38-110">Write a server console application to receive messages.</span></span>
4. <span data-ttu-id="e6b38-111">Skriv ett klientkonsolprogram för att ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="e6b38-111">Write a client console application to send messages.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e6b38-112">Krav</span><span class="sxs-lookup"><span data-stu-id="e6b38-112">Prerequisites</span></span>

1. <span data-ttu-id="e6b38-113">[Node.js](https://nodejs.org/en/).</span><span class="sxs-lookup"><span data-stu-id="e6b38-113">[Node.js](https://nodejs.org/en/).</span></span>
2. <span data-ttu-id="e6b38-114">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e6b38-114">An Azure subscription.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a><span data-ttu-id="e6b38-115">1. Skapa ett namnområde med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e6b38-115">1. Create a namespace using the Azure portal</span></span>

<span data-ttu-id="e6b38-116">Om du redan har skapat ett Relay-namnområde, går du vidare till avsnittet [Skapa en hybridanslutning med Azure Portal](#2-create-a-hybrid-connection-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="e6b38-116">If you already have a Relay namespace created, jump to the [Create a hybrid connection using the Azure portal](#2-create-a-hybrid-connection-using-the-azure-portal) section.</span></span>

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a><span data-ttu-id="e6b38-117">2. Skapa en hybridanslutning med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e6b38-117">2. Create a hybrid connection using the Azure portal</span></span>

<span data-ttu-id="e6b38-118">Om du redan har skapat en hybridanslutning, går du vidare till avsnittet [skapa ett serverprogram](#3-create-a-server-application-listener).</span><span class="sxs-lookup"><span data-stu-id="e6b38-118">If you already have a hybrid connection created, jump to the [Create a server application](#3-create-a-server-application-listener) section.</span></span>

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a><span data-ttu-id="e6b38-119">3. Skapa ett serverprogram (lyssnare)</span><span class="sxs-lookup"><span data-stu-id="e6b38-119">3. Create a server application (listener)</span></span>

<span data-ttu-id="e6b38-120">För att lyssna på och ta emot meddelanden från Relay skriver du ett Node.js-konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="e6b38-120">To listen and receive messages from the Relay, we will write a Node.js console application.</span></span>

[!INCLUDE [relay-hybrid-connections-node-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a><span data-ttu-id="e6b38-121">4. Skapa ett klientprogram (avsändare)</span><span class="sxs-lookup"><span data-stu-id="e6b38-121">4. Create a client application (sender)</span></span>

<span data-ttu-id="e6b38-122">För att skicka meddelanden till Relay skriver du ett Node.js-konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="e6b38-122">To send messages to the Relay, we will write a Node.js console application.</span></span>

[!INCLUDE [relay-hybrid-connections-node-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

## <a name="5-run-the-applications"></a><span data-ttu-id="e6b38-123">5. Köra programmen</span><span class="sxs-lookup"><span data-stu-id="e6b38-123">5. Run the applications</span></span>

1. <span data-ttu-id="e6b38-124">Kör serverprogrammet: i en Node.js-kommandotolk skriver du in `node listener.js`.</span><span class="sxs-lookup"><span data-stu-id="e6b38-124">Run the server application: from a Node.js command prompt type `node listener.js`.</span></span>
2. <span data-ttu-id="e6b38-125">Kör klientprogrammet: från en Node.js-kommandotolken skriver du in `node sender.js` och anger lite text.</span><span class="sxs-lookup"><span data-stu-id="e6b38-125">Run the client application: from a Node.js command prompt type `node sender.js`, and enter some text.</span></span>
3. <span data-ttu-id="e6b38-126">Se till att serverprogramkonsolen matar ut den text som angavs i klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="e6b38-126">Ensure that the server application console outputs the text that was entered in the client application.</span></span>

![running-applications](./media/relay-hybrid-connections-node-get-started/running-applications.png)

<span data-ttu-id="e6b38-128">Grattis, du har skapat ett hybridanslutningsprogram från slutpunkt till slutpunkt med hjälp av Node.js!</span><span class="sxs-lookup"><span data-stu-id="e6b38-128">Congratulations, you have created an end-to-end Hybrid Connections application using Node.js!</span></span>

## <a name="next-steps"></a><span data-ttu-id="e6b38-129">Nästa steg:</span><span class="sxs-lookup"><span data-stu-id="e6b38-129">Next steps:</span></span>

* [<span data-ttu-id="e6b38-130">Vanliga frågor och svar om Relay</span><span class="sxs-lookup"><span data-stu-id="e6b38-130">Relay FAQ</span></span>](relay-faq.md)
* [<span data-ttu-id="e6b38-131">Skapa ett namnområde</span><span class="sxs-lookup"><span data-stu-id="e6b38-131">Create a namespace</span></span>](relay-create-namespace-portal.md)
* [<span data-ttu-id="e6b38-132">Kom igång med .NET</span><span class="sxs-lookup"><span data-stu-id="e6b38-132">Get started with .NET</span></span>](relay-hybrid-connections-dotnet-get-started.md)
* [<span data-ttu-id="e6b38-133">Kom igång med Node</span><span class="sxs-lookup"><span data-stu-id="e6b38-133">Get started with Node</span></span>](relay-hybrid-connections-node-get-started.md)

