---
title: Kontinuerlig leverans med Git och Visual Studio Team Services i Azure | Microsoft Docs
description: "Lär dig hur du konfigurerar din Visual Studio Team Services grupprojekt att använda Git för att automatiskt skapa och distribuera till funktionen Web App i Azure App Service eller cloud services."
services: cloud-services
documentationcenter: .net
author: mlearned
manager: douge
editor: 
ms.assetid: 4b3297ef-0de6-4d5f-925c-fcdacc3085ac
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/06/2016
ms.author: mlearned
ms.openlocfilehash: f4f5f231536bc381d17898ff2c592be821168a65
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="continuous-delivery-to-azure-using-visual-studio-team-services-and-git"></a>Kontinuerlig leverans till Azure med Visual Studio Team Services och Git
Du kan använda Visual Studio Team Services grupprojekt att vara värd för en Git-lagringsplats för din källkod och automatiskt skapa och distribuera till Azure-webbappar eller molntjänster när du skickar ett genomförande till lagringsplatsen.

Du behöver Visual Studio 2013 och Azure SDK om du installerade. Om du inte redan har Visual Studio 2013 kan du hämta det genom att välja den **Kom igång gratis** länka när [www.visualstudio.com](http://www.visualstudio.com). Installera Azure SDK från [här](http://go.microsoft.com/fwlink/?LinkId=239540).

> [!NOTE]
> Du behöver ett Visual Studio Team Services-konto för att slutföra den här självstudiekursen: du kan [öppnar ett Visual Studio Team Services-konto gratis](http://go.microsoft.com/fwlink/p/?LinkId=512979).
> 
> 

Följ dessa steg om du vill konfigurera en molntjänst för att automatiskt skapa och distribuera till Azure med hjälp av Visual Studio Team Services.

## <a name="1-create-a-git-repository"></a>1: skapa en Git-lagringsplats
1. Om du inte redan har en Visual Studio Team Services-konto kan du skaffa ett [här](http://go.microsoft.com/fwlink/?LinkId=397665). När du skapar ditt team projekt, Välj Git som ditt källkontrollsystem. Följ instruktionerna för att ansluta Visual Studio till ditt team projekt.
2. I **Team Explorer**, Välj den **klona lagringsplatsen** länk.
   
    ![][3]
3. Ange platsen för den lokala kopian och välj sedan den **klona** knappen.

## <a name="2-create-a-project-and-commit-it-to-the-repository"></a>2: skapa ett projekt och överför den till databasen
1. I **Team Explorer**i den **lösningar** väljer du den **ny** länken för att skapa ett nytt projekt i lokal databas.
   
    ![][4]
2. Du kan distribuera en webbapp eller en tjänst i molnet (Azure-program) genom att följa stegen i den här genomgången. Skapa ett nytt Azure Cloud Service-projekt, eller ett nytt ASP.NET MVC-projekt. Se till att projektet riktar sig till .NET Framework 4 eller senare. Om du skapar ett molntjänstprojekt kan du lägga till en ASP.NET MVC-webbroll och en arbetsroll.
   Om du vill skapa en webbapp, välja den **ASP.NET-webbprogram** projektmall och välj sedan **MVC**. Se [skapa en ASP.NET-webbapp i Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md) för mer information.
3. Öppna snabbmenyn för lösningen och välj **genomför**.
   
    ![][7]
4. Om du har använt Git i Visual Studio Team Services måste du ange information för att identifiera dig i Git. I den **väntande ändringar** område i **Team Explorer**, ange ditt användarnamn och e-postadress. Skriv en kommentar för genomförandet och välj sedan den **Commit** knappen.
   
    ![][8]
5. Observera de alternativ som ska tas med eller undanta specifika ändringar när du checkar in. Om ändringarna är undantagna väljer **inkluderar alla**.
6. Du har nu genomfört ändringarna i den lokala kopian av databasen. Sedan synkronisera ändringarna med servern genom att välja den **Sync** länk.

## <a name="3-connect-the-project-to-azure"></a>3: Anslut projektet till Azure
1. Nu när du har en Git-lagringsplats i Visual Studio Team Services med vissa källkod i den är du redo att ansluta git-lagringsplatsen till Azure.  I den [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885), Välj din cloud service eller web app eller skapa en ny genom att välja den + -ikonen längst ned vänster och välja **Molntjänsten** eller **Web App** och sedan **Snabbregistrering**.
   
    ![][9]
2. Molntjänster, Välj den **Konfigurera publicering med Visual Studio Team Services** länk. Webbappar, Välj den **konfigurera distribution från källkontroll** länk.
   
    ![][10]
3. I guiden anger du namnet på ditt konto i Visual Studio Team Services i textrutan och välj den **auktorisera nu** länk. Du kan uppmanas att logga in.
   
    ![][11]
4. I den **anslutningsbegäran** popup-fönstret, Välj **acceptera** att auktorisera Azure konfigurera ditt team projekt i Visual Studio Team Services.
   
    ![][12]
5. När tillståndet lyckas visas en listruta som innehåller din Visual Studio Team Services grupprojekt.  Välj namnet på grupprojekt som du skapade i föregående steg och välj guidens markering.
   
    ![][13]
   
    Nästa gång du skicka ett genomförande till lagringsplatsen, Visual Studio Team Services skapar och distribuera projektet till Azure.

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: utlösa en återskapning och omdistribuera ditt projekt
1. Öppna en fil och ändra den i Visual Studio. Till exempel ändra filen `_Layout.cshtml` under vyerna\\delad mapp i en MVC-webbroll.
   
    ![][17]
2. Redigera sidfotstexten för platsen och spara filen.
   
    ![][18]
3. I **Solution Explorer**, öppna snabbmenyn för noden lösning, projektnoden eller filen du ändras och välj sedan **genomför**.
4. Ange en kommentar och välj **genomför**.
   
    ![][20]
5. Välj den **Sync** länk.
   
    ![][38]
6. Välj den **Push** länk trycka på din genomförande till lagringsplatsen i Visual Studio Team Services. (Du kan också använda den **Sync** för att kopiera ditt incheckningar till databasen. Skillnaden är att **Sync** också tar emot de senaste ändringarna från databasen.)
   
    ![][39]
7. Välj den **hem** vill gå tillbaka till den **Team Explorer** startsidan.
   
    ![][21]
8. Välj **bygger** att visa versionerna pågår.
   
    ![][22]
   
    **Teamet Explorer** visar att en version har utlösts för din incheckning.
   
    ![][23]
9. Om du vill visa detaljerade loggfil när build fortlöper, dubbelklicka på namnet på build pågår.
10. Versionen är pågående, ta en titt på build-definition som skapades när du använde guiden för att länka till Azure.  Öppna snabbmenyn för build-definition och välj **redigera skapa Definition**.
    
    ![][25]
11. I den **utlösaren** fliken visas build-definition anges att bygga på varje incheckning, som standard. (För en molnbaserad tjänst bör Visual Studio Team Services skapar och distribuerar mastergrenen till mellanlagringsmiljön automatiskt. Du måste göra ett manuellt steg att distribuera liveplatsen. För en webbapp som inte har mellanlagring miljö distribuerar mastergrenen direkt till webbplatsen live.
    
    ![][26]
12. I den **processen** fliken visas distributionsmiljö har angetts till namnet på din cloud service eller web app.
    
     ![][27]
13. Ange värden för egenskaper om du vill att andra värden än standardinställningarna. Egenskaperna för Azure-publicering finns i den **distribution** avsnitt, och du kan också behöva ange MSBuild-parametrar. Till exempel i ett moln service-projekt att ange en tjänstkonfiguration än ”moln” ange MSbuild parametrarna för `/p:TargetProfile=[YourProfile]` där *[YourProfile]* matchar en tjänstkonfigurationsfil med namnet ServiceConfiguration. *YourProfile*.cscfg.
    
     I följande tabell visas de tillgängliga egenskaperna i den **distribution** avsnitt:
    
    | Egenskap | Standardvärde |
    | --- | --- |
    | Tillåt ej betrodda certifikat |Om värdet är false måste SSL-certifikat signeras av en rotcertifikatutfärdare. |
    | Tillåt uppgradering |Gör att distributionen ska uppdatera en befintlig distribution i stället för att skapa en ny. Bevarar den IP-adressen. |
    | Ta inte bort |Om värdet är true, Skriv inte över en befintlig orelaterade distribution (uppgraderingen tillåts). |
    | Sökvägen till distributionsinställningar |Sökvägen till filen .pubxml för en webbapp i förhållande till rotmappen på lagringsplatsen. Ignoreras för molntjänster. |
    | SharePoint-miljön för distribution |Samma som namnet på tjänsten. |
    | Av Azure-distributionsmiljö |Web app eller molnet tjänstnamnet. |
14. Vid den tidpunkten ska din build slutföras.
    
     ![][28]
15. Om du dubbelklickar på build-namnet, Visual Studio visas en **skapa sammanfattning**, inklusive alla resultat från associerade testprojekt.
    
     ![][29]
16. I den [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885), du kan visa den associerade distributionen på de **distributioner** fliken när mellanlagringsmiljön är markerad.
    
     ![][30]
17. Gå till webbplatsens URL. Välj bara för en webbapp i **Bläddra** knappen i portalen. För en tjänst i molnet, väljer du URL-Adressen i den **snabb i korthet** avsnitt i den **instrumentpanelen** sidan som visar mellanlagringsmiljön.
    
    Distributioner från kontinuerlig integration för molntjänster publiceras till mellanlagringsmiljön som standard. Du kan ändra detta genom att ange den **alternativa Molntjänstmiljö** egenskapen **produktion**. Här är där webbplatsens URL på den molntjänst instrumentpanelssida.
    
    ![][31]
    
    En ny webbläsarflik öppnas för att visa webbplatsen körs.
    
    ![][32]
18. Om du gör andra ändringar i ditt projekt bygger mer du utlösare och du samlar flera distributioner. Den senaste som har markerats som aktiv.
    
    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: Distribuera om en tidigare version
Det här steget är valfritt. Välj en tidigare distribution i den klassiska Azure-portalen och väljer **omdistribuera** till spola platsen till en tidigare incheckning. Observera att detta utlöser en ny version i TFS och skapa en ny post i distributionshistoriken.

![][34]

## <a name="6-change-the-production-deployment"></a>6: ändra produktionsdistributionen
När du är klar kan du befordra mellanlagringsmiljön till produktionsmiljön genom att välja **växla** i den klassiska Azure-portalen. Nyligen distribuerade mellanlagringsmiljön höjs till produktionen och föregående produktionsmiljön, eventuella blir en mellanlagringsmiljön. Aktiv distribution kan vara olika för produktions- och Mellanlagringsmiljöer, men distributionshistoriken för senare versioner är detsamma oavsett miljö.

![][35]

## <a name="7-deploy-from-a-working-branch"></a>7: Distribuera från en arbetsgren.
När du använder Git du vanligtvis göra ändringar i en arbetsgren och integrera i mastergrenen när du utvecklar når slutfört tillstånd. Under utvecklingsfasen av ett projekt ska du vill skapa och distribuera arbetsgren till Azure.

1. I **Team Explorer**, Välj den **Start** knappen och välj sedan den **filialer** knappen.
   
    ![][40]
2. Välj den **ny gren** länk.
   
    ![][41]
3. Ange namnet på grenen, till exempel ”arbetar” och välj **skapa gren**. Detta skapar en ny lokal gren.
   
    ![][42]
4. Publicera grenen. Välj filialen namn i **Opublicerat filialer**, och välj **publicera**.
   
    ![][44]
5. Som standard utlösa endast ändringar till mastergrenen en kontinuerlig version. Ställ in kontinuerlig build för en arbetsgren genom att välja den **bygger** sidan i **Team Explorer**, och välj **redigera skapa Definition**.
6. Öppna den **inställningar för datakälla** fliken. Under **övervakas grenar för kontinuerlig integrering och bygga**, Välj **Klicka här om du vill lägga till en ny rad**.
   
    ![][47]
7. Ange grenen som du skapat, till exempel refs, huvuden/fungerande.
   
    ![][48]
8. Gör en ändring i koden, öppna snabbmenyn för den ändrade filen och välj sedan **genomför**.
   
    ![][43]
9. Välj den **osynkroniserade incheckningar** länka och välj den **Sync** knappen eller **Push** länken för att kopiera ändringarna till kopian av arbetsgren i Visual Studio Team Services.
   
   ![][45]
10. Navigera till den **bygger** visa och Sök efter den version som precis har utlösts för arbetsgren.

## <a name="next-steps"></a>Nästa steg
Läs mer tips om att använda Git med Visual Studio Team Services i [utveckla och dela din kod i Git med Visual Studio](https://www.visualstudio.com/en-us/docs/git/share-your-code-in-git-vs-2017) och för information om hur du använder en Git-lagringsplats som inte hanteras av Visual Studio Team Services att publicera till Azure, se [kontinuerlig distribution till Azure App Service](../app-service-web/app-service-continuous-deployment.md). Mer information om Visual Studio Team Services finns [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateTeamProjectInGit.PNG
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png
[3]: ./media/cloud-services-continuous-delivery-use-vso-git/CloneThisRepository.PNG
[4]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateNewSolutionInClonedRepo.PNG
[7]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitMenuItem.PNG
[8]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[9]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateCloudService.PNG
[10]: ./media/cloud-services-continuous-delivery-use-vso-git/SetUpPublishingWithVSO.PNG
[11]: ./media/cloud-services-continuous-delivery-use-vso-git/AuthorizeConnection.PNG
[12]: ./media/cloud-services-continuous-delivery-use-vso-git/ConnectionRequest.PNG
[13]: ./media/cloud-services-continuous-delivery-use-vso-git/ChooseARepo3.PNG
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso-git/MakeACodeChange.PNG
[20]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[21]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerHome.png
[22]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerBuilds.PNG
[23]: ./media/cloud-services-continuous-delivery-use-vso-git/BuildInQueue.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso-git/ProcessTab.PNG
[28]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateANewAccount.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso-git/PushCurrentBranch.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso-git/BranchesInTeamExplorer.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso-git/NewBranch.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateBranch.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso-git/PublishBranch.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso-git/SourceSettingsPage.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso-git/IncludeWorkingBranch.PNG
