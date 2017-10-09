---
title: aaaCreate en Azure line-of-business-program med AD FS-autentisering | Microsoft Docs
description: "Lär dig hur toocreate en line-of-business-app i Azure App Service som autentiserar med lokala STS. Den här kursen riktar sig till AD FS som hello lokala STS."
services: app-service\web
documentationcenter: .net
author: cephalin
manager: erikre
editor: 
ms.assetid: 0fa9f7a1-37bd-4d11-845f-aeff6fc9e4ca
ms.service: app-service-web
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 08/31/2016
ms.author: cephalin
ms.openlocfilehash: cfd6f14837642fbf58ca5da9dee72db69cd5ab7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-line-of-business-azure-app-with-ad-fs-authentication"></a>Skapa en Azure line-of-business-program med AD FS-autentisering
Den här artikeln beskrivs hur du toocreate en ASP.NET MVC line-of-business-program i [Azure App Service](../app-service/app-service-value-prop-what-is.md) med hjälp av en lokal [Active Directory Federation Services](http://technet.microsoft.com/library/hh831502.aspx) som hello identitetsleverantör. Det här scenariot kan arbeta när du vill toocreate line-of-business-program i Azure App Service, men din organisation kräver directory data toobe lagras på plats.

> [!NOTE]
> En översikt över hello olika enterprise autentisering och auktorisering alternativ för Azure App Service finns [autentisera med lokala Active Directory i din Azure-app](web-sites-authentication-authorization.md).
> 
> 

<a name="bkmk_build"></a>

## <a name="what-you-will-build"></a>Vad du ska skapa
Du skapar en grundläggande ASP.NET-program i Azure App Service Web Apps med hello följande funktioner:

* Autentiserar användare mot AD FS
* Använder `[Authorize]` tooauthorize användare för olika åtgärder
* Statisk konfiguration för både felsökning i Visual Studio och publicera tooApp Service Web Apps (konfigurera en gång, felsöka och publicera när som helst)  

<a name="bkmk_need"></a>

## <a name="what-you-need"></a>Vad du behöver
[!INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

Den här kursen behöver hello följande toocomplete:

* En lokal AD FS-distribution (slutpunkt till slutpunkt-genomgång av hello-testlabb som används i den här kursen finns [testlabbet: fristående STS med AD FS i Azure VM (för test)](https://blogs.msdn.microsoft.com/cephalin/2014/12/21/test-lab-standalone-sts-with-ad-fs-in-azure-vm-for-test-only/))
* Behörigheter toocreate förlitande part förtroende i AD FS-hantering
* Visual Studio 2013 Update 4 eller senare
* [Azure SDK 2.8.1](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) eller senare

<a name="bkmk_sample"></a>

## <a name="use-sample-application-for-line-of-business-template"></a>Använda exempelprogram för line-of-business-mall
Hej exempelprogrammet i den här självstudiekursen [WebApp-WSFederation-DotNet)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet), skapas av hello Azure Active Directory-teamet. Eftersom AD FS stöder WS-Federation, kan du använda den som en mall toocreate line-of-business-program utan problem. Det har hello följande funktioner:

* Använder [WS-Federation](http://msdn.microsoft.com/library/bb498017.aspx) tooauthenticate med en lokal AD FS-distribution
* Funktioner för inloggning och utloggning
* Använder [Microsoft.Owin](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana) (i stället för Windows Identity Foundation), vilket är hello framtida av ASP.NET och mycket enklare tooset för autentisering och auktorisering än WIF

<a name="bkmk_setup"></a>

## <a name="set-up-hello-sample-application"></a>Ställ in hello exempelprogrammet
1. Klona eller hämta hello exempellösning på [WebApp-WSFederation-DotNet](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) tooyour lokal katalog.
   
   > [!NOTE]
   > Hej instruktionerna på [README.md](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet/blob/master/README.md) visar hur tooset hello ansökan med Azure Active Directory. Men i den här självstudiekursen kommer du skapat med AD FS, så hello gör så här i stället.
   > 
   > 
2. Öppna hello lösningen och öppna Controllers\AccountController.cs i hello **Solution Explorer**.
   
   Du ser att hello kod bara utfärdar en autentisering challenge tooauthenticate hello-användare med hjälp av WS-Federation. Alla autentisering har konfigurerats i App_Start\Startup.Auth.cs.
3. Öppna App_Start\Startup.Auth.cs. I hello `ConfigureAuth` metod, Observera hello rad:
   
       app.UseWsFederationAuthentication(
           new WsFederationAuthenticationOptions
           {
               Wtrealm = realm,
               MetadataAddress = metadata                                      
           });
   
   I OWIN hälsningsmeddelande är den här fragment verkligen hello utan minsta måste tooconfigure WS-Federation-autentisering. Det är mycket enklare och mer elegant än WIF, där Web.config injekteras med XML över hela hello plats. Hej endast information du behöver är hello överförande partens (RP) identifierare och hello-URL för din AD FS-tjänsten metadatafil. Här är ett exempel:
   
   * RP-ID:`https://contoso.com/MyLOBApp`
   * Metadata-adress:`http://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`
4. I App_Start\Startup.Auth.cs, ändrar du hello följande definitioner av statiska sträng:  
   
   <pre class="prettyprint">
   private static string realm = ConfigurationManager.AppSettings["ida:<mark>RPIdentifier</mark>"];
   <mark><del>private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];</del></mark>
   <mark><del>private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];</del></mark>
   <mark><del>private static string metadata = string.Format("{0}/{1}/federationmetadata/2007-06/federationmetadata.xml", aadInstance, tenant);</del></mark>
   <mark>private static string metadata = string.Format("https://{0}/federationmetadata/2007-06/federationmetadata.xml", ConfigurationManager.AppSettings["ida:ADFS"]);</mark>
   
   <mark><del>string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);</del></mark>
   </pre>
5. Nu kan göra hello motsvarande ändringar i Web.config. Öppna hello Web.config och ändra hello följande app-inställningar:  
   
   <pre class="prettyprint">
   &lt;appSettings&gt;
   &lt;add key="webpages:Version" value="3.0.0.0" /&gt;
   &lt;add key="webpages:Enabled" value="false" /&gt;
   &lt;add key="ClientValidationEnabled" value="true" /&gt;
   &lt;add key="UnobtrusiveJavaScriptEnabled" value="true" /&gt;
   <mark><del>&lt;add key="ida:Wtrealm" value="[Enter hello App ID URI of WebApp-WSFederation-DotNet https://contoso.onmicrosoft.com/WebApp-WSFederation-DotNet]" /&gt;</del></mark>
   <mark><del>&lt;add key="ida:AADInstance" value="https://login.microsoftonline.com" /&gt;</del></mark>
   <mark><del>&lt;add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" /&gt;</del></mark>
   <mark>&lt;add key="ida:RPIdentifier" value="[Enter hello relying party identifier as configured in AD FS, e.g. https://localhost:44320/]" /&gt;</mark>
   <mark>&lt;add key="ida:ADFS" value="[Enter hello FQDN of AD FS service, e.g. adfs.contoso.com]" /&gt;</mark>
   
   &lt;/appSettings&gt;
   </pre>
   
   Fyll i hello nyckelvärden baserat på respektive miljön.
6. Skapa hello programmet toomake att det inte finns några fel.

Det var allt. Hello exempelprogrammet är nu redo toowork med AD FS. Du måste fortfarande tooconfigure ett RP förtroende med det här programmet i AD FS.

<a name="bkmk_deploy"></a>

## <a name="deploy-hello-sample-application-tooazure-app-service-web-apps"></a>Distribuera hello exempel programmet tooAzure App Service Web Apps
Här kan publicera hello programmet tooa webbapp i App Service Web Apps samtidigt som hello debug-miljö. Observera att du ska toopublish hello programmet innan det finns ett RP förtroende med AD FS så autentisering inte fungerar ändå ännu. Om du gör det kan nu du dock ha hello web app-URL som du kan använda tooconfigure hello RP förtroende senare.

1. Högerklicka på projektet och välj **publicera**.
   
    ![](./media/web-sites-dotnet-lob-application-adfs/01-publish-website.png)
2. Välj **Microsoft Azure App Service**.
3. Om du inte har registrerat i tooAzure, klickar du på **logga In** och använda hello Microsoft-konto för din Azure-prenumeration toosign i.
4. När du är inloggad klickar du på **ny** toocreate ett webbprogram.
5. Fyll i alla obligatoriska fält. Du kommer tooconnect tooon lokala data senare, så att inte skapa en databas för det här webbprogrammet.
   
    ![](./media/web-sites-dotnet-lob-application-adfs/02-create-website.png)
6. Klicka på **Skapa**. När hello webbprogrammet har skapats, öppnas hello Publicera webbplats dialogrutan.
7. I **Måladress**, ändra **http** för**https**. Kopiera hello hela URL tooa textredigerare för senare användning. Klicka på **publicera**.
   
    ![](./media/web-sites-dotnet-lob-application-adfs/03-destination-url.png)
8. Öppna i Visual Studio **Web.Release.config** i projektet. Infoga följande XML till hello hello `<configuration>` tagga och Ersätt hello nyckel/värde till ditt webbprogram för publicera URL: en.  
   
   <pre class="prettyprint">
   &lt;appSettings&gt;
   &lt;add key="ida:RPIdentifier" value="<mark>[e.g. https://mylobapp.azurewebsites.net/]</mark>" xdt:Transform="SetAttributes" xdt:Locator="Match(key)" /&gt;
   &lt;/appSettings&gt;</pre>

När du är klar, du har två RP-identifierare som konfigurerats i ditt projekt, en för din miljö för felsökning i Visual Studio och en för hello publicerade webb-app i Azure. Du ställer in ett RP-förtroende för varje hello två miljöer i AD FS. Vid felsökning hello app-inställningar i Web.config är används toomake din **felsöka** konfigurationen fungerar med AD FS. När den publiceras (som standard hello **versionen** configuration publiceras), omvandlade Web.config överförs som innehåller hello app inställningen ändringar i Web.Release.config.

Om du vill tooattach hello publicerade webbappen i Azure toohello felsökare (dvs. Du måste överföra symboler för felsökning av din kod i hello publicerade webb-app), kan du skapa en klon av hello felsöka konfigurationen för Azure-felsökning, men transformera (med sin egen anpassade Web.config t.ex. Web.AzureDebug.config) som använder hello app-inställningar från Web.Release.config. Detta ger dig toomaintain en statisk konfiguration mellan olika hello-miljöer.

<a name="bkmk_rptrusts"></a>

## <a name="configure-relying-party-trusts-in-ad-fs-management"></a>Konfigurera förtroenden för förlitande part i AD FS-hantering
Du måste nu tooconfigure ett RP förtroende i AD FS-hantering innan du kan använda exempelprogrammet och faktiskt autentisera med AD FS. Du behöver tooset in två separata RP-förtroenden, en för din miljö för felsökning och en för ditt publicerade webbprogram.

> [!NOTE]
> Kontrollera att du upprepar hello följande för både dina miljöer.
> 
> 

1. Logga in med autentiseringsuppgifter som har management rättigheter tooAD as på AD FS-servern.
2. Öppna AD FS-hantering. Högerklicka på **AD FS\Trusted Relationships\Relying partens förtroenden** och välj **Lägg till förlitande part**.
   
   ![](./media/web-sites-dotnet-lob-application-adfs/1-add-rptrust.png)
3. I hello **Välj datakälla** väljer **ange information om hello förlitande part manuellt**. 
   
   ![](./media/web-sites-dotnet-lob-application-adfs/2-enter-rp-manually.png)
4. I hello **ange visningsnamn** , Skriv ett namn för programmet hello och klickar på **nästa**.
5. I hello **Välj protokollet** klickar du på **nästa**.
6. I hello **konfigurera certifikat** klickar du på **nästa**.
   
   > [!NOTE]
   > Eftersom du ska använda HTTPS redan, är krypterade token valfria. Om du verkligen tooencrypt token från AD FS på den här sidan måste du också lägga till token-dekryptering logik i koden. Mer information finns i [manuellt konfigurera OWIN WS-Federation mellanprogram och ta emot krypterad token](http://chris.59north.com/post/2014/08/21/Manually-configuring-OWIN-WS-Federation-middleware-and-accepting-encrypted-tokens.aspx).
   > 
   > 
7. Innan du går vidare till nästa steg i hello, behöver du en typ av information från Visual Studio-projekt. Observera i hello projektegenskaperna hello **SSL URL** av programmet hello. 
   
   ![](./media/web-sites-dotnet-lob-application-adfs/3-ssl-url.png)
8. Tillbaka i AD FS-hantering, i hello **konfigurera URL** sidan hello **guiden Lägg till förlitande part förtroende**väljer **aktivera stöd för hello protokollet WS-Federation Passive** och Skriv i hello SSL-URL för Visual Studio-projekt som du antecknade i hello föregående steg. Klicka sedan på **Nästa**.
   
   ![](./media/web-sites-dotnet-lob-application-adfs/4-configure-url.png)
   
   > [!NOTE]
   > URL: en anger där toosend hello klient efter autentisering lyckas. För hello debug miljö bör man <code>https://localhost:&lt;port&gt;/</code>. Det bör vara hello web app-URL för hello publicerade webbprogrammet.
   > 
   > 
9. I hello **konfigurera identifierare** kontrollerar du att projektet SSL-URL finns redan i listan och klicka på **nästa**. Klicka på **nästa** alla hello sätt toohello slutet av hello guiden med standardinställningarna.
   
   > [!NOTE]
   > I App_Start\Startup.Auth.cs för Visual Studio-projekt identifieraren matchas mot hello värdet för <code>WsFederationAuthenticationOptions.Wtrealm</code> under federerad autentisering. Som standard läggs hello programmets URL hello föregående steg som en RP-identifierare.
   > 
   > 
10. Du har nu konfigurerat hello RP program för ditt projekt i AD FS. Därefter konfigurerar du det här programmet toosend hello anspråk som behövs i programmet. Hej **redigera Anspråksregler** dialogrutan är öppnas som standard hello slutet av hello guiden så att du kan starta direkt. Nu ska vi konfigurera hello minst följande anspråk (med scheman inom parentes):
    
    * Namn (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name -) som används av ASP.NET toohydrate `User.Identity.Name`.
    * Användarens huvudnamn (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn) - används toouniquely identifiera användare i hello organisation.
    * Gruppmedlemskap som roller (http://schemas.microsoft.com/ws/2008/06/identity/claims/role) - kan användas med `[Authorize(Roles="role1, role2,...")]` decoration tooauthorize domänkontrollanter/en åtgärd. I verkligheten kan kan den här metoden inte vara hello de flesta performant för auktorisering av roller. Om din AD-användare tillhör toohundreds säkerhetsgrupper, blir de hundratals rollanspråk i hello SAML-token. En annan metod är toosend som en enda roll anspråk villkorligt beroende på hello användarens medlemskap i en viss grupp. Men kommer vi enkelhet för den här självstudiekursen.
    * Name ID (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier) - kan användas för skydd mot förfalskning verifiering. Finns mer information om hur toomake är fungerar med skydd mot förfalskning validering hello **lägger till funktioner för line-of-business** avsnitt i [skapa en Azure line-of-business-app med Azure Active Directory-autentisering ](web-sites-dotnet-lob-application-azure-ad.md#bkmk_crud).
    
    > [!NOTE]
    > hello anspråk datatyper du behöver tooconfigure för ditt program bestäms av programmets behov. Hello listan med anspråk som stöds av Azure Active Directory-program (d.v.s. RP förtroenden), till exempel finns i [stöds Token och anspråkstyper](http://msdn.microsoft.com/library/azure/dn195587.aspx).
    > 
    > 
11. Klicka på i dialogrutan Redigera Anspråksregler hello **Lägg till regel**.
12. Konfigurera hello name, UPN och roll anspråk enligt hello skärmbild och klicka på **Slutför**.
    
    ![](./media/web-sites-dotnet-lob-application-adfs/5-ldap-claims.png)
    
    Därefter skapar du ett tillfälligt namn ID anspråk med hello steg visas i [namn identifierare i SAML intyg](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).
13. Klicka på **Lägg till regel** igen.
14. Välj **skicka anspråk med en anpassad regel** och på **nästa**.
15. Klistra in hello följande regel språk i hello **anpassad regel** rutan, hello regel **Per sessionsidentifierare** och klicka på **Slutför**.  
    
    <pre class="prettyprint">
    c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"] &amp;&amp;
    c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationinstant"]
     => add(
         store = "_OpaqueIdStore",
         types = ("<mark>http://contoso.com/internal/sessionid</mark>"),
         query = "{0};{1};{2};{3};{4}",
         param = "useEntropy",
         param = c1.Value,
         param = c1.OriginalIssuer,
         param = "",
         param = c2.Value);
    </pre>
    
    En anpassad regel bör se ut som liknar denna skärmbild:
    
    ![](./media/web-sites-dotnet-lob-application-adfs/6-per-session-identifier.png)
16. Klicka på **Lägg till regel** igen.
17. Välj **transformera ett inkommande anspråk** och på **nästa**.
18. Konfigurera hello regeln enligt hello skärmbild (med hello Anspråkstyp som du skapade i hello anpassad regel) och klicka på **Slutför**.
    
    ![](./media/web-sites-dotnet-lob-application-adfs/7-transient-name-id.png)
    
    Detaljerad information om hello steg för hello tillfälligt namn-ID-anspråk finns [namn identifierare i SAML intyg](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).
19. Klicka på **tillämpa** i hello **redigera Anspråksregler** dialogrutan. Det bör nu se ut som följande skärmbild hello:
    
    ![](./media/web-sites-dotnet-lob-application-adfs/8-all-claim-rules.png)
    
    > [!NOTE]
    > Se till att du upprepa dessa steg för både debug-miljön och publicerade webbprogram.
    > 
    > 

<a name="bkmk_test"></a>

## <a name="test-federated-authentication-for-your-application"></a>Testa federerad autentisering för ditt program
Du är klar tootest programmets autentiseringslogiken mot AD FS. Jag har en användare som tillhör tooa testgrupp i Active Directory (AD) i min AD FS i laboratoriemiljö.

![](./media/web-sites-dotnet-lob-application-adfs/10-test-user-and-group.png)

tootest autentisering i hello felsökare allt du behöver toodo nu är typ `F5`. Om du vill tootest autentisering i hello publicerade webbprogram, navigera toohello URL.

När hello webbprogrammet har lästs in, klickar du på **logga In**. Nu bör du få antingen en dialogruta eller hello inloggningen inloggningssida hanteras av AD FS, beroende på hello autentiseringsmetod som väljs av AD FS. Det här är vad jag i Internet Explorer 11.

![](./media/web-sites-dotnet-lob-application-adfs/9-test-debugging.png)

När du loggar in med en användare i hello AD-domänen för hello AD FS-distribution kan du bör nu se hello webbsida igen med **Hello, <User Name>!** i hello hörnet. Det här är vad jag.

![](./media/web-sites-dotnet-lob-application-adfs/11-test-debugging-success.png)

Hittills har du lyckades hello följande sätt:

* Programmet har nått AD FS och en matchande RP-identifierare finns i hello AD FS-databasen
* AD FS har autentiserats en AD-användare och omdirigering du säkerhetskopiera toohello programmets startsida
* AD FS som har skickats hello namn anspråk (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) tooyour program som anges av hello faktum hello användaren namnet visas i hello hörnet. 

Om anspråket hello saknas du skulle ha sett **Hello,!**. Om du tittar på Views\Shared\_LoginPartial.cshtml, som du hittar den använder `User.Identity.Name` toodisplay hello användarnamn. Som nämnts tidigare, om hello namn anspråk av hello autentiserade användaren är tillgänglig i hello SAML-token är hydrates den här egenskapen med det i ASP.NET. toosee alla hello anspråk som skickas av AD FS, placera en brytpunkt i Controllers\HomeController.cs, i hello Index åtgärdsmetod. När hello användaren autentiseras inspektera hello `System.Security.Claims.Current.Claims` samling.

![](./media/web-sites-dotnet-lob-application-adfs/12-test-debugging-all-claims.png) 

<a name="bkmk_authorize"></a>

## <a name="authorize-users-for-specific-controllers-or-actions"></a>Auktorisera användare till specifika domänkontrollanter eller åtgärder
Eftersom du har inkluderat gruppmedlemskap som rollanspråk i konfigurationen RP förtroende, du kan nu använda dem direkt i hello `[Authorize(Roles="...")]` decoration för domänkontrollanter och åtgärder. I ett line-of-business-program med hello skapa-Läs-Uppdatera-ta bort CRUD-mönster auktorisera du specifika roller tooaccess varje åtgärd. Just nu kommer du bara testa den här funktionen på hello befintliga Home domänkontrollant.

1. Öppna Controllers\HomeController.cs.
2. Skapa snygga hello `About` och `Contact` åtgärd metoder liknande toohello följande kod, med medlemskap i säkerhetsgrupper som den autentiserade användaren har.  
   
    <pre class="prettyprint">
    <mark>[Authorize(Roles="Test Group")]</mark>
    public ActionResult About()
    {
        ViewBag.Message = "Your application description page.";
   
        return View();
    }
   
    <mark>[Authorize(Roles="Domain Admins")]</mark>
    public ActionResult Contact()
    {
        ViewBag.Message = "Your contact page.";
   
        return View();
    }
    </pre>
   
    Eftersom jag har lagt till **testanvändare** för**testgrupp** i min testmiljö för AD FS ska jag använda testgrupp tootest tillstånd på `About`. För `Contact`, kan jag testa hello negativt fall av **Domänadministratörer**, toowhich **testa användare** hör inte.
3. Starta hello felsökare genom att skriva `F5` och logga in och klicka sedan på **om**. Du bör nu visa hello `~/About/Index` sidan, om den autentiserade användaren har behörighet för åtgärden.
4. Klicka på **Kontakta**, som i min fallet bör inte tillåta **testanvändare** för hello-åtgärd. Hello webbläsaren är dock omdirigerade tooAD FS som slutligen visar det här meddelandet:
   
    ![](./media/web-sites-dotnet-lob-application-adfs/13-authorize-adfs-error.png)
   
    Om du undersöka det här felet i Loggboken på hello AD FS-servern, visas den här Undantagsmeddelande:  
   
    <pre class="prettyprint">
    Microsoft.IdentityServer.Web.InvalidRequestException: MSIS7042: <mark>hello same client browser session has made '6' requests in hello last '11' seconds.</mark> Contact your administrator for details.
       at Microsoft.IdentityServer.Web.Protocols.PassiveProtocolHandler.UpdateLoopDetectionCookie(WrappedHttpListenerContext context)
       at Microsoft.IdentityServer.Web.Protocols.WSFederation.WSFederationProtocolHandler.SendSignInResponse(WSFederationContext context, MSISSignInResponse response)
       at Microsoft.IdentityServer.Web.PassiveProtocolListener.ProcessProtocolRequest(ProtocolContext protocolContext, PassiveProtocolHandler protocolHandler)
       at Microsoft.IdentityServer.Web.PassiveProtocolListener.OnGetContext(WrappedHttpListenerContext context)
    </pre>
   
    hello orsak till felet är att som standard MVC returnerar 401 obehörig när en användare inte har behörighet. Detta utlöser en identitetsleverantör under omautentisering begäran tooyour (AD FS). Eftersom hello användaren redan har autentiserats AD FS returnerar toohello samma sida som utfärdar en annan 401, skapa en omdirigering loop. Du kommer att åsidosätta Authorizeattribute's `HandleUnauthorizedRequest` metod med enkel logik tooshow något som passar i stället för fortsätter hello omdirigering loop.
5. Skapa en fil i hello projektet med namnet AuthorizeAttribute.cs och klistra in hello följande kod till den.
   
        using System;
        using System.Web.Mvc;
        using System.Web.Routing;
   
        namespace WebApp_WSFederation_DotNet
        {
            [AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, Inherited = true, AllowMultiple = true)]
            public class AuthorizeAttribute : System.Web.Mvc.AuthorizeAttribute
            {
                protected override void HandleUnauthorizedRequest(AuthorizationContext filterContext)
                {
                    if (filterContext.HttpContext.Request.IsAuthenticated)
                    {
                        filterContext.Result = new System.Web.Mvc.HttpStatusCodeResult((int)System.Net.HttpStatusCode.Forbidden);
                    }
                    else
                    {
                        base.HandleUnauthorizedRequest(filterContext);
                    }
                }
            }
        }
   
    hello åsidosätta koden skickar en HTTP 403 (förbjuden) i stället för HTTP 401 (obehörig) i autentiserade men obehörig fall.
6. Kör hello felsökare igen med `F5`. Klicka på **Kontakta** visas nu ett meddelande om mer informativ (men alternativet):
   
    ![](./media/web-sites-dotnet-lob-application-adfs/14-unauthorized-forbidden.png)
7. Publicera hello programmet tooAzure App Service Web Apps igen och testa hello hur hello live program.

<a name="bkmk_data"></a>

## <a name="connect-tooon-premises-data"></a>Ansluta tooon lokala data
En orsak till är att du vill ha tooimplement line-of-business-program med AD FS i stället för Azure Active Directory efterlevnadsproblem för att hålla organisation data från lokalt. Detta kan också innebära att ditt webbprogram i Azure måste komma åt lokala databaser, eftersom du inte kan toouse [SQL-databas](/services/sql-database/) som hello datanivå för web apps.

Azure App Service Web Apps stöder inte att komma åt lokala databaser med två sätt: [Hybridanslutningar](../biztalk-services/integration-hybrid-connection-overview.md) och [virtuella nätverk](web-sites-integrate-with-vnet.md). Mer information finns i [VNET med hjälp av integrering och Hybrid-anslutningar med Azure App Service Web Apps](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/).

<a name="bkmk_resources"></a>

## <a name="further-resources"></a>Ytterligare resurser
* [Autentisera med lokala Active Directory i din Azure-app](web-sites-authentication-authorization.md)
* [Skapa en Azure line-of-business-app med Azure Active Directory-autentisering](web-sites-dotnet-lob-application-azure-ad.md)
* [Använd hello lokalt organisationens autentisering alternativet (AD FS) med ASP.NET i Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/)
* [Migrera en VS2013 Web Project från WIF tooKatana](http://www.cloudidentity.com/blog/2014/09/15/MIGRATE-A-VS2013-WEB-PROJECT-FROM-WIF-TO-KATANA/)
* [Översikt över Active Directory Federation Services](http://technet.microsoft.com/library/hh831502.aspx)
* [Specifikation för WS-Federation 1.1](http://download.boulder.ibm.com/ibmdl/pub/software/dw/specs/ws-fed/WS-Federation-V1-1B.pdf?S_TACT=105AGX04&S_CMP=LP)

