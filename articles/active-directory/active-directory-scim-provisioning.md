---
title: "Med hjälp av System för Identitetshantering domäner automatiskt etablera användare och grupper från Azure Active Directory till program | Microsoft Docs"
description: "Azure Active Directory kan automatiskt etablera användare och grupper till några program eller identitet butik som är fronted av en webbtjänst med det gränssnitt som definierats i specifikationen av SCIM-protokollet"
services: active-directory
documentationcenter: 
author: asmalser-msft
manager: femila
editor: 
ms.assetid: 4d86f3dc-e2d3-4bde-81a3-4a0e092551c0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2017
ms.author: asmalser
ms.reviewer: asmalser
ms.custom: aaddev;it-pro;oldportal
ms.openlocfilehash: 91978cee88d55c99bcb63c63cdaf01581ae84668
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="using-system-for-cross-domain-identity-management-to-automatically-provision-users-and-groups-from-azure-active-directory-to-applications"></a><span data-ttu-id="e49e4-103">Med hjälp av System för Identitetshantering i domänerna att automatiskt etablera användare och grupper från Azure Active Directory till program</span><span class="sxs-lookup"><span data-stu-id="e49e4-103">Using System for Cross-Domain Identity Management to automatically provision users and groups from Azure Active Directory to applications</span></span>

## <a name="overview"></a><span data-ttu-id="e49e4-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="e49e4-104">Overview</span></span>
<span data-ttu-id="e49e4-105">Azure Active Directory (AD Azure) automatiskt kan etablera användare och grupper till några program eller identitet store som fronted av en webbtjänst med gränssnittet definieras i den [System för domäner Identity Management (SCIM) 2.0 protokollspecifikation](https://tools.ietf.org/html/draft-ietf-scim-api-19).</span><span class="sxs-lookup"><span data-stu-id="e49e4-105">Azure Active Directory (Azure AD) can automatically provision users and groups to any application or identity store that is fronted by a web service with the interface defined in the [System for Cross-Domain Identity Management (SCIM) 2.0 protocol specification](https://tools.ietf.org/html/draft-ietf-scim-api-19).</span></span> <span data-ttu-id="e49e4-106">Azure Active Directory kan skicka begäranden om att skapa, ändra eller ta bort tilldelade användare och grupper till webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="e49e4-106">Azure Active Directory can send requests to create, modify, or delete assigned users and groups to the web service.</span></span> <span data-ttu-id="e49e4-107">Webbtjänsten kan översätta dessa begäranden till åtgärder i Identitetslagret mål.</span><span class="sxs-lookup"><span data-stu-id="e49e4-107">The web service can then translate those requests into operations on the target identity store.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="e49e4-108">Microsoft rekommenderar att du hanterar Azure AD via [Azure AD administratörscenter](https://aad.portal.azure.com) på Azure Portal istället för via den klassiska Azure-portalen som nämns i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="e49e4-108">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span> 



<span data-ttu-id="e49e4-109">![][0]
*Bild 1: Etablering från Azure Active Directory till en butik identitet via en webbtjänst*</span><span class="sxs-lookup"><span data-stu-id="e49e4-109">![][0]
*Figure 1: Provisioning from Azure Active Directory to an identity store via a web service*</span></span>

<span data-ttu-id="e49e4-110">Den här funktionen kan användas tillsammans med funktioner som ”ta med din egen app” i Azure AD för att aktivera enkel inloggning och automatisk användaretablering för program som tillhandahåller eller fronted av en SCIM-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="e49e4-110">This capability can be used in conjunction with the “bring your own app” capability in Azure AD to enable single sign-on and automatic user provisioning for applications that provide or are fronted by a SCIM web service.</span></span>

<span data-ttu-id="e49e4-111">Det finns två användningsfall för att använda SCIM i Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="e49e4-111">There are two use cases for using SCIM in Azure Active Directory:</span></span>

* <span data-ttu-id="e49e4-112">**Etablering av användare och grupper till program som stöder SCIM** program som stöder SCIM 2.0 och använder OAuth ägar-token för autentisering fungerar med Azure AD utan konfiguration.</span><span class="sxs-lookup"><span data-stu-id="e49e4-112">**Provisioning users and groups to applications that support SCIM** Applications that support SCIM 2.0 and use OAuth bearer tokens for authentication works with Azure AD without configuration.</span></span>
* <span data-ttu-id="e49e4-113">**Skapa din egen lösning för etablering för program som stöder andra API-baserad etablering** för icke-SCIM program kan du skapa en slutpunkt för SCIM att översätta mellan SCIM för Azure AD-slutpunkten och API: er har stöd för programmet för användaretablering.</span><span class="sxs-lookup"><span data-stu-id="e49e4-113">**Build your own provisioning solution for applications that support other API-based provisioning** For non-SCIM applications, you can create a SCIM endpoint to translate between the Azure AD SCIM endpoint and any API the application supports for user provisioning.</span></span> <span data-ttu-id="e49e4-114">Vi ger Common Language Infrastructure (CLI) bibliotek och kodexempel som visar hur du gör ger en SCIM slutpunkt och översätta SCIM meddelanden för att hjälpa dig att utveckla en SCIM slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="e49e4-114">To help you develop a SCIM endpoint, we provide Common Language Infrastructure (CLI) libraries along with code samples that show you how to do provide a SCIM endpoint and translate SCIM messages.</span></span>  

## <a name="provisioning-users-and-groups-to-applications-that-support-scim"></a><span data-ttu-id="e49e4-115">Etablering av användare och grupper till program som stöder SCIM</span><span class="sxs-lookup"><span data-stu-id="e49e4-115">Provisioning users and groups to applications that support SCIM</span></span>
<span data-ttu-id="e49e4-116">Azure AD kan konfigureras för att automatiskt etablera tilldelade användare och grupper till program som implementerar en [System för domäner Identity Management 2 (SCIM)](https://tools.ietf.org/html/draft-ietf-scim-api-19) webbtjänsten och acceptera OAuth ägar-token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="e49e4-116">Azure AD can be configured to automatically provision assigned users and groups to applications that implement a [System for Cross-domain Identity Management 2 (SCIM)](https://tools.ietf.org/html/draft-ietf-scim-api-19) web service and accept OAuth bearer tokens for authentication.</span></span> <span data-ttu-id="e49e4-117">Inom SCIM 2.0-specifikationen måste program uppfylla följande villkor:</span><span class="sxs-lookup"><span data-stu-id="e49e4-117">Within the SCIM 2.0 specification, applications must meet these requirements:</span></span>

* <span data-ttu-id="e49e4-118">Har stöd för att skapa användare och/eller grupper, enligt avsnittet 3.3 av SCIM-protokollet.</span><span class="sxs-lookup"><span data-stu-id="e49e4-118">Supports creating users and/or groups, as per section 3.3 of the SCIM protocol.</span></span>  
* <span data-ttu-id="e49e4-119">Har stöd för ändring av användare och/eller grupper med patch-förfrågningar enligt avsnittet 3.5.2 av SCIM-protokollet.</span><span class="sxs-lookup"><span data-stu-id="e49e4-119">Supports modifying users and/or groups with patch requests as per section 3.5.2 of the SCIM protocol.</span></span>  
* <span data-ttu-id="e49e4-120">Har stöd för hämtning av en känd resurs enligt avsnittet 3.4.1 av SCIM-protokollet.</span><span class="sxs-lookup"><span data-stu-id="e49e4-120">Supports retrieving a known resource as per section 3.4.1 of the SCIM protocol.</span></span>  
* <span data-ttu-id="e49e4-121">Har stöd för frågor till användare och/eller grupper, enligt avsnittet 3.4.2 av SCIM-protokollet.</span><span class="sxs-lookup"><span data-stu-id="e49e4-121">Supports querying users and/or groups, as per section 3.4.2 of the SCIM protocol.</span></span>  <span data-ttu-id="e49e4-122">Som standard tillfrågas användare av externalId och grupper är efterfrågas av displayName.</span><span class="sxs-lookup"><span data-stu-id="e49e4-122">By default, users are queried by externalId and groups are queried by displayName.</span></span>  
* <span data-ttu-id="e49e4-123">Har stöd för frågor till användaren genom ID och manager enligt punkt 3.4.2 av SCIM-protokollet.</span><span class="sxs-lookup"><span data-stu-id="e49e4-123">Supports querying user by ID and by manager as per section 3.4.2 of the SCIM protocol.</span></span>  
* <span data-ttu-id="e49e4-124">Har stöd för frågor till grupper efter ID och av medlem enligt punkt 3.4.2 av SCIM-protokollet.</span><span class="sxs-lookup"><span data-stu-id="e49e4-124">Supports querying groups by ID and by member as per section 3.4.2 of the SCIM protocol.</span></span>  
* <span data-ttu-id="e49e4-125">Accepterar OAuth ägar-token för auktorisering enligt avsnittet 2.1 av SCIM-protokollet.</span><span class="sxs-lookup"><span data-stu-id="e49e4-125">Accepts OAuth bearer tokens for authorization as per section 2.1 of the SCIM protocol.</span></span>

<span data-ttu-id="e49e4-126">Kontrollera med leverantören av program eller program leverantörens dokumentation för rapporter för kompatibilitet med dessa krav.</span><span class="sxs-lookup"><span data-stu-id="e49e4-126">Check with your application provider, or your application provider's documentation for statements of compatibility with these requirements.</span></span>

### <a name="getting-started"></a><span data-ttu-id="e49e4-127">Komma igång</span><span class="sxs-lookup"><span data-stu-id="e49e4-127">Getting started</span></span>
<span data-ttu-id="e49e4-128">Program som stöder SCIM-profilen som beskrivs i den här artikeln kan anslutas till Azure Active Directory med funktionen ”icke-galleriet program” i Azure AD application gallery.</span><span class="sxs-lookup"><span data-stu-id="e49e4-128">Applications that support the SCIM profile described in this article can be connected to Azure Active Directory using the "non-gallery application" feature in the Azure AD application gallery.</span></span> <span data-ttu-id="e49e4-129">När du är ansluten, körs Azure AD en synkroniseringsprocess var tjugonde minut där den frågar programmets SCIM slutpunkt för tilldelade användare och grupper och skapar eller ändrar dem enligt tilldelning av information.</span><span class="sxs-lookup"><span data-stu-id="e49e4-129">Once connected, Azure AD runs a synchronization process every 20 minutes where it queries the application's SCIM endpoint for assigned users and groups, and creates or modifies them according to the assignment details.</span></span>

<span data-ttu-id="e49e4-130">**Om du vill ansluta ett program stöder som SCIM:**</span><span class="sxs-lookup"><span data-stu-id="e49e4-130">**To connect an application that supports SCIM:**</span></span>

1. <span data-ttu-id="e49e4-131">Logga in på [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e49e4-131">Sign in to [the Azure portal](https://portal.azure.com).</span></span> 
2. <span data-ttu-id="e49e4-132">Bläddra till ** Azure Active Directory > företagsprogram och välj **nytt program > alla > icke-galleriet programmet**.</span><span class="sxs-lookup"><span data-stu-id="e49e4-132">Browse to **Azure Active Directory > Enterprise Applications, and select **New application > All > Non-gallery application**.</span></span>
3. <span data-ttu-id="e49e4-133">Ange ett namn för ditt program och klicka på **Lägg till** ikon för att skapa ett app-objekt.</span><span class="sxs-lookup"><span data-stu-id="e49e4-133">Enter a name for your application, and click **Add** icon to create an app object.</span></span>
    
  <span data-ttu-id="e49e4-134">![][1]
  *Bild 2: Azure AD application gallery*</span><span class="sxs-lookup"><span data-stu-id="e49e4-134">![][1]
*Figure 2: Azure AD application gallery*</span></span>
    
4. <span data-ttu-id="e49e4-135">I skärmbilden som visas väljer du den **etablering** fliken i den vänstra kolumnen.</span><span class="sxs-lookup"><span data-stu-id="e49e4-135">In the resulting screen, select the **Provisioning** tab in the left column.</span></span>
5. <span data-ttu-id="e49e4-136">I den **etablering läge** väljer du **automatisk**.</span><span class="sxs-lookup"><span data-stu-id="e49e4-136">In the **Provisioning Mode** menu, select **Automatic**.</span></span>
    
  <span data-ttu-id="e49e4-137">![][2]
  *Bild 3: Konfigurera etablering i Azure-portalen*</span><span class="sxs-lookup"><span data-stu-id="e49e4-137">![][2]
*Figure 3: Configuring provisioning in the Azure portal*</span></span>
    
6. <span data-ttu-id="e49e4-138">I den **klient URL** , ange Webbadressen till programmets SCIM slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="e49e4-138">In the **Tenant URL** field, enter the URL of the application's SCIM endpoint.</span></span> <span data-ttu-id="e49e4-139">Exempel: https://api.contoso.com/scim/v2/</span><span class="sxs-lookup"><span data-stu-id="e49e4-139">Example: https://api.contoso.com/scim/v2/</span></span>
7. <span data-ttu-id="e49e4-140">Om slutpunkten SCIM kräver en OAuth ägar-token från en utfärdare än Azure AD, kopiera nödvändiga OAuth ägar-token till den valfria **hemlighet Token** fältet.</span><span class="sxs-lookup"><span data-stu-id="e49e4-140">If the SCIM endpoint requires an OAuth bearer token from an issuer other than Azure AD, then copy the required OAuth bearer token into the optional **Secret Token** field.</span></span> <span data-ttu-id="e49e4-141">Om det här fältet är tomt, med en OAuth ägar-token som utfärdas från Azure AD med varje begäran med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e49e4-141">If this field is left blank, then Azure AD included an OAuth bearer token issued from Azure AD with each request.</span></span> <span data-ttu-id="e49e4-142">Appar som använder Azure AD som en identitetsleverantör kan verifiera den här Azure AD-utfärdade token.</span><span class="sxs-lookup"><span data-stu-id="e49e4-142">Apps that use Azure AD as an identity provider can validate this Azure AD -issued token.</span></span>
8. <span data-ttu-id="e49e4-143">Klicka på den **Testanslutningen** så försöker ansluta till slutpunkten SCIM för Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e49e4-143">Click the **Test Connection** button to have Azure Active Directory attempt to connect to the SCIM endpoint.</span></span> <span data-ttu-id="e49e4-144">Om försöker misslyckas, visas information om felet.</span><span class="sxs-lookup"><span data-stu-id="e49e4-144">If the attempts fail, error information is displayed.</span></span>  
9. <span data-ttu-id="e49e4-145">Om försök att ansluta till program-lyckad klickar **spara** spara administratörsautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="e49e4-145">If the attempts to connect to the application succeed, then click **Save** to save the admin credentials.</span></span>
10. <span data-ttu-id="e49e4-146">I den **mappningar** avsnittet finns det två valbar uppsättningar attributmappning: en för användarobjekt och en för gruppobjekt.</span><span class="sxs-lookup"><span data-stu-id="e49e4-146">In the **Mappings** section, there are two selectable sets of attribute mappings: one for user objects and one for group objects.</span></span> <span data-ttu-id="e49e4-147">Välj var och en att granska de attribut som synkroniseras från Azure Active Directory till din app.</span><span class="sxs-lookup"><span data-stu-id="e49e4-147">Select each one to review the attributes that are synchronized from Azure Active Directory to your app.</span></span> <span data-ttu-id="e49e4-148">De attribut som valts som **matchande** egenskaper som används för att matcha de användare och grupper i din app för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="e49e4-148">The attributes selected as **Matching** properties are used to match the users and groups in your app for update operations.</span></span> <span data-ttu-id="e49e4-149">Välj knappen Spara för att genomföra ändringarna.</span><span class="sxs-lookup"><span data-stu-id="e49e4-149">Select the Save button to commit any changes.</span></span>

    >[!NOTE]
    ><span data-ttu-id="e49e4-150">Alternativt kan du inaktivera synkronisering av gruppobjekt genom att inaktivera ”grupper”-mappning.</span><span class="sxs-lookup"><span data-stu-id="e49e4-150">You can optionally disable syncing of group objects by disabling the "groups" mapping.</span></span> 

11. <span data-ttu-id="e49e4-151">Under **inställningar**, **omfång** fältet definierar vilka användare som är och eller grupper som ska synkroniseras.</span><span class="sxs-lookup"><span data-stu-id="e49e4-151">Under **Settings**, the **Scope** field defines which users and or groups are synchronized.</span></span> <span data-ttu-id="e49e4-152">Att välja ”Sync endast har tilldelats användare och grupper” (rekommenderas) bara synkronisera användare och grupper som tilldelats i den **användare och grupper** fliken.</span><span class="sxs-lookup"><span data-stu-id="e49e4-152">Selecting "Sync only assigned users and groups" (recommended) will only sync users and groups assigned in the **Users and groups** tab.</span></span>
12. <span data-ttu-id="e49e4-153">När konfigurationen är klar kan du ändra den **Status för etablering** till **på**.</span><span class="sxs-lookup"><span data-stu-id="e49e4-153">Once your configuration is complete, change the **Provisioning Status** to **On**.</span></span>
13. <span data-ttu-id="e49e4-154">Klicka på **spara** att starta Azure AD etableras.</span><span class="sxs-lookup"><span data-stu-id="e49e4-154">Click **Save** to start the Azure AD provisioning service.</span></span> 
14. <span data-ttu-id="e49e4-155">Om du synkroniserar endast har tilldelats användare och grupper (rekommenderas), måste du markera den **användare och grupper** fliken och tilldela användare och/eller grupper som du vill synkronisera.</span><span class="sxs-lookup"><span data-stu-id="e49e4-155">If syncing only assigned users and groups (recommended), be sure to select the **Users and groups** tab and assign the users and/or groups you wish to sync.</span></span>

<span data-ttu-id="e49e4-156">När den inledande synkroniseringen har startat, kan du använda den **granskningsloggar** att övervaka förloppet som visar alla åtgärder som utförs av tjänsten etablering på din app.</span><span class="sxs-lookup"><span data-stu-id="e49e4-156">Once the initial synchronization has started, you can use the **Audit logs** tab to monitor progress, which shows all actions performed by the provisioning service on your app.</span></span> <span data-ttu-id="e49e4-157">Mer information om hur du tolkar Azure AD-etablering loggar finns [rapportering om automatisk konto användaretablering](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="e49e4-157">For more information on how to read the Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>

>[!NOTE]
><span data-ttu-id="e49e4-158">Den första synkroniseringen tar längre tid än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge som tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="e49e4-158">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> 


## <a name="building-your-own-provisioning-solution-for-any-application"></a><span data-ttu-id="e49e4-159">Skapa din egen lösning för etablering för alla program</span><span class="sxs-lookup"><span data-stu-id="e49e4-159">Building your own provisioning solution for any application</span></span>
<span data-ttu-id="e49e4-160">Du kan aktivera enkel inloggning och automatisk användaretablering för nästan alla program som innehåller en REST- eller SOAP användaretablering API genom att skapa en SCIM-webbtjänst som samverkar med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e49e4-160">By creating a SCIM web service that interfaces with Azure Active Directory, you can enable single sign-on and automatic user provisioning for virtually any application that provides a REST or SOAP user provisioning API.</span></span>

<span data-ttu-id="e49e4-161">Här är hur det fungerar:</span><span class="sxs-lookup"><span data-stu-id="e49e4-161">Here’s how it works:</span></span>

1. <span data-ttu-id="e49e4-162">Azure AD innehåller ett vanligt språk infrastrukturbibliotek med namnet [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/).</span><span class="sxs-lookup"><span data-stu-id="e49e4-162">Azure AD provides a common language infrastructure library named [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/).</span></span> <span data-ttu-id="e49e4-163">Systemintegrerare och utvecklare kan använda det här biblioteket för att skapa och distribuera en SCIM-baserade webbtjänstslutpunkt kan ansluta Azure AD till Identitetslagret för alla program.</span><span class="sxs-lookup"><span data-stu-id="e49e4-163">System integrators and developers can use this library to create and deploy a SCIM-based web service endpoint capable of connecting Azure AD to any application’s identity store.</span></span>
2. <span data-ttu-id="e49e4-164">Mappningar implementeras i webbtjänsten för att mappa det standardiserade användarschemat till användarschemat och protokoll som krävs för programmet.</span><span class="sxs-lookup"><span data-stu-id="e49e4-164">Mappings are implemented in the web service to map the standardized user schema to the user schema and protocol required by the application.</span></span>
3. <span data-ttu-id="e49e4-165">Slutpunkts-URL är registrerad i Azure AD som en del av ett anpassat program i programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="e49e4-165">The endpoint URL is registered in Azure AD as part of a custom application in the application gallery.</span></span>
4. <span data-ttu-id="e49e4-166">Användare och grupper är tilldelade till det här programmet i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e49e4-166">Users and groups are assigned to this application in Azure AD.</span></span> <span data-ttu-id="e49e4-167">Vid tilldelning av placeras de i en kö som ska synkroniseras till målprogrammet.</span><span class="sxs-lookup"><span data-stu-id="e49e4-167">Upon assignment, they are put into a queue to be synchronized to the target application.</span></span> <span data-ttu-id="e49e4-168">Hantera kön synkroniseringsprocessen körs var tjugonde minut.</span><span class="sxs-lookup"><span data-stu-id="e49e4-168">The synchronization process handling the queue runs every 20 minutes.</span></span>

### <a name="code-samples"></a><span data-ttu-id="e49e4-169">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="e49e4-169">Code Samples</span></span>
<span data-ttu-id="e49e4-170">Att göra den här processen, en uppsättning [kodexempel](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) tillhandahålls som skapar en slutpunkt för webbtjänsten SCIM och visa Automatisk etablering.</span><span class="sxs-lookup"><span data-stu-id="e49e4-170">To make this process easier, a set of [code samples](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) are provided that create a SCIM web service endpoint and demonstrate automatic provisioning.</span></span> <span data-ttu-id="e49e4-171">Ett exempel är av en provider som underhåller en fil med rader av kommaavgränsade värden som representerar användare och grupper.</span><span class="sxs-lookup"><span data-stu-id="e49e4-171">One sample is of a provider that maintains a file with rows of comma-separated values representing users and groups.</span></span>  <span data-ttu-id="e49e4-172">Den andra är av en provider som fungerar på tjänsten Amazon Web Services identitets- och åtkomsthantering.</span><span class="sxs-lookup"><span data-stu-id="e49e4-172">The other is of a provider that operates on the Amazon Web Services Identity and Access Management service.</span></span>  

<span data-ttu-id="e49e4-173">**Förutsättningar**</span><span class="sxs-lookup"><span data-stu-id="e49e4-173">**Prerequisites**</span></span>

* <span data-ttu-id="e49e4-174">Visual Studio 2013 eller senare</span><span class="sxs-lookup"><span data-stu-id="e49e4-174">Visual Studio 2013 or later</span></span>
* [<span data-ttu-id="e49e4-175">Azure SDK för .NET</span><span class="sxs-lookup"><span data-stu-id="e49e4-175">Azure SDK for .NET</span></span>](https://azure.microsoft.com/downloads/)
* <span data-ttu-id="e49e4-176">Windows-datorn som stöder ASP.NET framework 4.5 som ska användas som SCIM slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="e49e4-176">Windows machine that supports the ASP.NET framework 4.5 to be used as the SCIM endpoint.</span></span> <span data-ttu-id="e49e4-177">Den här datorn måste vara tillgänglig från molnet</span><span class="sxs-lookup"><span data-stu-id="e49e4-177">This machine must be accessible from the cloud</span></span>
* [<span data-ttu-id="e49e4-178">En Azure-prenumeration med en utvärderingsversion eller licensierad version av Azure AD Premium</span><span class="sxs-lookup"><span data-stu-id="e49e4-178">An Azure subscription with a trial or licensed version of Azure AD Premium</span></span>](https://azure.microsoft.com/services/active-directory/)
* <span data-ttu-id="e49e4-179">Amazon AWS-exemplet kräver bibliotek från den [AWS Toolkit för Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html).</span><span class="sxs-lookup"><span data-stu-id="e49e4-179">The Amazon AWS sample requires libraries from the [AWS Toolkit for Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html).</span></span> <span data-ttu-id="e49e4-180">Mer information finns i README-filen som ingår i exemplet.</span><span class="sxs-lookup"><span data-stu-id="e49e4-180">For more information, see the README file included with the sample.</span></span>

### <a name="getting-started"></a><span data-ttu-id="e49e4-181">Komma igång</span><span class="sxs-lookup"><span data-stu-id="e49e4-181">Getting Started</span></span>
<span data-ttu-id="e49e4-182">Det enklaste sättet att implementera en SCIM-slutpunkt som kan acceptera etableringsbegäranden från Azure AD är att skapa och distribuera kodexempel som matar ut etablerade användare till en fil med kommaavgränsade värden (CSV).</span><span class="sxs-lookup"><span data-stu-id="e49e4-182">The easiest way to implement a SCIM endpoint that can accept provisioning requests from Azure AD is to build and deploy the code sample that outputs the provisioned users to a comma-separated value (CSV) file.</span></span>

<span data-ttu-id="e49e4-183">**Skapa en exempel SCIM slutpunkt:**</span><span class="sxs-lookup"><span data-stu-id="e49e4-183">**To create a sample SCIM endpoint:**</span></span>

1. <span data-ttu-id="e49e4-184">Hämta exempel kodpaketet på [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)</span><span class="sxs-lookup"><span data-stu-id="e49e4-184">Download the code sample package at [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)</span></span>
2. <span data-ttu-id="e49e4-185">Packa upp paketet och placera den på din Windows-dator på en plats, till exempel C:\AzureAD-BYOA-Provisioning-Samples\.</span><span class="sxs-lookup"><span data-stu-id="e49e4-185">Unzip the package and place it on your Windows machine at a location such as C:\AzureAD-BYOA-Provisioning-Samples\.</span></span>
3. <span data-ttu-id="e49e4-186">Starta FileProvisioningAgent lösningen i Visual Studio i den här mappen.</span><span class="sxs-lookup"><span data-stu-id="e49e4-186">In this folder, launch the FileProvisioningAgent solution in Visual Studio.</span></span>
4. <span data-ttu-id="e49e4-187">Välj **Verktyg > Library Package Manager > Package Manager-konsolen**, och kör följande kommandon att lösa lösning referenser FileProvisioningAgent projektet:</span><span class="sxs-lookup"><span data-stu-id="e49e4-187">Select **Tools > Library Package Manager > Package Manager Console**, and execute the following commands for the FileProvisioningAgent project to resolve the solution references:</span></span>
  ```` 
   Install-Package Microsoft.SystemForCrossDomainIdentityManagement
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   Install-Package Microsoft.Owin.Diagnostics
   Install-Package Microsoft.Owin.Host.SystemWeb
  ````
5. <span data-ttu-id="e49e4-188">Skapa FileProvisioningAgent-projektet.</span><span class="sxs-lookup"><span data-stu-id="e49e4-188">Build the FileProvisioningAgent project.</span></span>
6. <span data-ttu-id="e49e4-189">Starta Kommandotolken programmet i Windows (som administratör) och använda den **cd** kommando för att ändra katalogen till din **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** mapp.</span><span class="sxs-lookup"><span data-stu-id="e49e4-189">Launch the Command Prompt application in Windows (as an Administrator), and use the **cd** command to change the directory to your **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** folder.</span></span>
7. <span data-ttu-id="e49e4-190">Kör du följande kommando och ersätter < ip-adress > med IP-adress eller domännamn namnet på Windows-dator:</span><span class="sxs-lookup"><span data-stu-id="e49e4-190">Run the following command, replacing <ip-address> with the IP address or domain name of the Windows machine:</span></span>
  ````   
   FileAgnt.exe http://<ip-address>:9000 TargetFile.csv
  ````
8. <span data-ttu-id="e49e4-191">I under **Windows-inställningar > nätverk och Internet-inställningar**, Välj den **Windows-brandväggen > Avancerade inställningar**, och skapa en **inkommande regel** som tillåter inkommande åtkomst till port 9000.</span><span class="sxs-lookup"><span data-stu-id="e49e4-191">In Windows under **Windows Settings > Network & Internet Settings**, select the **Windows Firewall > Advanced Settings**, and create an **Inbound Rule** that allows inbound access to port 9000.</span></span>
9. <span data-ttu-id="e49e4-192">Om Windows-dator bakom en router kan måste routern konfigureras för att utföra Network Access Translation mellan dess port 9000 som exponeras för internet och port 9000 på Windows-datorn.</span><span class="sxs-lookup"><span data-stu-id="e49e4-192">If the Windows machine is behind a router, the router needs to be configured to perform Network Access Translation between its port 9000 that is exposed to the internet, and port 9000 on the Windows machine.</span></span> <span data-ttu-id="e49e4-193">Detta krävs för Azure AD för att kunna komma åt den här slutpunkten i molnet.</span><span class="sxs-lookup"><span data-stu-id="e49e4-193">This is required for Azure AD to be able to access this endpoint in the cloud.</span></span>

<span data-ttu-id="e49e4-194">**För att registrera slutpunkten exempel SCIM i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="e49e4-194">**To register the sample SCIM endpoint in Azure AD:**</span></span>

1. <span data-ttu-id="e49e4-195">Logga in på [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e49e4-195">Sign in to [the Azure portal](https://portal.azure.com).</span></span> 
2. <span data-ttu-id="e49e4-196">Bläddra till ** Azure Active Directory > företagsprogram och välj **nytt program > alla > icke-galleriet programmet**.</span><span class="sxs-lookup"><span data-stu-id="e49e4-196">Browse to **Azure Active Directory > Enterprise Applications, and select **New application > All > Non-gallery application**.</span></span>
3. <span data-ttu-id="e49e4-197">Ange ett namn för ditt program och klicka på **Lägg till** ikon för att skapa ett app-objekt.</span><span class="sxs-lookup"><span data-stu-id="e49e4-197">Enter a name for your application, and click **Add** icon to create an app object.</span></span> <span data-ttu-id="e49e4-198">Det programobjekt som skapas ska representera mål appen du skulle etablering till och implementera enkel inloggning för och inte bara SCIM slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="e49e4-198">The application object created is intended to represent the target app you would be provisioning to and implementing single sign-on for, and not just the SCIM endpoint.</span></span>
4. <span data-ttu-id="e49e4-199">I skärmbilden som visas väljer du den **etablering** fliken i den vänstra kolumnen.</span><span class="sxs-lookup"><span data-stu-id="e49e4-199">In the resulting screen, select the **Provisioning** tab in the left column.</span></span>
5. <span data-ttu-id="e49e4-200">I den **etablering läge** väljer du **automatisk**.</span><span class="sxs-lookup"><span data-stu-id="e49e4-200">In the **Provisioning Mode** menu, select **Automatic**.</span></span>
    
  <span data-ttu-id="e49e4-201">![][2]
  *Bild 4: Konfigurera etablering i Azure-portalen*</span><span class="sxs-lookup"><span data-stu-id="e49e4-201">![][2]
*Figure 4: Configuring provisioning in the Azure portal*</span></span>
    
6. <span data-ttu-id="e49e4-202">I den **klient URL** anger internet-exponerade URL och port för slutpunkten SCIM.</span><span class="sxs-lookup"><span data-stu-id="e49e4-202">In the **Tenant URL** field, enter the internet-exposed URL and port of your SCIM endpoint.</span></span> <span data-ttu-id="e49e4-203">Det är något som http://testmachine.contoso.com:9000 eller http://<ip-address>:9000/, där < ip-adress > är internet exponeras IP adress.</span><span class="sxs-lookup"><span data-stu-id="e49e4-203">This would be something like http://testmachine.contoso.com:9000 or http://<ip-address>:9000/, where <ip-address> is the internet exposed IP address.</span></span>  
7. <span data-ttu-id="e49e4-204">Om slutpunkten SCIM kräver en OAuth ägar-token från en utfärdare än Azure AD, kopiera nödvändiga OAuth ägar-token till den valfria **hemlighet Token** fältet.</span><span class="sxs-lookup"><span data-stu-id="e49e4-204">If the SCIM endpoint requires an OAuth bearer token from an issuer other than Azure AD, then copy the required OAuth bearer token into the optional **Secret Token** field.</span></span> <span data-ttu-id="e49e4-205">Om det här fältet är tomt, innehåller en OAuth ägar-token som utfärdas från Azure AD med varje begäran Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e49e4-205">If this field is left blank, then Azure AD will include an OAuth bearer token issued from Azure AD with each request.</span></span> <span data-ttu-id="e49e4-206">Appar som använder Azure AD som en identitetsleverantör kan verifiera den här Azure AD-utfärdade token.</span><span class="sxs-lookup"><span data-stu-id="e49e4-206">Apps that use Azure AD as an identity provider can validate this Azure AD -issued token.</span></span>
8. <span data-ttu-id="e49e4-207">Klicka på den **Testanslutningen** så försöker ansluta till slutpunkten SCIM för Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e49e4-207">Click the **Test Connection** button to have Azure Active Directory attempt to connect to the SCIM endpoint.</span></span> <span data-ttu-id="e49e4-208">Om försöker misslyckas, visas information om felet.</span><span class="sxs-lookup"><span data-stu-id="e49e4-208">If the attempts fail, error information is displayed.</span></span>  
9. <span data-ttu-id="e49e4-209">Om försök att ansluta till program-lyckad klickar **spara** spara administratörsautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="e49e4-209">If the attempts to connect to the application succeed, then click **Save** to save the admin credentials.</span></span>
10. <span data-ttu-id="e49e4-210">I den **mappningar** avsnittet finns det två valbar uppsättningar attributmappning: en för användarobjekt och en för gruppobjekt.</span><span class="sxs-lookup"><span data-stu-id="e49e4-210">In the **Mappings** section, there are two selectable sets of attribute mappings: one for user objects and one for group objects.</span></span> <span data-ttu-id="e49e4-211">Välj var och en att granska de attribut som synkroniseras från Azure Active Directory till din app.</span><span class="sxs-lookup"><span data-stu-id="e49e4-211">Select each one to review the attributes that are synchronized from Azure Active Directory to your app.</span></span> <span data-ttu-id="e49e4-212">De attribut som valts som **matchande** egenskaper som används för att matcha de användare och grupper i din app för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="e49e4-212">The attributes selected as **Matching** properties are used to match the users and groups in your app for update operations.</span></span> <span data-ttu-id="e49e4-213">Välj knappen Spara för att genomföra ändringarna.</span><span class="sxs-lookup"><span data-stu-id="e49e4-213">Select the Save button to commit any changes.</span></span>
11. <span data-ttu-id="e49e4-214">Under **inställningar**, **omfång** fältet definierar vilka användare som är och eller grupper som ska synkroniseras.</span><span class="sxs-lookup"><span data-stu-id="e49e4-214">Under **Settings**, the **Scope** field defines which users and or groups are synchronized.</span></span> <span data-ttu-id="e49e4-215">Att välja ”Sync endast har tilldelats användare och grupper” (rekommenderas) bara synkronisera användare och grupper som tilldelats i den **användare och grupper** fliken.</span><span class="sxs-lookup"><span data-stu-id="e49e4-215">Selecting "Sync only assigned users and groups" (recommended) will only sync users and groups assigned in the **Users and groups** tab.</span></span>
12. <span data-ttu-id="e49e4-216">När konfigurationen är klar kan du ändra den **Status för etablering** till **på**.</span><span class="sxs-lookup"><span data-stu-id="e49e4-216">Once your configuration is complete, change the **Provisioning Status** to **On**.</span></span>
13. <span data-ttu-id="e49e4-217">Klicka på **spara** att starta Azure AD etableras.</span><span class="sxs-lookup"><span data-stu-id="e49e4-217">Click **Save** to start the Azure AD provisioning service.</span></span> 
14. <span data-ttu-id="e49e4-218">Om du synkroniserar endast har tilldelats användare och grupper (rekommenderas), måste du markera den **användare och grupper** fliken och tilldela användare och/eller grupper som du vill synkronisera.</span><span class="sxs-lookup"><span data-stu-id="e49e4-218">If syncing only assigned users and groups (recommended), be sure to select the **Users and groups** tab and assign the users and/or groups you wish to sync.</span></span>

<span data-ttu-id="e49e4-219">När den inledande synkroniseringen har startat, kan du använda den **granskningsloggar** att övervaka förloppet som visar alla åtgärder som utförs av tjänsten etablering på din app.</span><span class="sxs-lookup"><span data-stu-id="e49e4-219">Once the initial synchronization has started, you can use the **Audit logs** tab to monitor progress, which shows all actions performed by the provisioning service on your app.</span></span> <span data-ttu-id="e49e4-220">Mer information om hur du tolkar Azure AD-etablering loggar finns [rapportering om automatisk konto användaretablering](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="e49e4-220">For more information on how to read the Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>

<span data-ttu-id="e49e4-221">Det sista steget vid verifiering av exemplet är att öppna filen TargetFile.csv i mappen \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug på din Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="e49e4-221">The final step in verifying the sample is to open the TargetFile.csv file in the \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug folder on your Windows machine.</span></span> <span data-ttu-id="e49e4-222">När etableringen har körts, visas den här filen detaljer om allt tilldelade och etablerad användare och grupper.</span><span class="sxs-lookup"><span data-stu-id="e49e4-222">Once the provisioning process is run, this file shows the details of all assigned and provisioned users and groups.</span></span>

### <a name="development-libraries"></a><span data-ttu-id="e49e4-223">Utvecklingsbibliotek</span><span class="sxs-lookup"><span data-stu-id="e49e4-223">Development libraries</span></span>
<span data-ttu-id="e49e4-224">För att utveckla egna webbtjänst som uppfyller specifikationerna SCIM uppbyggd följande bibliotek som tillhandahålls av Microsoft för att påskynda utvecklingsprocessen:</span><span class="sxs-lookup"><span data-stu-id="e49e4-224">To develop your own web service that conforms to the SCIM specification, first familiarize yourself with the following libraries provided by Microsoft to help accelerate the development process:</span></span> 

1. <span data-ttu-id="e49e4-225">Common Language Infrastructure (CLI) bibliotek erbjuds för användning med språk som är baserat på infrastrukturen, till exempel C#.</span><span class="sxs-lookup"><span data-stu-id="e49e4-225">Common Language Infrastructure (CLI) libraries are offered for use with languages based on that infrastructure, such as C#.</span></span> <span data-ttu-id="e49e4-226">En av dessa bibliotek [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), deklarerar ett gränssnitt, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, som visas i följande bild: en utvecklare som använder biblioteken skulle implementera gränssnittet med en klass som kan refereras till, Allmänt, som en provider.</span><span class="sxs-lookup"><span data-stu-id="e49e4-226">One of those libraries, [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), declares an interface, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, shown in the following illustration:  A developer using the libraries would implement that interface with a class that may be referred to, generically, as a provider.</span></span> <span data-ttu-id="e49e4-227">Biblioteken kan utvecklare att distribuera en webbtjänst som överensstämmer med SCIM-specifikationen.</span><span class="sxs-lookup"><span data-stu-id="e49e4-227">The libraries enable the developer to deploy a web service that conforms to the SCIM specification.</span></span> <span data-ttu-id="e49e4-228">Webbtjänsten kan finnas antingen i Internet Information Services eller alla körbara vanlig infrastruktur för språk-sammansättningen.</span><span class="sxs-lookup"><span data-stu-id="e49e4-228">The web service can be either hosted within Internet Information Services, or any executable Common Language Infrastructure assembly.</span></span> <span data-ttu-id="e49e4-229">Begäran översätts till anrop till leverantörens metoder som skulle vara programmerad av utvecklaren att använda vissa Identitetslagret.</span><span class="sxs-lookup"><span data-stu-id="e49e4-229">Request is translated into calls to the provider’s methods, which would be programmed by the developer to operate on some identity store.</span></span>
  
  ![][3]
  
2. <span data-ttu-id="e49e4-230">[Express route-hanterare](http://expressjs.com/guide/routing.html) tillgängliga för parsning av node.js begärandeobjekt som motsvarar anrop (enligt specifikationen SCIM), görs till en node.js-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="e49e4-230">[Express route handlers](http://expressjs.com/guide/routing.html) are available for parsing node.js request objects representing calls (as defined by the SCIM specification), made to a node.js web service.</span></span>   

### <a name="building-a-custom-scim-endpoint"></a><span data-ttu-id="e49e4-231">Skapa en anpassad SCIM slutpunkt</span><span class="sxs-lookup"><span data-stu-id="e49e4-231">Building a Custom SCIM Endpoint</span></span>
<span data-ttu-id="e49e4-232">Med hjälp av CLI-bibliotek, kan utvecklare som använder dessa bibliotek värd sina tjänster i alla körbara vanlig infrastruktur för språk-sammansättningen eller i Internet Information Services.</span><span class="sxs-lookup"><span data-stu-id="e49e4-232">Using the CLI libraries, developers using those libraries can host their services within any executable Common Language Infrastructure assembly, or within Internet Information Services.</span></span> <span data-ttu-id="e49e4-233">Här är exempelkod som värd för en tjänst i en körbar sammansättning på adressen http://localhost:9000:</span><span class="sxs-lookup"><span data-stu-id="e49e4-233">Here is sample code for hosting a service within an executable assembly, at the address http://localhost:9000:</span></span> 

    private static void Main(string[] arguments)
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IProvider and 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
      new DevelopersMonitor();
    Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
      new DevelopersProvider(arguments[1]);
    Microsoft.SystemForCrossDomainIdentityManagement.Service webService = null;
    try
    {
        webService = new WebService(monitor, provider);
        webService.Start("http://localhost:9000");

        Console.ReadKey(true);
    }
    finally
    {
        if (webService != null)
        {
            webService.Dispose();
            webService = null;
        }
    }
    }

    public class WebService : Microsoft.SystemForCrossDomainIdentityManagement.Service
    {
    private Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor;
    private Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider;

    public WebService(
      Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitoringBehavior, 
      Microsoft.SystemForCrossDomainIdentityManagement.IProvider providerBehavior)
    {
        this.monitor = monitoringBehavior;
        this.provider = providerBehavior;
    }

    public override IMonitor MonitoringBehavior
    {
        get
        {
            return this.monitor;
        }

        set
        {
            this.monitor = value;
        }
    }

    public override IProvider ProviderBehavior
    {
        get
        {
            return this.provider;
        }

        set
        {
            this.provider = value;
        }
    }
    }

<span data-ttu-id="e49e4-234">Den här tjänsten måste ha en HTTP-adress och servern certifikat för serverautentisering som rotcertifikatutfärdaren är något av följande:</span><span class="sxs-lookup"><span data-stu-id="e49e4-234">This service must have an HTTP address and server authentication certificate of which the root certification authority is one of the following:</span></span> 

* <span data-ttu-id="e49e4-235">CNNIC</span><span class="sxs-lookup"><span data-stu-id="e49e4-235">CNNIC</span></span>
* <span data-ttu-id="e49e4-236">Comodo</span><span class="sxs-lookup"><span data-stu-id="e49e4-236">Comodo</span></span>
* <span data-ttu-id="e49e4-237">CyberTrust</span><span class="sxs-lookup"><span data-stu-id="e49e4-237">CyberTrust</span></span>
* <span data-ttu-id="e49e4-238">DigiCert</span><span class="sxs-lookup"><span data-stu-id="e49e4-238">DigiCert</span></span>
* <span data-ttu-id="e49e4-239">GeoTrust</span><span class="sxs-lookup"><span data-stu-id="e49e4-239">GeoTrust</span></span>
* <span data-ttu-id="e49e4-240">GlobalSign</span><span class="sxs-lookup"><span data-stu-id="e49e4-240">GlobalSign</span></span>
* <span data-ttu-id="e49e4-241">Go Daddy</span><span class="sxs-lookup"><span data-stu-id="e49e4-241">Go Daddy</span></span>
* <span data-ttu-id="e49e4-242">VeriSign</span><span class="sxs-lookup"><span data-stu-id="e49e4-242">Verisign</span></span>
* <span data-ttu-id="e49e4-243">WoSign</span><span class="sxs-lookup"><span data-stu-id="e49e4-243">WoSign</span></span>

<span data-ttu-id="e49e4-244">Ett certifikat för serverautentisering kan bindas till en port på en Windows-värd med hjälp av verktyget network shell:</span><span class="sxs-lookup"><span data-stu-id="e49e4-244">A server authentication certificate can be bound to a port on a Windows host using the network shell utility:</span></span> 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  

<span data-ttu-id="e49e4-245">Värdet som angetts för argumentet certhash är här, tumavtrycket för certifikatet, men värdet som angetts för argumentet appid är en godtycklig globalt unik identifierare.</span><span class="sxs-lookup"><span data-stu-id="e49e4-245">Here, the value provided for the certhash argument is the thumbprint of the certificate, while the value provided for the appid argument is an arbitrary globally unique identifier.</span></span>  

<span data-ttu-id="e49e4-246">Som värd för tjänsten inom Internet Information Services, skulle en utvecklare skapa en CLA kod bibliotekssammansättning med en klass som heter Start i standardnamnområdet för sammansättningen.</span><span class="sxs-lookup"><span data-stu-id="e49e4-246">To host the service within Internet Information Services, a developer would build a CLA code library assembly with a class named Startup in the default namespace of the assembly.</span></span>  <span data-ttu-id="e49e4-247">Här är ett exempel på sådan klass:</span><span class="sxs-lookup"><span data-stu-id="e49e4-247">Here is a sample of such a class:</span></span> 

    public class Startup
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor and  
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter starter;

    public Startup()
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
          new DevelopersMonitor();
        Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
          new DevelopersProvider();
        this.starter = 
          new Microsoft.SystemForCrossDomainIdentityManagement.WebApplicationStarter(
            provider, 
            monitor);
    }

    public void Configuration(
      Owin.IAppBuilder builder) // Defined in in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

### <a name="handling-endpoint-authentication"></a><span data-ttu-id="e49e4-248">Hantering av slutpunkten autentisering</span><span class="sxs-lookup"><span data-stu-id="e49e4-248">Handling endpoint authentication</span></span>
<span data-ttu-id="e49e4-249">Begäranden från Azure Active Directory innehåller en OAuth 2.0-ägartoken.</span><span class="sxs-lookup"><span data-stu-id="e49e4-249">Requests from Azure Active Directory include an OAuth 2.0 bearer token.</span></span>   <span data-ttu-id="e49e4-250">Alla tjänster som tar emot begäran ska autentisera utfärdaren som Azure Active Directory för den förväntade Azure Active Directory-klienten för åtkomst till Azure Active Directory Graph-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="e49e4-250">Any service receiving the request should authenticate the issuer as being Azure Active Directory on behalf of the expected Azure Active Directory tenant, for access to the Azure Active Directory Graph web service.</span></span>  <span data-ttu-id="e49e4-251">I denna token identifieras utfärdaren av ett iss-anspråk som ”iss”: ”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”.</span><span class="sxs-lookup"><span data-stu-id="e49e4-251">In the token, the issuer is identified by an iss claim, like, "iss":"https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".</span></span>  <span data-ttu-id="e49e4-252">I det här exemplet basadressen för anspråksvärdet, https://sts.windows.net, identifierar Azure Active Directory som utfärdaren medan relativ adress segment, cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, är en unik identifierare för Azure Active Directory-klient som token har utfärdats.</span><span class="sxs-lookup"><span data-stu-id="e49e4-252">In this example, the base address of the claim value, https://sts.windows.net, identifies Azure Active Directory as the issuer, while the relative address segment, cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, is a unique identifier of the Azure Active Directory tenant on behalf of which the token was issued.</span></span>  <span data-ttu-id="e49e4-253">Om token har utfärdats för åtkomst till Azure Active Directory Graph-webbtjänsten ska identifierare för tjänsten, 00000002-0000-0000-c000-000000000000, i värdet för token eller anspråk.</span><span class="sxs-lookup"><span data-stu-id="e49e4-253">If the token was issued for accessing the Azure Active Directory Graph web service, then the identifier of that service, 00000002-0000-0000-c000-000000000000, should be in the value of the token’s aud claim.</span></span>  

<span data-ttu-id="e49e4-254">Utvecklare som använder biblioteken CLA som tillhandahålls av Microsoft för att skapa en SCIM-tjänst kan autentisera begäranden från Azure Active Directory med Microsoft.Owin.Security.ActiveDirectory paketet genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="e49e4-254">Developers using the CLA libraries provided by Microsoft for building a SCIM service can authenticate requests from Azure Active Directory using the Microsoft.Owin.Security.ActiveDirectory package by following these steps:</span></span> 

1. <span data-ttu-id="e49e4-255">I en provider, implementera egenskapen Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior genom att låta den returnera en metod som ska anropas när tjänsten startas:</span><span class="sxs-lookup"><span data-stu-id="e49e4-255">In a provider, implement the Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior property by having it return a method to be called whenever the service is started:</span></span> 

  ````
    public override Action\<Owin.IAppBuilder, System.Web.Http.HttpConfiguration.HttpConfiguration\> StartupBehavior
    {
      get
      {
        return this.OnServiceStartup;
      }
    }

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder,  // Defined in Owin.dll.  
      System.Web.Http.HttpConfiguration configuration)  // Defined in System.Web.Http.dll.  
    {
    }
  ````

2. <span data-ttu-id="e49e4-256">Lägg till följande kod i metoden att alla förfrågningar till någon av Tjänsteslutpunkter autentiserad som försetts med en token som utfärdas av Azure Active Directory för en angiven klient för åtkomst till Azure AD Graph-webbtjänsten:</span><span class="sxs-lookup"><span data-stu-id="e49e4-256">Add the following code to that method to have any request to any of the service’s endpoints authenticated as bearing a token issued by Azure Active Directory on behalf of a specified tenant, for access to the Azure AD Graph web service:</span></span> 

  ````
    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder IAppBuilder applicationBuilder, 
      System.Web.Http.HttpConfiguration HttpConfiguration configuration)
    {
      // IFilter is defined in System.Web.Http.dll.  
      System.Web.Http.Filters.IFilter authorizationFilter = 
        new System.Web.Http.AuthorizeAttribute(); // Defined in System.Web.Http.dll.configuration.Filters.Add(authorizationFilter);

      // SystemIdentityModel.Tokens.TokenValidationParameters is defined in    
      // System.IdentityModel.Token.Jwt.dll.
      SystemIdentityModel.Tokens.TokenValidationParameters tokenValidationParameters =     
        new TokenValidationParameters()
        {
          ValidAudience = "00000002-0000-0000-c000-000000000000"
        };

      // WindowsAzureActiveDirectoryBearerAuthenticationOptions is defined in 
      // Microsoft.Owin.Security.ActiveDirectory.dll
      Microsoft.Owin.Security.ActiveDirectory.
      WindowsAzureActiveDirectoryBearerAuthenticationOptions authenticationOptions =
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions()    {
        TokenValidationParameters = tokenValidationParameters,
        Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute the appropriate tenant’s 
                                                      // identifier for this one.  
      };

      applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
    }
  ````


## <a name="user-and-group-schema"></a><span data-ttu-id="e49e4-257">Användar- och schema</span><span class="sxs-lookup"><span data-stu-id="e49e4-257">User and group schema</span></span>
<span data-ttu-id="e49e4-258">Azure Active Directory kan etablera två typer av resurser till SCIM webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="e49e4-258">Azure Active Directory can provision two types of resources to SCIM web services.</span></span>  <span data-ttu-id="e49e4-259">Dessa typer av resurser är användare och grupper.</span><span class="sxs-lookup"><span data-stu-id="e49e4-259">Those types of resources are users and groups.</span></span>  

<span data-ttu-id="e49e4-260">Resurser för användare identifieras av schema-ID, urn: ietf:params:scim:schemas:extension:enterprise:2.0:User som ingår i den här protokollspecifikation: http://tools.ietf.org/html/draft-ietf-scim-core-schema.</span><span class="sxs-lookup"><span data-stu-id="e49e4-260">User resources are identified by the schema identifier, urn:ietf:params:scim:schemas:extension:enterprise:2.0:User, which is included in this protocol specification: http://tools.ietf.org/html/draft-ietf-scim-core-schema.</span></span>  <span data-ttu-id="e49e4-261">Standardmappningen attribut för användare i Azure Active Directory attributen för urn: ietf:params:scim:schemas:extension:enterprise:2.0:User resurser finns i tabell 1 nedan.</span><span class="sxs-lookup"><span data-stu-id="e49e4-261">The default mapping of the attributes of users in Azure Active Directory to the attributes of urn:ietf:params:scim:schemas:extension:enterprise:2.0:User resources is provided in table 1, below.</span></span>  

<span data-ttu-id="e49e4-262">Gruppera resurser identifieras av schema-ID, http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.</span><span class="sxs-lookup"><span data-stu-id="e49e4-262">Group resources are identified by the schema identifier, http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.</span></span>  <span data-ttu-id="e49e4-263">Tabell 2 nedan visar standardmappningen attribut för resursgrupper i Azure Active Directory attributen http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.</span><span class="sxs-lookup"><span data-stu-id="e49e4-263">Table 2, below, shows the default mapping of the attributes of groups in Azure Active Directory to the attributes of http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group resources.</span></span>  

### <a name="table-1-default-user-attribute-mapping"></a><span data-ttu-id="e49e4-264">Tabell 1: Standard användaren attributmappning</span><span class="sxs-lookup"><span data-stu-id="e49e4-264">Table 1: Default user attribute mapping</span></span>
| <span data-ttu-id="e49e4-265">Azure Active Directory-användare</span><span class="sxs-lookup"><span data-stu-id="e49e4-265">Azure Active Directory user</span></span> | <span data-ttu-id="e49e4-266">urn: ietf:params:scim:schemas:extension:enterprise:2.0:User</span><span class="sxs-lookup"><span data-stu-id="e49e4-266">urn:ietf:params:scim:schemas:extension:enterprise:2.0:User</span></span> |
| --- | --- |
| <span data-ttu-id="e49e4-267">IsSoftDeleted</span><span class="sxs-lookup"><span data-stu-id="e49e4-267">IsSoftDeleted</span></span> |<span data-ttu-id="e49e4-268">Aktiv</span><span class="sxs-lookup"><span data-stu-id="e49e4-268">active</span></span> |
| <span data-ttu-id="e49e4-269">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="e49e4-269">displayName</span></span> |<span data-ttu-id="e49e4-270">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="e49e4-270">displayName</span></span> |
| <span data-ttu-id="e49e4-271">Fax TelephoneNumber</span><span class="sxs-lookup"><span data-stu-id="e49e4-271">Facsimile-TelephoneNumber</span></span> |<span data-ttu-id="e49e4-272">phoneNumbers typ eq ”fax” .value</span><span class="sxs-lookup"><span data-stu-id="e49e4-272">phoneNumbers[type eq "fax"].value</span></span> |
| <span data-ttu-id="e49e4-273">givenName</span><span class="sxs-lookup"><span data-stu-id="e49e4-273">givenName</span></span> |<span data-ttu-id="e49e4-274">name.givenName</span><span class="sxs-lookup"><span data-stu-id="e49e4-274">name.givenName</span></span> |
| <span data-ttu-id="e49e4-275">Befattning</span><span class="sxs-lookup"><span data-stu-id="e49e4-275">jobTitle</span></span> |<span data-ttu-id="e49e4-276">Rubrik</span><span class="sxs-lookup"><span data-stu-id="e49e4-276">title</span></span> |
| <span data-ttu-id="e49e4-277">E-post</span><span class="sxs-lookup"><span data-stu-id="e49e4-277">mail</span></span> |<span data-ttu-id="e49e4-278">e-postmeddelanden typen eq ”arbete” .value</span><span class="sxs-lookup"><span data-stu-id="e49e4-278">emails[type eq "work"].value</span></span> |
| <span data-ttu-id="e49e4-279">mailNickname</span><span class="sxs-lookup"><span data-stu-id="e49e4-279">mailNickname</span></span> |<span data-ttu-id="e49e4-280">externalId</span><span class="sxs-lookup"><span data-stu-id="e49e4-280">externalId</span></span> |
| <span data-ttu-id="e49e4-281">Manager</span><span class="sxs-lookup"><span data-stu-id="e49e4-281">manager</span></span> |<span data-ttu-id="e49e4-282">Manager</span><span class="sxs-lookup"><span data-stu-id="e49e4-282">manager</span></span> |
| <span data-ttu-id="e49e4-283">mobila</span><span class="sxs-lookup"><span data-stu-id="e49e4-283">mobile</span></span> |<span data-ttu-id="e49e4-284">phoneNumbers typ eq ”mobil” .value</span><span class="sxs-lookup"><span data-stu-id="e49e4-284">phoneNumbers[type eq "mobile"].value</span></span> |
| <span data-ttu-id="e49e4-285">objekt-ID</span><span class="sxs-lookup"><span data-stu-id="e49e4-285">objectId</span></span> |<span data-ttu-id="e49e4-286">id</span><span class="sxs-lookup"><span data-stu-id="e49e4-286">id</span></span> |
| <span data-ttu-id="e49e4-287">Postnummer</span><span class="sxs-lookup"><span data-stu-id="e49e4-287">postalCode</span></span> |<span data-ttu-id="e49e4-288">adresser typen eq ”arbete” .postalCode</span><span class="sxs-lookup"><span data-stu-id="e49e4-288">addresses[type eq "work"].postalCode</span></span> |
| <span data-ttu-id="e49e4-289">Proxy-adresser</span><span class="sxs-lookup"><span data-stu-id="e49e4-289">proxy-Addresses</span></span> |<span data-ttu-id="e49e4-290">e-postmeddelanden [Ange eq ”andra”]. Värdet</span><span class="sxs-lookup"><span data-stu-id="e49e4-290">emails[type eq "other"].Value</span></span> |
| <span data-ttu-id="e49e4-291">fysisk-leverans – OfficeName</span><span class="sxs-lookup"><span data-stu-id="e49e4-291">physical-Delivery-OfficeName</span></span> |<span data-ttu-id="e49e4-292">[Ange eq ”andra”] adresser. Formaterad</span><span class="sxs-lookup"><span data-stu-id="e49e4-292">addresses[type eq "other"].Formatted</span></span> |
| <span data-ttu-id="e49e4-293">StreetAddress</span><span class="sxs-lookup"><span data-stu-id="e49e4-293">streetAddress</span></span> |<span data-ttu-id="e49e4-294">adresser typen eq ”arbete” .streetAddress</span><span class="sxs-lookup"><span data-stu-id="e49e4-294">addresses[type eq "work"].streetAddress</span></span> |
| <span data-ttu-id="e49e4-295">Efternamn</span><span class="sxs-lookup"><span data-stu-id="e49e4-295">surname</span></span> |<span data-ttu-id="e49e4-296">name.familyName</span><span class="sxs-lookup"><span data-stu-id="e49e4-296">name.familyName</span></span> |
| <span data-ttu-id="e49e4-297">Telefonnummer</span><span class="sxs-lookup"><span data-stu-id="e49e4-297">telephone-Number</span></span> |<span data-ttu-id="e49e4-298">phoneNumbers typ eq ”arbete” .value</span><span class="sxs-lookup"><span data-stu-id="e49e4-298">phoneNumbers[type eq "work"].value</span></span> |
| <span data-ttu-id="e49e4-299">användaren huvudkontot</span><span class="sxs-lookup"><span data-stu-id="e49e4-299">user-PrincipalName</span></span> |<span data-ttu-id="e49e4-300">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="e49e4-300">userName</span></span> |

### <a name="table-2-default-group-attribute-mapping"></a><span data-ttu-id="e49e4-301">Tabell 2: Standard grupp attributmappning</span><span class="sxs-lookup"><span data-stu-id="e49e4-301">Table 2: Default group attribute mapping</span></span>
| <span data-ttu-id="e49e4-302">Azure Active Directory-grupp</span><span class="sxs-lookup"><span data-stu-id="e49e4-302">Azure Active Directory group</span></span> | <span data-ttu-id="e49e4-303">http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group</span><span class="sxs-lookup"><span data-stu-id="e49e4-303">http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group</span></span> |
| --- | --- |
| <span data-ttu-id="e49e4-304">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="e49e4-304">displayName</span></span> |<span data-ttu-id="e49e4-305">externalId</span><span class="sxs-lookup"><span data-stu-id="e49e4-305">externalId</span></span> |
| <span data-ttu-id="e49e4-306">E-post</span><span class="sxs-lookup"><span data-stu-id="e49e4-306">mail</span></span> |<span data-ttu-id="e49e4-307">e-postmeddelanden typen eq ”arbete” .value</span><span class="sxs-lookup"><span data-stu-id="e49e4-307">emails[type eq "work"].value</span></span> |
| <span data-ttu-id="e49e4-308">mailNickname</span><span class="sxs-lookup"><span data-stu-id="e49e4-308">mailNickname</span></span> |<span data-ttu-id="e49e4-309">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="e49e4-309">displayName</span></span> |
| <span data-ttu-id="e49e4-310">Medlemmar</span><span class="sxs-lookup"><span data-stu-id="e49e4-310">members</span></span> |<span data-ttu-id="e49e4-311">Medlemmar</span><span class="sxs-lookup"><span data-stu-id="e49e4-311">members</span></span> |
| <span data-ttu-id="e49e4-312">objekt-ID</span><span class="sxs-lookup"><span data-stu-id="e49e4-312">objectId</span></span> |<span data-ttu-id="e49e4-313">id</span><span class="sxs-lookup"><span data-stu-id="e49e4-313">id</span></span> |
| <span data-ttu-id="e49e4-314">proxyAddresses</span><span class="sxs-lookup"><span data-stu-id="e49e4-314">proxyAddresses</span></span> |<span data-ttu-id="e49e4-315">e-postmeddelanden [Ange eq ”andra”]. Värdet</span><span class="sxs-lookup"><span data-stu-id="e49e4-315">emails[type eq "other"].Value</span></span> |

## <a name="user-provisioning-and-de-provisioning"></a><span data-ttu-id="e49e4-316">Användaretablering och avetablering</span><span class="sxs-lookup"><span data-stu-id="e49e4-316">User provisioning and de-provisioning</span></span>
<span data-ttu-id="e49e4-317">Följande bild visar meddelanden att Azure Active Directory skickar till en tjänst för SCIM att hantera livscykeln för en användare i en annan identitet store.</span><span class="sxs-lookup"><span data-stu-id="e49e4-317">The following illustration shows the messages that Azure Active Directory sends to a SCIM service to manage the lifecycle of a user in another identity store.</span></span> <span data-ttu-id="e49e4-318">Diagrammet visar även hur en SCIM tjänst implementeras med hjälp av CLI-bibliotek från Microsoft för skapande av dessa tjänster översätta dessa begäranden till anrop av metoderna av en provider.</span><span class="sxs-lookup"><span data-stu-id="e49e4-318">The diagram also shows how a SCIM service implemented using the CLI libraries provided by Microsoft for building such services translate those requests into calls to the methods of a provider.</span></span>  

<span data-ttu-id="e49e4-319">![][4]
*Bild 5: Användaretablering och avetablering sekvens*</span><span class="sxs-lookup"><span data-stu-id="e49e4-319">![][4]
*Figure 5: User provisioning and de-provisioning sequence*</span></span>

1. <span data-ttu-id="e49e4-320">Azure Active Directory frågar tjänsten för en användare med ett attributvärde för externalId matchar mailNickname attributvärdet för en användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e49e4-320">Azure Active Directory queries the service for a user with an externalId attribute value matching the mailNickname attribute value of a user in Azure AD.</span></span> <span data-ttu-id="e49e4-321">Frågan uttrycks som en begäran om Hypertext Transfer Protocol (HTTP) som det här exemplet där jyoung är ett exempel på en mailNickname för en användare i Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="e49e4-321">The query is expressed as a Hypertext Transfer Protocol (HTTP) request such as this example, wherein jyoung is a sample of a mailNickname of a user in Azure Active Directory:</span></span> 
  ````
    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...
  ````
  <span data-ttu-id="e49e4-322">Om tjänsten har skapats med vanlig infrastruktur för språk-bibliotek som tillhandahålls av Microsoft för att implementera SCIM tjänster, översätts begäran till ett anrop till metoden fråga i den tjänstleverantören.</span><span class="sxs-lookup"><span data-stu-id="e49e4-322">If the service was built using the Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, then the request is translated into a call to the Query method of the service’s provider.</span></span>  <span data-ttu-id="e49e4-323">Här är signaturen för metoden:</span><span class="sxs-lookup"><span data-stu-id="e49e4-323">Here is the signature of that method:</span></span> 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]> Query(
      Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
      string correlationIdentifier);
  ````
  <span data-ttu-id="e49e4-324">Här är definitionen av gränssnittet Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters:</span><span class="sxs-lookup"><span data-stu-id="e49e4-324">Here is the definition of the Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters interface:</span></span> 
  ````
    public interface IQueryParameters: 
      Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
        System.Collections.Generic.IReadOnlyCollection <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
        { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
      system.Collections.Generic.IReadOnlyCollection<string> ExcludedAttributePaths 
      { get; }
      System.Collections.Generic.IReadOnlyCollection<string> RequestedAttributePaths 
      { get; }
      string SchemaIdentifier 
      { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IFilter
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IFilter AdditionalFilter 
          { get; set; }
        string AttributePath 
          { get; } 
        Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator FilterOperator 
          { get; }
        string ComparisonValue 
          { get; }
    }

    public enum Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator
    {
        Equals
    }
  ````
  <span data-ttu-id="e49e4-325">I följande exempel i en fråga för en användare med ett angivet värde för attributet externalId är värden argument som skickas till metoden frågan:</span><span class="sxs-lookup"><span data-stu-id="e49e4-325">In the following sample of a query for a user with a given value for the externalId attribute, values of the arguments passed to the Query method are:</span></span> 
  * <span data-ttu-id="e49e4-326">parametrar. AlternateFilters.Count: 1</span><span class="sxs-lookup"><span data-stu-id="e49e4-326">parameters.AlternateFilters.Count: 1</span></span>
  * <span data-ttu-id="e49e4-327">parametrar. AlternateFilters.ElementAt(0). AttributePath: ”externalId”</span><span class="sxs-lookup"><span data-stu-id="e49e4-327">parameters.AlternateFilters.ElementAt(0).AttributePath: "externalId"</span></span>
  * <span data-ttu-id="e49e4-328">parametrar. AlternateFilters.ElementAt(0). Jämförelseoperator: ComparisonOperator.Equals</span><span class="sxs-lookup"><span data-stu-id="e49e4-328">parameters.AlternateFilters.ElementAt(0).ComparisonOperator: ComparisonOperator.Equals</span></span>
  * <span data-ttu-id="e49e4-329">parametrar. AlternateFilter.ElementAt(0). ComparisonValue: ”jyoung”</span><span class="sxs-lookup"><span data-stu-id="e49e4-329">parameters.AlternateFilter.ElementAt(0).ComparisonValue: "jyoung"</span></span>
  * <span data-ttu-id="e49e4-330">correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin. Begärande-ID ”]</span><span class="sxs-lookup"><span data-stu-id="e49e4-330">correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin.RequestId"]</span></span> 

2. <span data-ttu-id="e49e4-331">Om svaret på en fråga till webbtjänsten för en användare med ett externalId-attributvärde som matchar mailNickname attributvärdet för en användare inte returnerar några användare, begäranden Azure Active Directory för att etablera tjänsten en användare som motsvarar det i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e49e4-331">If the response to a query to the web service for a user with an externalId attribute value that matches the mailNickname attribute value of a user does not return any users, then Azure Active Directory requests that the service provision a user corresponding to the one in Azure Active Directory.</span></span>  <span data-ttu-id="e49e4-332">Här är ett exempel på en sådan begäran:</span><span class="sxs-lookup"><span data-stu-id="e49e4-332">Here is an example of such a request:</span></span> 
  ````
    POST https://.../scim/Users HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas":
      [
        "urn:ietf:params:scim:schemas:core:2.0:User",
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0User"],
      "externalId":"jyoung",
      "userName":"jyoung",
      "active":true,
      "addresses":null,
      "displayName":"Joy Young",
      "emails": [
        {
          "type":"work",
          "value":"jyoung@Contoso.com",
          "primary":true}],
      "meta": {
        "resourceType":"User"},
       "name":{
        "familyName":"Young",
        "givenName":"Joy"},
      "phoneNumbers":null,
      "preferredLanguage":null,
      "title":null,
      "department":null,
      "manager":null}
  ````
  <span data-ttu-id="e49e4-333">Vanlig infrastruktur för språk-bibliotek som tillhandahålls av Microsoft för att implementera SCIM tjänster kan översätta begäran i ett anrop till metoden skapa av tjänstens providern.</span><span class="sxs-lookup"><span data-stu-id="e49e4-333">The Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services would translate that request into a call to the Create method of the service’s provider.</span></span>  <span data-ttu-id="e49e4-334">Create-metoden har den här signaturen:</span><span class="sxs-lookup"><span data-stu-id="e49e4-334">The Create method has this signature:</span></span> 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create(
      Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
      string correlationIdentifier);
  ````
  <span data-ttu-id="e49e4-335">Värdet för argumentet resurs är en instans av Microsoft.SystemForCrossDomainIdentityManagement i en begäran att etablera en användare.</span><span class="sxs-lookup"><span data-stu-id="e49e4-335">In a request to provision a user, the value of the resource argument is an instance of the Microsoft.SystemForCrossDomainIdentityManagement.</span></span> <span data-ttu-id="e49e4-336">Core2EnterpriseUser klass som definierats i Microsoft.SystemForCrossDomainIdentityManagement.Schemas-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="e49e4-336">Core2EnterpriseUser class, defined in the Microsoft.SystemForCrossDomainIdentityManagement.Schemas library.</span></span>  <span data-ttu-id="e49e4-337">Om begäran om att etablera användaren lyckas, förväntas implementeringen av metoden för att returnera en instans av Microsoft.SystemForCrossDomainIdentityManagement.</span><span class="sxs-lookup"><span data-stu-id="e49e4-337">If the request to provision the user succeeds, then the implementation of the method is expected to return an instance of the Microsoft.SystemForCrossDomainIdentityManagement.</span></span> <span data-ttu-id="e49e4-338">Core2EnterpriseUser klass med värdet för egenskapen ID angetts till den unika identifieraren för den nyetablerade användaren.</span><span class="sxs-lookup"><span data-stu-id="e49e4-338">Core2EnterpriseUser class, with the value of the Identifier property set to the unique identifier of the newly provisioned user.</span></span>  

3. <span data-ttu-id="e49e4-339">Om du vill uppdatera en användare finns i en identity-butik fronted genom en SCIM fortsätter Azure Active Directory genom att begära det aktuella tillståndet för den användaren från tjänsten med en begäran som:</span><span class="sxs-lookup"><span data-stu-id="e49e4-339">To update a user known to exist in an identity store fronted by an SCIM, Azure Active Directory proceeds by requesting the current state of that user from the service with a request such as:</span></span> 
  ````
    GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  <span data-ttu-id="e49e4-340">I en tjänst som skapats med vanlig infrastruktur för språk-bibliotek som tillhandahålls av Microsoft för att implementera SCIM tjänster översätts begäran till ett anrop till metoden hämta i service provider.</span><span class="sxs-lookup"><span data-stu-id="e49e4-340">In a service built using the Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, the request is translated into a call to the Retrieve method of the service’s provider.</span></span>  <span data-ttu-id="e49e4-341">Här är signaturen för hämta-metoden:</span><span class="sxs-lookup"><span data-stu-id="e49e4-341">Here is the signature of the Retrieve method:</span></span> 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource and 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
    // are defined in Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> 
       Retrieve(
         Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
           parameters, 
           string correlationIdentifier);

    public interface 
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters:   
        IRetrievalParameters
        {
          Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
            ResourceIdentifier 
              { get; }
    }
    public interface Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier
    {
        string Identifier 
          { get; set; }
        string Microsoft.SystemForCrossDomainIdentityManagement.SchemaIdentifier 
          { get; set; }
    }
  ````
  <span data-ttu-id="e49e4-342">I en begäran om att hämta aktuell status för en användare exempelvis är värdena för egenskaperna för objektet som tillhandahålls som värde för argumentet parametrar följande:</span><span class="sxs-lookup"><span data-stu-id="e49e4-342">In the example of a request to retrieve the current state of a user, the values of the properties of the object provided as the value of the parameters argument are as follows:</span></span> 
  
  * <span data-ttu-id="e49e4-343">ID: ”54D382A4-2050-4C03-94D1-E769F1D15682”</span><span class="sxs-lookup"><span data-stu-id="e49e4-343">Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span></span>
  * <span data-ttu-id="e49e4-344">SchemaIdentifier: ”urn: ietf:params:scim:schemas:extension:enterprise:2.0:User”</span><span class="sxs-lookup"><span data-stu-id="e49e4-344">SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"</span></span>

4. <span data-ttu-id="e49e4-345">Om ett referensattribut är uppdateras Azure Active Directory frågar tjänsten för att fastställa huruvida det aktuella värdet för referensattributet i Identitetslagret fronted av tjänsten redan överensstämmer med värdet för attributet i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e49e4-345">If a reference attribute is to be updated, then Azure Active Directory queries the service to determine whether or not the current value of the reference attribute in the identity store fronted by the service already matches the value of that attribute in Azure Active Directory.</span></span> <span data-ttu-id="e49e4-346">För användare är det enda attributet som efterfrågas det aktuella värdet på det här sättet manager-attribut.</span><span class="sxs-lookup"><span data-stu-id="e49e4-346">For users, the only attribute of which the current value is queried in this way is the manager attribute.</span></span> <span data-ttu-id="e49e4-347">Här är ett exempel på en begäran om att avgöra om attributet manager för en viss användare-objektet har ett visst värde:</span><span class="sxs-lookup"><span data-stu-id="e49e4-347">Here is an example of a request to determine whether the manager attribute of a particular user object currently has a certain value:</span></span> 
  ````
    GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq 2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
    Authorization: Bearer ...
  ````
  <span data-ttu-id="e49e4-348">Värdet för Frågeparametern attribut-id, innebär att om ett användarobjekt finns som uppfyller de uttryck som värde för Filterparametern-frågan och sedan tjänsten förväntas att svara med en urn: ietf:params:scim:schemas:core:2.0:User eller urn: ietf:params:scim:schemas:extension:enterprise:2.0:User resurs, inklusive endast värdet för den resurs-id-attribut.</span><span class="sxs-lookup"><span data-stu-id="e49e4-348">The value of the attributes query parameter, id, signifies that if a user object exists that satisfies the expression provided as the value of the filter query parameter, then the service is expected to respond with a urn:ietf:params:scim:schemas:core:2.0:User or urn:ietf:params:scim:schemas:extension:enterprise:2.0:User resource, including only the value of that resource’s id attribute.</span></span>  <span data-ttu-id="e49e4-349">Värdet för den **id** attributet är känt som begäranden.</span><span class="sxs-lookup"><span data-stu-id="e49e4-349">The value of the **id** attribute is known to the requestor.</span></span> <span data-ttu-id="e49e4-350">Den ingår i värdet för Frågeparametern filter; Syftet med ber om den är faktiskt att begära en minimal representation av en resurs som uppfyller filteruttrycket som en indikation på om det finns sådana objekt.</span><span class="sxs-lookup"><span data-stu-id="e49e4-350">It is included in the value of the filter query parameter; the purpose of asking for it is actually to request a minimal representation of a resource that satisfying the filter expression as an indication of whether or not any such object exists.</span></span>   

  <span data-ttu-id="e49e4-351">Om tjänsten har skapats med vanlig infrastruktur för språk-bibliotek som tillhandahålls av Microsoft för att implementera SCIM tjänster, översätts begäran till ett anrop till metoden fråga i den tjänstleverantören.</span><span class="sxs-lookup"><span data-stu-id="e49e4-351">If the service was built using the Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, then the request is translated into a call to the Query method of the service’s provider.</span></span> <span data-ttu-id="e49e4-352">Värdet för egenskaperna för objektet som tillhandahålls som värde för argumentet parametrar är följande:</span><span class="sxs-lookup"><span data-stu-id="e49e4-352">The value of the properties of the object provided as the value of the parameters argument are as follows:</span></span> 
  
  * <span data-ttu-id="e49e4-353">parametrar. AlternateFilters.Count: 2</span><span class="sxs-lookup"><span data-stu-id="e49e4-353">parameters.AlternateFilters.Count: 2</span></span>
  * <span data-ttu-id="e49e4-354">parametrar. AlternateFilters.ElementAt(x). AttributePath: ”id”</span><span class="sxs-lookup"><span data-stu-id="e49e4-354">parameters.AlternateFilters.ElementAt(x).AttributePath: "id"</span></span>
  * <span data-ttu-id="e49e4-355">parametrar. AlternateFilters.ElementAt(x). Jämförelseoperator: ComparisonOperator.Equals</span><span class="sxs-lookup"><span data-stu-id="e49e4-355">parameters.AlternateFilters.ElementAt(x).ComparisonOperator: ComparisonOperator.Equals</span></span>
  * <span data-ttu-id="e49e4-356">parametrar. AlternateFilter.ElementAt(x). ComparisonValue: ”54D382A4-2050-4C03-94D1-E769F1D15682”</span><span class="sxs-lookup"><span data-stu-id="e49e4-356">parameters.AlternateFilter.ElementAt(x).ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"</span></span>
  * <span data-ttu-id="e49e4-357">parametrar. AlternateFilters.ElementAt(y). AttributePath: ”manager”</span><span class="sxs-lookup"><span data-stu-id="e49e4-357">parameters.AlternateFilters.ElementAt(y).AttributePath: "manager"</span></span>
  * <span data-ttu-id="e49e4-358">parametrar. AlternateFilters.ElementAt(y). Jämförelseoperator: ComparisonOperator.Equals</span><span class="sxs-lookup"><span data-stu-id="e49e4-358">parameters.AlternateFilters.ElementAt(y).ComparisonOperator: ComparisonOperator.Equals</span></span>
  * <span data-ttu-id="e49e4-359">parametrar. AlternateFilter.ElementAt(y). ComparisonValue: ”2819c223-7f76-453a-919d-413861904646”</span><span class="sxs-lookup"><span data-stu-id="e49e4-359">parameters.AlternateFilter.ElementAt(y).ComparisonValue: "2819c223-7f76-453a-919d-413861904646"</span></span>
  * <span data-ttu-id="e49e4-360">parametrar. RequestedAttributePaths.ElementAt(0): ”id”</span><span class="sxs-lookup"><span data-stu-id="e49e4-360">parameters.RequestedAttributePaths.ElementAt(0): "id"</span></span>
  * <span data-ttu-id="e49e4-361">parametrar. SchemaIdentifier: ”urn: ietf:params:scim:schemas:extension:enterprise:2.0:User”</span><span class="sxs-lookup"><span data-stu-id="e49e4-361">parameters.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"</span></span>

  <span data-ttu-id="e49e4-362">Här kan värdet för indexet x kan vara 0 och värdet för y indexet kan vara 1, eller värdet för x kan vara 1 och värdet för y kan vara 0, beroende på ordningen på uttryck för filter Frågeparametern.</span><span class="sxs-lookup"><span data-stu-id="e49e4-362">Here, the value of the index x may be 0 and the value of the index y may be 1, or the value of x may be 1 and the value of y may be 0, depending on the order of the expressions of the filter query parameter.</span></span>   

5. <span data-ttu-id="e49e4-363">Här är ett exempel på en begäran från Azure Active Directory till en SCIM-tjänsten för att uppdatera en användare:</span><span class="sxs-lookup"><span data-stu-id="e49e4-363">Here is an example of a request from Azure Active Directory to an SCIM service to update a user:</span></span> 
  ````
    PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas": 
      [
        "urn:ietf:params:scim:api:messages:2.0:PatchOp"],
      "Operations":
      [
        {
          "op":"Add",
          "path":"manager",
          "value":
            [
              {
                "$ref":"http://.../scim/Users/2819c223-7f76-453a-919d-413861904646",
                "value":"2819c223-7f76-453a-919d-413861904646"}]}]}
  ````
  <span data-ttu-id="e49e4-364">Infrastruktur för Microsoft Common Language-bibliotek för att implementera SCIM tjänster kan översätta begäran i ett anrop till metoden Update på tjänstens provider.</span><span class="sxs-lookup"><span data-stu-id="e49e4-364">The Microsoft Common Language Infrastructure libraries for implementing SCIM services would translate the request into a call to the Update method of the service’s provider.</span></span> <span data-ttu-id="e49e4-365">Här är signaturen för metoden Update:</span><span class="sxs-lookup"><span data-stu-id="e49e4-365">Here is the signature of the Update method:</span></span> 
  ````
    // System.Threading.Tasks.Tasks and 
    // System.Collections.Generic.IReadOnlyCollection<T>
    // are defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IPatch, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationName, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IPath and 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationValue 
    // are all defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 

    System.Threading.Tasks.Task Update(
      Microsoft.SystemForCrossDomainIdentityManagement.IPatch patch, 
      string correlationIdentifier);

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IPatch
    {
    Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase 
      PatchRequest 
        { get; set; }
    Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
      ResourceIdentifier 
        { get; set; }        
    }

    public class PatchRequest2: 
      Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase
    {
    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation> 
        Operations
        { get;}

    public void AddOperation(
      Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation operation);
    }

    public class PatchOperation
    {
    public Microsoft.SystemForCrossDomainIdentityManagement.OperationName 
      Name
      { get; set; }

    public Microsoft.SystemForCrossDomainIdentityManagement.IPath 
      Path
      { get; set; }

    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.OperationValue> Value
      { get; }

    public void AddValue(
      Microsoft.SystemForCrossDomainIdentityManagement.OperationValue value);
    }

    public enum OperationName
    {
      Add,
      Remove,
      Replace
    }

    public interface IPath
    {
      string AttributePath { get; }
      System.Collections.Generic.IReadOnlyCollection<IFilter> SubAttributes { get; }
      Microsoft.SystemForCrossDomainIdentityManagement.IPath ValuePath { get; }
    }

    public class OperationValue
    {
      public string Reference
      { get; set; }

      public string Value
      { get; set; }
    }
  ````
    <span data-ttu-id="e49e4-366">I en begäran om att uppdatera en användare exempelvis har det angivna objektet som värde för argumentet korrigering egenskapsvärdena:</span><span class="sxs-lookup"><span data-stu-id="e49e4-366">In the example of a request to update a user, the object provided as the value of the patch argument has these property values:</span></span> 
  
  * <span data-ttu-id="e49e4-367">ResourceIdentifier.Identifier: ”54D382A4-2050-4C03-94D1-E769F1D15682”</span><span class="sxs-lookup"><span data-stu-id="e49e4-367">ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span></span>
  * <span data-ttu-id="e49e4-368">ResourceIdentifier.SchemaIdentifier: ”urn: ietf:params:scim:schemas:extension:enterprise:2.0:User”</span><span class="sxs-lookup"><span data-stu-id="e49e4-368">ResourceIdentifier.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"</span></span>
  * <span data-ttu-id="e49e4-369">(PatchRequest som PatchRequest2). Operations.Count: 1</span><span class="sxs-lookup"><span data-stu-id="e49e4-369">(PatchRequest as PatchRequest2).Operations.Count: 1</span></span>
  * <span data-ttu-id="e49e4-370">(PatchRequest som PatchRequest2). Operations.ElementAt(0). OperationName: OperationName.Add</span><span class="sxs-lookup"><span data-stu-id="e49e4-370">(PatchRequest as PatchRequest2).Operations.ElementAt(0).OperationName: OperationName.Add</span></span>
  * <span data-ttu-id="e49e4-371">(PatchRequest som PatchRequest2). Operations.ElementAt(0). Path.AttributePath: ”manager”</span><span class="sxs-lookup"><span data-stu-id="e49e4-371">(PatchRequest as PatchRequest2).Operations.ElementAt(0).Path.AttributePath: "manager"</span></span>
  * <span data-ttu-id="e49e4-372">(PatchRequest som PatchRequest2). Operations.ElementAt(0). Value.Count: 1</span><span class="sxs-lookup"><span data-stu-id="e49e4-372">(PatchRequest as PatchRequest2).Operations.ElementAt(0).Value.Count: 1</span></span>
  * <span data-ttu-id="e49e4-373">(PatchRequest som PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Referens: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646</span><span class="sxs-lookup"><span data-stu-id="e49e4-373">(PatchRequest as PatchRequest2).Operations.ElementAt(0).Value.ElementAt(0).Reference: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646</span></span>
  * <span data-ttu-id="e49e4-374">(PatchRequest som PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Värde: 2819c223-7f76-453a-919d-413861904646</span><span class="sxs-lookup"><span data-stu-id="e49e4-374">(PatchRequest as PatchRequest2).Operations.ElementAt(0).Value.ElementAt(0).Value: 2819c223-7f76-453a-919d-413861904646</span></span>

6. <span data-ttu-id="e49e4-375">Om du vill avetablera en användare från en Identitetslagret fronted av SCIM-tjänsten, skickar Azure AD en begäran som:</span><span class="sxs-lookup"><span data-stu-id="e49e4-375">To de-provision a user from an identity store fronted by an SCIM service, Azure AD sends a request such as:</span></span> 
  ````
    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  <span data-ttu-id="e49e4-376">Om tjänsten har skapats med vanlig infrastruktur för språk-bibliotek som tillhandahålls av Microsoft för att implementera SCIM tjänster, översätts begäran till ett anrop till metoden Delete av tjänstens providern.</span><span class="sxs-lookup"><span data-stu-id="e49e4-376">If the service was built using the Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, then the request is translated into a call to the Delete method of the service’s provider.</span></span>   <span data-ttu-id="e49e4-377">Den här metoden har den här signaturen:</span><span class="sxs-lookup"><span data-stu-id="e49e4-377">That method has this signature:</span></span> 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
  ````
  <span data-ttu-id="e49e4-378">Objektet som tillhandahålls som värde för argumentet resourceIdentifier har egenskapsvärdena i exemplet med en begäran om att avetablera en användare:</span><span class="sxs-lookup"><span data-stu-id="e49e4-378">The object provided as the value of the resourceIdentifier argument has these property values in the example of a request to de-provision a user:</span></span> 
  
  * <span data-ttu-id="e49e4-379">ResourceIdentifier.Identifier: ”54D382A4-2050-4C03-94D1-E769F1D15682”</span><span class="sxs-lookup"><span data-stu-id="e49e4-379">ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span></span>
  * <span data-ttu-id="e49e4-380">ResourceIdentifier.SchemaIdentifier: ”urn: ietf:params:scim:schemas:extension:enterprise:2.0:User”</span><span class="sxs-lookup"><span data-stu-id="e49e4-380">ResourceIdentifier.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"</span></span>

## <a name="group-provisioning-and-de-provisioning"></a><span data-ttu-id="e49e4-381">Gruppen etablering och avetablering</span><span class="sxs-lookup"><span data-stu-id="e49e4-381">Group provisioning and de-provisioning</span></span>
<span data-ttu-id="e49e4-382">Följande bild visar meddelanden att Azure AcD skickar till en tjänst för SCIM att hantera livscykeln för en grupp i en annan identitet store.</span><span class="sxs-lookup"><span data-stu-id="e49e4-382">The following illustration shows the messages that Azure AcD sends to a SCIM service to manage the lifecycle of a group in another identity store.</span></span>  <span data-ttu-id="e49e4-383">Dessa meddelanden skiljer sig från de meddelanden som hör till användare på tre sätt:</span><span class="sxs-lookup"><span data-stu-id="e49e4-383">Those messages differ from the messages pertaining to users in three ways:</span></span> 

* <span data-ttu-id="e49e4-384">Schemat för en gruppresurs identifieras som http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.</span><span class="sxs-lookup"><span data-stu-id="e49e4-384">The schema of a group resource is identified as http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.</span></span>  
* <span data-ttu-id="e49e4-385">Begäranden att hämta grupper kräva att attributet medlemmar är som ska undantas från alla resurser som finns i svaret på begäran.</span><span class="sxs-lookup"><span data-stu-id="e49e4-385">Requests to retrieve groups stipulate that the members attribute is to be excluded from any resource provided in response to the request.</span></span>  
* <span data-ttu-id="e49e4-386">Förfrågningar att avgöra om ett referensattribut har ett visst värde är förfrågningar om attributet medlemmar.</span><span class="sxs-lookup"><span data-stu-id="e49e4-386">Requests to determine whether a reference attribute has a certain value are requests about the members attribute.</span></span>  

<span data-ttu-id="e49e4-387">![][5]
*Bild 6: Grupp etablering och avetablering sekvens*</span><span class="sxs-lookup"><span data-stu-id="e49e4-387">![][5]
*Figure 6: Group provisioning and de-provisioning sequence*</span></span>

## <a name="related-articles"></a><span data-ttu-id="e49e4-388">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="e49e4-388">Related articles</span></span>
* [<span data-ttu-id="e49e4-389">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e49e4-389">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="e49e4-390">Automatisera användaren etablering/avetablering för SaaS-appar</span><span class="sxs-lookup"><span data-stu-id="e49e4-390">Automate User Provisioning/Deprovisioning to SaaS Apps</span></span>](active-directory-saas-app-provisioning.md)
* [<span data-ttu-id="e49e4-391">Anpassa attributmappning för Användaretablering</span><span class="sxs-lookup"><span data-stu-id="e49e4-391">Customizing Attribute Mappings for User Provisioning</span></span>](active-directory-saas-customizing-attribute-mappings.md)
* [<span data-ttu-id="e49e4-392">Skriva uttryck för attributmappning</span><span class="sxs-lookup"><span data-stu-id="e49e4-392">Writing Expressions for Attribute Mappings</span></span>](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [<span data-ttu-id="e49e4-393">Omfångsfilter för Användaretablering</span><span class="sxs-lookup"><span data-stu-id="e49e4-393">Scoping Filters for User Provisioning</span></span>](active-directory-saas-scoping-filters.md)
* [<span data-ttu-id="e49e4-394">Kontot etablering meddelanden</span><span class="sxs-lookup"><span data-stu-id="e49e4-394">Account Provisioning Notifications</span></span>](active-directory-saas-account-provisioning-notifications.md)
* [<span data-ttu-id="e49e4-395">Lista över självstudier om hur du integrerar SaaS-appar</span><span class="sxs-lookup"><span data-stu-id="e49e4-395">List of Tutorials on How to Integrate SaaS Apps</span></span>](active-directory-saas-tutorial-list.md)

<!--Image references-->
[0]: ./media/active-directory-scim-provisioning/scim-figure-1.PNG
[1]: ./media/active-directory-scim-provisioning/scim-figure-2a.PNG
[2]: ./media/active-directory-scim-provisioning/scim-figure-2b.PNG
[3]: ./media/active-directory-scim-provisioning/scim-figure-3.PNG
[4]: ./media/active-directory-scim-provisioning/scim-figure-4.PNG
[5]: ./media/active-directory-scim-provisioning/scim-figure-5.PNG
