---
title: "Skicka händelser till Azure Event Hubs med .NET Framework| Microsoft Docs"
description: "Komma igång med att skicka händelser till Event Hubs med .NET Framework"
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
ms.openlocfilehash: 4eb0e7bcc14722010121c2a5945509d6ed736f4f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="send-events-to-azure-event-hubs-using-the-net-framework"></a><span data-ttu-id="14a3a-103">Skicka händelser till Azure Event Hubs med .NET Framework</span><span class="sxs-lookup"><span data-stu-id="14a3a-103">Send events to Azure Event Hubs using the .NET Framework</span></span>

## <a name="introduction"></a><span data-ttu-id="14a3a-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="14a3a-104">Introduction</span></span>

<span data-ttu-id="14a3a-105">Händelsehubbar är en tjänst som bearbetar stora mängder händelsedata (telemetri) från anslutna enheter och program.</span><span class="sxs-lookup"><span data-stu-id="14a3a-105">Event Hubs is a service that processes large amounts of event data (telemetry) from connected devices and applications.</span></span> <span data-ttu-id="14a3a-106">När du har samlat in data i händelsehubbar kan du lagra dem med ett lagringskluster eller omvandla dem med hjälp av en leverantör av realtidsanalys.</span><span class="sxs-lookup"><span data-stu-id="14a3a-106">After you collect data into Event Hubs, you can store the data using a storage cluster or transform it using a real-time analytics provider.</span></span> <span data-ttu-id="14a3a-107">Den här storskaliga händelseinsamlingen och bearbetningsfunktionen är en viktig komponent inom moderna programarkitekturer som t.ex. sakernas internet.</span><span class="sxs-lookup"><span data-stu-id="14a3a-107">This large-scale event collection and processing capability is a key component of modern application architectures including the Internet of Things (IoT).</span></span>

<span data-ttu-id="14a3a-108">I den här kursen får du lära dig att skapa en händelsehubb i [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="14a3a-108">This tutorial shows how to use the [Azure portal](https://portal.azure.com) to create an event hub.</span></span> <span data-ttu-id="14a3a-109">Du får också lära dig att skicka händelser till en händelsehubb med ett konsolprogram som skrivits i C# med .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="14a3a-109">It also shows how to send events to an event hub using a console application written in C# using the .NET Framework.</span></span> <span data-ttu-id="14a3a-110">För att ta emot händelser med .NET Framework kan du läsa artikeln [Receive events using the .NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md) (Ta emot händelser med .NET Framework) eller klicka på ditt mottagarspråk till vänster i innehållsförteckningen.</span><span class="sxs-lookup"><span data-stu-id="14a3a-110">To receive events using the .NET Framework, see the [Receive events using the .NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md) article, or click the appropriate receiving language in the left-hand table of contents.</span></span>

<span data-ttu-id="14a3a-111">För att slutföra den här självstudien, finns följande förhandskrav:</span><span class="sxs-lookup"><span data-stu-id="14a3a-111">To complete this tutorial, you need the following prerequisites:</span></span>

* <span data-ttu-id="14a3a-112">[Microsoft Visual Studio 2015 eller senare](http://visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="14a3a-112">[Microsoft Visual Studio 2015 or higher](http://visualstudio.com).</span></span> <span data-ttu-id="14a3a-113">För skärmdumparna i de här självstudierna används Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="14a3a-113">The screen shots in this tutorial use Visual Studio 2017.</span></span>
* <span data-ttu-id="14a3a-114">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="14a3a-114">An active Azure account.</span></span> <span data-ttu-id="14a3a-115">Om du inte har något konto kan du skapa ett utan kostnad på ett par minuter.</span><span class="sxs-lookup"><span data-stu-id="14a3a-115">If you don't have one, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="14a3a-116">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="14a3a-116">For details, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a><span data-ttu-id="14a3a-117">Skapa ett namnområde för Event Hubs och en händelsehubb</span><span class="sxs-lookup"><span data-stu-id="14a3a-117">Create an Event Hubs namespace and an event hub</span></span>

<span data-ttu-id="14a3a-118">Det första steget är att använda [Azure Portal](https://portal.azure.com) till att skapa ett namnområde av typen Event Hubs och hämta de autentiseringsuppgifter för hantering som programmet behöver för att kommunicera med händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="14a3a-118">The first step is to use the [Azure portal](https://portal.azure.com) to create a namespace of type Event Hubs, and obtain the management credentials your application needs to communicate with the event hub.</span></span> <span data-ttu-id="14a3a-119">Om du vill skapa ett namnområde och en händelsehubb följer du anvisningarna i [den här artikeln](event-hubs-create.md) och fortsätter sedan enligt följande steg i den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="14a3a-119">To create a namespace and event hub, follow the procedure in [this article](event-hubs-create.md), then proceed with the following steps in this tutorial.</span></span>

## <a name="create-a-sender-console-application"></a><span data-ttu-id="14a3a-120">Skapa ett avsändarkonsolprogram</span><span class="sxs-lookup"><span data-stu-id="14a3a-120">Create a sender console application</span></span>

<span data-ttu-id="14a3a-121">I det här avsnittet skriver du en Windows-konsolapp som skickar händelser till din händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="14a3a-121">In this section, you'll write a Windows console app that sends events to your event hub.</span></span>

1. <span data-ttu-id="14a3a-122">I Visual Studio skapar du ett nytt Visual C#-skrivbordsapprojekt med hjälp av projektmallen **Konsolprogram**.</span><span class="sxs-lookup"><span data-stu-id="14a3a-122">In Visual Studio, create a new Visual C# Desktop App project using the **Console Application** project template.</span></span> <span data-ttu-id="14a3a-123">Namnge projektet **Avsändare**.</span><span class="sxs-lookup"><span data-stu-id="14a3a-123">Name the project **Sender**.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp1.png)
2. <span data-ttu-id="14a3a-124">Högerklicka på projektet **Avsändare** i Solution Explorer och klicka sedan på **Hantera NuGet-paket för lösningen**.</span><span class="sxs-lookup"><span data-stu-id="14a3a-124">In Solution Explorer, right-click the **Sender** project, and then click **Manage NuGet Packages for Solution**.</span></span> 
3. <span data-ttu-id="14a3a-125">Klicka på **Bläddra**-fliken och sök sedan efter `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="14a3a-125">Click the **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="14a3a-126">Klicka på **Installera** och godkänn användningsvillkoren.</span><span class="sxs-lookup"><span data-stu-id="14a3a-126">Click **Install**, and accept the terms of use.</span></span> 
   
    ![](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp2.png)
   
    <span data-ttu-id="14a3a-127">Visual Studio laddar ned, installerar och lägger till en referens till [Azure Service Bus-bibliotekets NuGet-paket](https://www.nuget.org/packages/WindowsAzure.ServiceBus).</span><span class="sxs-lookup"><span data-stu-id="14a3a-127">Visual Studio downloads, installs, and adds a reference to the [Azure Service Bus library NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus).</span></span>
4. <span data-ttu-id="14a3a-128">Lägg till följande `using`-uttryck överst i **Program.cs**-filen:</span><span class="sxs-lookup"><span data-stu-id="14a3a-128">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
  ```csharp
  using System.Threading;
  using Microsoft.ServiceBus.Messaging;
  ```
5. <span data-ttu-id="14a3a-129">Lägg till följande fält i klassen **Program**, och ersätt platshållarvärdena med namnet på den händelsehubb du skapade i föregående avsnitt samt anslutningssträngen på namnområdesnivå som du sparat tidigare.</span><span class="sxs-lookup"><span data-stu-id="14a3a-129">Add the following fields to the **Program** class, substituting the placeholder values with the name of the event hub you created in the previous section, and the namespace-level connection string you saved previously.</span></span>
   
  ```csharp
  static string eventHubName = "{Event Hub name}";
  static string connectionString = "{send connection string}";
  ```
6. <span data-ttu-id="14a3a-130">Lägg till följande metod i klassen **Program**:</span><span class="sxs-lookup"><span data-stu-id="14a3a-130">Add the following method to the **Program** class:</span></span>
   
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
   
  <span data-ttu-id="14a3a-131">Med den här metoden skickas kontinuerligt händelser till din händelsehubb med en fördröjning på 200 ms.</span><span class="sxs-lookup"><span data-stu-id="14a3a-131">This method continuously sends events to your event hub with a 200-ms delay.</span></span>
7. <span data-ttu-id="14a3a-132">Slutligen lägger du till följande rader till **Main**-metoden:</span><span class="sxs-lookup"><span data-stu-id="14a3a-132">Finally, add the following lines to the **Main** method:</span></span>
   
  ```csharp
  Console.WriteLine("Press Ctrl-C to stop the sender process");
  Console.WriteLine("Press Enter to start now");
  Console.ReadLine();
  SendingRandomMessages();
  ```
8. <span data-ttu-id="14a3a-133">Kör programmet och kontrollera att det inte finns några fel.</span><span class="sxs-lookup"><span data-stu-id="14a3a-133">Run the program, and ensure that there are no errors.</span></span>
  
<span data-ttu-id="14a3a-134">Grattis!</span><span class="sxs-lookup"><span data-stu-id="14a3a-134">Congratulations!</span></span> <span data-ttu-id="14a3a-135">Du har nu skickat meddelanden till en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="14a3a-135">You have now sent messages to an event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="14a3a-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="14a3a-136">Next steps</span></span>
<span data-ttu-id="14a3a-137">Nu när du har skapat ett fungerande program som skapar en händelsehubb och skickar data kan du gå vidare till följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="14a3a-137">Now that you've built a working application that creates an event hub and sends data, you can move on to the following scenarios:</span></span>

* [<span data-ttu-id="14a3a-138">Ta emot händelser med hjälp av värd för händelsebearbetning</span><span class="sxs-lookup"><span data-stu-id="14a3a-138">Receive events using the Event Processor Host</span></span>](event-hubs-dotnet-framework-getstarted-receive-eph.md)
* [<span data-ttu-id="14a3a-139">Referens för händelsebearbetningsvärd</span><span class="sxs-lookup"><span data-stu-id="14a3a-139">Event Processor Host reference</span></span>](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)
* [<span data-ttu-id="14a3a-140">Event Hubs-översikt</span><span class="sxs-lookup"><span data-stu-id="14a3a-140">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

