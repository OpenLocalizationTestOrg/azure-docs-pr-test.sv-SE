---
title: aaaDeploy ett Azure Service Fabric-program med kontinuerlig integration (Team Services) | Microsoft Docs
description: "Lär dig hur tooset in kontinuerlig integrering och distribution för ett Service Fabric-program med hjälp av Visual Studio Team Services.  Distribuera ett program tooa Service Fabric-kluster i Azure."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: ryanwi
ms.openlocfilehash: ba9a632b247b0f467e7b66fbe77b4ad54fb3d9ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-application-with-cicd-tooa-service-fabric-cluster"></a>Distribuera ett program med CI/CD tooa Service Fabric-kluster
Den här kursen ingår tre av en serie och beskriver hur tooset in kontinuerlig integrering och distribution för ett Azure Service Fabric-program med Visual Studio Team Services.  Ett befintligt Service Fabric-program behövs, hello programmet skapas i [skapar ett .NET-program](service-fabric-tutorial-create-dotnet-app.md) används som exempel.

Del tre av hello serie du lära dig hur du:

> [!div class="checklist"]
> * Lägg till kontrollen tooyour projekt
> * Skapa en build-definition i Team Services
> * Skapa en definition för versionen i Team Services
> * Distribuera och uppgradera ett program automatiskt

I den här självstudiekursen serien lär du dig hur du:
> [!div class="checklist"]
> * [Skapa ett .NET Service Fabric-program](service-fabric-tutorial-create-dotnet-app.md)
> * [Distribuera programmet hello tooa fjärrkluster](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * Konfigurera CI/CD: N med hjälp av Visual Studio Team Services

## <a name="prerequisites"></a>Krav
Innan du börjar den här kursen:
- Om du inte har en Azure-prenumeration kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Installera Visual Studio 2017](https://www.visualstudio.com/) och installera hello **Azure-utveckling** och **ASP.NET och web development** arbetsbelastningar.
- [Installera hello Service Fabric-SDK](service-fabric-get-started.md)
- Skapa ett Service Fabric-program, till exempel genom [följa de här självstudierna](service-fabric-tutorial-create-dotnet-app.md). 
- Skapa ett Windows Service Fabric-kluster i Azure, till exempel med [följa de här självstudierna](service-fabric-tutorial-create-cluster-azure-ps.md)
- Skapa en [Team Services-konto](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).

## <a name="download-hello-voting-sample-application"></a>Hämta hello röst exempelprogrammet
Om du inte att skapa hello röst exempelprogrammet [ingår i den här självstudiekursen serie](service-fabric-tutorial-create-dotnet-app.md), du kan ladda ned den. Kör hello efter kommandot tooclone hello exempel app databasen tooyour lokala datorn i ett kommandofönster.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="prepare-a-publish-profile"></a>Förbereda en publiceringsprofil
Nu när du har [skapat ett program](service-fabric-tutorial-create-dotnet-app.md) och har [distribueras hello programmet tooAzure](service-fabric-tutorial-deploy-app-to-party-cluster.md), du är klar tooset in kontinuerlig integration.  Förbereda en publiceringsprofil i ditt program för användning av hello Distributionsprocess som körs i Team Services.  hello publiceringsprofil bör vara konfigurerade tootarget hello som du skapat tidigare.  Starta Visual Studio och öppna ett befintligt projekt för Service Fabric-programmet.  I **Solution Explorer**, högerklicka på programmet hello och välj **publicera...** .

Välj en profil för målet i ditt program projektet toouse för kontinuerlig integration arbetsflödet, till exempel molnet.  Ange hello klustret Anslutningens slutpunkt.  Kontrollera hello **uppgradera hello programmet** kryssrutan så att programmet uppgraderas för varje distribution i Team Services.  Klicka på hello **spara** hyperlink toosave hello inställningar toohello publiceringsprofil och klicka sedan på **Avbryt** tooclose hello dialogrutan.  

![Push-profil][publish-app-profile]

## <a name="share-your-visual-studio-solution-tooa-new-team-services-git-repo"></a>Dela din Visual Studio-lösning tooa nytt Team Services Git-lagringsplatsen
Dela källfilerna programmet tooa grupprojekt i Team Services så att du kan generera versioner.  

Skapa en ny lokal Git repo för ditt projekt genom att välja **lägga till tooSource kontrollen** -> **Git** hello statusfältet i hello nedre högra hörnet i Visual Studio. 

I hello **Push** visa i **Team Explorer**väljer hello **publicera Git Repo** knappen **Push tooVisual Studio Team Services**.

![Push-Git repo][push-git-repo]

Verifiera din e-post och välj ditt konto i hello **Team Services domän** listrutan. Ange databasens namn och välj **publicera databasen**.

![Push-Git repo][publish-code]

Publicerar hello lagringsplatsen skapas ett nytt grupprojekt i ditt konto med samma namn som hello lokala lagringsplatsen hello. toocreate hello lagringsplatsen i en befintlig grupprojekt klickar du på **Avancerat** nästa för**databasen** namn och välj ett grupprojekt. Du kan visa koden på hello webbplatsen genom att välja **finns på hello web**.

## <a name="configure-continuous-delivery-with-vsts"></a>Konfigurera kontinuerlig leverans med VSTS
En definition av Team Services build beskriver ett arbetsflöde som består av en uppsättning build-åtgärder som utförs i tur och ordning. Skapa en definition av build som som producerar ett Service Fabric-programpaket och andra artefakter, toodeploy tooa Service Fabric-klustret. Lär dig mer om [Team Services skapa definitioner](https://www.visualstudio.com/docs/build/define/create). 

En definition av Team Services versionen beskriver ett arbetsflöde som distribuerar ett program paketet tooa kluster. När de används tillsammans hello skapa definition och versionen definition köra hello hela arbetsflödet från och med källan filer tooending med ett program som körs i klustret. Mer information om Team Services [viktiga definitioner](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).

### <a name="create-a-build-definition"></a>Skapa en build-definition
Öppna en webbläsare och gå tooyour nytt grupprojekt på: https://myaccount.visualstudio.com/Voting/Voting%20Team/_git/Voting. 

Välj hello **Skapa & släpper** sedan fliken **bygger**, sedan **+ ny definition**.  I **Välj en mall**väljer hello **Azure Service Fabric-programmet** mall och klicka på **tillämpa**. 

![Välj build-mall][select-build-template] 

hello röstning program innehåller ett .NET Core-projekt, så lägger du till en uppgift som återställer hello beroenden. I hello **uppgifter** väljer **+ Lägg till aktivitet** i hello längst ned till vänster. Sök på toofind ”kommandoraden” Hej kommandoradsaktivitet och klicka sedan på **Lägg till**. 

![Lägg till aktivitet][add-task] 

I hello ny aktivitet, anger du ”kör dotnet.exe” i **visningsnamn**, ”dotnet.exe” i **verktyget**, och ”återställa” i **argument**. 

![Ny aktivitet][new-task] 

I hello **utlösare** klickar du på hello **aktivera den här utlösaren** växla **kontinuerlig Integration**. 

Välj **Spara & kö** och anger ”värd VS2017” Hej **Agent kön**. Välj **kön** toomanually starta en version.  Bygger också utlösare på push eller incheckning.

toocheck ändringarna build växeln toohello **bygger** fliken.  När du har kontrollerat att hello build körs korrekt kan du definiera en definition av versionen som distribuerar programmet tooa klustret. 

### <a name="create-a-release-definition"></a>Skapa en definition för versionen  

Välj hello **Skapa & släpper** sedan fliken **versioner**, sedan **+ ny definition**.  I **skapa versionen definition**väljer hello **Azure Service Fabric-distribution** mall från hello listan och klickar på **nästa**.  Välj hello **skapa** källa, kontrollera hello **kontinuerlig distribution** och på **skapa**. 

I hello **miljöer** klickar du på **Lägg till** toohello höger i **klustret anslutning**.  Ange anslutningsnamn ”mysftestcluster”, en klusterslutpunkten ”tcp://mysftestcluster.westus.cloudapp.azure.com:19000” hello Azure Active Directory och certifikatet autentiseringsuppgifter för hello-kluster. Azure Active Directory-autentiseringsuppgifter för att definiera hello autentiseringsuppgifter som du vill toouse tooconnect toohello klustret i hello **användarnamn** och **lösenord** fält. Definiera för certifikatbaserad autentisering hello Base64-kodning av hello klienten certifikatfilen i hello **klientcertifikat** fältet.  Se hello hjälp popup-fönster på fältet för information om hur tooget värde.  Om certifikatet är lösenordsskyddad, definiera hello lösenord i hello **lösenord** fältet.  Klicka på **spara** toosave hello versionen definition.

![Lägga till kluster-anslutning][add-cluster-connection] 

Klicka på **körs på agent**och välj **finns VS2017** för **distribution kön**. Klicka på **spara** toosave hello versionen definition.

![Körs på agent][run-on-agent]

Välj **+ släpper** -> **skapa släpper** -> **skapa** toomanually skapa en version.  Kontrollera att hello distributionen lyckades och hello programmet körs i hello klustret.  Öppna en webbläsare och gå för[http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).  Observera hello programversion i det här exemplet är det ”1.0.0.20170616.3”. 

## <a name="commit-and-push-changes-trigger-a-release"></a>Bekräfta och skicka ändringar kan utlösa en Versionspost
tooverify som hello kontinuerlig integration pipeline fungerar genom att söka i vissa kodändringar tooTeam tjänster.    

När du skriver koden spåras automatiskt dina ändringar av Visual Studio. Genomför ändringar tooyour lokal Git-lagringsplats genom att välja hello väntande ändringar ikonen (![Väntande åtgärder][pending]) från hello statusfältet i hello nedre högra hörnet.

På hello **ändringar** i teamet Explorer, lägga till ett meddelande som beskriver uppdateringen och sedan spara ändringarna.

![Genomför alla][changes]

Välj hello opublicerade ändringar statusikon-fältet (![opublicerade ändringar][unpublished-changes]) eller hello Sync vyn i teamet Explorer. Välj **Push** tooupdate koden i Team Services/TFS.

![Skicka ändringar][push]

Push-överföring hello ändringar tooTeam Services automatiskt utlösare en version.  När hello build definition har slutförts skapas automatiskt en Versionspost och börja uppgradera hello programmet på hello klustret.

toocheck ändringarna build växeln toohello **bygger** fliken i **Team Explorer** i Visual Studio.  När du har kontrollerat att hello build körs korrekt kan du definiera en definition av versionen som distribuerar programmet tooa klustret.

Kontrollera att hello distributionen lyckades och hello programmet körs i hello klustret.  Öppna en webbläsare och gå för[http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).  Observera hello programversion i det här exemplet är det ”1.0.0.20170815.3”.

![Service Fabric Explorer][sfx1]

## <a name="update-hello-application"></a>Uppdatera hello program
Göra kodändringar i hello program.  Spara och genomför hello ändringar, följande hello föregående steg.

När hello uppgradera hello program börjar, kan du titta på hello Uppgraderingsförlopp i Service Fabric Explorer:

![Service Fabric Explorer][sfx2]

uppgradering av programmet hello kan ta några minuter. När hello uppgraderingen är klar kör hello programmet hello nästa version.  I det här exemplet ”1.0.0.20170815.4”.

![Service Fabric Explorer][sfx3]

## <a name="next-steps"></a>Nästa steg
I den här självstudiekursen lärde du dig att:

> [!div class="checklist"]
> * Lägg till kontrollen tooyour projekt
> * Skapa en build-definition
> * Skapa en definition för versionen
> * Distribuera och uppgradera ett program automatiskt

Nu när du har distribuerat ett program och konfigurerat kontinuerlig integration, försök hello följande:
- [Uppgradera en app](service-fabric-application-upgrade.md)
- [Testa en app](service-fabric-testability-overview.md) 
- [Övervaka och diagnostisera](service-fabric-diagnostics-overview.md)


<!-- Image References -->
[publish-app-profile]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishAppProfile.png
[push-git-repo]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishGitRepo.png
[publish-code]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishCode.png
[select-build-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectBuildTemplate.png
[add-task]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddTask.png
[new-task]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewTask.png
[set-continuous-integration]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SetContinuousIntegration.png
[add-cluster-connection]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddClusterConnection.png
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
[run-on-agent]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/RunOnAgent.png