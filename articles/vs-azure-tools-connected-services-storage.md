---
title: "aaaAdd Azure Storage med hjälp av anslutna Services i Visual Studio | Microsoft Docs"
description: "Lägg till Azure Storage tooyour app med hjälp av dialogrutan för hello Visual Studio Lägg till anslutna tjänster"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 521ec044-ad4b-4828-8864-01decde2e758
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2017
ms.author: kraigb
ms.openlocfilehash: 56b42063d86510b330e405108e28d50e6ba4da05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="adding-azure-storage-by-using-visual-studio-connected-services"></a>Lägga till Azure storage med hjälp av Visual Studio anslutna Services
Med Visual Studio kan du ansluta någon av följande tooAzure lagring med hjälp av hello hello **Lägg till anslutna tjänster** dialogrutan:

- C#-Molntjänsten
- .NET-serverdel Mobiltjänst
- ASP.NET-webbplats eller tjänst
- ASP.NET Core service
- Azure Webjobs-tjänst 

hello anslutna ytterligare funktioner läggs alla hello behövs referenser och anslutningen Kodprojekt tooyour och ändrar konfigurationsfilerna på lämpligt sätt. 

När du har slutfört hello **Lägg till anslutna tjänster** dialogrutan visar dokumentationen om hello steg krävs toostart arbetar med blob storage, köer och tabeller automatiskt.

## <a name="connect-tooazure-storage-using-hello-connected-services-dialog"></a>Ansluta tooAzure lagring med hjälp av hello anslutna tjänster dialogrutan
1. Öppna projektet i Visual Studio

1. I **Solution Explorer**, högerklicka på hello **anslutna tjänster** nod, och hello snabbmenyn och välj **Lägg till ansluten tjänst**.
   
    ![Lägg till Azure ansluten tjänst](./media/vs-azure-tools-connected-services-storage/IC796702.png)

1. I hello **anslutna tjänster** väljer **lagringsutrymmet i molnet med Azure Storage**.
   
    ![Lägg till Azure Storage](./media/vs-azure-tools-connected-services-storage/add-azure-storage.png)

1. I hello **Azure Storage** dialogrutan, Välj ett befintligt lagringskonto och välj **Lägg till**.
   
    Om du behöver toocreate ett lagringskonto kan du gå toohello nästa steg. Annars fortsätter toostep 6.
    
    ![Lägg till befintlig lagring konto tooproject](./media/vs-azure-tools-connected-services-storage/select-azure-storage-account.png)

1. toocreate ett lagringskonto: 
   
   1. Välj **skapa ett nytt Lagringskonto** längst hello hello dialogrutan.

   1. Fyll i hello **skapa Lagringskonto** dialogrutan och välj **skapa**.
      
       ![Nya Azure storage-konto](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)
      
   1. När hello **Azure Storage** dialogrutan visas, visas hello nytt lagringskonto i hello-listan. Välj hello nytt lagringskonto hello listan och välj **Lägg till**.

1. Hej lagring ansluten visas under hello **referenser** nod i projektet.
   
## <a name="how-your-project-is-modified"></a>Hur projektet har ändrats
När du är klar hello dialogrutan lägger till referenser i Visual Studio och ändrar vissa konfigurationsfiler. vissa ändringar för hello beror på hello projekttypen: 

- ASP.NET-projekt - [vad hände – ASP.NET-projekt](http://go.microsoft.com/fwlink/p/?LinkId=513126)
- ASP.NET Core projekt - [vad hände – ASP.NET 5 projekt](http://go.microsoft.com/fwlink/p/?LinkId=513124) 
- Molntjänstprojekt (webb- och arbetsroller) - [vad hände – Cloud Service-projekt](http://go.microsoft.com/fwlink/p/?LinkId=516965)
- Webbjobbet projekt - [vad hände - Webbjobb projekt](visual-studio/vs-storage-webjobs-what-happened.md)

## <a name="next-steps"></a>Nästa steg
- [MSDN-Forum: Azure Storage](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)
- [Microsoft Azure Storage-teamets blogg](http://blogs.msdn.com/b/windowsazurestorage/)
- [Azure Storage-dokumentation](https://docs.microsoft.com/azure/storage/)
