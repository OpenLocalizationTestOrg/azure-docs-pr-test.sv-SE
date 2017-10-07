---
title: "aaaUser autentisering för API Apps i Azure App Service | Microsoft Docs"
description: "Lär dig hur tooprotect en API-app i Azure App Service genom att tillåta åtkomst till endast tooauthenticated användare."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 3896760d-46ff-4b67-b98a-edd233f24758
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 06/30/2016
ms.author: alkarche
ms.openlocfilehash: 4702fc77fcfe736405e22b033c35d22ae3c5a300
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="user-authentication-for-api-apps-in-azure-app-service"></a>Användarautentisering för API Apps i Azure App Service
## <a name="overview"></a>Översikt
Den här artikeln visar hur tooprotect Azure API-app så som endast autentiserade användare kan anropa. hello förutsätter att du har läst hello [översikt över Azure App Service-autentisering](../app-service/app-service-authentication-overview.md).

Du får lära dig:

* Hur tooconfigure en autentiseringsprovider, med information om Azure Active Directory (AD Azure).
* Hur tooconsume en skyddad API-app med hjälp av hello [Active Directory Authentication Library (ADAL) för JavaScript](https://github.com/AzureAD/azure-activedirectory-library-for-js).

hello artikeln innehåller två avsnitt:

* Hej [hur tooconfigure användarautentisering i Azure App Service](#authconfig) förklaras i allmänna hur tooconfigure användarautentisering för API-app och gäller även tooall ramverk som stöds av Apptjänsten, inklusive .NET, Node.js och Java.
* Från och med hello [fortsätter hello .NET API Apps självstudier](#tutorialstart) avsnittet, hello artikel guider som du konfigurerar ett exempelprogram med en .NET säkerhetskopiera slutet och en AngularJS-klient så att den använder Azure Active Directory för användare autentisering. 

## <a id="authconfig"></a>Hur tooconfigure användarautentisering i Azure App Service
Det här avsnittet innehåller allmänna anvisningar som gäller tooany API-app. För steg specifika toohello tooDo lista .NET exempelprogrammet gå för[fortsätter hello .NET komma igång-Självstudier](#tutorialstart).

1. I hello [Azure-portalen](https://portal.azure.com/), navigera toohello **inställningar** bladet hello API-appen som du vill tooprotect, hitta hello **funktioner** avsnittet och klicka sedan på  **Autentisering / auktorisering**.
   
    ![Azure-portalen autentisering/auktorisering](./media/app-service-api-dotnet-user-principal-auth/features.png)
2. I hello **autentisering / auktorisering** bladet, klickar du på **på**.
3. Välj ett alternativ för hello hello **åtgärd tootake när begäran inte har autentiserats** listrutan.
   
   * Om du vill endast autentiserade anrop tooreach API-appen, Välj något av hello **logga in med...**  alternativ. Det här alternativet kan du tooprotect hello API-app utan att skriva någon kod som körs i den.
   * Om du vill att alla anrop tooreach API-appen, Välj **Tillåt begäran (ingen åtgärd)**. Du kan använda det här alternativet toodirect oautentiserad anropare tooa valet av autentiseringsproviders. Med det här alternativet har toowrite kod toohandle tillstånd.
     
     Mer information finns i [Autentisering och auktorisering för API Apps i Azure Apptjänst](app-service-api-authentication.md#multiple-protection-options).
4. Välj en eller flera hello **autentiseringsproviders**.
   
    hello bilden visar val som kräver att alla anropare toobe autentiseras av Azure AD.
   
    ![Azure portal autentisering/auktorisering, blad](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
   
    När du väljer en autentiseringsprovider visas hello ett blad i konfigurationen för providern. 
   
    Detaljerade instruktioner som förklarar hur tooconfigure hello authentication provider configuration blad finns [hur tooconfigure Apptjänst programmet toouse Azure Active Directory inloggningen](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). (hello länken går tooan artikel om Azure AD, men själva hello artikeln innehåller flikar som länkar tooarticles för hello andra autentiseringsproviders.)
5. När du är klar med hello authentication provider configuration bladet, klickar du på **OK**.
6. I hello **autentisering / auktorisering** bladet, klickar du på **spara**.

När det är klart autentiserar alla API-anrop i App Service innan de når hello API-app. hello autentisering fungerar hello samma för alla språk som stöds av apptjänst, inklusive .NET, Node.js och Java. 

toomake autentiserad API-anrop, hello anroparen innehåller hello authentication provider OAuth 2.0-ägartoken i hello Authorization-huvud av HTTP-begäranden. hello token kan erhållas med hjälp av hello authentication provider SDK.

## <a id="tutorialstart"></a>Fortsätter hello .NET API Apps självstudier
Om du följer hello Node.js eller Java självstudier för API apps, hoppar du över nästa artikel toohello [primära autentiseringen av tjänsten för API apps](app-service-api-dotnet-service-principal-auth.md). 

Om du följer hello serien med .NET-självstudiekurs för API apps och redan har distribuerat hello exempelprogrammet enligt anvisningarna i hello [första](app-service-api-dotnet-get-started.md) och [andra](app-service-api-cors-consume-javascript.md) självstudier, hoppa över toohello [ställa in autentisering i Apptjänst och Azure AD](#azureauth) avsnitt.

Om du vill toofollow självstudierna utan att gå via hello första och andra självstudiekurser hello följande som förklarar hur tooget startats med hjälp av en automatiserad process toodeploy hello exempelprogrammet.

> [!NOTE]
> hello följande steg hjälper dig toohello samma utgångspunkten som om du hello två första självstudier, med ett undantag: Visual Studio inte redan känner till vilka webbprogram eller API-app som varje projekt distribueras till. Det innebär att du inte har exakta instruktioner i den här självstudiekursen som förklarar hur toodeploy toohello rätt mål. Om du inte är van vid att räkna ut hur toodo hello distributionsstegen på egen hand, är det bättre toofollow hello självstudiekursen serie från hello [första självstudierna](app-service-api-dotnet-get-started.md) automatisk än toostart med den här processen för distribution.
> 
> 

1. Kontrollera att du har alla hello förutsättningar som anges i hello [första självstudierna](app-service-api-dotnet-get-started.md). I tillägg toohello kraven, antas dessa självstudiekurser för autentisering som du har arbetat med App Service web apps och API apps i Visual Studio och hello Azure-portalen.
2. Klicka på hello **distribuera tooAzure** knapp i hello [tooDo lista exempel databasen viktigt-fil](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/readme.md) toodeploy hello API apps och hello web app. Anteckna hello Azure-resursgrupp som skapas, som du kan använda den här senare toolook in webbappen och API-appnamn.
3. Ladda ned eller klona hello [tooDo lista exempel databasen](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) tooget hello kod som du ska arbeta med i Visual Studio lokalt.

## <a id="azureauth"></a>Konfigurera autentisering i Apptjänst och Azure AD
Nu har du hello-program som körs i Azure App Service utan att kräva att användarna autentiseras. I det här avsnittet du lägger till autentisering genom att göra hello följande uppgifter:

* Konfigurera App Service toorequire Azure Active Directory (Azure AD)-autentisering för att anropa hello mellannivå-API-app.
* Skapa ett Azure AD-program.
* Konfigurera hello Azure AD application toosend hello ägar-token efter inloggning toohello AngularJS-klient. 

Om du stöter på problem när följande hello självstudiekursen anvisningar Se hello [felsökning](#troubleshooting) avsnittet hello slutet av hello kursen. 

### <a name="configure-authentication-for-hello-middle-tier-api-app"></a>Konfigurera autentisering för hello mellannivå-API-app
1. I hello [Azure-portalen](https://portal.azure.com/), navigera toohello **inställningar** bladet för hello API-app som du skapade för ToDoListAPI-projektet hello hitta hello **funktioner** avsnitt, och sedan Klicka på **autentisering / auktorisering**.
   
    ![Azure-portalen autentisering/auktorisering](./media/app-service-api-dotnet-user-principal-auth/features.png)
2. I hello **autentisering / auktorisering** bladet, klickar du på **på**.
3. I hello **åtgärd tootake när begäran inte har autentiserats** listrutan, Välj **logga in med Azure Active Directory**.
   
    Det här alternativet innebär att inga unauathenticated begäranden ska nå hello API-app. 
4. Under **autentiseringsproviders**, klickar du på **Azure Active Directory**.
   
    ![Azure portal autentisering/auktorisering, blad](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
5. I hello **inställningarna för Azure Active Directory** bladet, klickar du på **Express**
   
    ![Azure portal autentisering/auktorisering Express alternativet](./media/app-service-api-dotnet-user-principal-auth/aadsettings.png)
   
    Med hello **Express** alternativet Apptjänst kan automatiskt skapa ett Azure AD-program i din Azure AD [klient](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 
   
    Du har inte toocreate en klient, eftersom varje Azure-konto har en automatiskt.
6. Under **hanteringsläge**, klickar du på **Skapa nytt AD-App** om den inte redan är markerat, och notera hello värdet i hello **skapa App** textrutan; du måste slå upp den här AAD program i hello klassiska Azure-portalen senare.
   
    ![Azure-portalen Azure AD-inställningar](./media/app-service-api-dotnet-user-principal-auth/aadsettings2.png)
   
    Azure skapar automatiskt en Azure AD-program med hello angivet namn i Azure AD-klienten. Som standard hello Azure AD-program med namnet hello samma som hello API-app. Om du vill kan ange du ett annat namn.
7. Klicka på **OK**.
8. I hello **autentisering / auktorisering** bladet, klickar du på **spara**.
   
    ![Klicka på Spara](./media/app-service-api-dotnet-user-principal-auth/authsave.png)

Endast användare i Azure AD-klienten kan nu anropa hello API-app.

### <a name="optional-test-hello-api-app"></a>Valfritt: Testa hello API-app
1. I en webbläsare går toohello Webbadressen till hello API-app: i hello **API-app** bladet i hello Azure-portalen klickar du på länken hello under **URL**.  
   
    Du är omdirigerade tooa inloggningsskärm eftersom oautentiserade begäranden inte tillåts tooreach hello API-app.
   
    Om din webbläsare går toohello ”har skapats”-sida kan hello webbläsaren kanske redan har loggat på--i så fall öppna en InPrivate- eller Incognito-fönster och gå toohello API app-URL.
2. Logga in med autentiseringsuppgifterna för en användare i din Azure AD-klient.
   
    När du är inloggad, visas hello ”har skapats” i hello webbläsare.
3. Stäng hello webbläsare.

### <a name="configure-hello-azure-ad-application"></a>Konfigurera hello Azure AD-program
När du har konfigurerat Azure AD authentication skapats Apptjänst ett Azure AD-program. Som standard var hello nya Azure AD-program konfigurerade tooprovide hello ägar-token toohello API-appens URL. I det här avsnittet kan du konfigurera hello Azure AD programmet tooprovide hello ägar-token toohello AngularJS klientdelen i stället för direkt toohello mellannivå-API-app. Hej AngularJS-klient skickar hello token toohello API-app när den anropar hello API-app.

> [!NOTE]
> den här kursen används ett enda Azure AD tookeep hello processen så enkel som möjligt, för både hello klientdelen och hello mellannivå-API-app. Ett annat alternativ är toouse två Azure AD-program. Du måste i så fall toogive hello klientdelens Azure AD application behörighet toocall hello Mellannivås Azure AD-program. Den här metoden för flera program ger mer detaljerad kontroll över behörigheter tooeach nivå.
> 
> 

1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com/), gå för**Azure Active Directory**.
   
   Du har toouse hello klassiska portalen eftersom vissa Azure Active Directory-inställningar som du behöver komma åt tooare ännu inte är tillgänglig i hello aktuella Azure-portalen.
2. På hello **Directory** klickar du på AAD-klient.
   
   ![Azure AD i klassiska portalen](./media/app-service-api-dotnet-user-principal-auth/selecttenant.png)
3. Klicka på **program > program som företaget äger**, och klicka sedan på hello är markerat.
   
   Du kan också ha toorefresh hello sidan toosee hello nytt program.
4. Klicka på hello som Azure skapade när du har aktiverat autentisering för API-appen hello namn i hello listan med program.
   
   ![Fliken för Azure AD-program](./media/app-service-api-dotnet-user-principal-auth/appstab.png)
5. Klicka på **Konfigurera**.
   
   ![Azure AD-konfigurera fliken](./media/app-service-api-dotnet-user-principal-auth/configure.png)
6. Ange **inloggnings-URL** toohello URL: en för webbappen AngularJS, utan avslutande snedstreck.
   
   Till exempel: https://todolistangular.azurewebsites.net
   
   ![Inloggnings-URL](./media/app-service-api-dotnet-user-principal-auth/signonurlazure.png)
7. Ange **Reply URL** toohello URL: en för ditt webbprogram, utan avslutande snedstreck.
   
   Till exempel: https://todolistsangular.azurewebsites.net
8. Klicka på **Spara**.
   
   ![Reply-URL](./media/app-service-api-dotnet-user-principal-auth/replyurlazure.png)
9. Hello längst hello-sidan, klickar du på **hantera manifestet > hämta manifestet**.
   
   ![Hämta manifestet](./media/app-service-api-dotnet-user-principal-auth/downloadmanifest.png)
10. Hämta hello tooa plats där du kan redigera den.
11. I hello hämtas manifestfilen, söker du efter hello `oauth2AllowImplicitFlow` egenskapen. Ändra hello värdet på denna egenskap från `false` för`true`, och sedan spara hello-filen.
    
    Den här inställningen krävs för åtkomst från en enda sida JavaScript-programmet. Den gör hello Oauth 2.0 ägar-token toobe returneras i hello URL-fragment.
12. Klicka på **hantera manifestet > Överför manifestet**, och överför hello-fil som du har uppdaterat i hello föregående steg.
    
    ![Överför manifest](./media/app-service-api-dotnet-user-principal-auth/uploadmanifest.png)
13. Kopiera hello **klient-ID** värde och spara den någonstans du hämtar det senare.

## <a name="configure-hello-todolistangular-project-toouse-authentication"></a>Konfigurera hello ToDoListAngular-projektet toouse autentisering
I det här avsnittet Ändra hello AngularJS klientdelen så att den använder Active Directory Authentication Library (ADAL) för JS tooacquire ett ägartoken för hello inloggade användare från Azure AD. hello-koden innehåller hello token i HTTP-begäranden skickas toohello mellannivå, som visas i följande diagram hello. 

![Diagram för autentisering av användare](./media/app-service-api-dotnet-user-principal-auth/appdiagram.png)

Gör följande ändringar toofiles i hello ToDoListAngular-projektet hello.

1. Öppna hello *index.cshtml* fil.
2. Ta bort kommentarerna hello rader som refererar till hello Active Directory Authentication Library (ADAL) för JS-skript.
   
        <script src="app/scripts/adal.js"></script>
        <script src="app/scripts/adal-angular.js"></script>
3. Öppna hello *app/scripts/app.js* fil.
4. Kommentera hello kodblocket markerats för ”utan autentisering” och Avkommentera hello kodblocket markerats för ”med autentisering”.
   
    Den här ändringen refererar till hello ADAL JS autentiseringsprovider och ger configuration värden tooit. Du kan ange hello konfigurationsvärden för API-appen och Azure AD-program i hello följande steg.
5. I hello-kod som anger hello `endpoints` hello variabeln, ange URL för API-toohello URL för hello API-app som du skapade för hello ToDoListAPI-projektet och ange hello Azure AD-program-ID toohello klient-ID som du kopierade från hello klassiska Azure-portalen.
   
    hello koden är nu liknande toohello följande exempel.
   
        var endpoints = {
            "https://todolistapi0121.azurewebsites.net/": "1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3"
        };
6. Hello anropa för`adalProvider.init`, ange `tenant` tooyour klientnamn och `clientId` toosame-värde som du använde i hello föregående steg.
   
    hello koden är nu liknande toohello följande exempel.
   
        adalProvider.init(
            {
                instance: 'https://login.microsoftonline.com/', 
                tenant: 'contoso.onmicrosoft.com',
                clientId: '1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3',
                extraQueryParameter: 'nux=1',
                endpoints: endpoints
            },
            $httpProvider
            );
   
    Dessa ändringar för`app.js` anger att hello anropande koden och hello kallas API i hello samma Azure AD-program.
7. Öppna hello *app/scripts/homeCtrl.js* fil.
8. Kommentera hello kodblocket markerats för ”utan autentisering” och Avkommentera hello kodblocket markerats för ”med autentisering”.
9. Öppna hello *app/scripts/indexCtrl.js* fil.
10. Kommentera hello kodblocket markerats för ”utan autentisering” och Avkommentera hello kodblocket markerats för ”med autentisering”.

### <a name="deploy-hello-todolistangular-project-tooazure"></a>Distribuera hello ToDoListAngular-projektet tooAzure
1. I **Solution Explorer**, högerklicka på hello ToDoListAngular-projektet och klicka sedan på **publicera**.
2. Klicka på **Publicera**.
   
    Visual Studio distribuerar hello-projektet och öppnar en webbläsare toohello webbprogram bas-URL. Det här alternativet visas sidan 403-fel, vilket är normalt för ett försök toogo tooa Web API bas-URL från en webbläsare.
   
    Du har fortfarande toomake en ändring toohello mellannivå-API-app innan du kan testa programmet hello.
3. Stäng hello webbläsare.

## <a name="configure-hello-todolistapi-project-toouse-authentication"></a>Konfigurera hello ToDoListAPI-projektet toouse autentisering
För närvarande hello ToDoListAPI-projektet skickar ”*” som hello `owner` värdet tooToDoListDataAPI. I det här avsnittet kan du ändra hello kod toosend hello-ID för hello inloggade användare.

Gör följande ändringar i hello ToDoListAPI-projektet hello.

1. Öppna hello *Controllers/ToDoListController.cs* filen och Avkommentera hello rad i varje åtgärdsmetod som anger `owner` toohello Azure AD `NameIdentifier` anspråksvärde. Exempel:
   
        owner = ((ClaimsIdentity)User.Identity).FindFirst(ClaimTypes.NameIdentifier).Value;
   
    **Viktiga**: inte ta bort kommentarerna koden i hello `ToDoListDataAPI` metoden; gör du det senare hello service principal autentisering genomgång.

### <a name="deploy-hello-todolistapi-project-tooazure"></a>Distribuera hello ToDoListAPI-projektet tooAzure
1. I **Solution Explorer**, högerklicka på hello ToDoListAPI-projektet och klicka sedan på **publicera**.
2. Klicka på **Publicera**.
   
    Visual Studio distribuerar hello-projektet och öppnar en webbläsare toohello API-appens bas-URL.
3. Stäng hello webbläsare.

### <a name="test-hello-application"></a>Testa programmet hello
1. Gå toohello URL för hello webbapp **med hjälp av HTTPS, inte HTTP**.
2. Klicka på hello **tooDo lista** fliken.
   
    Du kan ange toolog i.
3. Logga in med hello autentiseringsuppgifterna för en användare i din AAD-klient.
4. Hej **tooDo listan** visas.
   
   ![sidan tooDo](./media/app-service-api-dotnet-user-principal-auth/webappindex.png)
   
   Inga arbetsuppgifter visas eftersom fram till nu de alla har ägaren av ”*”. Nu hello mellannivå begär objekt för hello inloggad användare och ingen har skapats ännu.
5. Lägga till nya att göra objekten tooverify som programmet hello fungerar.
6. Gå toohello Swagger UI-URL för hello ToDoListDataAPI API-app i ett nytt webbläsarfönster och klicka på **ToDoList > hämta**. Ange en asterisk för hello `owner` parameter och klicka sedan på **prova**.
   
   hello svaret visar att hello nya arbetsuppgifter har hello faktiska Azure AD användar-ID i hello ägare egenskap.
   
   ![Ägar-ID i JSON-svar](./media/app-service-api-dotnet-user-principal-auth/todolistapiauth.png)

## <a name="building-hello-projects-from-scratch"></a>Skapa hello projekt från grunden
hello två Web API-projekt har skapats med hjälp av hello **Azure API Apps** mall och ersätta hello standard värden controller med en ToDoList-styrenhet. 

För information om hur skapar för ett program för AngularJS-sida med en serverdel för Web API 2, se [händerna på Lab: skapa en enkel sida program (SPA) med ASP.NET Web API och Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Information om hur tooadd koden för Azure AD finns hello följande resurser:

* [Skydda AngularJS enda sidan appar med Azure AD](../active-directory/active-directory-devquickstarts-angular.md).
* [Introduktion till ADAL JS v1](http://www.cloudidentity.com/blog/2015/02/19/introducing-adal-js-v1/)

## <a name="troubleshooting"></a>Felsökning
[!INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Kontrollera att du blanda inte ihop ToDoListAPI (mellannivå) och ToDoListDataAPI (datanivå). Till exempel kontrollera att du lagt till autentisering toohello mellannivå-API-app, inte hello datanivå. 
* Kontrollera att hello AngularJS källa kod referenser hello mellannivå API app-URL (ToDoListAPI, inte ToDoListDataAPI) och hello korrekt Azure AD-klient-ID. 

## <a name="next-steps"></a>Nästa steg
I den här självstudiekursen du lärt dig hur toouse App Service-autentisering för en API-app och hur toocall hello API-app med hjälp av hello JS ADAL-biblioteket. I nästa kurs i hello lär du dig hur för[säker åtkomst tooyour API-app för service to service scenarier](app-service-api-dotnet-service-principal-auth.md).

