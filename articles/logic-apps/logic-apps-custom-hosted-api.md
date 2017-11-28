---
title: "aaaDeploy, anropar och autentisera webb-API: er och REST API: er för Azure Logic Apps | Microsoft Docs"
description: "Distribuera, autentisera och anropa webb-API: er och REST API: er i arbetsflöden för system-integration med Azure Logikappar"
keywords: "webb-API: er, REST API: er, kopplingar, arbetsflöden, system integreringar, autentisera"
services: logic-apps
author: stepsic-microsoft-com
manager: anneta
editor: 
documentationcenter: 
ms.assetid: f113005d-0ba6-496b-8230-c1eadbd6dbb9
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: LADocs; stepsic
ms.openlocfilehash: ca1e4f28196b21f43b7c9ab94029684121e36f63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-call-and-authenticate-custom-apis-as-connectors-for-logic-apps"></a><span data-ttu-id="50994-104">Distribuera, anropa och autentisera anpassade API: er som kopplingar för logic apps</span><span class="sxs-lookup"><span data-stu-id="50994-104">Deploy, call, and authenticate custom APIs as connectors for logic apps</span></span>

<span data-ttu-id="50994-105">När du [skapa anpassade API: er](./logic-apps-create-api-app.md) som tillhandahåller åtgärder eller utlösare toouse i logic apps arbetsflöden, måste du distribuera dina API: er innan du kan anropa dem.</span><span class="sxs-lookup"><span data-stu-id="50994-105">After you [create custom APIs](./logic-apps-create-api-app.md) that provide actions or triggers toouse in logic apps workflows, you must deploy your APIs before you can call them.</span></span> <span data-ttu-id="50994-106">Och även om du kan anropa API: er från en logikapp för hello bästa möjliga upplevelse, lägger du till [Swagger-metadata](http://swagger.io/specification/) som beskriver ditt API åtgärder och parametrar.</span><span class="sxs-lookup"><span data-stu-id="50994-106">And although you can call any API from a logic app, for hello best experience, add [Swagger metadata](http://swagger.io/specification/) that describes your API's operations and parameters.</span></span> <span data-ttu-id="50994-107">Den här Swagger-filen hjälper din API fungerar bättre och enklare integrera med logic apps.</span><span class="sxs-lookup"><span data-stu-id="50994-107">This Swagger file helps your API work better and integrate more easily with logic apps.</span></span>

<span data-ttu-id="50994-108">Du kan distribuera dina API: er som [webbappar](../app-service-web/app-service-web-overview.md), men överväga att distribuera dina API: er som [API apps](../app-service-api/app-service-api-apps-why-best-platform.md), som kan underlätta ditt jobb när du skapar, värd och använda API: er i hello molnet och lokalt.</span><span class="sxs-lookup"><span data-stu-id="50994-108">You can deploy your APIs as [web apps](../app-service-web/app-service-web-overview.md), but consider deploying your APIs as [API apps](../app-service-api/app-service-api-apps-why-best-platform.md), which can make your job easier when you build, host, and consume APIs in hello cloud and on premises.</span></span> <span data-ttu-id="50994-109">Du saknar toochange någon kod i dina API: er, distribuera bara din kod tooan API-app.</span><span class="sxs-lookup"><span data-stu-id="50994-109">You don't have toochange any code in your APIs -- just deploy your code tooan API app.</span></span> <span data-ttu-id="50994-110">Du kan vara värd för dina API: er på [Azure App Service](../app-service/app-service-value-prop-what-is.md), en plattform-som-en tjänst (PaaS) erbjudande som ger hello bästa enklaste och mest skalbara sätt för API-värd.</span><span class="sxs-lookup"><span data-stu-id="50994-110">You can host your APIs on [Azure App Service](../app-service/app-service-value-prop-what-is.md), a platform-as-a-service (PaaS) offering that provides one of hello best, easiest, and most scalable ways for API hosting.</span></span>

<span data-ttu-id="50994-111">tooauthenticate anrop från logic apps tooyour API: er, du kan ställa in Azure Active Directory i hello Azure-portalen så att du inte behöver tooupdate din kod.</span><span class="sxs-lookup"><span data-stu-id="50994-111">tooauthenticate calls from logic apps tooyour APIs, you can set up Azure Active Directory in hello Azure portal so you don't have tooupdate your code.</span></span> <span data-ttu-id="50994-112">Eller så du behöver och ställer in användarautentisering via din API-koden.</span><span class="sxs-lookup"><span data-stu-id="50994-112">Or, you can require and enforce authentication through your API's code.</span></span>

## <a name="deploy-your-api-as-a-web-app-or-api-app"></a><span data-ttu-id="50994-113">Distribuera din API som en webbapp eller API-app</span><span class="sxs-lookup"><span data-stu-id="50994-113">Deploy your API as a web app or API app</span></span>

<span data-ttu-id="50994-114">Innan du kan anropa ditt anpassade API från en logikapp, kan du distribuera din API som en webbapp eller API app tooAzure Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="50994-114">Before you can call your custom API from a logic app, deploy your API as a web app or API app tooAzure App Service.</span></span> <span data-ttu-id="50994-115">Dessutom toomake Swagger-dokument läsas av hello logik App Designer och ange egenskaper för hello API-definition och aktivera [resursdelning för korsande ursprung (CORS)](../app-service-api/app-service-api-cors-consume-javascript.md#corsconfig) för ditt webbprogram eller API-app.</span><span class="sxs-lookup"><span data-stu-id="50994-115">Also, toomake your Swagger document readable by hello Logic App Designer, set hello API definition properties and turn on [cross-origin resource sharing (CORS)](../app-service-api/app-service-api-cors-consume-javascript.md#corsconfig) for your web app or API app.</span></span>

1. <span data-ttu-id="50994-116">Välj ditt webbprogram eller API-app i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="50994-116">In hello Azure portal, select your web app or API app.</span></span>

2. <span data-ttu-id="50994-117">Hello-bladet som öppnas under **API**, Välj **API-definition**.</span><span class="sxs-lookup"><span data-stu-id="50994-117">In hello blade that opens, under **API**, choose **API definition**.</span></span> <span data-ttu-id="50994-118">Ange hello **plats för API-definition** toohello URL för swagger.json-filen.</span><span class="sxs-lookup"><span data-stu-id="50994-118">Set hello **API definition location** toohello URL for your swagger.json file.</span></span>

   <span data-ttu-id="50994-119">Vanligtvis visas hello URL i följande format:`https://{name}.azurewebsites.net/swagger/docs/v1)`</span><span class="sxs-lookup"><span data-stu-id="50994-119">Usually, hello URL appears in this format: `https://{name}.azurewebsites.net/swagger/docs/v1)`</span></span>

   ![TooSwagger länkfil för din anpassade API](media/logic-apps-custom-hosted-api/custom-api-swagger-url.png)

3. <span data-ttu-id="50994-121">Under **API**, Välj **CORS**.</span><span class="sxs-lookup"><span data-stu-id="50994-121">Under **API**, choose **CORS**.</span></span> <span data-ttu-id="50994-122">Skapa princip för hello CORS för **tillåtna ursprung** för**'*'** (Tillåt alla).</span><span class="sxs-lookup"><span data-stu-id="50994-122">Set hello CORS policy for **Allowed origins** too**'*'** (allow all).</span></span>

   <span data-ttu-id="50994-123">Den här inställningen tillåter begäranden från logik App Designer.</span><span class="sxs-lookup"><span data-stu-id="50994-123">This setting permits requests from Logic App Designer.</span></span>

   ![Tillåta begäranden från logik App Designer tooyour anpassade API](media/logic-apps-custom-hosted-api/custom-api-cors.png)

<span data-ttu-id="50994-125">Mer information finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="50994-125">For more information, see these articles:</span></span>

* [<span data-ttu-id="50994-126">Lägg till Swagger-metadata för ASP.NET web API: er</span><span class="sxs-lookup"><span data-stu-id="50994-126">Add Swagger metadata for ASP.NET web APIs</span></span>](../app-service-api/app-service-api-dotnet-get-started.md#use-swagger-api-metadata-and-ui)
* [<span data-ttu-id="50994-127">Distribuera ASP.NET web API: er tooAzure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="50994-127">Deploy ASP.NET web APIs tooAzure App Service</span></span>](../app-service-api/app-service-api-dotnet-get-started.md)

## <a name="call-your-custom-api-from-logic-app-workflows"></a><span data-ttu-id="50994-128">Anropa ditt anpassade API från logik app arbetsflöden</span><span class="sxs-lookup"><span data-stu-id="50994-128">Call your custom API from logic app workflows</span></span>

<span data-ttu-id="50994-129">När du har skapat hello API-egenskaper och CORS ska din anpassade API utlösare och åtgärder bli tillgänglig för tooinclude i logik app arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="50994-129">After you set up hello API definition properties and CORS, your custom API's triggers and actions should become available for you tooinclude in your logic app workflow.</span></span> 

*  <span data-ttu-id="50994-130">Du kan bläddra din prenumeration webbplatser i hello Logic Apps Designer tooview hello webbplatser som har Swagger URL: er.</span><span class="sxs-lookup"><span data-stu-id="50994-130">tooview hello websites that have Swagger URLs, you can browse your subscription websites in hello Logic Apps Designer.</span></span>

*  <span data-ttu-id="50994-131">tooview tillgängliga åtgärder och indata genom att peka på ett Swagger-dokument, använda hello [HTTP + Swagger åtgärden](../connectors/connectors-native-http-swagger.md).</span><span class="sxs-lookup"><span data-stu-id="50994-131">tooview available actions and inputs by pointing at a Swagger document, use hello [HTTP + Swagger action](../connectors/connectors-native-http-swagger.md).</span></span>

*  <span data-ttu-id="50994-132">toocall API: er, inklusive API: er som inte har eller exponera en Swagger-dokument alltid kan du skapa en begäran med hello [HTTP-åtgärden](../connectors/connectors-native-http.md).</span><span class="sxs-lookup"><span data-stu-id="50994-132">toocall any API, including APIs that don't have or expose a Swagger document, you can always create a request with hello [HTTP action](../connectors/connectors-native-http.md).</span></span>

## <a name="authenticate-calls-tooyour-custom-api"></a><span data-ttu-id="50994-133">Autentisera anrop tooyour anpassade API</span><span class="sxs-lookup"><span data-stu-id="50994-133">Authenticate calls tooyour custom API</span></span>

<span data-ttu-id="50994-134">Du kan skydda anrop tooyour anpassade API på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="50994-134">You can secure calls tooyour custom API in these ways:</span></span>

* <span data-ttu-id="50994-135">[Inga kodändringar](#no-code): skydda din API med [Azure Active Directory (AD Azure)](../active-directory/active-directory-whatis.md) via hello Azure-portalen, så du inte har tooupdate koden eller omdistribuera ditt API.</span><span class="sxs-lookup"><span data-stu-id="50994-135">[No code changes](#no-code): Protect your API with [Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md) through hello Azure portal, so you don't have tooupdate your code or redeploy your API.</span></span>

  > [!NOTE]
  > <span data-ttu-id="50994-136">Som standard ger inte hello Azure AD-autentisering som du aktiverar i hello Azure-portalen detaljerad behörighet.</span><span class="sxs-lookup"><span data-stu-id="50994-136">By default, hello Azure AD authentication that you turn on in hello Azure portal doesn't provide fine-grained authorization.</span></span> <span data-ttu-id="50994-137">Den här autentiseringen låser exempelvis API-toojust en viss klient, inte tooa specifik användare eller app.</span><span class="sxs-lookup"><span data-stu-id="50994-137">For example, this authentication locks your API toojust a specific tenant, not tooa specific user or app.</span></span> 

* <span data-ttu-id="50994-138">[Uppdatera din API-koden](#update-code): skydda din API genom tvingande [certifikatautentisering](#certificate), [grundläggande autentisering](#basic), eller [Azure AD authentication](#azure-ad-code) via kod.</span><span class="sxs-lookup"><span data-stu-id="50994-138">[Update your API's code](#update-code): Protect your API by enforcing [certificate authentication](#certificate), [basic authentication](#basic), or [Azure AD authentication](#azure-ad-code) through code.</span></span>

<a name="no-code"></a>

### <a name="authenticate-calls-tooyour-api-without-changing-code"></a><span data-ttu-id="50994-139">Autentisera anrop tooyour API utan att ändra koden</span><span class="sxs-lookup"><span data-stu-id="50994-139">Authenticate calls tooyour API without changing code</span></span>

<span data-ttu-id="50994-140">Här är hello allmänna steg för den här metoden:</span><span class="sxs-lookup"><span data-stu-id="50994-140">Here's hello general steps for this method:</span></span>

1. <span data-ttu-id="50994-141">Skapa två [Azure Active Directory (Azure AD) application identiteter](../app-service-api/app-service-api-dotnet-service-principal-auth.md): en för din logikapp och en för webbprogram (eller API-app).</span><span class="sxs-lookup"><span data-stu-id="50994-141">Create two [Azure Active Directory (Azure AD) application identities](../app-service-api/app-service-api-dotnet-service-principal-auth.md): one for your logic app and one for your web app (or API app).</span></span>

2. <span data-ttu-id="50994-142">tooauthenticate anrop tooyour API, använda hello autentiseringsuppgifter (klient-ID och hemligt) för hello [tjänstens huvudnamn](../app-service-api/app-service-api-dotnet-service-principal-auth.md) som associeras med hello Azure AD programidentitet för din logikapp.</span><span class="sxs-lookup"><span data-stu-id="50994-142">tooauthenticate calls tooyour API, use hello credentials (client ID and secret) for hello [service principal](../app-service-api/app-service-api-dotnet-service-principal-auth.md) that's associated with hello Azure AD application identity for your logic app.</span></span>

3. <span data-ttu-id="50994-143">Inkludera hello program-ID: N i din app logik-definition.</span><span class="sxs-lookup"><span data-stu-id="50994-143">Include hello application IDs in your logic app definition.</span></span>

#### <a name="part-1-create-an-azure-ad-application-identity-for-your-logic-app"></a><span data-ttu-id="50994-144">Del 1: Skapa en Azure AD application identitet för din logikapp</span><span class="sxs-lookup"><span data-stu-id="50994-144">Part 1: Create an Azure AD application identity for your logic app</span></span>

<span data-ttu-id="50994-145">Din logikapp använder den här Azure AD application identitet tooauthenticate mot Azure AD.</span><span class="sxs-lookup"><span data-stu-id="50994-145">Your logic app uses this Azure AD application identity tooauthenticate against Azure AD.</span></span> <span data-ttu-id="50994-146">Du har bara tooset upp den här identiteten en gång för din katalog.</span><span class="sxs-lookup"><span data-stu-id="50994-146">You only have tooset up this identity one time for your directory.</span></span> <span data-ttu-id="50994-147">Du kan till exempel välja toouse hello samma identitet för dina logikappar trots att du kan skapa unika identiteter för varje logikapp.</span><span class="sxs-lookup"><span data-stu-id="50994-147">For example, you can choose toouse hello same identity for all your logic apps, even though you can create unique identities for each logic app.</span></span> <span data-ttu-id="50994-148">Du kan konfigurera dessa identiteter i hello Azure-portalen [klassiska Azure-portalen](#app-identity-logic-classic), eller Använd [PowerShell](#powershell).</span><span class="sxs-lookup"><span data-stu-id="50994-148">You can set up these identities in hello Azure portal, [Azure classic portal](#app-identity-logic-classic), or use [PowerShell](#powershell).</span></span>

<span data-ttu-id="50994-149">**Skapa hello programidentitet för din logikapp i hello Azure-portalen**</span><span class="sxs-lookup"><span data-stu-id="50994-149">**Create hello application identity for your logic app in hello Azure portal**</span></span>

1. <span data-ttu-id="50994-150">I hello [Azure-portalen](https://portal.azure.com "https://portal.azure.com"), Välj **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="50994-150">In hello [Azure portal](https://portal.azure.com "https://portal.azure.com"), choose **Azure Active Directory**.</span></span> 

2. <span data-ttu-id="50994-151">Bekräfta att du är i hello samma katalog som ditt webbprogram eller API-app.</span><span class="sxs-lookup"><span data-stu-id="50994-151">Confirm that you're in hello same directory as your web app or API app.</span></span>

   > [!TIP]
   > <span data-ttu-id="50994-152">tooswitch kataloger på din profil och välj en annan katalog.</span><span class="sxs-lookup"><span data-stu-id="50994-152">tooswitch directories, click your profile and select another directory.</span></span> <span data-ttu-id="50994-153">Eller välj **översikt** > **växel directory**.</span><span class="sxs-lookup"><span data-stu-id="50994-153">Or, choose **Overview** > **Switch directory**.</span></span>

3. <span data-ttu-id="50994-154">På menyn directory hello under **hantera**, Välj **App registreringar** > **nya appregistrering**.</span><span class="sxs-lookup"><span data-stu-id="50994-154">On hello directory menu, under **Manage**, choose **App registrations** > **New application registration**.</span></span>

   > [!TIP]
   > <span data-ttu-id="50994-155">Som standard visar hello registreringar programlistan alla app registreringar i din katalog.</span><span class="sxs-lookup"><span data-stu-id="50994-155">By default, hello app registrations list shows all app registrations in your directory.</span></span> <span data-ttu-id="50994-156">tooview endast app-registreringar, nästa toohello Sök-rutan, Välj **Mina appar**.</span><span class="sxs-lookup"><span data-stu-id="50994-156">tooview only your app registrations, next toohello search box, select **My apps**.</span></span> 

   ![Skapa ny appregistrering](./media/logic-apps-custom-hosted-api/new-app-registration-azure-portal.png)

4. <span data-ttu-id="50994-158">Namnge din Programidentitet, lämna **programtyp** ange för**webbapp / API**, ange en unik sträng formaterad som en domän för **inloggnings-URL**, och välj  **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="50994-158">Give your application identity a name, leave **Application type** set too**Web app / API**, provide a unique string formatted as a domain for **Sign-on URL**, and choose **Create**.</span></span>

   ![Ange namn och inloggning URL för Programidentitet](./media/logic-apps-custom-hosted-api/logic-app-identity-azure-portal.png)

   <span data-ttu-id="50994-160">hello Programidentitet som du skapade för din logikapp nu visas i hello app registreringar.</span><span class="sxs-lookup"><span data-stu-id="50994-160">hello application identity that you created for your logic app now appears in hello app registrations list.</span></span>

   ![Tillämpningsprogrammets identitet för din logikapp](./media/logic-apps-custom-hosted-api/logic-app-identity-created.png)

5. <span data-ttu-id="50994-162">Markera din nya Programidentitet i hello app registreringar.</span><span class="sxs-lookup"><span data-stu-id="50994-162">In hello app registrations list, select your new application identity.</span></span> <span data-ttu-id="50994-163">Kopiera och spara hello **program-ID** toouse som hello ”klient-ID” för din logikapp i del 3.</span><span class="sxs-lookup"><span data-stu-id="50994-163">Copy and save hello **Application ID** toouse as hello "client ID" for your logic app in Part 3.</span></span>

   ![Kopiera och spara program-ID för logikapp](./media/logic-apps-custom-hosted-api/logic-app-application-id.png)

6. <span data-ttu-id="50994-165">Om din identitet programinställningar inte visas, väljer **inställningar** eller **alla inställningar**.</span><span class="sxs-lookup"><span data-stu-id="50994-165">If your application identity settings aren't visible, choose **Settings** or **All settings**.</span></span>

7. <span data-ttu-id="50994-166">Under **API-åtkomst**, Välj **nycklar**.</span><span class="sxs-lookup"><span data-stu-id="50994-166">Under **API Access**, choose **Keys**.</span></span> <span data-ttu-id="50994-167">Under **beskrivning**, ange ett namn för din nyckel.</span><span class="sxs-lookup"><span data-stu-id="50994-167">Under **Description**, provide a name for your key.</span></span> <span data-ttu-id="50994-168">Under **Expires**, markera en varaktighet för din nyckel.</span><span class="sxs-lookup"><span data-stu-id="50994-168">Under **Expires**, select a duration for your key.</span></span>

   <span data-ttu-id="50994-169">hello-nyckel som du skapar fungerar som hello tillämpningsprogrammets identitet ”hemliga” eller lösenordet för din logikapp.</span><span class="sxs-lookup"><span data-stu-id="50994-169">hello key that you're creating acts as hello application identity's "secret" or password for your logic app.</span></span>

   ![Skapa en nyckel för logik app identitet](./media/logic-apps-custom-hosted-api/create-logic-app-identity-key-secret-password.png)

8. <span data-ttu-id="50994-171">Välj hello verktygsfältet **spara**.</span><span class="sxs-lookup"><span data-stu-id="50994-171">On hello toolbar, choose **Save**.</span></span> <span data-ttu-id="50994-172">Under **värdet**, nyckeln visas nu.</span><span class="sxs-lookup"><span data-stu-id="50994-172">Under **Value**, your key now appears.</span></span> 
<span data-ttu-id="50994-173">**Se till att toocopy och spara din nyckel** för senare användning eftersom hello nyckeln döljs när du lämnar hello viktiga bladet.</span><span class="sxs-lookup"><span data-stu-id="50994-173">**Make sure toocopy and save your key** for later use because hello key is hidden when you leave hello key blade.</span></span>

   <span data-ttu-id="50994-174">När du konfigurerar din logikapp i del 3, ange den här nyckeln som hello ”hemliga” eller lösenord.</span><span class="sxs-lookup"><span data-stu-id="50994-174">When you configure your logic app in Part 3, you specify this key as hello "secret" or password.</span></span>

   ![Kopiera och spara nyckeln för senare](./media/logic-apps-custom-hosted-api/logic-app-copy-key-secret-password.png)

<a name="app-identity-logic-classic"></a>

<span data-ttu-id="50994-176">**Skapa hello programidentitet för din logikapp i hello klassiska Azure-portalen**</span><span class="sxs-lookup"><span data-stu-id="50994-176">**Create hello application identity for your logic app in hello Azure classic portal**</span></span>

1. <span data-ttu-id="50994-177">I Hej klassiska Azure-portalen, Välj [ **Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).</span><span class="sxs-lookup"><span data-stu-id="50994-177">In hello Azure classic portal, choose [**Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).</span></span>

2. <span data-ttu-id="50994-178">Välj hello samma katalog som du använder för ditt webbprogram eller API-app.</span><span class="sxs-lookup"><span data-stu-id="50994-178">Select hello same directory that you use for your web app or API app.</span></span>

3. <span data-ttu-id="50994-179">På hello **program** , Välj **Lägg till** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="50994-179">On hello **Applications** tab, choose **Add** at hello bottom of hello page.</span></span>

4. <span data-ttu-id="50994-180">Namnge din identitet för programmet och välj **nästa** (HÖGERPIL).</span><span class="sxs-lookup"><span data-stu-id="50994-180">Give your application identity a name, and choose **Next** (right arrow).</span></span>

5. <span data-ttu-id="50994-181">Under **appegenskaper**, ange en unik sträng formaterad som en domän för **inloggnings-URL** och **App-ID URI**, och välj **Slutför** (markering).</span><span class="sxs-lookup"><span data-stu-id="50994-181">Under **App properties**, provide a unique string formatted as a domain for **Sign-on URL** and **App ID URI**, and choose **Complete** (checkmark).</span></span>

6. <span data-ttu-id="50994-182">På hello **konfigurera** fliken, kopiera och spara hello **klient-ID** för din app toouse för logiken i del 3.</span><span class="sxs-lookup"><span data-stu-id="50994-182">On hello **Configure** tab, copy and save hello **Client ID** for your logic app toouse in Part 3.</span></span>

7. <span data-ttu-id="50994-183">Under **nycklar**öppnar hello **Markera varaktighet** lista.</span><span class="sxs-lookup"><span data-stu-id="50994-183">Under **Keys**, open hello **Select duration** list.</span></span> <span data-ttu-id="50994-184">Markera en varaktighet för din nyckel.</span><span class="sxs-lookup"><span data-stu-id="50994-184">Select a duration for your key.</span></span>

   <span data-ttu-id="50994-185">hello-nyckel som du skapar fungerar som hello tillämpningsprogrammets identitet ”hemliga” eller lösenordet för din logikapp.</span><span class="sxs-lookup"><span data-stu-id="50994-185">hello key that you're creating acts as hello application identity's "secret" or password for your logic app.</span></span>

8. <span data-ttu-id="50994-186">Längst ned hello hello-sidan, Välj **spara**.</span><span class="sxs-lookup"><span data-stu-id="50994-186">At hello bottom of hello page, choose **Save**.</span></span> <span data-ttu-id="50994-187">Du kanske toowait några sekunder.</span><span class="sxs-lookup"><span data-stu-id="50994-187">You might have toowait a few seconds.</span></span>

9. <span data-ttu-id="50994-188">Under **nycklar**, se till att toocopy och spara hello-nyckel som nu visas.</span><span class="sxs-lookup"><span data-stu-id="50994-188">Under **Keys**, make sure toocopy and save hello key that now appears.</span></span> 

   <span data-ttu-id="50994-189">När du konfigurerar din logikapp i del 3, ange den här nyckeln som hello ”hemliga” eller lösenord.</span><span class="sxs-lookup"><span data-stu-id="50994-189">When you configure your logic app in Part 3, you specify this key as hello "secret" or password.</span></span>

<span data-ttu-id="50994-190">Mer information lär du dig hur för [konfigurera Azure Active Directory din App tjänstinloggning programmet toouse](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="50994-190">For more information, learn how too [configure your App Service application toouse Azure Active Directory login](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).</span></span>

<a name="powershell"></a>

<span data-ttu-id="50994-191">**Skapa hello programidentitet för din logikapp i PowerShell**</span><span class="sxs-lookup"><span data-stu-id="50994-191">**Create hello application identity for your logic app in PowerShell**</span></span>

<span data-ttu-id="50994-192">Du kan utföra den här uppgiften via Azure Resource Manager med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="50994-192">You can perform this task through Azure Resource Manager with PowerShell.</span></span> <span data-ttu-id="50994-193">I PowerShell kör du följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="50994-193">In PowerShell, run these commands:</span></span>

1. `Switch-AzureMode AzureResourceManager`

2. `Add-AzureAccount`

3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://mydomain.tld" -IdentifierUris "http://mydomain.tld" -Password "identity-password"`

4. <span data-ttu-id="50994-194">Se till att toocopy hello **klient-ID** (GUID för din Azure AD-klient) hello **program-ID**, och hello-lösenord som du använde.</span><span class="sxs-lookup"><span data-stu-id="50994-194">Make sure toocopy hello **Tenant ID** (GUID for your Azure AD tenant), hello **Application ID**, and hello password that you used.</span></span>

<span data-ttu-id="50994-195">Mer information lär du dig hur för [skapa ett huvudnamn för tjänsten med PowerShell tooaccess resurser](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="50994-195">For more information, learn how too [create a service principal with PowerShell tooaccess resources](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span></span>

#### <a name="part-2-create-an-azure-ad-application-identity-for-your-web-app-or-api-app"></a><span data-ttu-id="50994-196">Del 2: Skapa en Azure AD-programidentitet för ditt webbprogram eller API-app</span><span class="sxs-lookup"><span data-stu-id="50994-196">Part 2: Create an Azure AD application identity for your web app or API app</span></span>

<span data-ttu-id="50994-197">Om ditt webbprogram eller API-app redan har distribuerats, kan du aktivera autentisering och skapa hello Programidentitet i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="50994-197">If your web app or API app is already deployed, you can turn on authentication and create hello application identity in hello Azure portal.</span></span> <span data-ttu-id="50994-198">Annars kan du [aktivera autentisering när du distribuerar med en Azure Resource Manager-mall](#authen-deploy).</span><span class="sxs-lookup"><span data-stu-id="50994-198">Otherwise, you can [turn on authentication when you deploy with an Azure Resource Manager template](#authen-deploy).</span></span> 

<span data-ttu-id="50994-199">**Skapa hello Programidentitet och aktivera autentisering i hello Azure portal för distribuerade appar**</span><span class="sxs-lookup"><span data-stu-id="50994-199">**Create hello application identity and turn on authentication in hello Azure portal for deployed apps**</span></span>

1. <span data-ttu-id="50994-200">I hello [Azure-portalen](https://portal.azure.com "https://portal.azure.com"), söka efter och välj webbprogram eller API-app.</span><span class="sxs-lookup"><span data-stu-id="50994-200">In hello [Azure portal](https://portal.azure.com "https://portal.azure.com"), find and select your web app or API app.</span></span> 

2. <span data-ttu-id="50994-201">Under **inställningar**, Välj **autentisering/auktorisering**.</span><span class="sxs-lookup"><span data-stu-id="50994-201">Under **Settings**, choose **Authentication/Authorization**.</span></span> <span data-ttu-id="50994-202">Under **App autentiseringen av tjänsten**, aktivera autentisering **på**.</span><span class="sxs-lookup"><span data-stu-id="50994-202">Under **App Service Authentication**, turn authentication **On**.</span></span> <span data-ttu-id="50994-203">Under **autentiseringsproviders**, Välj **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="50994-203">Under **Authentication Providers**, choose **Azure Active Directory**.</span></span>

   ![Aktivera autentisering](./media/logic-apps-custom-hosted-api/custom-web-api-app-authentication.png)

3. <span data-ttu-id="50994-205">Nu skapa en programidentitet för ditt webbprogram eller API-app som visas här.</span><span class="sxs-lookup"><span data-stu-id="50994-205">Now create an application identity for your web app or API app as shown here.</span></span> <span data-ttu-id="50994-206">På hello **inställningarna för Azure Active Directory** bladet ange **hanteringsläge** för**Express**.</span><span class="sxs-lookup"><span data-stu-id="50994-206">On hello **Azure Active Directory Settings** blade, set **Management mode** too**Express**.</span></span> <span data-ttu-id="50994-207">Välj **Skapa nytt AD-App**.</span><span class="sxs-lookup"><span data-stu-id="50994-207">Choose **Create New AD App**.</span></span> <span data-ttu-id="50994-208">Namnge din identitet för programmet och välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="50994-208">Give your application identity a name, and choose **OK**.</span></span> 

   ![Skapa programidentitet för ditt webbprogram eller API-app](./media/logic-apps-custom-hosted-api/custom-api-application-identity.png)

4. <span data-ttu-id="50994-210">På hello **autentisering / auktorisering bladet**, Välj **spara**.</span><span class="sxs-lookup"><span data-stu-id="50994-210">On hello **Authentication / Authorization blade**, choose **Save**.</span></span>

<span data-ttu-id="50994-211">Du måste nu hitta hello klient-ID och klient-ID för hello Programidentitet som är kopplad till ditt webbprogram eller API-app.</span><span class="sxs-lookup"><span data-stu-id="50994-211">Now you must find hello client ID and tenant ID for hello application identity that's associated with your web app or API app.</span></span> <span data-ttu-id="50994-212">Du kan använda dessa ID: N i del 3.</span><span class="sxs-lookup"><span data-stu-id="50994-212">You use these IDs in Part 3.</span></span> <span data-ttu-id="50994-213">Så att fortsätta med de här stegen för hello Azure-portalen eller [klassiska Azure-portalen](#find-id-classic).</span><span class="sxs-lookup"><span data-stu-id="50994-213">So continue with these steps for hello Azure portal or [Azure classic portal](#find-id-classic).</span></span>

<span data-ttu-id="50994-214">**Hitta tillämpningsprogrammets identitet klient-ID och klient-ID för ditt webbprogram eller API-app i hello Azure-portalen**</span><span class="sxs-lookup"><span data-stu-id="50994-214">**Find application identity's client ID and tenant ID for your web app or API app in hello Azure portal**</span></span>

1. <span data-ttu-id="50994-215">Under **autentiseringsproviders**, Välj **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="50994-215">Under **Authentication Providers**, choose **Azure Active Directory**.</span></span> 

   ![Välj ”Azure Active Directory”](./media/logic-apps-custom-hosted-api/custom-api-app-identity-client-id-tenant-id.png)

2. <span data-ttu-id="50994-217">På hello **inställningarna för Azure Active Directory** bladet ange **hanteringsläge** för**Avancerat**.</span><span class="sxs-lookup"><span data-stu-id="50994-217">On hello **Azure Active Directory Settings** blade, set **Management mode** too**Advanced**.</span></span>

3. <span data-ttu-id="50994-218">Kopiera hello **klient-ID**, och spara det GUID för användning i del 3.</span><span class="sxs-lookup"><span data-stu-id="50994-218">Copy hello **Client ID**, and save that GUID for use in Part 3.</span></span>

   > [!TIP] 
   > <span data-ttu-id="50994-219">Om **klient-ID** och **utfärdar-Url** inte visas, försök att uppdatera hello Azure-portalen och upprepa steg 1.</span><span class="sxs-lookup"><span data-stu-id="50994-219">If **Client ID** and **Issuer Url** don't appear, try refreshing hello Azure portal, and repeat Step 1.</span></span>

4. <span data-ttu-id="50994-220">Under **utfärdar-Url**, kopiera och spara bara hello GUID för en del 3.</span><span class="sxs-lookup"><span data-stu-id="50994-220">Under **Issuer Url**, copy and save just hello GUID for Part 3.</span></span> <span data-ttu-id="50994-221">Du kan också använda detta GUID i ditt webbprogram eller mallen för distribution av API-app om det behövs.</span><span class="sxs-lookup"><span data-stu-id="50994-221">You can also use this GUID in your web app or API app's deployment template, if necessary.</span></span>

   <span data-ttu-id="50994-222">Detta GUID är din klient-GUID (”klient-ID”) och som ska visas i den här URL:`https://sts.windows.net/{GUID}`</span><span class="sxs-lookup"><span data-stu-id="50994-222">This GUID is your specific tenant's GUID ("tenant ID") and should appear in this URL: `https://sts.windows.net/{GUID}`</span></span>

5. <span data-ttu-id="50994-223">Stäng utan att spara dina ändringar hello **inställningarna för Azure Active Directory** bladet.</span><span class="sxs-lookup"><span data-stu-id="50994-223">Without saving your changes, close hello **Azure Active Directory Settings** blade.</span></span>

<a name="find-id-classic"></a>

<span data-ttu-id="50994-224">**Hitta tillämpningsprogrammets identitet klient-ID och klient-ID för ditt webbprogram eller API-app i hello klassiska Azure-portalen**</span><span class="sxs-lookup"><span data-stu-id="50994-224">**Find application identity's client ID and tenant ID for your web app or API app in hello Azure classic portal**</span></span>

1. <span data-ttu-id="50994-225">I Hej klassiska Azure-portalen, Välj [ **Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).</span><span class="sxs-lookup"><span data-stu-id="50994-225">In hello Azure classic portal, choose [**Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).</span></span>

2.  <span data-ttu-id="50994-226">Välj hello-katalog som du använder för ditt webbprogram eller API-app.</span><span class="sxs-lookup"><span data-stu-id="50994-226">Select hello directory that you use for your web app or API app.</span></span>

3. <span data-ttu-id="50994-227">I hello **Sök** , söka efter och välj hello programidentitet för ditt webbprogram eller API-app.</span><span class="sxs-lookup"><span data-stu-id="50994-227">In hello **Search** box, find and select hello application identity for your web app or API app.</span></span>

4. <span data-ttu-id="50994-228">På hello **konfigurera** fliken, kopiera hello **klient-ID**, och spara det GUID för användning i del 3.</span><span class="sxs-lookup"><span data-stu-id="50994-228">On hello **Configure** tab, copy hello **Client ID**, and save that GUID for use in Part 3.</span></span>

5. <span data-ttu-id="50994-229">När du har fått hello klient-ID, längst ned hello hello **konfigurera** , Välj **visa slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="50994-229">After you get hello client ID, at hello bottom of hello **Configure** tab, choose **View endpoints**.</span></span>

6. <span data-ttu-id="50994-230">Kopiera hello URL: en för **Federation Metadata dokumentet**, och bläddra toothat URL.</span><span class="sxs-lookup"><span data-stu-id="50994-230">Copy hello URL for **Federation Metadata Document**, and browse toothat URL.</span></span>

7. <span data-ttu-id="50994-231">I hello metadata dokument som öppnas, hitta hello rot **EntityDescriptor ID** element som har en **ID för entiteterna** attribut i det här formuläret:`https://sts.windows.net/{GUID}`</span><span class="sxs-lookup"><span data-stu-id="50994-231">In hello metadata document that opens, find hello root **EntityDescriptor ID** element, which has an **entityID** attribute in this form: `https://sts.windows.net/{GUID}`</span></span> 

      <span data-ttu-id="50994-232">hello GUID i det här attributet är din klient-GUID (klient-ID).</span><span class="sxs-lookup"><span data-stu-id="50994-232">hello GUID in this attribute is your specific tenant's GUID (tenant ID).</span></span>

8. <span data-ttu-id="50994-233">Kopiera hello klient-ID och spara detta ID för användning i del 3 och toouse i ditt webbprogram eller mallen för distribution av API-app om det behövs.</span><span class="sxs-lookup"><span data-stu-id="50994-233">Copy hello tenant ID and save that ID for use in Part 3, and also toouse in your web app or API app's deployment template, if necessary.</span></span>

<span data-ttu-id="50994-234">Mer information finns i följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="50994-234">For more information, see these topics:</span></span>

* [<span data-ttu-id="50994-235">Användarautentisering för API apps i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="50994-235">User authentication for API apps in Azure App Service</span></span>](../app-service-api/app-service-api-dotnet-user-principal-auth.md)
* [<span data-ttu-id="50994-236">Autentisering och auktorisering i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="50994-236">Authentication and authorization in Azure App Service</span></span>](../app-service/app-service-authentication-overview.md)

<a name="authen-deploy"></a>

<span data-ttu-id="50994-237">**Aktivera autentisering när du distribuerar med en Azure Resource Manager-mall**</span><span class="sxs-lookup"><span data-stu-id="50994-237">**Turn on authentication when you deploy with an Azure Resource Manager template**</span></span>

<span data-ttu-id="50994-238">Du måste fortfarande skapa en Azure AD-programidentitet för ditt webbprogram eller API-app som skiljer sig från hello app identitet för din logikapp.</span><span class="sxs-lookup"><span data-stu-id="50994-238">You must still create an Azure AD application identity for your web app or API app that differs from hello app identity for your logic app.</span></span> <span data-ttu-id="50994-239">toocreate hello Programidentitet, följ hello föregående steg i en del 2 för hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="50994-239">toocreate hello application identity, follow hello previous steps in Part 2 for hello Azure portal.</span></span> <span data-ttu-id="50994-240">Du kan också hello åtgärderna i del 1, men gör att toouse ditt webbprogram eller API-app faktiska `https://{URL}` för **inloggnings-URL** och **App-ID URI**.</span><span class="sxs-lookup"><span data-stu-id="50994-240">You can also follow hello steps in Part 1, but make sure toouse your web app or API app's actual `https://{URL}` for **Sign-on URL** and **App ID URI**.</span></span> <span data-ttu-id="50994-241">Från de här stegen har du toosave både hello-klient-ID och klient-ID för användning i din app Distributionsmall och för del 3.</span><span class="sxs-lookup"><span data-stu-id="50994-241">From these steps, you have toosave both hello client ID and tenant ID for use in your app's deployment template and also for Part 3.</span></span>

> [!NOTE]
> <span data-ttu-id="50994-242">När du skapar hello Azure AD programidentitet för ditt webbprogram eller API-app måste du använda hello Azure-portalen eller klassiska Azure-portalen i stället PowerShell.</span><span class="sxs-lookup"><span data-stu-id="50994-242">When you create hello Azure AD application identity for your web app or API app, you must use hello Azure portal or Azure classic portal, rather than PowerShell.</span></span> <span data-ttu-id="50994-243">hello PowerShell cmdlet ställa inte in hello krävs behörigheter toosign användare till en webbplats.</span><span class="sxs-lookup"><span data-stu-id="50994-243">hello PowerShell commandlet doesn't set up hello required permissions toosign users into a website.</span></span>

<span data-ttu-id="50994-244">När du har fått hello klient-ID och klient-ID är dessa ID: N som en subresource av ditt webbprogram eller API-app i mallen för distribution:</span><span class="sxs-lookup"><span data-stu-id="50994-244">After you get hello client ID and tenant ID, include these IDs as a subresource of your web app or API app in your deployment template:</span></span>

   ```
   "resources": [
       {
           "apiVersion": "2015-08-01",
           "name": "web",
           "type": "config",
           "dependsOn": ["[concat('Microsoft.Web/sites/','parameters('webAppName'))]"],
           "properties": {
               "siteAuthEnabled": true,
               "siteAuthSettings": {
                 "clientId": "{client-ID}",
                 "issuer": "https://sts.windows.net/{tenant-ID}/",
               }
           }
       }
   ]
   ```

<span data-ttu-id="50994-245">tooautomatically distribuera ett tomt webbprogram och en logikapp tillsammans med Azure Active Directory-autentisering, [visa hello fullständig mall här](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-custom-api/azuredeploy.json), eller klicka på **distribuera tooAzure** här:</span><span class="sxs-lookup"><span data-stu-id="50994-245">tooautomatically deploy a blank web app and a logic app together with Azure Active Directory authentication, [view hello complete template here](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-custom-api/azuredeploy.json), or click **Deploy tooAzure** here:</span></span>

<span data-ttu-id="50994-246">[![Distribuera tooAzure](media/logic-apps-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="50994-246">[![Deploy tooAzure](media/logic-apps-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)</span></span>

#### <a name="part-3-populate-hello-authorization-section-in-your-logic-app"></a><span data-ttu-id="50994-247">Del 3: Fylla hello auktorisering avsnitt i din logikapp</span><span class="sxs-lookup"><span data-stu-id="50994-247">Part 3: Populate hello Authorization section in your logic app</span></span>

<span data-ttu-id="50994-248">tidigare hello-mall använder redan det här tillståndet avsnittet Konfigurera, men om du använder direkt hello logikapp, måste du inkludera hello fullständig behörighet avsnitt.</span><span class="sxs-lookup"><span data-stu-id="50994-248">hello previous template already has this authorization section set up, but if you are directly authoring hello logic app, you must include hello full authorization section.</span></span>

<span data-ttu-id="50994-249">Öppna din app logik-definition i kodvy, gå toohello **HTTP** åtgärd, hitta hello **auktorisering** avsnittet och inkludera den här raden:</span><span class="sxs-lookup"><span data-stu-id="50994-249">Open your logic app definition in code view, go toohello **HTTP** action section, find hello **Authorization** section, and include this line:</span></span>

`{"tenant": "{tenant-ID}", "audience": "{client-ID-from-Part-2-web-app-or-API app}", "clientId": "{client-ID-from-Part-1-logic-app}", "secret": "{key-from-Part-1-logic-app}", "type": "ActiveDirectoryOAuth" }`

| <span data-ttu-id="50994-250">Element</span><span class="sxs-lookup"><span data-stu-id="50994-250">Element</span></span> | <span data-ttu-id="50994-251">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="50994-251">Description</span></span> |
| ------- | ----------- |
| <span data-ttu-id="50994-252">Klient</span><span class="sxs-lookup"><span data-stu-id="50994-252">tenant</span></span> |<span data-ttu-id="50994-253">hello GUID för hello Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="50994-253">hello GUID for hello Azure AD tenant</span></span> |
| <span data-ttu-id="50994-254">målgrupp</span><span class="sxs-lookup"><span data-stu-id="50994-254">audience</span></span> |<span data-ttu-id="50994-255">Krävs.</span><span class="sxs-lookup"><span data-stu-id="50994-255">Required.</span></span> <span data-ttu-id="50994-256">hello GUID för hello målresurs som du vill tooaccess - hello klient-ID från hello programidentitet för ditt webbprogram eller API-app</span><span class="sxs-lookup"><span data-stu-id="50994-256">hello GUID for hello target resource that you want tooaccess - hello client ID from hello application identity for your web app or API app</span></span> |
| <span data-ttu-id="50994-257">clientId</span><span class="sxs-lookup"><span data-stu-id="50994-257">clientId</span></span> |<span data-ttu-id="50994-258">hello GUID för hello-klient som begär åtkomst - hello klient-ID från hello programidentitet för din logikapp</span><span class="sxs-lookup"><span data-stu-id="50994-258">hello GUID for hello client requesting access - hello client ID from hello application identity for your logic app</span></span> |
| <span data-ttu-id="50994-259">hemligt</span><span class="sxs-lookup"><span data-stu-id="50994-259">secret</span></span> |<span data-ttu-id="50994-260">Krävs.</span><span class="sxs-lookup"><span data-stu-id="50994-260">Required.</span></span> <span data-ttu-id="50994-261">hello nyckel eller lösenord från hello programidentitet för hello-klient som begär hello åtkomst-token</span><span class="sxs-lookup"><span data-stu-id="50994-261">hello key or password from hello application identity for hello client that's requesting hello access token</span></span> |
| <span data-ttu-id="50994-262">typ</span><span class="sxs-lookup"><span data-stu-id="50994-262">type</span></span> |<span data-ttu-id="50994-263">hello autentiseringstypen.</span><span class="sxs-lookup"><span data-stu-id="50994-263">hello authentication type.</span></span> <span data-ttu-id="50994-264">Hello-värdet är för ActiveDirectoryOAuth autentisering `ActiveDirectoryOAuth`.</span><span class="sxs-lookup"><span data-stu-id="50994-264">For ActiveDirectoryOAuth authentication, hello value is `ActiveDirectoryOAuth`.</span></span> |

<span data-ttu-id="50994-265">Exempel:</span><span class="sxs-lookup"><span data-stu-id="50994-265">For example:</span></span>

```
{
   ...
   "actions": {
      "some-action": {
         "conditions": [],
         "inputs": {
            "method": "post",
            "uri": "https://your-api-azurewebsites.net/api/your-method",
            "authentication": {
               "tenant": "tenant-ID",
               "audience": "client-ID-from-azure-ad-app-for-web-app-or-api-app",
               "clientId": "client-ID-from-azure-ad-app-for-logic-app",
               "secret": "key-from-azure-ad-app-for-logic-app",
               "type": "ActiveDirectoryOAuth"
            }
         },
      }
   }
}
```

<a name="update-code"></a>

### <a name="secure-api-calls-through-code"></a><span data-ttu-id="50994-266">Säker API-anrop med kod</span><span class="sxs-lookup"><span data-stu-id="50994-266">Secure API calls through code</span></span>

<a name="certificate"></a>

#### <a name="certificate-authentication"></a><span data-ttu-id="50994-267">Certifikatautentisering</span><span class="sxs-lookup"><span data-stu-id="50994-267">Certificate authentication</span></span>

<span data-ttu-id="50994-268">toovalidate hello inkommande förfrågningar från din logik app tooyour webbapp eller API-app, kan du använda klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="50994-268">toovalidate hello incoming requests from your logic app tooyour web app or API app, you can use client certificates.</span></span> <span data-ttu-id="50994-269">Läs tooset upp din kod [hur tooconfigure TLS ömsesidig autentisering](../app-service-web/app-service-web-configure-tls-mutual-auth.md).</span><span class="sxs-lookup"><span data-stu-id="50994-269">tooset up your code, learn [how tooconfigure TLS mutual authentication](../app-service-web/app-service-web-configure-tls-mutual-auth.md).</span></span>

<span data-ttu-id="50994-270">I hello **auktorisering** och inkludera den här raden:</span><span class="sxs-lookup"><span data-stu-id="50994-270">In hello **Authorization** section, include this line:</span></span> 

`{"type": "clientcertificate", "password": "password", "pfx": "long-pfx-key"}`

| <span data-ttu-id="50994-271">Element</span><span class="sxs-lookup"><span data-stu-id="50994-271">Element</span></span> | <span data-ttu-id="50994-272">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="50994-272">Description</span></span> |
| ------- | ----------- |
| <span data-ttu-id="50994-273">typ</span><span class="sxs-lookup"><span data-stu-id="50994-273">type</span></span> |<span data-ttu-id="50994-274">Krävs.</span><span class="sxs-lookup"><span data-stu-id="50994-274">Required.</span></span> <span data-ttu-id="50994-275">hello autentiseringstypen.</span><span class="sxs-lookup"><span data-stu-id="50994-275">hello authentication type.</span></span> <span data-ttu-id="50994-276">För SSL-klientcertifikat, hello-värdet måste vara `ClientCertificate`.</span><span class="sxs-lookup"><span data-stu-id="50994-276">For SSL client certificates, hello value must be `ClientCertificate`.</span></span> |
| <span data-ttu-id="50994-277">lösenord</span><span class="sxs-lookup"><span data-stu-id="50994-277">password</span></span> |<span data-ttu-id="50994-278">Krävs.</span><span class="sxs-lookup"><span data-stu-id="50994-278">Required.</span></span> <span data-ttu-id="50994-279">hello-lösenord för att komma åt hello-klientcertifikat (PFX-fil)</span><span class="sxs-lookup"><span data-stu-id="50994-279">hello password for accessing hello client certificate (PFX file)</span></span> |
| <span data-ttu-id="50994-280">Pfx</span><span class="sxs-lookup"><span data-stu-id="50994-280">pfx</span></span> |<span data-ttu-id="50994-281">Krävs.</span><span class="sxs-lookup"><span data-stu-id="50994-281">Required.</span></span> <span data-ttu-id="50994-282">Base64-kodad innehållet i hello-klientcertifikat (PFX-fil)</span><span class="sxs-lookup"><span data-stu-id="50994-282">Base64-encoded contents of hello client certificate (PFX file)</span></span> |

<a name="basic"></a>

#### <a name="basic-authentication"></a><span data-ttu-id="50994-283">Grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="50994-283">Basic authentication</span></span>

<span data-ttu-id="50994-284">toovalidate inkommande förfrågningar från din logik app tooyour webbapp eller API-app, kan du använda grundläggande autentisering, till exempel användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="50994-284">toovalidate incoming requests from your logic app tooyour web app or API app, you can use basic authentication, such as a username and password.</span></span> <span data-ttu-id="50994-285">Grundläggande autentisering är ett vanligt mönster och du kan använda den här autentiseringen i alla språk som används för toobuild ditt webbprogram eller API-app.</span><span class="sxs-lookup"><span data-stu-id="50994-285">Basic authentication is a common pattern, and you can use this authentication in any language used toobuild your web app or API app.</span></span>

<span data-ttu-id="50994-286">I hello **auktorisering** och inkludera den här raden:</span><span class="sxs-lookup"><span data-stu-id="50994-286">In hello **Authorization** section, include this line:</span></span>

<span data-ttu-id="50994-287">`{"type": "basic", "username": "username", "password": "password"}`.</span><span class="sxs-lookup"><span data-stu-id="50994-287">`{"type": "basic", "username": "username", "password": "password"}`.</span></span>

| <span data-ttu-id="50994-288">Element</span><span class="sxs-lookup"><span data-stu-id="50994-288">Element</span></span> | <span data-ttu-id="50994-289">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="50994-289">Description</span></span> |
| --- | --- |
| <span data-ttu-id="50994-290">typ</span><span class="sxs-lookup"><span data-stu-id="50994-290">type</span></span> |<span data-ttu-id="50994-291">Krävs.</span><span class="sxs-lookup"><span data-stu-id="50994-291">Required.</span></span> <span data-ttu-id="50994-292">hello autentiseringstypen.</span><span class="sxs-lookup"><span data-stu-id="50994-292">hello authentication type.</span></span> <span data-ttu-id="50994-293">För grundläggande autentisering hello-värdet måste vara `Basic`.</span><span class="sxs-lookup"><span data-stu-id="50994-293">For basic authentication, hello value must be `Basic`.</span></span> |
| <span data-ttu-id="50994-294">användarnamn</span><span class="sxs-lookup"><span data-stu-id="50994-294">username</span></span> |<span data-ttu-id="50994-295">Krävs.</span><span class="sxs-lookup"><span data-stu-id="50994-295">Required.</span></span> <span data-ttu-id="50994-296">hello användarnamn för autentisering</span><span class="sxs-lookup"><span data-stu-id="50994-296">hello username for authentication</span></span> |
| <span data-ttu-id="50994-297">lösenord</span><span class="sxs-lookup"><span data-stu-id="50994-297">password</span></span> |<span data-ttu-id="50994-298">Krävs.</span><span class="sxs-lookup"><span data-stu-id="50994-298">Required.</span></span> <span data-ttu-id="50994-299">hello lösenord för autentisering</span><span class="sxs-lookup"><span data-stu-id="50994-299">hello password for authentication</span></span> |

<a name="azure-ad-code"></a>

#### <a name="azure-active-directory-authentication-through-code"></a><span data-ttu-id="50994-300">Azure Active Directory-autentisering via kod</span><span class="sxs-lookup"><span data-stu-id="50994-300">Azure Active Directory authentication through code</span></span>

<span data-ttu-id="50994-301">Som standard ger inte hello Azure AD-autentisering som du aktiverar i hello Azure-portalen detaljerad behörighet.</span><span class="sxs-lookup"><span data-stu-id="50994-301">By default, hello Azure AD authentication that you turn on in hello Azure portal doesn't provide fine-grained authorization.</span></span> <span data-ttu-id="50994-302">Den här autentiseringen låser exempelvis API-toojust en viss klient, inte tooa specifik användare eller app.</span><span class="sxs-lookup"><span data-stu-id="50994-302">For example, this authentication locks your API toojust a specific tenant, not tooa specific user or app.</span></span> 

<span data-ttu-id="50994-303">toorestrict API åtkomst tooyour logikapp via kod, extrahera hello-huvudet som har hello JSON web token (JWT).</span><span class="sxs-lookup"><span data-stu-id="50994-303">toorestrict API access tooyour logic app through code, extract hello header that has hello JSON web token (JWT).</span></span> <span data-ttu-id="50994-304">Kontrollera hello Anroparens identitet och avvisar begäranden som inte matchar.</span><span class="sxs-lookup"><span data-stu-id="50994-304">Check hello caller's identity, and reject requests that don't match.</span></span>

<span data-ttu-id="50994-305">Dessutom kommer tooimplement den här autentiseringen helt i din egen kod och inte använda hello Azure-portalen Lär dig hur för [autentiseras med lokal Active Directory i din Azure-app](../app-service-web/web-sites-authentication-authorization.md).</span><span class="sxs-lookup"><span data-stu-id="50994-305">Going further, tooimplement this authentication entirely in your own code, and not use hello Azure portal, learn how too [authenticate with on-premises Active Directory in your Azure app](../app-service-web/web-sites-authentication-authorization.md).</span></span>

<span data-ttu-id="50994-306">toocreate en programidentitet för din logikapp och använda den identitet toocall ditt API, måste du följa hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="50994-306">toocreate an application identity for your logic app and use that identity toocall your API, you must follow hello previous steps.</span></span>

## <a name="next-steps"></a><span data-ttu-id="50994-307">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="50994-307">Next steps</span></span>

* [<span data-ttu-id="50994-308">Kontrollera logik app prestanda med diagnostiska loggar och aviseringar</span><span class="sxs-lookup"><span data-stu-id="50994-308">Check logic app performance with diagnostic logs and alerts</span></span>](logic-apps-monitor-your-logic-apps.md)