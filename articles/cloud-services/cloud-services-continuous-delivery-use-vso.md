---
title: Kontinuerlig leverans med Visual Studio Team Services i Azure | Microsoft Docs
description: "Lär dig hur du konfigurerar din Visual Studio Team Services grupprojekt för att automatiskt skapa och distribuera till funktionen Web App i Azure App Service eller cloud services."
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
ms.openlocfilehash: d80ce63eb7ddfd7c45726be887a772f9a7594b28
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="continuous-delivery-to-azure-using-visual-studio-team-services"></a>Kontinuerlig leverans till Azure med Visual Studio Team Services
Du kan konfigurera dina team projekt i Visual Studio Team Services för att automatiskt skapa och distribuera till Azure-webbappar eller molntjänster.  (Mer information om hur du ställer in en kontinuerlig version och distribuera system med hjälp av en *lokalt* Team Foundation Server finns [kontinuerlig leverans för molntjänster i Azure](cloud-services-dotnet-continuous-delivery.md).)

Den här kursen förutsätter att du har Visual Studio 2013 och Azure SDK om du installerade. Om du inte redan har Visual Studio 2013 kan du hämta det genom att välja den **Kom igång gratis** länka när [www.visualstudio.com](http://www.visualstudio.com). Installera Azure SDK från [här](http://go.microsoft.com/fwlink/?LinkId=239540).

> [!NOTE]
> Du behöver ett Visual Studio Team Services-konto för att slutföra den här självstudiekursen: du kan [öppnar ett Visual Studio Team Services-konto gratis](http://go.microsoft.com/fwlink/p/?LinkId=512979).
> 
> 

Följ dessa steg om du vill konfigurera en molntjänst för att automatiskt skapa och distribuera till Azure med hjälp av Visual Studio Team Services.

## <a name="1-create-a-team-project"></a>1: skapa ett team projekt
Följ instruktionerna [här](http://go.microsoft.com/fwlink/?LinkId=512980) att skapa projektet team och länka det till Visual Studio. Den här genomgången förutsätter att du använder Team Foundation Version kontrollen (TFVC) som käll-kontroll lösning. Om du vill använda Git för versionskontroll, se [Git-versionen av den här genomgången](http://go.microsoft.com/fwlink/p/?LinkId=397358).

## <a name="2-check-in-a-project-to-source-control"></a>2: Kontrollera i ett projekt till källkontroll
1. Öppnar du vill distribuera lösningen i Visual Studio eller skapa en ny.
   Du kan distribuera en webbapp eller en tjänst i molnet (Azure-program) genom att följa stegen i den här genomgången.
   Om du vill skapa en ny lösning, skapa ett nytt Azure Cloud Service-projekt, eller ett nytt ASP.NET MVC-projekt. Kontrollera att projektet riktar sig till .NET Framework 4 eller 4.5 och om du skapar ett molntjänstprojekt, lägga till en ASP.NET MVC-webbroll och en arbetsroll och välj Internetprogram för webbrollen. Välj **Internetprogram**.
   Om du vill skapa en webbapp Välj projektmall för ASP.NET-webbprogram och sedan välja MVC. Se [skapa en ASP.NET-webbapp i Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md).
   
   > [!NOTE]
   > Visual Studio Team Services har endast stöd för CI-distributioner av Visual Studio-webbprogram just nu. Webbplatsen projekt ligger utanför omfånget.
   > 
   > 
2. Öppna snabbmenyn för lösningen och välj **Lägg till lösning till källkontroll**.
   
    ![][5]
3. Acceptera eller ändra standardinställningar och välj den **OK** knappen. När processen har slutförts ikoner för datakällan visas i **Solution Explorer**.
   
    ![][6]
4. Öppna snabbmenyn för lösningen och välj **checka In**.
   
    ![][7]
5. I den **väntande ändringar** område i **Team Explorer**, skriver du en kommentar för att checka in och välj den **checka In** knappen.
   
    ![][8]
   
    Observera de alternativ som ska tas med eller undanta specifika ändringar när du checkar in. Om du vill ändringar utesluts, Välj den **inkluderar alla** länk.
   
    ![][9]

## <a name="3-connect-the-project-to-azure"></a>3: Anslut projektet till Azure
1. Nu när du har ett VS Team Services team projekt med vissa källkoden i den är du redo att ansluta din grupprojekt till Azure.  I den [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885), Välj din cloud service eller web app eller skapa en ny genom att välja den  **+**  ikonen längst ned vänster och välja **Molntjänsten** eller **Web App** och sedan **Snabbregistrering**. Välj den **Konfigurera publicering med Visual Studio Team Services** länk.
   
    ![][10]
2. I guiden anger du namnet på din Visual Studio Team Services-konto i textrutan och klicka på den **auktorisera nu** länk. Du kan uppmanas att logga in.
   
    ![][11]
3. I den **anslutningsbegäran** popup-fönstret väljer du den **acceptera** för att auktorisera Azure för att konfigurera ditt team projekt i VS Team Services.
   
    ![][12]
4. När tillståndet lyckas, ser du en listruta som innehåller en lista över dina Visual Studio Team Services projekt. Välj namnet på grupprojekt som du skapade i föregående steg och välj guidens markering.
   
    ![][13]
5. När projektet har länkats visas några instruktioner för att kontrollera ändringar i projektet Visual Studio Team Services-teamet.  Visual Studio Team Services kommer på din nästa incheckning, skapa och distribuera projektet till Azure.  Prova den här nu genom att klicka på den **checka In från Visual Studio** länk, och sedan den **starta Visual Studio** länk (eller motsvarande **Visual Studio** knappen längst ned på skärmen portal).
   
    ![][14]

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: utlösa en återskapning och omdistribuera ditt projekt
1. I Visual Studio **Team Explorer**, Välj den **källa kontrollen Explorer** länk.
   
    ![][15]
2. Navigera till din lösningsfilen och öppna den.
   
    ![][16]
3. I **Solution Explorer**, öppna upp en fil och ändra den. Till exempel ändra filen `_Layout.cshtml` under vyerna\\delad mapp i en MVC-webbroll.
   
    ![][17]
4. Redigera logotypen för platsen och välj **Ctrl + S** att spara filen.
   
    ![][18]
5. I **Team Explorer**, Välj den **väntande ändringar** länk.
   
    ![][19]
6. Ange en kommentar och välj sedan den **checka In** knappen.
   
    ![][20]
7. Välj den **hem** vill gå tillbaka till den **Team Explorer** startsidan.
   
    ![][21]
8. Välj den **bygger** länken om du vill visa versionerna pågår.
   
    ![][22]
   
    **Teamet Explorer** visar att en version har utlösts för din incheckning.
   
    ![][23]
9. Dubbelklicka på namnet på build pågår till en detaljerade loggen när build fortlöper.
   
    ![][24]
10. Versionen är pågående, ta en titt på build-definition som skapades när du har länkat TFS till Azure med hjälp av guiden.  Öppna snabbmenyn för build-definition och välj **redigera skapa Definition**.
    
     ![][25]
    
     I den **utlösaren** fliken ser du att skapa definition är standardinställningen att bygga på varje incheckning.
    
     ![][26]
    
     I den **processen** fliken visas distributionsmiljö har angetts till namnet på din cloud service eller web app. Om du arbetar med webbappar skilja egenskaper som du ser sig från de som visas här.
    
     ![][27]
11. Ange värden för egenskaper om du vill att andra värden än standardinställningarna. Egenskaperna för Azure-publicering finns i den **distribution** avsnitt.
    
     I följande tabell visas de tillgängliga egenskaperna i den **distribution** avsnitt:
    
    | Egenskap | Standardvärde |
    | --- | --- |
    | Tillåt ej betrodda certifikat |Om värdet är false måste SSL-certifikat signeras av en rotcertifikatutfärdare. |
    | Tillåt uppgradering |Gör att distributionen ska uppdatera en befintlig distribution i stället för att skapa en ny. Bevarar den IP-adressen. |
    | Ta inte bort |Om värdet är true, Skriv inte över en befintlig orelaterade distribution (uppgraderingen tillåts). |
    | Sökvägen till distributionsinställningar |Sökvägen till filen .pubxml för en webbapp i förhållande till rotmappen på lagringsplatsen. Ignoreras för molntjänster. |
    | SharePoint-miljön för distribution |Samma som namnet på tjänsten. |
    | Av Azure-distributionsmiljö |Web app eller molnet tjänstnamnet. |
12. Om du använder flera konfigurationer (.cscfg-filer), kan du ange den önskade tjänstinställningar i den **bygga, Avancerat MSBuild-argument** inställningen. Till exempel vill använda ServiceConfiguration.Test.cscfg anger MSBuild-argument alternativet `/p:TargetProfile=Test`.
    
     ![][38]
    
     Vid den tidpunkten ska din build slutföras.
    
     ![][28]
13. Om du dubbelklickar på build-namnet, Visual Studio visas en **skapa sammanfattning**, inklusive alla resultat från associerade testprojekt.
    
     ![][29]
14. I den [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885), du kan visa den associerade distributionen på de **distributioner** fliken när mellanlagringsmiljön är markerad.
    
     ![][30]
15. Gå till webbplatsens URL. För en webbapp, klicka bara på den **Bläddra** knappen i kommandofältet. För en tjänst i molnet, väljer du URL-Adressen i den **snabb i korthet** avsnitt i den **instrumentpanelen** sidan som visar mellanlagringsmiljön för en tjänst i molnet. Distributioner från kontinuerlig integration för molntjänster publiceras till mellanlagringsmiljön som standard. Du kan ändra detta genom att ange den **alternativa Molntjänstmiljö** egenskapen **produktion**. Den här skärmbilden visas där webbplatsens URL på den molntjänst instrumentpanelssida.
    
    ![][31]
    
    En ny webbläsarflik öppnas för att visa webbplatsen körs.
    
    ![][32]
    
    Om du gör andra ändringar i projektet för molntjänster, bygger mer du utlösare och du samlar flera distributioner. Den senaste som markerats som aktiv.
    
    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: Distribuera om en tidigare version
Det här steget gäller för molntjänster och är valfritt. Välj en tidigare distribution i den klassiska Azure-portalen och välj sedan den **omdistribuera** för att spola platsen till en tidigare incheckning.  Observera att detta utlöser en ny version i TFS och skapa en ny post i distributionshistoriken.

![][34]

## <a name="6-change-the-production-deployment"></a>6: ändra produktionsdistributionen
Det här steget gäller bara för molntjänster, inte webbprogram. När du är klar kan du befordra mellanlagringsmiljön till produktionsmiljön genom att välja den **växla** knappen i den klassiska Azure-portalen. Nyligen distribuerade mellanlagringsmiljön höjs till produktionen och föregående produktionsmiljön, eventuella blir en mellanlagringsmiljön. Aktiv distribution kan vara olika för produktions- och Mellanlagringsmiljöer, men distributionshistoriken för senare versioner är detsamma oavsett miljö.

![][35]

## <a name="7-run-unit-tests"></a>7: kör kontroller
Det här steget gäller bara för webbappar, inte molntjänster. Du kan köra kontroller för att placera en gate kvalitet på din distribution, och om de inte kan du stoppa distributionen.

1. Lägga till en enhet test-projekt i Visual Studio.
   
   ![][39]
2. Lägg till projektreferenser i projektet som du vill testa.
   
   ![][40]
3. Lägga till vissa kontroller. Försök ett dummy test som alltid ska gå att komma igång.
   
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
4. Redigera build-definition, Välj den **processen** fliken och expandera den **Test** nod.
5. Ange den **misslyckas bygger vidare testet misslyckades** till True. Detta innebär att distributionen inte uppstår om testet lyckas.
   
   ![][41]
6. Kön en ny version.
   
   ![][42]
   
   ![][43]
7. Medan bygga fortsätter, kontrollera förloppet.
   
    ![][44]
   
    ![][45]
8. Kontrollera testresultaten när bygga är klar.
   
    ![][46]
   
    ![][47]
9. Försök att skapa ett test som misslyckas. Lägg till ett nytt test genom att kopiera förstnämnda, byta namn på den och kommentera ut koden om NotImplementedException är ett förväntat undantag för.
   
       ```
       [TestMethod]
       //[ExpectedException(typeof(NotImplementedException))]
       public void TestMethod2()
       {
           throw new NotImplementedException();
       }
       ```
10. Kontrollera i Ändra till en ny version-kö.
    
     ![][48]
11. Visa testresultaten om du vill se information om felet.
    
     ![][49]
    
     ![][50]

## <a name="next-steps"></a>Nästa steg
Mer information om enhet testning i Visual Studio Team Services finns i [köra kontroller i din build](http://go.microsoft.com/fwlink/p/?LinkId=510474). Om du använder Git finns [dela din kod i Git](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) och [kontinuerlig distribution till Azure App Service](../app-service-web/app-service-continuous-deployment.md).  Läs mer om Visual Studio Team Services [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

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
