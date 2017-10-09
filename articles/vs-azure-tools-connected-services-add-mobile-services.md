---
title: "aaaAdding Mobile Services med anslutna tjänster i Visual Studio | Microsoft Docs"
description: "Lägga till Mobile Services med hjälp av dialogrutan för hello Visual Studio Lägg till anslutna tjänster"
services: visual-studio-online
documentationcenter: na
author: mlhoop
manager: douge
editor: 
ms.assetid: 75c3cb93-88e1-476d-a416-f34caa3608e3
ms.service: visual-studio-online
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: mobile
ms.date: 12/16/2015
ms.author: mlearned
ms.openlocfilehash: c47b6fb63dc99fbc012e1c627c6c7e95249de7a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="adding-mobile-services-by-using-visual-studio-connected-services"></a>Lägga till Mobile Services med hjälp av Visual Studio anslutna Services
Med Visual Studio 2015, kan du ansluta tooAzure Mobile Services med hello **Lägg till ansluten tjänst** dialogrutan. Du kan ansluta från alla klientappar för C#, JavaScript-app eller plattformsoberoende Cordova-app. När du ansluter kan du skapa och komma åt data, skapa anpassade API: er och schemalagda jobb eller lägga till stöd för push-meddelanden.  hello anslutna tjänster åtgärden lägger till alla lämpliga referenser och anslutningen kod. Du kan också dra nytta av inbyggt stöd för autentisering med en mängd olika populära identitet system, t.ex Azure AD, Facebook, Twitter och Microsoft Accounts.

## <a name="supported-project-types"></a>Stöds projekttyper
> [!NOTE]
> I Visual Studio 2015 stöds att lägga till Azure Mobile Services tooa Windows Universal (Windows 10)-projekt med hjälp av dialogrutan för hello Lägg till anslutna tjänster inte. Du kan lägga till Azure Mobile Services genom att installera hello lämpliga paket med hjälp av hello NuGet Package Manager för projektet.
> 
> 

Du kan använda hello Connected Services dialogrutan tooconnect tooAzure Mobile Services i hello följande projekttyper.

* .NET Windows 8.1 Store och Phone universell App-projekt
* JavaScript Windows 8.1 Store och Phone universell App-projekt
* Projekt som skapats med Visual Studio Tools för Apache Cordova

## <a name="connect-tooazure-mobile-services-using-hello-add-connected-services-dialog"></a>Ansluta tooAzure Mobile Services hello Lägg till anslutna tjänster dialogrutan
1. Kontrollera att du har ett Azure-konto. Om du inte har ett Azure-konto kan du registrera dig för en [kostnadsfri utvärderingsversion](http://go.microsoft.com/fwlink/?LinkId=518146).
2. Öppna hello **Lägg till anslutna tjänster** dialogrutan.
   
   * För .NET-appar, öppna ditt projekt i Visual Studio, öppna hello snabbmenyn för hello **referenser** nod i Solution Explorer och välj sedan **Lägg till ansluten tjänst**
     
        ![Ansluta tooAzure Mobiltjänst](./media/vs-azure-tools-connected-services-add-mobile-services/IC797635.png)
   * Öppna projektet i Visual Studio, öppna hello snabbmenyn för hello projektnoden i Solution Explorer för Apache Cordova-app-projekt, och välj sedan **Lägg till ansluten tjänst**.
3. I hello **Lägg till ansluten tjänst** dialogrutan Välj **Azure Mobile Services**, och välj sedan hello **konfigurera** knappen. Du kanske ange toolog till Azure om du inte redan gjort det..
   
    ![Lägga till en Azure-mobiltjänsten](./media/vs-azure-tools-connected-services-add-mobile-services/IC797636.png)
4. I hello **Azure Mobile Services** dialogrutan Välj en befintlig mobila tjänst om du har en. Om du behöver toocreate en ny Azure-mobiltjänsten proceduren hello nedan toodo så. Annars fortsätter toohello nästa steg.
   
    toocreate ett nytt mobila tjänstkonto:
   
   1. Välj hello ** skapa tjänst ** länken längst ned hello hello dialogrutan.
       ![Lägg till ny anslutna Mobiltjänst](./media/vs-azure-tools-connected-services-add-mobile-services/IC797637.png)
   2. På hello **skapa Mobiltjänst** dialogrutan du kan välja en JavaScript-serverdel Mobiltjänst, eller en Mobiltjänst för .NET-serverdel från hello **Runtime** listrutan. 
      
       ![Skapa en Mobiltjänst](./media/vs-azure-tools-connected-services-add-mobile-services/IC797638.png)
      
       En JavaScript-serverdelstjänst är enkelt och kraftfullt. Om du skapar en Mobiltjänst för JavaScript-serverdel hello serversidan JavaScript-kod är lagrad i hello moln, men du kan redigera server-skript med hjälp av Server Explorer eller hello Azure-hanteringsportalen. 
      
       En .NET-serverdel Mobiltjänst ger hello full effekt och flexibilitet för webb-API och Entity Framework. Om du skapar en Mobiltjänst för .NET-serverdel skapas ett projekt och lagt till tooyour lösning. 
   3. Välj hello **Region** där du vill hello Mobiltjänst och sedan ange ett användarnamn och lösenord för hello-servern.
   4. När du har angett alla hello krävs information, Välj hello **skapa** knappen toocreate hello mobiltjänst.
   5. hello nya mobiltjänsten ska visas i hello Tjänstlista på hello **Azure Mobile Services** dialogrutan. Välj hello nya mobiltjänsten i hello lista och sedan hello **Lägg till** knappen tooadd hello service tooyour-projekt.
5. Granska igång hello sida som visas och ta reda på hur projektet har ändrats. En komma igång-sida visas i webbläsaren när du lägger till en ansluten tjänst. Du kan granska hello föreslagna nästa steg och kodexempel eller växla toohello vad hände sidan toosee vilka referenser har lagts till tooyour projekt och hur din kod och configuration-filer har ändrats.
6. Starta med hello kodexempel som vägledning och skriva kod tooaccess mobiltjänsten!

## <a name="how-your-project-is-modified"></a>Hur projektet har ändrats
Hur Visual Studio ändrar projektet beror på hello-projekt. C# klientprogram, se [vad hände – C#-projekt](http://go.microsoft.com/fwlink/p/?LinkId=513119). JavaScript-klientprogram, se [vad hände – JavaScript-projekt](http://go.microsoft.com/fwlink/p/?LinkId=513120). Cordova-appar finns [vad hände – Cordova-projekt](http://go.microsoft.com/fwlink/p/?LinkId=513116).

## <a name="next-steps"></a>Nästa steg
Ställ frågor och få hjälp: 

* [MSDN-Forum: Azure Mobile Services](https://social.msdn.microsoft.com/forums/azure/home?forum=azuremobile)
* [Azure Mobile Services på hello Microsoft Azure-teamets blogg](https://azure.microsoft.com/blog/topics/mobile/)
* [Azure Mobile Services på azure.microsoft.com](https://azure.microsoft.com/services/mobile-services/)
* [Azure Mobile Services-dokumentationen på azure.microsoft.com](https://azure.microsoft.com/documentation/services/mobile-services/)

