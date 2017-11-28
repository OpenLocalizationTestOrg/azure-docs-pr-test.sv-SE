---
title: "aaaGet igång med Azure Relay Hybridanslutningar i noden | Microsoft Docs"
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
ms.openlocfilehash: 235548399570074f7fd160fec28de8d3633625c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-relay-hybrid-connections"></a><span data-ttu-id="e978b-103">Kom igång med Relay hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="e978b-103">Get started with Relay Hybrid Connections</span></span>

[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

<span data-ttu-id="e978b-104">Den här självstudiekursen innehåller en introduktion för[Azure Relay Hybridanslutningar](relay-what-is-it.md#hybrid-connections), och visar hur toouse Node.js toocreate ett klientprogram som skickar meddelanden tooa motsvarande lyssnarprogram.</span><span class="sxs-lookup"><span data-stu-id="e978b-104">This tutorial provides an introduction too[Azure Relay Hybrid Connections](relay-what-is-it.md#hybrid-connections), and shows how toouse Node.js toocreate a client application that sends messages tooa corresponding listener application.</span></span> 

## <a name="what-will-be-accomplished"></a><span data-ttu-id="e978b-105">Detta kommer att utföras</span><span class="sxs-lookup"><span data-stu-id="e978b-105">What will be accomplished</span></span>

<span data-ttu-id="e978b-106">Eftersom hybridanslutningar kräver både en klient och en serverkomponent, kommer vi att skapa två konsolprogram i den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="e978b-106">Because Hybrid Connections requires both a client and a server component, we will create two console applications in this tutorial.</span></span> <span data-ttu-id="e978b-107">Här är hello steg:</span><span class="sxs-lookup"><span data-stu-id="e978b-107">Here are hello steps:</span></span>

1. <span data-ttu-id="e978b-108">Skapa en Relay-namnområde med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e978b-108">Create a Relay namespace, using hello Azure portal.</span></span>
2. <span data-ttu-id="e978b-109">Skapa en hybridanslutning med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e978b-109">Create a hybrid connection, using hello Azure portal.</span></span>
3. <span data-ttu-id="e978b-110">Skriv en server konsolen tooreceive programmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="e978b-110">Write a server console application tooreceive messages.</span></span>
4. <span data-ttu-id="e978b-111">Skriv en klient konsolen toosend programmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="e978b-111">Write a client console application toosend messages.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e978b-112">Krav</span><span class="sxs-lookup"><span data-stu-id="e978b-112">Prerequisites</span></span>

1. <span data-ttu-id="e978b-113">[Node.js](https://nodejs.org/en/).</span><span class="sxs-lookup"><span data-stu-id="e978b-113">[Node.js](https://nodejs.org/en/).</span></span>
2. <span data-ttu-id="e978b-114">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e978b-114">An Azure subscription.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-hello-azure-portal"></a><span data-ttu-id="e978b-115">1. Skapa ett namnområde med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e978b-115">1. Create a namespace using hello Azure portal</span></span>

<span data-ttu-id="e978b-116">Om du redan har en Relay-namnområdet som skapade hoppa toohello [skapa en hybridanslutning med hello Azure-portalen](#2-create-a-hybrid-connection-using-the-azure-portal) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e978b-116">If you already have a Relay namespace created, jump toohello [Create a hybrid connection using hello Azure portal](#2-create-a-hybrid-connection-using-the-azure-portal) section.</span></span>

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-hello-azure-portal"></a><span data-ttu-id="e978b-117">2. Skapa en hybridanslutning med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e978b-117">2. Create a hybrid connection using hello Azure portal</span></span>

<span data-ttu-id="e978b-118">Om du redan har en hybridanslutning skapade hoppa toohello [skapa ett serverprogram](#3-create-a-server-application-listener) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e978b-118">If you already have a hybrid connection created, jump toohello [Create a server application](#3-create-a-server-application-listener) section.</span></span>

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a><span data-ttu-id="e978b-119">3. Skapa ett serverprogram (lyssnare)</span><span class="sxs-lookup"><span data-stu-id="e978b-119">3. Create a server application (listener)</span></span>

<span data-ttu-id="e978b-120">toolisten och ta emot meddelanden från hello Relay, kommer vi att skriva ett Node.js-konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="e978b-120">toolisten and receive messages from hello Relay, we will write a Node.js console application.</span></span>

[!INCLUDE [relay-hybrid-connections-node-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a><span data-ttu-id="e978b-121">4. Skapa ett klientprogram (avsändare)</span><span class="sxs-lookup"><span data-stu-id="e978b-121">4. Create a client application (sender)</span></span>

<span data-ttu-id="e978b-122">toosend meddelanden toohello Relay, kommer vi att skriva ett Node.js-konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="e978b-122">toosend messages toohello Relay, we will write a Node.js console application.</span></span>

[!INCLUDE [relay-hybrid-connections-node-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

## <a name="5-run-hello-applications"></a><span data-ttu-id="e978b-123">5. Köra hello program</span><span class="sxs-lookup"><span data-stu-id="e978b-123">5. Run hello applications</span></span>

1. <span data-ttu-id="e978b-124">Kör serverprogrammet hello: från en kommandotolk Node.js-typ `node listener.js`.</span><span class="sxs-lookup"><span data-stu-id="e978b-124">Run hello server application: from a Node.js command prompt type `node listener.js`.</span></span>
2. <span data-ttu-id="e978b-125">Köra hello-klientprogram: från en Node.js-Kommandotolken skriver `node sender.js`, och ange lite text.</span><span class="sxs-lookup"><span data-stu-id="e978b-125">Run hello client application: from a Node.js command prompt type `node sender.js`, and enter some text.</span></span>
3. <span data-ttu-id="e978b-126">Se till att servern hello programmet konsolen utdata hello text som angetts i hello-klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="e978b-126">Ensure that hello server application console outputs hello text that was entered in hello client application.</span></span>

![running-applications](./media/relay-hybrid-connections-node-get-started/running-applications.png)

<span data-ttu-id="e978b-128">Grattis, du har skapat ett hybridanslutningsprogram från slutpunkt till slutpunkt med hjälp av Node.js!</span><span class="sxs-lookup"><span data-stu-id="e978b-128">Congratulations, you have created an end-to-end Hybrid Connections application using Node.js!</span></span>

## <a name="next-steps"></a><span data-ttu-id="e978b-129">Nästa steg:</span><span class="sxs-lookup"><span data-stu-id="e978b-129">Next steps:</span></span>

* [<span data-ttu-id="e978b-130">Vanliga frågor och svar om Relay</span><span class="sxs-lookup"><span data-stu-id="e978b-130">Relay FAQ</span></span>](relay-faq.md)
* [<span data-ttu-id="e978b-131">Skapa ett namnområde</span><span class="sxs-lookup"><span data-stu-id="e978b-131">Create a namespace</span></span>](relay-create-namespace-portal.md)
* [<span data-ttu-id="e978b-132">Kom igång med .NET</span><span class="sxs-lookup"><span data-stu-id="e978b-132">Get started with .NET</span></span>](relay-hybrid-connections-dotnet-get-started.md)
* [<span data-ttu-id="e978b-133">Kom igång med Node</span><span class="sxs-lookup"><span data-stu-id="e978b-133">Get started with Node</span></span>](relay-hybrid-connections-node-get-started.md)

