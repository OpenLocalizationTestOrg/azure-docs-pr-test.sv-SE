---
title: aaaContinuous leverans med Git och Visual Studio Team Services i Azure | Microsoft Docs
description: "Lär dig hur tooconfigure din Visual Studio Team Services team projekt toouse Git tooautomatically skapa och distribuera toohello webbprogram funktion i Azure App Service eller cloud services."
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
ms.openlocfilehash: 936c42194f45be55597a77f9a3a6deb4480ed94b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-delivery-tooazure-using-visual-studio-team-services-and-git"></a>Kontinuerlig leverans tooAzure med hjälp av Visual Studio Team Services och Git
Du kan använda Visual Studio Team Services team projekt toohost en Git-lagringsplats för din källkod och automatiskt skapa och distribuera tooAzure webbprogram eller molntjänster när du överför en commit toohello databas.

Du behöver Visual Studio 2013 och hello Azure SDK är installerat. Om du inte redan har Visual Studio 2013 kan du hämta det genom att välja hello **Kom igång gratis** länka när [www.visualstudio.com](http://www.visualstudio.com). Installera hello Azure SDK från [här](http://go.microsoft.com/fwlink/?LinkId=239540).

> [!NOTE]
> Du behöver ett Visual Studio Team Services-konto toocomplete självstudierna: du kan [öppnar ett Visual Studio Team Services-konto gratis](http://go.microsoft.com/fwlink/p/?LinkId=512979).
> 
> 

tooset upp en cloud service tooautomatically skapa och distribuera tooAzure med hjälp av Visual Studio Team Services, Följ dessa steg.

## <a name="1-create-a-git-repository"></a>1: skapa en Git-lagringsplats
1. Om du inte redan har en Visual Studio Team Services-konto kan du skaffa ett [här](http://go.microsoft.com/fwlink/?LinkId=397665). När du skapar ditt team projekt, Välj Git som ditt källkontrollsystem. Följ hello instruktioner tooconnect Visual Studio tooyour team projekt.
2. I **Team Explorer**, Välj hello **klona lagringsplatsen** länk.
   
    ![][3]
3. Anger hello plats hello lokal kopia och sedan välja hello **klona** knappen.

## <a name="2-create-a-project-and-commit-it-toohello-repository"></a>2: skapa ett projekt och genomför den toohello databasen
1. I **Team Explorer**, i hello **lösningar** väljer hello **ny** länka toocreate ett nytt projekt i hello lokal databas.
   
    ![][4]
2. Du kan distribuera en webbapp eller en tjänst i molnet (Azure-program) med följande hello steg i den här genomgången. Skapa ett nytt Azure Cloud Service-projekt, eller ett nytt ASP.NET MVC-projekt. Se till att hello projektet mål hello .NET Framework 4 eller senare. Om du skapar ett molntjänstprojekt kan du lägga till en ASP.NET MVC-webbroll och en arbetsroll.
   Om du vill toocreate ett webbprogram väljer hello **ASP.NET-webbprogram** projektmall och välj sedan **MVC**. Se [skapa en ASP.NET-webbapp i Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md) för mer information.
3. Öppna hello snabbmenyn för hello lösning och välj **genomför**.
   
    ![][7]
4. Om detta är hello första gången som du har använt Git i Visual Studio Team Services behöver du tooprovide vissa information tooidentify själv i Git. I hello **väntande ändringar** område i **Team Explorer**, ange ditt användarnamn och e-postadress. Ange en kommentar för hello genomförande och välj hello **Commit** knappen.
   
    ![][8]
5. Observera hello alternativ tooinclude med eller undanta specifika ändringar när du checkar in. Om hello ändringar du vill utesluts, väljer **inkluderar alla**.
6. Du har nu allokerade hello ändringar i den lokala kopian av hello-databasen. Sedan synkronisera ändringarna med hello server genom att välja hello **Sync** länk.

## <a name="3-connect-hello-project-tooazure"></a>3: Anslut hello projekt tooAzure
1. Nu när du har en Git-lagringsplats i Visual Studio Team Services med vissa källkod i den är du redo tooconnect tooAzure din git-lagringsplatsen.  I hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885), Välj din cloud service eller web app eller skapa en ny genom att välja hello + längst hello nedre vänstra hörnet och välja **Molntjänsten** eller **webbprogrammet**och sedan **Snabbregistrering**.
   
    ![][9]
2. Välj hello för molntjänster, **Konfigurera publicering med Visual Studio Team Services** länk. Välj hello för web apps **konfigurera distribution från källkontroll** länk.
   
    ![][10]
3. Hello i guiden anger hello namnet på ditt konto i Visual Studio Team Services hello textrutan med och välj hello **auktorisera nu** länk. Du kan uppmanas toosign i.
   
    ![][11]
4. I hello **anslutningsbegäran** popup-fönstret, Välj **acceptera** tooauthorize Azure tooconfigure ditt team projekt i Visual Studio Team Services.
   
    ![][12]
5. När tillståndet lyckas visas en listruta som innehåller din Visual Studio Team Services grupprojekt.  Välj hello grupprojekt som du skapade i föregående steg i hello och välj hello guiden markering.
   
    ![][13]
   
    hello nästa gång som du överför en commit tooyour databas, Visual Studio Team Services skapar och distribuerar ditt projekt tooAzure.

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: utlösa en återskapning och omdistribuera ditt projekt
1. Öppna en fil och ändra den i Visual Studio. Till exempel ändra hello filen `_Layout.cshtml` under hello vyer\\delad mapp i en MVC-webbroll.
   
    ![][17]
2. Redigera hello sidfotstexten för hello plats och spara hello-filen.
   
    ![][18]
3. I **Solution Explorer**, öppna hello snabbmenyn för hello lösning nod, projektnoden eller hello-filen som du har ändrats och välj sedan **genomför**.
4. Ange en kommentar och välj **genomför**.
   
    ![][20]
5. Välj hello **Sync** länk.
   
    ![][38]
6. Välj hello **Push** länka toopush commit toohello databasen i Visual Studio Team Services. (Du kan också använda hello **Sync** knappen toocopy incheckningar toohello databasen. hello skillnaden är att **Sync** också hämtar hello senaste ändringarna från databasen hello.)
   
    ![][39]
7. Välj hello **hem** knappen tooreturn toohello **Team Explorer** startsidan.
   
    ![][21]
8. Välj **bygger** tooview hello bygger pågår.
   
    ![][22]
   
    **Teamet Explorer** visar att en version har utlösts för din incheckning.
   
    ![][23]
9. tooview detaljerade loggfil som hello skapa fortlöper, dubbelklickar du på hello namnet på hello build pågår.
10. Hello build är pågående, ta en titt på hello build definition som skapades när du använde hello guiden toolink tooAzure.  Öppna hello snabbmenyn för hello build definition och välj **redigera skapa Definition**.
    
    ![][25]
11. I hello **utlösaren** fliken ser du att hello build definition anges toobuild på varje incheckning, som standard. (För en molnbaserad tjänst bör Visual Studio Team Services skapar och distribuerar hello mastergrenen toohello mellanlagring miljö automatiskt. Du har fortfarande toodo en manuell åtgärd toodeploy toohello drift. För en webbapp som inte har mellanlagring miljön distribueras hello mastergrenen direkt toohello live plats.
    
    ![][26]
12. I hello **processen** fliken visas hello distributionsmiljö anges toohello namnet på din cloud service eller web app.
    
     ![][27]
13. Ange värden för hello egenskaper om du vill att andra värden än hello standardvärden. hello egenskaper för Azure publicering finns i hello **distribution** avsnitt, och du kanske också tooset MSBuild-parametrar. Till exempel i molntjänstprojektet toospecify en tjänstkonfiguration än ”moln” och ange hello MSbuild-parametrar för`/p:TargetProfile=[YourProfile]` där *[YourProfile]* matchar en tjänstkonfigurationsfil med namnet ServiceConfiguration. *YourProfile*.cscfg.
    
     hello följande tabell visar hello tillgängliga egenskaper i hello **distribution** avsnitt:
    
    | Egenskap | Standardvärde |
    | --- | --- |
    | Tillåt ej betrodda certifikat |Om värdet är false måste SSL-certifikat signeras av en rotcertifikatutfärdare. |
    | Tillåt uppgradering |Tillåter hello distribution tooupdate en befintlig distribution i stället för att skapa en ny. Bevarar hello IP-adress. |
    | Ta inte bort |Om värdet är true, Skriv inte över en befintlig orelaterade distribution (uppgraderingen tillåts). |
    | Sökvägen tooDeployment inställningar |hello sökväg tooyour .pubxml fil för en webbapp, relativa toohello rotmapp hello lagringsplatsen. Ignoreras för molntjänster. |
    | SharePoint-miljön för distribution |Hej samma som hello tjänstnamn. |
    | Av Azure-distributionsmiljö |hello web app eller molnet tjänstnamn. |
14. Vid den tidpunkten ska din build slutföras.
    
     ![][28]
15. Om du dubbelklickar på hello build namn, Visual Studio visas en **skapa sammanfattning**, inklusive alla resultat från associerade testprojekt.
    
     ![][29]
16. I hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885), du kan visa hello associerad distribution på hello **distributioner** fliken när hello mellanlagring miljön är markerad.
    
     ![][30]
17. Bläddra tooyour webbplatsens URL. För en webbapp väljer hello **Bläddra** knappen hello-portalen. Välj hello URL för en molntjänst i hello **snabb i korthet** avsnitt i hello **instrumentpanelen** sidan som visar hello mellanlagringsmiljön.
    
    Distributioner från kontinuerlig integration för molntjänster är publicerade toohello mellanlagringsmiljön som standard. Du kan ändra detta genom att ange hello **alternativa Molntjänstmiljö** egenskapen för**produktion**. Här är där hello Webbadress på hello molnbaserad tjänst instrumentpanelssida.
    
    ![][31]
    
    En ny webbläsarflik öppnas tooreveal webbplatsen körs.
    
    ![][32]
18. Om du gör andra ändringar tooyour projekt och du utlösaren bygger mer du samlar flera distributioner. hello senaste en är markerad som aktiv.
    
    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: Distribuera om en tidigare version
Det här steget är valfritt. I hello klassiska Azure-portalen, Välj en tidigare distribution och **omdistribuera** toorewind din webbplats tooan tidigare incheckning. Observera att detta utlöser en ny version i TFS och skapa en ny post i distributionshistoriken.

![][34]

## <a name="6-change-hello-production-deployment"></a>6: ändra hello Produktionsdistribution
När du är klar kan du befordra hello mellanlagring toohello produktionsmiljön genom att välja **växla** i hello klassiska Azure-portalen. hello tidigare produktionsmiljö eventuella blir en mellanlagringsmiljön hello nyligen distribuerade mellanlagringsmiljön är upphöjt tooProduction. hello aktiv distribution kan vara olika för hello produktions- och mellanlagringsmiljöer, men hello distributionshistoriken för senare versioner är hello samma oavsett miljö.

![][35]

## <a name="7-deploy-from-a-working-branch"></a>7: Distribuera från en arbetsgren.
När du använder Git du vanligtvis göra ändringar i en arbetsgren och integrera i hello mastergrenen när du utvecklar når slutfört tillstånd. Under hello utvecklingsfasen av ett projekt du vill toobuild och distribuera hello fungerande gren tooAzure.

1. I **Team Explorer**, Välj hello **Start** knappen och välj sedan hello **filialer** knappen.
   
    ![][40]
2. Välj hello **ny gren** länk.
   
    ![][41]
3. Ange hello namnet på hello gren, till exempel ”arbetar” och välj **skapa gren**. Detta skapar en ny lokal gren.
   
    ![][42]
4. Publicera hello grenen. Välj hello gren namn i **Opublicerat filialer**, och välj **publicera**.
   
    ![][44]
5. Som standard bara ändras toohello mastergrenen utlösaren en kontinuerlig version. tooset in kontinuerlig build för en arbetsgren, Välj hello **bygger** sidan i **Team Explorer**, och välj **redigera skapa Definition**.
6. Öppna hello **inställningar för datakälla** fliken. Under **övervakas grenar för kontinuerlig integrering och bygga**, Välj **Klicka här tooadd en ny rad**.
   
    ![][47]
7. Ange hello grenen som du skapat, till exempel refs, huvuden/fungerande.
   
    ![][48]
8. Gör en ändring i hello kod, öppna hello snabbmenyn för hello ändras filen och välj sedan **genomför**.
   
    ![][43]
9. Välj hello **osynkroniserade incheckningar** länka och välj hello **Sync** knapp eller hello **Push** länk toocopy hello ändras toohello kopia av hello arbetsgren i Visual Studio Team Services.
   
   ![][45]
10. Navigera toohello **bygger** visa och hitta hello-version som precis har utlösts för hello arbetsgren.

## <a name="next-steps"></a>Nästa steg
toolearn mer tips om att använda Git med Visual Studio Team Services, se [utveckla och dela din kod i Git med Visual Studio](https://www.visualstudio.com/en-us/docs/git/share-your-code-in-git-vs-2017) och för information om hur du använder en Git-lagringsplats som inte hanteras av Visual Studio Team Services toopublish tooAzure, se [kontinuerlig distribution tooAzure Apptjänst](../app-service-web/app-service-continuous-deployment.md). Mer information om Visual Studio Team Services finns [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

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
