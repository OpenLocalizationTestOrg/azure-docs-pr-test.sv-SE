---
title: "Stream Analytics: Rotera logga in autentiseringsuppgifterna för in- och utdataenheter | Microsoft Docs"
description: "Lär dig hur tooupdate hello autentiseringsuppgifter för Stream Analytics-in- och utdataenheter."
keywords: "autentiseringsuppgifter för inloggning"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 42ae83e1-cd33-49bb-a455-a39a7c151ea4
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: ac2374c539012b66ab390656c5750024e02f6bdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="rotate-login-credentials-for-inputs-and-outputs-in-stream-analytics-jobs"></a>Rotera inloggningsuppgifterna för in- och utdataenheter i Stream Analytics-jobb
## <a name="abstract"></a>Abstrakt
Azure Stream Analytics idag tillåter inte att ersätta hello autentiseringsuppgifter på en in-/ utdata när hello jobbet körs.

Medan Azure Stream Analytics stöder återuppta ett jobb från senaste utdata, vill vi tooshare hello hela processen för att minimera hello fördröjning mellan hello stoppar och startar för hello jobb och rotera hello inloggningsuppgifter.

## <a name="part-1---prepare-hello-new-set-of-credentials"></a>Del 1 – förbereda hello ny uppsättning autentiseringsuppgifter:
Den här delen är tillämpliga toohello följande indata/utdata:

* Blob Storage
* Händelsehubbar
* SQL Database
* Table Storage

Andra indata/utdata, fortsätter du med del 2.

### <a name="blob-storagetable-storage"></a>BLOB storage/tabellagring
1. Gå toohello lagring-tillägget i hello Azure Management portal:  
   ![graphic1][graphic1]
2. Leta upp hello lagringsutrymme som används av ditt jobb och gå till den:  
   ![graphic2][graphic2]
3. Klicka på hello hantera åtkomstnycklar kommando:  
   ![graphic3][graphic3]
4. Mellan hello primära åtkomstnyckeln och hello sekundära åtkomstnyckeln **Välj hello en inte används av ditt jobb**.
5. Klicka på Generera:  
   ![graphic4][graphic4]
6. Kopiera hello nya nyckeln:  
   ![graphic5][graphic5]
7. Fortsätt tooPart 2.

### <a name="event-hubs"></a>Händelsehubbar
1. Gå toohello Service Bus-tillägget i hello Azure Management portal:  
   ![graphic6][graphic6]
2. Leta upp hello Service Bus-Namespace som används av ditt jobb och gå till den:  
   ![graphic7][graphic7]
3. Om jobbet använder en princip för delad åtkomst på hello Service Bus Namespace, hoppa toostep 6  
4. Gå toohello Händelsehubbar fliken:  
   ![graphic8][graphic8]
5. Leta upp hello Event Hub som används av ditt jobb och gå till den:  
   ![graphic9][graphic9]
6. Gå toohello konfigurera fliken:  
   ![graphic10][graphic10]
7. Leta upp hello delad åtkomstprincip som används av ditt jobb på hello principnamn listrutan:  
   ![graphic11][graphic11]
8. Mellan hello primärnyckel och hello sekundärnyckeln, **Välj hello en inte används av ditt jobb**.  
9. Klicka på Generera:  
   ![graphic12][graphic12]
10. Kopiera hello nya nyckeln:  
   ![graphic13][graphic13]
11. Fortsätt tooPart 2.  

### <a name="sql-database"></a>SQL Database
> [!NOTE]
> Obs: du behöver tooconnect toohello SQL Database-tjänsten. Vi tooshow hur toodo detta med hjälp av hello hanteringen på hello Azure-hanteringsportalen, men du toouse vissa klientsidan verktyg som till exempel SQL Server Management Studio samt.
>
> 

1. Gå toohello SQL-databaser tillägg på hello Azure Management portal:  
   ![graphic14][graphic14]
2. Leta upp hello SQL-databas som används av ditt jobb och **klickar du på hello server** länka hello samma rad:  
   ![graphic15][graphic15]
3. Klicka på hello hantera kommando:  
   ![graphic16][graphic16]
4. Typen databasen Master:  
   ![graphic17][graphic17]
5. Ange ditt användarnamn, lösenord och klicka på Logga in:  
   ![graphic18][graphic18]
6. Klicka på ny fråga:  
   ![graphic19][graphic19]
7. Typen i hello följande fråga ersätter < login_name > med ditt användarnamn och ersätta <enterStrongPasswordHere> med ditt nya lösenord:  
   `CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'`
8. Klicka på Kör:  
   ![graphic20][graphic20]
9. Gå tillbaka toostep 2 och nu på hello databasen:  
   ![graphic21][graphic21]
10. Klicka på hello hantera kommando:  
   ![graphic22][graphic22]
11. Ange ditt användarnamn, lösenord och klicka på Logga in:  
   ![graphic23][graphic23]
12. Klicka på ny fråga:  
   ![graphic24][graphic24]
13. Skriv i följande fråga ersätter < användarnamn > med ett namn som du vill använda tooidentify hello inloggningen i hello kontexten för den här databasen (du kan ange samma värde som du gav för < login_name >, till exempel hello) och ersätter < login_name > med nytt användarnamn:  
   `CREATE USER <user_name> FROM LOGIN <login_name>`
14. Klicka på Kör:  
   ![graphic25][graphic25]
15. Nu bör du ge din nya användare hello samma roller och privilegier som den ursprungliga användaren hade.
16. Fortsätt tooPart 2.

## <a name="part-2-stopping-hello-stream-analytics-job"></a>Del 2: Stoppar hello Stream Analytics-jobbet
1. Gå toohello Stream Analytics-tillägget i hello Azure Management portal:  
   ![graphic26][graphic26]
2. Leta upp ditt jobb och gå till den:  
   ![graphic27][graphic27]
3. Gå toohello indata fliken eller hello utdata baserat på om du rotera hello autentiseringsuppgifter på indata eller utdata.  
   ![graphic28][graphic28]
4. Klicka på kommandot för hello stoppa och bekräfta hello jobbet har stoppats:  
   ![graphic29][graphic29] vänta hello jobbet toostop.
5. Leta upp hello indata/utdata som du vill att toorotate autentiseringsuppgifter på och gå till den:  
   ![graphic30][graphic30]
6. Fortsätt tooPart 3.

## <a name="part-3-editing-hello-credentials-on-hello-stream-analytics-job"></a>Del 3: Redigera hello autentiseringsuppgifter på hello Stream Analytics-jobbet
### <a name="blob-storagetable-storage"></a>BLOB storage/tabellagring
1. Hitta hello Lagringskontonyckel fältet och klistra in den nyligen skapade nyckeln till den:  
   ![graphic31][graphic31]
2. Klicka på Spara hello-kommando och Bekräfta spara dina ändringar:  
   ![graphic32][graphic32]
3. En anslutningstest startar automatiskt när du sparar ändringarna, se till att har har passerat.
4. Fortsätt tooPart 4.

### <a name="event-hubs"></a>Händelsehubbar
1. Hitta hello Event Hub princip nyckelfältet och klistra in den nyligen skapade nyckeln i den:  
   ![graphic33][graphic33]
2. Klicka på Spara hello-kommando och Bekräfta spara dina ändringar:  
   ![graphic34][graphic34]
3. En anslutningstest startar automatiskt när du sparar ändringarna, se till att den har passerat.
4. Fortsätt tooPart 4.

### <a name="power-bi"></a>Power BI
1. Klicka på hello förnya tillstånd:  

   ![graphic35][graphic35]
2. Du får följande bekräftelse hello:  

   ![graphic36][graphic36]
3. Klicka på Spara hello-kommando och Bekräfta spara dina ändringar:  
   ![graphic37][graphic37]
4. En anslutningstest startar automatiskt när du sparar ändringarna, se till att den passerat har.
5. Fortsätt tooPart 4.

### <a name="sql-database"></a>SQL Database
1. Hitta hello fälten användarnamn och lösenord och klistra in den nyligen skapade uppsättningen autentiseringsuppgifter i dem:  
   ![graphic38][graphic38]
2. Klicka på Spara hello-kommando och Bekräfta spara dina ändringar:  
   ![graphic39][graphic39]
3. En anslutningstest startar automatiskt när du sparar ändringarna, se till att den har passerat.  
4. Fortsätt tooPart 4.

## <a name="part-4-starting-your-job-from-last-stopped-time"></a>Del 4: Från och med jobbet senast stoppad
1. Navigera bort från hello in-/ utdata:  
   ![graphic40][graphic40]
2. Klicka på hello Start-kommando:  
   ![graphic41][graphic41]
3. Välj hello senast stoppad och klicka på OK:  
   ![graphic42][graphic42]
4. Fortsätt tooPart 5.  

## <a name="part-5-removing-hello-old-set-of-credentials"></a>Del 5: Ta bort hello gamla uppsättning autentiseringsuppgifter
Den här delen är tillämpliga toohello följande indata/utdata:

* Blob Storage
* Händelsehubbar
* SQL Database
* Table Storage

### <a name="blob-storagetable-storage"></a>BLOB storage/tabellagring
Upprepa del 1 för hello åtkomstnyckel som tidigare användes av ditt jobb toorenew hello nu oanvända snabbtangent.

### <a name="event-hubs"></a>Händelsehubbar
Upprepa del 1 för hello nyckel som tidigare användes av ditt jobb toorenew hello nu oanvända nyckel.

### <a name="sql-database"></a>SQL Database
1. Gå tillbaka toohello frågefönstret från del 1 steg 7 och anger hello följande fråga, ersätta < previous_login_name > med hello användarnamn som tidigare användes av ditt jobb:  
   `DROP LOGIN <previous_login_name>`  
2. Klicka på Kör:  
   ![graphic43][graphic43]  

Du bör få hello efter bekräftelse: 

    Command(s) completed successfully.

## <a name="get-help"></a>Få hjälp
Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

[graphic1]: ./media/stream-analytics-login-credentials-inputs-outputs/1-stream-analytics-login-credentials-inputs-outputs.png
[graphic2]: ./media/stream-analytics-login-credentials-inputs-outputs/2-stream-analytics-login-credentials-inputs-outputs.png
[graphic3]: ./media/stream-analytics-login-credentials-inputs-outputs/3-stream-analytics-login-credentials-inputs-outputs.png
[graphic4]: ./media/stream-analytics-login-credentials-inputs-outputs/4-stream-analytics-login-credentials-inputs-outputs.png
[graphic5]: ./media/stream-analytics-login-credentials-inputs-outputs/5-stream-analytics-login-credentials-inputs-outputs.png
[graphic6]: ./media/stream-analytics-login-credentials-inputs-outputs/6-stream-analytics-login-credentials-inputs-outputs.png
[graphic7]: ./media/stream-analytics-login-credentials-inputs-outputs/7-stream-analytics-login-credentials-inputs-outputs.png
[graphic8]: ./media/stream-analytics-login-credentials-inputs-outputs/8-stream-analytics-login-credentials-inputs-outputs.png
[graphic9]: ./media/stream-analytics-login-credentials-inputs-outputs/9-stream-analytics-login-credentials-inputs-outputs.png
[graphic10]: ./media/stream-analytics-login-credentials-inputs-outputs/10-stream-analytics-login-credentials-inputs-outputs.png
[graphic11]: ./media/stream-analytics-login-credentials-inputs-outputs/11-stream-analytics-login-credentials-inputs-outputs.png
[graphic12]: ./media/stream-analytics-login-credentials-inputs-outputs/12-stream-analytics-login-credentials-inputs-outputs.png
[graphic13]: ./media/stream-analytics-login-credentials-inputs-outputs/13-stream-analytics-login-credentials-inputs-outputs.png
[graphic14]: ./media/stream-analytics-login-credentials-inputs-outputs/14-stream-analytics-login-credentials-inputs-outputs.png
[graphic15]: ./media/stream-analytics-login-credentials-inputs-outputs/15-stream-analytics-login-credentials-inputs-outputs.png
[graphic16]: ./media/stream-analytics-login-credentials-inputs-outputs/16-stream-analytics-login-credentials-inputs-outputs.png
[graphic17]: ./media/stream-analytics-login-credentials-inputs-outputs/17-stream-analytics-login-credentials-inputs-outputs.png
[graphic18]: ./media/stream-analytics-login-credentials-inputs-outputs/18-stream-analytics-login-credentials-inputs-outputs.png
[graphic19]: ./media/stream-analytics-login-credentials-inputs-outputs/19-stream-analytics-login-credentials-inputs-outputs.png
[graphic20]: ./media/stream-analytics-login-credentials-inputs-outputs/20-stream-analytics-login-credentials-inputs-outputs.png
[graphic21]: ./media/stream-analytics-login-credentials-inputs-outputs/21-stream-analytics-login-credentials-inputs-outputs.png
[graphic22]: ./media/stream-analytics-login-credentials-inputs-outputs/22-stream-analytics-login-credentials-inputs-outputs.png
[graphic23]: ./media/stream-analytics-login-credentials-inputs-outputs/23-stream-analytics-login-credentials-inputs-outputs.png
[graphic24]: ./media/stream-analytics-login-credentials-inputs-outputs/24-stream-analytics-login-credentials-inputs-outputs.png
[graphic25]: ./media/stream-analytics-login-credentials-inputs-outputs/25-stream-analytics-login-credentials-inputs-outputs.png
[graphic26]: ./media/stream-analytics-login-credentials-inputs-outputs/26-stream-analytics-login-credentials-inputs-outputs.png
[graphic27]: ./media/stream-analytics-login-credentials-inputs-outputs/27-stream-analytics-login-credentials-inputs-outputs.png
[graphic28]: ./media/stream-analytics-login-credentials-inputs-outputs/28-stream-analytics-login-credentials-inputs-outputs.png
[graphic29]: ./media/stream-analytics-login-credentials-inputs-outputs/29-stream-analytics-login-credentials-inputs-outputs.png
[graphic30]: ./media/stream-analytics-login-credentials-inputs-outputs/30-stream-analytics-login-credentials-inputs-outputs.png
[graphic31]: ./media/stream-analytics-login-credentials-inputs-outputs/31-stream-analytics-login-credentials-inputs-outputs.png
[graphic32]: ./media/stream-analytics-login-credentials-inputs-outputs/32-stream-analytics-login-credentials-inputs-outputs.png
[graphic33]: ./media/stream-analytics-login-credentials-inputs-outputs/33-stream-analytics-login-credentials-inputs-outputs.png
[graphic34]: ./media/stream-analytics-login-credentials-inputs-outputs/34-stream-analytics-login-credentials-inputs-outputs.png
[graphic35]: ./media/stream-analytics-login-credentials-inputs-outputs/35-stream-analytics-login-credentials-inputs-outputs.png
[graphic36]: ./media/stream-analytics-login-credentials-inputs-outputs/36-stream-analytics-login-credentials-inputs-outputs.png
[graphic37]: ./media/stream-analytics-login-credentials-inputs-outputs/37-stream-analytics-login-credentials-inputs-outputs.png
[graphic38]: ./media/stream-analytics-login-credentials-inputs-outputs/38-stream-analytics-login-credentials-inputs-outputs.png
[graphic39]: ./media/stream-analytics-login-credentials-inputs-outputs/39-stream-analytics-login-credentials-inputs-outputs.png
[graphic40]: ./media/stream-analytics-login-credentials-inputs-outputs/40-stream-analytics-login-credentials-inputs-outputs.png
[graphic41]: ./media/stream-analytics-login-credentials-inputs-outputs/41-stream-analytics-login-credentials-inputs-outputs.png
[graphic42]: ./media/stream-analytics-login-credentials-inputs-outputs/42-stream-analytics-login-credentials-inputs-outputs.png
[graphic43]: ./media/stream-analytics-login-credentials-inputs-outputs/43-stream-analytics-login-credentials-inputs-outputs.png

