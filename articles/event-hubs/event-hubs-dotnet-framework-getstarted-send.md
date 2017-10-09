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
# <a name="send-events-tooazure-event-hubs-using-hello-net-framework"></a>Skicka händelser tooAzure Händelsehubbar med hello .NET Framework

## <a name="introduction"></a>Introduktion

Händelsehubbar är en tjänst som bearbetar stora mängder händelsedata (telemetri) från anslutna enheter och program. När du samlar in data i Händelsehubbar kan du lagra hello data med ett lagringskluster eller omvandla dem med hjälp av en leverantör av realtidsanalys. Den här storskaliga händelse och bearbetningsfunktionen är en viktig komponent inom moderna programarkitekturer inklusive hello Sakernas Internet (IoT).

Den här kursen visar hur toouse hello [Azure-portalen](https://portal.azure.com) toocreate en händelsehubb. Den visar även hur hello toosend händelser tooan händelsehubb med hjälp av ett konsolprogram som skrivits i C# med hjälp av .NET Framework. tooreceive händelser med hjälp av hello .NET Framework finns hello [ta emot händelser med hjälp av hello .NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md) artikel, eller klicka på hello mottagande språket i hello vänstra innehållsförteckning.

toocomplete den här kursen behöver du hello följande krav:

* [Microsoft Visual Studio 2015 eller senare](http://visualstudio.com). hello skärmdumpar i den här självstudiekursen använder Visual Studio 2017.
* Ett aktivt Azure-konto. Om du inte har något konto kan du skapa ett utan kostnad på ett par minuter. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/free/).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Skapa ett namnområde för Event Hubs och en händelsehubb

hello första steget är toouse hello [Azure-portalen](https://portal.azure.com) toocreate en namnrymd Skriv Händelsehubbar och hämta hello management-autentiseringsuppgifter som programmet behöver toocommunicate med hello händelsehubb. toocreate ett namnområde och händelsehubb, följer du proceduren hello i [i den här artikeln](event-hubs-create.md), fortsätt sedan med hello följa stegen i den här självstudiekursen.

## <a name="create-a-sender-console-application"></a>Skapa ett avsändarkonsolprogram

I det här avsnittet skriver du en Windows-konsolapp som skickar händelser tooyour händelsehubb.

1. I Visual Studio skapar du ett nytt Visual C#-Skrivbordsapprojekt-projekt med hello **konsolprogram** projektmall. Namnet hello projektet **avsändaren**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp1.png)
2. I Solution Explorer högerklickar du på hello **avsändaren** projektet och klicka sedan på **hantera NuGet-paket för lösningen**. 
3. Klicka på hello **Bläddra** fliken, och sök sedan efter `Microsoft Azure Service Bus`. Klicka på **installera**, och Godkänn hello villkor för användning. 
   
    ![](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp2.png)
   
    Visual Studio hämtar, installerar och lägger till en referens toohello [Azure Service Bus-bibliotekets NuGet-paket](https://www.nuget.org/packages/WindowsAzure.ServiceBus).
4. Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:
   
  ```csharp
  using System.Threading;
  using Microsoft.ServiceBus.Messaging;
  ```
5. Lägg till följande fält toohello hello **programmet** klass, ersätter hello platshållarvärdena med namnet hello hello event hub du skapat i föregående avsnitt i hello och hello namnområdesnivå anslutningssträngen du sparat tidigare.
   
  ```csharp
  static string eventHubName = "{Event Hub name}";
  static string connectionString = "{send connection string}";
  ```
6. Lägg till följande metod toohello hello **programmet** klass:
   
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
   
  Den här metoden skickar kontinuerligt händelser tooyour event hub med en fördröjning på 200 ms.
7. Slutligen lägger du till följande rader toohello hello **Main** metoden:
   
  ```csharp
  Console.WriteLine("Press Ctrl-C toostop hello sender process");
  Console.WriteLine("Press Enter toostart now");
  Console.ReadLine();
  SendingRandomMessages();
  ```
8. Kör programmet hello och kontrollera att det inte finns några fel.
  
Grattis! Du har nu skickar meddelanden tooan händelsehubb.

## <a name="next-steps"></a>Nästa steg
Nu när du har skapat ett fungerande program som skapar en händelsehubb och skickar data, kan du gå vidare toohello följande scenarier:

* [Ta emot händelser med hjälp av hello värd för händelsebearbetning](event-hubs-dotnet-framework-getstarted-receive-eph.md)
* [Referens för händelsebearbetningsvärd](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)
* [Event Hubs-översikt](event-hubs-what-is-event-hubs.md)

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

