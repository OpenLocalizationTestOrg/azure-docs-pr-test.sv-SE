---
title: aaaCreate en ASP.NET-webbapp i Azure | Microsoft Docs
description: "Lär dig hur toorun web apps i Azure App Service genom att distribuera hello standard ASP.NET web app."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 06/14/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: eec916b3c32b6c8b68083177938c5c822a9782b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-aspnet-web-app-in-azure"></a>Skapa en ASP.NET-webbapp i Azure

Med [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) får du en mycket skalbar och automatiskt uppdaterad webbvärdtjänst.  Den här snabbstarten visar hur toodeploy din första ASP.NET web app tooAzure Web Apps. När du är klar har du en resursgrupp som består av en App Service-plan och en Azure-webbapp med en distribuerad webbapp.

Titta hello video toosee denna Snabbstart i praktiken och följ sedan hello steg själv toopublish din första .NET-app på Azure.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-for-NET-Developers/Create-a-NET-app-in-Azure-Quickstart/player]

## <a name="prerequisites"></a>Krav

toocomplete den här kursen:

* Installera [Visual Studio 2017](https://www.visualstudio.com/downloads/) med hello följande arbetsbelastningar:
    - **ASP.NET och webbutveckling**
    - **Azure Development**

    ![ASP.NET och webbutveckling och Azure Development (under webb och moln)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-aspnet-web-app"></a>Skapa en ASP.NET-webbapp

Skapa ett nytt projekt i Visual Studio genom att välja **Arkiv > Nytt > Projekt**. 

I hello **nytt projekt** markerar **Visual C# > webb > ASP.NET-webbprogram (.NET Framework)**.

Namnge programmet hello _myFirstAzureWebApp_, och välj sedan **OK**.
   
![Dialogrutan Nytt projekt](./media/app-service-web-get-started-dotnet/new-project.png)

Du kan distribuera någon typ av ASP.NET web app tooAzure. För Snabbstart, väljer du hello **MVC** mall, och kontrollera att autentisering har angetts för**ingen autentisering**.
      
Välj **OK**.

![Dialogrutan Nytt ASP.NET-projekt](./media/app-service-web-get-started-dotnet/select-mvc-template.png)

Hello-menyn, Välj **Felsök > Starta utan Debugging** toorun hello webbappen lokalt.

![Kör appen lokalt](./media/app-service-web-get-started-dotnet/local-web-app.png)

## <a name="publish-tooazure"></a>Publicera tooAzure

I hello **Solution Explorer**, högerklicka på hello **myFirstAzureWebApp** projektet och välj **publicera**.

![Publicera från Solution Explorer](./media/app-service-web-get-started-dotnet/solution-explorer-publish.png)

Se till att **Microsoft Azure App Service** är markerat och välj **Publicera**.

![Publicera från projektöversiktssidan](./media/app-service-web-get-started-dotnet/publish-to-app-service.png)

Då öppnas hello **skapa App Service** dialog, som hjälper dig att skapa alla hello Azure resurser toorun hello ASP.NET-webbapp i Azure.

## <a name="sign-in-tooazure"></a>Logga in tooAzure

I hello **skapa Apptjänst** markerar **Lägg till ett konto**, och logga in tooyour Azure-prenumeration. Om du redan är inloggad önskade väljer hello-konto som innehåller hello prenumeration hello listrutan.

> [!NOTE]
> Välj inte **Skapa** ännu om du redan är inloggad.
>
>
   
![Logga in tooAzure](./media/app-service-web-get-started-dotnet/sign-in-azure.png)

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

[!INCLUDE [resource group intro text](../../includes/resource-group.md)]

Nästa för**resursgruppen**väljer **ny**.

Namnet hello resursgruppen **myResourceGroup** och välj **OK**.

## <a name="create-an-app-service-plan"></a>Skapa en App Service-plan

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Nästa för**Apptjänstplan**väljer **ny**. 

I hello **konfigurera Apptjänstplan** dialogrutan Inställningar för hello i hello tabell efter hello skärmbild.

![Skapa apptjänstplan](./media/app-service-web-get-started-dotnet/configure-app-service-plan.png)

| Inställning | Föreslaget värde | Beskrivning |
|-|-|-|
|App Service-plan| myAppServicePlan | Namnet på hello App Service-plan. |
| Plats | Västra Europa | hello datacenter där hello webbprogram finns. |
| Storlek | Kostnadsfri | [Prisnivån](https://azure.microsoft.com/pricing/details/app-service/) avgör tillgängliga värdfunktioner. |

Välj **OK**.

## <a name="create-and-publish-hello-web-app"></a>Skapa och publicera hello webbapp

I **Webbprogramnamnet**, Skriv ett unikt appnamn (giltiga tecken är `a-z`, `0-9`, och `-`), eller Godkänn hello genereras automatiskt unikt namn. hello webbadressen hello webbprogrammet `http://<app_name>.azurewebsites.net`, där `<app_name>` är din webbprogrammets namn.

Välj **skapa** toostart skapar hello Azure-resurser.

![Ange webbappnamn](./media/app-service-web-get-started-dotnet/web-app-name.png)

När hello guiden slutförs kommer den publicerar hello ASP.NET web app tooAzure och sedan startar hello app i hello standardwebbläsare.

![Publicerad ASP.NET-webbapp i Azure](./media/app-service-web-get-started-dotnet/published-azure-web-app.png)

hello webbprogrammets namn anges i hello [skapa och publicera steg](#create-and-publish-the-web-app) används som hello URL-prefixet i hello format `http://<app_name>.azurewebsites.net`.

Grattis, din ASP.NET-webbapp körs live i Azure App Service.

## <a name="update-hello-app-and-redeploy"></a>Uppdatera hello app och distribuera om

Från hello **Solution Explorer**öppnar _Views\Home\Index.cshtml_.

Hitta hello `<div class="jumbotron">` HTML-tagg hello övre delen och Ersätt hello hela elementet med hello följande kod:

```HTML
<div class="jumbotron">
    <h1>ASP.NET in Azure!</h1>
    <p class="lead">This is a simple app that we’ve built that demonstrates how toodeploy a .NET app tooAzure App Service.</p>
</div>
```

tooredeploy tooAzure, högerklicka på hello **myFirstAzureWebApp** projektet i **Solution Explorer** och välj **publicera**.

I hello publicera sidan, Välj **publicera**.

När publiceringen är klar startar webbläsaren toohello URL: en hello webbprogram i Visual Studio.

![Uppdaterad ASP.NET-webbapp i Azure](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

## <a name="manage-hello-azure-web-app"></a>Hantera hello Azure-webbapp

Gå toohello <a href="https://portal.azure.com" target="_blank">Azure-portalen</a> toomanage hello-webbprogram.

Välj hello vänstra menyn **Apptjänster**, och välj sedan hello namnet på din Azure webbapp.

![Portalen navigering tooAzure webbprogram](./media/app-service-web-get-started-dotnet/access-portal.png)

Nu visas sidan Översikt för din webbapp. Här kan du utföra grundläggande hanteringsåtgärder som att bläddra, stoppa, starta, starta om och ta bort. 

![App Service-blad på Azure Portal](./media/app-service-web-get-started-dotnet/web-app-blade.png)

hello vänstra menyn innehåller olika sidor för att konfigurera din app. 

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [ASP.NET med SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md)
