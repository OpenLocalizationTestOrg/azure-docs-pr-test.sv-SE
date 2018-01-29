---
title: Distribuera ett Azure Service Fabric-program med kontinuerlig integration (Team Services) | Microsoft Docs
description: "Lär dig hur du ställer in kontinuerlig integrering och distribution för ett Service Fabric-program med hjälp av Visual Studio Team Services.  Distribuera ett program till ett Service Fabric-kluster i Azure."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/13/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 2fb7ab906208a58c0b5cd3af8b53188fbab94029
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/15/2017
---
# <a name="deploy-an-application-with-cicd-to-a-service-fabric-cluster"></a>Distribuera ett program med CI/CD: N till ett Service Fabric-kluster
Den här kursen ingår tre av en serie och beskriver hur du ställer in kontinuerlig integrering och distribution för ett Azure Service Fabric-program med Visual Studio Team Services.  Behövs för ett befintligt Service Fabric-program, programmet skapas i [skapar ett .NET-program](service-fabric-tutorial-create-dotnet-app.md) används som exempel.

I del tre av serien får du lära dig hur du:

> [!div class="checklist"]
> * Lägg till källkontroll i projektet
> * Skapa en build-definition i Team Services
> * Skapa en definition för versionen i Team Services
> * Distribuera och uppgradera ett program automatiskt

I den här självstudiekursen serien lär du dig hur du:
> [!div class="checklist"]
> * [Skapa ett .NET Service Fabric-program](service-fabric-tutorial-create-dotnet-app.md)
> * [Distribuera programmet till ett kluster](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * Konfigurera CI/CD: N med hjälp av Visual Studio Team Services
> * [Konfigurera övervakning och diagnostik för programmet](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="prerequisites"></a>Krav
Innan du börjar den här kursen:
- Om du inte har en Azure-prenumeration kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Installera Visual Studio 2017](https://www.visualstudio.com/) och installera den **Azure-utveckling** och **ASP.NET och web development** arbetsbelastningar.
- [Installera Service Fabric SDK](service-fabric-get-started.md)
- Skapa ett Windows Service Fabric-kluster i Azure, till exempel med [följa de här självstudierna](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
- Skapa en [Team Services-konto](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).

## <a name="download-the-voting-sample-application"></a>Ladda ned exempelprogrammet röst
Om du inte att skapa exempelprogrammet röst [ingår i den här självstudiekursen serie](service-fabric-tutorial-create-dotnet-app.md), du kan ladda ned den. Kör följande kommando i ett kommandofönster för att klona databasen för exempelappen till den lokala datorn.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="prepare-a-publish-profile"></a>Förbereda en publiceringsprofil
Nu när du har [skapat ett program](service-fabric-tutorial-create-dotnet-app.md) och har [distribuerat program till Azure](service-fabric-tutorial-deploy-app-to-party-cluster.md), är du redo att konfigurera kontinuerlig integration.  Förbereda en publiceringsprofil i ditt program för användning av distributionsprocessen som körs i Team Services.  Profilen som ska konfigureras för att rikta det kluster som du skapat tidigare.  Starta Visual Studio och öppna ett befintligt projekt för Service Fabric-programmet.  I **Solution Explorer**, högerklicka på programmet och välj **publicera...** .

Välj en profil för målet i projektet program att använda för kontinuerlig integration arbetsflödet, till exempel molnet.  Ange klustret Anslutningens slutpunkt.  Kontrollera den **uppgradera programmet** kryssrutan så att programmet uppgraderas för varje distribution i Team Services.  Klicka på den **spara** hyperlänk till spara inställningarna i profilen och klickar sedan på **Avbryt** att stänga dialogrutan.  

![Push-profil][publish-app-profile]

## <a name="share-your-visual-studio-solution-to-a-new-team-services-git-repo"></a>Dela din Visual Studio-lösning till ett nytt Team Services Git-lagringsplatsen
Dela källfilerna för programmet till ett team projekt i Team Services så att du kan generera versioner.  

Skapa en ny lokal Git repo för ditt projekt genom att välja **lägga till källkontroll** -> **Git** i statusfältet i det nedre högra hörnet av Visual Studio. 

I den **Push** visa i **Team Explorer**, Välj den **publicera Git Repo** knappen **Push till Visual Studio Team Services**.

![Push-Git repo][push-git-repo]

Verifiera din e-post och välj ditt konto i den **Team Services domän** listrutan. Ange databasens namn och välj **publicera databasen**.

![Push-Git repo][publish-code]

Publicerar lagringsplatsen skapas ett nytt grupprojekt i ditt konto med samma namn som den lokala lagringsplatsen. Klicka för att skapa lagringsplatsen i en befintlig grupprojekt **Avancerat** bredvid **databasen** namn och välj ett grupprojekt. Du kan visa koden på webben genom att välja **finns på webben**.

## <a name="configure-continuous-delivery-with-vsts"></a>Konfigurera kontinuerlig leverans med VSTS
En definition av Team Services build beskriver ett arbetsflöde som består av en uppsättning build-åtgärder som utförs i tur och ordning. Skapa en definition av build som som producerar ett Service Fabric-programpaket och andra artefakter att distribuera till ett Service Fabric-kluster. Lär dig mer om [Team Services skapa definitioner](https://www.visualstudio.com/docs/build/define/create). 

En definition av Team Services versionen beskriver ett arbetsflöde som distribuerar ett programpaket till ett kluster. Köra i hela arbetsflödet som börjar med källfiler som slutar med ett program som körs i klustret när de används tillsammans build definitionen och versionen definition. Mer information om Team Services [viktiga definitioner](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).

### <a name="create-a-build-definition"></a>Skapa en build-definition
Öppna en webbläsare och gå till det nya projektet för team på: [https://&lt;MITTKONTO&gt;.visualstudio.com/Voting/Voting%20Team/_git/Voting](https://myaccount.visualstudio.com/Voting/Voting%20Team/_git/Voting). 

Välj den **Skapa & släpper** sedan fliken **bygger**, sedan **+ ny definition**.  I **Välj en mall**, Välj den **Azure Service Fabric-programmet** mall och klicka på **tillämpa**. 

![Välj build-mall][select-build-template] 

I **uppgifter**, anger du ”värd VS2017” som den **Agent kön**. 

![Välj uppgifter][save-and-queue]

Under **utlösare**, aktivera kontinuerlig integration genom att ange **utlösa status**.  Välj **spara och kö** manuellt starta en version.  

![Välj utlösare][save-and-queue2]

Bygger också utlösare på push eller incheckning. Om du vill kontrollera förloppet build växla till den **bygger** fliken.  När du har kontrollerat att bygga körs korrekt kan du definiera en definition av versionen som distribuerar programmet till ett kluster. 

### <a name="create-a-release-definition"></a>Skapa en definition för versionen  

Välj den **Skapa & släpper** sedan fliken **versioner**, sedan **+ ny definition**.  I **Välj en mall**, Välj den **Azure Service Fabric-distribution** mall från listan och sedan **tillämpa**.  

![Välj versionsmall][select-release-template]

Välj **uppgifter**->**miljö 1** och sedan **+ ny** att lägga till en ny anslutning för klustret.

![Lägga till kluster-anslutning][add-cluster-connection]

I den **lägga till ny Service Fabric-anslutning** Visa Välj **certifikatbaserad** eller **Azure Active Directory** autentisering.  Ange ett anslutningsnamn ”mysftestcluster” och en klusterslutpunkten för ”tcp://mysftestcluster.southcentralus.cloudapp.azure.com:19000” (eller slutpunkt i klustret som du distribuerar till). 

För certifikatbaserad autentisering, lägga till den **Server certifikatets tumavtryck** för certifikatet används för att skapa klustret.  I **klientcertifikat**, lägga till Base64-kodning av klienten certifikatfilen. Om fältet för information om hur du hämtar den Base64-kodade representationen av certifikatet finns i Hjälp popup-fönstret. Också lägga till den **lösenord** för certifikatet.  Du kan använda certifikat för kluster eller server om du inte har ett separat klientcertifikat. 

Azure Active Directory-autentiseringsuppgifter för att lägga till den **Server certifikatets tumavtryck** för certifikatet används för att skapa klustret och autentiseringsuppgifterna som du vill använda för att ansluta till klustret i den **användarnamn** och **lösenord** fält. 

Klicka på **Lägg till** spara kluster-anslutningen.

Lägg sedan till ett build-artefakt pipeline så versionen definitionen kan hitta utdata från versionen. Välj **Pipeline** och **artefakter**->**+ Lägg till**.  I **källa (Build definition)**, Välj definitionen för den version som du skapade tidigare.  Klicka på **Lägg till** spara build-artefakt.

![Lägg till artefakt][add-artifact]

Aktivera en utlösare för kontinuerlig distribution så att en version skapas automatiskt när versionen har slutförts. Klicka på ikonen blixtsnabb i artefakten, aktivera utlösaren och på **spara** att spara versionen-definitionen.

![Aktivera utlösare][enable-trigger]

Välj **+ släpper** -> **skapa släpper** -> **skapa** att manuellt skapa en version.  Kontrollera att distributionen har slutförts och programmet körs i klustret.  Öppna en webbläsare och gå till [http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/).  Observera programversionen i det här exemplet är det ”1.0.0.20170616.3”. 

## <a name="commit-and-push-changes-trigger-a-release"></a>Bekräfta och skicka ändringar kan utlösa en Versionspost
Kontrollera att kontinuerlig integration pipeline fungerar genom att kontrollera kod ändrar till Team Services.    

När du skriver koden spåras automatiskt dina ändringar av Visual Studio. Genomför ändringar till din lokala Git-lagringsplats genom att välja ikonen-(väntande ändringar![Väntande åtgärder][pending]) från statusfältet i nederkant högra hörnet.

På den **ändringar** i teamet Explorer, lägga till ett meddelande som beskriver uppdateringen och sedan spara ändringarna.

![Genomför alla][changes]

Välj ikonen i opublicerade ändringar statusfältet (![opublicerade ändringar][unpublished-changes]) eller synkronisera vyn i teamet Explorer. Välj **Push** att uppdatera din kod i Team Services/TFS.

![Skicka ändringar][push]

Skicka ändringar till Team Services automatiskt utlöser en version.  När build-definition har slutförts, skapas automatiskt en version och börja uppgradera programmet på klustret.

Om du vill kontrollera förloppet build växla till den **bygger** fliken i **Team Explorer** i Visual Studio.  När du har kontrollerat att bygga körs korrekt kan du definiera en definition av versionen som distribuerar programmet till ett kluster.

Kontrollera att distributionen har slutförts och programmet körs i klustret.  Öppna en webbläsare och gå till [http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/).  Observera programversionen i det här exemplet är det ”1.0.0.20170815.3”.

![Service Fabric Explorer][sfx1]

## <a name="update-the-application"></a>Uppdatera programmet
Göra kodändringar i programmet.  Spara och genomföra ändringarna, följa de här stegen.

När du börjar uppgraderingen av programmet kan du titta på Uppgraderingsförlopp i Service Fabric Explorer:

![Service Fabric Explorer][sfx2]

Program-uppgraderingen kan ta flera minuter. När uppgraderingen är slutförd, kör programmet nästa version.  I det här exemplet ”1.0.0.20170815.4”.

![Service Fabric Explorer][sfx3]

## <a name="next-steps"></a>Nästa steg
I den här självstudiekursen lärde du dig att:

> [!div class="checklist"]
> * Lägg till källkontroll i projektet
> * Skapa en build-definition
> * Skapa en definition för versionen
> * Distribuera och uppgradera ett program automatiskt

Gå vidare till nästa kurs:
> [!div class="nextstepaction"]
> [Konfigurera övervakning och diagnostik för programmet](service-fabric-tutorial-monitoring-aspnet.md) 


<!-- Image References -->
[publish-app-profile]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishAppProfile.png
[push-git-repo]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishGitRepo.png
[publish-code]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishCode.png
[select-build-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectBuildTemplate.png
[save-and-queue]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SaveAndQueue.png
[save-and-queue2]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SaveAndQueue2.png
[select-release-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectReleaseTemplate.png
[set-continuous-integration]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SetContinuousIntegration.png
[add-cluster-connection]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddClusterConnection.png
[add-artifact]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddArtifact.png
[enable-trigger]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/EnableTrigger.png
[sfx1]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX1.png
[sfx2]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX2.png
[sfx3]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX3.png
[pending]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Pending.png
[changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Changes.png
[unpublished-changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/UnpublishedChanges.png
[push]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Push.png
[continuous-delivery-with-VSTS]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/VSTS-Dialog.png
[new-service-endpoint]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpoint.png
[new-service-endpoint-dialog]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpointDialog.png
