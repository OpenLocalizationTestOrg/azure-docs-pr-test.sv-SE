---
title: "aaaUse Azure Active Directory tooauthenticate Azure Batch-tjänstelösningar | Microsoft Docs"
description: "Batch har stöd för Azure AD för autentisering från hello Batch-tjänsten."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/20/2017
ms.author: tamram
ms.openlocfilehash: 6c825c30f1c80bb059a797a2e78367e599acd109
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-batch-service-solutions-with-active-directory"></a><span data-ttu-id="56b07-103">Autentisera Batch tjänstelösningar med Active Directory</span><span class="sxs-lookup"><span data-stu-id="56b07-103">Authenticate Batch service solutions with Active Directory</span></span>

<span data-ttu-id="56b07-104">Azure Batch stöder autentisering med [Azure Active Directory] [ aad_about] (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="56b07-104">Azure Batch supports authentication with [Azure Active Directory][aad_about] (Azure AD).</span></span> <span data-ttu-id="56b07-105">Azure AD är Microsofts flera innehavare molnbaserad katalog och identity management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="56b07-105">Azure AD is Microsoft’s multi-tenant cloud based directory and identity management service.</span></span> <span data-ttu-id="56b07-106">Azure själva använder Azure AD tooauthenticate dess kunder, administratörer och användare i organisationer.</span><span class="sxs-lookup"><span data-stu-id="56b07-106">Azure itself uses Azure AD tooauthenticate its customers, service administrators, and organizational users.</span></span>

<span data-ttu-id="56b07-107">När du använder Azure AD-autentisering med Azure Batch kan du autentisera i ett av två sätt:</span><span class="sxs-lookup"><span data-stu-id="56b07-107">When using Azure AD authentication with Azure Batch, you can authenticate in one of two ways:</span></span>

- <span data-ttu-id="56b07-108">Med hjälp av **integrerad autentisering** tooauthenticate en användare som interagerar med programmet hello.</span><span class="sxs-lookup"><span data-stu-id="56b07-108">By using **integrated authentication** tooauthenticate a user that is interacting with hello application.</span></span> <span data-ttu-id="56b07-109">Ett program med integrerad autentisering samlar in en användares autentiseringsuppgifter och använder dessa autentiseringsuppgifter tooauthenticate åtkomst tooBatch resurser.</span><span class="sxs-lookup"><span data-stu-id="56b07-109">An application using integrated authentication gathers a user's credentials and uses those credentials tooauthenticate access tooBatch resources.</span></span>
- <span data-ttu-id="56b07-110">Med hjälp av en **tjänstens huvudnamn** tooauthenticate en oövervakad.</span><span class="sxs-lookup"><span data-stu-id="56b07-110">By using a **service principal** tooauthenticate an unattended application.</span></span> <span data-ttu-id="56b07-111">Ett huvudnamn för tjänsten definierar hello principer och behörigheter för ett program i ordning toorepresent hello program vid åtkomst till resurser vid körning.</span><span class="sxs-lookup"><span data-stu-id="56b07-111">A service principal defines hello policy and permissions for an application in order toorepresent hello application when accessing resources at runtime.</span></span>

<span data-ttu-id="56b07-112">toolearn mer om Azure AD finns hello [Azure Active Directory-dokumentationen](https://docs.microsoft.com/azure/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="56b07-112">toolearn more about Azure AD, see hello [Azure Active Directory Documentation](https://docs.microsoft.com/azure/active-directory/).</span></span>

## <a name="authentication-and-pool-allocation-mode"></a><span data-ttu-id="56b07-113">Läget för autentisering och pool allokering</span><span class="sxs-lookup"><span data-stu-id="56b07-113">Authentication and pool allocation mode</span></span>

<span data-ttu-id="56b07-114">När du skapar ett Batch-konto kan ange du där pooler för detta konto bör tilldelas.</span><span class="sxs-lookup"><span data-stu-id="56b07-114">When you create a Batch account, you can specify where pools created for that account should be allocated.</span></span> <span data-ttu-id="56b07-115">Du kan välja tooallocate pooler i hello standardabonnemang Batch-tjänsten eller i en prenumeration för användaren.</span><span class="sxs-lookup"><span data-stu-id="56b07-115">You can choose tooallocate pools either in hello default Batch service subscription or in a user subscription.</span></span> <span data-ttu-id="56b07-116">Du väljer påverkar hur du autentisera åtkomst tooresources på det kontot.</span><span class="sxs-lookup"><span data-stu-id="56b07-116">Your choice affects how you authenticate access tooresources in that account.</span></span>

- <span data-ttu-id="56b07-117">**Batch-tjänstprenumeration**.</span><span class="sxs-lookup"><span data-stu-id="56b07-117">**Batch service subscription**.</span></span> <span data-ttu-id="56b07-118">Som standard tilldelas Batch-pooler i en prenumeration på Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="56b07-118">By default, Batch pools are allocated in a Batch service subscription.</span></span> <span data-ttu-id="56b07-119">Om du väljer det här alternativet kan du kan autentisera åtkomst tooresources på det kontot antingen med [delad nyckel](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service) eller med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="56b07-119">If you choose this option, you can authenticate access tooresources in that account either with [Shared Key](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service) or with Azure AD.</span></span>
- <span data-ttu-id="56b07-120">**Användaren prenumeration.**</span><span class="sxs-lookup"><span data-stu-id="56b07-120">**User subscription.**</span></span> <span data-ttu-id="56b07-121">Du kan välja tooallocate Batch-pooler i en prenumeration för användare som du anger.</span><span class="sxs-lookup"><span data-stu-id="56b07-121">You can choose tooallocate Batch pools in a user subscription that you specify.</span></span> <span data-ttu-id="56b07-122">Om du väljer det här alternativet måste du autentisera med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="56b07-122">If you choose this option, you must authenticate with Azure AD.</span></span>

## <a name="endpoints-for-authentication"></a><span data-ttu-id="56b07-123">Slutpunkter för autentisering</span><span class="sxs-lookup"><span data-stu-id="56b07-123">Endpoints for authentication</span></span>

<span data-ttu-id="56b07-124">tooauthenticate Batch-program med Azure AD, måste tooinclude vissa välkända slutpunkter i koden.</span><span class="sxs-lookup"><span data-stu-id="56b07-124">tooauthenticate Batch applications with Azure AD, you need tooinclude some well-known endpoints in your code.</span></span>

### <a name="azure-ad-endpoint"></a><span data-ttu-id="56b07-125">Azure AD-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="56b07-125">Azure AD endpoint</span></span>

<span data-ttu-id="56b07-126">grundläggande hello Azure AD myndighet slutpunkten är:</span><span class="sxs-lookup"><span data-stu-id="56b07-126">hello base Azure AD authority endpoint is:</span></span>

`https://login.microsoftonline.com/`

<span data-ttu-id="56b07-127">tooauthenticate med Azure AD, Använd den här slutpunkten tillsammans med hello klient-ID (katalog-ID).</span><span class="sxs-lookup"><span data-stu-id="56b07-127">tooauthenticate with Azure AD, you use this endpoint together with hello tenant ID (directory ID).</span></span> <span data-ttu-id="56b07-128">hello klient-ID identifierar hello Azure AD-klient toouse för autentisering.</span><span class="sxs-lookup"><span data-stu-id="56b07-128">hello tenant ID identifies hello Azure AD tenant toouse for authentication.</span></span> <span data-ttu-id="56b07-129">tooretrieve Hej klient-ID, Följ stegen i hello [hämta hello klient-ID för Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span><span class="sxs-lookup"><span data-stu-id="56b07-129">tooretrieve hello tenant ID, follow hello steps outlined in [Get hello tenant ID for your Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span></span>

`https://login.microsoftonline.com/<tenant-id>`

> [!NOTE] 
> <span data-ttu-id="56b07-130">hello klient-specifika slutpunkt krävs när du autentiserar med hjälp av ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="56b07-130">hello tenant-specific endpoint is required when you authenticate using a service principal.</span></span> 
> 
> <span data-ttu-id="56b07-131">hello klient-specifika slutpunkt är valfritt när du autentiserar med integrerad autentisering, men rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="56b07-131">hello tenant-specific endpoint is optional when you authenticate using integrated authentication, but recommended.</span></span> <span data-ttu-id="56b07-132">Du kan också använda vanliga hello Azure AD-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="56b07-132">However, you can also use hello Azure AD common endpoint.</span></span> <span data-ttu-id="56b07-133">hello vanliga endpoint ger en allmän referens samla in gränssnittet när en viss klient inte har angetts.</span><span class="sxs-lookup"><span data-stu-id="56b07-133">hello common endpoint provides a generic credential gathering interface when a specific tenant is not provided.</span></span> <span data-ttu-id="56b07-134">vanliga hello-slutpunkten är `https://login.microsoftonline.com/common`.</span><span class="sxs-lookup"><span data-stu-id="56b07-134">hello common endpoint is `https://login.microsoftonline.com/common`.</span></span>
>
>

<span data-ttu-id="56b07-135">Läs mer om Azure AD-slutpunkter [Autentiseringsscenarier för Azure AD][aad_auth_scenarios].</span><span class="sxs-lookup"><span data-stu-id="56b07-135">For more information about Azure AD endpoints, see [Authentication Scenarios for Azure AD][aad_auth_scenarios].</span></span>

### <a name="batch-resource-endpoint"></a><span data-ttu-id="56b07-136">Slutpunkten för batch-resurs</span><span class="sxs-lookup"><span data-stu-id="56b07-136">Batch resource endpoint</span></span>

<span data-ttu-id="56b07-137">Använd hello **Azure Batch resurs endpoint** tooacquire en token för att autentisera begäranden toohello Batch-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="56b07-137">Use hello **Azure Batch resource endpoint** tooacquire a token for authenticating requests toohello Batch service:</span></span>

`https://batch.core.windows.net/`

## <a name="register-your-application-with-a-tenant"></a><span data-ttu-id="56b07-138">Registrera ditt program till en klient</span><span class="sxs-lookup"><span data-stu-id="56b07-138">Register your application with a tenant</span></span>

<span data-ttu-id="56b07-139">hello första steget i med hjälp av Azure AD tooauthenticate registrerar ditt program i en Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="56b07-139">hello first step in using Azure AD tooauthenticate is registering your application in an Azure AD tenant.</span></span> <span data-ttu-id="56b07-140">Registrera ditt program kan du toocall hello Azure [Active Directory Authentication Library] [ aad_adal] (ADAL) från din kod.</span><span class="sxs-lookup"><span data-stu-id="56b07-140">Registering your application enables you toocall hello Azure [Active Directory Authentication Library][aad_adal] (ADAL) from your code.</span></span> <span data-ttu-id="56b07-141">Hej ADAL tillhandahåller ett API för att autentisera med Azure AD från ditt program.</span><span class="sxs-lookup"><span data-stu-id="56b07-141">hello ADAL provides an API for authenticating with Azure AD from your application.</span></span> <span data-ttu-id="56b07-142">Registrera ditt program krävs om du planerar toouse integrerad autentisering eller ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="56b07-142">Registering your application is required whether you plan toouse integrated authentication or a service principal.</span></span>

<span data-ttu-id="56b07-143">När du registrerar ditt program kan du ange information om ditt program tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="56b07-143">When you register your application, you supply information about your application tooAzure AD.</span></span> <span data-ttu-id="56b07-144">Azure AD sedan innehåller ett program-ID som du använder tooassociate ditt program med Azure AD under körning.</span><span class="sxs-lookup"><span data-stu-id="56b07-144">Azure AD then provides an application ID that you use tooassociate your application with Azure AD at runtime.</span></span> <span data-ttu-id="56b07-145">toolearn mer om hello program-ID, se [program och tjänstens huvudnamn objekt i Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="56b07-145">toolearn more about hello application ID, see [Application and service principal objects in Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span></span>

<span data-ttu-id="56b07-146">tooregister Batch programmet, följ hello stegen i hello [lägga till ett program](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) i avsnittet [integrera program med Azure Active Directory][aad_integrate].</span><span class="sxs-lookup"><span data-stu-id="56b07-146">tooregister your Batch application, follow hello steps in hello [Adding an Application](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) section in [Integrating applications with Azure Active Directory][aad_integrate].</span></span> <span data-ttu-id="56b07-147">Om du registrerar ditt program som det ursprungliga programmet kan du ange en giltig URI för hello **omdirigerings-URI**.</span><span class="sxs-lookup"><span data-stu-id="56b07-147">If you register your application as a Native Application, you can specify any valid URI for hello **Redirect URI**.</span></span> <span data-ttu-id="56b07-148">Det behöver inte toobe en verklig slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="56b07-148">It does not need toobe a real endpoint.</span></span>

<span data-ttu-id="56b07-149">När du har registrerat ditt program, ser du hello program-ID:</span><span class="sxs-lookup"><span data-stu-id="56b07-149">After you've registered your application, you'll see hello application ID:</span></span>

![Registrera ditt Batch-program med Azure AD](./media/batch-aad-auth/app-registration-data-plane.png)

<span data-ttu-id="56b07-151">Mer information om hur du registrerar ett program med Azure AD finns [Autentiseringsscenarier för Azure AD](../active-directory/develop/active-directory-authentication-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="56b07-151">For more information about registering an application with Azure AD, see [Authentication Scenarios for Azure AD](../active-directory/develop/active-directory-authentication-scenarios.md).</span></span>

## <a name="get-hello-tenant-id-for-your-active-directory"></a><span data-ttu-id="56b07-152">Hämta hello klient-ID för ditt Active Directory</span><span class="sxs-lookup"><span data-stu-id="56b07-152">Get hello tenant ID for your Active Directory</span></span>

<span data-ttu-id="56b07-153">hello klient-ID identifierar hello Azure AD-klient som tillhandahåller autentisering services tooyour-programmet.</span><span class="sxs-lookup"><span data-stu-id="56b07-153">hello tenant ID identifies hello Azure AD tenant that provides authentication services tooyour application.</span></span> <span data-ttu-id="56b07-154">tooget Hej klient-ID, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="56b07-154">tooget hello tenant ID, follow these steps:</span></span>

1. <span data-ttu-id="56b07-155">Välj din Active Directory i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="56b07-155">In hello Azure portal, select your Active Directory.</span></span>
2. <span data-ttu-id="56b07-156">Klicka på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="56b07-156">Click **Properties**.</span></span>
3. <span data-ttu-id="56b07-157">Kopiera hello GUID-värde för hello directory-ID.</span><span class="sxs-lookup"><span data-stu-id="56b07-157">Copy hello GUID value provided for hello directory ID.</span></span> <span data-ttu-id="56b07-158">Det här värdet kallas även hello klient-ID.</span><span class="sxs-lookup"><span data-stu-id="56b07-158">This value is also called hello tenant ID.</span></span>

![Kopiera hello katalog-ID](./media/batch-aad-auth/aad-directory-id.png)


## <a name="use-integrated-authentication"></a><span data-ttu-id="56b07-160">Använda integrerad autentisering</span><span class="sxs-lookup"><span data-stu-id="56b07-160">Use integrated authentication</span></span>

<span data-ttu-id="56b07-161">tooauthenticate med integrerad autentisering måste du toogrant ditt program behörighet tooconnect toohello API för Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="56b07-161">tooauthenticate with integrated authentication, you need toogrant your application permissions tooconnect toohello Batch service API.</span></span> <span data-ttu-id="56b07-162">Det här steget kan ditt program tooauthenticate anrop toohello Batch-tjänsten API med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="56b07-162">This step enables your application tooauthenticate calls toohello Batch service API with Azure AD.</span></span>

<span data-ttu-id="56b07-163">När du har [registrerade programmet](#register-your-application-with-an-azure-ad-tenant), Följ dessa steg i hello Azure portal toogrant den komma åt toohello Batch-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="56b07-163">Once you've [registered your application](#register-your-application-with-an-azure-ad-tenant), follow these steps in hello Azure portal toogrant it access toohello Batch service:</span></span>

1. <span data-ttu-id="56b07-164">Hello vänstra navigeringsfönstret för hello Azure-portalen och väljer **fler tjänster**, klickar du på **App registreringar**.</span><span class="sxs-lookup"><span data-stu-id="56b07-164">In hello left-hand navigation pane of hello Azure portal, choose **More Services**, click **App Registrations**.</span></span>
2. <span data-ttu-id="56b07-165">Sök efter hello namnet på ditt program i hello lista över app registreringar:</span><span class="sxs-lookup"><span data-stu-id="56b07-165">Search for hello name of your application in hello list of app registrations:</span></span>

    ![Sök efter programnamnet](./media/batch-aad-auth/search-app-registration.png)

3. <span data-ttu-id="56b07-167">Öppna hello **inställningar** bladet för ditt program.</span><span class="sxs-lookup"><span data-stu-id="56b07-167">Open hello **Settings** blade for your application.</span></span> <span data-ttu-id="56b07-168">I hello **API-åtkomst** väljer **nödvändiga behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="56b07-168">In hello **API Access** section, select **Required permissions**.</span></span>
4. <span data-ttu-id="56b07-169">I hello **nödvändiga behörigheter** bladet, klickar du på hello **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="56b07-169">In hello **Required permissions** blade, click hello **Add** button.</span></span>
5. <span data-ttu-id="56b07-170">I steg 1 ska söka efter hello Batch-API.</span><span class="sxs-lookup"><span data-stu-id="56b07-170">In step 1, search for hello Batch API.</span></span> <span data-ttu-id="56b07-171">Sök efter var och en av de här strängarna tills du hittar hello-API:</span><span class="sxs-lookup"><span data-stu-id="56b07-171">Search for each of these strings until you find hello API:</span></span>
    1. <span data-ttu-id="56b07-172">**MicrosoftAzureBatch**.</span><span class="sxs-lookup"><span data-stu-id="56b07-172">**MicrosoftAzureBatch**.</span></span>
    2. <span data-ttu-id="56b07-173">**Microsoft Azure Batch**.</span><span class="sxs-lookup"><span data-stu-id="56b07-173">**Microsoft Azure Batch**.</span></span> <span data-ttu-id="56b07-174">Nyare Azure AD-klientorganisationer kan använda det här namnet.</span><span class="sxs-lookup"><span data-stu-id="56b07-174">Newer Azure AD tenants may use this name.</span></span>
    3. <span data-ttu-id="56b07-175">**ddbf3205-c6bd-46ae-8127-60eb93363864** är hello-ID för hello Batch-API.</span><span class="sxs-lookup"><span data-stu-id="56b07-175">**ddbf3205-c6bd-46ae-8127-60eb93363864** is hello ID for hello Batch API.</span></span> 
6. <span data-ttu-id="56b07-176">När du har hittat hello Batch-API och på hello **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="56b07-176">Once you find hello Batch API, select it and click hello **Select** button.</span></span>
6. <span data-ttu-id="56b07-177">I steg 2, Välj hello kryssrutan bredvid för**Azure Batch-tjänsten för dataåtkomst** och klicka på hello **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="56b07-177">In step 2, select hello check box next too**Access Azure Batch Service** and click hello **Select** button.</span></span>
7. <span data-ttu-id="56b07-178">Klicka på hello **klar** knappen.</span><span class="sxs-lookup"><span data-stu-id="56b07-178">Click hello **Done** button.</span></span>

<span data-ttu-id="56b07-179">Hej **nödvändiga behörigheter** bladet nu visas som Azure AD-program har åtkomst till tooboth ADAL och hello API för Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="56b07-179">hello **Required Permissions** blade now shows that your Azure AD application has access tooboth ADAL and hello Batch service API.</span></span> <span data-ttu-id="56b07-180">Behörigheter tooADAL automatiskt när du först registrera din app med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="56b07-180">Permissions are granted tooADAL automatically when you first register your app with Azure AD.</span></span>

![Bevilja API-behörigheter](./media/batch-aad-auth/required-permissions-data-plane.png)

## <a name="use-a-service-principal"></a><span data-ttu-id="56b07-182">Använd ett huvudnamn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="56b07-182">Use a service principal</span></span> 

<span data-ttu-id="56b07-183">tooauthenticate ett program som körs obevakad, Använd ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="56b07-183">tooauthenticate an application that runs unattended, you use a service principal.</span></span> <span data-ttu-id="56b07-184">När du har registrerat ditt program, gör du så här i hello Azure portal tooconfigure en tjänstens huvudnamn:</span><span class="sxs-lookup"><span data-stu-id="56b07-184">After you've registered your application, follow these steps in hello Azure portal tooconfigure a service principal:</span></span>

1. <span data-ttu-id="56b07-185">Begär en hemlig nyckel för ditt program.</span><span class="sxs-lookup"><span data-stu-id="56b07-185">Request a secret key for your application.</span></span>
2. <span data-ttu-id="56b07-186">Tilldela ett RBAC rollen tooyour program.</span><span class="sxs-lookup"><span data-stu-id="56b07-186">Assign an RBAC role tooyour application.</span></span>

### <a name="request-a-secret-key-for-your-application"></a><span data-ttu-id="56b07-187">Begär en hemlig nyckel för ditt program</span><span class="sxs-lookup"><span data-stu-id="56b07-187">Request a secret key for your application</span></span>

<span data-ttu-id="56b07-188">När programmet autentiseras med ett huvudnamn för tjänsten skickar den både hello program-ID och hemlig nyckel tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="56b07-188">When your application authenticates with a service principal, it sends both hello application ID and a secret key tooAzure AD.</span></span> <span data-ttu-id="56b07-189">Du behöver toocreate och kopiera hello hemlig nyckel toouse från din kod.</span><span class="sxs-lookup"><span data-stu-id="56b07-189">You'll need toocreate and copy hello secret key toouse from your code.</span></span>

<span data-ttu-id="56b07-190">Följ anvisningarna i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="56b07-190">Follow these steps in hello Azure portal:</span></span>

1. <span data-ttu-id="56b07-191">Hello vänstra navigeringsfönstret för hello Azure-portalen och väljer **fler tjänster**, klickar du på **App registreringar**.</span><span class="sxs-lookup"><span data-stu-id="56b07-191">In hello left-hand navigation pane of hello Azure portal, choose **More Services**, click **App Registrations**.</span></span>
2. <span data-ttu-id="56b07-192">Sök efter hello namnet på ditt program i hello lista över app registreringar.</span><span class="sxs-lookup"><span data-stu-id="56b07-192">Search for hello name of your application in hello list of app registrations.</span></span>
3. <span data-ttu-id="56b07-193">Visa hello **inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="56b07-193">Display hello **Settings** blade.</span></span> <span data-ttu-id="56b07-194">I hello **API-åtkomst** väljer **nycklar**.</span><span class="sxs-lookup"><span data-stu-id="56b07-194">In hello **API Access** section, select **Keys**.</span></span>
4. <span data-ttu-id="56b07-195">toocreate en nyckel, ange en beskrivning för hello nyckeln.</span><span class="sxs-lookup"><span data-stu-id="56b07-195">toocreate a key, enter a description for hello key.</span></span> <span data-ttu-id="56b07-196">Välj sedan en varaktighet för hello nyckeln för en eller två år.</span><span class="sxs-lookup"><span data-stu-id="56b07-196">Then select a duration for hello key of either one or two years.</span></span> 
5. <span data-ttu-id="56b07-197">Klicka på hello **spara** knappen toocreate och visa hello nyckel.</span><span class="sxs-lookup"><span data-stu-id="56b07-197">Click hello **Save** button toocreate and display hello key.</span></span> <span data-ttu-id="56b07-198">Kopiera hello nyckelvärdet tooa säker plats som du inte kan tooaccess den igen när du lämnar hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="56b07-198">Copy hello key value tooa safe place, as you won't be able tooaccess it again after you leave hello blade.</span></span> 

    ![Skapa en hemlig nyckel](./media/batch-aad-auth/secret-key.png)

### <a name="assign-an-rbac-role-tooyour-application"></a><span data-ttu-id="56b07-200">Tilldela ett RBAC rollen tooyour program</span><span class="sxs-lookup"><span data-stu-id="56b07-200">Assign an RBAC role tooyour application</span></span>

<span data-ttu-id="56b07-201">tooauthenticate med ett huvudnamn för tjänsten måste tooassign ett RBAC rollen tooyour program.</span><span class="sxs-lookup"><span data-stu-id="56b07-201">tooauthenticate with a service principal, you need tooassign an RBAC role tooyour application.</span></span> <span data-ttu-id="56b07-202">Följ de här stegen:</span><span class="sxs-lookup"><span data-stu-id="56b07-202">Follow these steps:</span></span>

1. <span data-ttu-id="56b07-203">Navigera toohello Batch-kontot som används av programmet hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="56b07-203">In hello Azure portal, navigate toohello Batch account used by your application.</span></span>
2. <span data-ttu-id="56b07-204">I hello **inställningar** bladet för hello Batch-kontot, Välj **Access Control (IAM)**.</span><span class="sxs-lookup"><span data-stu-id="56b07-204">In hello **Settings** blade for hello Batch account, select **Access Control (IAM)**.</span></span>
3. <span data-ttu-id="56b07-205">Klicka på hello **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="56b07-205">Click hello **Add** button.</span></span> 
4. <span data-ttu-id="56b07-206">Från hello **rollen** listrutan, Välj antingen hello _deltagare_ eller _Reader_ roll för ditt program.</span><span class="sxs-lookup"><span data-stu-id="56b07-206">From hello **Role** drop-down, choose either hello _Contributor_ or _Reader_ role for your application.</span></span> <span data-ttu-id="56b07-207">Mer information om dessa roller finns [Kom igång med rollbaserad åtkomstkontroll i hello Azure-portalen](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="56b07-207">For more information on these roles, see [Get started with Role-Based Access Control in hello Azure portal](../active-directory/role-based-access-control-what-is.md).</span></span>  
5. <span data-ttu-id="56b07-208">I hello **Välj** anger hello namnet på ditt program.</span><span class="sxs-lookup"><span data-stu-id="56b07-208">In hello **Select** field, enter hello name of your application.</span></span> <span data-ttu-id="56b07-209">Välj programmet hello listan och klickar på **spara**.</span><span class="sxs-lookup"><span data-stu-id="56b07-209">Select your application from hello list, and click **Save**.</span></span>

<span data-ttu-id="56b07-210">Ditt program bör nu visas i dina inställningar för åtkomstkontroll med en RBAC-roll som tilldelats.</span><span class="sxs-lookup"><span data-stu-id="56b07-210">Your application should now appear in your access control settings with an RBAC role assigned.</span></span> 

![Tilldela ett RBAC rollen tooyour program](./media/batch-aad-auth/app-rbac-role.png)

### <a name="get-hello-tenant-id-for-your-azure-active-directory"></a><span data-ttu-id="56b07-212">Hämta hello klient-ID för Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="56b07-212">Get hello tenant ID for your Azure Active Directory</span></span>

<span data-ttu-id="56b07-213">hello klient-ID identifierar hello Azure AD-klient som tillhandahåller autentisering services tooyour-programmet.</span><span class="sxs-lookup"><span data-stu-id="56b07-213">hello tenant ID identifies hello Azure AD tenant that provides authentication services tooyour application.</span></span> <span data-ttu-id="56b07-214">tooget Hej klient-ID, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="56b07-214">tooget hello tenant ID, follow these steps:</span></span>

1. <span data-ttu-id="56b07-215">Välj din Active Directory i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="56b07-215">In hello Azure portal, select your Active Directory.</span></span>
2. <span data-ttu-id="56b07-216">Klicka på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="56b07-216">Click **Properties**.</span></span>
3. <span data-ttu-id="56b07-217">Kopiera hello GUID-värde för hello directory-ID.</span><span class="sxs-lookup"><span data-stu-id="56b07-217">Copy hello GUID value provided for hello directory ID.</span></span> <span data-ttu-id="56b07-218">Det här värdet kallas även hello klient-ID.</span><span class="sxs-lookup"><span data-stu-id="56b07-218">This value is also called hello tenant ID.</span></span>

![Kopiera hello katalog-ID](./media/batch-aad-auth/aad-directory-id.png)


## <a name="code-examples"></a><span data-ttu-id="56b07-220">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="56b07-220">Code examples</span></span>

<span data-ttu-id="56b07-221">hello kodexempel i det här avsnittet visar hur tooauthenticate med hjälp av Azure AD-integrerad autentisering och med ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="56b07-221">hello code examples in this section show how tooauthenticate with Azure AD using integrated authentication and with a service principal.</span></span> <span data-ttu-id="56b07-222">Dessa kodexempel använda .NET, men hello begrepp är liknande för andra språk.</span><span class="sxs-lookup"><span data-stu-id="56b07-222">These code examples use .NET, but hello concepts are similar for other languages.</span></span>

> [!NOTE]
> <span data-ttu-id="56b07-223">En Azure AD authentication token upphör att gälla efter en timme.</span><span class="sxs-lookup"><span data-stu-id="56b07-223">An Azure AD authentication token expires after one hour.</span></span> <span data-ttu-id="56b07-224">När du använder en långlivade **BatchClient** objekt, rekommenderar vi att du hämtar en token från ADAL på varje begäran tooensure du alltid har en giltig token.</span><span class="sxs-lookup"><span data-stu-id="56b07-224">When using a long-lived **BatchClient** object, we recommend that you retrieve a token from ADAL on every request tooensure you always have a valid token.</span></span> 
>
>
> <span data-ttu-id="56b07-225">tooachieve detta i .NET, skriva en metod som hämtar hello-token från Azure AD och överför den metoden tooa **BatchTokenCredentials** objektet som ett ombud.</span><span class="sxs-lookup"><span data-stu-id="56b07-225">tooachieve this in .NET, write a method that retrieves hello token from Azure AD and pass that method tooa **BatchTokenCredentials** object as a delegate.</span></span> <span data-ttu-id="56b07-226">hello ombud metoden anropas för varje begäran toohello Batch-tjänsten tooensure som en giltig token har angetts.</span><span class="sxs-lookup"><span data-stu-id="56b07-226">hello delegate method is called on every request toohello Batch service tooensure that a valid token is provided.</span></span> <span data-ttu-id="56b07-227">Som standard cachelagrar ADAL-token så att en ny token hämtas från Azure AD bara när det behövs.</span><span class="sxs-lookup"><span data-stu-id="56b07-227">By default ADAL caches tokens, so a new token is retrieved from Azure AD only when necessary.</span></span> <span data-ttu-id="56b07-228">Mer information om token i Azure AD finns [Autentiseringsscenarier för Azure AD][aad_auth_scenarios].</span><span class="sxs-lookup"><span data-stu-id="56b07-228">For more information about tokens in Azure AD, see [Authentication Scenarios for Azure AD][aad_auth_scenarios].</span></span>
>
>

### <a name="code-example-using-azure-ad-integrated-authentication-with-batch-net"></a><span data-ttu-id="56b07-229">Exempel: använda Azure AD-integrerad autentisering med Batch .NET</span><span class="sxs-lookup"><span data-stu-id="56b07-229">Code example: Using Azure AD integrated authentication with Batch .NET</span></span>

<span data-ttu-id="56b07-230">tooauthenticate med integrerad autentisering från Batch .NET referens hello [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) paket- och hello [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) paketet.</span><span class="sxs-lookup"><span data-stu-id="56b07-230">tooauthenticate with integrated authentication from Batch .NET, reference hello [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) package and hello [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) package.</span></span>

<span data-ttu-id="56b07-231">Inkludera hello följande `using` instruktioner i koden:</span><span class="sxs-lookup"><span data-stu-id="56b07-231">Include hello following `using` statements in your code:</span></span>

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

<span data-ttu-id="56b07-232">Referens hello Azure AD-slutpunkt i din kod, inklusive hello klient-ID.</span><span class="sxs-lookup"><span data-stu-id="56b07-232">Reference hello Azure AD endpoint in your code, including hello tenant ID.</span></span> <span data-ttu-id="56b07-233">tooretrieve Hej klient-ID, Följ stegen i hello [hämta hello klient-ID för Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span><span class="sxs-lookup"><span data-stu-id="56b07-233">tooretrieve hello tenant ID, follow hello steps outlined in [Get hello tenant ID for your Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span></span>

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

<span data-ttu-id="56b07-234">Referera till hello Batch tjänstslutpunkten resurs:</span><span class="sxs-lookup"><span data-stu-id="56b07-234">Reference hello Batch service resource endpoint:</span></span>

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

<span data-ttu-id="56b07-235">Referera till Batch-kontot:</span><span class="sxs-lookup"><span data-stu-id="56b07-235">Reference your Batch account:</span></span>

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

<span data-ttu-id="56b07-236">Ange hello program-ID (klient-ID) för ditt program.</span><span class="sxs-lookup"><span data-stu-id="56b07-236">Specify hello application ID (client ID) for your application.</span></span> <span data-ttu-id="56b07-237">hello program-ID är tillgänglig från din appregistrering i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="56b07-237">hello application ID is available from your app registration in hello Azure portal:</span></span>

```csharp
private const string ClientId = "<application-id>";
```

<span data-ttu-id="56b07-238">Också kopiera hello omdirigerings-URI som du angav under hello registreringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="56b07-238">Also copy hello redirect URI that you specified during hello registration process.</span></span> <span data-ttu-id="56b07-239">Hej omdirigerings-URI som anges i din kod måste matcha hello omdirigerings-URI som du angav när du registrerade programmet hello:</span><span class="sxs-lookup"><span data-stu-id="56b07-239">hello redirect URI specified in your code must match hello redirect URI that you provided when you registered hello application:</span></span>

```csharp
private const string RedirectUri = "http://mybatchdatasample";
```

<span data-ttu-id="56b07-240">Skriva ett återanrop metoden tooacquire hello autentiseringstoken från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="56b07-240">Write a callback method tooacquire hello authentication token from Azure AD.</span></span> <span data-ttu-id="56b07-241">Hej **GetAuthenticationTokenAsync** Återanropsmetoden som visas här anropar ADAL tooauthenticate när en användare interagerar med programmet hello.</span><span class="sxs-lookup"><span data-stu-id="56b07-241">hello **GetAuthenticationTokenAsync** callback method shown here calls ADAL tooauthenticate a user who is interacting with hello application.</span></span> <span data-ttu-id="56b07-242">Hej **AcquireTokenAsync** metoden tillhandahålls av ADAL prompter hello användaren sina autentiseringsuppgifter och hello programmet fortsätter när hello användare ger dem (om det redan har cachelagrade autentiseringsuppgifter):</span><span class="sxs-lookup"><span data-stu-id="56b07-242">hello **AcquireTokenAsync** method provided by ADAL prompts hello user for their credentials, and hello application proceeds once hello user provides them (unless it has already cached credentials):</span></span>

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    var authContext = new AuthenticationContext(AuthorityUri);

    // Acquire hello authentication token from Azure AD.
    var authResult = await authContext.AcquireTokenAsync(BatchResourceUri, 
                                                        ClientId, 
                                                        new Uri(RedirectUri), 
                                                        new PlatformParameters(PromptBehavior.Auto));

    return authResult.AccessToken;
}
```

<span data-ttu-id="56b07-243">Skapa en **BatchTokenCredentials** objekt som tar hello delegat som en parameter.</span><span class="sxs-lookup"><span data-stu-id="56b07-243">Construct a **BatchTokenCredentials** object that takes hello delegate as a parameter.</span></span> <span data-ttu-id="56b07-244">Använd dessa autentiseringsuppgifter tooopen en **BatchClient** objekt.</span><span class="sxs-lookup"><span data-stu-id="56b07-244">Use those credentials tooopen a **BatchClient** object.</span></span> <span data-ttu-id="56b07-245">Du kan använda som **BatchClient** objekt för efterföljande åtgärder mot hello Batch-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="56b07-245">You can use that **BatchClient** object for subsequent operations against hello Batch service:</span></span>

```csharp
public static async Task PerformBatchOperations()
{
    Func<Task<string>> tokenProvider = () => GetAuthenticationTokenAsync();

    using (var client = await BatchClient.OpenAsync(new BatchTokenCredentials(BatchAccountUrl, tokenProvider)))
    {
        await client.JobOperations.ListJobs().ToListAsync();
    }
}
```

### <a name="code-example-using-an-azure-ad-service-principal-with-batch-net"></a><span data-ttu-id="56b07-246">Exempel: med hjälp av en Azure AD-tjänstens huvudnamn med Batch .NET</span><span class="sxs-lookup"><span data-stu-id="56b07-246">Code example: Using an Azure AD service principal with Batch .NET</span></span>

<span data-ttu-id="56b07-247">tooauthenticate med ett huvudnamn för tjänsten från Batch .NET referens hello [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) paket- och hello [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) paketet.</span><span class="sxs-lookup"><span data-stu-id="56b07-247">tooauthenticate with a service principal from Batch .NET, reference hello [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) package and hello [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) package.</span></span>

<span data-ttu-id="56b07-248">Inkludera hello följande `using` instruktioner i koden:</span><span class="sxs-lookup"><span data-stu-id="56b07-248">Include hello following `using` statements in your code:</span></span>

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

<span data-ttu-id="56b07-249">Referens hello Azure AD-slutpunkt i din kod, inklusive hello klient-ID.</span><span class="sxs-lookup"><span data-stu-id="56b07-249">Reference hello Azure AD endpoint in your code, including hello tenant ID.</span></span> <span data-ttu-id="56b07-250">Du måste ange en slutpunkt för klienten när du använder ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="56b07-250">When using a service principal, you must provide a tenant-specific endpoint.</span></span> <span data-ttu-id="56b07-251">tooretrieve Hej klient-ID, Följ stegen i hello [hämta hello klient-ID för Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span><span class="sxs-lookup"><span data-stu-id="56b07-251">tooretrieve hello tenant ID, follow hello steps outlined in [Get hello tenant ID for your Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span></span>

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

<span data-ttu-id="56b07-252">Referera till hello Batch tjänstslutpunkten resurs:</span><span class="sxs-lookup"><span data-stu-id="56b07-252">Reference hello Batch service resource endpoint:</span></span>  

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

<span data-ttu-id="56b07-253">Referera till Batch-kontot:</span><span class="sxs-lookup"><span data-stu-id="56b07-253">Reference your Batch account:</span></span>

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

<span data-ttu-id="56b07-254">Ange hello program-ID (klient-ID) för ditt program.</span><span class="sxs-lookup"><span data-stu-id="56b07-254">Specify hello application ID (client ID) for your application.</span></span> <span data-ttu-id="56b07-255">hello program-ID är tillgänglig från din appregistrering i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="56b07-255">hello application ID is available from your app registration in hello Azure portal:</span></span>

```csharp
private const string ClientId = "<application-id>";
```

<span data-ttu-id="56b07-256">Ange hello hemlig nyckel som du kopierade från hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="56b07-256">Specify hello secret key that you copied from hello Azure portal:</span></span>

```csharp
private const string ClientKey = "<secret-key>";
```

<span data-ttu-id="56b07-257">Skriva ett återanrop metoden tooacquire hello autentiseringstoken från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="56b07-257">Write a callback method tooacquire hello authentication token from Azure AD.</span></span> <span data-ttu-id="56b07-258">Hej **GetAuthenticationTokenAsync** Återanropsmetoden som visas här anrop ADAL för obevakad autentisering:</span><span class="sxs-lookup"><span data-stu-id="56b07-258">hello **GetAuthenticationTokenAsync** callback method shown here calls ADAL for unattended authentication:</span></span>

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
    AuthenticationResult authResult = await authContext.AcquireTokenAsync(BatchResourceUri, new ClientCredential(ClientId, ClientKey));

    return authResult.AccessToken;
}
```

<span data-ttu-id="56b07-259">Skapa en **BatchTokenCredentials** objekt som tar hello delegat som en parameter.</span><span class="sxs-lookup"><span data-stu-id="56b07-259">Construct a **BatchTokenCredentials** object that takes hello delegate as a parameter.</span></span> <span data-ttu-id="56b07-260">Använd dessa autentiseringsuppgifter tooopen en **BatchClient** objekt.</span><span class="sxs-lookup"><span data-stu-id="56b07-260">Use those credentials tooopen a **BatchClient** object.</span></span> <span data-ttu-id="56b07-261">Du kan sedan använda **BatchClient** objekt för efterföljande åtgärder mot hello Batch-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="56b07-261">You can then use that **BatchClient** object for subsequent operations against hello Batch service:</span></span>

```csharp
public static async Task PerformBatchOperations()
{
    Func<Task<string>> tokenProvider = () => GetAuthenticationTokenAsync();

    using (var client = await BatchClient.OpenAsync(new BatchTokenCredentials(BatchAccountUrl, tokenProvider)))
    {
        await client.JobOperations.ListJobs().ToListAsync();
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="56b07-262">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="56b07-262">Next steps</span></span>

<span data-ttu-id="56b07-263">toolearn mer om Azure AD finns hello [Azure Active Directory-dokumentationen](https://docs.microsoft.com/azure/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="56b07-263">toolearn more about Azure AD, see hello [Azure Active Directory Documentation](https://docs.microsoft.com/azure/active-directory/).</span></span> <span data-ttu-id="56b07-264">Djupgående exempel som visar hur toouse ADAL är tillgängliga i hello [Azure kodexempel](https://azure.microsoft.com/resources/samples/?service=active-directory) bibliotek.</span><span class="sxs-lookup"><span data-stu-id="56b07-264">In-depth examples showing how toouse ADAL are available in hello [Azure Code Samples](https://azure.microsoft.com/resources/samples/?service=active-directory) library.</span></span>

<span data-ttu-id="56b07-265">toolearn mer om tjänstens huvudnamn finns [program och tjänstens huvudnamn objekt i Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="56b07-265">toolearn more about service principals, see [Application and service principal objects in Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span></span> <span data-ttu-id="56b07-266">toocreate en tjänstens huvudnamn med hjälp av hello Azure portal, se [använda portalen toocreate Active Directory-program och tjänstens huvudnamn som kan komma åt resurser](../resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="56b07-266">toocreate a service principal using hello Azure portal, see [Use portal toocreate Active Directory application and service principal that can access resources](../resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="56b07-267">Du kan också skapa ett huvudnamn för tjänsten med PowerShell eller Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="56b07-267">You can also create a service principal with PowerShell or Azure CLI.</span></span> 

<span data-ttu-id="56b07-268">tooauthenticate Batch-hantering av program med Azure AD finns [autentiserar Batch hanteringslösningar med Active Directory](batch-aad-auth-management.md).</span><span class="sxs-lookup"><span data-stu-id="56b07-268">tooauthenticate Batch Management applications using Azure AD, see [Authenticate Batch Management solutions with Active Directory](batch-aad-auth-management.md).</span></span> 

[aad_about]: ../active-directory/active-directory-whatis.md "Vad är Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Autentiseringsscenarier för Azure AD"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrera program med Azure Active Directory"
[azure_portal]: http://portal.azure.com
