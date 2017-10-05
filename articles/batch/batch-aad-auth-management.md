---
title: "Använda Azure Active Directory för att autentisera Batch hanteringslösningar | Microsoft Docs"
description: Program som skapats med Azure resource manager och Batch-resursprovidern autentisera med Azure AD.
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
ms.openlocfilehash: 26d4adf4f74f9aacc4cf8cf24be293ebdb4d63c8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="authenticate-batch-management-solutions-with-active-directory"></a><span data-ttu-id="4fac3-103">Autentisera Batch hanteringslösningar med Active Directory</span><span class="sxs-lookup"><span data-stu-id="4fac3-103">Authenticate Batch Management solutions with Active Directory</span></span>

<span data-ttu-id="4fac3-104">Program som anropar tjänsten Azure Batch Management autentiseras med [Azure Active Directory] [ aad_about] (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4fac3-104">Applications that call the Azure Batch Management service authenticate with [Azure Active Directory][aad_about] (Azure AD).</span></span> <span data-ttu-id="4fac3-105">Azure AD är Microsofts flera innehavare molnbaserad katalog och identity management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4fac3-105">Azure AD is Microsoft’s multi-tenant cloud based directory and identity management service.</span></span> <span data-ttu-id="4fac3-106">Azure själva använder Azure AD för autentisering av dess kunder, administratörer och användare i organisationer.</span><span class="sxs-lookup"><span data-stu-id="4fac3-106">Azure itself uses Azure AD for the authentication of its customers, service administrators, and organizational users.</span></span>

<span data-ttu-id="4fac3-107">Batch Management .NET-biblioteket visar typer för att arbeta med Batch-konton, nycklar, program och programpaket.</span><span class="sxs-lookup"><span data-stu-id="4fac3-107">The Batch Management .NET library exposes types for working with Batch accounts, account keys, applications, and application packages.</span></span> <span data-ttu-id="4fac3-108">Batch Management .NET-biblioteket är en Azure resource provider-klient och används tillsammans med [Azure Resource Manager] [ resman_overview] att hantera resurserna via programmering.</span><span class="sxs-lookup"><span data-stu-id="4fac3-108">The Batch Management .NET library is an Azure resource provider client, and is used together with [Azure Resource Manager][resman_overview] to manage these resources programmatically.</span></span> <span data-ttu-id="4fac3-109">Azure AD för att kunna autentisera begäranden som görs via alla Azure-resurs providern klienter, inklusive Batch Management .NET-bibliotek och via [Azure Resource Manager][resman_overview].</span><span class="sxs-lookup"><span data-stu-id="4fac3-109">Azure AD is required to authenticate requests made through any Azure resource provider client, including the Batch Management .NET library, and through [Azure Resource Manager][resman_overview].</span></span>

<span data-ttu-id="4fac3-110">I den här artikeln förklarar vi använda Azure AD för autentisering från program som använder Batch Management .NET-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="4fac3-110">In this article, we explore using Azure AD to authenticate from applications that use the Batch Management .NET library.</span></span> <span data-ttu-id="4fac3-111">Vi hur du använder Azure AD för att autentisera en prenumeration administratör eller medadministratör, med integrerad autentisering.</span><span class="sxs-lookup"><span data-stu-id="4fac3-111">We show how to use Azure AD to authenticate a subscription administrator or co-administrator, using integrated authentication.</span></span> <span data-ttu-id="4fac3-112">Vi använder den [AccountManagment] [ acct_mgmt_sample] exempelprojektet finns på GitHub, kan du gå igenom med hjälp av Azure AD med Batch Management .NET-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="4fac3-112">We use the [AccountManagment][acct_mgmt_sample] sample project, available on GitHub, to walk through using Azure AD with the Batch Management .NET library.</span></span>

<span data-ttu-id="4fac3-113">Mer information om hur du använder Batch Management .NET-bibliotek och AccountManagement-exempel finns [hantera Batch-konton och kvoter med Batch Management-klientbibliotek för .NET](batch-management-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="4fac3-113">To learn more about using the Batch Management .NET library and the AccountManagement sample, see [Manage Batch accounts and quotas with the Batch Management client library for .NET](batch-management-dotnet.md).</span></span>

## <a name="register-your-application-with-azure-ad"></a><span data-ttu-id="4fac3-114">Registrera ditt program med Azure AD</span><span class="sxs-lookup"><span data-stu-id="4fac3-114">Register your application with Azure AD</span></span>

<span data-ttu-id="4fac3-115">Azure [Active Directory Authentication Library] [ aad_adal] (ADAL) tillhandahåller ett programmeringsgränssnitt till Azure AD för användning i dina program.</span><span class="sxs-lookup"><span data-stu-id="4fac3-115">The Azure [Active Directory Authentication Library][aad_adal] (ADAL) provides a programmatic interface to Azure AD for use within your applications.</span></span> <span data-ttu-id="4fac3-116">För att anropa ADAL från ditt program, måste du registrera ditt program i en Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="4fac3-116">To call ADAL from your application, you must register your application in an Azure AD tenant.</span></span> <span data-ttu-id="4fac3-117">När du registrerar ditt program kan ange du Azure AD med information om ditt program, inklusive ett namn för den i Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="4fac3-117">When you register your application, you supply Azure AD with information about your application, including a name for it within the Azure AD tenant.</span></span> <span data-ttu-id="4fac3-118">Azure AD sedan innehåller ett program-ID som används för att associera ditt program med Azure AD under körning.</span><span class="sxs-lookup"><span data-stu-id="4fac3-118">Azure AD then provides an application ID that you use to associate your application with Azure AD at runtime.</span></span> <span data-ttu-id="4fac3-119">Mer information om program-ID finns [program och tjänstens huvudnamn objekt i Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="4fac3-119">To learn more about the application ID, see [Application and service principal objects in Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span></span>

<span data-ttu-id="4fac3-120">Om du vill registrera AccountManagement exempelprogrammet, följer du stegen i den [lägga till ett program](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) i avsnittet [integrera program med Azure Active Directory] [ aad_integrate].</span><span class="sxs-lookup"><span data-stu-id="4fac3-120">To register the AccountManagement sample application, follow the steps in the [Adding an Application](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) section in [Integrating applications with Azure Active Directory][aad_integrate].</span></span> <span data-ttu-id="4fac3-121">Ange **internt klientprogram** för typ av program.</span><span class="sxs-lookup"><span data-stu-id="4fac3-121">Specify **Native Client Application** for the type of application.</span></span> <span data-ttu-id="4fac3-122">Branschens standard OAuth 2.0-URI för den **omdirigerings-URI** är `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="4fac3-122">The industry standard OAuth 2.0 URI for the **Redirect URI** is `urn:ietf:wg:oauth:2.0:oob`.</span></span> <span data-ttu-id="4fac3-123">Du kan dock ange en giltig URI (t.ex `http://myaccountmanagementsample`) för den **omdirigerings-URI**eftersom den inte behöver vara en verklig slutpunkt:</span><span class="sxs-lookup"><span data-stu-id="4fac3-123">However, you can specify any valid URI (such as `http://myaccountmanagementsample`) for the **Redirect URI**, as it does not need to be a real endpoint:</span></span>

![](./media/batch-aad-auth-management/app-registration-management-plane.png)

<span data-ttu-id="4fac3-124">När du har slutfört registreringsprocessen visas det program-ID och Objektidentifieraren (tjänstens huvudnamn) för ditt program.</span><span class="sxs-lookup"><span data-stu-id="4fac3-124">Once you complete the registration process, you'll see the application ID and the object (service principal) ID listed for your application.</span></span>  

![](./media/batch-aad-auth-management/app-registration-client-id.png)

## <a name="grant-the-azure-resource-manager-api-access-to-your-application"></a><span data-ttu-id="4fac3-125">Ge Azure Resource Manager API-åtkomst till ditt program</span><span class="sxs-lookup"><span data-stu-id="4fac3-125">Grant the Azure Resource Manager API access to your application</span></span>

<span data-ttu-id="4fac3-126">Därefter måste du delegera åtkomst till ditt program till Azure Resource Manager API.</span><span class="sxs-lookup"><span data-stu-id="4fac3-126">Next, you'll need to delegate access to your application to the Azure Resource Manager API.</span></span> <span data-ttu-id="4fac3-127">Azure AD-identifieraren för Resource Manager API är **Windows Azure Service Management API**.</span><span class="sxs-lookup"><span data-stu-id="4fac3-127">The Azure AD identifier for the Resource Manager API is **Windows Azure Service Management API**.</span></span>

<span data-ttu-id="4fac3-128">Följ dessa steg i Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="4fac3-128">Follow these steps in the Azure portal:</span></span>

1. <span data-ttu-id="4fac3-129">I det vänstra navigeringsfönstret i Azure portal väljer **fler tjänster**, klickar du på **App registreringar**, och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="4fac3-129">In the left-hand navigation pane of the Azure portal, choose **More Services**, click **App Registrations**, and click **Add**.</span></span>
2. <span data-ttu-id="4fac3-130">Sök efter namnet på programmet i listan över app registreringar:</span><span class="sxs-lookup"><span data-stu-id="4fac3-130">Search for the name of your application in the list of app registrations:</span></span>

    ![Sök efter programnamnet](./media/batch-aad-auth-management/search-app-registration.png)

3. <span data-ttu-id="4fac3-132">Visa den **inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="4fac3-132">Display the **Settings** blade.</span></span> <span data-ttu-id="4fac3-133">I den **API-åtkomst** väljer **nödvändiga behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="4fac3-133">In the **API Access** section, select **Required permissions**.</span></span>
4. <span data-ttu-id="4fac3-134">Klicka på **Lägg till** att lägga till en ny nödvändig behörighet.</span><span class="sxs-lookup"><span data-stu-id="4fac3-134">Click **Add** to add a new required permission.</span></span> 
5. <span data-ttu-id="4fac3-135">I steg 1, ange **Windows Azure Service Management API**, välja den API från listan över resultat och på den **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="4fac3-135">In step 1, enter **Windows Azure Service Management API**, select that API from the list of results, and click the **Select** button.</span></span>
6. <span data-ttu-id="4fac3-136">I steg 2, markerar du kryssrutan bredvid **åtkomst Azure klassiska distributionsmodellen som organisationen användare**, och klicka på den **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="4fac3-136">In step 2, select the check box next to **Access Azure classic deployment model as organization users**, and click the **Select** button.</span></span>
7. <span data-ttu-id="4fac3-137">Klicka på den **klar** knappen.</span><span class="sxs-lookup"><span data-stu-id="4fac3-137">Click the **Done** button.</span></span>

<span data-ttu-id="4fac3-138">Den **nödvändiga behörigheter** bladet visar nu att ditt program behörigheter till både ADAL och Resource Manager API: er.</span><span class="sxs-lookup"><span data-stu-id="4fac3-138">The **Required Permissions** blade now shows that permissions to your application are granted to both the ADAL and Resource Manager APIs.</span></span> <span data-ttu-id="4fac3-139">Behörigheter som ADAL som standard när du först registrera din app med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4fac3-139">Permissions are granted to ADAL by default when you first register your app with Azure AD.</span></span>

![Delegera behörigheter till Azure Resource Manager API](./media/batch-aad-auth-management/required-permissions-management-plane.png)

## <a name="azure-ad-endpoints"></a><span data-ttu-id="4fac3-141">Azure AD-slutpunkter</span><span class="sxs-lookup"><span data-stu-id="4fac3-141">Azure AD endpoints</span></span>

<span data-ttu-id="4fac3-142">Du behöver två välkända slutpunkter för att autentisera dina lösningar för Batch-hantering med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4fac3-142">To authenticate your Batch Management solutions with Azure AD, you'll need two well-known endpoints.</span></span>

- <span data-ttu-id="4fac3-143">Den **vanliga Azure AD-slutpunkt** ger en allmän referens samla in gränssnittet när en viss klient inte tillhandahålls, när det gäller integrerad autentisering:</span><span class="sxs-lookup"><span data-stu-id="4fac3-143">The **Azure AD common endpoint** provides a generic credential gathering interface when a specific tenant is not provided, as in the case of integrated authentication:</span></span>

    `https://login.microsoftonline.com/common`

- <span data-ttu-id="4fac3-144">Den **Azure Resource Manager-slutpunkt** används för att hämta en token för att autentisera förfrågningar till Batch management-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="4fac3-144">The **Azure Resource Manager endpoint** is used to acquire a token for authenticating requests to the Batch management service:</span></span>

    `https://management.core.windows.net/`

<span data-ttu-id="4fac3-145">Exempelprogrammet AccountManagement definierar konstanter för dessa slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="4fac3-145">The AccountManagement sample application defines constants for these endpoints.</span></span> <span data-ttu-id="4fac3-146">Lämna dessa konstanter oförändrade:</span><span class="sxs-lookup"><span data-stu-id="4fac3-146">Leave these constants unchanged:</span></span>

```csharp
// Azure Active Directory "common" endpoint.
private const string AuthorityUri = "https://login.microsoftonline.com/common";
// Azure Resource Manager endpoint 
private const string ResourceUri = "https://management.core.windows.net/";
```

## <a name="reference-your-application-id"></a><span data-ttu-id="4fac3-147">Referens-program-ID</span><span class="sxs-lookup"><span data-stu-id="4fac3-147">Reference your application ID</span></span> 

<span data-ttu-id="4fac3-148">Klientprogrammet använder program-ID (kallas även för klient-ID) för att få åtkomst till Azure AD under körning.</span><span class="sxs-lookup"><span data-stu-id="4fac3-148">Your client application uses the application ID (also referred to as the client ID) to access Azure AD at runtime.</span></span> <span data-ttu-id="4fac3-149">När du har registrerat ditt program i Azure-portalen, kan du uppdatera din kod för att använda det program-ID som tillhandahålls av Azure AD för registrerade programmet.</span><span class="sxs-lookup"><span data-stu-id="4fac3-149">Once you've registered your application in the Azure portal, update your code to use the application ID provided by Azure AD for your registered application.</span></span> <span data-ttu-id="4fac3-150">Kopiera program-ID på exempelprogrammet AccountManagement från Azure portal till lämplig konstant:</span><span class="sxs-lookup"><span data-stu-id="4fac3-150">In the AccountManagement sample application, copy your application ID from the Azure portal to the appropriate constant:</span></span>

```csharp
// Specify the unique identifier (the "Client ID") for your application. This is required so that your
// native client application (i.e. this sample) can access the Microsoft Azure AD Graph API. For information
// about registering an application in Azure Active Directory, please see "Adding an Application" here:
// https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
private const string ClientId = "<application-id>";
```
<span data-ttu-id="4fac3-151">Också kopiera omdirigerings-URI som du angav under registreringen.</span><span class="sxs-lookup"><span data-stu-id="4fac3-151">Also copy the redirect URI that you specified during the registration process.</span></span> <span data-ttu-id="4fac3-152">Omdirigerings-URI som angetts i din kod måste matcha omdirigerings-URI som du angav när du registrerade programmet.</span><span class="sxs-lookup"><span data-stu-id="4fac3-152">The redirect URI specified in your code must match the redirect URI that you provided when you registered the application.</span></span>

```csharp
// The URI to which Azure AD will redirect in response to an OAuth 2.0 request. This value is
// specified by you when you register an application with AAD (see ClientId comment). It does not
// need to be a real endpoint, but must be a valid URI (e.g. https://accountmgmtsampleapp).
private const string RedirectUri = "http://myaccountmanagementsample";
```

## <a name="acquire-an-azure-ad-authentication-token"></a><span data-ttu-id="4fac3-153">Skaffa en Azure AD-token för autentisering</span><span class="sxs-lookup"><span data-stu-id="4fac3-153">Acquire an Azure AD authentication token</span></span>

<span data-ttu-id="4fac3-154">När du registrerar AccountManagement exemplet i Azure AD-klient och uppdatera källan exempelkoden med värdena är exemplet redo att autentisera med hjälp av Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4fac3-154">After you register the AccountManagement sample in the Azure AD tenant and update the sample source code with your values, the sample is ready to authenticate using Azure AD.</span></span> <span data-ttu-id="4fac3-155">När du kör exemplet försöker ADAL hämta en autentiseringstoken.</span><span class="sxs-lookup"><span data-stu-id="4fac3-155">When you run the sample, the ADAL attempts to acquire an authentication token.</span></span> <span data-ttu-id="4fac3-156">I det här steget ombeds du ange autentiseringsuppgifterna för ditt Microsoft:</span><span class="sxs-lookup"><span data-stu-id="4fac3-156">At this step, it prompts you for your Microsoft credentials:</span></span> 

```csharp
// Obtain an access token using the "common" AAD resource. This allows the application
// to query AAD for information that lies outside the application's tenant (such as for
// querying subscription information in your Azure account).
AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
AuthenticationResult authResult = authContext.AcquireToken(ResourceUri,
                                                        ClientId,
                                                        new Uri(RedirectUri),
                                                        PromptBehavior.Auto);
```

<span data-ttu-id="4fac3-157">När du har angett dina autentiseringsuppgifter kan exempelprogrammet fortsätta att utfärda autentiserade begäranden till Batch management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4fac3-157">After you provide your credentials, the sample application can proceed to issue authenticated requests to the Batch management service.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="4fac3-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4fac3-158">Next steps</span></span>

<span data-ttu-id="4fac3-159">Mer information om körs den [AccountManagement exempelprogrammet][acct_mgmt_sample], se [hantera Batch-konton och kvoter med Batch Management-klientbibliotek för .NET](batch-management-dotnet.md) .</span><span class="sxs-lookup"><span data-stu-id="4fac3-159">For more information on running the [AccountManagement sample application][acct_mgmt_sample], see [Manage Batch accounts and quotas with the Batch Management client library for .NET](batch-management-dotnet.md).</span></span>

<span data-ttu-id="4fac3-160">Mer information om Azure AD finns i [Azure Active Directory-dokumentationen](https://docs.microsoft.com/azure/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="4fac3-160">To learn more about Azure AD, see the [Azure Active Directory Documentation](https://docs.microsoft.com/azure/active-directory/).</span></span> <span data-ttu-id="4fac3-161">Djupgående exempel som visar hur du använder ADAL finns i den [Azure kodexempel](https://azure.microsoft.com/resources/samples/?service=active-directory) bibliotek.</span><span class="sxs-lookup"><span data-stu-id="4fac3-161">In-depth examples showing how to use ADAL are available in the [Azure Code Samples](https://azure.microsoft.com/resources/samples/?service=active-directory) library.</span></span>

<span data-ttu-id="4fac3-162">För att autentisera Batch-tjänstprogram med Azure AD finns [autentiserar Batch tjänstelösningar med Active Directory](batch-aad-auth.md).</span><span class="sxs-lookup"><span data-stu-id="4fac3-162">To authenticate Batch service applications using Azure AD, see [Authenticate Batch service solutions with Active Directory](batch-aad-auth.md).</span></span> 


<span data-ttu-id="4fac3-163">[aad_about]: ../active-directory/active-directory-whatis.md "Vad är Azure Active Directory?"</span><span class="sxs-lookup"><span data-stu-id="4fac3-163">[aad_about]: ../active-directory/active-directory-whatis.md "What is Azure Active Directory?"</span></span>
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
<span data-ttu-id="4fac3-164">[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Autentiseringsscenarier för Azure AD"</span><span class="sxs-lookup"><span data-stu-id="4fac3-164">[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Authentication Scenarios for Azure AD"</span></span>
<span data-ttu-id="4fac3-165">[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrera program med Azure Active Directory"</span><span class="sxs-lookup"><span data-stu-id="4fac3-165">[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrating Applications with Azure Active Directory"</span></span>
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[azure_portal]: http://portal.azure.com
[resman_overview]: ../azure-resource-manager/resource-group-overview.md
