---
title: "aaaUse Azure Active Directory tooauthenticate Batch lösningar | Microsoft Docs"
description: Program som skapats med Azure resource manager och hello Batch-resursprovidern autentisera med Azure AD.
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/27/2017
ms.author: tamram
ms.openlocfilehash: 192aa9f8d7cbfc0282a4a0c33ab1659f1f351525
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-batch-management-solutions-with-active-directory"></a><span data-ttu-id="1f7e9-103">Autentisera Batch hanteringslösningar med Active Directory</span><span class="sxs-lookup"><span data-stu-id="1f7e9-103">Authenticate Batch Management solutions with Active Directory</span></span>

<span data-ttu-id="1f7e9-104">Program som anropar hello Azure Batch Management-tjänsten autentiseras med [Azure Active Directory] [ aad_about] (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1f7e9-104">Applications that call hello Azure Batch Management service authenticate with [Azure Active Directory][aad_about] (Azure AD).</span></span> <span data-ttu-id="1f7e9-105">Azure AD är Microsofts flera innehavare molnbaserad katalog och identity management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-105">Azure AD is Microsoft’s multi-tenant cloud based directory and identity management service.</span></span> <span data-ttu-id="1f7e9-106">Azure själva använder Azure AD för hello autentisering för sina kunder, administratörer och organisationens användare.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-106">Azure itself uses Azure AD for hello authentication of its customers, service administrators, and organizational users.</span></span>

<span data-ttu-id="1f7e9-107">hello Batch Management .NET-biblioteket visar typer för att arbeta med Batch-konton, nycklar, program och programpaket.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-107">hello Batch Management .NET library exposes types for working with Batch accounts, account keys, applications, and application packages.</span></span> <span data-ttu-id="1f7e9-108">hello Batch Management .NET-biblioteket är en Azure resource provider-klient och används tillsammans med [Azure Resource Manager] [ resman_overview] toomanage dessa resurser via programmering.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-108">hello Batch Management .NET library is an Azure resource provider client, and is used together with [Azure Resource Manager][resman_overview] toomanage these resources programmatically.</span></span> <span data-ttu-id="1f7e9-109">Azure AD är obligatoriska tooauthenticate begäranden som görs via en Azure resource provider klienten, inklusive hello Batch Management .NET-biblioteket, och via [Azure Resource Manager][resman_overview].</span><span class="sxs-lookup"><span data-stu-id="1f7e9-109">Azure AD is required tooauthenticate requests made through any Azure resource provider client, including hello Batch Management .NET library, and through [Azure Resource Manager][resman_overview].</span></span>

<span data-ttu-id="1f7e9-110">I den här artikeln förklarar vi med hjälp av Azure AD tooauthenticate från program som använder hello Batch Management .NET-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-110">In this article, we explore using Azure AD tooauthenticate from applications that use hello Batch Management .NET library.</span></span> <span data-ttu-id="1f7e9-111">Visar vi hur toouse Azure AD tooauthenticate en prenumeration administratör eller medadministratör, med hjälp av integrerad autentisering.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-111">We show how toouse Azure AD tooauthenticate a subscription administrator or co-administrator, using integrated authentication.</span></span> <span data-ttu-id="1f7e9-112">Vi använder hello [AccountManagment] [ acct_mgmt_sample] exempelprojektet finns på GitHub, toowalk genom att använda Azure AD med hello Batch Management .NET-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-112">We use hello [AccountManagment][acct_mgmt_sample] sample project, available on GitHub, toowalk through using Azure AD with hello Batch Management .NET library.</span></span>

<span data-ttu-id="1f7e9-113">toolearn mer information om hur du använder hello Batch Management .NET-bibliotek och exempelapp för hello AccountManagement, se [hantera Batch-konton och kvoter med hello Batch Management-klientbibliotek för .NET](batch-management-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="1f7e9-113">toolearn more about using hello Batch Management .NET library and hello AccountManagement sample, see [Manage Batch accounts and quotas with hello Batch Management client library for .NET](batch-management-dotnet.md).</span></span>

## <a name="register-your-application-with-azure-ad"></a><span data-ttu-id="1f7e9-114">Registrera ditt program med Azure AD</span><span class="sxs-lookup"><span data-stu-id="1f7e9-114">Register your application with Azure AD</span></span>

<span data-ttu-id="1f7e9-115">hello Azure [Active Directory Authentication Library] [ aad_adal] (ADAL) tillhandahåller ett programmeringsgränssnitt tooAzure AD för användning i dina program.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-115">hello Azure [Active Directory Authentication Library][aad_adal] (ADAL) provides a programmatic interface tooAzure AD for use within your applications.</span></span> <span data-ttu-id="1f7e9-116">toocall ADAL från ditt program måste du registrera ditt program i en Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-116">toocall ADAL from your application, you must register your application in an Azure AD tenant.</span></span> <span data-ttu-id="1f7e9-117">När du registrerar ditt program kan ange du Azure AD med information om ditt program, inklusive ett namn för den inom hello Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-117">When you register your application, you supply Azure AD with information about your application, including a name for it within hello Azure AD tenant.</span></span> <span data-ttu-id="1f7e9-118">Azure AD sedan innehåller ett program-ID som du använder tooassociate ditt program med Azure AD under körning.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-118">Azure AD then provides an application ID that you use tooassociate your application with Azure AD at runtime.</span></span> <span data-ttu-id="1f7e9-119">toolearn mer om hello program-ID, se [program och tjänstens huvudnamn objekt i Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="1f7e9-119">toolearn more about hello application ID, see [Application and service principal objects in Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span></span>

<span data-ttu-id="1f7e9-120">tooregister hello AccountManagement exempelprogrammet åtgärderna hello i hello [lägga till ett program](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) i avsnittet [integrera program med Azure Active Directory] [ aad_integrate].</span><span class="sxs-lookup"><span data-stu-id="1f7e9-120">tooregister hello AccountManagement sample application, follow hello steps in hello [Adding an Application](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) section in [Integrating applications with Azure Active Directory][aad_integrate].</span></span> <span data-ttu-id="1f7e9-121">Ange **internt klientprogram** för hello typ av program.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-121">Specify **Native Client Application** for hello type of application.</span></span> <span data-ttu-id="1f7e9-122">Hej industry standard OAuth 2.0-URI för hello **omdirigerings-URI** är `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-122">hello industry standard OAuth 2.0 URI for hello **Redirect URI** is `urn:ietf:wg:oauth:2.0:oob`.</span></span> <span data-ttu-id="1f7e9-123">Du kan dock ange en giltig URI (t.ex `http://myaccountmanagementsample`) för hello **omdirigerings-URI**eftersom det inte behöver toobe verkliga endpoint:</span><span class="sxs-lookup"><span data-stu-id="1f7e9-123">However, you can specify any valid URI (such as `http://myaccountmanagementsample`) for hello **Redirect URI**, as it does not need toobe a real endpoint:</span></span>

![](./media/batch-aad-auth-management/app-registration-management-plane.png)

<span data-ttu-id="1f7e9-124">När du har slutfört registreringsprocessen hello visas hello program-ID samt hello objekt (tjänstens huvudnamn)-ID som anges för ditt program.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-124">Once you complete hello registration process, you'll see hello application ID and hello object (service principal) ID listed for your application.</span></span>  

![](./media/batch-aad-auth-management/app-registration-client-id.png)

## <a name="grant-hello-azure-resource-manager-api-access-tooyour-application"></a><span data-ttu-id="1f7e9-125">Bevilja hello Azure Resource Manager API åtkomst tooyour program</span><span class="sxs-lookup"><span data-stu-id="1f7e9-125">Grant hello Azure Resource Manager API access tooyour application</span></span>

<span data-ttu-id="1f7e9-126">Därefter behöver du toodelegate åtkomst tooyour programmet toohello Azure Resource Manager API.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-126">Next, you'll need toodelegate access tooyour application toohello Azure Resource Manager API.</span></span> <span data-ttu-id="1f7e9-127">hello Azure AD-ID för hello Resource Manager API är **Windows Azure Service Management API**.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-127">hello Azure AD identifier for hello Resource Manager API is **Windows Azure Service Management API**.</span></span>

<span data-ttu-id="1f7e9-128">Följ anvisningarna i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="1f7e9-128">Follow these steps in hello Azure portal:</span></span>

1. <span data-ttu-id="1f7e9-129">Hello vänstra navigeringsfönstret för hello Azure-portalen och väljer **fler tjänster**, klickar du på **App registreringar**, och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-129">In hello left-hand navigation pane of hello Azure portal, choose **More Services**, click **App Registrations**, and click **Add**.</span></span>
2. <span data-ttu-id="1f7e9-130">Sök efter hello namnet på ditt program i hello lista över app registreringar:</span><span class="sxs-lookup"><span data-stu-id="1f7e9-130">Search for hello name of your application in hello list of app registrations:</span></span>

    ![Sök efter programnamnet](./media/batch-aad-auth-management/search-app-registration.png)

3. <span data-ttu-id="1f7e9-132">Visa hello **inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-132">Display hello **Settings** blade.</span></span> <span data-ttu-id="1f7e9-133">I hello **API-åtkomst** väljer **nödvändiga behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-133">In hello **API Access** section, select **Required permissions**.</span></span>
4. <span data-ttu-id="1f7e9-134">Klicka på **Lägg till** tooadd en ny nödvändig behörighet.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-134">Click **Add** tooadd a new required permission.</span></span> 
5. <span data-ttu-id="1f7e9-135">I steg 1, ange **Windows Azure Service Management API**, Välj den API hello listan över resultat och på hello **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-135">In step 1, enter **Windows Azure Service Management API**, select that API from hello list of results, and click hello **Select** button.</span></span>
6. <span data-ttu-id="1f7e9-136">I steg 2, Välj hello kryssrutan bredvid för**åtkomst Azure klassiska distributionsmodellen som organisationen användare**, och klicka på hello **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-136">In step 2, select hello check box next too**Access Azure classic deployment model as organization users**, and click hello **Select** button.</span></span>
7. <span data-ttu-id="1f7e9-137">Klicka på hello **klar** knappen.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-137">Click hello **Done** button.</span></span>

<span data-ttu-id="1f7e9-138">Hej **nödvändiga behörigheter** bladet nu visar att tooboth beviljas behörighet tooyour programmet hello ADAL och Resource Manager API: er.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-138">hello **Required Permissions** blade now shows that permissions tooyour application are granted tooboth hello ADAL and Resource Manager APIs.</span></span> <span data-ttu-id="1f7e9-139">Behörigheter tooADAL som standard när du först registrera din app med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-139">Permissions are granted tooADAL by default when you first register your app with Azure AD.</span></span>

![Delegera behörigheter toohello Azure Resource Manager API](./media/batch-aad-auth-management/required-permissions-management-plane.png)

## <a name="azure-ad-endpoints"></a><span data-ttu-id="1f7e9-141">Azure AD-slutpunkter</span><span class="sxs-lookup"><span data-stu-id="1f7e9-141">Azure AD endpoints</span></span>

<span data-ttu-id="1f7e9-142">tooauthenticate Batch Management-lösningar med Azure AD, behöver du två välkända slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-142">tooauthenticate your Batch Management solutions with Azure AD, you'll need two well-known endpoints.</span></span>

- <span data-ttu-id="1f7e9-143">Hej **vanliga Azure AD-slutpunkt** ger en allmän referens samla in gränssnittet när en viss klient inte har angetts som hello fallet med integrerad autentisering:</span><span class="sxs-lookup"><span data-stu-id="1f7e9-143">hello **Azure AD common endpoint** provides a generic credential gathering interface when a specific tenant is not provided, as in hello case of integrated authentication:</span></span>

    `https://login.microsoftonline.com/common`

- <span data-ttu-id="1f7e9-144">Hej **Azure Resource Manager-slutpunkt** är används tooacquire en token för att autentisera begäranden toohello Batch management-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="1f7e9-144">hello **Azure Resource Manager endpoint** is used tooacquire a token for authenticating requests toohello Batch management service:</span></span>

    `https://management.core.windows.net/`

<span data-ttu-id="1f7e9-145">Hej AccountManagement exempelprogrammet definierar konstanter för dessa slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-145">hello AccountManagement sample application defines constants for these endpoints.</span></span> <span data-ttu-id="1f7e9-146">Lämna dessa konstanter oförändrade:</span><span class="sxs-lookup"><span data-stu-id="1f7e9-146">Leave these constants unchanged:</span></span>

```csharp
// Azure Active Directory "common" endpoint.
private const string AuthorityUri = "https://login.microsoftonline.com/common";
// Azure Resource Manager endpoint 
private const string ResourceUri = "https://management.core.windows.net/";
```

## <a name="reference-your-application-id"></a><span data-ttu-id="1f7e9-147">Referens-program-ID</span><span class="sxs-lookup"><span data-stu-id="1f7e9-147">Reference your application ID</span></span> 

<span data-ttu-id="1f7e9-148">Klientprogrammet använder hello program-ID (även hänvisade tooas hello klient-ID) tooaccess Azure AD under körning.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-148">Your client application uses hello application ID (also referred tooas hello client ID) tooaccess Azure AD at runtime.</span></span> <span data-ttu-id="1f7e9-149">När du har registrerat ditt program i hello Azure-portalen, uppdatera din kod toouse hello program-ID som tillhandahålls av Azure AD för registrerade programmet.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-149">Once you've registered your application in hello Azure portal, update your code toouse hello application ID provided by Azure AD for your registered application.</span></span> <span data-ttu-id="1f7e9-150">Kopiera program-ID från hello Azure portal toohello lämplig konstant i hello AccountManagement exempelprogrammet:</span><span class="sxs-lookup"><span data-stu-id="1f7e9-150">In hello AccountManagement sample application, copy your application ID from hello Azure portal toohello appropriate constant:</span></span>

```csharp
// Specify hello unique identifier (hello "Client ID") for your application. This is required so that your
// native client application (i.e. this sample) can access hello Microsoft Azure AD Graph API. For information
// about registering an application in Azure Active Directory, please see "Adding an Application" here:
// https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
private const string ClientId = "<application-id>";
```
<span data-ttu-id="1f7e9-151">Också kopiera hello omdirigerings-URI som du angav under hello registreringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-151">Also copy hello redirect URI that you specified during hello registration process.</span></span> <span data-ttu-id="1f7e9-152">hello omdirigerings-URI som angetts i din kod måste matcha hello omdirigerings-URI som du angav när du registrerade hello program.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-152">hello redirect URI specified in your code must match hello redirect URI that you provided when you registered hello application.</span></span>

```csharp
// hello URI toowhich Azure AD will redirect in response tooan OAuth 2.0 request. This value is
// specified by you when you register an application with AAD (see ClientId comment). It does not
// need toobe a real endpoint, but must be a valid URI (e.g. https://accountmgmtsampleapp).
private const string RedirectUri = "http://myaccountmanagementsample";
```

## <a name="acquire-an-azure-ad-authentication-token"></a><span data-ttu-id="1f7e9-153">Skaffa en Azure AD-token för autentisering</span><span class="sxs-lookup"><span data-stu-id="1f7e9-153">Acquire an Azure AD authentication token</span></span>

<span data-ttu-id="1f7e9-154">När du registrerar hello AccountManagement exemplet i hello Azure AD-klient och uppdatera hello exempelkod källa med värdena är hello prov klar tooauthenticate med hjälp av Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-154">After you register hello AccountManagement sample in hello Azure AD tenant and update hello sample source code with your values, hello sample is ready tooauthenticate using Azure AD.</span></span> <span data-ttu-id="1f7e9-155">När du kör hello exempel försöker hello ADAL tooacquire en autentiseringstoken.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-155">When you run hello sample, hello ADAL attempts tooacquire an authentication token.</span></span> <span data-ttu-id="1f7e9-156">I det här steget ombeds du ange autentiseringsuppgifterna för ditt Microsoft:</span><span class="sxs-lookup"><span data-stu-id="1f7e9-156">At this step, it prompts you for your Microsoft credentials:</span></span> 

```csharp
// Obtain an access token using hello "common" AAD resource. This allows hello application
// tooquery AAD for information that lies outside hello application's tenant (such as for
// querying subscription information in your Azure account).
AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
AuthenticationResult authResult = authContext.AcquireToken(ResourceUri,
                                                        ClientId,
                                                        new Uri(RedirectUri),
                                                        PromptBehavior.Auto);
```

<span data-ttu-id="1f7e9-157">När du har angett dina autentiseringsuppgifter fortsätta hello exempelprogrammet tooissue autentiserade begäranden toohello Batch management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-157">After you provide your credentials, hello sample application can proceed tooissue authenticated requests toohello Batch management service.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="1f7e9-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1f7e9-158">Next steps</span></span>

<span data-ttu-id="1f7e9-159">Mer information om hur du kör hello [AccountManagement exempelprogrammet][acct_mgmt_sample], se [hantera Batch-konton och kvoter med hello Batch Management-klientbibliotek för .NET](batch-management-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="1f7e9-159">For more information on running hello [AccountManagement sample application][acct_mgmt_sample], see [Manage Batch accounts and quotas with hello Batch Management client library for .NET](batch-management-dotnet.md).</span></span>

<span data-ttu-id="1f7e9-160">toolearn mer om Azure AD finns hello [Azure Active Directory-dokumentationen](https://docs.microsoft.com/azure/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="1f7e9-160">toolearn more about Azure AD, see hello [Azure Active Directory Documentation](https://docs.microsoft.com/azure/active-directory/).</span></span> <span data-ttu-id="1f7e9-161">Djupgående exempel som visar hur toouse ADAL är tillgängliga i hello [Azure kodexempel](https://azure.microsoft.com/resources/samples/?service=active-directory) bibliotek.</span><span class="sxs-lookup"><span data-stu-id="1f7e9-161">In-depth examples showing how toouse ADAL are available in hello [Azure Code Samples](https://azure.microsoft.com/resources/samples/?service=active-directory) library.</span></span>

<span data-ttu-id="1f7e9-162">tooauthenticate Batch-tjänstprogram med Azure AD finns [autentiserar Batch tjänstelösningar med Active Directory](batch-aad-auth.md).</span><span class="sxs-lookup"><span data-stu-id="1f7e9-162">tooauthenticate Batch service applications using Azure AD, see [Authenticate Batch service solutions with Active Directory](batch-aad-auth.md).</span></span> 


[aad_about]: ../active-directory/active-directory-whatis.md "Vad är Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Autentiseringsscenarier för Azure AD"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrera program med Azure Active Directory"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[azure_portal]: http://portal.azure.com
[resman_overview]: ../azure-resource-manager/resource-group-overview.md
