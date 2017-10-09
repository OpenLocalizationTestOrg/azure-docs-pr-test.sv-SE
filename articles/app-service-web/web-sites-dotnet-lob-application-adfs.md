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
# <a name="create-a-line-of-business-azure-app-with-ad-fs-authentication"></a><span data-ttu-id="a2bb5-104">Skapa en Azure line-of-business-program med AD FS-autentisering</span><span class="sxs-lookup"><span data-stu-id="a2bb5-104">Create a line-of-business Azure app with AD FS authentication</span></span>
<span data-ttu-id="a2bb5-105">Den här artikeln beskrivs hur du toocreate en ASP.NET MVC line-of-business-program i [Azure App Service](../app-service/app-service-value-prop-what-is.md) med hjälp av en lokal [Active Directory Federation Services](http://technet.microsoft.com/library/hh831502.aspx) som hello identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-105">This article shows you how toocreate an ASP.NET MVC line-of-business application in [Azure App Service](../app-service/app-service-value-prop-what-is.md) using an on-premises [Active Directory Federation Services](http://technet.microsoft.com/library/hh831502.aspx) as hello identity provider.</span></span> <span data-ttu-id="a2bb5-106">Det här scenariot kan arbeta när du vill toocreate line-of-business-program i Azure App Service, men din organisation kräver directory data toobe lagras på plats.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-106">This scenario can work when you want toocreate line-of-business applications in Azure App Service but your organization requires directory data toobe stored on-site.</span></span>

> [!NOTE]
> <span data-ttu-id="a2bb5-107">En översikt över hello olika enterprise autentisering och auktorisering alternativ för Azure App Service finns [autentisera med lokala Active Directory i din Azure-app](web-sites-authentication-authorization.md).</span><span class="sxs-lookup"><span data-stu-id="a2bb5-107">For an overview of hello different enterprise authentication and authorization options for Azure App Service, see [Authenticate with on-premises Active Directory in your Azure app](web-sites-authentication-authorization.md).</span></span>
> 
> 

<a name="bkmk_build"></a>

## <a name="what-you-will-build"></a><span data-ttu-id="a2bb5-108">Vad du ska skapa</span><span class="sxs-lookup"><span data-stu-id="a2bb5-108">What you will build</span></span>
<span data-ttu-id="a2bb5-109">Du skapar en grundläggande ASP.NET-program i Azure App Service Web Apps med hello följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="a2bb5-109">You will build a basic ASP.NET application in Azure App Service Web Apps with hello following features:</span></span>

* <span data-ttu-id="a2bb5-110">Autentiserar användare mot AD FS</span><span class="sxs-lookup"><span data-stu-id="a2bb5-110">Authenticates users against AD FS</span></span>
* <span data-ttu-id="a2bb5-111">Använder `[Authorize]` tooauthorize användare för olika åtgärder</span><span class="sxs-lookup"><span data-stu-id="a2bb5-111">Uses `[Authorize]` tooauthorize users for different actions</span></span>
* <span data-ttu-id="a2bb5-112">Statisk konfiguration för både felsökning i Visual Studio och publicera tooApp Service Web Apps (konfigurera en gång, felsöka och publicera när som helst)</span><span class="sxs-lookup"><span data-stu-id="a2bb5-112">Static configuration for both debugging in Visual Studio and publishing tooApp Service Web Apps (configure once, debug and publish anytime)</span></span>  

<a name="bkmk_need"></a>

## <a name="what-you-need"></a><span data-ttu-id="a2bb5-113">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="a2bb5-113">What you need</span></span>
[!INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

<span data-ttu-id="a2bb5-114">Den här kursen behöver hello följande toocomplete:</span><span class="sxs-lookup"><span data-stu-id="a2bb5-114">You need hello following toocomplete this tutorial:</span></span>

* <span data-ttu-id="a2bb5-115">En lokal AD FS-distribution (slutpunkt till slutpunkt-genomgång av hello-testlabb som används i den här kursen finns [testlabbet: fristående STS med AD FS i Azure VM (för test)](https://blogs.msdn.microsoft.com/cephalin/2014/12/21/test-lab-standalone-sts-with-ad-fs-in-azure-vm-for-test-only/))</span><span class="sxs-lookup"><span data-stu-id="a2bb5-115">An on-premises AD FS deployment (for an end-to-end walkthrough of hello test lab used in this tutorial, see [Test Lab: Standalone STS with AD FS in Azure VM (for test only)](https://blogs.msdn.microsoft.com/cephalin/2014/12/21/test-lab-standalone-sts-with-ad-fs-in-azure-vm-for-test-only/))</span></span>
* <span data-ttu-id="a2bb5-116">Behörigheter toocreate förlitande part förtroende i AD FS-hantering</span><span class="sxs-lookup"><span data-stu-id="a2bb5-116">Permissions toocreate relying party trusts in AD FS Management</span></span>
* <span data-ttu-id="a2bb5-117">Visual Studio 2013 Update 4 eller senare</span><span class="sxs-lookup"><span data-stu-id="a2bb5-117">Visual Studio 2013 Update 4 or later</span></span>
* <span data-ttu-id="a2bb5-118">[Azure SDK 2.8.1](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) eller senare</span><span class="sxs-lookup"><span data-stu-id="a2bb5-118">[Azure SDK 2.8.1](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) or later</span></span>

<a name="bkmk_sample"></a>

## <a name="use-sample-application-for-line-of-business-template"></a><span data-ttu-id="a2bb5-119">Använda exempelprogram för line-of-business-mall</span><span class="sxs-lookup"><span data-stu-id="a2bb5-119">Use sample application for line-of-business template</span></span>
<span data-ttu-id="a2bb5-120">Hej exempelprogrammet i den här självstudiekursen [WebApp-WSFederation-DotNet)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet), skapas av hello Azure Active Directory-teamet.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-120">hello sample application in this tutorial, [WebApp-WSFederation-DotNet)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet), is created by hello Azure Active Directory team.</span></span> <span data-ttu-id="a2bb5-121">Eftersom AD FS stöder WS-Federation, kan du använda den som en mall toocreate line-of-business-program utan problem.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-121">Since AD FS supports WS-Federation, you can use it as a template toocreate line-of-business applications with ease.</span></span> <span data-ttu-id="a2bb5-122">Det har hello följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="a2bb5-122">It has hello following features:</span></span>

* <span data-ttu-id="a2bb5-123">Använder [WS-Federation](http://msdn.microsoft.com/library/bb498017.aspx) tooauthenticate med en lokal AD FS-distribution</span><span class="sxs-lookup"><span data-stu-id="a2bb5-123">Uses [WS-Federation](http://msdn.microsoft.com/library/bb498017.aspx) tooauthenticate with an on-premises AD FS deployment</span></span>
* <span data-ttu-id="a2bb5-124">Funktioner för inloggning och utloggning</span><span class="sxs-lookup"><span data-stu-id="a2bb5-124">Sign-in and sign-out functionality</span></span>
* <span data-ttu-id="a2bb5-125">Använder [Microsoft.Owin](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana) (i stället för Windows Identity Foundation), vilket är hello framtida av ASP.NET och mycket enklare tooset för autentisering och auktorisering än WIF</span><span class="sxs-lookup"><span data-stu-id="a2bb5-125">Uses [Microsoft.Owin](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana) (instead of Windows Identity Foundation), which is hello future of ASP.NET and much simpler tooset up for authentication and authorization than WIF</span></span>

<a name="bkmk_setup"></a>

## <a name="set-up-hello-sample-application"></a><span data-ttu-id="a2bb5-126">Ställ in hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="a2bb5-126">Set up hello sample application</span></span>
1. <span data-ttu-id="a2bb5-127">Klona eller hämta hello exempellösning på [WebApp-WSFederation-DotNet](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) tooyour lokal katalog.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-127">Clone or download hello sample solution at [WebApp-WSFederation-DotNet](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) tooyour local directory.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="a2bb5-128">Hej instruktionerna på [README.md](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet/blob/master/README.md) visar hur tooset hello ansökan med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-128">hello instructions at [README.md](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet/blob/master/README.md) show you how tooset up hello application with Azure Active Directory.</span></span> <span data-ttu-id="a2bb5-129">Men i den här självstudiekursen kommer du skapat med AD FS, så hello gör så här i stället.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-129">But in this tutorial, you set it up with AD FS, so follow hello steps here instead.</span></span>
   > 
   > 
2. <span data-ttu-id="a2bb5-130">Öppna hello lösningen och öppna Controllers\AccountController.cs i hello **Solution Explorer**.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-130">Open hello solution, and then open Controllers\AccountController.cs in hello **Solution Explorer**.</span></span>
   
   <span data-ttu-id="a2bb5-131">Du ser att hello kod bara utfärdar en autentisering challenge tooauthenticate hello-användare med hjälp av WS-Federation.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-131">You will see that hello code simply issues an authentication challenge tooauthenticate hello user using WS-Federation.</span></span> <span data-ttu-id="a2bb5-132">Alla autentisering har konfigurerats i App_Start\Startup.Auth.cs.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-132">All authentication is configured in App_Start\Startup.Auth.cs.</span></span>
3. <span data-ttu-id="a2bb5-133">Öppna App_Start\Startup.Auth.cs.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-133">Open App_Start\Startup.Auth.cs.</span></span> <span data-ttu-id="a2bb5-134">I hello `ConfigureAuth` metod, Observera hello rad:</span><span class="sxs-lookup"><span data-stu-id="a2bb5-134">In hello `ConfigureAuth` method, note hello line:</span></span>
   
       app.UseWsFederationAuthentication(
           new WsFederationAuthenticationOptions
           {
               Wtrealm = realm,
               MetadataAddress = metadata                                      
           });
   
   <span data-ttu-id="a2bb5-135">I OWIN hälsningsmeddelande är den här fragment verkligen hello utan minsta måste tooconfigure WS-Federation-autentisering.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-135">In hello OWIN world, this snippet is really hello bare minimum you need tooconfigure WS-Federation authentication.</span></span> <span data-ttu-id="a2bb5-136">Det är mycket enklare och mer elegant än WIF, där Web.config injekteras med XML över hela hello plats.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-136">It is much simpler and more elegant than WIF, where Web.config is injected with XML all over hello place.</span></span> <span data-ttu-id="a2bb5-137">Hej endast information du behöver är hello överförande partens (RP) identifierare och hello-URL för din AD FS-tjänsten metadatafil.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-137">hello only information you need is hello relying party's (RP) identifier and hello URL of your AD FS service's metadata file.</span></span> <span data-ttu-id="a2bb5-138">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="a2bb5-138">Here's an example:</span></span>
   
   * <span data-ttu-id="a2bb5-139">RP-ID:`https://contoso.com/MyLOBApp`</span><span class="sxs-lookup"><span data-stu-id="a2bb5-139">RP identifier: `https://contoso.com/MyLOBApp`</span></span>
   * <span data-ttu-id="a2bb5-140">Metadata-adress:`http://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`</span><span class="sxs-lookup"><span data-stu-id="a2bb5-140">Metadata address: `http://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`</span></span>
4. <span data-ttu-id="a2bb5-141">I App_Start\Startup.Auth.cs, ändrar du hello följande definitioner av statiska sträng:</span><span class="sxs-lookup"><span data-stu-id="a2bb5-141">In App_Start\Startup.Auth.cs, change hello following static string definitions:</span></span>  
   
   <pre class="prettyprint">
   private static string realm = ConfigurationManager.AppSettings["ida:<mark>RPIdentifier</mark>"];
   <mark><del>private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];</del></mark>
   <mark><del>private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];</del></mark>
   <mark><del>private static string metadata = string.Format("{0}/{1}/federationmetadata/2007-06/federationmetadata.xml", aadInstance, tenant);</del></mark>
   <mark>private static string metadata = string.Format("https://{0}/federationmetadata/2007-06/federationmetadata.xml", ConfigurationManager.AppSettings["ida:ADFS"]);</mark>
   
   <mark><del>string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);</del></mark>
   </pre>
5. <span data-ttu-id="a2bb5-142">Nu kan göra hello motsvarande ändringar i Web.config. Öppna hello Web.config och ändra hello följande app-inställningar:</span><span class="sxs-lookup"><span data-stu-id="a2bb5-142">Now, make hello corresponding changes in Web.config. Open hello Web.config and modify hello following app settings:</span></span>  
   
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
   
   <span data-ttu-id="a2bb5-143">Fyll i hello nyckelvärden baserat på respektive miljön.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-143">Fill in hello key values based on your respective environment.</span></span>
6. <span data-ttu-id="a2bb5-144">Skapa hello programmet toomake att det inte finns några fel.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-144">Build hello application toomake sure there are no errors.</span></span>

<span data-ttu-id="a2bb5-145">Det var allt.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-145">That's it.</span></span> <span data-ttu-id="a2bb5-146">Hello exempelprogrammet är nu redo toowork med AD FS.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-146">Now hello sample application is ready toowork with AD FS.</span></span> <span data-ttu-id="a2bb5-147">Du måste fortfarande tooconfigure ett RP förtroende med det här programmet i AD FS.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-147">You still need tooconfigure an RP trust with this application in AD FS later.</span></span>

<a name="bkmk_deploy"></a>

## <a name="deploy-hello-sample-application-tooazure-app-service-web-apps"></a><span data-ttu-id="a2bb5-148">Distribuera hello exempel programmet tooAzure App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="a2bb5-148">Deploy hello sample application tooAzure App Service Web Apps</span></span>
<span data-ttu-id="a2bb5-149">Här kan publicera hello programmet tooa webbapp i App Service Web Apps samtidigt som hello debug-miljö.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-149">Here, you publish hello application tooa web app in App Service Web Apps while preserving hello debug environment.</span></span> <span data-ttu-id="a2bb5-150">Observera att du ska toopublish hello programmet innan det finns ett RP förtroende med AD FS så autentisering inte fungerar ändå ännu.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-150">Note that you're going toopublish hello application before it has an RP trust with AD FS, so authentication still doesn't work yet.</span></span> <span data-ttu-id="a2bb5-151">Om du gör det kan nu du dock ha hello web app-URL som du kan använda tooconfigure hello RP förtroende senare.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-151">However, if you do it now you can have hello web app URL that you can use tooconfigure hello RP trust later.</span></span>

1. <span data-ttu-id="a2bb5-152">Högerklicka på projektet och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-152">Right-click your project and select **Publish**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-adfs/01-publish-website.png)
2. <span data-ttu-id="a2bb5-153">Välj **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-153">Select **Microsoft Azure App Service**.</span></span>
3. <span data-ttu-id="a2bb5-154">Om du inte har registrerat i tooAzure, klickar du på **logga In** och använda hello Microsoft-konto för din Azure-prenumeration toosign i.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-154">If you haven't signed in tooAzure, click **Sign In** and use hello Microsoft account for your Azure subscription toosign in.</span></span>
4. <span data-ttu-id="a2bb5-155">När du är inloggad klickar du på **ny** toocreate ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-155">Once signed in, click **New** toocreate a web app.</span></span>
5. <span data-ttu-id="a2bb5-156">Fyll i alla obligatoriska fält.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-156">Fill in all required fields.</span></span> <span data-ttu-id="a2bb5-157">Du kommer tooconnect tooon lokala data senare, så att inte skapa en databas för det här webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-157">You are going tooconnect tooon-premises data later, so don't create a database for this web app.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-adfs/02-create-website.png)
6. <span data-ttu-id="a2bb5-158">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-158">Click **Create**.</span></span> <span data-ttu-id="a2bb5-159">När hello webbprogrammet har skapats, öppnas hello Publicera webbplats dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-159">Once hello web app is created, hello Publish Web dialog is opened.</span></span>
7. <span data-ttu-id="a2bb5-160">I **Måladress**, ändra **http** för**https**.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-160">In **Destination URL**, change **http** too**https**.</span></span> <span data-ttu-id="a2bb5-161">Kopiera hello hela URL tooa textredigerare för senare användning.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-161">Copy hello entire URL tooa text editor for later use.</span></span> <span data-ttu-id="a2bb5-162">Klicka på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-162">Then, click **Publish**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-adfs/03-destination-url.png)
8. <span data-ttu-id="a2bb5-163">Öppna i Visual Studio **Web.Release.config** i projektet.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-163">In Visual Studio, open **Web.Release.config** in your project.</span></span> <span data-ttu-id="a2bb5-164">Infoga följande XML till hello hello `<configuration>` tagga och Ersätt hello nyckel/värde till ditt webbprogram för publicera URL: en.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-164">Insert hello following XML into hello `<configuration>` tag, and replace hello key value with your publish web app's URL.</span></span>  
   
   <pre class="prettyprint">
   &lt;appSettings&gt;
   &lt;add key="ida:RPIdentifier" value="<mark>[e.g. https://mylobapp.azurewebsites.net/]</mark>" xdt:Transform="SetAttributes" xdt:Locator="Match(key)" /&gt;
   &lt;/appSettings&gt;</pre>

<span data-ttu-id="a2bb5-165">När du är klar, du har två RP-identifierare som konfigurerats i ditt projekt, en för din miljö för felsökning i Visual Studio och en för hello publicerade webb-app i Azure.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-165">When you're done, you have two RP identifiers configured in your project, one for your debug environment in Visual Studio, and one for hello published web app in Azure.</span></span> <span data-ttu-id="a2bb5-166">Du ställer in ett RP-förtroende för varje hello två miljöer i AD FS.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-166">You will set up an RP trust for each of hello two environments in AD FS.</span></span> <span data-ttu-id="a2bb5-167">Vid felsökning hello app-inställningar i Web.config är används toomake din **felsöka** konfigurationen fungerar med AD FS.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-167">During debugging, hello app settings in Web.config are used toomake your **Debug** configuration work with AD FS.</span></span> <span data-ttu-id="a2bb5-168">När den publiceras (som standard hello **versionen** configuration publiceras), omvandlade Web.config överförs som innehåller hello app inställningen ändringar i Web.Release.config.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-168">When it's published (by default, hello **Release** configuration is published), a transformed Web.config is uploaded that incorporates hello app setting changes in Web.Release.config.</span></span>

<span data-ttu-id="a2bb5-169">Om du vill tooattach hello publicerade webbappen i Azure toohello felsökare (dvs. Du måste överföra symboler för felsökning av din kod i hello publicerade webb-app), kan du skapa en klon av hello felsöka konfigurationen för Azure-felsökning, men transformera (med sin egen anpassade Web.config t.ex. Web.AzureDebug.config) som använder hello app-inställningar från Web.Release.config. Detta ger dig toomaintain en statisk konfiguration mellan olika hello-miljöer.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-169">If you want tooattach hello published web app in Azure toohello debugger (i.e. you must upload debug symbols of your code in hello published web app), you can create a clone of hello Debug configuration for Azure debugging, but with its own custom Web.config transform (e.g. Web.AzureDebug.config) that uses hello app settings from Web.Release.config. This allows you toomaintain a static configuration across hello different environments.</span></span>

<a name="bkmk_rptrusts"></a>

## <a name="configure-relying-party-trusts-in-ad-fs-management"></a><span data-ttu-id="a2bb5-170">Konfigurera förtroenden för förlitande part i AD FS-hantering</span><span class="sxs-lookup"><span data-stu-id="a2bb5-170">Configure relying party trusts in AD FS Management</span></span>
<span data-ttu-id="a2bb5-171">Du måste nu tooconfigure ett RP förtroende i AD FS-hantering innan du kan använda exempelprogrammet och faktiskt autentisera med AD FS.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-171">Now you need tooconfigure an RP trust in AD FS Management before you can use your sample application and actually authenticate with AD FS.</span></span> <span data-ttu-id="a2bb5-172">Du behöver tooset in två separata RP-förtroenden, en för din miljö för felsökning och en för ditt publicerade webbprogram.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-172">You will need tooset up two separate RP trusts, one for your debug environment and one for your published web app.</span></span>

> [!NOTE]
> <span data-ttu-id="a2bb5-173">Kontrollera att du upprepar hello följande för både dina miljöer.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-173">Make sure that you repeat hello following steps for both of your environments.</span></span>
> 
> 

1. <span data-ttu-id="a2bb5-174">Logga in med autentiseringsuppgifter som har management rättigheter tooAD as på AD FS-servern.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-174">On your AD FS server, log in with credentials that have management rights tooAD FS.</span></span>
2. <span data-ttu-id="a2bb5-175">Öppna AD FS-hantering.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-175">Open AD FS Management.</span></span> <span data-ttu-id="a2bb5-176">Högerklicka på **AD FS\Trusted Relationships\Relying partens förtroenden** och välj **Lägg till förlitande part**.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-176">Right-click **AD FS\Trusted Relationships\Relying Party Trusts** and select **Add Relying Party Trust**.</span></span>
   
   ![](./media/web-sites-dotnet-lob-application-adfs/1-add-rptrust.png)
3. <span data-ttu-id="a2bb5-177">I hello **Välj datakälla** väljer **ange information om hello förlitande part manuellt**.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-177">In hello **Select Data Source** page, select **Enter data about hello relying party manually**.</span></span> 
   
   ![](./media/web-sites-dotnet-lob-application-adfs/2-enter-rp-manually.png)
4. <span data-ttu-id="a2bb5-178">I hello **ange visningsnamn** , Skriv ett namn för programmet hello och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-178">In hello **Specify Display Name** page, type a display name for hello application and click **Next**.</span></span>
5. <span data-ttu-id="a2bb5-179">I hello **Välj protokollet** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-179">In hello **Choose Protocol** page, click **Next**.</span></span>
6. <span data-ttu-id="a2bb5-180">I hello **konfigurera certifikat** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-180">In hello **Configure Certificate** page, click **Next**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="a2bb5-181">Eftersom du ska använda HTTPS redan, är krypterade token valfria.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-181">Since you should be using HTTPS already, encrypted tokens are optional.</span></span> <span data-ttu-id="a2bb5-182">Om du verkligen tooencrypt token från AD FS på den här sidan måste du också lägga till token-dekryptering logik i koden.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-182">If you really want tooencrypt tokens from AD FS on this page, you must also add token-decrypting logic in your code.</span></span> <span data-ttu-id="a2bb5-183">Mer information finns i [manuellt konfigurera OWIN WS-Federation mellanprogram och ta emot krypterad token](http://chris.59north.com/post/2014/08/21/Manually-configuring-OWIN-WS-Federation-middleware-and-accepting-encrypted-tokens.aspx).</span><span class="sxs-lookup"><span data-stu-id="a2bb5-183">For more information, see [Manually configuring OWIN WS-Federation middleware and accepting encrypted tokens](http://chris.59north.com/post/2014/08/21/Manually-configuring-OWIN-WS-Federation-middleware-and-accepting-encrypted-tokens.aspx).</span></span>
   > 
   > 
7. <span data-ttu-id="a2bb5-184">Innan du går vidare till nästa steg i hello, behöver du en typ av information från Visual Studio-projekt.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-184">Before you move onto hello next step, you need one piece of information from your Visual Studio project.</span></span> <span data-ttu-id="a2bb5-185">Observera i hello projektegenskaperna hello **SSL URL** av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-185">In hello project properties, note hello **SSL URL** of hello application.</span></span> 
   
   ![](./media/web-sites-dotnet-lob-application-adfs/3-ssl-url.png)
8. <span data-ttu-id="a2bb5-186">Tillbaka i AD FS-hantering, i hello **konfigurera URL** sidan hello **guiden Lägg till förlitande part förtroende**väljer **aktivera stöd för hello protokollet WS-Federation Passive** och Skriv i hello SSL-URL för Visual Studio-projekt som du antecknade i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-186">Back in AD FS Management, in hello **Configure URL** page of hello **Add Relying Party Trust Wizard**, select **Enable support for hello WS-Federation Passive protocol** and type in hello SSL URL of your Visual Studio project that you noted in hello previous step.</span></span> <span data-ttu-id="a2bb5-187">Klicka sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-187">Then, click **Next**.</span></span>
   
   ![](./media/web-sites-dotnet-lob-application-adfs/4-configure-url.png)
   
   > [!NOTE]
   > <span data-ttu-id="a2bb5-188">URL: en anger där toosend hello klient efter autentisering lyckas.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-188">URL specifies where toosend hello client after authentication succeeds.</span></span> <span data-ttu-id="a2bb5-189">För hello debug miljö bör man <code>https://localhost:&lt;port&gt;/</code>.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-189">For hello debug environment, it should be <code>https://localhost:&lt;port&gt;/</code>.</span></span> <span data-ttu-id="a2bb5-190">Det bör vara hello web app-URL för hello publicerade webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-190">For hello published web app, it should be hello web app URL.</span></span>
   > 
   > 
9. <span data-ttu-id="a2bb5-191">I hello **konfigurera identifierare** kontrollerar du att projektet SSL-URL finns redan i listan och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-191">In hello **Configure Identifiers** page, verify that your project SSL URL is already listed and click **Next**.</span></span> <span data-ttu-id="a2bb5-192">Klicka på **nästa** alla hello sätt toohello slutet av hello guiden med standardinställningarna.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-192">Click **Next** all hello way toohello end of hello wizard with default selections.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="a2bb5-193">I App_Start\Startup.Auth.cs för Visual Studio-projekt identifieraren matchas mot hello värdet för <code>WsFederationAuthenticationOptions.Wtrealm</code> under federerad autentisering.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-193">In App_Start\Startup.Auth.cs of your Visual Studio project, this identifier is matched against hello value of <code>WsFederationAuthenticationOptions.Wtrealm</code> during federated authentication.</span></span> <span data-ttu-id="a2bb5-194">Som standard läggs hello programmets URL hello föregående steg som en RP-identifierare.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-194">By default, hello application's URL from hello previous step is added as an RP identifier.</span></span>
   > 
   > 
10. <span data-ttu-id="a2bb5-195">Du har nu konfigurerat hello RP program för ditt projekt i AD FS.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-195">You have now finished configuring hello RP application for your project in AD FS.</span></span> <span data-ttu-id="a2bb5-196">Därefter konfigurerar du det här programmet toosend hello anspråk som behövs i programmet.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-196">Next, you configure this application toosend hello claims needed by your application.</span></span> <span data-ttu-id="a2bb5-197">Hej **redigera Anspråksregler** dialogrutan är öppnas som standard hello slutet av hello guiden så att du kan starta direkt.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-197">hello **Edit Claim Rules** dialog is opened by default for you at hello end of hello wizard so you can start immediately.</span></span> <span data-ttu-id="a2bb5-198">Nu ska vi konfigurera hello minst följande anspråk (med scheman inom parentes):</span><span class="sxs-lookup"><span data-stu-id="a2bb5-198">Let's configure at least hello following claims (with schemas in parentheses):</span></span>
    
    * <span data-ttu-id="a2bb5-199">Namn (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name -) som används av ASP.NET toohydrate `User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-199">Name (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) - used by ASP.NET toohydrate `User.Identity.Name`.</span></span>
    * <span data-ttu-id="a2bb5-200">Användarens huvudnamn (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn) - används toouniquely identifiera användare i hello organisation.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-200">User principal name (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn) - used toouniquely identify users in hello organization.</span></span>
    * <span data-ttu-id="a2bb5-201">Gruppmedlemskap som roller (http://schemas.microsoft.com/ws/2008/06/identity/claims/role) - kan användas med `[Authorize(Roles="role1, role2,...")]` decoration tooauthorize domänkontrollanter/en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-201">Group memberships as roles (http://schemas.microsoft.com/ws/2008/06/identity/claims/role) - can be used with `[Authorize(Roles="role1, role2,...")]` decoration tooauthorize controllers/actions.</span></span> <span data-ttu-id="a2bb5-202">I verkligheten kan kan den här metoden inte vara hello de flesta performant för auktorisering av roller.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-202">In reality, this approach may not be hello most performant for role authorization.</span></span> <span data-ttu-id="a2bb5-203">Om din AD-användare tillhör toohundreds säkerhetsgrupper, blir de hundratals rollanspråk i hello SAML-token.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-203">If your AD users belong toohundreds of security groups, they become hundreds of role claims in hello SAML token.</span></span> <span data-ttu-id="a2bb5-204">En annan metod är toosend som en enda roll anspråk villkorligt beroende på hello användarens medlemskap i en viss grupp.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-204">An alternative approach is toosend a single role claim conditionally depending on hello user's membership in a particular group.</span></span> <span data-ttu-id="a2bb5-205">Men kommer vi enkelhet för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-205">However, we'll keep it simple for this tutorial.</span></span>
    * <span data-ttu-id="a2bb5-206">Name ID (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier) - kan användas för skydd mot förfalskning verifiering.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-206">Name ID (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier) - can be used for anti-forgery validation.</span></span> <span data-ttu-id="a2bb5-207">Finns mer information om hur toomake är fungerar med skydd mot förfalskning validering hello **lägger till funktioner för line-of-business** avsnitt i [skapa en Azure line-of-business-app med Azure Active Directory-autentisering ](web-sites-dotnet-lob-application-azure-ad.md#bkmk_crud).</span><span class="sxs-lookup"><span data-stu-id="a2bb5-207">For more information on how toomake it work with anti-forgery validation, see hello **Add line-of-business functionality** section of [Create a line-of-business Azure app with Azure Active Directory authentication](web-sites-dotnet-lob-application-azure-ad.md#bkmk_crud).</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="a2bb5-208">hello anspråk datatyper du behöver tooconfigure för ditt program bestäms av programmets behov.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-208">hello claim types you need tooconfigure for your application is determined by your application's needs.</span></span> <span data-ttu-id="a2bb5-209">Hello listan med anspråk som stöds av Azure Active Directory-program (d.v.s. RP förtroenden), till exempel finns i [stöds Token och anspråkstyper](http://msdn.microsoft.com/library/azure/dn195587.aspx).</span><span class="sxs-lookup"><span data-stu-id="a2bb5-209">For hello list of claims supported by Azure Active Directory applications (i.e. RP trusts), for example, see [Supported Token and Claim Types](http://msdn.microsoft.com/library/azure/dn195587.aspx).</span></span>
    > 
    > 
11. <span data-ttu-id="a2bb5-210">Klicka på i dialogrutan Redigera Anspråksregler hello **Lägg till regel**.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-210">In hello Edit Claim Rules dialog, click **Add Rule**.</span></span>
12. <span data-ttu-id="a2bb5-211">Konfigurera hello name, UPN och roll anspråk enligt hello skärmbild och klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-211">Configure hello name, UPN, and role claims as shown in hello screenshot and click **Finish**.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-adfs/5-ldap-claims.png)
    
    <span data-ttu-id="a2bb5-212">Därefter skapar du ett tillfälligt namn ID anspråk med hello steg visas i [namn identifierare i SAML intyg](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).</span><span class="sxs-lookup"><span data-stu-id="a2bb5-212">Next, you create a transient name ID claim using hello steps demonstrated in [Name Identifiers in SAML assertions](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).</span></span>
13. <span data-ttu-id="a2bb5-213">Klicka på **Lägg till regel** igen.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-213">Click **Add Rule** again.</span></span>
14. <span data-ttu-id="a2bb5-214">Välj **skicka anspråk med en anpassad regel** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-214">Select **Send Claims Using a Custom Rule** and click **Next**.</span></span>
15. <span data-ttu-id="a2bb5-215">Klistra in hello följande regel språk i hello **anpassad regel** rutan, hello regel **Per sessionsidentifierare** och klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-215">Paste hello following rule language into hello **Custom rule** box, name hello rule **Per Session Identifier** and click **Finish**.</span></span>  
    
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
    
    <span data-ttu-id="a2bb5-216">En anpassad regel bör se ut som liknar denna skärmbild:</span><span class="sxs-lookup"><span data-stu-id="a2bb5-216">Your custom rule should look like this screenshot:</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-adfs/6-per-session-identifier.png)
16. <span data-ttu-id="a2bb5-217">Klicka på **Lägg till regel** igen.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-217">Click **Add Rule** again.</span></span>
17. <span data-ttu-id="a2bb5-218">Välj **transformera ett inkommande anspråk** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-218">Select **Transform an Incoming Claim** and click **Next**.</span></span>
18. <span data-ttu-id="a2bb5-219">Konfigurera hello regeln enligt hello skärmbild (med hello Anspråkstyp som du skapade i hello anpassad regel) och klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-219">Configure hello rule as shown in hello screenshot (using hello claim type you created in hello custom rule) and click **Finish**.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-adfs/7-transient-name-id.png)
    
    <span data-ttu-id="a2bb5-220">Detaljerad information om hello steg för hello tillfälligt namn-ID-anspråk finns [namn identifierare i SAML intyg](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).</span><span class="sxs-lookup"><span data-stu-id="a2bb5-220">For detailed information on hello steps for hello transient Name ID claim, see [Name Identifiers in SAML assertions](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).</span></span>
19. <span data-ttu-id="a2bb5-221">Klicka på **tillämpa** i hello **redigera Anspråksregler** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-221">Click **Apply** in hello **Edit Claim Rules** dialog.</span></span> <span data-ttu-id="a2bb5-222">Det bör nu se ut som följande skärmbild hello:</span><span class="sxs-lookup"><span data-stu-id="a2bb5-222">It should now look like hello following screenshot:</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-adfs/8-all-claim-rules.png)
    
    > [!NOTE]
    > <span data-ttu-id="a2bb5-223">Se till att du upprepa dessa steg för både debug-miljön och publicerade webbprogram.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-223">Again, make sure that you repeat these steps for both your debug environment and published web app.</span></span>
    > 
    > 

<a name="bkmk_test"></a>

## <a name="test-federated-authentication-for-your-application"></a><span data-ttu-id="a2bb5-224">Testa federerad autentisering för ditt program</span><span class="sxs-lookup"><span data-stu-id="a2bb5-224">Test federated authentication for your application</span></span>
<span data-ttu-id="a2bb5-225">Du är klar tootest programmets autentiseringslogiken mot AD FS.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-225">You are ready tootest your application's authentication logic against AD FS.</span></span> <span data-ttu-id="a2bb5-226">Jag har en användare som tillhör tooa testgrupp i Active Directory (AD) i min AD FS i laboratoriemiljö.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-226">In my AD FS lab environment, I have a test user that belongs tooa test group in Active Directory (AD).</span></span>

![](./media/web-sites-dotnet-lob-application-adfs/10-test-user-and-group.png)

<span data-ttu-id="a2bb5-227">tootest autentisering i hello felsökare allt du behöver toodo nu är typ `F5`.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-227">tootest authentication in hello debugger, all you need toodo now is type `F5`.</span></span> <span data-ttu-id="a2bb5-228">Om du vill tootest autentisering i hello publicerade webbprogram, navigera toohello URL.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-228">If you want tootest authentication in hello published web app, navigate toohello URL.</span></span>

<span data-ttu-id="a2bb5-229">När hello webbprogrammet har lästs in, klickar du på **logga In**.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-229">After hello web application loads, click **Sign In**.</span></span> <span data-ttu-id="a2bb5-230">Nu bör du få antingen en dialogruta eller hello inloggningen inloggningssida hanteras av AD FS, beroende på hello autentiseringsmetod som väljs av AD FS.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-230">You should now get either a login dialog or hello login page served by AD FS, depending on hello authentication method chosen by AD FS.</span></span> <span data-ttu-id="a2bb5-231">Det här är vad jag i Internet Explorer 11.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-231">Here's what I get in Internet Explorer 11.</span></span>

![](./media/web-sites-dotnet-lob-application-adfs/9-test-debugging.png)

<span data-ttu-id="a2bb5-232">När du loggar in med en användare i hello AD-domänen för hello AD FS-distribution kan du bör nu se hello webbsida igen med **Hello, <User Name>!**</span><span class="sxs-lookup"><span data-stu-id="a2bb5-232">Once you log in with a user in hello AD domain of hello AD FS deployment, you should now see hello homepage again with **Hello, <User Name>!**</span></span> <span data-ttu-id="a2bb5-233">i hello hörnet.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-233">in hello corner.</span></span> <span data-ttu-id="a2bb5-234">Det här är vad jag.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-234">Here's what I get.</span></span>

![](./media/web-sites-dotnet-lob-application-adfs/11-test-debugging-success.png)

<span data-ttu-id="a2bb5-235">Hittills har du lyckades hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="a2bb5-235">So far, you've succeeded in hello following ways:</span></span>

* <span data-ttu-id="a2bb5-236">Programmet har nått AD FS och en matchande RP-identifierare finns i hello AD FS-databasen</span><span class="sxs-lookup"><span data-stu-id="a2bb5-236">Your application has successfully reached AD FS and a matching RP identifier is found in hello AD FS database</span></span>
* <span data-ttu-id="a2bb5-237">AD FS har autentiserats en AD-användare och omdirigering du säkerhetskopiera toohello programmets startsida</span><span class="sxs-lookup"><span data-stu-id="a2bb5-237">AD FS has successfully authenticated an AD user and redirect you back toohello application's homepage</span></span>
* <span data-ttu-id="a2bb5-238">AD FS som har skickats hello namn anspråk (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) tooyour program som anges av hello faktum hello användaren namnet visas i hello hörnet.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-238">AD FS as successfully sent hello name claim (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) tooyour application, as indicated by hello fact that hello user name is displayed in hello corner.</span></span> 

<span data-ttu-id="a2bb5-239">Om anspråket hello saknas du skulle ha sett **Hello,!**.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-239">If hello name claim is missing, you would have seen **Hello, !**.</span></span> <span data-ttu-id="a2bb5-240">Om du tittar på Views\Shared\_LoginPartial.cshtml, som du hittar den använder `User.Identity.Name` toodisplay hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-240">If you look at Views\Shared\_LoginPartial.cshtml, you find that it uses `User.Identity.Name` toodisplay hello user name.</span></span> <span data-ttu-id="a2bb5-241">Som nämnts tidigare, om hello namn anspråk av hello autentiserade användaren är tillgänglig i hello SAML-token är hydrates den här egenskapen med det i ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-241">As mentioned previously, if hello name claim of hello authenticated user is available in hello SAML token, ASP.NET hydrates this property with it.</span></span> <span data-ttu-id="a2bb5-242">toosee alla hello anspråk som skickas av AD FS, placera en brytpunkt i Controllers\HomeController.cs, i hello Index åtgärdsmetod.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-242">toosee all hello claims that are sent by AD FS, put a breakpoint in Controllers\HomeController.cs, in hello Index action method.</span></span> <span data-ttu-id="a2bb5-243">När hello användaren autentiseras inspektera hello `System.Security.Claims.Current.Claims` samling.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-243">After hello user is authenticated, inspect hello `System.Security.Claims.Current.Claims` collection.</span></span>

![](./media/web-sites-dotnet-lob-application-adfs/12-test-debugging-all-claims.png) 

<a name="bkmk_authorize"></a>

## <a name="authorize-users-for-specific-controllers-or-actions"></a><span data-ttu-id="a2bb5-244">Auktorisera användare till specifika domänkontrollanter eller åtgärder</span><span class="sxs-lookup"><span data-stu-id="a2bb5-244">Authorize users for specific controllers or actions</span></span>
<span data-ttu-id="a2bb5-245">Eftersom du har inkluderat gruppmedlemskap som rollanspråk i konfigurationen RP förtroende, du kan nu använda dem direkt i hello `[Authorize(Roles="...")]` decoration för domänkontrollanter och åtgärder.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-245">Since you have included group memberships as role claims in your RP trust configuration, you can now use them directly in hello `[Authorize(Roles="...")]` decoration for controllers and actions.</span></span> <span data-ttu-id="a2bb5-246">I ett line-of-business-program med hello skapa-Läs-Uppdatera-ta bort CRUD-mönster auktorisera du specifika roller tooaccess varje åtgärd.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-246">In a line-of-business application with hello Create-Read-Update-Delete (CRUD) pattern, you can authorize specific roles tooaccess each action.</span></span> <span data-ttu-id="a2bb5-247">Just nu kommer du bara testa den här funktionen på hello befintliga Home domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-247">For now, you will just try out this feature on hello existing Home controller.</span></span>

1. <span data-ttu-id="a2bb5-248">Öppna Controllers\HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-248">Open Controllers\HomeController.cs.</span></span>
2. <span data-ttu-id="a2bb5-249">Skapa snygga hello `About` och `Contact` åtgärd metoder liknande toohello följande kod, med medlemskap i säkerhetsgrupper som den autentiserade användaren har.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-249">Decorate hello `About` and `Contact` action methods similar toohello following code, using security group memberships that your authenticated user has.</span></span>  
   
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
   
    <span data-ttu-id="a2bb5-250">Eftersom jag har lagt till **testanvändare** för**testgrupp** i min testmiljö för AD FS ska jag använda testgrupp tootest tillstånd på `About`.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-250">Since I added **Test User** too**Test Group** in my AD FS lab environment, I'll use Test Group tootest authorization on `About`.</span></span> <span data-ttu-id="a2bb5-251">För `Contact`, kan jag testa hello negativt fall av **Domänadministratörer**, toowhich **testa användare** hör inte.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-251">For `Contact`, I'll test hello negative case of **Domain Admins**, toowhich **Test User** doesn't belong.</span></span>
3. <span data-ttu-id="a2bb5-252">Starta hello felsökare genom att skriva `F5` och logga in och klicka sedan på **om**.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-252">Start hello debugger by typing `F5` and sign in, then click **About**.</span></span> <span data-ttu-id="a2bb5-253">Du bör nu visa hello `~/About/Index` sidan, om den autentiserade användaren har behörighet för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-253">You should now be viewing hello `~/About/Index` page successfully, if your authenticated user is authorized for that action.</span></span>
4. <span data-ttu-id="a2bb5-254">Klicka på **Kontakta**, som i min fallet bör inte tillåta **testanvändare** för hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-254">Now click **Contact**, which in my case should not authorize **Test User** for hello action.</span></span> <span data-ttu-id="a2bb5-255">Hello webbläsaren är dock omdirigerade tooAD FS som slutligen visar det här meddelandet:</span><span class="sxs-lookup"><span data-stu-id="a2bb5-255">However, hello browser is redirected tooAD FS, which eventually shows this message:</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-adfs/13-authorize-adfs-error.png)
   
    <span data-ttu-id="a2bb5-256">Om du undersöka det här felet i Loggboken på hello AD FS-servern, visas den här Undantagsmeddelande:</span><span class="sxs-lookup"><span data-stu-id="a2bb5-256">If you investigate this error in Event Viewer on hello AD FS server, you see this exception message:</span></span>  
   
    <pre class="prettyprint">
    Microsoft.IdentityServer.Web.InvalidRequestException: MSIS7042: <mark>hello same client browser session has made '6' requests in hello last '11' seconds.</mark> Contact your administrator for details.
       at Microsoft.IdentityServer.Web.Protocols.PassiveProtocolHandler.UpdateLoopDetectionCookie(WrappedHttpListenerContext context)
       at Microsoft.IdentityServer.Web.Protocols.WSFederation.WSFederationProtocolHandler.SendSignInResponse(WSFederationContext context, MSISSignInResponse response)
       at Microsoft.IdentityServer.Web.PassiveProtocolListener.ProcessProtocolRequest(ProtocolContext protocolContext, PassiveProtocolHandler protocolHandler)
       at Microsoft.IdentityServer.Web.PassiveProtocolListener.OnGetContext(WrappedHttpListenerContext context)
    </pre>
   
    <span data-ttu-id="a2bb5-257">hello orsak till felet är att som standard MVC returnerar 401 obehörig när en användare inte har behörighet.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-257">hello reason for this error is that by default, MVC returns a 401 Unauthorized when a user's roles are not authorized.</span></span> <span data-ttu-id="a2bb5-258">Detta utlöser en identitetsleverantör under omautentisering begäran tooyour (AD FS).</span><span class="sxs-lookup"><span data-stu-id="a2bb5-258">This triggers a reauthentication request tooyour identity provider (AD FS).</span></span> <span data-ttu-id="a2bb5-259">Eftersom hello användaren redan har autentiserats AD FS returnerar toohello samma sida som utfärdar en annan 401, skapa en omdirigering loop.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-259">Since hello user is already authenticated, AD FS returns toohello same page, which then issues another 401, creating a redirect loop.</span></span> <span data-ttu-id="a2bb5-260">Du kommer att åsidosätta Authorizeattribute's `HandleUnauthorizedRequest` metod med enkel logik tooshow något som passar i stället för fortsätter hello omdirigering loop.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-260">You will override AuthorizeAttribute's `HandleUnauthorizedRequest` method with simple logic tooshow something that makes sense instead of continuing hello redirect loop.</span></span>
5. <span data-ttu-id="a2bb5-261">Skapa en fil i hello projektet med namnet AuthorizeAttribute.cs och klistra in hello följande kod till den.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-261">Create a file in hello project called AuthorizeAttribute.cs, and paste hello following code into it.</span></span>
   
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
   
    <span data-ttu-id="a2bb5-262">hello åsidosätta koden skickar en HTTP 403 (förbjuden) i stället för HTTP 401 (obehörig) i autentiserade men obehörig fall.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-262">hello override code sends an HTTP 403 (Forbidden) instead of HTTP 401 (Unauthorized) in  authenticated but unauthorized cases.</span></span>
6. <span data-ttu-id="a2bb5-263">Kör hello felsökare igen med `F5`.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-263">Run hello debugger again with `F5`.</span></span> <span data-ttu-id="a2bb5-264">Klicka på **Kontakta** visas nu ett meddelande om mer informativ (men alternativet):</span><span class="sxs-lookup"><span data-stu-id="a2bb5-264">Clicking **Contact** now shows a more informative (albeit unattractive) error message:</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-adfs/14-unauthorized-forbidden.png)
7. <span data-ttu-id="a2bb5-265">Publicera hello programmet tooAzure App Service Web Apps igen och testa hello hur hello live program.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-265">Publish hello application tooAzure App Service Web Apps again, and test hello behavior of hello live application.</span></span>

<a name="bkmk_data"></a>

## <a name="connect-tooon-premises-data"></a><span data-ttu-id="a2bb5-266">Ansluta tooon lokala data</span><span class="sxs-lookup"><span data-stu-id="a2bb5-266">Connect tooon-premises data</span></span>
<span data-ttu-id="a2bb5-267">En orsak till är att du vill ha tooimplement line-of-business-program med AD FS i stället för Azure Active Directory efterlevnadsproblem för att hålla organisation data från lokalt.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-267">A reason that you would want tooimplement your line-of-business application with AD FS instead of Azure Active Directory is compliance issues with keeping organization data off-premise.</span></span> <span data-ttu-id="a2bb5-268">Detta kan också innebära att ditt webbprogram i Azure måste komma åt lokala databaser, eftersom du inte kan toouse [SQL-databas](/services/sql-database/) som hello datanivå för web apps.</span><span class="sxs-lookup"><span data-stu-id="a2bb5-268">This may also mean that your web app in Azure must access on-premises databases, since you are not allowed toouse [SQL Database](/services/sql-database/) as hello data tier for your web apps.</span></span>

<span data-ttu-id="a2bb5-269">Azure App Service Web Apps stöder inte att komma åt lokala databaser med två sätt: [Hybridanslutningar](../biztalk-services/integration-hybrid-connection-overview.md) och [virtuella nätverk](web-sites-integrate-with-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="a2bb5-269">Azure App Service Web Apps supports accessing on-premises databases with two approaches: [Hybrid Connections](../biztalk-services/integration-hybrid-connection-overview.md) and [Virtual Networks](web-sites-integrate-with-vnet.md).</span></span> <span data-ttu-id="a2bb5-270">Mer information finns i [VNET med hjälp av integrering och Hybrid-anslutningar med Azure App Service Web Apps](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/).</span><span class="sxs-lookup"><span data-stu-id="a2bb5-270">For more information, see [Using VNET integration and Hybrid connections with Azure App Service Web Apps](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/).</span></span>

<a name="bkmk_resources"></a>

## <a name="further-resources"></a><span data-ttu-id="a2bb5-271">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a2bb5-271">Further resources</span></span>
* [<span data-ttu-id="a2bb5-272">Autentisera med lokala Active Directory i din Azure-app</span><span class="sxs-lookup"><span data-stu-id="a2bb5-272">Authenticate with on-premises Active Directory in your Azure app</span></span>](web-sites-authentication-authorization.md)
* [<span data-ttu-id="a2bb5-273">Skapa en Azure line-of-business-app med Azure Active Directory-autentisering</span><span class="sxs-lookup"><span data-stu-id="a2bb5-273">Create a line-of-business Azure app with Azure Active Directory authentication</span></span>](web-sites-dotnet-lob-application-azure-ad.md)
* [<span data-ttu-id="a2bb5-274">Använd hello lokalt organisationens autentisering alternativet (AD FS) med ASP.NET i Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="a2bb5-274">Use hello On-Premises Organizational Authentication Option (ADFS) With ASP.NET in Visual Studio 2013</span></span>](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/)
* [<span data-ttu-id="a2bb5-275">Migrera en VS2013 Web Project från WIF tooKatana</span><span class="sxs-lookup"><span data-stu-id="a2bb5-275">Migrate a VS2013 Web Project From WIF tooKatana</span></span>](http://www.cloudidentity.com/blog/2014/09/15/MIGRATE-A-VS2013-WEB-PROJECT-FROM-WIF-TO-KATANA/)
* [<span data-ttu-id="a2bb5-276">Översikt över Active Directory Federation Services</span><span class="sxs-lookup"><span data-stu-id="a2bb5-276">Active Directory Federation Services Overview</span></span>](http://technet.microsoft.com/library/hh831502.aspx)
* [<span data-ttu-id="a2bb5-277">Specifikation för WS-Federation 1.1</span><span class="sxs-lookup"><span data-stu-id="a2bb5-277">WS-Federation 1.1 specification</span></span>](http://download.boulder.ibm.com/ibmdl/pub/software/dw/specs/ws-fed/WS-Federation-V1-1B.pdf?S_TACT=105AGX04&S_CMP=LP)

