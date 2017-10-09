---
title: aaaContinuous leverans med Visual Studio Team Services i Azure | Microsoft Docs
description: "Lär dig hur tooconfigure din Visual Studio Team Services team projekt tooautomatically skapa och distribuera toohello webbprogram funktion i Azure App Service eller cloud services."
services: cloud-services
documentationcenter: .net
author: mlearned
manager: douge
editor: 
ms.assetid: 797f67ad-e4d4-4063-ae91-41cbdf154191
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/06/2016
ms.author: mlearned
ms.openlocfilehash: eae75729e1c1a55f9bc3375604a8192f329d0042
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-delivery-tooazure-using-visual-studio-team-services"></a>Kontinuerlig leverans tooAzure med hjälp av Visual Studio Team Services
Du kan konfigurera din Visual Studio Team Services team projekt tooautomatically skapa och distribuera tooAzure webbprogram och molntjänster.  (Mer information om hur tooset upp en kontinuerlig version och distribuera system med en *lokalt* Team Foundation Server finns [kontinuerlig leverans för molntjänster i Azure](cloud-services-dotnet-continuous-delivery.md).)

Den här kursen förutsätter att du har Visual Studio 2013 och hello Azure SDK är installerat. Om du inte redan har Visual Studio 2013 kan du hämta det genom att välja hello **Kom igång gratis** länka när [www.visualstudio.com](http://www.visualstudio.com). Installera hello Azure SDK från [här](http://go.microsoft.com/fwlink/?LinkId=239540).

> [!NOTE]
> Du behöver ett Visual Studio Team Services-konto toocomplete självstudierna: du kan [öppnar ett Visual Studio Team Services-konto gratis](http://go.microsoft.com/fwlink/p/?LinkId=512979).
> 
> 

tooset upp en cloud service tooautomatically skapa och distribuera tooAzure med hjälp av Visual Studio Team Services, Följ dessa steg.

## <a name="1-create-a-team-project"></a>1: skapa ett team projekt
Följ instruktionerna för hello [här](http://go.microsoft.com/fwlink/?LinkId=512980) toocreate ditt team projekt och länka det tooVisual Studio. Den här genomgången förutsätter att du använder Team Foundation Version kontrollen (TFVC) som käll-kontroll lösning. Om du vill toouse Git för versionskontroll, se [hello Git-versionen av den här genomgången](http://go.microsoft.com/fwlink/p/?LinkId=397358).

## <a name="2-check-in-a-project-toosource-control"></a>2: Kontrollera i projektet toosource kontroll
1. Öppna i Visual Studio hello lösning du vill toodeploy eller skapa en ny.
   Du kan distribuera en webbapp eller en tjänst i molnet (Azure-program) med följande hello steg i den här genomgången.
   Om du vill toocreate en ny lösning, skapa ett nytt Azure Cloud Service-projekt, eller ett nytt ASP.NET MVC-projekt. Kontrollera att som hello projektet mål .NET Framework 4 eller 4.5 och om du skapar ett molntjänstprojekt lägga till en ASP.NET MVC-webbroll och en arbetsroll och välj Internetprogram för hello webbroll. Välj **Internetprogram**.
   Om du vill toocreate en webbapp, Välj hello projektmall för ASP.NET-webbprogram och välja MVC. Se [skapa en ASP.NET-webbapp i Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md).
   
   > [!NOTE]
   > Visual Studio Team Services har endast stöd för CI-distributioner av Visual Studio-webbprogram just nu. Webbplatsen projekt ligger utanför omfånget.
   > 
   > 
2. Öppna hello snabbmenyn för hello lösning och välj **Lägg till lösning tooSource kontrollen**.
   
    ![][5]
3. Acceptera eller ändra hello standard och välj hello **OK** knappen. När hello är klart ikoner för datakällan visas i **Solution Explorer**.
   
    ![][6]
4. Öppna hello snabbmenyn för hello lösning och välj **checka In**.
   
    ![][7]
5. I hello **väntande ändringar** område i **Team Explorer**, skriver du en kommentar för incheckning hello och välj hello **checka In** knappen.
   
    ![][8]
   
    Observera hello alternativ tooinclude med eller undanta specifika ändringar när du checkar in. Om du vill ändringar utesluts, Välj hello **inkluderar alla** länk.
   
    ![][9]

## <a name="3-connect-hello-project-tooazure"></a>3: Anslut hello projekt tooAzure
1. Nu när du har ett VS Team Services team projekt med vissa källkoden i den är du redo tooconnect ditt team projekt tooAzure.  I hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885), Välj din cloud service eller web app eller skapa en ny genom att välja hello  **+**  längst hello nedre vänstra hörnet och välja **Molntjänsten** eller **Webbapp** och sedan **Snabbregistrering**. Välj hello **Konfigurera publicering med Visual Studio Team Services** länk.
   
    ![][10]
2. Hello i guiden anger hello namnet på ditt konto i Visual Studio Team Services hello textrutan och klickar på hello **auktorisera nu** länk. Du kan uppmanas toosign i.
   
    ![][11]
3. I hello **anslutningsbegäran** popup-fönstret Välj hello **acceptera** knappen tooauthorize Azure tooconfigure ditt team projekt i VS Team Services.
   
    ![][12]
4. När tillståndet lyckas, ser du en listruta som innehåller en lista över dina Visual Studio Team Services projekt. Välj hello namnet på grupprojekt som du skapade i föregående steg i hello och välj hello guiden markering.
   
    ![][13]
5. När projektet har länkats visas några instruktioner för att kontrollera i ändringar tooyour Visual Studio Team Services grupprojekt.  På din nästa incheckning, Visual Studio Team Services skapar och distribuerar ditt projekt tooAzure.  Prova den här nu genom att klicka på hello **checka In från Visual Studio** länka och sedan hello **starta Visual Studio** länk (eller motsvarande hello **Visual Studio** knappen längst ned hello hello portal skärmen).
   
    ![][14]

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: utlösa en återskapning och omdistribuera ditt projekt
1. I Visual Studio **Team Explorer**, Välj hello **källa kontrollen Explorer** länk.
   
    ![][15]
2. Navigera tooyour lösningsfilen och öppna den.
   
    ![][16]
3. I **Solution Explorer**, öppna upp en fil och ändra den. Till exempel ändra hello filen `_Layout.cshtml` under hello vyer\\delad mapp i en MVC-webbroll.
   
    ![][17]
4. Redigera hello logotyp för hello webbplats och väljer **Ctrl + S** toosave hello-filen.
   
    ![][18]
5. I **Team Explorer**, Välj hello **väntande ändringar** länk.
   
    ![][19]
6. Ange en kommentar och välj hello **checka In** knappen.
   
    ![][20]
7. Välj hello **hem** knappen tooreturn toohello **Team Explorer** startsidan.
   
    ![][21]
8. Välj hello **bygger** länk tooview hello bygger pågår.
   
    ![][22]
   
    **Teamet Explorer** visar att en version har utlösts för din incheckning.
   
    ![][23]
9. Dubbelklicka hello namnet på hello skapa i förloppet tooview detaljerade loggfil som hello build fortskrider.
   
    ![][24]
10. Hello build är pågående, ta en titt på hello build definition som skapades när du länkade TFS tooAzure med hjälp av guiden hello.  Öppna hello snabbmenyn för hello build definition och välj **redigera skapa Definition**.
    
     ![][25]
    
     I hello **utlösaren** fliken ser du att hello build definition är standardinställningen toobuild på varje incheckning.
    
     ![][26]
    
     I hello **processen** fliken visas hello distributionsmiljö anges toohello namnet på din cloud service eller web app. Om du arbetar med webbappar skilja hello egenskaper som du ser sig från de som visas här.
    
     ![][27]
11. Ange värden för hello egenskaper om du vill att andra värden än hello standardvärden. hello egenskaper för Azure publicering finns i hello **distribution** avsnitt.
    
     hello följande tabell visar hello tillgängliga egenskaper i hello **distribution** avsnitt:
    
    | Egenskap | Standardvärde |
    | --- | --- |
    | Tillåt ej betrodda certifikat |Om värdet är false måste SSL-certifikat signeras av en rotcertifikatutfärdare. |
    | Tillåt uppgradering |Tillåter hello distribution tooupdate en befintlig distribution i stället för att skapa en ny. Bevarar hello IP-adress. |
    | Ta inte bort |Om värdet är true, Skriv inte över en befintlig orelaterade distribution (uppgraderingen tillåts). |
    | Sökvägen tooDeployment inställningar |hello sökväg tooyour .pubxml fil för en webbapp, relativa toohello rotmapp hello lagringsplatsen. Ignoreras för molntjänster. |
    | SharePoint-miljön för distribution |Hej samma som hello tjänstnamn. |
    | Av Azure-distributionsmiljö |hello web app eller molnet tjänstnamn. |
12. Om du använder flera konfigurationer (.cscfg-filer), kan du ange hello önskad konfiguration i hello **bygga, Avancerat MSBuild-argument** inställningen. Till exempel ange toouse ServiceConfiguration.Test.cscfg, MSBuild-argument alternativet `/p:TargetProfile=Test`.
    
     ![][38]
    
     Vid den tidpunkten ska din build slutföras.
    
     ![][28]
13. Om du dubbelklickar på hello build namn, Visual Studio visas en **skapa sammanfattning**, inklusive alla resultat från associerade testprojekt.
    
     ![][29]
14. I hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885), du kan visa hello associerad distribution på hello **distributioner** fliken när hello mellanlagring miljön är markerad.
    
     ![][30]
15. Bläddra tooyour webbplatsens URL. För en webbapp och klicka bara på hello **Bläddra** hello kommandofältet-knappen. Välj hello URL för en molntjänst i hello **snabb i korthet** avsnitt i hello **instrumentpanelen** sidan som visar hello mellanlagringsmiljön för en tjänst i molnet. Distributioner från kontinuerlig integration för molntjänster är publicerade toohello mellanlagringsmiljön som standard. Du kan ändra detta genom att ange hello **alternativa Molntjänstmiljö** egenskapen för**produktion**. Den här skärmbilden visar där hello Webbadress är på hello molnbaserad tjänst instrumentpanelssida.
    
    ![][31]
    
    En ny webbläsarflik öppnas tooreveal webbplatsen körs.
    
    ![][32]
    
    För molntjänster, om du gör andra ändringar tooyour projekt bygger mer du utlösare och du samlar flera distributioner. hello senaste markerats som aktiv.
    
    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: Distribuera om en tidigare version
Det här steget gäller toocloud tjänster och är valfritt. I hello klassiska Azure-portalen, Välj en tidigare distribution och sedan hello **omdistribuera** knappen toorewind din webbplats tooan tidigare incheckning.  Observera att detta utlöser en ny version i TFS och skapa en ny post i distributionshistoriken.

![][34]

## <a name="6-change-hello-production-deployment"></a>6: ändra hello Produktionsdistribution
Det här steget gäller endast toocloud tjänster, inte webbprogram. När du är klar kan du befordra hello mellanlagring toohello produktionsmiljön genom att välja hello **växla** knapp i hello klassiska Azure-portalen. hello tidigare produktionsmiljö eventuella blir en mellanlagringsmiljön hello nyligen distribuerade mellanlagringsmiljön är upphöjt tooProduction. hello aktiv distribution kan vara olika för hello produktions- och mellanlagringsmiljöer, men hello distributionshistoriken för senare versioner är hello samma oavsett miljö.

![][35]

## <a name="7-run-unit-tests"></a>7: kör kontroller
Det här steget gäller endast tooweb appar, inte molntjänster. tooput en kvalitet gate på din distribution, kan du köra kontroller och om de inte kan du stoppa hello-distribution.

1. Lägga till en enhet test-projekt i Visual Studio.
   
   ![][39]
2. Lägg till projektet referenser toohello projektet som du vill tootest.
   
   ![][40]
3. Lägga till vissa kontroller. tooget igång, försök ett dummy test som släpps alltid.
   
       ```
       using System;
       using Microsoft.VisualStudio.TestTools.UnitTesting;
   
       namespace UnitTestProject1
       {
           [TestClass]
           public class UnitTest1
           {
               [TestMethod]
               [ExpectedException(typeof(NotImplementedException))]
               public void TestMethod1()
               {
                   throw new NotImplementedException();
               }
           }
       }
       ```
4. Redigera definition av hello build, Välj hello **processen** fliken och expandera hello **Test** nod.
5. Ange hello **misslyckas bygger vidare testet misslyckades** tooTrue. Detta innebär att hello distributionen inte uppstår om hello tester lyckas.
   
   ![][41]
6. Kön en ny version.
   
   ![][42]
   
   ![][43]
7. Medan hello build fortsätter, kontrollera förloppet.
   
    ![][44]
   
    ![][45]
8. Kontrollera hello testresultaten när hello build är klar.
   
    ![][46]
   
    ![][47]
9. Försök att skapa ett test som misslyckas. Lägg till ett nytt test genom att kopiera hello första, byta namn på den och kommentera ut hello kodrad om NotImplementedException är ett förväntat undantag.
   
       ```
       [TestMethod]
       //[ExpectedException(typeof(NotImplementedException))]
       public void TestMethod2()
       {
           throw new NotImplementedException();
       }
       ```
10. Incheckning hello ändra tooqueue en ny version.
    
     ![][48]
11. Visa hello test resultat toosee information om hello-fel.
    
     ![][49]
    
     ![][50]

## <a name="next-steps"></a>Nästa steg
Mer information om enhet testning i Visual Studio Team Services finns i [köra kontroller i din build](http://go.microsoft.com/fwlink/p/?LinkId=510474). Om du använder Git finns [dela din kod i Git](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) och [kontinuerlig distribution tooAzure Apptjänst](../app-service-web/app-service-continuous-deployment.md).  Läs mer om Visual Studio Team Services [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso/tfs1.png
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png

[5]: ./media/cloud-services-continuous-delivery-use-vso/tfs5.png
[6]: ./media/cloud-services-continuous-delivery-use-vso/tfs6.png
[7]: ./media/cloud-services-continuous-delivery-use-vso/tfs7.png
[8]: ./media/cloud-services-continuous-delivery-use-vso/tfs8.png
[9]: ./media/cloud-services-continuous-delivery-use-vso/tfs9.png
[10]: ./media/cloud-services-continuous-delivery-use-vso/tfs10.png
[11]: ./media/cloud-services-continuous-delivery-use-vso/tfs11.png
[12]: ./media/cloud-services-continuous-delivery-use-vso/tfs12.png
[13]: ./media/cloud-services-continuous-delivery-use-vso/tfs13.png
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso/tfs18.png
[19]: ./media/cloud-services-continuous-delivery-use-vso/tfs19.png
[20]: ./media/cloud-services-continuous-delivery-use-vso/tfs20.png
[21]: ./media/cloud-services-continuous-delivery-use-vso/tfs21.png
[22]: ./media/cloud-services-continuous-delivery-use-vso/tfs22.png
[23]: ./media/cloud-services-continuous-delivery-use-vso/tfs23.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso/tfs27.png
[28]: ./media/cloud-services-continuous-delivery-use-vso/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso/tfs37.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso/AdvancedMSBuildSettings.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso/AddUnitTestProject.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso/AddProjectReferences.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso/EditBuildDefinitionForTest.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso/QueueNewBuild.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso/QueueBuildDialog.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso/BuildInTeamExplorer.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso/BuildRequestInfo.PNG
[46]: ./media/cloud-services-continuous-delivery-use-vso/BuildSucceeded.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso/TestResultsPassed.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso/CheckInChangeToMakeTestsFail.PNG
[49]: ./media/cloud-services-continuous-delivery-use-vso/TestsFailed.PNG
[50]: ./media/cloud-services-continuous-delivery-use-vso/TestsResultsFailed.PNG
