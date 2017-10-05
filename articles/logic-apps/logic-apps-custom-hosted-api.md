---
title: "Distribuera, anropa och autentisera webb-API: er och REST API: er för Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: 88c62d5ab046d8cf4bce5d23b776e517bb0e1d87
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-call-and-authenticate-custom-apis-as-connectors-for-logic-apps"></a><span data-ttu-id="f137c-104">Distribuera, anropa och autentisera anpassade API: er som kopplingar för logic apps</span><span class="sxs-lookup"><span data-stu-id="f137c-104">Deploy, call, and authenticate custom APIs as connectors for logic apps</span></span>

<span data-ttu-id="f137c-105">När du [skapa anpassade API: er](./logic-apps-create-api-app.md) som tillhandahåller åtgärder eller utlösare i logic apps arbetsflöden, måste du distribuera dina API: er innan du kan anropa dem.</span><span class="sxs-lookup"><span data-stu-id="f137c-105">After you [create custom APIs](./logic-apps-create-api-app.md) that provide actions or triggers to use in logic apps workflows, you must deploy your APIs before you can call them.</span></span> <span data-ttu-id="f137c-106">Och även om du kan anropa API: er från en logikapp för bästa möjliga upplevelse, lägger du till [Swagger-metadata](http://swagger.io/specification/) som beskriver ditt API åtgärder och parametrar.</span><span class="sxs-lookup"><span data-stu-id="f137c-106">And although you can call any API from a logic app, for the best experience, add [Swagger metadata](http://swagger.io/specification/) that describes your API's operations and parameters.</span></span> <span data-ttu-id="f137c-107">Den här Swagger-filen hjälper din API fungerar bättre och enklare integrera med logic apps.</span><span class="sxs-lookup"><span data-stu-id="f137c-107">This Swagger file helps your API work better and integrate more easily with logic apps.</span></span>

<span data-ttu-id="f137c-108">Du kan distribuera dina API: er som [webbappar](../app-service-web/app-service-web-overview.md), men överväga att distribuera dina API: er som [API apps](../app-service-api/app-service-api-apps-why-best-platform.md), som kan underlätta ditt jobb när du skapar, värd och använda API: er i molnet och lokalt.</span><span class="sxs-lookup"><span data-stu-id="f137c-108">You can deploy your APIs as [web apps](../app-service-web/app-service-web-overview.md), but consider deploying your APIs as [API apps](../app-service-api/app-service-api-apps-why-best-platform.md), which can make your job easier when you build, host, and consume APIs in the cloud and on premises.</span></span> <span data-ttu-id="f137c-109">Du behöver ändra någon kod i dina API: er, distribuera bara din kod till API-app.</span><span class="sxs-lookup"><span data-stu-id="f137c-109">You don't have to change any code in your APIs -- just deploy your code to an API app.</span></span> <span data-ttu-id="f137c-110">Du kan vara värd för dina API: er på [Azure App Service](../app-service/app-service-value-prop-what-is.md), en plattform-som-en tjänst (PaaS) erbjudande som ger ett sätt bästa enklaste och mest skalbara för API-värd.</span><span class="sxs-lookup"><span data-stu-id="f137c-110">You can host your APIs on [Azure App Service](../app-service/app-service-value-prop-what-is.md), a platform-as-a-service (PaaS) offering that provides one of the best, easiest, and most scalable ways for API hosting.</span></span>

<span data-ttu-id="f137c-111">Om du vill autentisera anrop från logikappar till dina API: er, kan du ställa in Azure Active Directory i Azure-portalen så du behöver uppdatera din kod.</span><span class="sxs-lookup"><span data-stu-id="f137c-111">To authenticate calls from logic apps to your APIs, you can set up Azure Active Directory in the Azure portal so you don't have to update your code.</span></span> <span data-ttu-id="f137c-112">Eller så du behöver och ställer in användarautentisering via din API-koden.</span><span class="sxs-lookup"><span data-stu-id="f137c-112">Or, you can require and enforce authentication through your API's code.</span></span>

## <a name="deploy-your-api-as-a-web-app-or-api-app"></a><span data-ttu-id="f137c-113">Distribuera din API som en webbapp eller API-app</span><span class="sxs-lookup"><span data-stu-id="f137c-113">Deploy your API as a web app or API app</span></span>

<span data-ttu-id="f137c-114">Innan du kan anropa ditt anpassade API från en logikapp, kan du distribuera din API som en webbapp eller API-app till Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="f137c-114">Before you can call your custom API from a logic app, deploy your API as a web app or API app to Azure App Service.</span></span> <span data-ttu-id="f137c-115">Även om du vill göra Swagger-dokument läsas av logik App Designer ange egenskaper för API-definition och aktivera [resursdelning för korsande ursprung (CORS)](../app-service-api/app-service-api-cors-consume-javascript.md#corsconfig) för ditt webbprogram eller API-app.</span><span class="sxs-lookup"><span data-stu-id="f137c-115">Also, to make your Swagger document readable by the Logic App Designer, set the API definition properties and turn on [cross-origin resource sharing (CORS)](../app-service-api/app-service-api-cors-consume-javascript.md#corsconfig) for your web app or API app.</span></span>

1. <span data-ttu-id="f137c-116">Välj ditt webbprogram eller API-app i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f137c-116">In the Azure portal, select your web app or API app.</span></span>

2. <span data-ttu-id="f137c-117">I bladet som öppnas under **API**, Välj **API-definition**.</span><span class="sxs-lookup"><span data-stu-id="f137c-117">In the blade that opens, under **API**, choose **API definition**.</span></span> <span data-ttu-id="f137c-118">Ange den **plats för API-definition** till URL: en för swagger.json-filen.</span><span class="sxs-lookup"><span data-stu-id="f137c-118">Set the **API definition location** to the URL for your swagger.json file.</span></span>

   <span data-ttu-id="f137c-119">Webbadressen visas vanligtvis i det här formatet:`https://{name}.azurewebsites.net/swagger/docs/v1)`</span><span class="sxs-lookup"><span data-stu-id="f137c-119">Usually, the URL appears in this format: `https://{name}.azurewebsites.net/swagger/docs/v1)`</span></span>

   ![Länka till Swagger-filen för din anpassade API](media/logic-apps-custom-hosted-api/custom-api-swagger-url.png)

3. <span data-ttu-id="f137c-121">Under **API**, Välj **CORS**.</span><span class="sxs-lookup"><span data-stu-id="f137c-121">Under **API**, choose **CORS**.</span></span> <span data-ttu-id="f137c-122">Skapa princip för CORS för **tillåtna ursprung** till  **'*'** (Tillåt alla).</span><span class="sxs-lookup"><span data-stu-id="f137c-122">Set the CORS policy for **Allowed origins** to **'*'** (allow all).</span></span>

   <span data-ttu-id="f137c-123">Den här inställningen tillåter begäranden från logik App Designer.</span><span class="sxs-lookup"><span data-stu-id="f137c-123">This setting permits requests from Logic App Designer.</span></span>

   ![Tillåt begäranden från logik App Designer för ditt anpassade API](media/logic-apps-custom-hosted-api/custom-api-cors.png)

<span data-ttu-id="f137c-125">Mer information finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="f137c-125">For more information, see these articles:</span></span>

* [<span data-ttu-id="f137c-126">Lägg till Swagger-metadata för ASP.NET web API: er</span><span class="sxs-lookup"><span data-stu-id="f137c-126">Add Swagger metadata for ASP.NET web APIs</span></span>](../app-service-api/app-service-api-dotnet-get-started.md#use-swagger-api-metadata-and-ui)
* [<span data-ttu-id="f137c-127">Distribuera ASP.NET web API: er till Azure App Service</span><span class="sxs-lookup"><span data-stu-id="f137c-127">Deploy ASP.NET web APIs to Azure App Service</span></span>](../app-service-api/app-service-api-dotnet-get-started.md)

## <a name="call-your-custom-api-from-logic-app-workflows"></a><span data-ttu-id="f137c-128">Anropa ditt anpassade API från logik app arbetsflöden</span><span class="sxs-lookup"><span data-stu-id="f137c-128">Call your custom API from logic app workflows</span></span>

<span data-ttu-id="f137c-129">När du ställer in egenskaperna för API-definition och CORS ska din anpassade API utlösare och åtgärder bli tillgänglig att inkludera i logik app arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="f137c-129">After you set up the API definition properties and CORS, your custom API's triggers and actions should become available for you to include in your logic app workflow.</span></span> 

*  <span data-ttu-id="f137c-130">Om du vill visa de webbplatser som har Swagger URL: er kan du bläddra din prenumeration webbplatser i Logic Apps Designer.</span><span class="sxs-lookup"><span data-stu-id="f137c-130">To view the websites that have Swagger URLs, you can browse your subscription websites in the Logic Apps Designer.</span></span>

*  <span data-ttu-id="f137c-131">Du kan visa tillgängliga åtgärder och indata genom att peka på en Swagger-dokument i [HTTP + Swagger åtgärden](../connectors/connectors-native-http-swagger.md).</span><span class="sxs-lookup"><span data-stu-id="f137c-131">To view available actions and inputs by pointing at a Swagger document, use the [HTTP + Swagger action](../connectors/connectors-native-http-swagger.md).</span></span>

*  <span data-ttu-id="f137c-132">För att anropa API: er, inklusive API: er som inte har eller exponera en Swagger-dokument kan du alltid skapa en begäran med den [HTTP-åtgärden](../connectors/connectors-native-http.md).</span><span class="sxs-lookup"><span data-stu-id="f137c-132">To call any API, including APIs that don't have or expose a Swagger document, you can always create a request with the [HTTP action](../connectors/connectors-native-http.md).</span></span>

## <a name="authenticate-calls-to-your-custom-api"></a><span data-ttu-id="f137c-133">Autentisera anrop till din anpassade API</span><span class="sxs-lookup"><span data-stu-id="f137c-133">Authenticate calls to your custom API</span></span>

<span data-ttu-id="f137c-134">Du kan skydda anrop till din anpassade API på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f137c-134">You can secure calls to your custom API in these ways:</span></span>

* <span data-ttu-id="f137c-135">[Inga kodändringar](#no-code): skydda din API med [Azure Active Directory (AD Azure)](../active-directory/active-directory-whatis.md) via Azure-portalen så du behöver uppdatera din kod eller distribuera din API.</span><span class="sxs-lookup"><span data-stu-id="f137c-135">[No code changes](#no-code): Protect your API with [Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md) through the Azure portal, so you don't have to update your code or redeploy your API.</span></span>

  > [!NOTE]
  > <span data-ttu-id="f137c-136">Som standard ger inte Azure AD-autentisering som du aktiverar i Azure-portalen detaljerad behörighet.</span><span class="sxs-lookup"><span data-stu-id="f137c-136">By default, the Azure AD authentication that you turn on in the Azure portal doesn't provide fine-grained authorization.</span></span> <span data-ttu-id="f137c-137">Den här autentiseringen låser exempelvis din API till bara en viss klient, inte till en specifik användare eller en app.</span><span class="sxs-lookup"><span data-stu-id="f137c-137">For example, this authentication locks your API to just a specific tenant, not to a specific user or app.</span></span> 

* <span data-ttu-id="f137c-138">[Uppdatera din API-koden](#update-code): skydda din API genom tvingande [certifikatautentisering](#certificate), [grundläggande autentisering](#basic), eller [Azure AD authentication](#azure-ad-code) via kod.</span><span class="sxs-lookup"><span data-stu-id="f137c-138">[Update your API's code](#update-code): Protect your API by enforcing [certificate authentication](#certificate), [basic authentication](#basic), or [Azure AD authentication](#azure-ad-code) through code.</span></span>

<a name="no-code"></a>

### <a name="authenticate-calls-to-your-api-without-changing-code"></a><span data-ttu-id="f137c-139">Autentisera anrop till ditt API utan att ändra koden</span><span class="sxs-lookup"><span data-stu-id="f137c-139">Authenticate calls to your API without changing code</span></span>

<span data-ttu-id="f137c-140">Här är de allmänna stegen för den här metoden:</span><span class="sxs-lookup"><span data-stu-id="f137c-140">Here's the general steps for this method:</span></span>

1. <span data-ttu-id="f137c-141">Skapa två [Azure Active Directory (Azure AD) application identiteter](../app-service-api/app-service-api-dotnet-service-principal-auth.md): en för din logikapp och en för webbprogram (eller API-app).</span><span class="sxs-lookup"><span data-stu-id="f137c-141">Create two [Azure Active Directory (Azure AD) application identities](../app-service-api/app-service-api-dotnet-service-principal-auth.md): one for your logic app and one for your web app (or API app).</span></span>

2. <span data-ttu-id="f137c-142">Om du vill autentisera anrop till ditt API, använda autentiseringsuppgifter (klient-ID och hemligt) för den [tjänstens huvudnamn](../app-service-api/app-service-api-dotnet-service-principal-auth.md) som associeras med Azure AD-programidentitet för din logikapp.</span><span class="sxs-lookup"><span data-stu-id="f137c-142">To authenticate calls to your API, use the credentials (client ID and secret) for the [service principal](../app-service-api/app-service-api-dotnet-service-principal-auth.md) that's associated with the Azure AD application identity for your logic app.</span></span>

3. <span data-ttu-id="f137c-143">Inkludera program-ID: N i din app logik-definition.</span><span class="sxs-lookup"><span data-stu-id="f137c-143">Include the application IDs in your logic app definition.</span></span>

#### <a name="part-1-create-an-azure-ad-application-identity-for-your-logic-app"></a><span data-ttu-id="f137c-144">Del 1: Skapa en Azure AD application identitet för din logikapp</span><span class="sxs-lookup"><span data-stu-id="f137c-144">Part 1: Create an Azure AD application identity for your logic app</span></span>

<span data-ttu-id="f137c-145">Din logikapp använder den här identiteten för Azure AD-program för att autentisera mot Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f137c-145">Your logic app uses this Azure AD application identity to authenticate against Azure AD.</span></span> <span data-ttu-id="f137c-146">Du behöver bara konfigurera den här identiteten en gång för din katalog.</span><span class="sxs-lookup"><span data-stu-id="f137c-146">You only have to set up this identity one time for your directory.</span></span> <span data-ttu-id="f137c-147">Exempelvis kan du använda samma identitet för dina logikappar trots att du kan skapa unika identiteter för varje logikapp.</span><span class="sxs-lookup"><span data-stu-id="f137c-147">For example, you can choose to use the same identity for all your logic apps, even though you can create unique identities for each logic app.</span></span> <span data-ttu-id="f137c-148">Du kan konfigurera dessa identiteter i Azure-portalen [klassiska Azure-portalen](#app-identity-logic-classic), eller Använd [PowerShell](#powershell).</span><span class="sxs-lookup"><span data-stu-id="f137c-148">You can set up these identities in the Azure portal, [Azure classic portal](#app-identity-logic-classic), or use [PowerShell](#powershell).</span></span>

<span data-ttu-id="f137c-149">**Skapa programidentiteten för logikappen i Azure-portalen**</span><span class="sxs-lookup"><span data-stu-id="f137c-149">**Create the application identity for your logic app in the Azure portal**</span></span>

1. <span data-ttu-id="f137c-150">I den [Azure-portalen](https://portal.azure.com "https://portal.azure.com"), Välj **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="f137c-150">In the [Azure portal](https://portal.azure.com "https://portal.azure.com"), choose **Azure Active Directory**.</span></span> 

2. <span data-ttu-id="f137c-151">Bekräfta att du är i samma katalog som ditt webbprogram eller API-app.</span><span class="sxs-lookup"><span data-stu-id="f137c-151">Confirm that you're in the same directory as your web app or API app.</span></span>

   > [!TIP]
   > <span data-ttu-id="f137c-152">Om du vill växla kataloger på din profil och välj en annan katalog.</span><span class="sxs-lookup"><span data-stu-id="f137c-152">To switch directories, click your profile and select another directory.</span></span> <span data-ttu-id="f137c-153">Eller välj **översikt** > **växel directory**.</span><span class="sxs-lookup"><span data-stu-id="f137c-153">Or, choose **Overview** > **Switch directory**.</span></span>

3. <span data-ttu-id="f137c-154">På menyn directory under **hantera**, Välj **App registreringar** > **nya appregistrering**.</span><span class="sxs-lookup"><span data-stu-id="f137c-154">On the directory menu, under **Manage**, choose **App registrations** > **New application registration**.</span></span>

   > [!TIP]
   > <span data-ttu-id="f137c-155">Som standard visar listan över appar registreringar alla app registreringar i din katalog.</span><span class="sxs-lookup"><span data-stu-id="f137c-155">By default, the app registrations list shows all app registrations in your directory.</span></span> <span data-ttu-id="f137c-156">Om du vill visa endast appen registreringar, bredvid sökrutan **Mina appar**.</span><span class="sxs-lookup"><span data-stu-id="f137c-156">To view only your app registrations, next to the search box, select **My apps**.</span></span> 

   ![Skapa ny appregistrering](./media/logic-apps-custom-hosted-api/new-app-registration-azure-portal.png)

4. <span data-ttu-id="f137c-158">Namnge din Programidentitet, lämna **programtyp** inställd på **webbapp / API**, ange en unik sträng formaterad som en domän för **inloggnings-URL**, och välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="f137c-158">Give your application identity a name, leave **Application type** set to **Web app / API**, provide a unique string formatted as a domain for **Sign-on URL**, and choose **Create**.</span></span>

   ![Ange namn och inloggning URL för Programidentitet](./media/logic-apps-custom-hosted-api/logic-app-identity-azure-portal.png)

   <span data-ttu-id="f137c-160">Programidentiteten som du skapade för din logikapp nu visas i listan över appar registreringar.</span><span class="sxs-lookup"><span data-stu-id="f137c-160">The application identity that you created for your logic app now appears in the app registrations list.</span></span>

   ![Tillämpningsprogrammets identitet för din logikapp](./media/logic-apps-custom-hosted-api/logic-app-identity-created.png)

5. <span data-ttu-id="f137c-162">Välj din nya Programidentitet i programlistan registreringar.</span><span class="sxs-lookup"><span data-stu-id="f137c-162">In the app registrations list, select your new application identity.</span></span> <span data-ttu-id="f137c-163">Kopiera och spara den **program-ID** ska användas som ”klient-ID” för din logikapp i del 3.</span><span class="sxs-lookup"><span data-stu-id="f137c-163">Copy and save the **Application ID** to use as the "client ID" for your logic app in Part 3.</span></span>

   ![Kopiera och spara program-ID för logikapp](./media/logic-apps-custom-hosted-api/logic-app-application-id.png)

6. <span data-ttu-id="f137c-165">Om din identitet programinställningar inte visas, väljer **inställningar** eller **alla inställningar**.</span><span class="sxs-lookup"><span data-stu-id="f137c-165">If your application identity settings aren't visible, choose **Settings** or **All settings**.</span></span>

7. <span data-ttu-id="f137c-166">Under **API-åtkomst**, Välj **nycklar**.</span><span class="sxs-lookup"><span data-stu-id="f137c-166">Under **API Access**, choose **Keys**.</span></span> <span data-ttu-id="f137c-167">Under **beskrivning**, ange ett namn för din nyckel.</span><span class="sxs-lookup"><span data-stu-id="f137c-167">Under **Description**, provide a name for your key.</span></span> <span data-ttu-id="f137c-168">Under **Expires**, markera en varaktighet för din nyckel.</span><span class="sxs-lookup"><span data-stu-id="f137c-168">Under **Expires**, select a duration for your key.</span></span>

   <span data-ttu-id="f137c-169">Den nyckel som du skapar fungerar som programidentiteten ”hemliga” eller lösenordet för din logikapp.</span><span class="sxs-lookup"><span data-stu-id="f137c-169">The key that you're creating acts as the application identity's "secret" or password for your logic app.</span></span>

   ![Skapa en nyckel för logik app identitet](./media/logic-apps-custom-hosted-api/create-logic-app-identity-key-secret-password.png)

8. <span data-ttu-id="f137c-171">I verktygsfältet, välja **spara**.</span><span class="sxs-lookup"><span data-stu-id="f137c-171">On the toolbar, choose **Save**.</span></span> <span data-ttu-id="f137c-172">Under **värdet**, nyckeln visas nu.</span><span class="sxs-lookup"><span data-stu-id="f137c-172">Under **Value**, your key now appears.</span></span> 
<span data-ttu-id="f137c-173">**Se till att kopiera och spara din nyckel** för senare användning eftersom nyckeln döljs när du lämnar bladet nycklar.</span><span class="sxs-lookup"><span data-stu-id="f137c-173">**Make sure to copy and save your key** for later use because the key is hidden when you leave the key blade.</span></span>

   <span data-ttu-id="f137c-174">När du konfigurerar din logikapp i del 3 kan ange du den här nyckeln ”secret” eller lösenord.</span><span class="sxs-lookup"><span data-stu-id="f137c-174">When you configure your logic app in Part 3, you specify this key as the "secret" or password.</span></span>

   ![Kopiera och spara nyckeln för senare](./media/logic-apps-custom-hosted-api/logic-app-copy-key-secret-password.png)

<a name="app-identity-logic-classic"></a>

<span data-ttu-id="f137c-176">**Skapa programidentiteten för din logikapp i den klassiska Azure-portalen**</span><span class="sxs-lookup"><span data-stu-id="f137c-176">**Create the application identity for your logic app in the Azure classic portal**</span></span>

1. <span data-ttu-id="f137c-177">I den klassiska Azure-portalen väljer [ **Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).</span><span class="sxs-lookup"><span data-stu-id="f137c-177">In the Azure classic portal, choose [**Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).</span></span>

2. <span data-ttu-id="f137c-178">Välj den katalog som du använder för ditt webbprogram eller API-app.</span><span class="sxs-lookup"><span data-stu-id="f137c-178">Select the same directory that you use for your web app or API app.</span></span>

3. <span data-ttu-id="f137c-179">På den **program** , Välj **Lägg till** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="f137c-179">On the **Applications** tab, choose **Add** at the bottom of the page.</span></span>

4. <span data-ttu-id="f137c-180">Namnge din identitet för programmet och välj **nästa** (HÖGERPIL).</span><span class="sxs-lookup"><span data-stu-id="f137c-180">Give your application identity a name, and choose **Next** (right arrow).</span></span>

5. <span data-ttu-id="f137c-181">Under **appegenskaper**, ange en unik sträng formaterad som en domän för **inloggnings-URL** och **App-ID URI**, och välj **Slutför** (markering).</span><span class="sxs-lookup"><span data-stu-id="f137c-181">Under **App properties**, provide a unique string formatted as a domain for **Sign-on URL** and **App ID URI**, and choose **Complete** (checkmark).</span></span>

6. <span data-ttu-id="f137c-182">På den **konfigurera** fliken, kopiera och spara den **klient-ID** för din logikapp ska användas i en del 3.</span><span class="sxs-lookup"><span data-stu-id="f137c-182">On the **Configure** tab, copy and save the **Client ID** for your logic app to use in Part 3.</span></span>

7. <span data-ttu-id="f137c-183">Under **nycklar**öppnar den **Markera varaktighet** lista.</span><span class="sxs-lookup"><span data-stu-id="f137c-183">Under **Keys**, open the **Select duration** list.</span></span> <span data-ttu-id="f137c-184">Markera en varaktighet för din nyckel.</span><span class="sxs-lookup"><span data-stu-id="f137c-184">Select a duration for your key.</span></span>

   <span data-ttu-id="f137c-185">Den nyckel som du skapar fungerar som programidentiteten ”hemliga” eller lösenordet för din logikapp.</span><span class="sxs-lookup"><span data-stu-id="f137c-185">The key that you're creating acts as the application identity's "secret" or password for your logic app.</span></span>

8. <span data-ttu-id="f137c-186">Längst ned på sidan Välj **spara**.</span><span class="sxs-lookup"><span data-stu-id="f137c-186">At the bottom of the page, choose **Save**.</span></span> <span data-ttu-id="f137c-187">Du kan behöva vänta några sekunder.</span><span class="sxs-lookup"><span data-stu-id="f137c-187">You might have to wait a few seconds.</span></span>

9. <span data-ttu-id="f137c-188">Under **nycklar**, Kom ihåg att kopiera och spara krypteringsnyckeln som nu visas.</span><span class="sxs-lookup"><span data-stu-id="f137c-188">Under **Keys**, make sure to copy and save the key that now appears.</span></span> 

   <span data-ttu-id="f137c-189">När du konfigurerar din logikapp i del 3 kan ange du den här nyckeln ”secret” eller lösenord.</span><span class="sxs-lookup"><span data-stu-id="f137c-189">When you configure your logic app in Part 3, you specify this key as the "secret" or password.</span></span>

<span data-ttu-id="f137c-190">Mer information lär du dig hur du [konfigurera din App Service-program att använda Azure Active Directory-inloggningen](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="f137c-190">For more information, learn how to [configure your App Service application to use Azure Active Directory login](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).</span></span>

<a name="powershell"></a>

<span data-ttu-id="f137c-191">**Skapa programidentiteten för din logikapp i PowerShell**</span><span class="sxs-lookup"><span data-stu-id="f137c-191">**Create the application identity for your logic app in PowerShell**</span></span>

<span data-ttu-id="f137c-192">Du kan utföra den här uppgiften via Azure Resource Manager med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f137c-192">You can perform this task through Azure Resource Manager with PowerShell.</span></span> <span data-ttu-id="f137c-193">I PowerShell kör du följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="f137c-193">In PowerShell, run these commands:</span></span>

1. `Switch-AzureMode AzureResourceManager`

2. `Add-AzureAccount`

3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://mydomain.tld" -IdentifierUris "http://mydomain.tld" -Password "identity-password"`

4. <span data-ttu-id="f137c-194">Kom ihåg att kopiera den **klient-ID** (GUID för din Azure AD-klient), den **program-ID**, och det lösenord som du använde.</span><span class="sxs-lookup"><span data-stu-id="f137c-194">Make sure to copy the **Tenant ID** (GUID for your Azure AD tenant), the **Application ID**, and the password that you used.</span></span>

<span data-ttu-id="f137c-195">Mer information lär du dig hur du [skapar ett huvudnamn för tjänsten med PowerShell för att komma åt resurser](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="f137c-195">For more information, learn how to [create a service principal with PowerShell to access resources](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span></span>

#### <a name="part-2-create-an-azure-ad-application-identity-for-your-web-app-or-api-app"></a><span data-ttu-id="f137c-196">Del 2: Skapa en Azure AD-programidentitet för ditt webbprogram eller API-app</span><span class="sxs-lookup"><span data-stu-id="f137c-196">Part 2: Create an Azure AD application identity for your web app or API app</span></span>

<span data-ttu-id="f137c-197">Om ditt webbprogram eller API-app redan har distribuerats, kan du aktivera autentisering och skapa programidentiteten i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f137c-197">If your web app or API app is already deployed, you can turn on authentication and create the application identity in the Azure portal.</span></span> <span data-ttu-id="f137c-198">Annars kan du [aktivera autentisering när du distribuerar med en Azure Resource Manager-mall](#authen-deploy).</span><span class="sxs-lookup"><span data-stu-id="f137c-198">Otherwise, you can [turn on authentication when you deploy with an Azure Resource Manager template](#authen-deploy).</span></span> 

<span data-ttu-id="f137c-199">**Skapa programidentiteten och aktivera autentisering i Azure portal för distribuerade appar**</span><span class="sxs-lookup"><span data-stu-id="f137c-199">**Create the application identity and turn on authentication in the Azure portal for deployed apps**</span></span>

1. <span data-ttu-id="f137c-200">I den [Azure-portalen](https://portal.azure.com "https://portal.azure.com"), söka efter och välj webbprogram eller API-app.</span><span class="sxs-lookup"><span data-stu-id="f137c-200">In the [Azure portal](https://portal.azure.com "https://portal.azure.com"), find and select your web app or API app.</span></span> 

2. <span data-ttu-id="f137c-201">Under **inställningar**, Välj **autentisering/auktorisering**.</span><span class="sxs-lookup"><span data-stu-id="f137c-201">Under **Settings**, choose **Authentication/Authorization**.</span></span> <span data-ttu-id="f137c-202">Under **App autentiseringen av tjänsten**, aktivera autentisering **på**.</span><span class="sxs-lookup"><span data-stu-id="f137c-202">Under **App Service Authentication**, turn authentication **On**.</span></span> <span data-ttu-id="f137c-203">Under **autentiseringsproviders**, Välj **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="f137c-203">Under **Authentication Providers**, choose **Azure Active Directory**.</span></span>

   ![Aktivera autentisering](./media/logic-apps-custom-hosted-api/custom-web-api-app-authentication.png)

3. <span data-ttu-id="f137c-205">Nu skapa en programidentitet för ditt webbprogram eller API-app som visas här.</span><span class="sxs-lookup"><span data-stu-id="f137c-205">Now create an application identity for your web app or API app as shown here.</span></span> <span data-ttu-id="f137c-206">På den **inställningarna för Azure Active Directory** bladet ange **hanteringsläge** till **Express**.</span><span class="sxs-lookup"><span data-stu-id="f137c-206">On the **Azure Active Directory Settings** blade, set **Management mode** to **Express**.</span></span> <span data-ttu-id="f137c-207">Välj **Skapa nytt AD-App**.</span><span class="sxs-lookup"><span data-stu-id="f137c-207">Choose **Create New AD App**.</span></span> <span data-ttu-id="f137c-208">Namnge din identitet för programmet och välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="f137c-208">Give your application identity a name, and choose **OK**.</span></span> 

   ![Skapa programidentitet för ditt webbprogram eller API-app](./media/logic-apps-custom-hosted-api/custom-api-application-identity.png)

4. <span data-ttu-id="f137c-210">På den **autentisering / auktorisering bladet**, Välj **spara**.</span><span class="sxs-lookup"><span data-stu-id="f137c-210">On the **Authentication / Authorization blade**, choose **Save**.</span></span>

<span data-ttu-id="f137c-211">Nu måste du hittar klient-ID och klient-ID för Programidentitet som är kopplad till ditt webbprogram eller API-app.</span><span class="sxs-lookup"><span data-stu-id="f137c-211">Now you must find the client ID and tenant ID for the application identity that's associated with your web app or API app.</span></span> <span data-ttu-id="f137c-212">Du kan använda dessa ID: N i del 3.</span><span class="sxs-lookup"><span data-stu-id="f137c-212">You use these IDs in Part 3.</span></span> <span data-ttu-id="f137c-213">Så att fortsätta med de här stegen för Azure-portalen eller [klassiska Azure-portalen](#find-id-classic).</span><span class="sxs-lookup"><span data-stu-id="f137c-213">So continue with these steps for the Azure portal or [Azure classic portal](#find-id-classic).</span></span>

<span data-ttu-id="f137c-214">**Hitta tillämpningsprogrammets identitet klient-ID och klient-ID för ditt webbprogram eller API-app i Azure-portalen**</span><span class="sxs-lookup"><span data-stu-id="f137c-214">**Find application identity's client ID and tenant ID for your web app or API app in the Azure portal**</span></span>

1. <span data-ttu-id="f137c-215">Under **autentiseringsproviders**, Välj **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="f137c-215">Under **Authentication Providers**, choose **Azure Active Directory**.</span></span> 

   ![Välj ”Azure Active Directory”](./media/logic-apps-custom-hosted-api/custom-api-app-identity-client-id-tenant-id.png)

2. <span data-ttu-id="f137c-217">På den **inställningarna för Azure Active Directory** bladet ange **hanteringsläge** till **Avancerat**.</span><span class="sxs-lookup"><span data-stu-id="f137c-217">On the **Azure Active Directory Settings** blade, set **Management mode** to **Advanced**.</span></span>

3. <span data-ttu-id="f137c-218">Kopiera den **klient-ID**, och spara det GUID för användning i del 3.</span><span class="sxs-lookup"><span data-stu-id="f137c-218">Copy the **Client ID**, and save that GUID for use in Part 3.</span></span>

   > [!TIP] 
   > <span data-ttu-id="f137c-219">Om **klient-ID** och **utfärdar-Url** inte visas, försök att uppdatera Azure-portalen och upprepa steg 1.</span><span class="sxs-lookup"><span data-stu-id="f137c-219">If **Client ID** and **Issuer Url** don't appear, try refreshing the Azure portal, and repeat Step 1.</span></span>

4. <span data-ttu-id="f137c-220">Under **utfärdar-Url**, kopiera och spara bara GUID för en del 3.</span><span class="sxs-lookup"><span data-stu-id="f137c-220">Under **Issuer Url**, copy and save just the GUID for Part 3.</span></span> <span data-ttu-id="f137c-221">Du kan också använda detta GUID i ditt webbprogram eller mallen för distribution av API-app om det behövs.</span><span class="sxs-lookup"><span data-stu-id="f137c-221">You can also use this GUID in your web app or API app's deployment template, if necessary.</span></span>

   <span data-ttu-id="f137c-222">Detta GUID är din klient-GUID (”klient-ID”) och som ska visas i den här URL:`https://sts.windows.net/{GUID}`</span><span class="sxs-lookup"><span data-stu-id="f137c-222">This GUID is your specific tenant's GUID ("tenant ID") and should appear in this URL: `https://sts.windows.net/{GUID}`</span></span>

5. <span data-ttu-id="f137c-223">Utan att spara ändringarna, Stäng av **inställningarna för Azure Active Directory** bladet.</span><span class="sxs-lookup"><span data-stu-id="f137c-223">Without saving your changes, close the **Azure Active Directory Settings** blade.</span></span>

<a name="find-id-classic"></a>

<span data-ttu-id="f137c-224">**Hitta tillämpningsprogrammets identitet klient-ID och klient-ID för ditt webbprogram eller API-app i den klassiska Azure-portalen**</span><span class="sxs-lookup"><span data-stu-id="f137c-224">**Find application identity's client ID and tenant ID for your web app or API app in the Azure classic portal**</span></span>

1. <span data-ttu-id="f137c-225">I den klassiska Azure-portalen väljer [ **Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).</span><span class="sxs-lookup"><span data-stu-id="f137c-225">In the Azure classic portal, choose [**Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).</span></span>

2.  <span data-ttu-id="f137c-226">Välj en katalog som du använder för ditt webbprogram eller API-app.</span><span class="sxs-lookup"><span data-stu-id="f137c-226">Select the directory that you use for your web app or API app.</span></span>

3. <span data-ttu-id="f137c-227">I den **Sök** , söka efter och välj programidentiteten för ditt webbprogram eller API-app.</span><span class="sxs-lookup"><span data-stu-id="f137c-227">In the **Search** box, find and select the application identity for your web app or API app.</span></span>

4. <span data-ttu-id="f137c-228">På den **konfigurera** fliken, kopiera den **klient-ID**, och spara det GUID för användning i del 3.</span><span class="sxs-lookup"><span data-stu-id="f137c-228">On the **Configure** tab, copy the **Client ID**, and save that GUID for use in Part 3.</span></span>

5. <span data-ttu-id="f137c-229">När du har fått klient-ID längst ned i den **konfigurera** , Välj **visa slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="f137c-229">After you get the client ID, at the bottom of the **Configure** tab, choose **View endpoints**.</span></span>

6. <span data-ttu-id="f137c-230">Kopiera URL-Adressen för **Federation Metadata dokumentet**, och bläddra till URL: en.</span><span class="sxs-lookup"><span data-stu-id="f137c-230">Copy the URL for **Federation Metadata Document**, and browse to that URL.</span></span>

7. <span data-ttu-id="f137c-231">Hitta roten i metadata-dokument som öppnar **EntityDescriptor ID** element som har en **ID för entiteterna** attribut i det här formuläret:`https://sts.windows.net/{GUID}`</span><span class="sxs-lookup"><span data-stu-id="f137c-231">In the metadata document that opens, find the root **EntityDescriptor ID** element, which has an **entityID** attribute in this form: `https://sts.windows.net/{GUID}`</span></span> 

      <span data-ttu-id="f137c-232">GUID i det här attributet är din klient-GUID (klient-ID).</span><span class="sxs-lookup"><span data-stu-id="f137c-232">The GUID in this attribute is your specific tenant's GUID (tenant ID).</span></span>

8. <span data-ttu-id="f137c-233">Kopiera klient-ID och spara detta ID för användning i en del 3 och även för att använda i ditt webbprogram eller mallen för distribution av API-app om det behövs.</span><span class="sxs-lookup"><span data-stu-id="f137c-233">Copy the tenant ID and save that ID for use in Part 3, and also to use in your web app or API app's deployment template, if necessary.</span></span>

<span data-ttu-id="f137c-234">Mer information finns i följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="f137c-234">For more information, see these topics:</span></span>

* [<span data-ttu-id="f137c-235">Användarautentisering för API apps i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="f137c-235">User authentication for API apps in Azure App Service</span></span>](../app-service-api/app-service-api-dotnet-user-principal-auth.md)
* [<span data-ttu-id="f137c-236">Autentisering och auktorisering i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="f137c-236">Authentication and authorization in Azure App Service</span></span>](../app-service/app-service-authentication-overview.md)

<a name="authen-deploy"></a>

<span data-ttu-id="f137c-237">**Aktivera autentisering när du distribuerar med en Azure Resource Manager-mall**</span><span class="sxs-lookup"><span data-stu-id="f137c-237">**Turn on authentication when you deploy with an Azure Resource Manager template**</span></span>

<span data-ttu-id="f137c-238">Du måste fortfarande skapa en Azure AD-programidentitet för ditt webbprogram eller API-app som skiljer sig från appen identitet för din logikapp.</span><span class="sxs-lookup"><span data-stu-id="f137c-238">You must still create an Azure AD application identity for your web app or API app that differs from the app identity for your logic app.</span></span> <span data-ttu-id="f137c-239">Följ föregående steg i en del 2 för Azure-portalen för att skapa programidentiteten.</span><span class="sxs-lookup"><span data-stu-id="f137c-239">To create the application identity, follow the previous steps in Part 2 for the Azure portal.</span></span> <span data-ttu-id="f137c-240">Du kan också följa stegen i del 1, men se till att använda din webbapp eller API-app faktiska `https://{URL}` för **inloggnings-URL** och **App-ID URI**.</span><span class="sxs-lookup"><span data-stu-id="f137c-240">You can also follow the steps in Part 1, but make sure to use your web app or API app's actual `https://{URL}` for **Sign-on URL** and **App ID URI**.</span></span> <span data-ttu-id="f137c-241">Du måste spara både klient-ID och klient-ID för användning i din app Distributionsmall och för del 3 från de här stegen.</span><span class="sxs-lookup"><span data-stu-id="f137c-241">From these steps, you have to save both the client ID and tenant ID for use in your app's deployment template and also for Part 3.</span></span>

> [!NOTE]
> <span data-ttu-id="f137c-242">När du skapar Azure AD-programidentitet för ditt webbprogram eller API-app måste du använda den Azure-portalen eller klassiska Azure-portalen i stället PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f137c-242">When you create the Azure AD application identity for your web app or API app, you must use the Azure portal or Azure classic portal, rather than PowerShell.</span></span> <span data-ttu-id="f137c-243">PowerShell-cmdlet ställa inte in behörigheterna som krävs för att logga in användare på en webbplats.</span><span class="sxs-lookup"><span data-stu-id="f137c-243">The PowerShell commandlet doesn't set up the required permissions to sign users into a website.</span></span>

<span data-ttu-id="f137c-244">När du har fått klient-ID och klient-ID är dessa ID: N som en subresource av ditt webbprogram eller API-app i mallen för distribution:</span><span class="sxs-lookup"><span data-stu-id="f137c-244">After you get the client ID and tenant ID, include these IDs as a subresource of your web app or API app in your deployment template:</span></span>

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

<span data-ttu-id="f137c-245">Du distribuerar automatiskt en tom webbapp och en logikapp tillsammans med Azure Active Directory-autentisering, [visa den fullständiga mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-custom-api/azuredeploy.json), eller klicka på **till Azure** här:</span><span class="sxs-lookup"><span data-stu-id="f137c-245">To automatically deploy a blank web app and a logic app together with Azure Active Directory authentication, [view the complete template here](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-custom-api/azuredeploy.json), or click **Deploy to Azure** here:</span></span>

<span data-ttu-id="f137c-246">[![Distribuera till Azure](media/logic-apps-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="f137c-246">[![Deploy to Azure](media/logic-apps-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)</span></span>

#### <a name="part-3-populate-the-authorization-section-in-your-logic-app"></a><span data-ttu-id="f137c-247">Del 3: Lägg till i avsnittet om auktorisering i din logikapp</span><span class="sxs-lookup"><span data-stu-id="f137c-247">Part 3: Populate the Authorization section in your logic app</span></span>

<span data-ttu-id="f137c-248">Den tidigare mallen har redan det här tillståndet avsnittet Konfigurera, men om du använder direkt logikappen, måste du inkludera avsnittet fullständig behörighet.</span><span class="sxs-lookup"><span data-stu-id="f137c-248">The previous template already has this authorization section set up, but if you are directly authoring the logic app, you must include the full authorization section.</span></span>

<span data-ttu-id="f137c-249">Öppna din app definition för logiken i kodvy, gå till den **HTTP** delen åtgärd att hitta den **auktorisering** avsnittet och inkludera den här raden:</span><span class="sxs-lookup"><span data-stu-id="f137c-249">Open your logic app definition in code view, go to the **HTTP** action section, find the **Authorization** section, and include this line:</span></span>

`{"tenant": "{tenant-ID}", "audience": "{client-ID-from-Part-2-web-app-or-API app}", "clientId": "{client-ID-from-Part-1-logic-app}", "secret": "{key-from-Part-1-logic-app}", "type": "ActiveDirectoryOAuth" }`

| <span data-ttu-id="f137c-250">Element</span><span class="sxs-lookup"><span data-stu-id="f137c-250">Element</span></span> | <span data-ttu-id="f137c-251">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f137c-251">Description</span></span> |
| ------- | ----------- |
| <span data-ttu-id="f137c-252">Klient</span><span class="sxs-lookup"><span data-stu-id="f137c-252">tenant</span></span> |<span data-ttu-id="f137c-253">GUID för Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="f137c-253">The GUID for the Azure AD tenant</span></span> |
| <span data-ttu-id="f137c-254">målgrupp</span><span class="sxs-lookup"><span data-stu-id="f137c-254">audience</span></span> |<span data-ttu-id="f137c-255">Krävs.</span><span class="sxs-lookup"><span data-stu-id="f137c-255">Required.</span></span> <span data-ttu-id="f137c-256">GUID för målresursen som du vill komma åt - klient-ID från programidentiteten för ditt webbprogram eller API-app</span><span class="sxs-lookup"><span data-stu-id="f137c-256">The GUID for the target resource that you want to access - the client ID from the application identity for your web app or API app</span></span> |
| <span data-ttu-id="f137c-257">clientId</span><span class="sxs-lookup"><span data-stu-id="f137c-257">clientId</span></span> |<span data-ttu-id="f137c-258">GUID för klienten som begär åtkomst till - klient-ID från programidentiteten för din logikapp</span><span class="sxs-lookup"><span data-stu-id="f137c-258">The GUID for the client requesting access - the client ID from the application identity for your logic app</span></span> |
| <span data-ttu-id="f137c-259">hemligt</span><span class="sxs-lookup"><span data-stu-id="f137c-259">secret</span></span> |<span data-ttu-id="f137c-260">Krävs.</span><span class="sxs-lookup"><span data-stu-id="f137c-260">Required.</span></span> <span data-ttu-id="f137c-261">Nyckel eller lösenord från programidentiteten för klienten som begär åtkomst-token</span><span class="sxs-lookup"><span data-stu-id="f137c-261">The key or password from the application identity for the client that's requesting the access token</span></span> |
| <span data-ttu-id="f137c-262">typ</span><span class="sxs-lookup"><span data-stu-id="f137c-262">type</span></span> |<span data-ttu-id="f137c-263">Autentiseringstypen.</span><span class="sxs-lookup"><span data-stu-id="f137c-263">The authentication type.</span></span> <span data-ttu-id="f137c-264">Värdet är för ActiveDirectoryOAuth autentisering `ActiveDirectoryOAuth`.</span><span class="sxs-lookup"><span data-stu-id="f137c-264">For ActiveDirectoryOAuth authentication, the value is `ActiveDirectoryOAuth`.</span></span> |

<span data-ttu-id="f137c-265">Exempel:</span><span class="sxs-lookup"><span data-stu-id="f137c-265">For example:</span></span>

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

### <a name="secure-api-calls-through-code"></a><span data-ttu-id="f137c-266">Säker API-anrop med kod</span><span class="sxs-lookup"><span data-stu-id="f137c-266">Secure API calls through code</span></span>

<a name="certificate"></a>

#### <a name="certificate-authentication"></a><span data-ttu-id="f137c-267">Certifikatautentisering</span><span class="sxs-lookup"><span data-stu-id="f137c-267">Certificate authentication</span></span>

<span data-ttu-id="f137c-268">Du kan använda klientcertifikat för att validera inkommande förfrågningar från din logikapp till ditt webbprogram eller API-app.</span><span class="sxs-lookup"><span data-stu-id="f137c-268">To validate the incoming requests from your logic app to your web app or API app, you can use client certificates.</span></span> <span data-ttu-id="f137c-269">Om du vill konfigurera din kod Läs [hur du konfigurerar TLS ömsesidig autentisering](../app-service-web/app-service-web-configure-tls-mutual-auth.md).</span><span class="sxs-lookup"><span data-stu-id="f137c-269">To set up your code, learn [how to configure TLS mutual authentication](../app-service-web/app-service-web-configure-tls-mutual-auth.md).</span></span>

<span data-ttu-id="f137c-270">I den **auktorisering** och inkludera den här raden:</span><span class="sxs-lookup"><span data-stu-id="f137c-270">In the **Authorization** section, include this line:</span></span> 

`{"type": "clientcertificate", "password": "password", "pfx": "long-pfx-key"}`

| <span data-ttu-id="f137c-271">Element</span><span class="sxs-lookup"><span data-stu-id="f137c-271">Element</span></span> | <span data-ttu-id="f137c-272">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f137c-272">Description</span></span> |
| ------- | ----------- |
| <span data-ttu-id="f137c-273">typ</span><span class="sxs-lookup"><span data-stu-id="f137c-273">type</span></span> |<span data-ttu-id="f137c-274">Krävs.</span><span class="sxs-lookup"><span data-stu-id="f137c-274">Required.</span></span> <span data-ttu-id="f137c-275">Autentiseringstypen.</span><span class="sxs-lookup"><span data-stu-id="f137c-275">The authentication type.</span></span> <span data-ttu-id="f137c-276">För SSL-klientcertifikat, värdet måste vara `ClientCertificate`.</span><span class="sxs-lookup"><span data-stu-id="f137c-276">For SSL client certificates, the value must be `ClientCertificate`.</span></span> |
| <span data-ttu-id="f137c-277">lösenord</span><span class="sxs-lookup"><span data-stu-id="f137c-277">password</span></span> |<span data-ttu-id="f137c-278">Krävs.</span><span class="sxs-lookup"><span data-stu-id="f137c-278">Required.</span></span> <span data-ttu-id="f137c-279">Lösenord för att komma åt klientcertifikatet (PFX-fil)</span><span class="sxs-lookup"><span data-stu-id="f137c-279">The password for accessing the client certificate (PFX file)</span></span> |
| <span data-ttu-id="f137c-280">Pfx</span><span class="sxs-lookup"><span data-stu-id="f137c-280">pfx</span></span> |<span data-ttu-id="f137c-281">Krävs.</span><span class="sxs-lookup"><span data-stu-id="f137c-281">Required.</span></span> <span data-ttu-id="f137c-282">Base64-kodad innehållet i klientens certifikat (PFX-fil)</span><span class="sxs-lookup"><span data-stu-id="f137c-282">Base64-encoded contents of the client certificate (PFX file)</span></span> |

<a name="basic"></a>

#### <a name="basic-authentication"></a><span data-ttu-id="f137c-283">Grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="f137c-283">Basic authentication</span></span>

<span data-ttu-id="f137c-284">Du kan använda grundläggande autentisering, till exempel användarnamn och lösenord för att validera inkommande förfrågningar från din logikapp till ditt webbprogram eller API-app.</span><span class="sxs-lookup"><span data-stu-id="f137c-284">To validate incoming requests from your logic app to your web app or API app, you can use basic authentication, such as a username and password.</span></span> <span data-ttu-id="f137c-285">Grundläggande autentisering är ett vanligt mönster och du kan använda den här autentiseringen på alla språk som används för att skapa ditt webbprogram eller API-app.</span><span class="sxs-lookup"><span data-stu-id="f137c-285">Basic authentication is a common pattern, and you can use this authentication in any language used to build your web app or API app.</span></span>

<span data-ttu-id="f137c-286">I den **auktorisering** och inkludera den här raden:</span><span class="sxs-lookup"><span data-stu-id="f137c-286">In the **Authorization** section, include this line:</span></span>

<span data-ttu-id="f137c-287">`{"type": "basic", "username": "username", "password": "password"}`.</span><span class="sxs-lookup"><span data-stu-id="f137c-287">`{"type": "basic", "username": "username", "password": "password"}`.</span></span>

| <span data-ttu-id="f137c-288">Element</span><span class="sxs-lookup"><span data-stu-id="f137c-288">Element</span></span> | <span data-ttu-id="f137c-289">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f137c-289">Description</span></span> |
| --- | --- |
| <span data-ttu-id="f137c-290">typ</span><span class="sxs-lookup"><span data-stu-id="f137c-290">type</span></span> |<span data-ttu-id="f137c-291">Krävs.</span><span class="sxs-lookup"><span data-stu-id="f137c-291">Required.</span></span> <span data-ttu-id="f137c-292">Autentiseringstypen.</span><span class="sxs-lookup"><span data-stu-id="f137c-292">The authentication type.</span></span> <span data-ttu-id="f137c-293">För grundläggande autentisering, värdet måste vara `Basic`.</span><span class="sxs-lookup"><span data-stu-id="f137c-293">For basic authentication, the value must be `Basic`.</span></span> |
| <span data-ttu-id="f137c-294">användarnamn</span><span class="sxs-lookup"><span data-stu-id="f137c-294">username</span></span> |<span data-ttu-id="f137c-295">Krävs.</span><span class="sxs-lookup"><span data-stu-id="f137c-295">Required.</span></span> <span data-ttu-id="f137c-296">Användarnamn för autentisering</span><span class="sxs-lookup"><span data-stu-id="f137c-296">The username for authentication</span></span> |
| <span data-ttu-id="f137c-297">lösenord</span><span class="sxs-lookup"><span data-stu-id="f137c-297">password</span></span> |<span data-ttu-id="f137c-298">Krävs.</span><span class="sxs-lookup"><span data-stu-id="f137c-298">Required.</span></span> <span data-ttu-id="f137c-299">Lösenordet för autentisering</span><span class="sxs-lookup"><span data-stu-id="f137c-299">The password for authentication</span></span> |

<a name="azure-ad-code"></a>

#### <a name="azure-active-directory-authentication-through-code"></a><span data-ttu-id="f137c-300">Azure Active Directory-autentisering via kod</span><span class="sxs-lookup"><span data-stu-id="f137c-300">Azure Active Directory authentication through code</span></span>

<span data-ttu-id="f137c-301">Som standard ger inte Azure AD-autentisering som du aktiverar i Azure-portalen detaljerad behörighet.</span><span class="sxs-lookup"><span data-stu-id="f137c-301">By default, the Azure AD authentication that you turn on in the Azure portal doesn't provide fine-grained authorization.</span></span> <span data-ttu-id="f137c-302">Den här autentiseringen låser exempelvis din API till bara en viss klient, inte till en specifik användare eller en app.</span><span class="sxs-lookup"><span data-stu-id="f137c-302">For example, this authentication locks your API to just a specific tenant, not to a specific user or app.</span></span> 

<span data-ttu-id="f137c-303">För att begränsa API-åtkomst till din logikapp med kod, extrahera det huvud som innehåller JSON web token (JWT).</span><span class="sxs-lookup"><span data-stu-id="f137c-303">To restrict API access to your logic app through code, extract the header that has the JSON web token (JWT).</span></span> <span data-ttu-id="f137c-304">Kontrollera Anroparens identitet och avvisar begäranden som inte matchar.</span><span class="sxs-lookup"><span data-stu-id="f137c-304">Check the caller's identity, and reject requests that don't match.</span></span>

<span data-ttu-id="f137c-305">Lär dig fortsätter att implementera den här autentiseringen helt i din kod och inte använda Azure-portalen så [autentiseras med lokal Active Directory i din Azure-app](../app-service-web/web-sites-authentication-authorization.md).</span><span class="sxs-lookup"><span data-stu-id="f137c-305">Going further, to implement this authentication entirely in your own code, and not use the Azure portal, learn how to [authenticate with on-premises Active Directory in your Azure app](../app-service-web/web-sites-authentication-authorization.md).</span></span>

<span data-ttu-id="f137c-306">Du måste följa de här stegen för att skapa en programidentitet för din logikapp och använda identitet för att anropa ditt API.</span><span class="sxs-lookup"><span data-stu-id="f137c-306">To create an application identity for your logic app and use that identity to call your API, you must follow the previous steps.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f137c-307">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f137c-307">Next steps</span></span>

* [<span data-ttu-id="f137c-308">Kontrollera logik app prestanda med diagnostiska loggar och aviseringar</span><span class="sxs-lookup"><span data-stu-id="f137c-308">Check logic app performance with diagnostic logs and alerts</span></span>](logic-apps-monitor-your-logic-apps.md)