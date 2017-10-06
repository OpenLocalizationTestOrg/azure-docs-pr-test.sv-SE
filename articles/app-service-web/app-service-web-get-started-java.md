---
title: "aaaCreate din första Java-webbapp i Azure"
description: "Lär dig hur toorun webbappar i App Service genom att distribuera en grundläggande Java-app."
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: quickstart
ms.date: 6/7/2017
ms.author: cephalin;robmcm
ms.custom: mvc
ms.openlocfilehash: 81315c07b5aa84cbec50a17b2cb3914927b19c00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-java-web-app-in-azure"></a>Skapa din första Java-webbapp i Azure

Hej [Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) funktion i [Azure App Service](../app-service/app-service-value-prop-what-is.md) ger en mycket skalbar, automatisk uppdatering värdtjänst. Den här snabbstarten visar hur toodeploy en Java web app tooApp tjänsten med hjälp av hello [Eclipse IDE för Java EE-utvecklare](http://www.eclipse.org/).

!["Hello Azure!" exempelwebbapp](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="prerequisites"></a>Krav

toocomplete Snabbstart, installera:

* hello ledigt [Eclipse IDE för Java EE-utvecklare](http://www.eclipse.org/downloads/). Den här snabbstarten använder Eclipse Neon.
* Hej [Azure Toolkit för Eclipse](/azure/azure-toolkit-for-eclipse-installation).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-dynamic-web-project-in-eclipse"></a>Skapa ett dynamiskt webbprojekt i Eclipse

Välj **Arkiv** > **Nytt** > **Dynamic Web Project** (Dynamiskt webbprojekt) Eclipse.

I hello **nya dynamiskt webbprojekt** dialogrutan, namnet hello projekt **MyFirstJavaOnAzureWebApp**, och välj **Slutför**.
   
![Dialogrutan för nytt dynamiskt webbprojekt](./media/app-service-web-get-started-java/new-dynamic-web-project-dialog-box.png)

### <a name="add-a-jsp-page"></a>Lägg till en JSP-sida

Om projektutforskaren inte visas kan du återställa den.

![Java EE-arbetsytan för Eclipse](./media/app-service-web-get-started-java/pe.png)

Expandera hello i Projektutforskaren **MyFirstJavaOnAzureWebApp** projekt.
Högerklicka på **Webbinnehåll** och välj sedan **Ny** > **JSP-fil**.

![Menyn för en ny JSP-fil i projektutforskaren](./media/app-service-web-get-started-java/new-jsp-file-menu.png)

I hello **ny JSP-fil** dialogrutan:

* Namnet hello filen **index.jsp**.
* Välj **Slutför**.

  ![Dialogruta för en ny JSP-fil](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-1.png)

Ersätt hello i hello index.jsp-filen, `<body></body>` element med hello följande kod:

```jsp
<body>
<h1><% out.println("Hello Azure!"); %></h1>
</body>
```

Spara hello ändringar.

## <a name="publish-hello-web-app-tooazure"></a>Publicera hello web app tooAzure

Högerklicka på hello-projekt i Projektutforskaren, och välj sedan **Azure** > **Publicera som Azure Web App**.

![Publicera som Azure Web App (snabbmeny)](./media/app-service-web-get-started-java/publish-as-azure-web-app-context-menu.png)

I hello **Azure logga In** dialogrutan, Behåll hello **interaktiv** alternativet och välj sedan **logga in**.

Följ instruktionerna för hello-inloggning.

### <a name="deploy-web-app-dialog-box"></a>Dialogrutan Distribuera webbapp

När du har loggat in tooyour Azure-konto hello **distribuera webbprogram** dialogrutan visas.

Välj **Skapa**.

![Dialogrutan Distribuera webbapp](./media/app-service-web-get-started-java/deploy-web-app-dialog-box.png)

### <a name="create-app-service-dialog-box"></a>Dialogrutan Skapa App Service

Hej **skapa App Service** dialogruta med standardvärden. Hej nummer **170602185241** visas i följande hello bilden är olika i dialogrutan.

![Dialogrutan Skapa App Service](./media/app-service-web-get-started-java/cas1.png)

I hello **skapa App Service** dialogrutan:

* Behåll hello genererade namnet för hello webbprogrammet. Användarnamnet måste vara unikt inom Azure. hello namn är en del av hello URL-adressen för hello webbprogrammet. Exempel: om hello webbprogrammets namn är **MyJavaWebApp**, hello URL: en är *myjavawebapp.azurewebsites.net*.
* Behåll hello standardbehållaren för webbprogram.
* Välj en Azure-prenumeration.
* På hello **apptjänstplan** fliken:

  * **Skapa en ny**: Behåll hello standard, vilket är hello namnet på hello App Service-plan.
  * **Plats**: Välj **Europa, västra** eller en plats nära dig.
  * **Prisnivån**: Välj hello kostnadsfritt alternativ. Se [Priser för App Service](https://azure.microsoft.com/pricing/details/app-service/) för funktioner.

   ![Dialogrutan Skapa App Service](./media/app-service-web-get-started-java/create-app-service-dialog-box.png)

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

### <a name="resource-group-tab"></a>Flik för resursgrupp

Välj hello **resursgruppen** fliken. Behåll hello genereras standardvärdet för hello resursgrupp.

![Flik för resursgrupp](./media/app-service-web-get-started-java/create-app-service-resource-group.png)

[!INCLUDE [resource-group](../../includes/resource-group.md)]

Välj **Skapa**.

<!--
### hello JDK tab

Select hello **JDK** tab. Keep hello default, and then select **Create**.

![Create App Service plan](./media/app-service-web-get-started-java/create-app-service-specify-jdk.png)
-->

hello Azure Toolkit skapar hello webbprogram och visar en förloppsindikator för.

![Dialogruta med förloppsindikator för hur apptjänsten skapas](./media/app-service-web-get-started-java/create-app-service-progress-bar.png)

### <a name="deploy-web-app-dialog-box"></a>Dialogrutan Distribuera webbapp

I hello **distribuera webbprogram** dialogrutan **distribuera tooroot**. Om du har en apptjänst vid *wingtiptoys.azurewebsites.net* och du inte distribuera toohello rot, hello webbprogram med namnet **MyFirstJavaOnAzureWebApp** distribueras för *wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp*.

![Dialogrutan Distribuera webbapp](./media/app-service-web-get-started-java/deploy-web-app-to-root.png)

hello dialogrutan visar hello Azure och JDK web behållaren val.

Välj **distribuera** toopublish hello web app tooAzure.

När hello publiceringen är klar väljer du hello **publicerade** länken i hello **Azure-aktivitetsloggen** dialogrutan.

![Dialogrutan för Azure aktivitetsloggen](./media/app-service-web-get-started-java/aal.png)

Grattis! Din web app tooAzure har distribuerats. 

!["Hello Azure!" exempelwebbapp](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="update-hello-web-app"></a>Uppdatera hello webbprogrammet

Ändra hello JSP kod tooa olika exempelmeddelande.

```jsp
<body>
<h1><% out.println("Hello again Azure!"); %></h1>
</body>
```

Spara hello ändringar.

Högerklicka på hello-projekt i Projektutforskaren, och välj sedan **Azure** > **Publicera som Azure Web App**.

Hej **distribuera webbprogram** dialogrutan visas och visar hello app service som du skapade tidigare. 

> [!NOTE]
> Välj **distribuera tooroot** varje gång du publicerar.
>

Välj hello webbapp och markera **distribuera**, som publicerar hello ändringar.

När hello **Publishing** länk visas markerar du den toobrowse toohello webbapp och se hello ändringar.

## <a name="manage-hello-web-app"></a>Hantera hello-webbprogram

Gå toohello <a href="https://portal.azure.com" target="_blank">Azure-portalen</a> toosee hello webbapp som du skapade.

Välj hello vänstra menyn **resursgrupper**.

![Portalen navigering tooresource grupper](media/app-service-web-get-started-java/rg.png)

Välj hello resursgrupp. hello sidan visar hello-resurser som du skapade i den här snabbstarten.

![Resursgruppen myResourceGroup](media/app-service-web-get-started-java/rg2.png)

Välj hello webbprogram (**webapp 170602193915** i föregående bild hello).

Hej **översikt** visas. Den här sidan ger dig en överblick över hur hello appen fungerar. Här kan du utföra grundläggande hanteringsåtgärder som att bläddra, stoppa, starta, starta om och ta bort. hello flikar på hello vänster hello sidan visar hello olika konfigurationer som du kan öppna. 

![App Service-sidan på Azure Portal](media/app-service-web-get-started-java/web-app-blade.png)

[!INCLUDE [clean-up-section-portal-web-app](../../includes/clean-up-section-portal-web-app.md)]

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Mappa anpassad domän](app-service-web-tutorial-custom-domain.md)
