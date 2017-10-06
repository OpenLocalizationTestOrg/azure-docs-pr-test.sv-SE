---
title: "aaaService primär autentisering för API Apps i Azure App Service | Microsoft Docs"
description: "Lär dig hur tooprotect en API-app i Azure App Service för scenarier med tjänst-till-tjänst."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 7ca0bab2-1d29-4d51-b779-dce0edd34f8b
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 06/30/2016
ms.author: alkarche
ms.openlocfilehash: 94d9ee11f38293df4a2fd815ef02c59cc6defed4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-principal-authentication-for-api-apps-in-azure-app-service"></a>Tjänstens huvudnamn autentisering för API Apps i Azure App Service
## <a name="overview"></a>Översikt
Den här artikeln förklarar hur toouse Apptjänst autentisering för *interna* åt tooAPI appar. Ett internt scenario är där du har en API-app som du vill toobe kan användas endast av egna programkod. hello rekommenderat sätt tooimplement det här scenariot i App Service toouse Azure AD tooprotect hello kallas API-app. Du kan anropa hello skyddade API-app med en ägartoken som du får från Azure AD genom att tillhandahålla Programidentitet (tjänstens huvudnamn) autentiseringsuppgifter. Alternativ toousing Azure AD finns hello **tjänst-till-tjänst autentisering** avsnitt i hello [översikt över Azure App Service-autentisering](../app-service/app-service-authentication-overview.md#service-to-service-authentication).

I den här artikeln lär du dig:

* Hur toouse Azure Active Directory (AD Azure) tooprotect en API-app från oautentiserad åtkomst.
* Hur tooconsume skyddade API-app från en API-app, webbapp eller mobilapp med hjälp av autentiseringsuppgifter för Azure AD service principal (app identitet). Information om hur tooconsume från en logikapp finns [använda anpassat API som finns på Apptjänst med Logic apps](../logic-apps/logic-apps-custom-hosted-api.md).
* Hur toomake till att hello skyddade API-appen kan inte anropas från en webbläsare med inloggade användare.
* Hur toomake till att hello skyddade API-app kan endast anropas med en specifik Azure AD tjänstens huvudnamn.

hello artikeln innehåller två avsnitt:

* Hej [hur tooconfigure service principal autentisering i Azure App Service](#authconfig) förklaras i allmänna hur tooconfigure autentisering för API-app och hur tooconsume hello skyddade API-app. Det här avsnittet gäller även tooall ramverk som stöds av Apptjänsten, inklusive .NET, Node.js och Java.
* Från och med hello [fortsätter hello .NET komma igång-Självstudier](#tutorialstart) avsnittet hello kursen hjälper dig att konfigurera ett ”intern åtkomst” scenario för en .NET-exempelprogram som körs i Apptjänst. 

## <a id="authconfig"></a>Hur tooconfigure service principal autentisering i Azure App Service
Det här avsnittet innehåller allmänna anvisningar som gäller tooany API-app. För steg specifika toohello tooDo lista .NET exempelprogrammet gå för[fortsätter hello .NET API Apps självstudiekursen serien](#tutorialstart).

1. I hello [Azure-portalen](https://portal.azure.com/), navigera toohello **inställningar** bladet hello API-appen som du vill tooprotect och hitta hello **funktioner** avsnittet och klicka på **Autentisering / auktorisering**.
   
    ![Autentisering/auktorisering i Azure-portalen](./media/app-service-api-dotnet-user-principal-auth/features.png)
2. I hello **autentisering / auktorisering** bladet, klickar du på **på**.
3. I hello **åtgärd tootake när begäran inte har autentiserats** listrutan, Välj **logga in med Azure Active Directory** .
4. Under **autentiseringsproviders**väljer **Azure Active Directory**.
   
    ![Autentisering/auktorisering i bladet i Azure-portalen](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
5. Konfigurera hello **inställningarna för Azure Active Directory** bladet toocreate en ny Azure AD-program eller Använd ett befintligt Azure AD-program om du redan har en som du vill toouse.
   
    Internt scenarier innebär vanligen en API-app som anropar en API-app. Du kan använda separata Azure AD-program för varje API-app eller en Azure AD-program.
   
    Detaljerad information om det här bladet finns [hur tooconfigure Apptjänst programmet toouse Azure Active Directory inloggningen](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).
6. När du är klar med hello authentication provider configuration bladet, klickar du på **OK**.
7. I hello **autentisering / auktorisering** bladet, klickar du på **spara**.
   
    ![Klicka på Spara](./media/app-service-api-dotnet-service-principal-auth/authsave.png)

När det är klart gör Apptjänst endast begäranden från anropare i hello konfigurerat Azure AD-klient. Ingen autentisering eller auktorisering kod krävs i hello skyddade API-app. hello ägartoken skickas toohello API-app tillsammans med vanliga anspråk i HTTP-rubriker, och du kan läsa informationen i koden toovalidate som begäranden från en viss anropare, till exempel ett huvudnamn för tjänsten.

Det här autentisering fungerar hello samma sätt för alla språk som apptjänst, inklusive .NET, Node.js och Java. 

#### <a name="how-tooconsume-hello-protected-api-app"></a>Hur tooconsume hello skyddade API-app
hello anroparen måste ange en Azure AD ägar-token med API-anrop. tooget ägartoken med hjälp av service principal autentiseringsuppgifter, hello anroparen använder Active Directory Authentication Library (ADAL för [.NET](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory), [Node.js](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs), eller [Java](https://github.com/AzureAD/azure-activedirectory-library-for-java)). tooget en token hello-kod som anropar ADAL ger tooADAL hello följande information:

* hello namnet på din Azure AD-klient.
* hello-ID och klienten klienthemlighet (app-nyckel) av hello Azure AD-app som är associerade med hello anroparen.
* hello klient-ID för hello Azure AD-program som är associerade med hello skyddade API-app. (Om en Azure AD-program används det här är hello samma klient-ID som hello en för hello anroparen.)

Dessa värden är tillgängliga i hello Azure AD-sidor i hello [klassiska Azure-portalen](https://manage.windowsazure.com/).

När hello token har erhållits inkluderar hello anroparen den med HTTP-förfrågningar i hello Authorization-huvud.  Apptjänst validerar hello token och tillåter hello begäranden tooreach hello skyddade API-app.

#### <a name="how-tooprotect-hello-api-app-from-access-by-users-in-hello-same-tenant"></a>Hur tooprotect hello API-app från åtkomst av användare i hello samma klient
Ägar-token för användare i samma klient betraktas som giltigt för hello hello skyddade API-app.  Om du vill tooensure som bara en tjänst som kan anropa hello skyddade API-app, Lägg till kod i hello skyddade API app toovalidate hello följande anspråk från token hello:

* `appid`bör vara hello klient-ID för hello Azure AD-program som är associerad med hello anroparen. 
* `oid`(`objectidentifier`) ska vara hello service ägar-ID för hello anroparen. 

Apptjänst innehåller också hello `objectidentifier` anspråk i hello X-MS-CLIENT-SÄKERHETSOBJEKT-ID-huvudet.

### <a name="how-tooprotect-hello-api-app-from-browser-access"></a>Hur tooprotect hello API-app från webbläsaråtkomst
Om du inte verifiera anspråk i koden i hello skyddade API-app och om du använder en separat Azure AD-program för hello skyddade API-app, se till att hello Azure AD-program svars-URL: en är inte hello samma som hello bas-URL för API-app. Om hello Reply URL pekar direkt toohello skyddade API-app, en användare i hello samma Azure AD-klient Bläddra toohello API-app, logga in och anropa hello API.

## <a id="tutorialstart"></a>Fortsätter hello .NET API Apps självstudiekursen serien
Om du följer hello Node.js eller Java självstudiekursen serier för API apps, hoppar du över toohello [nästa steg](#next-steps) avsnitt. 

hello resten av den här artikeln fortsätter hello .NET API Apps självstudiekursen serien och förutsätter att du har slutfört hello [användaren autentisering kursen](app-service-api-dotnet-user-principal-auth.md) och ha hello exempelprogram som körs i Azure med autentisering av användare aktiverad.

## <a name="set-up-authentication-in-azure"></a>Konfigurera autentisering i Azure
I det här avsnittet konfigurerar du Apptjänst så att endast HTTP-begäranden kan tooreach hello API-appen på datanivå hello är hello de som har en giltig Azure AD-ägar-token. 

I följande avsnitt hello, konfigurerar hello mellannivå-API-app toosend programmet autentiseringsuppgifter tooAzure AD, få tillbaka en ägartoken och skicka hello ägar-token toohello appen på datanivå-API. Den här processen illustreras i hello diagram.

![Diagram för autentisering av tjänsten](./media/app-service-api-dotnet-service-principal-auth/appdiagram.png)

Om du stöter på problem när följande hello självstudiekursen anvisningar Se hello [felsökning](#troubleshooting) avsnittet hello slutet av hello kursen. 

1. I hello [Azure-portalen](https://portal.azure.com/), navigera toohello **inställningar** bladet hello API-appen som du skapade för hello ToDoListDataAPI (datanivå) API-app och klicka sedan på **inställningar**.
2. I hello **inställningar** bladet, hitta hello **funktioner** avsnittet och klicka sedan på **autentisering / auktorisering**.
   
    ![Autentisering/auktorisering i Azure-portalen](./media/app-service-api-dotnet-user-principal-auth/features.png)
3. I hello **autentisering / auktorisering** bladet, klickar du på **på**.
4. I hello **åtgärd tootake när begäran inte har autentiserats** listrutan, Välj **logga in med Azure Active Directory**.
   
    Detta är hello-inställningen som orsakar Apptjänst tooensure som endast autentiserade begäranden reach hello API-app. App Service för begäranden som har giltigt ägar-token, skickar hello token längs toohello API-app och fyller HTTP-huvuden med vanliga anspråk toomake informationen enklare tillgängliga tooyour kod.
5. Under **autentiseringsproviders**, klickar du på **Azure Active Directory**.
   
    ![Autentisering/auktorisering i bladet i Azure-portalen](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
6. I hello **inställningarna för Azure Active Directory** bladet, klickar du på **Express**.
   
    Med hello **Express** alternativet Azure kan automatiskt skapa ett AAD-program i din Azure AD [klient](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 
   
    Du har inte toocreate en klient, eftersom varje Azure-konto har en automatiskt.
7. Under **hanteringsläge**, klickar du på **Skapa nytt AD-App** om den inte redan har markerats.
   
    hello portal ansluts hello **skapa App** textrutan med ett standardvärde. Som standard hello Azure AD-program med namnet hello samma som hello API-app. Om du vill kan ange du ett annat namn.
   
    ![Azure AD-inställningar](./media/app-service-api-dotnet-service-principal-auth/aadsettings.png)
   
    **Obs**: som ett alternativ kan du använda en enda Azure AD program för både hello anropa API-app och hello skyddade API-app. Om du väljer detta alternativ kan du inte behöver hello **Skapa nytt AD-App** alternativet här eftersom du redan har skapat ett Azure AD-program tidigare i hello användaren autentisering kursen. Den här kursen ska du använda separata Azure AD-program för hello anropa API-app och hello skyddade API-app.
8. Anteckna hello värdet i hello **skapa App** textrutan; du måste slå upp AAD-program i hello klassiska Azure-portalen senare.
9. Klicka på **OK**.
10. I hello **autentisering / auktorisering** bladet, klickar du på **spara**.
    
    ![Klicka på Spara](./media/app-service-api-dotnet-service-principal-auth/saveauth.png)
    
    App Service skapar ett Azure Active Directory-program med **inloggnings-URL** och **Reply URL** automatiskt in toohello URL för API-appen. hello senare värdet kan användare i din AAD-klient toolog i och åtkomst hello API-app.

### <a name="verify-that-hello-api-app-is-protected"></a>Kontrollera att hello API-app är skyddat
1. I en webbläsare går toohello Webbadressen till hello API-app: i hello **API-app** bladet i hello Azure-portalen klickar du på länken hello under **URL**. 
   
    Du är omdirigerade tooa inloggningsskärm eftersom oautentiserade begäranden inte tillåts tooreach hello API-app. 
   
    Om din webbläsare gå toohello Swagger-användargränssnitt, webbläsaren kanske redan har loggat in – i så fall öppna en InPrivate- eller Incognito fönster och gå toohello Swagger UI-URL.
2. Logga in med autentiseringsuppgifterna för en användare i din AAD-klient.
   
   När du är inloggad, visas hello ”har skapats” i hello webbläsare.

## <a name="configure-hello-todolistapi-project-tooacquire-and-send-hello-azure-ad-token"></a>Konfigurera hello ToDoListAPI-projektet tooacquire och skicka hello Azure AD-token
I det här avsnittet du hello följande uppgifter:

* Lägg till kod i hello mellannivå-API-app som använder Azure AD application autentiseringsuppgifter tooacquire en token och skickar den med HTTP-begäranden toohello appen på datanivå-API.
* Hämta hello-autentiseringsuppgifter som du behöver från Azure AD.
* Ange hello autentiseringsuppgifter i Azure App Service runtime miljöinställningar i hello mellannivå-API-app. 

### <a name="configure-hello-todolistapi-project-tooacquire-and-send-hello-azure-ad-token"></a>Konfigurera hello ToDoListAPI-projektet tooacquire och skicka hello Azure AD-token
Gör följande ändringar i hello ToDoListAPI-projektet i Visual Studio hello.

1. Ta bort kommentarerna all hello kod i hello *ServicePrincipal.cs* fil.
   
    Detta är hello-kod som använder ADAL för .NET tooacquire hello Azure AD-ägartoken.  Den använder flera konfigurationsvärden som du ska ange i hello Azure körningsmiljö senare. Här är hello kod: 
   
        public static class ServicePrincipal
        {
            static string authority = ConfigurationManager.AppSettings["ida:Authority"];
            static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
            static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
            static string resource = ConfigurationManager.AppSettings["ida:Resource"];
   
            public static AuthenticationResult GetS2SAccessTokenForProdMSA()
            {
                return GetS2SAccessToken(authority, resource, clientId, clientSecret);
            }
   
            static AuthenticationResult GetS2SAccessToken(string authority, string resource, string clientId, string clientSecret)
            {
                var clientCredential = new ClientCredential(clientId, clientSecret);
                AuthenticationContext context = new AuthenticationContext(authority, false);
                AuthenticationResult authenticationResult = context.AcquireToken(
                    resource,
                    clientCredential);
                return authenticationResult;
            }
        }
   
    **Obs:** koden kräver hello ADAL för .NET-NuGet-paketet (Microsoft.IdentityModel.Clients.ActiveDirectory) som redan är installerad i hello-projekt. Om du skapar det här projektet från början, har du tooinstall det här paketet. Det här paketet installeras inte automatiskt av hello API app nytt projekt mallen.
2. I *domänkontrollanter/ToDoListController*, ta bort kommentarerna hello koden i hello `NewDataAPIClient` metod som lägger till hello token tooHTTP begäranden i hello authorization-huvud.
   
        client.HttpClient.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", ServicePrincipal.GetS2SAccessTokenForProdMSA().AccessToken);
3. Distribuera hello ToDoListAPI-projektet. (Högerklicka på hello-projektet och klicka sedan på **publicera > Publicera**.)
   
    Visual Studio distribuerar hello-projektet och öppnar en webbläsare toohello webbprogram bas-URL. Det här alternativet visas sidan 403-fel, vilket är normalt för ett försök toogo tooa Web API bas-URL från en webbläsare.
4. Stäng hello webbläsare.

### <a name="get-azure-ad-configuration-values"></a>Få konfigurationsvärden för Azure AD
1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com/), gå för**Azure Active Directory**.
2. På hello **Directory** klickar du på AAD-klient.
3. Klicka på **program > program som företaget äger**, och klicka sedan på hello är markerat.
4. Klicka på hello namnet på hello som Azure skapade när du aktiverat autentisering för hello ToDoListDataAPI (datanivå) API-app i hello listan med program.
5. Klicka på hello **konfigurera** fliken.
6. Kopiera hello **klient-ID** värde och spara den någonstans du hämtar det senare. 
7. Gå tillbaka i hello klassiska Azure-portalen toohello lista över **program som företaget äger**, och klicka på hello AAD-program som du skapade för hello mellannivå-API-appen ToDoListAPI (hello du skapade i föregående självstudien hello inte hello du skapade i den här kursen).
8. Klicka på hello **konfigurera** fliken.
9. Kopiera hello **klient-ID** värde och spara den någonstans du hämtar det senare.
10. Under **nycklar**väljer **1 års** från hello **Markera varaktighet** listrutan.
11. Klicka på **Spara**.
    
     ![Generera appkey](./media/app-service-api-dotnet-service-principal-auth/genkey.png)
12. Kopiera hello nyckelvärdet och spara den någonstans du hämtar det senare.
    
     ![Kopiera den nya appen nyckeln](./media/app-service-api-dotnet-service-principal-auth/genkeycopy.png)

### <a name="configure-azure-ad-settings-in-hello-middle-tier-api-apps-runtime-environment"></a>Konfigurera Azure AD-inställningar i hello mellannivå-API app körningsmiljön
1. Gå toohello [Azure-portalen](https://portal.azure.com/), och sedan gå toohello **API-App** bladet för hello API-app som är värd för TodoListAPI (mellannivå) hello-projektet.
2. Klicka på **Inställningar > Programinställningar**.
3. I hello **appinställningar** lägger du till följande hello nycklar och värden:
   
   | **Nyckel** | IDA: utfärdare |
   | --- | --- |
   | **Värde** |https://login.microsoftonline.com/ {Azure AD klientnamn} |
   | **Exempel** |https://login.microsoftonline.com/contoso.onmicrosoft.com |
   
   | **Nyckel** | IDA: ClientId |
   | --- | --- |
   | **Värde** |Klient-ID för hello anropar programmet (mellannivå - ToDoListAPI) |
   | **Exempel** |960adec2-b74a-484a-960adec2-b74a-484a |
   
   | **Nyckel** | IDA: ClientSecret |
   | --- | --- |
   | **Värde** |App-nyckeln för hello anropar programmet (mellannivå - ToDoListAPI) |
   | **Exempel** |e65e8fc9-5f6b-48e8-e65e8fc9-5f6b-48e8 |
   
   | **Nyckel** | IDA: resurs |
   | --- | --- |
   | **Värde** |Klient-ID för hello kallas program (ToDoListDataAPI på datanivå -) |
   | **Exempel** |e65e8fc9-5f6b-48e8-e65e8fc9-5f6b-48e8 |
   
    **Obs**: för `ida:Resource`, kontrollera att du använder hello kallas programmets **klient-ID** och inte dess **App-ID URI**.
   
    `ida:ClientId`och `ida:Resource` är olika värden för den här självstudiekursen eftersom du använder separata Azure AD applicaations för hello mellannivå och datanivå. Om du använder en enda Azure AD-program för hello anropa API-app och hello skyddas API-app kan du vill använda samma värde i både hello `ida:ClientId` och `ida:Resource`.
   
    hello koden använder ConfigurationManager tooget värdena, så att de kan lagras i Web.config-filen för hello projekt eller i hello Azure körningsmiljön. När ett ASP.NET-program körs i Azure App Service, miljö automatiskt inställningar åsidosätter inställningar från Web.config. Miljöinställningarna är vanligtvis en [säkrare sätt toostore känslig information jämfört med tooa Web.config-filen](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).
4. Klicka på **Spara**.
   
    ![Klicka på Spara](./media/app-service-api-dotnet-service-principal-auth/appsettings.png)

### <a name="test-hello-application"></a>Testa programmet hello
1. I en webbläsare går toohello HTTPS-URL för hello AngularJS frontend-webbprogram.
2. Klicka på hello **tooDo lista** fliken och logga in med autentiseringsuppgifterna för en användare i din Azure AD-klient. 
3. Lägg till uppgiften objekt tooverify som programmet hello fungerar.
   
    ![sidan tooDo](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)
   
    Kontrollera alla hello-inställningar som du angav i hello Azure-portalen om hello program inte fungerar som förväntat. Om alla hello inställningar visas toobe rätt finns hello [felsökning](#troubleshooting) senare i den här kursen.

## <a name="protect-hello-api-app-from-browser-access"></a>Skydda hello API-app från webbläsaråtkomst
Du har skapat en separat för den här självstudiekursen Azure AD-program för hello ToDoListDataAPI (datanivå) API-app. Som du har sett när App Service skapar ett AAD-program, konfigurerar hello AAD-program på ett sätt som gör att en användare toogo toohello API-appens URL i en webbläsare och logga på. Det innebär är det möjligt för en användare i din Azure AD-klient, inte bara en tjänstens huvudnamn, tooaccess hello API. 

Om du vill tooprevent webbläsare utan att skriva någon kod i hello skyddade API-app, kan du ändra hello **Reply URL** i hello AAD-program så att det skiljer sig från hello API app bas-URL. 

### <a name="disable-browser-access"></a>Inaktivera webbläsaråtkomst
1. I hello klassiska portalen **konfigurera** fliken för hello AAD-program som har skapats för hello TodoListService, ändrar hello värdet i hello **Reply URL** så att det är en giltig URL, men inte hello API Apps URL-ADRESSEN.
2. Klicka på **Spara**.

### <a name="verify-browser-access-no-longer-works"></a>Kontrollera webbläsaråtkomst fungerar inte längre
Tidigare verifierade du att du kan gå toohello API-appens URL i en webbläsare genom att logga in med autentiseringsuppgifterna för en enskild användare. I det här avsnittet kan kontrollera du att det inte längre. 

1. Gå toohello URL för hello API-app igen i ett nytt webbläsarfönster.
2. Logga in när du uppmanas till detta toodo så.
3. Inloggningen lyckas men leder tooan felsidan.
   
    Du har konfigurerat hello AAD app så att användare i hello AAD-klient inte kan logga in och komma åt hello-API från en webbläsare. Du kan fortfarande komma åt hello API-app med hjälp av en service principal-token som du kan kontrollera genom att gå toohello webbappens Webbadress och lägga till flera arbetsuppgifter.

## <a name="restrict-access-tooa-particular-service-principal"></a>Begränsa åtkomst tooa viss tjänstens huvudnamn
Just nu, och varje anropare som kan hämta en token för en användare eller tjänstens huvudnamn i Azure AD-klienten kan anropa hello TodoListDataAPI (datanivå) API-app. Du kanske vill toomake till att hello API-appen på datanivå accepterar bara anrop från hello TodoListAPI (mellannivå) API-app och bara från en viss tjänstens huvudnamn. 

Du kan lägga till dessa begränsningar genom att lägga till kod toovalidate hello `appid` och `objectidentifier` anspråk på inkommande samtal.

För den här kursen kan du placera hello-kod som verifierar app-ID och tjänsten ägar-ID direkt i domänkontrollanten-åtgärder.  Alternativen är toouse en anpassad `Authorize` attribut eller toodo verifieringen i Start-sekvenser (t.ex. OWIN mellanprogram). Ett exempel på hello senare finns [i det här exempelprogrammet](https://github.com/mohitsriv/EasyAuthMultiTierSample/blob/master/MyDashDataAPI/Startup.cs). 

Gör följande ändringar toohello TodoListDataAPI-projektet hello.

1. Öppna hello *Controllers/TodoListController.cs* fil.
2. Ta bort kommentarerna hello rader som anges `trustedCallerClientId` och `trustedCallerServicePrincipalId`.
   
        private static string trustedCallerClientId = ConfigurationManager.AppSettings["todo:TrustedCallerClientId"];
        private static string trustedCallerServicePrincipalId = ConfigurationManager.AppSettings["todo:TrustedCallerServicePrincipalId"];
3. Ta bort kommentarerna hello koden i hello CheckCallerId metod. Den här metoden anropas hello början av varje åtgärdsmetod i hello controller. 
   
        private static void CheckCallerId()
        {
            string currentCallerClientId = ClaimsPrincipal.Current.FindFirst("appid").Value;
            string currentCallerServicePrincipalId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
            if (currentCallerClientId != trustedCallerClientId || currentCallerServicePrincipalId != trustedCallerServicePrincipalId)
            {
                throw new HttpResponseException(new HttpResponseMessage { StatusCode = HttpStatusCode.Unauthorized, ReasonPhrase = "hello appID or service principal ID is not hello expected value." });
            }
        }
4. Omdistribuera hello ToDoListDataAPI-projektet tooAzure Apptjänst.
5. Gå toohello AngularJS klientdelen webbprogram HTTPS-URL i webbläsaren, och klicka på hello hello på startsidan i **tooDo lista** fliken.
   
    hello program fungerar inte eftersom anrop toohello tillbaka avslutas inte importeras. hello ny kod kontrollerar faktiska appid och objectidentifier men som inte ännu har hello rätt värden toocheck dem mot. hello webbläsare verktyg Utvecklarkonsolen rapporterar hello servern returnerar ett HTTP 401-fel.
   
    ![Fel i Developer Tools-konsolen](./media/app-service-api-dotnet-service-principal-auth/webapperror.png)
   
    I följande steg hello konfigurerar du hello förväntade värden.
6. Med hjälp av Azure AD PowerShell, hämta hello värdet för hello service principal för hello Azure AD-program som du skapade för hello TodoListWebApp projekt.
   
    a. Anvisningar för hur tooinstall Azure PowerShell och ansluter tooyour prenumeration, se [med hjälp av Azure PowerShell med Azure Resource Manager](../powershell-azure-resource-manager.md).
   
    b. tooget en lista över tjänstens huvudnamn köra hello `Login-AzureRmAccount` kommando och sedan hello `Get-AzureRmADServicePrincipal` kommando.
   
    c. Hitta hello objectid för hello tjänstens huvudnamn för hello TodoListAPI program och spara den på en plats som du kan kopiera från senare.
7. Navigera toohello API app bladet för hello API-app som du har distribuerat hello ToDoListDataAPI-projektet till i hello Azure-portalen.
8. Klicka på **Inställningar > programinställningar**.
9. I hello **appinställningar** lägger du till följande hello nycklar och värden:
   
   | **Nyckel** | TODO:TrustedCallerServicePrincipalId |
   | --- | --- |
   | **Värde** |Tjänsten ägar-id på anrop av programmet |
   | **Exempel** |4f4a94a4-6f0d-4072-4f4a94a4-6f0d-4072 |
   
   | **Nyckel** | TODO:TrustedCallerClientId |
   | --- | --- |
   | **Värde** |Klient-ID för anropar program - kopieras från hello TodoListAPI Azure AD-program |
   | **Exempel** |960adec2-b74a-484a-960adec2-b74a-484a |
10. Klicka på **Spara**.
    
     ![Klicka på Spara](./media/app-service-api-dotnet-service-principal-auth/trustedcaller.png)
11. Returnera toohello webbappens Webbadress i webbläsaren, och klicka på hello hello på startsidan i **tooDo lista** fliken.
    
     Det här programmet hello fungerar som förväntat eftersom hello betrodda anropare app-ID och tjänstens huvudnamn hello förväntade värden.
    
     ![sidan tooDo](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)

## <a name="building-hello-projects-from-scratch"></a>Skapa hello projekt från grunden
hello två Web API-projekt har skapats med hjälp av hello **Azure API Apps** mall och ersätta hello standard värden controller med en ToDoList-styrenhet. För att hämta Azure AD service principal token i hello ToDoListAPI-projektet hello [Active Directory Authentication Library (ADAL) för .NET](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) NuGet-paketet har installerats.

För information om hur skapar för ett program för AngularJS-sida med en webb-API-serverdel som ToDoListAngular, se [händerna på Lab: skapa en enkel sida program (SPA) med ASP.NET Web API och Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Information om hur tooadd koden för Azure AD finns [skydda AngularJS enda sidan appar med Azure AD](../active-directory/active-directory-devquickstarts-angular.md).

## <a name="troubleshooting"></a>Felsökning
[!INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Kontrollera att du blanda inte ihop ToDoListAPI (mellannivå) och ToDoListDataAPI (datanivå). Till exempel i den här självstudiekursen du lägger till autentisering toohello API-appen på datanivå, **men hello appkey måste komma från hello Azure AD-program som du skapade för hello mellannivå-API-app**.

## <a name="next-steps"></a>Nästa steg
Detta är hello sista kurs i hello API Apps serien. 

Mer information om Azure Active Directory finns i hello följande resurser.

* [Azure AD-utvecklarhandboken](http://aka.ms/aaddev)
* [Azure AD-scenarier](http://aka.ms/aadscenarios)
* [Azure AD-exempel](http://aka.ms/aadsamples)
  
    Hej [WebApp-WebAPI-OAuth2-AppIdentity-DotNet](http://github.com/AzureADSamples/WebApp-WebAPI-OAuth2-AppIdentity-DotNet) exempel liknar toowhat visas i den här självstudiekursen, men utan att använda autentisering i Apptjänst.

Information om andra sätt toodeploy Visual Studio-projekt tooAPI appar med hjälp av Visual Studio eller [automatisera distributionen](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) från en [källkontrollsystem](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control), se [hur toodeploy en Azure Apptjänst-app](../app-service-web/web-sites-deploy.md).

