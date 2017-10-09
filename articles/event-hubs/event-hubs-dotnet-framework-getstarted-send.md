---
title: "aaaSend händelser tooAzure Händelsehubbar med hjälp av hello .NET Framework | Microsoft Docs"
description: "Komma igång med att skicka händelser tooEvent Hub genom att använda hello .NET Framework"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: c4974bd3-2a79-48a1-aa3b-8ee2d6655b28
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/12/2017
ms.author: sethm
ms.openlocfilehash: 05514546a6094096e4a3c800db058190076de80a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="send-events-tooazure-event-hubs-using-hello-net-framework"></a><span data-ttu-id="6effc-103">Skicka händelser tooAzure Händelsehubbar med hello .NET Framework</span><span class="sxs-lookup"><span data-stu-id="6effc-103">Send events tooAzure Event Hubs using hello .NET Framework</span></span>

## <a name="introduction"></a><span data-ttu-id="6effc-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="6effc-104">Introduction</span></span>

<span data-ttu-id="6effc-105">Händelsehubbar är en tjänst som bearbetar stora mängder händelsedata (telemetri) från anslutna enheter och program.</span><span class="sxs-lookup"><span data-stu-id="6effc-105">Event Hubs is a service that processes large amounts of event data (telemetry) from connected devices and applications.</span></span> <span data-ttu-id="6effc-106">När du samlar in data i Händelsehubbar kan du lagra hello data med ett lagringskluster eller omvandla dem med hjälp av en leverantör av realtidsanalys.</span><span class="sxs-lookup"><span data-stu-id="6effc-106">After you collect data into Event Hubs, you can store hello data using a storage cluster or transform it using a real-time analytics provider.</span></span> <span data-ttu-id="6effc-107">Den här storskaliga händelse och bearbetningsfunktionen är en viktig komponent inom moderna programarkitekturer inklusive hello Sakernas Internet (IoT).</span><span class="sxs-lookup"><span data-stu-id="6effc-107">This large-scale event collection and processing capability is a key component of modern application architectures including hello Internet of Things (IoT).</span></span>

<span data-ttu-id="6effc-108">Den här kursen visar hur toouse hello [Azure-portalen](https://portal.azure.com) toocreate en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="6effc-108">This tutorial shows how toouse hello [Azure portal](https://portal.azure.com) toocreate an event hub.</span></span> <span data-ttu-id="6effc-109">Den visar även hur hello toosend händelser tooan händelsehubb med hjälp av ett konsolprogram som skrivits i C# med hjälp av .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="6effc-109">It also shows how toosend events tooan event hub using a console application written in C# using hello .NET Framework.</span></span> <span data-ttu-id="6effc-110">tooreceive händelser med hjälp av hello .NET Framework finns hello [ta emot händelser med hjälp av hello .NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md) artikel, eller klicka på hello mottagande språket i hello vänstra innehållsförteckning.</span><span class="sxs-lookup"><span data-stu-id="6effc-110">tooreceive events using hello .NET Framework, see hello [Receive events using hello .NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md) article, or click hello appropriate receiving language in hello left-hand table of contents.</span></span>

<span data-ttu-id="6effc-111">toocomplete den här kursen behöver du hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="6effc-111">toocomplete this tutorial, you need hello following prerequisites:</span></span>

* <span data-ttu-id="6effc-112">[Microsoft Visual Studio 2015 eller senare](http://visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="6effc-112">[Microsoft Visual Studio 2015 or higher](http://visualstudio.com).</span></span> <span data-ttu-id="6effc-113">hello skärmdumpar i den här självstudiekursen använder Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="6effc-113">hello screen shots in this tutorial use Visual Studio 2017.</span></span>
* <span data-ttu-id="6effc-114">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="6effc-114">An active Azure account.</span></span> <span data-ttu-id="6effc-115">Om du inte har något konto kan du skapa ett utan kostnad på ett par minuter.</span><span class="sxs-lookup"><span data-stu-id="6effc-115">If you don't have one, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="6effc-116">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="6effc-116">For details, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a><span data-ttu-id="6effc-117">Skapa ett namnområde för Event Hubs och en händelsehubb</span><span class="sxs-lookup"><span data-stu-id="6effc-117">Create an Event Hubs namespace and an event hub</span></span>

<span data-ttu-id="6effc-118">hello första steget är toouse hello [Azure-portalen](https://portal.azure.com) toocreate en namnrymd Skriv Händelsehubbar och hämta hello management-autentiseringsuppgifter som programmet behöver toocommunicate med hello händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="6effc-118">hello first step is toouse hello [Azure portal](https://portal.azure.com) toocreate a namespace of type Event Hubs, and obtain hello management credentials your application needs toocommunicate with hello event hub.</span></span> <span data-ttu-id="6effc-119">toocreate ett namnområde och händelsehubb, följer du proceduren hello i [i den här artikeln](event-hubs-create.md), fortsätt sedan med hello följa stegen i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="6effc-119">toocreate a namespace and event hub, follow hello procedure in [this article](event-hubs-create.md), then proceed with hello following steps in this tutorial.</span></span>

## <a name="create-a-sender-console-application"></a><span data-ttu-id="6effc-120">Skapa ett avsändarkonsolprogram</span><span class="sxs-lookup"><span data-stu-id="6effc-120">Create a sender console application</span></span>

<span data-ttu-id="6effc-121">I det här avsnittet skriver du en Windows-konsolapp som skickar händelser tooyour händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="6effc-121">In this section, you'll write a Windows console app that sends events tooyour event hub.</span></span>

1. <span data-ttu-id="6effc-122">I Visual Studio skapar du ett nytt Visual C#-Skrivbordsapprojekt-projekt med hello **konsolprogram** projektmall.</span><span class="sxs-lookup"><span data-stu-id="6effc-122">In Visual Studio, create a new Visual C# Desktop App project using hello **Console Application** project template.</span></span> <span data-ttu-id="6effc-123">Namnet hello projektet **avsändaren**.</span><span class="sxs-lookup"><span data-stu-id="6effc-123">Name hello project **Sender**.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp1.png)
2. <span data-ttu-id="6effc-124">I Solution Explorer högerklickar du på hello **avsändaren** projektet och klicka sedan på **hantera NuGet-paket för lösningen**.</span><span class="sxs-lookup"><span data-stu-id="6effc-124">In Solution Explorer, right-click hello **Sender** project, and then click **Manage NuGet Packages for Solution**.</span></span> 
3. <span data-ttu-id="6effc-125">Klicka på hello **Bläddra** fliken, och sök sedan efter `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="6effc-125">Click hello **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="6effc-126">Klicka på **installera**, och Godkänn hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="6effc-126">Click **Install**, and accept hello terms of use.</span></span> 
   
    ![](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp2.png)
   
    <span data-ttu-id="6effc-127">Visual Studio hämtar, installerar och lägger till en referens toohello [Azure Service Bus-bibliotekets NuGet-paket](https://www.nuget.org/packages/WindowsAzure.ServiceBus).</span><span class="sxs-lookup"><span data-stu-id="6effc-127">Visual Studio downloads, installs, and adds a reference toohello [Azure Service Bus library NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus).</span></span>
4. <span data-ttu-id="6effc-128">Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:</span><span class="sxs-lookup"><span data-stu-id="6effc-128">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
  ```csharp
  using System.Threading;
  using Microsoft.ServiceBus.Messaging;
  ```
5. <span data-ttu-id="6effc-129">Lägg till följande fält toohello hello **programmet** klass, ersätter hello platshållarvärdena med namnet hello hello event hub du skapat i föregående avsnitt i hello och hello namnområdesnivå anslutningssträngen du sparat tidigare.</span><span class="sxs-lookup"><span data-stu-id="6effc-129">Add hello following fields toohello **Program** class, substituting hello placeholder values with hello name of hello event hub you created in hello previous section, and hello namespace-level connection string you saved previously.</span></span>
   
  ```csharp
  static string eventHubName = "{Event Hub name}";
  static string connectionString = "{send connection string}";
  ```
6. <span data-ttu-id="6effc-130">Lägg till följande metod toohello hello **programmet** klass:</span><span class="sxs-lookup"><span data-stu-id="6effc-130">Add hello following method toohello **Program** class:</span></span>
   
  ```csharp
  static void SendingRandomMessages()
  {
      var eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, eventHubName);
      while (true)
      {
          try
          {
              var message = Guid.NewGuid().ToString();
              Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, message);
              eventHubClient.Send(new EventData(Encoding.UTF8.GetBytes(message)));
          }
          catch (Exception exception)
          {
              Console.ForegroundColor = ConsoleColor.Red;
              Console.WriteLine("{0} > Exception: {1}", DateTime.Now, exception.Message);
              Console.ResetColor();
          }
   
          Thread.Sleep(200);
      }
  }
  ```
   
  <span data-ttu-id="6effc-131">Den här metoden skickar kontinuerligt händelser tooyour event hub med en fördröjning på 200 ms.</span><span class="sxs-lookup"><span data-stu-id="6effc-131">This method continuously sends events tooyour event hub with a 200-ms delay.</span></span>
7. <span data-ttu-id="6effc-132">Slutligen lägger du till följande rader toohello hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="6effc-132">Finally, add hello following lines toohello **Main** method:</span></span>
   
  ```csharp
  Console.WriteLine("Press Ctrl-C toostop hello sender process");
  Console.WriteLine("Press Enter toostart now");
  Console.ReadLine();
  SendingRandomMessages();
  ```
8. <span data-ttu-id="6effc-133">Kör programmet hello och kontrollera att det inte finns några fel.</span><span class="sxs-lookup"><span data-stu-id="6effc-133">Run hello program, and ensure that there are no errors.</span></span>
  
<span data-ttu-id="6effc-134">Grattis!</span><span class="sxs-lookup"><span data-stu-id="6effc-134">Congratulations!</span></span> <span data-ttu-id="6effc-135">Du har nu skickar meddelanden tooan händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="6effc-135">You have now sent messages tooan event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6effc-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6effc-136">Next steps</span></span>
<span data-ttu-id="6effc-137">Nu när du har skapat ett fungerande program som skapar en händelsehubb och skickar data, kan du gå vidare toohello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="6effc-137">Now that you've built a working application that creates an event hub and sends data, you can move on toohello following scenarios:</span></span>

* [<span data-ttu-id="6effc-138">Ta emot händelser med hjälp av hello värd för händelsebearbetning</span><span class="sxs-lookup"><span data-stu-id="6effc-138">Receive events using hello Event Processor Host</span></span>](event-hubs-dotnet-framework-getstarted-receive-eph.md)
* [<span data-ttu-id="6effc-139">Referens för händelsebearbetningsvärd</span><span class="sxs-lookup"><span data-stu-id="6effc-139">Event Processor Host reference</span></span>](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)
* [<span data-ttu-id="6effc-140">Event Hubs-översikt</span><span class="sxs-lookup"><span data-stu-id="6effc-140">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

