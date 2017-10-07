---
title: "aaaRelease anteckningar för Application Insights | Microsoft Docs"
description: "Lägg till distribution eller skapa markörer tooyour metrics explorer-diagram i Application Insights."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 23173e33-d4f2-4528-a730-913a8fd5f02e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: bwren
ms.openlocfilehash: e802d22701cb69e96fd1a6b469ea67454195f77a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="annotations-on-metric-charts-in-application-insights"></a>Anteckningar i mått diagram i Application Insights
Anteckningar på [Metrics Explorer](app-insights-metrics-explorer.md) diagram visas där du har distribuerat en ny version eller betydande händelse. De gör det enkelt toosee om ändringarna hade någon inverkan på programmets prestanda. De kan skapas automatiskt av hello [Visual Studio Team Services bygg system](https://www.visualstudio.com/en-us/get-started/build/build-your-app-vs). Du kan också skapa anteckningar tooflag alla händelser som du vill genom att [skapa dem från PowerShell](#create-annotations-from-powershell).

![Exempel på anteckningar med synliga korrelation med serversvarstid](./media/app-insights-annotations/00.png)



## <a name="release-annotations-with-vsts-build"></a>Släpp anteckningar med VSTS build

Versionen anteckningar är en funktion i hello molnbaserade version och utgåva tjänst av Visual Studio Team Services. 

### <a name="install-hello-annotations-extension-one-time"></a>Installera hello anteckningar tillägg (en gång)
toobe kan toocreate versionen anteckningar, behöver du tooinstall en av hello hello många Team Service tillägg som är tillgängliga i Visual Studio Marketplace.

1. Logga in tooyour [Visual Studio Team Services](https://www.visualstudio.com/en-us/get-started/setup/sign-up-for-visual-studio-online) projekt.
2. I Visual Studio Marketplace [ta hello versionen anteckningar tillägget](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations), och Lägg till den tooyour Team Services-konto.

![AT övre högra av Team Services webbsida, öppna Marketplace. Välj Visual Team Services och under version och utgåva, Välj finns mer.](./media/app-insights-annotations/10.png)

Du behöver bara toodo detta en gång för Visual Studio Team Services-konto. Versionen anteckningar kan nu konfigureras för alla projekt i ditt konto. 

### <a name="configure-release-annotations"></a>Konfigurera versionen anteckningar

Du behöver tooget separat API-nyckel för varje VSTS versionsmall.

1. Logga in toohello [Microsoft Azure Portal](https://portal.azure.com) och öppna hello Application Insights-resurs som övervakar dina program. (Eller [skapa en nu](app-insights-overview.md), om du inte gjort det ännu.)
2. Öppna **API-åtkomst**, **Application Insights Id**.
   
    ![Öppna Application Insights-resurs i portal.azure.com, och välja inställningar. Öppna API-åtkomst. Kopiera hello program-ID](./media/app-insights-annotations/20.png)

4. Öppna (eller skapa) på hello versionsmall som hanterar din distribution från Visual Studio Team Services i ett nytt webbläsarfönster. 
   
    Lägga till en aktivitet och markera hello Application Insights versionen anteckningen aktiviteten hello-menyn.
   
    Klistra in hello **program-Id** som du kopierade från hello bladet för API-åtkomst.
   
    ![Öppna versionen i Visual Studio Team Services, Välj en definition av versionen och väljer Redigera. Klicka på Lägg till aktivitet och välj Application Insights versionen anteckningen. Klistra in hello Application Insights-Id.](./media/app-insights-annotations/30.png)
4. Ange hello **APIKey** tooa variabeln `$(ApiKey)`.

5. Skapa en ny API-nyckel i hello Azure fönstret, och ta en kopia av den.
   
    ![Klicka på Skapa API-nyckel i hello bladet API-åtkomst i hello Azure fönster. Ange en kommentar, skriva anteckningar och klickar på Generera nyckel. Kopiera hello ny nyckel.](./media/app-insights-annotations/40.png)

6. Öppna fliken för hello konfiguration av hello versionsmall.
   
    Skapa en variabel definition för `ApiKey`.
   
    Klistra in din nyckel API-toohello ApiKey variabeldefinitionen.
   
    ![Välj fliken för konfiguration av hello hello Team Services fönstret och klicka på Lägg till variabel. Ange hello namnet tooApiKey och klistra in hello-nyckel som du just genererade i hello värdet, och på hello låsikon.](./media/app-insights-annotations/50.png)
7. Slutligen **spara** hello versionen definition.


## <a name="view-annotations"></a>Visa anteckningar
Nu när du använder hello versionen mallen toodeploy en ny version skickas anteckningen tooApplication insikter. hello anteckningar visas i diagram i Metrics Explorer.

Klicka på någon anteckning markör tooopen information om hello versionen, inklusive begärande, källa kontrollen gren, versionen definition, miljö med mera.

![Klicka på valfri utgåva anteckningen markör.](./media/app-insights-annotations/60.png)

## <a name="create-custom-annotations-from-powershell"></a>Skapa anpassade anteckningar från PowerShell
Du kan också skapa anteckningar från någon annan process som du vill (utan att använda VS Team System). 


1. Skapa en lokal kopia av hello [Powershell-skript från GitHub](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).

2. Hämta hello program-ID och skapa en API-nyckel från hello bladet för API-åtkomst.

3. Anropa hello skript så här:

```PS

     .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }
```

Det är enkelt toomodify hello skript, till exempel toocreate anteckningar för hello tidigare.

## <a name="next-steps"></a>Nästa steg

* [Skapa arbetsobjekt](app-insights-diagnostic-search.md#create-work-item)
* [Automatisering med PowerShell](app-insights-powershell.md)
