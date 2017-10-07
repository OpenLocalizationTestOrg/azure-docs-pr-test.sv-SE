---
title: aaaDeploy ett webbprogram med en mall - Azure Cosmos DB | Microsoft Docs
description: "Lär dig hur toodeploy ett konto i Azure Cosmos DB, Azure App Service Web Apps och ett exempel på webbprogrammet med en Azure Resource Manager-mall."
services: cosmos-db, app-service\web
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 087d8786-1155-42c7-924b-0eaba5a8b3e0
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.openlocfilehash: b2bdde9279aad570606d7bf06dfc710f564b4d0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-azure-cosmos-db-and-azure-app-service-web-apps-using-an-azure-resource-manager-template"></a>Distribuera Azure Cosmos DB och Azure App Service Web Apps med en Azure Resource Manager-mall
De här självstudierna visar hur toouse toodeploy en Azure Resource Manager-mall och integrera [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/), en [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webbapp och en exempelwebbapp.

Med Azure Resource Manager-mallar kan automatisera du enkelt hello distributionen och konfigurationen av Azure-resurser.  Den här kursen visar hur toodeploy ett webbprogram och automatiskt konfigurera Azure Cosmos DB kontoinformation för anslutningen.

När du har slutfört den här självstudiekursen kommer du att kunna tooanswer hello följande frågor:  

* Hur kan använda en Azure Resource Manager-mall toodeploy och integrera Azure DB som Cosmos-konto och en webbapp i Azure App Service?
* Hur kan använda en Azure Resource Manager-mall toodeploy och integrera Azure DB som Cosmos-konto, en webbapp i App Service Web Apps och Webdeploy-program?

<a id="Prerequisites"></a>

## <a name="prerequisites"></a>Krav
> [!TIP]
> När den här kursen inte förutsätter tidigare erfarenhet av Azure Resource Manager-mallar eller JSON, bör du inte vill toomodify hello refereras mallar eller distributionsalternativ och kunskap om dessa olika områden kommer att krävas.
> 
> 

Se till att du har hello följande innan du följer hello instruktioner i den här självstudiekursen:

* En Azure-prenumeration. Azure är en plattform som baseras på prenumerationer.  Mer information om hur du skaffar en prenumeration finns [köpalternativ](https://azure.microsoft.com/pricing/purchase-options/), [Medlemserbjudanden](https://azure.microsoft.com/pricing/member-offers/), eller [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).

## <a id="CreateDB"></a>Steg 1: Hämta hello mallfilerna
Låt oss börja genom att hämta hello mallfilerna som vi ska använda i den här kursen.

1. Hämta hello [skapa ett Azure DB som Cosmos-konto, Web Apps och distribuerar ett exempel på program demo](https://portalcontent.blob.core.windows.net/samples/DocDBWebsiteTodo.json) mallen tooa lokal mapp (t.ex. C:\Azure Cosmos DBTemplates). Den här mallen ska distribuera ett Azure DB som Cosmos-konto, en App Service webbapp och ett webbprogram.  Hello web application tooconnect toohello Azure Cosmos DB konto konfigurerar automatiskt.
2. Hämta hello [skapa ett Azure DB som Cosmos-konto och Web Apps exempel](https://portalcontent.blob.core.windows.net/samples/DocDBWebSite.json) mallen tooa lokal mapp (t.ex. C:\Azure Cosmos DBTemplates). Den här mallen ska distribuera ett Azure DB som Cosmos-konto, en Apptjänst-webbapp och ändrar hello platsens program inställningar tooeasily ytan Azure Cosmos DB-anslutningsinformationen, men innehåller inte ett webbprogram.  

<a id="Build"></a>

## <a name="step-2-deploy-hello-azure-cosmos-db-account-app-service-web-app-and-demo-application-sample"></a>Steg 2: Distribuera hello Azure Cosmos DB konto App Service web app och demo programmet exempel
Nu ska vi distribuera vårt första mallen.

> [!TIP]
> hello mallen kan inte valideras hello web app och Azure Cosmos DB kontonamn som anges nedan är en) giltiga och b) tillgängliga.  Vi rekommenderar att du kontrollerar hello tillgängligheten för hello namn du planera toosupply tidigare toosubmitting hello-distribution.
> 
> 

1. Inloggningen toohello [Azure Portal](https://portal.azure.com), klickar du på nytt och Sök efter ”malldistribution”.
    ![Skärmbild av hello malldistribution UI](./media/create-website/TemplateDeployment1.png)
2. Markerar hello mall för distribution och klickar på **skapa** ![Skärmbild av hello malldistribution UI](./media/create-website/TemplateDeployment2.png)
3. Klicka på **redigera mallen**, klistra in hello innehållet i hello DocDBWebsiteTodo.json mallfilen och på **spara**.
   ![Skärmbild av hello malldistribution UI](./media/create-website/TemplateDeployment3.png)
4. Klicka på **redigera parametrar**, ange värden för varje hello obligatoriska parametrar och på **OK**.  hello parametrar är följande:
   
   1. PLATSNAMN: Anger hello Apptjänst webbprogrammets namn och är används tooconstruct hello URL som du ska använda tooaccess hello webbprogram (t.ex. Om du anger ”mydemodocdbwebapp” och sedan hello-URL som du kommer åt hello webbprogram kommer att mydemodocdbwebapp.azurewebsites.NET).
   2. HOSTINGPLANNAME: Anger hello av App Service som värd för planen toocreate.
   3. PLATS: Anger hello Azure-plats i vilka toocreate hello Azure Cosmos DB och web app resurser.
   4. DATABASEACCOUNTNAME: Anger hello Azure Cosmos DB konto toocreate hello namn.   
      
      ![Skärmbild av hello malldistribution UI](./media/create-website/TemplateDeployment4.png)
5. Välj en befintlig resursgrupp eller ange ett namn toomake en ny resursgrupp och välja en plats för resursgruppen hello.

    ![Skärmbild av hello malldistribution UI](./media/create-website/TemplateDeployment5.png)
6. Klicka på **Granska juridiska villkor**, **inköp**, och klicka sedan på **skapa** toobegin hello distribution.  Välj **PIN-kod toodashboard** så hello resulterande distribution är synlig på startsidan Azure portal.
   ![Skärmbild av hello malldistribution UI](./media/create-website/TemplateDeployment6.png)
7. När hello distributionen är klar öppnas hello blad för resursgrupp.
   ![Skärmbild av hello blad för resursgrupp](./media/create-website/TemplateDeployment7.png)  
8. toouse programmet hello, bara gå toohello web app-URL (hello vore i exemplet ovan hello URL http://mydemodocdbwebapp.azurewebsites.net).  Hello följande webbprogram visas:
   
   ![Todo exempelprogrammet](./media/create-website/image2.png)
9. Gå vidare och skapa några uppgifter i hello webbapp och sedan returnera toohello blad för resursgrupp i hello Azure-portalen. Hello Azure Cosmos DB konto resurs i hello resurslistan och klicka sedan på **Frågeutforskaren**.
    ![Skärmbild av hello sammanfattning lins med hello webbprogrammet markerat](./media/create-website/TemplateDeployment8.png)  
10. Kör hello standardfråga ”Välj * från c” och inspektera hello resultat.  Observera att hello-frågan har hämtats hello JSON-representation av hello todo-objekt som du skapade i steg 7 ovan.  Känna sig fria tooexperiment med frågor. exempelvis kör SELECT * FROM c WHERE c.isComplete = true tooreturn alla todo-objekt som har markerats som slutförd.
    
    ![Skärmbild av hello Frågeutforskaren och resultat blad som visar hello frågeresultat](./media/create-website/image5.png)
11. Känna sig fria tooexplore hello Azure DB som Cosmos-portaler eller ändra hello Todo exempelprogrammet.  När du är klar kan vi distribuera en annan mall.

<a id="Build"></a> 

## <a name="step-3-deploy-hello-document-account-and-web-app-sample"></a>Steg 3: Distribuera hello dokumentet konto och web app exempelkod
Nu ska vi distribuera våra andra mallen.  Den här mallen är användbara tooshow hur du kan mata in Azure Cosmos DB anslutningsinformation, till exempel kontot slutpunkt och huvudnyckeln i en webbapp som programinställningar eller som en anpassad anslutningssträng. Till exempel har du kanske egna webbprogram som du skulle t.ex toodeploy med ett konto i Azure Cosmos DB och har hello anslutningsinformationen fylls automatiskt under distributionen.

> [!TIP]
> hello mallen kan inte valideras hello web app och Azure Cosmos DB kontonamn som anges nedan är en) giltiga och b) tillgängliga.  Vi rekommenderar att du kontrollerar hello tillgängligheten för hello namn du planera toosupply tidigare toosubmitting hello-distribution.
> 
> 

1. I hello [Azure Portal](https://portal.azure.com), klickar du på nytt och Sök efter ”malldistribution”.
    ![Skärmbild av hello malldistribution UI](./media/create-website/TemplateDeployment1.png)
2. Markerar hello mall för distribution och klickar på **skapa** ![Skärmbild av hello malldistribution UI](./media/create-website/TemplateDeployment2.png)
3. Klicka på **redigera mallen**, klistra in hello innehållet i hello DocDBWebSite.json mallfilen och på **spara**.
   ![Skärmbild av hello malldistribution UI](./media/create-website/TemplateDeployment3.png)
4. Klicka på **redigera parametrar**, ange värden för varje hello obligatoriska parametrar och på **OK**.  hello parametrar är följande:
   
   1. PLATSNAMN: Anger hello Apptjänst webbprogrammets namn och är används tooconstruct hello URL som du ska använda tooaccess hello webbprogram (t.ex. Om du anger ”mydemodocdbwebapp” och sedan hello-URL som du kommer åt hello webbprogram kommer att mydemodocdbwebapp.azurewebsites.NET).
   2. HOSTINGPLANNAME: Anger hello av App Service som värd för planen toocreate.
   3. PLATS: Anger hello Azure-plats i vilka toocreate hello Azure Cosmos DB och web app resurser.
   4. DATABASEACCOUNTNAME: Anger hello Azure Cosmos DB konto toocreate hello namn.   
      
      ![Skärmbild av hello malldistribution UI](./media/create-website/TemplateDeployment4.png)
5. Välj en befintlig resursgrupp eller ange ett namn toomake en ny resursgrupp och välja en plats för resursgruppen hello.

    ![Skärmbild av hello malldistribution UI](./media/create-website/TemplateDeployment5.png)
6. Klicka på **Granska juridiska villkor**, **inköp**, och klicka sedan på **skapa** toobegin hello distribution.  Välj **PIN-kod toodashboard** så hello resulterande distribution är synlig på startsidan Azure portal.
   ![Skärmbild av hello malldistribution UI](./media/create-website/TemplateDeployment6.png)
7. När hello distributionen är klar öppnas hello blad för resursgrupp.
   ![Skärmbild av hello blad för resursgrupp](./media/create-website/TemplateDeployment7.png)  
8. Hello webbprogrammet resurs i hello resurslistan och klicka sedan på **programinställningar** ![Skärmbild av hello resursgruppen.](./media/create-website/TemplateDeployment9.png)  
9. Observera hur programinställningar för hello Azure Cosmos DB slutpunkt och var och en av hello Azure Cosmos DB huvudnycklar.

    ![Skärmbild av programinställningar](./media/create-website/TemplateDeployment10.png)  
10. Känna sig fria toocontinue utforska hello Azure-portalen, eller Använd en av våra Azure Cosmos-DB [exempel](http://go.microsoft.com/fwlink/?LinkID=402386) toocreate Azure Cosmos DB programmet.

<a name="NextSteps"></a>

## <a name="next-steps"></a>Nästa steg
Grattis! Du har distribuerat Azure Cosmos DB App Service webbapp och ett exempelwebbprogram med hjälp av Azure Resource Manager-mallar.

* toolearn mer om Azure Cosmos DB klickar du på [här](http://azure.com/docdb).
* toolearn mer om Azure App Service Web apps, klickar du på [här](http://go.microsoft.com/fwlink/?LinkId=325362).
* toolearn mer om Azure Resource Manager-mallar, klickar du på [här](https://msdn.microsoft.com/library/azure/dn790549.aspx).

## <a name="whats-changed"></a>Nyheter
* En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)
* En guide toohello ändring av hello gamla portalen toohello nya portalen finns: [referens för navigering hello klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkId=529715)

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](http://go.microsoft.com/fwlink/?LinkId=523751), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

