---
title: "aaaGet igång med Azure Relay Hybridanslutningar i .NET | Microsoft Docs"
description: "Skriv ett C#-konsolprogram för Azure Relay-hybridanslutningar."
services: service-bus-relay
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: d1386900-b942-4abf-acfc-38d2ef826253
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 07/07/2017
ms.author: sethm
ms.openlocfilehash: 1e4af28e7cd4393c8ca965a149a0b83ebcc44f22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-relay-hybrid-connections"></a><span data-ttu-id="2125c-103">Kom igång med Relay hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="2125c-103">Get started with Relay Hybrid Connections</span></span>
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

<span data-ttu-id="2125c-104">Den här självstudiekursen innehåller en introduktion för[Azure Relay Hybridanslutningar](relay-what-is-it.md#hybrid-connections), och visar hur toouse .NET toocreate ett klientprogram som skickar meddelanden tooa motsvarande lyssnarprogram.</span><span class="sxs-lookup"><span data-stu-id="2125c-104">This tutorial provides an introduction too[Azure Relay Hybrid Connections](relay-what-is-it.md#hybrid-connections), and shows how toouse .NET toocreate a client application that sends messages tooa corresponding listener application.</span></span> 

## <a name="what-will-be-accomplished"></a><span data-ttu-id="2125c-105">Detta kommer att utföras</span><span class="sxs-lookup"><span data-stu-id="2125c-105">What will be accomplished</span></span>
<span data-ttu-id="2125c-106">Eftersom Hybridanslutningar kräver både en klient och en serverkomponent, skapar hello självstudiekursen två konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="2125c-106">Because Hybrid Connections requires both a client and a server component, hello tutorial creates two console applications.</span></span> <span data-ttu-id="2125c-107">Här är hello steg:</span><span class="sxs-lookup"><span data-stu-id="2125c-107">Here are hello steps:</span></span>

1. <span data-ttu-id="2125c-108">Skapa en Relay-namnområde med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2125c-108">Create a Relay namespace, using hello Azure portal.</span></span>
2. <span data-ttu-id="2125c-109">Skapa en hybridanslutning i detta namnområde med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2125c-109">Create a hybrid connection in that namespace, using hello Azure portal.</span></span>
3. <span data-ttu-id="2125c-110">Skriva serverkonsolen (lyssnare) tooreceive programmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="2125c-110">Write a server (listener) console application tooreceive messages.</span></span>
4. <span data-ttu-id="2125c-111">Skriva klientkonsolen (avsändare) toosend programmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="2125c-111">Write a client (sender) console application toosend messages.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2125c-112">Krav</span><span class="sxs-lookup"><span data-stu-id="2125c-112">Prerequisites</span></span>

<span data-ttu-id="2125c-113">toocomplete den här kursen behöver du hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="2125c-113">toocomplete this tutorial, you'll need hello following prerequisites:</span></span>

1. <span data-ttu-id="2125c-114">[Visual Studio 2015 eller senare](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="2125c-114">[Visual Studio 2015 or higher](http://www.visualstudio.com).</span></span> <span data-ttu-id="2125c-115">hello exemplen i den här självstudiekursen använder Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="2125c-115">hello examples in this tutorial use Visual Studio 2017.</span></span>
2. <span data-ttu-id="2125c-116">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2125c-116">An Azure subscription.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-hello-azure-portal"></a><span data-ttu-id="2125c-117">1. Skapa ett namnområde med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2125c-117">1. Create a namespace using hello Azure portal</span></span>
<span data-ttu-id="2125c-118">Om du redan har skapat en Relay-namnrymd hoppa toohello [skapa en hybridanslutning med hello Azure-portalen](#2-create-a-hybrid-connection-using-the-azure-portal) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2125c-118">If you have already created a Relay namespace, jump toohello [Create a hybrid connection using hello Azure portal](#2-create-a-hybrid-connection-using-the-azure-portal) section.</span></span>

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-hello-azure-portal"></a><span data-ttu-id="2125c-119">2. Skapa en hybridanslutning med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2125c-119">2. Create a hybrid connection using hello Azure portal</span></span>
<span data-ttu-id="2125c-120">Om du redan har skapat en hybridanslutning hoppa toohello [skapa ett serverprogram](#3-create-a-server-application-listener) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2125c-120">If you have already created a hybrid connection, jump toohello [Create a server application](#3-create-a-server-application-listener) section.</span></span>

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a><span data-ttu-id="2125c-121">3. Skapa ett serverprogram (lyssnare)</span><span class="sxs-lookup"><span data-stu-id="2125c-121">3. Create a server application (listener)</span></span>
<span data-ttu-id="2125c-122">toolisten och ta emot meddelanden från hello Relay, kommer vi att skriva en C#-konsolapp med Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2125c-122">toolisten and receive messages from hello Relay, we will write a C# console application using Visual Studio.</span></span>

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-dotnet-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a><span data-ttu-id="2125c-123">4. Skapa ett klientprogram (avsändare)</span><span class="sxs-lookup"><span data-stu-id="2125c-123">4. Create a client application (sender)</span></span>
<span data-ttu-id="2125c-124">toosend meddelanden toohello vidarebefordra vi kommer att skriva en C#-konsolapp med Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2125c-124">toosend messages toohello Relay, we will write a C# console application using Visual Studio.</span></span>

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-dotnet-get-started-client.md)]

## <a name="5-run-hello-applications"></a><span data-ttu-id="2125c-125">5. Köra hello program</span><span class="sxs-lookup"><span data-stu-id="2125c-125">5. Run hello applications</span></span>
1. <span data-ttu-id="2125c-126">Kör hello-serverprogram.</span><span class="sxs-lookup"><span data-stu-id="2125c-126">Run hello server application.</span></span>
2. <span data-ttu-id="2125c-127">Kör hello klientprogrammet och skriva in text.</span><span class="sxs-lookup"><span data-stu-id="2125c-127">Run hello client application and enter some text.</span></span>
3. <span data-ttu-id="2125c-128">Se till att servern hello programmet konsolen utdata hello text som angetts i hello-klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="2125c-128">Ensure that hello server application console outputs hello text that was entered in hello client application.</span></span>

![running-applications](./media/relay-hybrid-connections-dotnet-get-started/running-applications.png)

<span data-ttu-id="2125c-130">Grattis, du har skapat ett end-to-end hybridanslutningsprogram.</span><span class="sxs-lookup"><span data-stu-id="2125c-130">Congratulations, you have created an end-to-end Hybrid Connections application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2125c-131">Nästa steg:</span><span class="sxs-lookup"><span data-stu-id="2125c-131">Next steps:</span></span>
* [<span data-ttu-id="2125c-132">Vanliga frågor och svar om Relay</span><span class="sxs-lookup"><span data-stu-id="2125c-132">Relay FAQ</span></span>](relay-faq.md)
* [<span data-ttu-id="2125c-133">Skapa ett namnområde</span><span class="sxs-lookup"><span data-stu-id="2125c-133">Create a namespace</span></span>](relay-create-namespace-portal.md)
* [<span data-ttu-id="2125c-134">Kom igång med Node</span><span class="sxs-lookup"><span data-stu-id="2125c-134">Get started with Node</span></span>](relay-hybrid-connections-node-get-started.md)

