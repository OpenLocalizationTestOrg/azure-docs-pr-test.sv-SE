---
title: aaaDeploy WebJobs med Visual Studio
description: "Lär dig hur toodeploy Azure WebJobs tooAzure App Service Web Apps med Visual Studio."
services: app-service
documentationcenter: 
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: a3a9d320-1201-4ac8-9398-b4c9535ba755
ms.service: app-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2016
ms.author: glenga
ms.openlocfilehash: 5fc5d9562e8836348f5ab6844fb6c23ff40a321c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-webjobs-using-visual-studio"></a>Distribuera WebJobs med hjälp av Visual Studio
## <a name="overview"></a>Översikt
Det här avsnittet beskrivs hur toouse Visual Studio toodeploy ett konsolprogram projektet tooa webbapp i [Apptjänst](http://go.microsoft.com/fwlink/?LinkId=529714) som en [Azure Webjobs](http://go.microsoft.com/fwlink/?LinkId=390226). Information om hur toodeploy WebJobs med hjälp av hello [Azure Portal](https://portal.azure.com), se [kör bakgrundsaktiviteter med WebJobs](web-sites-create-web-jobs.md).

När Visual Studio distribuerar ett WebJobs-aktiverade konsolprogram projekt, utför två aktiviteter:

* Kopior runtime filer toohello lämplig mapp i hello webbapp (*App_Data/jobb/kontinuerlig* för kontinuerliga Webbjobb *App_Data/jobb/utlöst* för WebJobs schemalagda och på begäran).
* Ställer in [Azure schemaläggare](#scheduler) för WebJobs som är schemalagda toorun vid en viss tidpunkt. (Detta behövs inte för kontinuerliga Webbjobb.)

Ett WebJobs-aktiverade projekt har hello efter objekt som lagts till tooit:

* Hej [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet-paketet.
* En [webbjobb publicera settings.json](#publishsettings) -fil som innehåller inställningar för distribution och Schemaläggaren. 

![Diagram över vad läggs tooa konsolen tooenable appdistribution som ett Webbjobb](./media/websites-dotnet-deploy-webjobs/convert.png)

Du kan lägga till dessa objekt tooan befintliga konsolprogram projekt eller använda en mall toocreate ett nytt projekt WebJobs-aktiverade program. 

Du kan distribuera ett projekt som ett Webbjobb ensamt eller länka det tooa webbprojekt så att den distribuerar automatiskt när du distribuerar hello webbprojekt. toolink projekt, Visual Studio innehåller hello namnet på hello WebJobs-aktiverade projekt i en [webjobs list.json](#webjobslist) filen i hello webbprojekt.

![Diagram över Webbjobb projekt länka tooweb projekt](./media/websites-dotnet-deploy-webjobs/link.png)

## <a name="prerequisites"></a>Krav
Funktioner för distribution av WebJobs är tillgängliga i Visual Studio när du installerar hello Azure SDK för .NET:

* [Azure SDK för .NET (Visual Studio)](https://azure.microsoft.com/downloads/).

## <a id="convert"></a>Aktivera WebJobs-distribution för ett befintligt projekt konsolprogram
Du kan välja mellan två alternativ:

* [Aktivera automatisk distribution med ett webbprojekt](#convertlink).
  
    Konfigurera ett befintligt projekt konsolprogram så att den distribuerar automatiskt som ett Webbjobb när du distribuerar ett webbprojekt. Använd det här alternativet när du vill toorun din Webbjobb i hello samma webbprogram som du kör hello relaterade webbprogram.
* [Aktivera distributionen utan ett webbprojekt](#convertnolink).
  
    Konfigurera en befintlig konsolprogram projektet toodeploy som ett Webbjobb, med ingen länk tooa webbprojekt. Använd det här alternativet när du vill toorun ett Webbjobb i en webbapp, med Inga webbprogram som körs i hello webbapp. Du kanske vill toodo detta beställa toobe kan tooscale resurserna Webbjobb oberoende av webbtillämpningsresurser.

### <a id="convertlink"></a>Aktivera automatisk WebJobs-distribution med ett webbprojekt
1. Högerklicka på hello webbprojekt i **Solution Explorer**, och klicka sedan på **Lägg till** > **befintligt projekt som Azure Webjobs**.
   
    ![Befintligt projekt som Azure Webbjobbet](./media/websites-dotnet-deploy-webjobs/eawj.png)
   
    Hej [lägga till Azure Webjobs](#configure) dialogrutan visas.
2. I hello **projektnamn** listrutan, Välj hello konsolprogram projektet tooadd som ett Webbjobb.
   
    ![Att markera projekt i dialogrutan Lägg till Azure Webbjobbet](./media/websites-dotnet-deploy-webjobs/aaw1.png)
3. Fullständig hello [lägga till Azure Webjobs](#configure) dialogrutan och klicka sedan på **OK**. 

### <a id="convertnolink"></a>Aktivera WebJobs distribution utan ett webbprojekt
1. Högerklicka på hello konsolprogram projekt i **Solution Explorer**, och klicka sedan på **Publicera som Azure Webjobs...** . 
   
    ![Publicera som Azure Webbjobbet](./media/websites-dotnet-deploy-webjobs/paw.png)
   
    Hej [lägga till Azure Webjobs](#configure) dialogruta visas med hello-projektet som väljs i hello **projektnamn** rutan.
2. Fullständig hello [lägga till Azure Webjobs](#configure) dialogrutan och klicka sedan på **OK**.
   
   Hej **Publicera webbplats** guiden visas.  Stäng hello guiden om du inte vill toopublish omedelbart. hello-inställningar som du har angett sparas för när du vill för[distribuera hello projekt](#deploy).

## <a id="create"></a>Skapa ett nytt projekt WebJobs-aktiverad
toocreate ett nytt WebJobs-aktiverade projekt, du kan använda hello konsolen projektet mall och aktivera WebJobs programdistribution enligt beskrivningen i [hello föregående avsnitt](#convert). Alternativt kan använda du hello WebJobs nytt projekt mallen:

* [Använd hello WebJobs nytt projekt mall för en oberoende Webbjobbet](#createnolink)
  
    Skapa ett projekt och konfigurera den toodeploy ensamt som ett Webbjobb med ingen länk tooa webbprojekt. Använd det här alternativet när du vill toorun ett Webbjobb i en webbapp, med Inga webbprogram som körs i hello webbapp. Du kanske vill toodo detta beställa toobe kan tooscale resurserna Webbjobb oberoende av webbtillämpningsresurser.
* [Använd hello WebJobs nytt projekt mall för ett Webbjobb länkade tooa webbprojekt](#createlink)
  
    Skapa ett projekt som är konfigurerade toodeploy automatiskt som ett Webbjobb när ett webbprojekt i hello samma lösningen har distribuerats. Använd det här alternativet när du vill toorun din Webbjobb i hello samma webbprogram som du kör hello relaterade webbprogram.

> [!NOTE]
> Hej WebJobs nytt projekt mallen som automatiskt installerar NuGet-paket och inkluderar den kod i *Program.cs* för hello [WebJobs SDK](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs). Om du inte vill toouse hello WebJobs SDK, ta bort eller ändra hello `host.RunAndBlock` instruktionen i *Program.cs*.
> 
> 

### <a id="createnolink"></a>Använd hello WebJobs nytt projekt mall för en oberoende Webbjobbet
1. Klicka på **filen** > **nytt projekt**, och klicka sedan på hello **nytt projekt** klickar du på **moln**  >   **Azure Webjobs (.NET Framework)**.
   
    ![Dialogrutan Nytt projekt som visar Webbjobb mall](./media/websites-dotnet-deploy-webjobs/np.png)
2. Följ hello anvisningar som visades tidigare för[Se hello konsolprogram projektet ett oberoende WebJobs-projekt](#convertnolink).

### <a id="createlink"></a>Använd hello WebJobs nytt projekt mall för ett Webbjobb länkade tooa webbprojekt
1. Högerklicka på hello webbprojekt i **Solution Explorer**, och klicka sedan på **Lägg till** > **nytt projekt för Azure Webjobs**.
   
    ![Nya menypost för Azure Webjobs-projekt](./media/websites-dotnet-deploy-webjobs/nawj.png)
   
    Hej [lägga till Azure Webjobs](#configure) dialogrutan visas.
2. Fullständig hello [lägga till Azure Webjobs](#configure) dialogrutan och klicka sedan på **OK**.

## <a id="configure"></a>hello lägga till Azure Webjobs dialogrutan
Hej **lägga till Azure Webjobs** dialogrutan låter dig ange hello webbjobbsnamnet och kör inställning för din Webbjobb. 

![Azure Webjobs dialogrutan Lägg till](./media/websites-dotnet-deploy-webjobs/aaw2.png)

hello fält i den här dialogrutan motsvarar toofields på hello **nytt jobb** dialogrutan av hello Azure-portalen. Mer information finns i [kör bakgrundsaktiviteter med WebJobs](web-sites-create-web-jobs.md).

> [!NOTE]
> * Information om kommandoradsverktyget distribution finns [aktiverar kommandoraden eller kontinuerlig leverans Azure WebJobs](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/).
> * Om du distribuerar ett Webbjobb och sedan bestämmer du vill toochange hello typ av Webbjobb och distribuera om, behöver du toodelete hello webjobs publicera settings.json fil. Detta gör Visual Studio som visar hello publiceringsalternativ igen, så att du kan ändra hello typ av Webbjobb.
> * Om du distribuerar ett Webbjobb och senare ändrar hello körningsläge från kontinuerlig toonon kontinuerlig eller tvärtom, Visual Studio skapar ett nytt Webbjobb i Azure när du distribuerar. Om du ändrar andra schemainställningarna men lämna kör läge hello samma eller växla mellan schemalagda och på begäran, Visual Studio uppdateringar hello befintligt jobb i stället skapa en ny.
> 
> 

## <a id="publishsettings"></a>webbjobbet publicera settings.json
När du konfigurerar ett konsolprogram för distribution av WebJobs installerar Visual Studio hello [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet paket- och lagrar schemainformation i en *webbjobb publicera settings.json*  fil i hello projekt *egenskaper* för hello WebJobs-projekt. Här är ett exempel på filen:

        {
          "$schema": "http://schemastore.org/schemas/json/webjob-publish-settings.json",
          "webJobName": "WebJob1",
          "startTime": "null",
          "endTime": "null",
          "jobRecurrenceFrequency": "null",
          "interval": null,
          "runMode": "Continuous"
        }

Du kan redigera den här filen direkt och Visual Studio har IntelliSense. hello filen schemat lagras på [http://schemastore.org](http://schemastore.org/schemas/json/webjob-publish-settings.json) och kan visa.  

## <a id="webjobslist"></a>webjobs list.json
När du länkar ett WebJobs-aktiverade projektet tooa webbprojekt Visual Studio lagrar hello namnet på hello WebJobs-projekt i en *webjobs list.json* filen i hello-webbprojekt *egenskaper* mapp. hello listan kan innehålla flera WebJobs projekt som visas i följande exempel hello:

        {
          "$schema": "http://schemastore.org/schemas/json/webjobs-list.json",
          "WebJobs": [
            {
              "filePath": "../ConsoleApplication1/ConsoleApplication1.csproj"
            },
            {
              "filePath": "../WebJob1/WebJob1.csproj"
            }
          ]
        }

Du kan redigera den här filen direkt och Visual Studio har IntelliSense. hello filen schemat lagras på [http://schemastore.org](http://schemastore.org/schemas/json/webjobs-list.json) och kan visa.

## <a id="deploy"></a>Distribuera ett WebJobs-projekt
Ett WebJobs-projekt som du har länkat tooa webbprojekt distribuerar automatiskt med hello webbprojekt. Information om web project distribution finns [hur toodeploy tooWeb appar](web-sites-deploy.md).

toodeploy WebJobs-projekt, högerklicka på hello-projekt i **Solution Explorer** och på **Publicera som Azure Webjobs...** . 

![Publicera som Azure Webbjobbet](./media/websites-dotnet-deploy-webjobs/paw.png)

För en oberoende Webbjobb hello samma **Publicera webbplats** guiden som används för webbprojekt visas, men med färre inställningar tillgängliga toochange.

## <a id="nextsteps"></a>Nästa steg
Den här artikeln har förklaras hur toodeploy WebJobs med hjälp av Visual Studio. Mer information om hur toodeploy Azure WebJobs Se [Azure WebJobs - rekommenderade resurser - distribution](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/azure-webjobs-recommended-resources#deploying).

