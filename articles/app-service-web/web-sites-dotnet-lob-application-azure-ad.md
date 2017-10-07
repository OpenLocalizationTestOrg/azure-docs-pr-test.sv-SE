---
title: aaaCreate en Azure line-of-business-app med Azure Active Directory-autentisering | Microsoft Docs
description: "Lär dig hur toocreate en ASP.NET MVC line-of-business-app i Azure App Service som autentiserar med Azure Active Directory"
services: app-service\web, active-directory
documentationcenter: .net
author: cephalin
manager: erikre
editor: 
ms.assetid: ad947bdb-4463-43ff-a5e3-91d9b2169b60
ms.service: app-service-web
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 09/01/2016
ms.author: cephalin
ms.openlocfilehash: 3bcafad78ac0151889b3e336784cc561009f244f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-line-of-business-azure-app-with-azure-active-directory-authentication"></a>Skapa en Azure line-of-business-app med Azure Active Directory-autentisering
Den här artikeln beskrivs hur du toocreate en .NET line-of-business-app i [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) med hello [autentisering / auktorisering](../app-service/app-service-authentication-overview.md) funktion. Den visar även hur toouse hello [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) tooquery katalogdata i hello program.

hello Azure Active Directory-klient som du använder kan vara en endast Azure-katalog. Eller så kan det vara [synkroniserats med din lokala Active Directory](../active-directory/active-directory-aadconnect.md) toocreate en enkel inloggning för anställda som är lokala och fjärranslutna. Den här artikeln använder hello standardkatalog för ditt Azure-konto.

<a name="bkmk_build"></a>

## <a name="what-you-will-build"></a>Vad du ska skapa
Du skapar ett enkelt line-of-business skapa-Läs-Uppdatera-ta bort CRUD-program i App Service Web Apps att spårar arbetsobjekt med hello följande funktioner:

* Autentiserar användare mot Azure Active Directory
* Frågar directory-användare och grupper med [Azure Active Directory Graph API](http://msdn.microsoft.com/library/azure/hh974476.aspx)
* Använd hello ASP.NET MVC *ingen autentisering* mall

Om du behöver rollbaserad åtkomstkontroll (RBAC) för din line-of-business-app i Azure finns [nästa steg](#next).

<a name="bkmk_need"></a>

## <a name="what-you-need"></a>Vad du behöver
[!INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

Den här kursen behöver hello följande toocomplete:

* En Azure Active Directory-klient med användare i olika grupper
* Behörigheter toocreate program på hello Azure Active Directory-klient
* Visual Studio 2013 Update 4 eller senare
* [Azure SDK 2.8.1 eller senare](https://azure.microsoft.com/downloads/)

<a name="bkmk_deploy"></a>

## <a name="create-and-deploy-a-web-app-tooazure"></a>Skapa och distribuera en web app tooAzure
1. I Visual Studio klickar du på **filen** > **ny** > **projekt**.
2. Välj **ASP.NET-webbprogram**, namnge projektet och klicka på **OK**.
3. Välj hello **MVC** mall, ändra hello autentisering också**ingen autentisering**. Kontrollera att **värd i hello molnet** är markerad och klicka på **OK**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/1-create-mvc-no-authentication.png)
4. I hello **skapa Apptjänst** dialogrutan klickar du på **Lägg till ett konto** (och sedan **Lägg till ett konto** i listrutan hello) toolog i tooyour Azure-konto.
5. När du loggade in konfigurera ditt webbprogram. Skapa en resursgrupp och en ny programtjänstplan genom att klicka på respektive hello **ny** knappen. Klicka på **utforska ytterligare Azure-tjänster** toocontinue.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/2-create-app-service.png)
6. I hello **Services** klickar du på ** + ** tooadd en SQL-databas för din app. 
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/3-add-sql-database.png)
7. I **Konfigurera SQL-databas**, klickar du på **ny** toocreate en SQL Server-instans.
8. I **Konfigurera SQL Server**, konfigurera din SQL Server-instans. Klicka på **OK**, **OK**, och **skapa** tookick av hello skapa en app i Azure.
9. I **aktivitet för Azure App Service**, du kan se när hello skapa en app är klar. Klicka på * *publicera &lt; *appname*> toothis Webbapp nu ** Klicka **publicera**. 
   
    När Visual Studio är klar öppnas hello publicera appen i hello webbläsare. 
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/4-published-shown-in-browser.png)

<a name="bkmk_auth"></a>

## <a name="configure-authentication-and-directory-access"></a>Konfigurera autentisering och åtkomst
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Hello vänstra menyn klickar du på **Apptjänster** > **&lt;*appname*> ** > **autentisering / auktorisering**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/5-app-service-authentication.png)
3. Aktivera Azure Active Directory-autentisering genom att klicka **på** > **Azure Active Directory** > **Express**  >  ** OK**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/6-authentication-express.png)
4. Klicka på **spara** i hello kommandofältet.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/7-authentication-save.png)
   
    När har sparats hello autentiseringsinställningar, kan du försöka gå tooyour app igen i hello webbläsare. Standardinställningarna för att autentisering på hello hela appen. Om du inte redan är inloggad är omdirigerade tooa inloggningsskärmen. Efter loggat in kan se du din app skyddas av HTTPS. Sedan måste tooenable åtkomst toodirectory data. 
5. Navigera toohello [klassiska portalen](https://manage.windowsazure.com).
6. Hello vänstra menyn klickar du på **Active Directory** > **standardkatalog** > **program**  >  * * &lt; *appname*> **.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/8-find-aad-application.png)
   
    Detta är hello Azure Active Directory-program som Apptjänst som skapats för du tooenable hello auktorisering / autentisering.
7. Klicka på **användare** och **grupper** toomake till att du har vissa användare och grupper i hello directory. Om inte, skapa några testa användare och grupper.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/9-create-users-groups.png)
8. Klicka på **konfigurera** tooconfigure det här programmet.
9. Bläddra nedåt toohello **nycklar** och lägger till en nyckel genom att välja en varaktighet. Klicka på **delegerade behörigheter** och välj **läsa katalogdata**. 
   Klicka på **Spara**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/10-configure-aad-application.png)
10. När dina inställningar sparas, gå tillbaka toohello **nycklar** avsnittet och klicka på hello **kopiera** knappen toocopy hello klientens nyckel. 
    
     ![](./media/web-sites-dotnet-lob-application-azure-ad/11-get-app-key.png)
    
    > [!IMPORTANT]
    > Om du navigerar bort från den här sidan nu inte kan tooaccess klienten nyckeln någonsin igen.
    > 
    > 
11. Sedan måste tooconfigure ditt webbprogram med den här nyckeln. Logga in toohello [resursutforskaren Azure](https://resources.azure.com) med ditt Azure-konto.
12. Hello överst på hello-sidan, klickar du på **Skrivskyddstyp** toomake ändringar i hello Azure Resursläsaren.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/12-resource-manager-writable.png)
13. Hitta hello autentiseringsinställningar för din app på prenumerationer > * * &lt; *subscriptionname*> ** > **resursgrupper**  >  * * &lt; *resourcegroupname*> ** > **providers** > **Microsoft.Web**  >  **platser** > **&lt;*appname*> ** > **config**  >  **authsettings**.
14. Klicka på **Redigera**.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/13-edit-authsettings.png)
15. Hello Redigera fönstret, ange hello `clientSecret` och `additionalLoginParams` egenskaper på följande sätt.
    
        ...
        "clientSecret": "<client key from hello Azure Active Directory application>",
        ...
        "additionalLoginParams": ["response_type=code id_token", "resource=https://graph.windows.net"],
        ...
16. Klicka på **placera** på hello översta toosubmit ändringarna.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/14-edit-parameters.png)
17. Nu tootest om du har hello auktorisering token tooaccess hello Azure Active Directory Graph API, bara gå till * *https://&lt;*appname*>.azurewebsites.net/.auth/me** i din webbläsare. Om du har konfigurerat allt korrekt, bör du se hello `access_token` egenskap i hello JSON-svar.
    
    Hej `~/.auth/me` URL-sökväg som hanteras av App Service autentisering / auktorisering toogive du alla hello information som rör tooyour autentiserad session. Mer information finns i [autentisering och auktorisering i Azure App Service](../app-service/app-service-authentication-overview.md).
    
    > [!NOTE]
    > Hej `access_token` har en utgångsperiod. Dock App Service autentisering / auktorisering tillhandahåller token uppdateringsfunktionen med `~/.auth/refresh`. Mer information om hur toouse, se [App Service-Token Store](https://cgillum.tech/2016/03/07/app-service-token-store/).
    > 
    > 

Därefter kan du göra något användbart med katalogdata.

<a name="bkmk_crud"></a>

## <a name="add-line-of-business-functionality-tooyour-app"></a>Lägg till funktioner för line-of-business tooyour app
Nu kan skapa du en enkla CRUD arbete objekt spåraren.  

1. Skapa en klassfil som heter WorkItem.cs i hello ~\Models mapp och Ersätt `public class WorkItem {...}` med hello följande kod:
   
     med hjälp av System.ComponentModel.DataAnnotations;
   
     publik klass arbetsobjekt {
   
         [Key]
         public int ItemID { get; set; }
         public string AssignedToID { get; set; }
         public string AssignedToName { get; set; }
         public string Description { get; set; }
         public WorkItemStatus Status { get; set; }
     }
   
     offentliga enum WorkItemStatus {
   
         Open,
         Investigating,
         Resolved,
         Closed
     }
2. Skapa hello projektet toomake din nya modell tillgänglig toohello scaffold-teknik-logiken i Visual Studio.
3. Lägg till ett nytt autogenererade objekt `WorkItemsController` toohello ~\Controllers mapp (Högerklicka på **domänkontrollanter**, peka för**Lägg till**, och välj **nytt autogenererade objekt**). 
4. Välj **MVC 5 styrenhet med vyer, med hjälp av Entity Framework** och på **Lägg till**.
5. Välj hello-modell som du skapade, klicka på ** + ** och sedan **Lägg till** tooadd datakontexten, och klicka sedan på **Lägg till**.
   
   ![](./media/web-sites-dotnet-lob-application-azure-ad/16-add-scaffolded-controller.png)
6. Hitta hello i ~\Views\WorkItems\Create.cshtml (en automatiskt autogenererade artikel) `Html.BeginForm` hjälpmetod och att Hej följande markerade ändringar:  
   
   <pre class="prettyprint">
   @model WebApplication1.Models.WorkItem
   
   @{
    ViewBag.Title = &quot;Create&quot;;
   }
   
   &lt;h2&gt;Create&lt;/h2&gt;
   
   @using (Html.BeginForm(<mark>&quot;Create&quot;, &quot;WorkItems&quot;, FormMethod.Post, new { id = &quot;main-form&quot; }</mark>)) 
   {
    @Html.AntiForgeryToken()
   
    &lt;div class=&quot;form-horizontal&quot;&gt;
        &lt;h4&gt;WorkItem&lt;/h4&gt;
        &lt;hr /&gt;
        @Html.ValidationSummary(true, &quot;&quot;, new { @class = &quot;text-danger&quot; })
        &lt;div class=&quot;form-group&quot;&gt;
            @Html.LabelFor(model =&gt; model.AssignedToID, htmlAttributes: new { @class = &quot;control-label col-md-2&quot; })
            &lt;div class=&quot;col-md-10&quot;&gt;
                @Html.EditorFor(model =&gt; model.AssignedToID, new { htmlAttributes = new { @class = &quot;form-control&quot;<mark>, @type = &quot;hidden&quot;</mark> } })
                @Html.ValidationMessageFor(model =&gt; model.AssignedToID, &quot;&quot;, new { @class = &quot;text-danger&quot; })
            &lt;/div&gt;
        &lt;/div&gt;
   
        &lt;div class=&quot;form-group&quot;&gt;
            @Html.LabelFor(model =&gt; model.AssignedToName, htmlAttributes: new { @class = &quot;control-label col-md-2&quot; })
            &lt;div class=&quot;col-md-10&quot;&gt;
                @Html.EditorFor(model =&gt; model.AssignedToName, new { htmlAttributes = new { @class = &quot;form-control&quot; } })
                @Html.ValidationMessageFor(model =&gt; model.AssignedToName, &quot;&quot;, new { @class = &quot;text-danger&quot; })
            &lt;/div&gt;
        &lt;/div&gt;
   
        &lt;div class=&quot;form-group&quot;&gt;
            @Html.LabelFor(model =&gt; model.Description, htmlAttributes: new { @class = &quot;control-label col-md-2&quot; })
            &lt;div class=&quot;col-md-10&quot;&gt;
                @Html.EditorFor(model =&gt; model.Description, new { htmlAttributes = new { @class = &quot;form-control&quot; } })
                @Html.ValidationMessageFor(model =&gt; model.Description, &quot;&quot;, new { @class = &quot;text-danger&quot; })
            &lt;/div&gt;
        &lt;/div&gt;
   
        &lt;div class=&quot;form-group&quot;&gt;
            @Html.LabelFor(model =&gt; model.Status, htmlAttributes: new { @class = &quot;control-label col-md-2&quot; })
            &lt;div class=&quot;col-md-10&quot;&gt;
                @Html.EnumDropDownListFor(model =&gt; model.Status, htmlAttributes: new { @class = &quot;form-control&quot; })
                @Html.ValidationMessageFor(model =&gt; model.Status, &quot;&quot;, new { @class = &quot;text-danger&quot; })
            &lt;/div&gt;
        &lt;/div&gt;
   
        &lt;div class=&quot;form-group&quot;&gt;
            &lt;div class=&quot;col-md-offset-2 col-md-10&quot;&gt;
                &lt;input type=&quot;submit&quot; value=&quot;Create&quot; class=&quot;btn btn-default&quot;<mark> id=&quot;submit-button&quot;</mark> /&gt;
            &lt;/div&gt;
        &lt;/div&gt;
    &lt;/div&gt;
   }
   
   &lt;div&gt;
    @Html.ActionLink(&quot;Back tooList&quot;, &quot;Index&quot;)
   &lt;/div&gt;
   
   @section Scripts {
    @Scripts.Render(&quot;~/bundles/jqueryval&quot;)
    <mark>&lt;script&gt;
        // People/Group Picker Code
        var maxResultsPerPage = 14;
        var input = document.getElementById(&quot;AssignedToName&quot;);
   
        // Access token from request header, and tenantID from claims identity
        var token = &quot;@Request.Headers[&quot;X-MS-TOKEN-AAD-ACCESS-TOKEN&quot;]&quot;;
        var tenant =&quot;@(System.Security.Claims.ClaimsPrincipal.Current.Claims
                        .Where(c => c.Type == &quot;http://schemas.microsoft.com/identity/claims/tenantid&quot;)
                        .Select(c => c.Value).SingleOrDefault())&quot;;
   
        var picker = new AadPicker(maxResultsPerPage, input, token, tenant);
   
        // Submit hello selected user/group toobe asssigned.
        $(&quot;#submit-button&quot;).click({ picker: picker }, function () {
            if (!picker.Selected())
                return;
            $(&quot;#main-form&quot;).get()[0].elements[&quot;AssignedToID&quot;].value = picker.Selected().objectId;
        });
    &lt;/script&gt;</mark>
   }
   </pre>
   
   Observera att `token` och `tenant` används av hello `AadPicker` objektet toomake Azure Active Directory Graph API-anrop. Du lägger till `AadPicker` senare.     
   
   > [!NOTE]
   > Du får bara samt `token` och `tenant` från hello på klientsidan med `~/.auth/me`, men som skulle vara en ytterligare server-anrop. Exempel:
   > 
   > $.ajax ({dataType: ”json”, url: ”/.auth/me”, lyckade: funktionen (data) {var token = data [0] .access_token; var innehavare = data [0] .user_claims .find (c = > c.typ === 'http://schemas.microsoft.com/identity/claims/tenantid') .val;}});
   > 
   > 
7. Ändra hello samma med ~ \Views\WorkItems\Edit.cshtml.
8. Hej `AadPicker` objekt definieras i ett skript som du behöver tooadd tooyour projekt. Högerklicka på hello ~\Scripts mappen, peka för**Lägg till**, och klicka på **JavaScript-fil**. Typen `AadPickerLibrary` för hello filnamn och klickar på **OK**.
9. Kopiera hello innehåll från [här](https://raw.githubusercontent.com/cephalin/active-directory-dotnet-webapp-roleclaims/master/WebApp-RoleClaims-DotNet/Scripts/AadPickerLibrary.js) i ~ \Scripts\AadPickerLibrary.js.
   
   I hello skript hello `AadPicker` objekt anrop [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) toosearch för användare och grupper som matchar hello indata.  
10. ~\Scripts\AadPickerLibrary.js använder också hello [jQuery UI Autocomplete widget](https://jqueryui.com/autocomplete/). Du behöver tooadd jQuery UI tooyour projekt. Högerklicka på projektet i och klickar på **hantera NuGet-paket**.
11. I hello NuGet-Pakethanteraren, klickar du på Bläddra, typen **jquery ui-** i hello sökfältet och klickar på **jQuery.UI.Combined**.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/17-add-jquery-ui-nuget.png)
12. I hello högra rutan, klickar du på **installera**, klicka på **OK** tooproceed.
13. Öppna ~\App_Start\BundleConfig.cs och att Hej följande markerade ändringar:  
    
    <pre class="prettyprint">
    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle(&quot;~/bundles/jquery&quot;).Include(
                    &quot;~/Scripts/jquery-{version}.js&quot;<mark>,
                    &quot;~/Scripts/jquery-ui-{version}.js&quot;,
                    &quot;~/Scripts/AadPickerLibrary.js&quot;</mark>));
    
        bundles.Add(new ScriptBundle(&quot;~/bundles/jqueryval&quot;).Include(
                    &quot;~/Scripts/jquery.validate*&quot;));
    
        // Use hello development version of Modernizr toodevelop with and learn from. Then, when you&#39;re
        // ready for production, use hello build tool at http://modernizr.com toopick only hello tests you need.
        bundles.Add(new ScriptBundle(&quot;~/bundles/modernizr&quot;).Include(
                    &quot;~/Scripts/modernizr-*&quot;));
    
        bundles.Add(new ScriptBundle(&quot;~/bundles/bootstrap&quot;).Include(
                    &quot;~/Scripts/bootstrap.js&quot;,
                    &quot;~/Scripts/respond.js&quot;));
    
        bundles.Add(new StyleBundle(&quot;~/Content/css&quot;).Include(
                    &quot;~/Content/bootstrap.css&quot;,
                    &quot;~/Content/site.css&quot;<mark>,
                    &quot;~/Content/themes/base/jquery-ui.css&quot;</mark>));
    }
    </pre>
    
    Det finns flera performant sätt toomanage JavaScript och CSS-filer i din app. Men för enkelhetens skull ska bara toopiggyback på hello-paket som har lästs in med varje vy.
14. Slutligen i ~ \Global.asax, Lägg till följande kodrad i hello hello `Application_Start()` metod. `Ctrl`+`.`på varje namngivning matchningsfel för åtgärdas.
    
        AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.NameIdentifier;
    
    > [!NOTE]
    > Du behöver följande kodrad eftersom hello standard MVC-mallen använder <code>[ValidateAntiForgeryToken]</code> decoration på vissa hello åtgärder. På grund av toohello problemet som beskrivs av [Brock Allen](https://twitter.com/BrockLAllen) på [MVC 4, AntiForgeryToken och anspråk](http://brockallen.com/2012/07/08/mvc-4-antiforgerytoken-and-claims/) HTTP POST kan misslyckas skydd mot förfalskning token verifieringen eftersom:
    > 
    > * Azure Active Directory skickar inte hello http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider, vilket krävs normalt av hello antiförfalskningstoken.
    > * Om Azure Active Directory är directory synkroniseras med AD FS, skickar hello AD FS-förtroende som standard inte hello http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider anspråk, även om du kan konfigurera AD FS toosend manuellt Denna begäran.
    > 
    > `ClaimTypes.NameIdentifies`Anger hello anspråk `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`, som tillhandahåller Azure Active Directory.  
    > 
    > 
15. Nu kan publicera dina ändringar. Högerklicka på projektet och klicka på **publicera**.
16. Klicka på **inställningar**, kontrollera att det finns en anslutning sträng tooyour SQL-databasen, väljer **Update Database** toomake hello schemaändringar för din modell och på **publicera** .
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/18-publish-crud-changes.png)
17. I hello webbläsare navigerar toohttps: / /&lt;*appname*>.azurewebsites.net/workitems och klicka på **Skapa nytt**.
18. Klicka i hello **AssignedToName** rutan. Du bör nu se användare och grupper från din Azure Active Directory-klient i en listruta. Du kan ange toofilter eller använda hello `Up` eller `Down` nyckel eller klicka på tooselect hello användare eller grupp. 
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/19-use-aadpicker.png)
19. Klicka på **skapa** toosave hello ändringar. Klicka på **redigera** på hello skapade arbete objektet tooobserve hello samma beteende.

Congrats, nu körs en av branschspecifika app i Azure med katalogåtkomst! Det finns mycket mer du kan göra med hello Graph API. Se [Azure AD Graph API-referens för](https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog).

<a name="next"></a>

## <a name="next-step"></a>Nästa steg
Om du behöver rollbaserad åtkomstkontroll (RBAC) för din line-of-business-app i azure finns [WebApp-RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) ett exempel från hello Azure Active Directory-teamet. Den visar hur tooenable roller för ditt Azure Active Directory-program och auktorisera användare med hello `[Authorize]` decoration.

Om din line-of-business-app behöver komma åt tooon lokala data, se [åtkomst till lokala resurser genom att använda hybridanslutningar i Azure App Service](web-sites-hybrid-connection-get-started.md).

<a name="bkmk_resources"></a>

## <a name="further-resources"></a>Ytterligare resurser
* [Autentisering och auktorisering i Azure App Service](../app-service/app-service-authentication-overview.md)
* [Autentisera med lokala Active Directory i din Azure-app](web-sites-authentication-authorization.md)
* [Skapa en line-of-business-app i Azure med AD FS-autentisering](web-sites-dotnet-lob-application-adfs.md)
* [App Service Auth och hello Azure AD Graph API](https://cgillum.tech/2016/03/25/app-service-auth-aad-graph-api/)
* [Microsoft Azure Active Directory-exempel och dokumentation](https://github.com/AzureADSamples)
* [Azure Active Directory-Token och anspråkstyper som stöds](http://msdn.microsoft.com/library/azure/dn195587.aspx)
