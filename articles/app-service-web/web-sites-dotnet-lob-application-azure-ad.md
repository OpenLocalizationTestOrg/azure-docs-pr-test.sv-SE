---
title: Skapa en Azure line-of-business-app med Azure Active Directory-autentisering | Microsoft Docs
description: "Lär dig att skapa en ASP.NET MVC-line-of-business-app i Azure App Service som autentiserar med Azure Active Directory"
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
ms.openlocfilehash: 6eadf0a521a32c5bc580908e4e4b7f4305e2bf7e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-line-of-business-azure-app-with-azure-active-directory-authentication"></a>Skapa en Azure line-of-business-app med Azure Active Directory-autentisering
Den här artikeln visar hur du skapar en .NET-line-of-business-app i [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) med hjälp av den [autentisering / auktorisering](../app-service/app-service-authentication-overview.md) funktion. Visar även hur du använder den [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) till frågan katalogdata i programmet.

Azure Active Directory-klient som du använder kan vara en endast Azure-katalog. Eller så kan det vara [synkroniserats med din lokala Active Directory](../active-directory/active-directory-aadconnect.md) att skapa en enkel inloggning för anställda som är lokala och fjärranslutna. Den här artikeln använder standardkatalog för ditt Azure-konto.

<a name="bkmk_build"></a>

## <a name="what-you-will-build"></a>Vad du ska skapa
Du skapar ett enkelt line-of-business skapa-Läs-Uppdatera-ta bort CRUD-program i App Service Web Apps att spårar arbetsobjekt med följande funktioner:

* Autentiserar användare mot Azure Active Directory
* Frågar directory-användare och grupper med [Azure Active Directory Graph API](http://msdn.microsoft.com/library/azure/hh974476.aspx)
* Använda ASP.NET MVC *ingen autentisering* mall

Om du behöver rollbaserad åtkomstkontroll (RBAC) för din line-of-business-app i Azure finns [nästa steg](#next).

<a name="bkmk_need"></a>

## <a name="what-you-need"></a>Vad du behöver
[!INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

Du behöver följande för att slutföra den här självstudiekursen:

* En Azure Active Directory-klient med användare i olika grupper
* Behörighet att skapa program på Azure Active Directory-klient
* Visual Studio 2013 Update 4 eller senare
* [Azure SDK 2.8.1 eller senare](https://azure.microsoft.com/downloads/)

<a name="bkmk_deploy"></a>

## <a name="create-and-deploy-a-web-app-to-azure"></a>Skapa och distribuera en webbapp till Azure
1. I Visual Studio klickar du på **filen** > **ny** > **projekt**.
2. Välj **ASP.NET-webbprogram**, namnge projektet och klicka på **OK**.
3. Välj den **MVC** mall, ändra autentisering till **ingen autentisering**. Kontrollera att **värd i molnet** är markerad och klicka på **OK**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/1-create-mvc-no-authentication.png)
4. I den **skapa Apptjänst** dialogrutan klickar du på **Lägg till ett konto** (och sedan **Lägg till ett konto** i listrutan) att logga in på ditt Azure-konto.
5. När du loggade in konfigurera ditt webbprogram. Skapa en resursgrupp och en ny programtjänstplan genom att klicka på respektive **ny** knappen. Klicka på **utforska ytterligare Azure-tjänster** att fortsätta.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/2-create-app-service.png)
6. I den **Services** klickar du på  **+**  att lägga till en SQL-databas för din app. 
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/3-add-sql-database.png)
7. I **Konfigurera SQL-databas**, klickar du på **ny** att skapa en SQL Server-instans.
8. I **Konfigurera SQL Server**, konfigurera din SQL Server-instans. Klicka på **OK**, **OK**, och **skapa** till startar skapandet en app i Azure.
9. I **aktivitet för Azure App Service**, du kan se när skapandet en app är klar. Klicka på  **publicera &lt;* appname*> till det här webbprogrammet nu ** och klicka sedan på **publicera**. 
   
    När Visual Studio har slutförts öppnar publicera appen i webbläsaren. 
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/4-published-shown-in-browser.png)

<a name="bkmk_auth"></a>

## <a name="configure-authentication-and-directory-access"></a>Konfigurera autentisering och åtkomst
1. Logga in på [Azure-portalen](https://portal.azure.com).
2. I den vänstra menyn klickar du på **Apptjänster** > **&lt;*appname*> ** > **autentisering / auktorisering**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/5-app-service-authentication.png)
3. Aktivera Azure Active Directory-autentisering genom att klicka **på** > **Azure Active Directory** > **Express**  >   **OK**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/6-authentication-express.png)
4. Klicka på **spara** i kommandofältet.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/7-authentication-save.png)
   
    När autentiseringsinställningarna sparades försök att gå till din app igen i webbläsaren. Standardinställningarna för att autentisering används för hela appen. Om du inte redan har loggat in omdirigeras till en inloggningsskärm. Efter loggat in kan se du din app skyddas av HTTPS. Därefter måste du aktivera åtkomst till katalogdata. 
5. Navigera till den [klassiska portalen](https://manage.windowsazure.com).
6. I den vänstra menyn klickar du på **Active Directory** > **standardkatalog** > **program**  >   **&lt;* appname*> **.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/8-find-aad-application.png)
   
    Detta är det Azure Active Directory-program som Apptjänst skapas för dig att tillståndet / autentisering.
7. Klicka på **användare** och **grupper** se till att du har vissa användare och grupper i katalogen. Om inte, skapa några testa användare och grupper.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/9-create-users-groups.png)
8. Klicka på **konfigurera** konfigurera programmet.
9. Rulla ned till den **nycklar** och lägger till en nyckel genom att välja en varaktighet. Klicka på **delegerade behörigheter** och välj **läsa katalogdata**. 
   Klicka på **Spara**.
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/10-configure-aad-application.png)
10. När dina inställningar sparas Rulla tillbaka till den **nycklar** avsnittet och klicka på den **kopiera** för att kopiera klientnyckeln för. 
    
     ![](./media/web-sites-dotnet-lob-application-azure-ad/11-get-app-key.png)
    
    > [!IMPORTANT]
    > Om du navigerar bort från den här sidan nu kan du inte komma åt den här klientnyckeln aldrig igen.
    > 
    > 
11. Därefter måste du konfigurera ditt webbprogram med den här nyckeln. Logga in på den [resursutforskaren Azure](https://resources.azure.com) med ditt Azure-konto.
12. Överst på sidan, klickar du på **läsning och skrivning** att göra ändringar i resursutforskaren i Azure.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/12-resource-manager-writable.png)
13. Hitta autentiseringsinställningarna för din app på prenumerationer >  **&lt;* subscriptionname*> ** > **resursgrupper**  >   **&lt;* resourcegroupname*> ** > **providers** > **Microsoft.Web**  >  **platser** > **&lt;*appname*> ** > **config**  >  **authsettings**.
14. Klicka på **Redigera**.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/13-edit-authsettings.png)
15. I fönstret Redigera ange den `clientSecret` och `additionalLoginParams` egenskaper på följande sätt.
    
        ...
        "clientSecret": "<client key from the Azure Active Directory application>",
        ...
        "additionalLoginParams": ["response_type=code id_token", "resource=https://graph.windows.net"],
        ...
16. Klicka på **placera** längst upp för att skicka dina ändringar.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/14-edit-parameters.png)
17. Nu för att testa om du har den autentiseringstoken åtkomst till Azure Active Directory Graph API bara gå till  **https://&lt;*appname*>.azurewebsites.net/.auth/me** i webbläsaren . Om du har konfigurerat allt korrekt, bör du se den `access_token` egenskap i JSON-svar.
    
    Den `~/.auth/me` URL-sökväg som hanteras av App Service autentisering / auktorisering för att ge dig informationen som rör din autentiserad session. Mer information finns i [autentisering och auktorisering i Azure App Service](../app-service/app-service-authentication-overview.md).
    
    > [!NOTE]
    > Den `access_token` har en utgångsperiod. Dock App Service autentisering / auktorisering tillhandahåller token uppdateringsfunktionen med `~/.auth/refresh`. Läs mer om hur du använder den [App Service-Token Store](https://cgillum.tech/2016/03/07/app-service-token-store/).
    > 
    > 

Därefter kan du göra något användbart med katalogdata.

<a name="bkmk_crud"></a>

## <a name="add-line-of-business-functionality-to-your-app"></a>Lägg till line-of-business-funktionerna i appen
Nu kan skapa du en enkla CRUD arbete objekt spåraren.  

1. Skapa en klassfil som heter WorkItem.cs i mappen ~\Models och Ersätt `public class WorkItem {...}` med följande kod:
   
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
2. Skapa projektet för att göra din nya modell tillgänglig för scaffold-teknik för logiken i Visual Studio.
3. Lägg till ett nytt autogenererade objekt `WorkItemsController` till mappen ~\Controllers (Högerklicka på **domänkontrollanter**, peka på **Lägg till**, och välj **nytt autogenererade objekt**). 
4. Välj **MVC 5 styrenhet med vyer, med hjälp av Entity Framework** och på **Lägg till**.
5. Markera den modell som du skapade och klickar sedan  **+**  och sedan **Lägg till** att lägga till en datakontext och klicka sedan på **Lägg till**.
   
   ![](./media/web-sites-dotnet-lob-application-azure-ad/16-add-scaffolded-controller.png)
6. Hitta i ~\Views\WorkItems\Create.cshtml (ett automatiskt autogenererade objekt), den `Html.BeginForm` hjälpmetod och gör följande markerade ändringar:  
   
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
    @Html.ActionLink(&quot;Back to List&quot;, &quot;Index&quot;)
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
   
        // Submit the selected user/group to be asssigned.
        $(&quot;#submit-button&quot;).click({ picker: picker }, function () {
            if (!picker.Selected())
                return;
            $(&quot;#main-form&quot;).get()[0].elements[&quot;AssignedToID&quot;].value = picker.Selected().objectId;
        });
    &lt;/script&gt;</mark>
   }
   </pre>
   
   Observera att `token` och `tenant` används av den `AadPicker` objekt till Azure Active Directory Graph API-anrop. Du lägger till `AadPicker` senare.     
   
   > [!NOTE]
   > Du får bara samt `token` och `tenant` från klienten med `~/.auth/me`, men som skulle vara en ytterligare server-anrop. Exempel:
   > 
   > $.ajax ({dataType: ”json”, url: ”/.auth/me”, lyckade: funktionen (data) {var token = data [0] .access_token; var innehavare = data [0] .user_claims .find (c = > c.typ === 'http://schemas.microsoft.com/identity/claims/tenantid') .val;}});
   > 
   > 
7. Göra samma ändringar med ~ \Views\WorkItems\Edit.cshtml.
8. Den `AadPicker` objekt definieras i ett skript som du behöver lägga till i projektet. Högerklicka på mappen ~\Scripts, peka på **Lägg till**, och klicka på **JavaScript-fil**. Typen `AadPickerLibrary` för filnamn och klickar på **OK**.
9. Kopiera innehållet från [här](https://raw.githubusercontent.com/cephalin/active-directory-dotnet-webapp-roleclaims/master/WebApp-RoleClaims-DotNet/Scripts/AadPickerLibrary.js) i ~ \Scripts\AadPickerLibrary.js.
   
   I skriptet på `AadPicker` objekt anrop [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) att söka efter användare och grupper som matchar angivna indata.  
10. ~\Scripts\AadPickerLibrary.js använder också den [jQuery UI Autocomplete widget](https://jqueryui.com/autocomplete/). Så behöver du lägga till jQuery UI i projektet. Högerklicka på projektet i och klickar på **hantera NuGet-paket**.
11. I NuGet Package Manager klickar du på Bläddra, typen **jquery ui-** i sökfältet och klicka på **jQuery.UI.Combined**.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/17-add-jquery-ui-nuget.png)
12. I den högra rutan, klickar du på **installera**, klicka på **OK** att fortsätta.
13. Öppna ~\App_Start\BundleConfig.cs och gör följande markerade ändringar:  
    
    <pre class="prettyprint">
    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle(&quot;~/bundles/jquery&quot;).Include(
                    &quot;~/Scripts/jquery-{version}.js&quot;<mark>,
                    &quot;~/Scripts/jquery-ui-{version}.js&quot;,
                    &quot;~/Scripts/AadPickerLibrary.js&quot;</mark>));
    
        bundles.Add(new ScriptBundle(&quot;~/bundles/jqueryval&quot;).Include(
                    &quot;~/Scripts/jquery.validate*&quot;));
    
        // Use the development version of Modernizr to develop with and learn from. Then, when you&#39;re
        // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
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
    
    Det finns flera performant sätt att hantera JavaScript och CSS-filer i din app. För enkelhetens skull ska du bara dock andra datorn på de paket som har lästs in med varje vy.
14. Slutligen i ~ \Global.asax, Lägg till följande kodrad i den `Application_Start()` metoden. `Ctrl`+`.`på varje namngivnings-matchningsfel kan åtgärdas.
    
        AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.NameIdentifier;
    
    > [!NOTE]
    > Du behöver följande kodrad eftersom standard MVC-mallen använder <code>[ValidateAntiForgeryToken]</code> decoration på några åtgärder. På grund av det beteende som beskrivs av [Brock Allen](https://twitter.com/BrockLAllen) på [MVC 4, AntiForgeryToken och anspråk](http://brockallen.com/2012/07/08/mvc-4-antiforgerytoken-and-claims/) HTTP POST kan misslyckas skydd mot förfalskning token verifieringen eftersom:
    > 
    > * Azure Active Directory skickar inte http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider, vilket krävs normalt av antiförfalskningstoken.
    > * Om Azure Active Directory är directory synkroniseras med AD FS, skickar AD FS-förtroende som standard inte http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider anspråket, även om du kan konfigurera AD FS för att skicka den här manuellt anspråk.
    > 
    > `ClaimTypes.NameIdentifies`Anger anspråket `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`, som tillhandahåller Azure Active Directory.  
    > 
    > 
15. Nu kan publicera dina ändringar. Högerklicka på projektet och klicka på **publicera**.
16. Klicka på **inställningar**, kontrollera att det finns en anslutningssträng i SQL-databasen, väljer **Update Database** att göra schemaändringar för din modell och på **publicera**.
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/18-publish-crud-changes.png)
17. I webbläsaren, navigera till https://&lt;*appname*>.azurewebsites.net/workitems och klicka på **Skapa nytt**.
18. Klicka i den **AssignedToName** rutan. Du bör nu se användare och grupper från din Azure Active Directory-klient i en listruta. Du kan ange om du vill filtrera eller använda den `Up` eller `Down` nyckel eller Markera användare eller grupp. 
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/19-use-aadpicker.png)
19. Klicka på **skapa** spara ändringarna. Klicka på **redigera** på skapade arbetsobjektet vill se samma beteende.

Congrats, nu körs en av branschspecifika app i Azure med katalogåtkomst! Det finns mycket mer du kan göra med Graph API. Se [Azure AD Graph API-referens för](https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog).

<a name="next"></a>

## <a name="next-step"></a>Nästa steg
Om du behöver rollbaserad åtkomstkontroll (RBAC) för din line-of-business-app i azure finns [WebApp-RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) ett exempel från Azure Active Directory-teamet. Den visar hur du aktiverar roller för programmet Azure Active Directory och auktorisera användare med den `[Authorize]` decoration.

Om din line-of-business-program behöver tillgång till lokala data, se [åtkomst till lokala resurser genom att använda hybridanslutningar i Azure App Service](web-sites-hybrid-connection-get-started.md).

<a name="bkmk_resources"></a>

## <a name="further-resources"></a>Ytterligare resurser
* [Autentisering och auktorisering i Azure App Service](../app-service/app-service-authentication-overview.md)
* [Autentisera med lokala Active Directory i din Azure-app](web-sites-authentication-authorization.md)
* [Skapa en line-of-business-app i Azure med AD FS-autentisering](web-sites-dotnet-lob-application-adfs.md)
* [Skriv Apptjänst och Azure AD Graph API](https://cgillum.tech/2016/03/25/app-service-auth-aad-graph-api/)
* [Microsoft Azure Active Directory-exempel och dokumentation](https://github.com/AzureADSamples)
* [Azure Active Directory-Token och anspråkstyper som stöds](http://msdn.microsoft.com/library/azure/dn195587.aspx)
