---
title: "Använda Azure Active Directory för att autentisera Azure Batch tjänstelösningar | Microsoft Docs"
description: "Batch har stöd för Azure AD för autentisering från Batch-tjänsten."
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
ms.openlocfilehash: 9c03bde919c46cd301229255c0b12ee69dda6f78
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="authenticate-batch-service-solutions-with-active-directory"></a><span data-ttu-id="222c4-103">Autentisera Batch tjänstelösningar med Active Directory</span><span class="sxs-lookup"><span data-stu-id="222c4-103">Authenticate Batch service solutions with Active Directory</span></span>

<span data-ttu-id="222c4-104">Azure Batch stöder autentisering med [Azure Active Directory] [ aad_about] (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="222c4-104">Azure Batch supports authentication with [Azure Active Directory][aad_about] (Azure AD).</span></span> <span data-ttu-id="222c4-105">Azure AD är Microsofts flera innehavare molnbaserad katalog och identity management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="222c4-105">Azure AD is Microsoft’s multi-tenant cloud based directory and identity management service.</span></span> <span data-ttu-id="222c4-106">Azure själva använder Azure AD för att autentisera sina kunder, administratörer och organisationens användare.</span><span class="sxs-lookup"><span data-stu-id="222c4-106">Azure itself uses Azure AD to authenticate its customers, service administrators, and organizational users.</span></span>

<span data-ttu-id="222c4-107">När du använder Azure AD-autentisering med Azure Batch kan du autentisera i ett av två sätt:</span><span class="sxs-lookup"><span data-stu-id="222c4-107">When using Azure AD authentication with Azure Batch, you can authenticate in one of two ways:</span></span>

- <span data-ttu-id="222c4-108">Med hjälp av **integrerad autentisering** att autentisera en användare interagerar med programmet.</span><span class="sxs-lookup"><span data-stu-id="222c4-108">By using **integrated authentication** to authenticate a user that is interacting with the application.</span></span> <span data-ttu-id="222c4-109">Ett program med integrerad autentisering samlar in en användares autentiseringsuppgifter och använder dessa autentiseringsuppgifter för att autentisera åtkomst till Batch-resurser.</span><span class="sxs-lookup"><span data-stu-id="222c4-109">An application using integrated authentication gathers a user's credentials and uses those credentials to authenticate access to Batch resources.</span></span>
- <span data-ttu-id="222c4-110">Med hjälp av en **tjänstens huvudnamn** att autentisera en oövervakad.</span><span class="sxs-lookup"><span data-stu-id="222c4-110">By using a **service principal** to authenticate an unattended application.</span></span> <span data-ttu-id="222c4-111">Ett huvudnamn för tjänsten definierar principer och behörigheter för ett program för att kunna representera programmet vid åtkomst till resurser vid körning.</span><span class="sxs-lookup"><span data-stu-id="222c4-111">A service principal defines the policy and permissions for an application in order to represent the application when accessing resources at runtime.</span></span>

<span data-ttu-id="222c4-112">Mer information om Azure AD finns i [Azure Active Directory-dokumentationen](https://docs.microsoft.com/azure/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="222c4-112">To learn more about Azure AD, see the [Azure Active Directory Documentation](https://docs.microsoft.com/azure/active-directory/).</span></span>

## <a name="authentication-and-pool-allocation-mode"></a><span data-ttu-id="222c4-113">Läget för autentisering och pool allokering</span><span class="sxs-lookup"><span data-stu-id="222c4-113">Authentication and pool allocation mode</span></span>

<span data-ttu-id="222c4-114">När du skapar ett Batch-konto kan ange du där pooler för detta konto bör tilldelas.</span><span class="sxs-lookup"><span data-stu-id="222c4-114">When you create a Batch account, you can specify where pools created for that account should be allocated.</span></span> <span data-ttu-id="222c4-115">Du kan välja att allokera pooler i standardabonnemang Batch-tjänsten eller i en prenumeration för användaren.</span><span class="sxs-lookup"><span data-stu-id="222c4-115">You can choose to allocate pools either in the default Batch service subscription or in a user subscription.</span></span> <span data-ttu-id="222c4-116">Du väljer påverkar hur du autentiserar dig åtkomst till resurser i kontot.</span><span class="sxs-lookup"><span data-stu-id="222c4-116">Your choice affects how you authenticate access to resources in that account.</span></span>

- <span data-ttu-id="222c4-117">**Batch-tjänstprenumeration**.</span><span class="sxs-lookup"><span data-stu-id="222c4-117">**Batch service subscription**.</span></span> <span data-ttu-id="222c4-118">Som standard tilldelas Batch-pooler i en prenumeration på Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="222c4-118">By default, Batch pools are allocated in a Batch service subscription.</span></span> <span data-ttu-id="222c4-119">Om du väljer det här alternativet kan du kan autentisera åtkomst till resurser i kontot antingen med [delad nyckel](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service) eller med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="222c4-119">If you choose this option, you can authenticate access to resources in that account either with [Shared Key](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service) or with Azure AD.</span></span>
- <span data-ttu-id="222c4-120">**Användaren prenumeration.**</span><span class="sxs-lookup"><span data-stu-id="222c4-120">**User subscription.**</span></span> <span data-ttu-id="222c4-121">Du kan välja att allokera Batch-pooler i en prenumeration för användare som du anger.</span><span class="sxs-lookup"><span data-stu-id="222c4-121">You can choose to allocate Batch pools in a user subscription that you specify.</span></span> <span data-ttu-id="222c4-122">Om du väljer det här alternativet måste du autentisera med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="222c4-122">If you choose this option, you must authenticate with Azure AD.</span></span>

## <a name="endpoints-for-authentication"></a><span data-ttu-id="222c4-123">Slutpunkter för autentisering</span><span class="sxs-lookup"><span data-stu-id="222c4-123">Endpoints for authentication</span></span>

<span data-ttu-id="222c4-124">Om du vill autentisera Batch-program med Azure AD, måste du inkludera vissa välkända slutpunkter i koden.</span><span class="sxs-lookup"><span data-stu-id="222c4-124">To authenticate Batch applications with Azure AD, you need to include some well-known endpoints in your code.</span></span>

### <a name="azure-ad-endpoint"></a><span data-ttu-id="222c4-125">Azure AD-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="222c4-125">Azure AD endpoint</span></span>

<span data-ttu-id="222c4-126">Basen Azure AD myndighet slutpunkten är:</span><span class="sxs-lookup"><span data-stu-id="222c4-126">The base Azure AD authority endpoint is:</span></span>

`https://login.microsoftonline.com/`

<span data-ttu-id="222c4-127">Om du vill autentisera med Azure AD, kan du använda den här slutpunkten tillsammans med klient-ID (katalog-ID).</span><span class="sxs-lookup"><span data-stu-id="222c4-127">To authenticate with Azure AD, you use this endpoint together with the tenant ID (directory ID).</span></span> <span data-ttu-id="222c4-128">Klient-ID identifierar Azure AD-klienten ska användas för autentisering.</span><span class="sxs-lookup"><span data-stu-id="222c4-128">The tenant ID identifies the Azure AD tenant to use for authentication.</span></span> <span data-ttu-id="222c4-129">Om du vill hämta klient-ID, Följ stegen som beskrivs i [hämta klient-ID för Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span><span class="sxs-lookup"><span data-stu-id="222c4-129">To retrieve the tenant ID, follow the steps outlined in [Get the tenant ID for your Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span></span>

`https://login.microsoftonline.com/<tenant-id>`

> [!NOTE] 
> <span data-ttu-id="222c4-130">Klient-specifika slutpunkten krävs när du autentiserar med hjälp av ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="222c4-130">The tenant-specific endpoint is required when you authenticate using a service principal.</span></span> 
> 
> <span data-ttu-id="222c4-131">Klient-specifika slutpunkten är valfritt när du autentiserar med integrerad autentisering, men rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="222c4-131">The tenant-specific endpoint is optional when you authenticate using integrated authentication, but recommended.</span></span> <span data-ttu-id="222c4-132">Du kan också använda den vanliga Azure AD-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="222c4-132">However, you can also use the Azure AD common endpoint.</span></span> <span data-ttu-id="222c4-133">Vanliga slutpunkten ger en allmän referens samla in gränssnittet när en viss klient inte har angetts.</span><span class="sxs-lookup"><span data-stu-id="222c4-133">The common endpoint provides a generic credential gathering interface when a specific tenant is not provided.</span></span> <span data-ttu-id="222c4-134">Vanliga slutpunkten är `https://login.microsoftonline.com/common`.</span><span class="sxs-lookup"><span data-stu-id="222c4-134">The common endpoint is `https://login.microsoftonline.com/common`.</span></span>
>
>

<span data-ttu-id="222c4-135">Läs mer om Azure AD-slutpunkter [Autentiseringsscenarier för Azure AD][aad_auth_scenarios].</span><span class="sxs-lookup"><span data-stu-id="222c4-135">For more information about Azure AD endpoints, see [Authentication Scenarios for Azure AD][aad_auth_scenarios].</span></span>

### <a name="batch-resource-endpoint"></a><span data-ttu-id="222c4-136">Slutpunkten för batch-resurs</span><span class="sxs-lookup"><span data-stu-id="222c4-136">Batch resource endpoint</span></span>

<span data-ttu-id="222c4-137">Använd den **Azure Batch resurs endpoint** att hämta en token för att autentisera förfrågningar till Batch-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="222c4-137">Use the **Azure Batch resource endpoint** to acquire a token for authenticating requests to the Batch service:</span></span>

`https://batch.core.windows.net/`

## <a name="register-your-application-with-a-tenant"></a><span data-ttu-id="222c4-138">Registrera ditt program till en klient</span><span class="sxs-lookup"><span data-stu-id="222c4-138">Register your application with a tenant</span></span>

<span data-ttu-id="222c4-139">Det första steget i att använda Azure AD för autentisering är registrera ditt program i en Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="222c4-139">The first step in using Azure AD to authenticate is registering your application in an Azure AD tenant.</span></span> <span data-ttu-id="222c4-140">Registrera ditt program kan du anropa Azure [Active Directory Authentication Library] [ aad_adal] (ADAL) från din kod.</span><span class="sxs-lookup"><span data-stu-id="222c4-140">Registering your application enables you to call the Azure [Active Directory Authentication Library][aad_adal] (ADAL) from your code.</span></span> <span data-ttu-id="222c4-141">ADAL tillhandahåller ett API för att autentisera med Azure AD från ditt program.</span><span class="sxs-lookup"><span data-stu-id="222c4-141">The ADAL provides an API for authenticating with Azure AD from your application.</span></span> <span data-ttu-id="222c4-142">Registrera ditt program krävs om du planerar att använda integrerad autentisering eller ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="222c4-142">Registering your application is required whether you plan to use integrated authentication or a service principal.</span></span>

<span data-ttu-id="222c4-143">När du registrerar ditt program kan ange du information om ditt program till Azure AD.</span><span class="sxs-lookup"><span data-stu-id="222c4-143">When you register your application, you supply information about your application to Azure AD.</span></span> <span data-ttu-id="222c4-144">Azure AD sedan innehåller ett program-ID som används för att associera ditt program med Azure AD under körning.</span><span class="sxs-lookup"><span data-stu-id="222c4-144">Azure AD then provides an application ID that you use to associate your application with Azure AD at runtime.</span></span> <span data-ttu-id="222c4-145">Mer information om program-ID finns [program och tjänstens huvudnamn objekt i Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="222c4-145">To learn more about the application ID, see [Application and service principal objects in Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span></span>

<span data-ttu-id="222c4-146">Om du vill registrera din Batch-program, följer du stegen i den [lägga till ett program](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) i avsnittet [integrera program med Azure Active Directory][aad_integrate].</span><span class="sxs-lookup"><span data-stu-id="222c4-146">To register your Batch application, follow the steps in the [Adding an Application](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) section in [Integrating applications with Azure Active Directory][aad_integrate].</span></span> <span data-ttu-id="222c4-147">Om du registrerar ditt program som det ursprungliga programmet kan du ange en giltig URI för den **omdirigerings-URI**.</span><span class="sxs-lookup"><span data-stu-id="222c4-147">If you register your application as a Native Application, you can specify any valid URI for the **Redirect URI**.</span></span> <span data-ttu-id="222c4-148">Det behöver inte vara en verklig slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="222c4-148">It does not need to be a real endpoint.</span></span>

<span data-ttu-id="222c4-149">När du har registrerat ditt program, ser du det program-ID:</span><span class="sxs-lookup"><span data-stu-id="222c4-149">After you've registered your application, you'll see the application ID:</span></span>

![Registrera ditt Batch-program med Azure AD](./media/batch-aad-auth/app-registration-data-plane.png)

<span data-ttu-id="222c4-151">Mer information om hur du registrerar ett program med Azure AD finns [Autentiseringsscenarier för Azure AD](../active-directory/develop/active-directory-authentication-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="222c4-151">For more information about registering an application with Azure AD, see [Authentication Scenarios for Azure AD](../active-directory/develop/active-directory-authentication-scenarios.md).</span></span>

## <a name="get-the-tenant-id-for-your-active-directory"></a><span data-ttu-id="222c4-152">Hämta klient-ID för ditt Active Directory</span><span class="sxs-lookup"><span data-stu-id="222c4-152">Get the tenant ID for your Active Directory</span></span>

<span data-ttu-id="222c4-153">Klient-ID identifierar Azure AD-klient som tillhandahåller autentiseringstjänster för ditt program.</span><span class="sxs-lookup"><span data-stu-id="222c4-153">The tenant ID identifies the Azure AD tenant that provides authentication services to your application.</span></span> <span data-ttu-id="222c4-154">Följ dessa steg för att hämta klient-ID:</span><span class="sxs-lookup"><span data-stu-id="222c4-154">To get the tenant ID, follow these steps:</span></span>

1. <span data-ttu-id="222c4-155">Välj din Active Directory i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="222c4-155">In the Azure portal, select your Active Directory.</span></span>
2. <span data-ttu-id="222c4-156">Klicka på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="222c4-156">Click **Properties**.</span></span>
3. <span data-ttu-id="222c4-157">Kopiera GUID-värde som angetts för directory-ID.</span><span class="sxs-lookup"><span data-stu-id="222c4-157">Copy the GUID value provided for the directory ID.</span></span> <span data-ttu-id="222c4-158">Det här värdet kallas även för klient-ID.</span><span class="sxs-lookup"><span data-stu-id="222c4-158">This value is also called the tenant ID.</span></span>

![Kopiera katalog-ID](./media/batch-aad-auth/aad-directory-id.png)


## <a name="use-integrated-authentication"></a><span data-ttu-id="222c4-160">Använda integrerad autentisering</span><span class="sxs-lookup"><span data-stu-id="222c4-160">Use integrated authentication</span></span>

<span data-ttu-id="222c4-161">Du måste ge ditt program behörighet att ansluta till API för Batch-tjänsten för att autentisera med integrerad autentisering.</span><span class="sxs-lookup"><span data-stu-id="222c4-161">To authenticate with integrated authentication, you need to grant your application permissions to connect to the Batch service API.</span></span> <span data-ttu-id="222c4-162">Det här steget gör att programmet kan autentisera anrop till API för Batch-tjänsten med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="222c4-162">This step enables your application to authenticate calls to the Batch service API with Azure AD.</span></span>

<span data-ttu-id="222c4-163">När du har [registrerade programmet](#register-your-application-with-an-azure-ad-tenant), Följ dessa steg i Azure portal för att bevilja åtkomst till Batch-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="222c4-163">Once you've [registered your application](#register-your-application-with-an-azure-ad-tenant), follow these steps in the Azure portal to grant it access to the Batch service:</span></span>

1. <span data-ttu-id="222c4-164">I det vänstra navigeringsfönstret i Azure portal väljer **fler tjänster**, klickar du på **App registreringar**.</span><span class="sxs-lookup"><span data-stu-id="222c4-164">In the left-hand navigation pane of the Azure portal, choose **More Services**, click **App Registrations**.</span></span>
2. <span data-ttu-id="222c4-165">Sök efter namnet på programmet i listan över app registreringar:</span><span class="sxs-lookup"><span data-stu-id="222c4-165">Search for the name of your application in the list of app registrations:</span></span>

    ![Sök efter programnamnet](./media/batch-aad-auth/search-app-registration.png)

3. <span data-ttu-id="222c4-167">Öppna den **inställningar** bladet för ditt program.</span><span class="sxs-lookup"><span data-stu-id="222c4-167">Open the **Settings** blade for your application.</span></span> <span data-ttu-id="222c4-168">I den **API-åtkomst** väljer **nödvändiga behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="222c4-168">In the **API Access** section, select **Required permissions**.</span></span>
4. <span data-ttu-id="222c4-169">I den **nödvändiga behörigheter** bladet, klickar du på den **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="222c4-169">In the **Required permissions** blade, click the **Add** button.</span></span>
5. <span data-ttu-id="222c4-170">I steg 1 ska söka efter Batch-API.</span><span class="sxs-lookup"><span data-stu-id="222c4-170">In step 1, search for the Batch API.</span></span> <span data-ttu-id="222c4-171">Sök efter var och en av de här strängarna tills du hittar API:t:</span><span class="sxs-lookup"><span data-stu-id="222c4-171">Search for each of these strings until you find the API:</span></span>
    1. <span data-ttu-id="222c4-172">**MicrosoftAzureBatch**.</span><span class="sxs-lookup"><span data-stu-id="222c4-172">**MicrosoftAzureBatch**.</span></span>
    2. <span data-ttu-id="222c4-173">**Microsoft Azure Batch**.</span><span class="sxs-lookup"><span data-stu-id="222c4-173">**Microsoft Azure Batch**.</span></span> <span data-ttu-id="222c4-174">Nyare Azure AD-klientorganisationer kan använda det här namnet.</span><span class="sxs-lookup"><span data-stu-id="222c4-174">Newer Azure AD tenants may use this name.</span></span>
    3. <span data-ttu-id="222c4-175">**ddbf3205-c6bd-46ae-8127-60eb93363864** är id:t för API:t.</span><span class="sxs-lookup"><span data-stu-id="222c4-175">**ddbf3205-c6bd-46ae-8127-60eb93363864** is the ID for the Batch API.</span></span> 
6. <span data-ttu-id="222c4-176">När du har hittat Batch-API, markerar du den och klicka på den **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="222c4-176">Once you find the Batch API, select it and click the **Select** button.</span></span>
6. <span data-ttu-id="222c4-177">I steg 2, markerar du kryssrutan bredvid **Azure Batch-tjänsten för dataåtkomst** och klicka på den **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="222c4-177">In step 2, select the check box next to **Access Azure Batch Service** and click the **Select** button.</span></span>
7. <span data-ttu-id="222c4-178">Klicka på den **klar** knappen.</span><span class="sxs-lookup"><span data-stu-id="222c4-178">Click the **Done** button.</span></span>

<span data-ttu-id="222c4-179">Den **nödvändiga behörigheter** bladet nu visar att din Azure AD-program har åtkomst till både ADAL och batchen service API.</span><span class="sxs-lookup"><span data-stu-id="222c4-179">The **Required Permissions** blade now shows that your Azure AD application has access to both ADAL and the Batch service API.</span></span> <span data-ttu-id="222c4-180">Behörigheter som ADAL automatiskt när du först registrera din app med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="222c4-180">Permissions are granted to ADAL automatically when you first register your app with Azure AD.</span></span>

![Bevilja API-behörigheter](./media/batch-aad-auth/required-permissions-data-plane.png)

## <a name="use-a-service-principal"></a><span data-ttu-id="222c4-182">Använd ett huvudnamn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="222c4-182">Use a service principal</span></span> 

<span data-ttu-id="222c4-183">Om du vill autentisera ett program som körs obevakad, kan du använda ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="222c4-183">To authenticate an application that runs unattended, you use a service principal.</span></span> <span data-ttu-id="222c4-184">När du har registrerat ditt program, Följ dessa steg i Azure portal för att konfigurera ett huvudnamn för tjänsten:</span><span class="sxs-lookup"><span data-stu-id="222c4-184">After you've registered your application, follow these steps in the Azure portal to configure a service principal:</span></span>

1. <span data-ttu-id="222c4-185">Begär en hemlig nyckel för ditt program.</span><span class="sxs-lookup"><span data-stu-id="222c4-185">Request a secret key for your application.</span></span>
2. <span data-ttu-id="222c4-186">Tilldela en RBAC-roll i tillämpningsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="222c4-186">Assign an RBAC role to your application.</span></span>

### <a name="request-a-secret-key-for-your-application"></a><span data-ttu-id="222c4-187">Begär en hemlig nyckel för ditt program</span><span class="sxs-lookup"><span data-stu-id="222c4-187">Request a secret key for your application</span></span>

<span data-ttu-id="222c4-188">När programmet autentiseras med en tjänst som skickar både program-ID och en hemlig nyckel till Azure AD.</span><span class="sxs-lookup"><span data-stu-id="222c4-188">When your application authenticates with a service principal, it sends both the application ID and a secret key to Azure AD.</span></span> <span data-ttu-id="222c4-189">Du måste skapa och kopiera den hemliga nyckeln ska användas från din kod.</span><span class="sxs-lookup"><span data-stu-id="222c4-189">You'll need to create and copy the secret key to use from your code.</span></span>

<span data-ttu-id="222c4-190">Följ dessa steg i Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="222c4-190">Follow these steps in the Azure portal:</span></span>

1. <span data-ttu-id="222c4-191">I det vänstra navigeringsfönstret i Azure portal väljer **fler tjänster**, klickar du på **App registreringar**.</span><span class="sxs-lookup"><span data-stu-id="222c4-191">In the left-hand navigation pane of the Azure portal, choose **More Services**, click **App Registrations**.</span></span>
2. <span data-ttu-id="222c4-192">Sök efter namnet på programmet i listan över app registreringar.</span><span class="sxs-lookup"><span data-stu-id="222c4-192">Search for the name of your application in the list of app registrations.</span></span>
3. <span data-ttu-id="222c4-193">Visa den **inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="222c4-193">Display the **Settings** blade.</span></span> <span data-ttu-id="222c4-194">I den **API-åtkomst** väljer **nycklar**.</span><span class="sxs-lookup"><span data-stu-id="222c4-194">In the **API Access** section, select **Keys**.</span></span>
4. <span data-ttu-id="222c4-195">Ange en beskrivning av nyckeln för att skapa en nyckel.</span><span class="sxs-lookup"><span data-stu-id="222c4-195">To create a key, enter a description for the key.</span></span> <span data-ttu-id="222c4-196">Välj sedan en varaktighet för nyckeln för en eller två år.</span><span class="sxs-lookup"><span data-stu-id="222c4-196">Then select a duration for the key of either one or two years.</span></span> 
5. <span data-ttu-id="222c4-197">Klicka på den **spara** för att skapa och visa nyckeln.</span><span class="sxs-lookup"><span data-stu-id="222c4-197">Click the **Save** button to create and display the key.</span></span> <span data-ttu-id="222c4-198">Kopiera värdet för nyckeln till en säker plats som du inte åtkomst till den igen när du lämnar bladet.</span><span class="sxs-lookup"><span data-stu-id="222c4-198">Copy the key value to a safe place, as you won't be able to access it again after you leave the blade.</span></span> 

    ![Skapa en hemlig nyckel](./media/batch-aad-auth/secret-key.png)

### <a name="assign-an-rbac-role-to-your-application"></a><span data-ttu-id="222c4-200">Tilldela en RBAC roll till ditt program</span><span class="sxs-lookup"><span data-stu-id="222c4-200">Assign an RBAC role to your application</span></span>

<span data-ttu-id="222c4-201">För att autentisera med en tjänstens huvudnamn, måste du tilldela en RBAC-roll i tillämpningsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="222c4-201">To authenticate with a service principal, you need to assign an RBAC role to your application.</span></span> <span data-ttu-id="222c4-202">Följ de här stegen:</span><span class="sxs-lookup"><span data-stu-id="222c4-202">Follow these steps:</span></span>

1. <span data-ttu-id="222c4-203">Navigera till Batch-kontot som används av ditt program i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="222c4-203">In the Azure portal, navigate to the Batch account used by your application.</span></span>
2. <span data-ttu-id="222c4-204">I den **inställningar** bladet för Batch-kontot väljer **Access Control (IAM)**.</span><span class="sxs-lookup"><span data-stu-id="222c4-204">In the **Settings** blade for the Batch account, select **Access Control (IAM)**.</span></span>
3. <span data-ttu-id="222c4-205">Klicka på den **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="222c4-205">Click the **Add** button.</span></span> 
4. <span data-ttu-id="222c4-206">Från den **rollen** listrutan och välj någon av _deltagare_ eller _Reader_ roll för ditt program.</span><span class="sxs-lookup"><span data-stu-id="222c4-206">From the **Role** drop-down, choose either the _Contributor_ or _Reader_ role for your application.</span></span> <span data-ttu-id="222c4-207">Mer information om dessa roller finns [Kom igång med rollbaserad åtkomstkontroll i Azure portal](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="222c4-207">For more information on these roles, see [Get started with Role-Based Access Control in the Azure portal](../active-directory/role-based-access-control-what-is.md).</span></span>  
5. <span data-ttu-id="222c4-208">I den **Välj** anger du namnet på ditt program.</span><span class="sxs-lookup"><span data-stu-id="222c4-208">In the **Select** field, enter the name of your application.</span></span> <span data-ttu-id="222c4-209">Markera programmet i listan och klickar på **spara**.</span><span class="sxs-lookup"><span data-stu-id="222c4-209">Select your application from the list, and click **Save**.</span></span>

<span data-ttu-id="222c4-210">Ditt program bör nu visas i dina inställningar för åtkomstkontroll med en RBAC-roll som tilldelats.</span><span class="sxs-lookup"><span data-stu-id="222c4-210">Your application should now appear in your access control settings with an RBAC role assigned.</span></span> 

![Tilldela en RBAC roll till ditt program](./media/batch-aad-auth/app-rbac-role.png)

### <a name="get-the-tenant-id-for-your-azure-active-directory"></a><span data-ttu-id="222c4-212">Hämta klient-ID för Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="222c4-212">Get the tenant ID for your Azure Active Directory</span></span>

<span data-ttu-id="222c4-213">Klient-ID identifierar Azure AD-klient som tillhandahåller autentiseringstjänster för ditt program.</span><span class="sxs-lookup"><span data-stu-id="222c4-213">The tenant ID identifies the Azure AD tenant that provides authentication services to your application.</span></span> <span data-ttu-id="222c4-214">Följ dessa steg för att hämta klient-ID:</span><span class="sxs-lookup"><span data-stu-id="222c4-214">To get the tenant ID, follow these steps:</span></span>

1. <span data-ttu-id="222c4-215">Välj din Active Directory i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="222c4-215">In the Azure portal, select your Active Directory.</span></span>
2. <span data-ttu-id="222c4-216">Klicka på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="222c4-216">Click **Properties**.</span></span>
3. <span data-ttu-id="222c4-217">Kopiera GUID-värde som angetts för directory-ID.</span><span class="sxs-lookup"><span data-stu-id="222c4-217">Copy the GUID value provided for the directory ID.</span></span> <span data-ttu-id="222c4-218">Det här värdet kallas även för klient-ID.</span><span class="sxs-lookup"><span data-stu-id="222c4-218">This value is also called the tenant ID.</span></span>

![Kopiera katalog-ID](./media/batch-aad-auth/aad-directory-id.png)


## <a name="code-examples"></a><span data-ttu-id="222c4-220">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="222c4-220">Code examples</span></span>

<span data-ttu-id="222c4-221">Kodexemplen i det här avsnittet visar hur du autentisera med Azure AD med hjälp av integrerad autentisering och ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="222c4-221">The code examples in this section show how to authenticate with Azure AD using integrated authentication and with a service principal.</span></span> <span data-ttu-id="222c4-222">Dessa kodexempel använda .NET, men begrepp som är liknande för andra språk.</span><span class="sxs-lookup"><span data-stu-id="222c4-222">These code examples use .NET, but the concepts are similar for other languages.</span></span>

> [!NOTE]
> <span data-ttu-id="222c4-223">En Azure AD authentication token upphör att gälla efter en timme.</span><span class="sxs-lookup"><span data-stu-id="222c4-223">An Azure AD authentication token expires after one hour.</span></span> <span data-ttu-id="222c4-224">När du använder en långlivade **BatchClient** objekt, rekommenderar vi att du hämtar en token från ADAL för varje begäran att säkerställa att du alltid har en giltig token.</span><span class="sxs-lookup"><span data-stu-id="222c4-224">When using a long-lived **BatchClient** object, we recommend that you retrieve a token from ADAL on every request to ensure you always have a valid token.</span></span> 
>
>
> <span data-ttu-id="222c4-225">För att uppnå i .NET skriva en metod som hämtar en token från Azure AD och överför denna metod för att en **BatchTokenCredentials** objektet som ett ombud.</span><span class="sxs-lookup"><span data-stu-id="222c4-225">To achieve this in .NET, write a method that retrieves the token from Azure AD and pass that method to a **BatchTokenCredentials** object as a delegate.</span></span> <span data-ttu-id="222c4-226">Delegate-metoden anropas för varje begäran att Batch-tjänsten för att säkerställa att en giltig token.</span><span class="sxs-lookup"><span data-stu-id="222c4-226">The delegate method is called on every request to the Batch service to ensure that a valid token is provided.</span></span> <span data-ttu-id="222c4-227">Som standard cachelagrar ADAL-token så att en ny token hämtas från Azure AD bara när det behövs.</span><span class="sxs-lookup"><span data-stu-id="222c4-227">By default ADAL caches tokens, so a new token is retrieved from Azure AD only when necessary.</span></span> <span data-ttu-id="222c4-228">Mer information om token i Azure AD finns [Autentiseringsscenarier för Azure AD][aad_auth_scenarios].</span><span class="sxs-lookup"><span data-stu-id="222c4-228">For more information about tokens in Azure AD, see [Authentication Scenarios for Azure AD][aad_auth_scenarios].</span></span>
>
>

### <a name="code-example-using-azure-ad-integrated-authentication-with-batch-net"></a><span data-ttu-id="222c4-229">Exempel: använda Azure AD-integrerad autentisering med Batch .NET</span><span class="sxs-lookup"><span data-stu-id="222c4-229">Code example: Using Azure AD integrated authentication with Batch .NET</span></span>

<span data-ttu-id="222c4-230">Om du vill autentisera med integrerad autentisering från Batch .NET, referera till den [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) paketet och [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) paketet.</span><span class="sxs-lookup"><span data-stu-id="222c4-230">To authenticate with integrated authentication from Batch .NET, reference the [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) package and the [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) package.</span></span>

<span data-ttu-id="222c4-231">Inkludera följande `using` instruktioner i koden:</span><span class="sxs-lookup"><span data-stu-id="222c4-231">Include the following `using` statements in your code:</span></span>

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

<span data-ttu-id="222c4-232">Referens för Azure AD-slutpunkt i din kod, inklusive klient-ID.</span><span class="sxs-lookup"><span data-stu-id="222c4-232">Reference the Azure AD endpoint in your code, including the tenant ID.</span></span> <span data-ttu-id="222c4-233">Om du vill hämta klient-ID, Följ stegen som beskrivs i [hämta klient-ID för Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span><span class="sxs-lookup"><span data-stu-id="222c4-233">To retrieve the tenant ID, follow the steps outlined in [Get the tenant ID for your Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span></span>

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

<span data-ttu-id="222c4-234">Resursen tjänstslutpunkten Batch-referens:</span><span class="sxs-lookup"><span data-stu-id="222c4-234">Reference the Batch service resource endpoint:</span></span>

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

<span data-ttu-id="222c4-235">Referera till Batch-kontot:</span><span class="sxs-lookup"><span data-stu-id="222c4-235">Reference your Batch account:</span></span>

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

<span data-ttu-id="222c4-236">Ange program-ID (klient-ID) för ditt program.</span><span class="sxs-lookup"><span data-stu-id="222c4-236">Specify the application ID (client ID) for your application.</span></span> <span data-ttu-id="222c4-237">Program-ID är tillgänglig från din appregistrering i Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="222c4-237">The application ID is available from your app registration in the Azure portal:</span></span>

```csharp
private const string ClientId = "<application-id>";
```

<span data-ttu-id="222c4-238">Också kopiera omdirigerings-URI som du angav under registreringen.</span><span class="sxs-lookup"><span data-stu-id="222c4-238">Also copy the redirect URI that you specified during the registration process.</span></span> <span data-ttu-id="222c4-239">Omdirigerings-URI som angetts i din kod måste matcha omdirigerings-URI som du angav när du registrerade programmet:</span><span class="sxs-lookup"><span data-stu-id="222c4-239">The redirect URI specified in your code must match the redirect URI that you provided when you registered the application:</span></span>

```csharp
private const string RedirectUri = "http://mybatchdatasample";
```

<span data-ttu-id="222c4-240">Skriva en metod för att hämta autentiseringstoken från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="222c4-240">Write a callback method to acquire the authentication token from Azure AD.</span></span> <span data-ttu-id="222c4-241">Den **GetAuthenticationTokenAsync** Återanropsmetoden som visas här anrop ADAL att autentisera en användare som interagerar med programmet.</span><span class="sxs-lookup"><span data-stu-id="222c4-241">The **GetAuthenticationTokenAsync** callback method shown here calls ADAL to authenticate a user who is interacting with the application.</span></span> <span data-ttu-id="222c4-242">Den **AcquireTokenAsync** metod som tillhandahålls av ADAL efterfrågar sina autentiseringsuppgifter och programmet fortsätter när användaren anger dem (om det redan har cachelagrade autentiseringsuppgifter):</span><span class="sxs-lookup"><span data-stu-id="222c4-242">The **AcquireTokenAsync** method provided by ADAL prompts the user for their credentials, and the application proceeds once the user provides them (unless it has already cached credentials):</span></span>

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    var authContext = new AuthenticationContext(AuthorityUri);

    // Acquire the authentication token from Azure AD.
    var authResult = await authContext.AcquireTokenAsync(BatchResourceUri, 
                                                        ClientId, 
                                                        new Uri(RedirectUri), 
                                                        new PlatformParameters(PromptBehavior.Auto));

    return authResult.AccessToken;
}
```

<span data-ttu-id="222c4-243">Skapa en **BatchTokenCredentials** objekt som tar ombudet som en parameter.</span><span class="sxs-lookup"><span data-stu-id="222c4-243">Construct a **BatchTokenCredentials** object that takes the delegate as a parameter.</span></span> <span data-ttu-id="222c4-244">Använd dessa autentiseringsuppgifter för att öppna en **BatchClient** objekt.</span><span class="sxs-lookup"><span data-stu-id="222c4-244">Use those credentials to open a **BatchClient** object.</span></span> <span data-ttu-id="222c4-245">Du kan använda som **BatchClient** objekt för efterföljande åtgärder mot Batch-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="222c4-245">You can use that **BatchClient** object for subsequent operations against the Batch service:</span></span>

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

### <a name="code-example-using-an-azure-ad-service-principal-with-batch-net"></a><span data-ttu-id="222c4-246">Exempel: med hjälp av en Azure AD-tjänstens huvudnamn med Batch .NET</span><span class="sxs-lookup"><span data-stu-id="222c4-246">Code example: Using an Azure AD service principal with Batch .NET</span></span>

<span data-ttu-id="222c4-247">Om du vill autentisera med ett huvudnamn för tjänsten från Batch .NET, referera till den [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) paketet och [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) paketet.</span><span class="sxs-lookup"><span data-stu-id="222c4-247">To authenticate with a service principal from Batch .NET, reference the [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) package and the [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) package.</span></span>

<span data-ttu-id="222c4-248">Inkludera följande `using` instruktioner i koden:</span><span class="sxs-lookup"><span data-stu-id="222c4-248">Include the following `using` statements in your code:</span></span>

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

<span data-ttu-id="222c4-249">Referens för Azure AD-slutpunkt i din kod, inklusive klient-ID.</span><span class="sxs-lookup"><span data-stu-id="222c4-249">Reference the Azure AD endpoint in your code, including the tenant ID.</span></span> <span data-ttu-id="222c4-250">Du måste ange en slutpunkt för klienten när du använder ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="222c4-250">When using a service principal, you must provide a tenant-specific endpoint.</span></span> <span data-ttu-id="222c4-251">Om du vill hämta klient-ID, Följ stegen som beskrivs i [hämta klient-ID för Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span><span class="sxs-lookup"><span data-stu-id="222c4-251">To retrieve the tenant ID, follow the steps outlined in [Get the tenant ID for your Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span></span>

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

<span data-ttu-id="222c4-252">Resursen tjänstslutpunkten Batch-referens:</span><span class="sxs-lookup"><span data-stu-id="222c4-252">Reference the Batch service resource endpoint:</span></span>  

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

<span data-ttu-id="222c4-253">Referera till Batch-kontot:</span><span class="sxs-lookup"><span data-stu-id="222c4-253">Reference your Batch account:</span></span>

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

<span data-ttu-id="222c4-254">Ange program-ID (klient-ID) för ditt program.</span><span class="sxs-lookup"><span data-stu-id="222c4-254">Specify the application ID (client ID) for your application.</span></span> <span data-ttu-id="222c4-255">Program-ID är tillgänglig från din appregistrering i Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="222c4-255">The application ID is available from your app registration in the Azure portal:</span></span>

```csharp
private const string ClientId = "<application-id>";
```

<span data-ttu-id="222c4-256">Ange den hemliga nyckeln som du kopierade från Azure portal:</span><span class="sxs-lookup"><span data-stu-id="222c4-256">Specify the secret key that you copied from the Azure portal:</span></span>

```csharp
private const string ClientKey = "<secret-key>";
```

<span data-ttu-id="222c4-257">Skriva en metod för att hämta autentiseringstoken från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="222c4-257">Write a callback method to acquire the authentication token from Azure AD.</span></span> <span data-ttu-id="222c4-258">Den **GetAuthenticationTokenAsync** Återanropsmetoden som visas här anrop ADAL för obevakad autentisering:</span><span class="sxs-lookup"><span data-stu-id="222c4-258">The **GetAuthenticationTokenAsync** callback method shown here calls ADAL for unattended authentication:</span></span>

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
    AuthenticationResult authResult = await authContext.AcquireTokenAsync(BatchResourceUri, new ClientCredential(ClientId, ClientKey));

    return authResult.AccessToken;
}
```

<span data-ttu-id="222c4-259">Skapa en **BatchTokenCredentials** objekt som tar ombudet som en parameter.</span><span class="sxs-lookup"><span data-stu-id="222c4-259">Construct a **BatchTokenCredentials** object that takes the delegate as a parameter.</span></span> <span data-ttu-id="222c4-260">Använd dessa autentiseringsuppgifter för att öppna en **BatchClient** objekt.</span><span class="sxs-lookup"><span data-stu-id="222c4-260">Use those credentials to open a **BatchClient** object.</span></span> <span data-ttu-id="222c4-261">Du kan sedan använda **BatchClient** objekt för efterföljande åtgärder mot Batch-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="222c4-261">You can then use that **BatchClient** object for subsequent operations against the Batch service:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="222c4-262">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="222c4-262">Next steps</span></span>

<span data-ttu-id="222c4-263">Mer information om Azure AD finns i [Azure Active Directory-dokumentationen](https://docs.microsoft.com/azure/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="222c4-263">To learn more about Azure AD, see the [Azure Active Directory Documentation](https://docs.microsoft.com/azure/active-directory/).</span></span> <span data-ttu-id="222c4-264">Djupgående exempel som visar hur du använder ADAL finns i den [Azure kodexempel](https://azure.microsoft.com/resources/samples/?service=active-directory) bibliotek.</span><span class="sxs-lookup"><span data-stu-id="222c4-264">In-depth examples showing how to use ADAL are available in the [Azure Code Samples](https://azure.microsoft.com/resources/samples/?service=active-directory) library.</span></span>

<span data-ttu-id="222c4-265">Läs mer om tjänstens huvudnamn i [program och tjänstens huvudnamn objekt i Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="222c4-265">To learn more about service principals, see [Application and service principal objects in Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span></span> <span data-ttu-id="222c4-266">Om du vill skapa ett huvudnamn för tjänsten med hjälp av Azure portal finns [använda portalen för att skapa Active Directory applikationen eller tjänsten säkerhetsobjekt som kan komma åt resurser](../resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="222c4-266">To create a service principal using the Azure portal, see [Use portal to create Active Directory application and service principal that can access resources](../resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="222c4-267">Du kan också skapa ett huvudnamn för tjänsten med PowerShell eller Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="222c4-267">You can also create a service principal with PowerShell or Azure CLI.</span></span> 

<span data-ttu-id="222c4-268">För att autentisera Batch-hantering av program med Azure AD finns [autentiserar Batch hanteringslösningar med Active Directory](batch-aad-auth-management.md).</span><span class="sxs-lookup"><span data-stu-id="222c4-268">To authenticate Batch Management applications using Azure AD, see [Authenticate Batch Management solutions with Active Directory](batch-aad-auth-management.md).</span></span> 

<span data-ttu-id="222c4-269">[aad_about]: ../active-directory/active-directory-whatis.md "Vad är Azure Active Directory?"</span><span class="sxs-lookup"><span data-stu-id="222c4-269">[aad_about]: ../active-directory/active-directory-whatis.md "What is Azure Active Directory?"</span></span>
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
<span data-ttu-id="222c4-270">[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Autentiseringsscenarier för Azure AD"</span><span class="sxs-lookup"><span data-stu-id="222c4-270">[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Authentication Scenarios for Azure AD"</span></span>
<span data-ttu-id="222c4-271">[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrera program med Azure Active Directory"</span><span class="sxs-lookup"><span data-stu-id="222c4-271">[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrating Applications with Azure Active Directory"</span></span>
[azure_portal]: http://portal.azure.com
