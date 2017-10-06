---
title: "aaaContinuous distribution tooAzure Apptjänst | Microsoft Docs"
description: "Lär dig hur tooenable kontinuerlig distribution tooAzure Apptjänst."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: 6adb5c84-6cf3-424e-a336-c554f23b4000
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/28/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 62a22cbda354fd5b0a1b9729c8c375408e75049f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-deployment-tooazure-app-service"></a>Kontinuerlig distribution tooAzure Apptjänst
De här självstudierna visar hur tooconfigure ett arbetsflöde för kontinuerlig distribution för din [Azure App Service] app. Apptjänst-integrering med BitBucket, GitHub och [Visual Studio Team Services VSTS ()](https://www.visualstudio.com/team-services/) gör en kontinuerlig arbetsflöde för distribution där Azure tar emot hello de senaste uppdateringarna från projektet publicerade tooone av dessa tjänster. Kontinuerlig distribution är ett bra alternativ för projekt där flera och ofta återkommande bidrag integreras.

toofind ut hur tooconfigure kontinuerlig distribution manuellt från en moln-databas som inte visas av hello Azure-portalen (exempelvis [GitLab](https://gitlab.com/)), se [ställa in kontinuerlig distribution med manuella steg](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

## <a name="overview"></a>Aktivera kontinuerlig distribution
tooenable kontinuerlig distribution

1. Publicera appen innehåll toohello databasen som ska användas för kontinuerlig distribution.  
    Mer information om hur du publicerar dina projekt toothese tjänster finns [skapa lagringsplatsen (GitHub)], [skapa lagringsplatsen (BitBucket)], och [Kom igång med VSTS].
2. I bladet för din app-menyn i hello [Azure-portalen], klickar du på **APPDISTRIBUTION > distributionsalternativ**. Klicka på **Välj källa**och välj hello distributionskälla.  
   
    ![](./media/app-service-continuous-deployment/cd_options.png)
   
   > [!NOTE]
   > tooconfigure ett VSTS konto för distribution av App Service finns [kursen](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).
   > 
   > 
3. Slutför hello auktorisering arbetsflödet.
4. I hello **distributionskälla** bladet välj hello projekt och avdelningskontor toodeploy från. Klicka på **OK** när du är klar.
   
    ![](./media/app-service-continuous-deployment/github_option.png)
   
   > [!NOTE]
   > När du aktiverar kontinuerlig distribution med GitHub eller BitBucket visas både offentliga och privata projekt.
   > 
   > 
   
    Skapar en association med hello markerade databasen Apptjänst, tar emot hello filer från hello angivna grenen och underhåller en klon av databasen för din Apptjänst-app. När du konfigurerar VSTS kontinuerlig distribution från hello Azure-portalen hello integration använder hello Apptjänst [Kudu-distributionsmotorn](https://github.com/projectkudu/kudu/wiki), vilken redan automatiserar bygg- och distributionsprocessen uppgifter med varje `git push`. Du behöver inte tooseparately ställa in kontinuerlig distribution i VSTS. När den här processen är klar hello **distributionsalternativ** app bladet visar en aktiv distribution som anger att distributionen har slutförts.
5. tooverify hello appen har distribuerats, klicka på hello **URL** hello överst på bladet hello app i hello Azure-portalen.
6. tooverify kontinuerlig distribution sker från valfri, hello lagringsplats push en ändring toohello databas. Appen bör uppdatera tooreflect hello ändringar kort efter hello push toohello databasen har slutförts. Du kan verifiera att den har mottagit hello uppdatering i hello **distributionsalternativ** bladet för din app.

## <a name="VSsolution"></a>Kontinuerlig distribution av en Visual Studio-lösning
Överföra en App Service med Visual Studio-lösning tooAzure är lika enkelt som att trycka på en enkel index.html-fil. Hej Apptjänst distributionsprocessen förenklar alla hello-information, inklusive återställa NuGet beroenden och bygga hello binärfiler. Du kan följa hello källa kontrollen bästa praxis för underhåll av koden endast i Git-lagringsplatsen och låta App Service-distributionen tar hand om hello rest.

hello steg för att överföra din tjänst i Visual Studio-lösning tooApp hello samma som i hello [föregående avsnitt](#overview), förutsatt att du konfigurerar din lösning och databasen på följande sätt:

* Använd hello Visual Studio källa kontrollen alternativet toogenerate en `.gitignore` filen, till exempel hello bild nedan eller Lägg manuellt till en `.gitignore` filen i Lagringsplatsens rot med innehåll liknande toothis [.gitignore exempel](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore).
  
  ![](./media/app-service-continuous-deployment/VS_source_control.png)
* Lägg till hello hela lösningen directory trädet tooyour databasen med hello SLN-filen i roten för hello-databasen.

När du har Ställ in din databas som beskrivs och konfigurerat din app i Azure för kontinuerlig publicering från en hello online Git databaser, kan du utveckla din ASP.NET-program lokalt i Visual Studio och kontinuerligt av helt enkelt distribuera din kod Skicka ändringar tooyour online Git-lagringsplatsen.

## <a name="disableCD"></a>Inaktivera kontinuerlig distribution
toodisable kontinuerlig distribution

1. I bladet för din app-menyn i hello [Azure-portalen], klickar du på **APPDISTRIBUTION > distributionsalternativ**. Klicka på **frånkoppling** i hello **distributionsalternativ** bladet.
   
    ![](./media/app-service-continuous-deployment/cd_disconnect.png)
2. Efter **Ja** toohello bekräftelsemeddelande som du kan returnera tooyour appens blad och klicka på **APPDISTRIBUTION > distributionsalternativ** om du vill att tooset upp publicering från en annan källa.

## <a name="additional-resources"></a>Ytterligare resurser
* [Hur tooinvestigate vanliga problem med kontinuerlig distribution](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Hur toouse PowerShell för Azure]
* [Hur toouse hello Azure-kommandoradsverktyg för Mac- och Linux]
* [Git-dokumentation]
* [Kudu-projektet](https://github.com/projectkudu/kudu/wiki)
* [Använda Azure tooautomatically Generera en CI/CD-pipeline toodeploy en ASP.NET 4-app](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic)

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

[Azure App Service]: https://azure.microsoft.com/en-us/documentation/articles/app-service-changes-existing-services/
[Azure-portalen]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Hur toouse PowerShell för Azure]: /powershell/azureps-cmdlets-docs
[Hur toouse hello Azure-kommandoradsverktyg för Mac- och Linux]:../cli-install-nodejs.md
[Git-dokumentation]: http://git-scm.com/documentation

[skapa lagringsplatsen (GitHub)]: https://help.github.com/articles/create-a-repo
[skapa lagringsplatsen (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[Kom igång med VSTS]: https://www.visualstudio.com/docs/vsts-tfs-overview
