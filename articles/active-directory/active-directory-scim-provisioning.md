---
title: "aaaUsing System för Identitetshantering domäner automatiskt etablera användare och grupper från Azure Active Directory tooapplications | Microsoft Docs"
description: "Azure Active Directory kan automatiskt etablera användare och grupper tooany program eller identitet butik som är fronted av en webbtjänst med hello-gränssnitt som definierats i hello SCIM protokollspecifikation"
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
ms.openlocfilehash: 43045c97e68d0d22db598dcb5ec23481c4e97718
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-system-for-cross-domain-identity-management-tooautomatically-provision-users-and-groups-from-azure-active-directory-tooapplications"></a><span data-ttu-id="f5fc0-103">Med hjälp av System för Identitetshantering i domänerna tooautomatically etablera användare och grupper från Azure Active Directory tooapplications</span><span class="sxs-lookup"><span data-stu-id="f5fc0-103">Using System for Cross-Domain Identity Management tooautomatically provision users and groups from Azure Active Directory tooapplications</span></span>

## <a name="overview"></a><span data-ttu-id="f5fc0-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="f5fc0-104">Overview</span></span>
<span data-ttu-id="f5fc0-105">Azure Active Directory (AD Azure) automatiskt kan etablera användare och grupper tooany program eller identitet butik som är fronted av en webbtjänst med hello-gränssnitt som definierats i hello [System för domäner Identity Management (SCIM) 2.0 specifikation för protokollet](https://tools.ietf.org/html/draft-ietf-scim-api-19).</span><span class="sxs-lookup"><span data-stu-id="f5fc0-105">Azure Active Directory (Azure AD) can automatically provision users and groups tooany application or identity store that is fronted by a web service with hello interface defined in hello [System for Cross-Domain Identity Management (SCIM) 2.0 protocol specification](https://tools.ietf.org/html/draft-ietf-scim-api-19).</span></span> <span data-ttu-id="f5fc0-106">Azure Active Directory kan skicka begäranden toocreate, ändra eller ta bort tilldelade användare och grupper toohello webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-106">Azure Active Directory can send requests toocreate, modify, or delete assigned users and groups toohello web service.</span></span> <span data-ttu-id="f5fc0-107">hello-webbtjänsten kan översätta dessa begäranden till åtgärder på hello mål identitet store.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-107">hello web service can then translate those requests into operations on hello target identity store.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="f5fc0-108">Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-108">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span> 



<span data-ttu-id="f5fc0-109">![][0]
*Bild 1: Etablering från Azure Active Directory tooan identitet store via en webbtjänst*</span><span class="sxs-lookup"><span data-stu-id="f5fc0-109">![][0]
*Figure 1: Provisioning from Azure Active Directory tooan identity store via a web service*</span></span>

<span data-ttu-id="f5fc0-110">Den här funktionen kan användas tillsammans med hello ”ta med din egen app” funktioner i Azure AD tooenable enkel inloggning och automatisk användaretablering för program som tillhandahåller eller fronted av en SCIM-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-110">This capability can be used in conjunction with hello “bring your own app” capability in Azure AD tooenable single sign-on and automatic user provisioning for applications that provide or are fronted by a SCIM web service.</span></span>

<span data-ttu-id="f5fc0-111">Det finns två användningsfall för att använda SCIM i Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-111">There are two use cases for using SCIM in Azure Active Directory:</span></span>

* <span data-ttu-id="f5fc0-112">**Etablering av användare och grupper tooapplications som stöder SCIM** program som stöder SCIM 2.0 och använder OAuth ägar-token för autentisering fungerar med Azure AD utan konfiguration.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-112">**Provisioning users and groups tooapplications that support SCIM** Applications that support SCIM 2.0 and use OAuth bearer tokens for authentication works with Azure AD without configuration.</span></span>
* <span data-ttu-id="f5fc0-113">**Skapa din egen lösning för etablering för program som stöder andra API-baserad etablering** för icke-SCIM program, kan du skapa en SCIM slutpunkt tootranslate mellan hello Azure AD SCIM slutpunkten och eventuella API hello programmet stöder för användaretablering.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-113">**Build your own provisioning solution for applications that support other API-based provisioning** For non-SCIM applications, you can create a SCIM endpoint tootranslate between hello Azure AD SCIM endpoint and any API hello application supports for user provisioning.</span></span> <span data-ttu-id="f5fc0-114">toohelp som du utvecklar en slutpunkt för SCIM vi ger Common Language Infrastructure (CLI) bibliotek och kodexempel som visar hur toodo ger en SCIM slutpunkt och översätta SCIM meddelanden.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-114">toohelp you develop a SCIM endpoint, we provide Common Language Infrastructure (CLI) libraries along with code samples that show you how toodo provide a SCIM endpoint and translate SCIM messages.</span></span>  

## <a name="provisioning-users-and-groups-tooapplications-that-support-scim"></a><span data-ttu-id="f5fc0-115">Etablering av användare och grupper tooapplications som stöder SCIM</span><span class="sxs-lookup"><span data-stu-id="f5fc0-115">Provisioning users and groups tooapplications that support SCIM</span></span>
<span data-ttu-id="f5fc0-116">Azure AD kan vara konfigurerade tooautomatically etablera tilldelade användare och grupper tooapplications som implementerar en [System för domäner Identity Management 2 (SCIM)](https://tools.ietf.org/html/draft-ietf-scim-api-19) webbtjänsten och acceptera OAuth ägar-token för autentisering .</span><span class="sxs-lookup"><span data-stu-id="f5fc0-116">Azure AD can be configured tooautomatically provision assigned users and groups tooapplications that implement a [System for Cross-domain Identity Management 2 (SCIM)](https://tools.ietf.org/html/draft-ietf-scim-api-19) web service and accept OAuth bearer tokens for authentication.</span></span> <span data-ttu-id="f5fc0-117">Inom hello SCIM 2.0-specifikationen måste program uppfylla följande villkor:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-117">Within hello SCIM 2.0 specification, applications must meet these requirements:</span></span>

* <span data-ttu-id="f5fc0-118">Har stöd för att skapa användare och/eller grupper, enligt avsnittet 3.3 i hello SCIM-protokollet.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-118">Supports creating users and/or groups, as per section 3.3 of hello SCIM protocol.</span></span>  
* <span data-ttu-id="f5fc0-119">Har stöd för ändring av användare och/eller grupper med patch-förfrågningar enligt avsnitt 3.5.2 i hello SCIM-protokollet.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-119">Supports modifying users and/or groups with patch requests as per section 3.5.2 of hello SCIM protocol.</span></span>  
* <span data-ttu-id="f5fc0-120">Har stöd för hämtning av en känd resurs enligt punkt 3.4.1 i hello SCIM protokoll.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-120">Supports retrieving a known resource as per section 3.4.1 of hello SCIM protocol.</span></span>  
* <span data-ttu-id="f5fc0-121">Har stöd för frågor till användare och/eller grupper, enligt avsnittet 3.4.2 hello SCIM-protokollet.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-121">Supports querying users and/or groups, as per section 3.4.2 of hello SCIM protocol.</span></span>  <span data-ttu-id="f5fc0-122">Som standard tillfrågas användare av externalId och grupper är efterfrågas av displayName.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-122">By default, users are queried by externalId and groups are queried by displayName.</span></span>  
* <span data-ttu-id="f5fc0-123">Har stöd för frågor till användaren genom ID och manager enligt punkt 3.4.2 i hello SCIM-protokollet.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-123">Supports querying user by ID and by manager as per section 3.4.2 of hello SCIM protocol.</span></span>  
* <span data-ttu-id="f5fc0-124">Har stöd för frågor grupper efter ID och av medlem enligt punkt 3.4.2 i hello SCIM-protokollet.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-124">Supports querying groups by ID and by member as per section 3.4.2 of hello SCIM protocol.</span></span>  
* <span data-ttu-id="f5fc0-125">Accepterar OAuth ägar-token för auktorisering enligt avsnittet 2.1 av hello SCIM-protokollet.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-125">Accepts OAuth bearer tokens for authorization as per section 2.1 of hello SCIM protocol.</span></span>

<span data-ttu-id="f5fc0-126">Kontrollera med leverantören av program eller program leverantörens dokumentation för rapporter för kompatibilitet med dessa krav.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-126">Check with your application provider, or your application provider's documentation for statements of compatibility with these requirements.</span></span>

### <a name="getting-started"></a><span data-ttu-id="f5fc0-127">Komma igång</span><span class="sxs-lookup"><span data-stu-id="f5fc0-127">Getting started</span></span>
<span data-ttu-id="f5fc0-128">Program som stöder hello SCIM profil beskrivs i den här artikeln kan vara anslutna tooAzure Active Directory funktionen hello ”icke-galleriet programmet” hello Azure AD application Gallery.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-128">Applications that support hello SCIM profile described in this article can be connected tooAzure Active Directory using hello "non-gallery application" feature in hello Azure AD application gallery.</span></span> <span data-ttu-id="f5fc0-129">När ansluten, Azure AD körs synkroniseringsprocessen var tjugonde minut där frågor till hello programmets SCIM slutpunkt för tilldelade användare och grupper och skapar eller ändrar dem enligt toohello tilldelningsinformation.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-129">Once connected, Azure AD runs a synchronization process every 20 minutes where it queries hello application's SCIM endpoint for assigned users and groups, and creates or modifies them according toohello assignment details.</span></span>

<span data-ttu-id="f5fc0-130">**tooconnect ett program som stöder SCIM:**</span><span class="sxs-lookup"><span data-stu-id="f5fc0-130">**tooconnect an application that supports SCIM:**</span></span>

1. <span data-ttu-id="f5fc0-131">Logga in för[hello Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f5fc0-131">Sign in too[hello Azure portal](https://portal.azure.com).</span></span> 
2. <span data-ttu-id="f5fc0-132">Bläddra för ** Azure Active Directory > företagsprogram och välj **nytt program > alla > icke-galleriet programmet**.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-132">Browse too**Azure Active Directory > Enterprise Applications, and select **New application > All > Non-gallery application**.</span></span>
3. <span data-ttu-id="f5fc0-133">Ange ett namn för ditt program och klicka på **Lägg till** ikonen toocreate ett app-objekt.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-133">Enter a name for your application, and click **Add** icon toocreate an app object.</span></span>
    
  <span data-ttu-id="f5fc0-134">![][1]
  *Bild 2: Azure AD application gallery*</span><span class="sxs-lookup"><span data-stu-id="f5fc0-134">![][1]
*Figure 2: Azure AD application gallery*</span></span>
    
4. <span data-ttu-id="f5fc0-135">Välj hello i resulterande hello-skärmen **etablering** fliken i hello vänstra kolumnen.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-135">In hello resulting screen, select hello **Provisioning** tab in hello left column.</span></span>
5. <span data-ttu-id="f5fc0-136">I hello **etablering läge** väljer du **automatisk**.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-136">In hello **Provisioning Mode** menu, select **Automatic**.</span></span>
    
  <span data-ttu-id="f5fc0-137">![][2]
  *Bild 3: Konfigurera allokering i hello Azure-portalen*</span><span class="sxs-lookup"><span data-stu-id="f5fc0-137">![][2]
*Figure 3: Configuring provisioning in hello Azure portal*</span></span>
    
6. <span data-ttu-id="f5fc0-138">I hello **klient URL** anger hello URL för hello programmets SCIM slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-138">In hello **Tenant URL** field, enter hello URL of hello application's SCIM endpoint.</span></span> <span data-ttu-id="f5fc0-139">Exempel: https://api.contoso.com/scim/v2/</span><span class="sxs-lookup"><span data-stu-id="f5fc0-139">Example: https://api.contoso.com/scim/v2/</span></span>
7. <span data-ttu-id="f5fc0-140">Om hello SCIM slutpunkten kräver en OAuth ägar-token från en utfärdare än Azure AD och sedan kopiera hello krävs OAuth ägar-token i hello valfria **hemlighet Token** fältet.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-140">If hello SCIM endpoint requires an OAuth bearer token from an issuer other than Azure AD, then copy hello required OAuth bearer token into hello optional **Secret Token** field.</span></span> <span data-ttu-id="f5fc0-141">Om det här fältet är tomt, med en OAuth ägar-token som utfärdas från Azure AD med varje begäran med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-141">If this field is left blank, then Azure AD included an OAuth bearer token issued from Azure AD with each request.</span></span> <span data-ttu-id="f5fc0-142">Appar som använder Azure AD som en identitetsleverantör kan verifiera den här Azure AD-utfärdade token.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-142">Apps that use Azure AD as an identity provider can validate this Azure AD -issued token.</span></span>
8. <span data-ttu-id="f5fc0-143">Klicka på hello **Testanslutningen** knappen toohave Azure Active Directory försök tooconnect toohello SCIM slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-143">Click hello **Test Connection** button toohave Azure Active Directory attempt tooconnect toohello SCIM endpoint.</span></span> <span data-ttu-id="f5fc0-144">Om hello försök misslyckas, visas information om felet.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-144">If hello attempts fail, error information is displayed.</span></span>  
9. <span data-ttu-id="f5fc0-145">Om det lyckas hello försök tooconnect toohello program, klicka sedan på **spara** toosave hello-administratörsautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-145">If hello attempts tooconnect toohello application succeed, then click **Save** toosave hello admin credentials.</span></span>
10. <span data-ttu-id="f5fc0-146">I hello **mappningar** avsnittet finns det två valbar uppsättningar attributmappning: en för användarobjekt och en för gruppobjekt.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-146">In hello **Mappings** section, there are two selectable sets of attribute mappings: one for user objects and one for group objects.</span></span> <span data-ttu-id="f5fc0-147">Markera varje en tooreview hello attribut som synkroniseras från Azure Active Directory tooyour app.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-147">Select each one tooreview hello attributes that are synchronized from Azure Active Directory tooyour app.</span></span> <span data-ttu-id="f5fc0-148">Hej attribut som valts som **matchande** egenskaper är används toomatch hello användare och grupper i din app för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-148">hello attributes selected as **Matching** properties are used toomatch hello users and groups in your app for update operations.</span></span> <span data-ttu-id="f5fc0-149">Välj hello spara knappen toocommit ändringar.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-149">Select hello Save button toocommit any changes.</span></span>

    >[!NOTE]
    ><span data-ttu-id="f5fc0-150">Alternativt kan du inaktivera synkronisering av gruppobjekt genom att inaktivera hello ”grupper” mappning.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-150">You can optionally disable syncing of group objects by disabling hello "groups" mapping.</span></span> 

11. <span data-ttu-id="f5fc0-151">Under **inställningar**, hello **omfång** fältet definierar vilka användare som är och eller grupper som ska synkroniseras.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-151">Under **Settings**, hello **Scope** field defines which users and or groups are synchronized.</span></span> <span data-ttu-id="f5fc0-152">Att välja ”Sync endast har tilldelats användare och grupper” (rekommenderas) kommer bara synkronisera användare och grupper som tilldelas i hello **användare och grupper** fliken.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-152">Selecting "Sync only assigned users and groups" (recommended) will only sync users and groups assigned in hello **Users and groups** tab.</span></span>
12. <span data-ttu-id="f5fc0-153">När konfigurationen är klar kan du ändra hello **Status för etablering** för**på**.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-153">Once your configuration is complete, change hello **Provisioning Status** too**On**.</span></span>
13. <span data-ttu-id="f5fc0-154">Klicka på **spara** toostart hello etablering Azure AD-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-154">Click **Save** toostart hello Azure AD provisioning service.</span></span> 
14. <span data-ttu-id="f5fc0-155">Om synkroniserar endast tilldelas användare och grupper (rekommenderas), vara säker på att tooselect hello **användare och grupper** och tilldelar hello användare och/eller grupper som du vill toosync.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-155">If syncing only assigned users and groups (recommended), be sure tooselect hello **Users and groups** tab and assign hello users and/or groups you wish toosync.</span></span>

<span data-ttu-id="f5fc0-156">När hello inledande synkroniseringen har startat, kan du använda hello **granskningsloggar** fliken toomonitor pågår, som visar alla åtgärder som utförs av hello etableras på din app.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-156">Once hello initial synchronization has started, you can use hello **Audit logs** tab toomonitor progress, which shows all actions performed by hello provisioning service on your app.</span></span> <span data-ttu-id="f5fc0-157">Mer information om hur tooread hello Azure AD-etablering loggar finns [rapportering om automatisk konto användaretablering](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="f5fc0-157">For more information on how tooread hello Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>

>[!NOTE]
><span data-ttu-id="f5fc0-158">hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello-tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-158">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> 


## <a name="building-your-own-provisioning-solution-for-any-application"></a><span data-ttu-id="f5fc0-159">Skapa din egen lösning för etablering för alla program</span><span class="sxs-lookup"><span data-stu-id="f5fc0-159">Building your own provisioning solution for any application</span></span>
<span data-ttu-id="f5fc0-160">Du kan aktivera enkel inloggning och automatisk användaretablering för nästan alla program som innehåller en REST- eller SOAP användaretablering API genom att skapa en SCIM-webbtjänst som samverkar med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-160">By creating a SCIM web service that interfaces with Azure Active Directory, you can enable single sign-on and automatic user provisioning for virtually any application that provides a REST or SOAP user provisioning API.</span></span>

<span data-ttu-id="f5fc0-161">Här är hur det fungerar:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-161">Here’s how it works:</span></span>

1. <span data-ttu-id="f5fc0-162">Azure AD innehåller ett vanligt språk infrastrukturbibliotek med namnet [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/).</span><span class="sxs-lookup"><span data-stu-id="f5fc0-162">Azure AD provides a common language infrastructure library named [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/).</span></span> <span data-ttu-id="f5fc0-163">Systemintegrerare och utvecklare kan använda det här biblioteket toocreate och distribuera en SCIM-baserade webbtjänstslutpunkt kan ansluta Azure AD tooany programmets identitet store.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-163">System integrators and developers can use this library toocreate and deploy a SCIM-based web service endpoint capable of connecting Azure AD tooany application’s identity store.</span></span>
2. <span data-ttu-id="f5fc0-164">Mappningar implementeras i hello web service toomap hello standardiserade användaren schemat toohello användarschema och protokoll som krävs av hello program.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-164">Mappings are implemented in hello web service toomap hello standardized user schema toohello user schema and protocol required by hello application.</span></span>
3. <span data-ttu-id="f5fc0-165">hello slutpunkts-URL är registrerad i Azure AD som en del av ett anpassat program i hello programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-165">hello endpoint URL is registered in Azure AD as part of a custom application in hello application gallery.</span></span>
4. <span data-ttu-id="f5fc0-166">Användare och grupper är tilldelade toothis program i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-166">Users and groups are assigned toothis application in Azure AD.</span></span> <span data-ttu-id="f5fc0-167">Vid tilldelning av placeras de i en kö toobe synkroniseras toohello målprogrammet.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-167">Upon assignment, they are put into a queue toobe synchronized toohello target application.</span></span> <span data-ttu-id="f5fc0-168">hello synkroniseringsprocessen hantering hello kön körs var tjugonde minut.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-168">hello synchronization process handling hello queue runs every 20 minutes.</span></span>

### <a name="code-samples"></a><span data-ttu-id="f5fc0-169">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="f5fc0-169">Code Samples</span></span>
<span data-ttu-id="f5fc0-170">toomake detta bearbeta lättare, en uppsättning [kodexempel](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) tillhandahålls som skapar en slutpunkt för webbtjänsten SCIM och visa Automatisk etablering.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-170">toomake this process easier, a set of [code samples](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) are provided that create a SCIM web service endpoint and demonstrate automatic provisioning.</span></span> <span data-ttu-id="f5fc0-171">Ett exempel är av en provider som underhåller en fil med rader av kommaavgränsade värden som representerar användare och grupper.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-171">One sample is of a provider that maintains a file with rows of comma-separated values representing users and groups.</span></span>  <span data-ttu-id="f5fc0-172">hello andra är av en provider som körs på hello Amazon Web Services identitets- och åtkomsthantering service.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-172">hello other is of a provider that operates on hello Amazon Web Services Identity and Access Management service.</span></span>  

<span data-ttu-id="f5fc0-173">**Förutsättningar**</span><span class="sxs-lookup"><span data-stu-id="f5fc0-173">**Prerequisites**</span></span>

* <span data-ttu-id="f5fc0-174">Visual Studio 2013 eller senare</span><span class="sxs-lookup"><span data-stu-id="f5fc0-174">Visual Studio 2013 or later</span></span>
* [<span data-ttu-id="f5fc0-175">Azure SDK för .NET</span><span class="sxs-lookup"><span data-stu-id="f5fc0-175">Azure SDK for .NET</span></span>](https://azure.microsoft.com/downloads/)
* <span data-ttu-id="f5fc0-176">Windows-dator som har stöd för hello ASP.NET framework 4.5 toobe används som hello SCIM slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-176">Windows machine that supports hello ASP.NET framework 4.5 toobe used as hello SCIM endpoint.</span></span> <span data-ttu-id="f5fc0-177">Den här datorn måste vara tillgänglig från hello moln</span><span class="sxs-lookup"><span data-stu-id="f5fc0-177">This machine must be accessible from hello cloud</span></span>
* [<span data-ttu-id="f5fc0-178">En Azure-prenumeration med en utvärderingsversion eller licensierad version av Azure AD Premium</span><span class="sxs-lookup"><span data-stu-id="f5fc0-178">An Azure subscription with a trial or licensed version of Azure AD Premium</span></span>](https://azure.microsoft.com/services/active-directory/)
* <span data-ttu-id="f5fc0-179">hello Amazon AWS urvalet kräver bibliotek från hello [AWS Toolkit för Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html).</span><span class="sxs-lookup"><span data-stu-id="f5fc0-179">hello Amazon AWS sample requires libraries from hello [AWS Toolkit for Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html).</span></span> <span data-ttu-id="f5fc0-180">Mer information finns i hello viktigt fil som ingår i hello exempel.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-180">For more information, see hello README file included with hello sample.</span></span>

### <a name="getting-started"></a><span data-ttu-id="f5fc0-181">Komma igång</span><span class="sxs-lookup"><span data-stu-id="f5fc0-181">Getting Started</span></span>
<span data-ttu-id="f5fc0-182">hello enklaste sättet tooimplement en SCIM slutpunkten som kan acceptera etablering begäranden från Azure AD är toobuild och distribuera hello-kodexempel som matar ut filen med hello etablerade användare tooa fil med kommaavgränsade värden (CSV).</span><span class="sxs-lookup"><span data-stu-id="f5fc0-182">hello easiest way tooimplement a SCIM endpoint that can accept provisioning requests from Azure AD is toobuild and deploy hello code sample that outputs hello provisioned users tooa comma-separated value (CSV) file.</span></span>

<span data-ttu-id="f5fc0-183">**toocreate en exempel SCIM slutpunkt:**</span><span class="sxs-lookup"><span data-stu-id="f5fc0-183">**toocreate a sample SCIM endpoint:**</span></span>

1. <span data-ttu-id="f5fc0-184">Hämta hello exempel kodpaketet på [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)</span><span class="sxs-lookup"><span data-stu-id="f5fc0-184">Download hello code sample package at [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)</span></span>
2. <span data-ttu-id="f5fc0-185">Packa hello paketet och placera den på din Windows-dator på en plats, till exempel C:\AzureAD-BYOA-Provisioning-Samples\.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-185">Unzip hello package and place it on your Windows machine at a location such as C:\AzureAD-BYOA-Provisioning-Samples\.</span></span>
3. <span data-ttu-id="f5fc0-186">Starta hello FileProvisioningAgent lösningen i Visual Studio i den här mappen.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-186">In this folder, launch hello FileProvisioningAgent solution in Visual Studio.</span></span>
4. <span data-ttu-id="f5fc0-187">Välj **Verktyg > Library Package Manager > Package Manager-konsolen**, och kör följande kommandon för hello FileProvisioningAgent tooresolve hello lösning referenser hello:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-187">Select **Tools > Library Package Manager > Package Manager Console**, and execute hello following commands for hello FileProvisioningAgent project tooresolve hello solution references:</span></span>
  ```` 
   Install-Package Microsoft.SystemForCrossDomainIdentityManagement
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   Install-Package Microsoft.Owin.Diagnostics
   Install-Package Microsoft.Owin.Host.SystemWeb
  ````
5. <span data-ttu-id="f5fc0-188">Skapa hello FileProvisioningAgent projekt.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-188">Build hello FileProvisioningAgent project.</span></span>
6. <span data-ttu-id="f5fc0-189">Starta hello kommandotolk program i Windows (som administratör) och använda hello **cd** kommandot toochange hello directory tooyour **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** mapp.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-189">Launch hello Command Prompt application in Windows (as an Administrator), and use hello **cd** command toochange hello directory tooyour **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** folder.</span></span>
7. <span data-ttu-id="f5fc0-190">Kör hello följande kommando, ersätter < ip-adress > med hello IP-adress eller domännamn namnet hello Windows-dator:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-190">Run hello following command, replacing <ip-address> with hello IP address or domain name of hello Windows machine:</span></span>
  ````   
   FileAgnt.exe http://<ip-address>:9000 TargetFile.csv
  ````
8. <span data-ttu-id="f5fc0-191">I under **Windows-inställningar > nätverk och Internet-inställningar**väljer hello **Windows-brandväggen > Avancerade inställningar**, och skapa en **inkommande regel** som tillåter inkommande åtkomst tooport 9000.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-191">In Windows under **Windows Settings > Network & Internet Settings**, select hello **Windows Firewall > Advanced Settings**, and create an **Inbound Rule** that allows inbound access tooport 9000.</span></span>
9. <span data-ttu-id="f5fc0-192">Om hello Windows-datorn är bakom en router, hello router behov toobe konfigurerats tooperform Network Access Translation mellan dess port 9000 som är exponerade toohello internet och port 9000 på hello Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-192">If hello Windows machine is behind a router, hello router needs toobe configured tooperform Network Access Translation between its port 9000 that is exposed toohello internet, and port 9000 on hello Windows machine.</span></span> <span data-ttu-id="f5fc0-193">Detta krävs för Azure AD toobe kan tooaccess den här slutpunkten i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-193">This is required for Azure AD toobe able tooaccess this endpoint in hello cloud.</span></span>

<span data-ttu-id="f5fc0-194">**tooregister hello exempel SCIM slutpunkt i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="f5fc0-194">**tooregister hello sample SCIM endpoint in Azure AD:**</span></span>

1. <span data-ttu-id="f5fc0-195">Logga in för[hello Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f5fc0-195">Sign in too[hello Azure portal](https://portal.azure.com).</span></span> 
2. <span data-ttu-id="f5fc0-196">Bläddra för ** Azure Active Directory > företagsprogram och välj **nytt program > alla > icke-galleriet programmet**.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-196">Browse too**Azure Active Directory > Enterprise Applications, and select **New application > All > Non-gallery application**.</span></span>
3. <span data-ttu-id="f5fc0-197">Ange ett namn för ditt program och klicka på **Lägg till** ikonen toocreate ett app-objekt.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-197">Enter a name for your application, and click **Add** icon toocreate an app object.</span></span> <span data-ttu-id="f5fc0-198">hello programobjektet skapas är avsedda toorepresent hello mål app du vill etablera tooand implementera enkel inloggning för, och inte bara hello SCIM slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-198">hello application object created is intended toorepresent hello target app you would be provisioning tooand implementing single sign-on for, and not just hello SCIM endpoint.</span></span>
4. <span data-ttu-id="f5fc0-199">Välj hello i resulterande hello-skärmen **etablering** fliken i hello vänstra kolumnen.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-199">In hello resulting screen, select hello **Provisioning** tab in hello left column.</span></span>
5. <span data-ttu-id="f5fc0-200">I hello **etablering läge** väljer du **automatisk**.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-200">In hello **Provisioning Mode** menu, select **Automatic**.</span></span>
    
  <span data-ttu-id="f5fc0-201">![][2]
  *Bild 4: Konfigurera allokering i hello Azure-portalen*</span><span class="sxs-lookup"><span data-stu-id="f5fc0-201">![][2]
*Figure 4: Configuring provisioning in hello Azure portal*</span></span>
    
6. <span data-ttu-id="f5fc0-202">I hello **klient URL** anger hello internet-exponerade URL och portnummer för för SCIM slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-202">In hello **Tenant URL** field, enter hello internet-exposed URL and port of your SCIM endpoint.</span></span> <span data-ttu-id="f5fc0-203">Det är något som http://testmachine.contoso.com:9000 eller http://<ip-address>:9000/, där < ip-adress > är hello internet exponeras IP adress.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-203">This would be something like http://testmachine.contoso.com:9000 or http://<ip-address>:9000/, where <ip-address> is hello internet exposed IP address.</span></span>  
7. <span data-ttu-id="f5fc0-204">Om hello SCIM slutpunkten kräver en OAuth ägar-token från en utfärdare än Azure AD och sedan kopiera hello krävs OAuth ägar-token i hello valfria **hemlighet Token** fältet.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-204">If hello SCIM endpoint requires an OAuth bearer token from an issuer other than Azure AD, then copy hello required OAuth bearer token into hello optional **Secret Token** field.</span></span> <span data-ttu-id="f5fc0-205">Om det här fältet är tomt, innehåller en OAuth ägar-token som utfärdas från Azure AD med varje begäran Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-205">If this field is left blank, then Azure AD will include an OAuth bearer token issued from Azure AD with each request.</span></span> <span data-ttu-id="f5fc0-206">Appar som använder Azure AD som en identitetsleverantör kan verifiera den här Azure AD-utfärdade token.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-206">Apps that use Azure AD as an identity provider can validate this Azure AD -issued token.</span></span>
8. <span data-ttu-id="f5fc0-207">Klicka på hello **Testanslutningen** knappen toohave Azure Active Directory försök tooconnect toohello SCIM slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-207">Click hello **Test Connection** button toohave Azure Active Directory attempt tooconnect toohello SCIM endpoint.</span></span> <span data-ttu-id="f5fc0-208">Om hello försök misslyckas, visas information om felet.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-208">If hello attempts fail, error information is displayed.</span></span>  
9. <span data-ttu-id="f5fc0-209">Om det lyckas hello försök tooconnect toohello program, klicka sedan på **spara** toosave hello-administratörsautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-209">If hello attempts tooconnect toohello application succeed, then click **Save** toosave hello admin credentials.</span></span>
10. <span data-ttu-id="f5fc0-210">I hello **mappningar** avsnittet finns det två valbar uppsättningar attributmappning: en för användarobjekt och en för gruppobjekt.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-210">In hello **Mappings** section, there are two selectable sets of attribute mappings: one for user objects and one for group objects.</span></span> <span data-ttu-id="f5fc0-211">Markera varje en tooreview hello attribut som synkroniseras från Azure Active Directory tooyour app.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-211">Select each one tooreview hello attributes that are synchronized from Azure Active Directory tooyour app.</span></span> <span data-ttu-id="f5fc0-212">Hej attribut som valts som **matchande** egenskaper är används toomatch hello användare och grupper i din app för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-212">hello attributes selected as **Matching** properties are used toomatch hello users and groups in your app for update operations.</span></span> <span data-ttu-id="f5fc0-213">Välj hello spara knappen toocommit ändringar.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-213">Select hello Save button toocommit any changes.</span></span>
11. <span data-ttu-id="f5fc0-214">Under **inställningar**, hello **omfång** fältet definierar vilka användare som är och eller grupper som ska synkroniseras.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-214">Under **Settings**, hello **Scope** field defines which users and or groups are synchronized.</span></span> <span data-ttu-id="f5fc0-215">Att välja ”Sync endast har tilldelats användare och grupper” (rekommenderas) kommer bara synkronisera användare och grupper som tilldelas i hello **användare och grupper** fliken.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-215">Selecting "Sync only assigned users and groups" (recommended) will only sync users and groups assigned in hello **Users and groups** tab.</span></span>
12. <span data-ttu-id="f5fc0-216">När konfigurationen är klar kan du ändra hello **Status för etablering** för**på**.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-216">Once your configuration is complete, change hello **Provisioning Status** too**On**.</span></span>
13. <span data-ttu-id="f5fc0-217">Klicka på **spara** toostart hello etablering Azure AD-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-217">Click **Save** toostart hello Azure AD provisioning service.</span></span> 
14. <span data-ttu-id="f5fc0-218">Om synkroniserar endast tilldelas användare och grupper (rekommenderas), vara säker på att tooselect hello **användare och grupper** och tilldelar hello användare och/eller grupper som du vill toosync.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-218">If syncing only assigned users and groups (recommended), be sure tooselect hello **Users and groups** tab and assign hello users and/or groups you wish toosync.</span></span>

<span data-ttu-id="f5fc0-219">När hello inledande synkroniseringen har startat, kan du använda hello **granskningsloggar** fliken toomonitor pågår, som visar alla åtgärder som utförs av hello etableras på din app.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-219">Once hello initial synchronization has started, you can use hello **Audit logs** tab toomonitor progress, which shows all actions performed by hello provisioning service on your app.</span></span> <span data-ttu-id="f5fc0-220">Mer information om hur tooread hello Azure AD-etablering loggar finns [rapportering om automatisk konto användaretablering](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="f5fc0-220">For more information on how tooread hello Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>

<span data-ttu-id="f5fc0-221">hello sista steget vid verifiering av hello exempel är tooopen hello TargetFile.csv fil i hello \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug mapp på din Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-221">hello final step in verifying hello sample is tooopen hello TargetFile.csv file in hello \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug folder on your Windows machine.</span></span> <span data-ttu-id="f5fc0-222">När hello etableringsprocessen körs med den här filen hello detaljer om allt tilldelade och etableras användare och grupper.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-222">Once hello provisioning process is run, this file shows hello details of all assigned and provisioned users and groups.</span></span>

### <a name="development-libraries"></a><span data-ttu-id="f5fc0-223">Utvecklingsbibliotek</span><span class="sxs-lookup"><span data-stu-id="f5fc0-223">Development libraries</span></span>
<span data-ttu-id="f5fc0-224">toodevelop egna webbtjänst som överensstämmer med toohello SCIM-specifikationen först bekanta dig med hello efter bibliotek som föreskrivs av Microsoft toohelp påskynda hello utvecklingsprocessen:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-224">toodevelop your own web service that conforms toohello SCIM specification, first familiarize yourself with hello following libraries provided by Microsoft toohelp accelerate hello development process:</span></span> 

1. <span data-ttu-id="f5fc0-225">Common Language Infrastructure (CLI) bibliotek erbjuds för användning med språk som är baserat på infrastrukturen, till exempel C#.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-225">Common Language Infrastructure (CLI) libraries are offered for use with languages based on that infrastructure, such as C#.</span></span> <span data-ttu-id="f5fc0-226">En av dessa bibliotek [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), deklarerar ett gränssnitt, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, som visas i följande illustration hello: A utvecklare som använder hello bibliotek skulle implementera gränssnittet med en klass som kan refereras till, Allmänt, som en provider.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-226">One of those libraries, [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), declares an interface, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, shown in hello following illustration:  A developer using hello libraries would implement that interface with a class that may be referred to, generically, as a provider.</span></span> <span data-ttu-id="f5fc0-227">hello bibliotek aktivera hello developer toodeploy en webbtjänst som överensstämmer med toohello SCIM-specifikationen.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-227">hello libraries enable hello developer toodeploy a web service that conforms toohello SCIM specification.</span></span> <span data-ttu-id="f5fc0-228">hello-webbtjänsten kan finnas antingen i Internet Information Services eller alla körbara vanlig infrastruktur för språk-sammansättningen.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-228">hello web service can be either hosted within Internet Information Services, or any executable Common Language Infrastructure assembly.</span></span> <span data-ttu-id="f5fc0-229">Begäran översätts till anrop toohello providern metoder som skulle vara programmerad av hello developer toooperate på vissa Identitetslagret.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-229">Request is translated into calls toohello provider’s methods, which would be programmed by hello developer toooperate on some identity store.</span></span>
  
  ![][3]
  
2. <span data-ttu-id="f5fc0-230">[Express route-hanterare](http://expressjs.com/guide/routing.html) är tillgängliga för parsning av node.js begärandeobjekt som motsvarar anrop (som definieras av hello SCIM specification) tooa node.js-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-230">[Express route handlers](http://expressjs.com/guide/routing.html) are available for parsing node.js request objects representing calls (as defined by hello SCIM specification), made tooa node.js web service.</span></span>   

### <a name="building-a-custom-scim-endpoint"></a><span data-ttu-id="f5fc0-231">Skapa en anpassad SCIM slutpunkt</span><span class="sxs-lookup"><span data-stu-id="f5fc0-231">Building a Custom SCIM Endpoint</span></span>
<span data-ttu-id="f5fc0-232">Använda hello CLI bibliotek, värd utvecklare som använder dessa bibliotek sina tjänster i alla körbara vanlig infrastruktur för språk-sammansättningen eller i Internet Information Services.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-232">Using hello CLI libraries, developers using those libraries can host their services within any executable Common Language Infrastructure assembly, or within Internet Information Services.</span></span> <span data-ttu-id="f5fc0-233">Här är exempelkod som värd för en tjänst i en körbar sammansättning på hello adressen http://localhost:9000:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-233">Here is sample code for hosting a service within an executable assembly, at hello address http://localhost:9000:</span></span> 

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

<span data-ttu-id="f5fc0-234">Den här tjänsten måste ha en HTTP-adress och servern certifikat för serverautentisering för vilka hello rotcertifikatutfärdaren är en av följande hello:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-234">This service must have an HTTP address and server authentication certificate of which hello root certification authority is one of hello following:</span></span> 

* <span data-ttu-id="f5fc0-235">CNNIC</span><span class="sxs-lookup"><span data-stu-id="f5fc0-235">CNNIC</span></span>
* <span data-ttu-id="f5fc0-236">Comodo</span><span class="sxs-lookup"><span data-stu-id="f5fc0-236">Comodo</span></span>
* <span data-ttu-id="f5fc0-237">CyberTrust</span><span class="sxs-lookup"><span data-stu-id="f5fc0-237">CyberTrust</span></span>
* <span data-ttu-id="f5fc0-238">DigiCert</span><span class="sxs-lookup"><span data-stu-id="f5fc0-238">DigiCert</span></span>
* <span data-ttu-id="f5fc0-239">GeoTrust</span><span class="sxs-lookup"><span data-stu-id="f5fc0-239">GeoTrust</span></span>
* <span data-ttu-id="f5fc0-240">GlobalSign</span><span class="sxs-lookup"><span data-stu-id="f5fc0-240">GlobalSign</span></span>
* <span data-ttu-id="f5fc0-241">Go Daddy</span><span class="sxs-lookup"><span data-stu-id="f5fc0-241">Go Daddy</span></span>
* <span data-ttu-id="f5fc0-242">VeriSign</span><span class="sxs-lookup"><span data-stu-id="f5fc0-242">Verisign</span></span>
* <span data-ttu-id="f5fc0-243">WoSign</span><span class="sxs-lookup"><span data-stu-id="f5fc0-243">WoSign</span></span>

<span data-ttu-id="f5fc0-244">Ett certifikat för serverautentisering kan vara bundna tooa port på en Windows-värd med hello network shell-verktyget:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-244">A server authentication certificate can be bound tooa port on a Windows host using hello network shell utility:</span></span> 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  

<span data-ttu-id="f5fc0-245">Här är hello-värdet som angetts för hello certhash argumentet hello certifikatets hello tumavtryck medan hello-värdet som angetts för hello appid argumentet är en godtycklig globalt unik identifierare.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-245">Here, hello value provided for hello certhash argument is hello thumbprint of hello certificate, while hello value provided for hello appid argument is an arbitrary globally unique identifier.</span></span>  

<span data-ttu-id="f5fc0-246">toohost hello-tjänsten i IIS, en utvecklare skulle skapa en CLA kod bibliotekssammansättning med en klass som heter Start i hello standardnamnområdet hello sammansättningen.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-246">toohost hello service within Internet Information Services, a developer would build a CLA code library assembly with a class named Startup in hello default namespace of hello assembly.</span></span>  <span data-ttu-id="f5fc0-247">Här är ett exempel på sådan klass:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-247">Here is a sample of such a class:</span></span> 

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

### <a name="handling-endpoint-authentication"></a><span data-ttu-id="f5fc0-248">Hantering av slutpunkten autentisering</span><span class="sxs-lookup"><span data-stu-id="f5fc0-248">Handling endpoint authentication</span></span>
<span data-ttu-id="f5fc0-249">Begäranden från Azure Active Directory innehåller en OAuth 2.0-ägartoken.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-249">Requests from Azure Active Directory include an OAuth 2.0 bearer token.</span></span>   <span data-ttu-id="f5fc0-250">Alla mottagande hello tjänstbegäran ska autentisera hello utfärdaren som Azure Active Directory uppdrag hello förväntades Azure Active Directory-klient för åtkomst toohello Azure Active Directory Graph-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-250">Any service receiving hello request should authenticate hello issuer as being Azure Active Directory on behalf of hello expected Azure Active Directory tenant, for access toohello Azure Active Directory Graph web service.</span></span>  <span data-ttu-id="f5fc0-251">I hello token identifieras hello utfärdaren av ett iss-anspråk som ”iss”: ”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-251">In hello token, hello issuer is identified by an iss claim, like, "iss":"https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".</span></span>  <span data-ttu-id="f5fc0-252">I det här exemplet hello basadressen för hello anspråksvärdet https://sts.windows.net, identifierar Azure Active Directory som hello utfärdare, medan hello relativ adress segment, cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, är en unik identifierare för hello Azure Active Directory innehavaren för vilken hello token har utfärdats.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-252">In this example, hello base address of hello claim value, https://sts.windows.net, identifies Azure Active Directory as hello issuer, while hello relative address segment, cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, is a unique identifier of hello Azure Active Directory tenant on behalf of which hello token was issued.</span></span>  <span data-ttu-id="f5fc0-253">Om hello token har utfärdats för att komma åt hello Azure Active Directory Graph webbtjänsten sedan hello identifierare för att tjänsten bör 00000002-0000-0000-c000-000000000000, vara i hello värdet av hello-token eller anspråk.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-253">If hello token was issued for accessing hello Azure Active Directory Graph web service, then hello identifier of that service, 00000002-0000-0000-c000-000000000000, should be in hello value of hello token’s aud claim.</span></span>  

<span data-ttu-id="f5fc0-254">Utvecklare som använder hello CLA bibliotek som tillhandahålls av Microsoft för att skapa en SCIM-tjänst kan autentisera begäranden från Azure Active Directory med hello Microsoft.Owin.Security.ActiveDirectory paketet genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-254">Developers using hello CLA libraries provided by Microsoft for building a SCIM service can authenticate requests from Azure Active Directory using hello Microsoft.Owin.Security.ActiveDirectory package by following these steps:</span></span> 

1. <span data-ttu-id="f5fc0-255">I en provider, implementera hello Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior egenskapen genom att låta den returnera en metoden toobe anropas när hello-tjänsten har startats:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-255">In a provider, implement hello Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior property by having it return a method toobe called whenever hello service is started:</span></span> 

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

2. <span data-ttu-id="f5fc0-256">Lägg till följande kod toothat metoden toohave hello alla begäran tooany hello-tjänstens slutpunkter autentiserad som försetts med en token som utfärdas av Azure Active Directory för en angiven klient för åtkomst toohello Azure AD Graph-webbtjänsten:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-256">Add hello following code toothat method toohave any request tooany of hello service’s endpoints authenticated as bearing a token issued by Azure Active Directory on behalf of a specified tenant, for access toohello Azure AD Graph web service:</span></span> 

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
        Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute hello appropriate tenant’s 
                                                      // identifier for this one.  
      };

      applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
    }
  ````


## <a name="user-and-group-schema"></a><span data-ttu-id="f5fc0-257">Användar- och schema</span><span class="sxs-lookup"><span data-stu-id="f5fc0-257">User and group schema</span></span>
<span data-ttu-id="f5fc0-258">Azure Active Directory kan etablera två typer av resurser tooSCIM webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-258">Azure Active Directory can provision two types of resources tooSCIM web services.</span></span>  <span data-ttu-id="f5fc0-259">Dessa typer av resurser är användare och grupper.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-259">Those types of resources are users and groups.</span></span>  

<span data-ttu-id="f5fc0-260">Resurser för användare identifieras av hello schema-ID, urn: ietf:params:scim:schemas:extension:enterprise:2.0:User som ingår i den här protokollspecifikation: http://tools.ietf.org/html/draft-ietf-scim-core-schema.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-260">User resources are identified by hello schema identifier, urn:ietf:params:scim:schemas:extension:enterprise:2.0:User, which is included in this protocol specification: http://tools.ietf.org/html/draft-ietf-scim-core-schema.</span></span>  <span data-ttu-id="f5fc0-261">hello standardmappningen hello attribut för användare i Azure Active Directory toohello attribut för urn: ietf:params:scim:schemas:extension:enterprise:2.0:User resurser finns i tabell 1 nedan.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-261">hello default mapping of hello attributes of users in Azure Active Directory toohello attributes of urn:ietf:params:scim:schemas:extension:enterprise:2.0:User resources is provided in table 1, below.</span></span>  

<span data-ttu-id="f5fc0-262">Gruppera resurser identifieras av hello schemaidentifierare http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-262">Group resources are identified by hello schema identifier, http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.</span></span>  <span data-ttu-id="f5fc0-263">Tabell 2 nedan visar hello standardmappningen hello attribut för resursgrupper i Azure Active Directory-attribut för toohello http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-263">Table 2, below, shows hello default mapping of hello attributes of groups in Azure Active Directory toohello attributes of http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group resources.</span></span>  

### <a name="table-1-default-user-attribute-mapping"></a><span data-ttu-id="f5fc0-264">Tabell 1: Standard användaren attributmappning</span><span class="sxs-lookup"><span data-stu-id="f5fc0-264">Table 1: Default user attribute mapping</span></span>
| <span data-ttu-id="f5fc0-265">Azure Active Directory-användare</span><span class="sxs-lookup"><span data-stu-id="f5fc0-265">Azure Active Directory user</span></span> | <span data-ttu-id="f5fc0-266">urn: ietf:params:scim:schemas:extension:enterprise:2.0:User</span><span class="sxs-lookup"><span data-stu-id="f5fc0-266">urn:ietf:params:scim:schemas:extension:enterprise:2.0:User</span></span> |
| --- | --- |
| <span data-ttu-id="f5fc0-267">IsSoftDeleted</span><span class="sxs-lookup"><span data-stu-id="f5fc0-267">IsSoftDeleted</span></span> |<span data-ttu-id="f5fc0-268">Aktiv</span><span class="sxs-lookup"><span data-stu-id="f5fc0-268">active</span></span> |
| <span data-ttu-id="f5fc0-269">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="f5fc0-269">displayName</span></span> |<span data-ttu-id="f5fc0-270">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="f5fc0-270">displayName</span></span> |
| <span data-ttu-id="f5fc0-271">Fax TelephoneNumber</span><span class="sxs-lookup"><span data-stu-id="f5fc0-271">Facsimile-TelephoneNumber</span></span> |<span data-ttu-id="f5fc0-272">phoneNumbers typ eq ”fax” .value</span><span class="sxs-lookup"><span data-stu-id="f5fc0-272">phoneNumbers[type eq "fax"].value</span></span> |
| <span data-ttu-id="f5fc0-273">givenName</span><span class="sxs-lookup"><span data-stu-id="f5fc0-273">givenName</span></span> |<span data-ttu-id="f5fc0-274">name.givenName</span><span class="sxs-lookup"><span data-stu-id="f5fc0-274">name.givenName</span></span> |
| <span data-ttu-id="f5fc0-275">Befattning</span><span class="sxs-lookup"><span data-stu-id="f5fc0-275">jobTitle</span></span> |<span data-ttu-id="f5fc0-276">Rubrik</span><span class="sxs-lookup"><span data-stu-id="f5fc0-276">title</span></span> |
| <span data-ttu-id="f5fc0-277">E-post</span><span class="sxs-lookup"><span data-stu-id="f5fc0-277">mail</span></span> |<span data-ttu-id="f5fc0-278">e-postmeddelanden typen eq ”arbete” .value</span><span class="sxs-lookup"><span data-stu-id="f5fc0-278">emails[type eq "work"].value</span></span> |
| <span data-ttu-id="f5fc0-279">mailNickname</span><span class="sxs-lookup"><span data-stu-id="f5fc0-279">mailNickname</span></span> |<span data-ttu-id="f5fc0-280">externalId</span><span class="sxs-lookup"><span data-stu-id="f5fc0-280">externalId</span></span> |
| <span data-ttu-id="f5fc0-281">Manager</span><span class="sxs-lookup"><span data-stu-id="f5fc0-281">manager</span></span> |<span data-ttu-id="f5fc0-282">Manager</span><span class="sxs-lookup"><span data-stu-id="f5fc0-282">manager</span></span> |
| <span data-ttu-id="f5fc0-283">mobila</span><span class="sxs-lookup"><span data-stu-id="f5fc0-283">mobile</span></span> |<span data-ttu-id="f5fc0-284">phoneNumbers typ eq ”mobil” .value</span><span class="sxs-lookup"><span data-stu-id="f5fc0-284">phoneNumbers[type eq "mobile"].value</span></span> |
| <span data-ttu-id="f5fc0-285">objekt-ID</span><span class="sxs-lookup"><span data-stu-id="f5fc0-285">objectId</span></span> |<span data-ttu-id="f5fc0-286">id</span><span class="sxs-lookup"><span data-stu-id="f5fc0-286">id</span></span> |
| <span data-ttu-id="f5fc0-287">Postnummer</span><span class="sxs-lookup"><span data-stu-id="f5fc0-287">postalCode</span></span> |<span data-ttu-id="f5fc0-288">adresser typen eq ”arbete” .postalCode</span><span class="sxs-lookup"><span data-stu-id="f5fc0-288">addresses[type eq "work"].postalCode</span></span> |
| <span data-ttu-id="f5fc0-289">Proxy-adresser</span><span class="sxs-lookup"><span data-stu-id="f5fc0-289">proxy-Addresses</span></span> |<span data-ttu-id="f5fc0-290">e-postmeddelanden [Ange eq ”andra”]. Värdet</span><span class="sxs-lookup"><span data-stu-id="f5fc0-290">emails[type eq "other"].Value</span></span> |
| <span data-ttu-id="f5fc0-291">fysisk-leverans – OfficeName</span><span class="sxs-lookup"><span data-stu-id="f5fc0-291">physical-Delivery-OfficeName</span></span> |<span data-ttu-id="f5fc0-292">[Ange eq ”andra”] adresser. Formaterad</span><span class="sxs-lookup"><span data-stu-id="f5fc0-292">addresses[type eq "other"].Formatted</span></span> |
| <span data-ttu-id="f5fc0-293">StreetAddress</span><span class="sxs-lookup"><span data-stu-id="f5fc0-293">streetAddress</span></span> |<span data-ttu-id="f5fc0-294">adresser typen eq ”arbete” .streetAddress</span><span class="sxs-lookup"><span data-stu-id="f5fc0-294">addresses[type eq "work"].streetAddress</span></span> |
| <span data-ttu-id="f5fc0-295">Efternamn</span><span class="sxs-lookup"><span data-stu-id="f5fc0-295">surname</span></span> |<span data-ttu-id="f5fc0-296">name.familyName</span><span class="sxs-lookup"><span data-stu-id="f5fc0-296">name.familyName</span></span> |
| <span data-ttu-id="f5fc0-297">Telefonnummer</span><span class="sxs-lookup"><span data-stu-id="f5fc0-297">telephone-Number</span></span> |<span data-ttu-id="f5fc0-298">phoneNumbers typ eq ”arbete” .value</span><span class="sxs-lookup"><span data-stu-id="f5fc0-298">phoneNumbers[type eq "work"].value</span></span> |
| <span data-ttu-id="f5fc0-299">användaren huvudkontot</span><span class="sxs-lookup"><span data-stu-id="f5fc0-299">user-PrincipalName</span></span> |<span data-ttu-id="f5fc0-300">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="f5fc0-300">userName</span></span> |

### <a name="table-2-default-group-attribute-mapping"></a><span data-ttu-id="f5fc0-301">Tabell 2: Standard grupp attributmappning</span><span class="sxs-lookup"><span data-stu-id="f5fc0-301">Table 2: Default group attribute mapping</span></span>
| <span data-ttu-id="f5fc0-302">Azure Active Directory-grupp</span><span class="sxs-lookup"><span data-stu-id="f5fc0-302">Azure Active Directory group</span></span> | <span data-ttu-id="f5fc0-303">http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group</span><span class="sxs-lookup"><span data-stu-id="f5fc0-303">http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group</span></span> |
| --- | --- |
| <span data-ttu-id="f5fc0-304">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="f5fc0-304">displayName</span></span> |<span data-ttu-id="f5fc0-305">externalId</span><span class="sxs-lookup"><span data-stu-id="f5fc0-305">externalId</span></span> |
| <span data-ttu-id="f5fc0-306">E-post</span><span class="sxs-lookup"><span data-stu-id="f5fc0-306">mail</span></span> |<span data-ttu-id="f5fc0-307">e-postmeddelanden typen eq ”arbete” .value</span><span class="sxs-lookup"><span data-stu-id="f5fc0-307">emails[type eq "work"].value</span></span> |
| <span data-ttu-id="f5fc0-308">mailNickname</span><span class="sxs-lookup"><span data-stu-id="f5fc0-308">mailNickname</span></span> |<span data-ttu-id="f5fc0-309">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="f5fc0-309">displayName</span></span> |
| <span data-ttu-id="f5fc0-310">Medlemmar</span><span class="sxs-lookup"><span data-stu-id="f5fc0-310">members</span></span> |<span data-ttu-id="f5fc0-311">Medlemmar</span><span class="sxs-lookup"><span data-stu-id="f5fc0-311">members</span></span> |
| <span data-ttu-id="f5fc0-312">objekt-ID</span><span class="sxs-lookup"><span data-stu-id="f5fc0-312">objectId</span></span> |<span data-ttu-id="f5fc0-313">id</span><span class="sxs-lookup"><span data-stu-id="f5fc0-313">id</span></span> |
| <span data-ttu-id="f5fc0-314">proxyAddresses</span><span class="sxs-lookup"><span data-stu-id="f5fc0-314">proxyAddresses</span></span> |<span data-ttu-id="f5fc0-315">e-postmeddelanden [Ange eq ”andra”]. Värdet</span><span class="sxs-lookup"><span data-stu-id="f5fc0-315">emails[type eq "other"].Value</span></span> |

## <a name="user-provisioning-and-de-provisioning"></a><span data-ttu-id="f5fc0-316">Användaretablering och avetablering</span><span class="sxs-lookup"><span data-stu-id="f5fc0-316">User provisioning and de-provisioning</span></span>
<span data-ttu-id="f5fc0-317">Följande bild visar hälsningsmeddelande att Azure Active Directory skickar tooa SCIM service toomanage hello livscykeln för en användare i en annan identitet store hello.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-317">hello following illustration shows hello messages that Azure Active Directory sends tooa SCIM service toomanage hello lifecycle of a user in another identity store.</span></span> <span data-ttu-id="f5fc0-318">hello diagram visar även hur en SCIM tjänst som implementeras med hjälp av hello CLI bibliotek som tillhandahålls av Microsoft för skapande av dessa tjänster översätta dessa begäranden till anrop toohello metoder för en provider.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-318">hello diagram also shows how a SCIM service implemented using hello CLI libraries provided by Microsoft for building such services translate those requests into calls toohello methods of a provider.</span></span>  

<span data-ttu-id="f5fc0-319">![][4]
*Bild 5: Användaretablering och avetablering sekvens*</span><span class="sxs-lookup"><span data-stu-id="f5fc0-319">![][4]
*Figure 5: User provisioning and de-provisioning sequence*</span></span>

1. <span data-ttu-id="f5fc0-320">Azure Active Directory-frågor hello tjänst för en användare med ett attributvärde för externalId matchar hello mailNickname attributvärde för en användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-320">Azure Active Directory queries hello service for a user with an externalId attribute value matching hello mailNickname attribute value of a user in Azure AD.</span></span> <span data-ttu-id="f5fc0-321">hello frågan uttrycks som en begäran om Hypertext Transfer Protocol (HTTP) som det här exemplet där jyoung är ett exempel på en mailNickname för en användare i Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-321">hello query is expressed as a Hypertext Transfer Protocol (HTTP) request such as this example, wherein jyoung is a sample of a mailNickname of a user in Azure Active Directory:</span></span> 
  ````
    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...
  ````
  <span data-ttu-id="f5fc0-322">Om hello-tjänsten har skapats med hello vanlig infrastruktur för språket bibliotek som tillhandahålls av Microsoft för att implementera SCIM tjänster, översätts hello begäran till en anropet toohello frågemetoden av hello-leverantör.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-322">If hello service was built using hello Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, then hello request is translated into a call toohello Query method of hello service’s provider.</span></span>  <span data-ttu-id="f5fc0-323">Här är hello signaturen för metoden:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-323">Here is hello signature of that method:</span></span> 
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
  <span data-ttu-id="f5fc0-324">Här är hello definition av hello Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters gränssnitt:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-324">Here is hello definition of hello Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters interface:</span></span> 
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
  <span data-ttu-id="f5fc0-325">I följande exempel på en fråga för en användare med ett angivet värde för hello externalId attributet hello, är värden hello-argument som skickas toohello frågemetoden:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-325">In hello following sample of a query for a user with a given value for hello externalId attribute, values of hello arguments passed toohello Query method are:</span></span> 
  * <span data-ttu-id="f5fc0-326">parametrar. AlternateFilters.Count: 1</span><span class="sxs-lookup"><span data-stu-id="f5fc0-326">parameters.AlternateFilters.Count: 1</span></span>
  * <span data-ttu-id="f5fc0-327">parametrar. AlternateFilters.ElementAt(0). AttributePath: ”externalId”</span><span class="sxs-lookup"><span data-stu-id="f5fc0-327">parameters.AlternateFilters.ElementAt(0).AttributePath: "externalId"</span></span>
  * <span data-ttu-id="f5fc0-328">parametrar. AlternateFilters.ElementAt(0). Jämförelseoperator: ComparisonOperator.Equals</span><span class="sxs-lookup"><span data-stu-id="f5fc0-328">parameters.AlternateFilters.ElementAt(0).ComparisonOperator: ComparisonOperator.Equals</span></span>
  * <span data-ttu-id="f5fc0-329">parametrar. AlternateFilter.ElementAt(0). ComparisonValue: ”jyoung”</span><span class="sxs-lookup"><span data-stu-id="f5fc0-329">parameters.AlternateFilter.ElementAt(0).ComparisonValue: "jyoung"</span></span>
  * <span data-ttu-id="f5fc0-330">correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin. Begärande-ID ”]</span><span class="sxs-lookup"><span data-stu-id="f5fc0-330">correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin.RequestId"]</span></span> 

2. <span data-ttu-id="f5fc0-331">Om hello svar tooa frågan toohello webbtjänst för en användare med ett externalId-attributvärde som matchar hello mailNickname attributvärde för en användare inte returnerar några användare begär Azure Active Directory som hello tillhandahållande en användare motsvarande toohello något i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-331">If hello response tooa query toohello web service for a user with an externalId attribute value that matches hello mailNickname attribute value of a user does not return any users, then Azure Active Directory requests that hello service provision a user corresponding toohello one in Azure Active Directory.</span></span>  <span data-ttu-id="f5fc0-332">Här är ett exempel på en sådan begäran:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-332">Here is an example of such a request:</span></span> 
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
  <span data-ttu-id="f5fc0-333">hello vanlig infrastruktur för språket bibliotek som tillhandahålls av Microsoft för att implementera SCIM tjänster kan översätta begäran till en anropet toohello Create-metoden för hello-leverantör.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-333">hello Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services would translate that request into a call toohello Create method of hello service’s provider.</span></span>  <span data-ttu-id="f5fc0-334">hello Create-metoden har den här signaturen:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-334">hello Create method has this signature:</span></span> 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create(
      Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
      string correlationIdentifier);
  ````
  <span data-ttu-id="f5fc0-335">Hello-värdet för hello resurs argumentet är en instans av hello Microsoft.SystemForCrossDomainIdentityManagement i begäran-tooprovision en användare.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-335">In a request tooprovision a user, hello value of hello resource argument is an instance of hello Microsoft.SystemForCrossDomainIdentityManagement.</span></span> <span data-ttu-id="f5fc0-336">Core2EnterpriseUser klass som definierats i hello Microsoft.SystemForCrossDomainIdentityManagement.Schemas-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-336">Core2EnterpriseUser class, defined in hello Microsoft.SystemForCrossDomainIdentityManagement.Schemas library.</span></span>  <span data-ttu-id="f5fc0-337">Om hello begäran tooprovision hello användare lyckas, sedan är hello implementering av hello-metoden förväntade tooreturn en instans av hello Microsoft.SystemForCrossDomainIdentityManagement.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-337">If hello request tooprovision hello user succeeds, then hello implementation of hello method is expected tooreturn an instance of hello Microsoft.SystemForCrossDomainIdentityManagement.</span></span> <span data-ttu-id="f5fc0-338">Core2EnterpriseUser-klass med hello värdet för hello ID-egenskapen angetts toohello Unik identifierare för hello nyetablerade användare.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-338">Core2EnterpriseUser class, with hello value of hello Identifier property set toohello unique identifier of hello newly provisioned user.</span></span>  

3. <span data-ttu-id="f5fc0-339">tooupdate en känd tooexist i en identity-butik fronted genom en SCIM Azure Active Directory fortsätter genom att begära hello aktuell status för den användaren från hello-tjänsten med en begäran som användare:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-339">tooupdate a user known tooexist in an identity store fronted by an SCIM, Azure Active Directory proceeds by requesting hello current state of that user from hello service with a request such as:</span></span> 
  ````
    GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  <span data-ttu-id="f5fc0-340">I en tjänst som skapats med hello vanlig infrastruktur för språket bibliotek som tillhandahålls av Microsoft för att implementera SCIM tjänster översätts hello-begäran till en anropet toohello hämta-metoden i hello-leverantör.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-340">In a service built using hello Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, hello request is translated into a call toohello Retrieve method of hello service’s provider.</span></span>  <span data-ttu-id="f5fc0-341">Här är hello signaturen för hello hämta-metoden:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-341">Here is hello signature of hello Retrieve method:</span></span> 
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
  <span data-ttu-id="f5fc0-342">I en begäran tooretrieve hello aktuell status för en användare hello exempelvis är hello värdena för hello egenskaper för hello-objekt som angetts för hello värde för argumentet för hello-parametrar följande:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-342">In hello example of a request tooretrieve hello current state of a user, hello values of hello properties of hello object provided as hello value of hello parameters argument are as follows:</span></span> 
  
  * <span data-ttu-id="f5fc0-343">ID: ”54D382A4-2050-4C03-94D1-E769F1D15682”</span><span class="sxs-lookup"><span data-stu-id="f5fc0-343">Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span></span>
  * <span data-ttu-id="f5fc0-344">SchemaIdentifier: ”urn: ietf:params:scim:schemas:extension:enterprise:2.0:User”</span><span class="sxs-lookup"><span data-stu-id="f5fc0-344">SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"</span></span>

4. <span data-ttu-id="f5fc0-345">Om ett referensattribut toobe uppdaterats sedan Azure Active Directory-frågor hello service toodetermine oavsett hello aktuella värdet för hello referensattribut i hello Identitetslagret fronted av matchar hello-tjänst redan hello värdet för attributet i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-345">If a reference attribute is toobe updated, then Azure Active Directory queries hello service toodetermine whether or not hello current value of hello reference attribute in hello identity store fronted by hello service already matches hello value of that attribute in Azure Active Directory.</span></span> <span data-ttu-id="f5fc0-346">För användare är hello endast attribut för vilka hello efterfrågas aktuella värdet på det här sättet hello manager attribut.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-346">For users, hello only attribute of which hello current value is queried in this way is hello manager attribute.</span></span> <span data-ttu-id="f5fc0-347">Här är ett exempel på en begäran toodetermine om hello manager attribut för en viss användare-objekt har ett visst värde:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-347">Here is an example of a request toodetermine whether hello manager attribute of a particular user object currently has a certain value:</span></span> 
  ````
    GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq 2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
    Authorization: Bearer ...
  ````
  <span data-ttu-id="f5fc0-348">Hej värdet för Frågeparametern för hello attribut-id, innebär det att om det finns ett användarobjekt som motsvarar hello uttryck som hello värdet för Frågeparametern för hello filter och sedan hello-tjänsten är förväntade toorespond med urn: ietf:params:scim:schemas: kärnor: 2.0:User eller urn: ietf:params:scim:schemas:extension:enterprise:2.0:User resurs, inklusive endast hello värdet för den resurs-id-attribut.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-348">hello value of hello attributes query parameter, id, signifies that if a user object exists that satisfies hello expression provided as hello value of hello filter query parameter, then hello service is expected toorespond with a urn:ietf:params:scim:schemas:core:2.0:User or urn:ietf:params:scim:schemas:extension:enterprise:2.0:User resource, including only hello value of that resource’s id attribute.</span></span>  <span data-ttu-id="f5fc0-349">Hej värdet för hello **id** attributet är känd toohello begärande.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-349">hello value of hello **id** attribute is known toohello requestor.</span></span> <span data-ttu-id="f5fc0-350">Den ingår i hello värdet för Frågeparametern för hello filter; hello syftet med ber om den är faktiskt toorequest en minimal representation av en resurs som uppfyller hello filteruttrycket som en indikation på om huruvida varje sådan objekt finns.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-350">It is included in hello value of hello filter query parameter; hello purpose of asking for it is actually toorequest a minimal representation of a resource that satisfying hello filter expression as an indication of whether or not any such object exists.</span></span>   

  <span data-ttu-id="f5fc0-351">Om hello-tjänsten har skapats med hello vanlig infrastruktur för språket bibliotek som tillhandahålls av Microsoft för att implementera SCIM tjänster, översätts hello begäran till en anropet toohello frågemetoden av hello-leverantör.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-351">If hello service was built using hello Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, then hello request is translated into a call toohello Query method of hello service’s provider.</span></span> <span data-ttu-id="f5fc0-352">hello-värdet för hello egenskaper för hello-objekt som angetts för hello värde för argumentet för hello-parametrar är följande:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-352">hello value of hello properties of hello object provided as hello value of hello parameters argument are as follows:</span></span> 
  
  * <span data-ttu-id="f5fc0-353">parametrar. AlternateFilters.Count: 2</span><span class="sxs-lookup"><span data-stu-id="f5fc0-353">parameters.AlternateFilters.Count: 2</span></span>
  * <span data-ttu-id="f5fc0-354">parametrar. AlternateFilters.ElementAt(x). AttributePath: ”id”</span><span class="sxs-lookup"><span data-stu-id="f5fc0-354">parameters.AlternateFilters.ElementAt(x).AttributePath: "id"</span></span>
  * <span data-ttu-id="f5fc0-355">parametrar. AlternateFilters.ElementAt(x). Jämförelseoperator: ComparisonOperator.Equals</span><span class="sxs-lookup"><span data-stu-id="f5fc0-355">parameters.AlternateFilters.ElementAt(x).ComparisonOperator: ComparisonOperator.Equals</span></span>
  * <span data-ttu-id="f5fc0-356">parametrar. AlternateFilter.ElementAt(x). ComparisonValue: ”54D382A4-2050-4C03-94D1-E769F1D15682”</span><span class="sxs-lookup"><span data-stu-id="f5fc0-356">parameters.AlternateFilter.ElementAt(x).ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"</span></span>
  * <span data-ttu-id="f5fc0-357">parametrar. AlternateFilters.ElementAt(y). AttributePath: ”manager”</span><span class="sxs-lookup"><span data-stu-id="f5fc0-357">parameters.AlternateFilters.ElementAt(y).AttributePath: "manager"</span></span>
  * <span data-ttu-id="f5fc0-358">parametrar. AlternateFilters.ElementAt(y). Jämförelseoperator: ComparisonOperator.Equals</span><span class="sxs-lookup"><span data-stu-id="f5fc0-358">parameters.AlternateFilters.ElementAt(y).ComparisonOperator: ComparisonOperator.Equals</span></span>
  * <span data-ttu-id="f5fc0-359">parametrar. AlternateFilter.ElementAt(y). ComparisonValue: ”2819c223-7f76-453a-919d-413861904646”</span><span class="sxs-lookup"><span data-stu-id="f5fc0-359">parameters.AlternateFilter.ElementAt(y).ComparisonValue: "2819c223-7f76-453a-919d-413861904646"</span></span>
  * <span data-ttu-id="f5fc0-360">parametrar. RequestedAttributePaths.ElementAt(0): ”id”</span><span class="sxs-lookup"><span data-stu-id="f5fc0-360">parameters.RequestedAttributePaths.ElementAt(0): "id"</span></span>
  * <span data-ttu-id="f5fc0-361">parametrar. SchemaIdentifier: ”urn: ietf:params:scim:schemas:extension:enterprise:2.0:User”</span><span class="sxs-lookup"><span data-stu-id="f5fc0-361">parameters.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"</span></span>

  <span data-ttu-id="f5fc0-362">Här, hello index x hello värdet kan vara 0 hello värdet för hello index y kan vara 1, eller hello värdet för x kan vara 1 och hello värdet för y kan vara 0, beroende på hello ordning hello uttryck av hello filter frågeparameter.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-362">Here, hello value of hello index x may be 0 and hello value of hello index y may be 1, or hello value of x may be 1 and hello value of y may be 0, depending on hello order of hello expressions of hello filter query parameter.</span></span>   

5. <span data-ttu-id="f5fc0-363">Här är ett exempel på en begäran från Azure Active Directory tooan SCIM service tooupdate en användare:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-363">Here is an example of a request from Azure Active Directory tooan SCIM service tooupdate a user:</span></span> 
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
  <span data-ttu-id="f5fc0-364">hello infrastruktur för Microsoft Common Language bibliotek för att implementera SCIM tjänster kan översätta hello-begäran till en anropet toohello uppdateringsmetoden av hello-leverantör.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-364">hello Microsoft Common Language Infrastructure libraries for implementing SCIM services would translate hello request into a call toohello Update method of hello service’s provider.</span></span> <span data-ttu-id="f5fc0-365">Här är hello signaturen för hello metoden Update:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-365">Here is hello signature of hello Update method:</span></span> 
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
    <span data-ttu-id="f5fc0-366">I en begäran tooupdate användaren hello exempelvis har hello-objekt som angetts för hello värde för hello korrigering argumentet egenskapsvärdena:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-366">In hello example of a request tooupdate a user, hello object provided as hello value of hello patch argument has these property values:</span></span> 
  
  * <span data-ttu-id="f5fc0-367">ResourceIdentifier.Identifier: ”54D382A4-2050-4C03-94D1-E769F1D15682”</span><span class="sxs-lookup"><span data-stu-id="f5fc0-367">ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span></span>
  * <span data-ttu-id="f5fc0-368">ResourceIdentifier.SchemaIdentifier: ”urn: ietf:params:scim:schemas:extension:enterprise:2.0:User”</span><span class="sxs-lookup"><span data-stu-id="f5fc0-368">ResourceIdentifier.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"</span></span>
  * <span data-ttu-id="f5fc0-369">(PatchRequest som PatchRequest2). Operations.Count: 1</span><span class="sxs-lookup"><span data-stu-id="f5fc0-369">(PatchRequest as PatchRequest2).Operations.Count: 1</span></span>
  * <span data-ttu-id="f5fc0-370">(PatchRequest som PatchRequest2). Operations.ElementAt(0). OperationName: OperationName.Add</span><span class="sxs-lookup"><span data-stu-id="f5fc0-370">(PatchRequest as PatchRequest2).Operations.ElementAt(0).OperationName: OperationName.Add</span></span>
  * <span data-ttu-id="f5fc0-371">(PatchRequest som PatchRequest2). Operations.ElementAt(0). Path.AttributePath: ”manager”</span><span class="sxs-lookup"><span data-stu-id="f5fc0-371">(PatchRequest as PatchRequest2).Operations.ElementAt(0).Path.AttributePath: "manager"</span></span>
  * <span data-ttu-id="f5fc0-372">(PatchRequest som PatchRequest2). Operations.ElementAt(0). Value.Count: 1</span><span class="sxs-lookup"><span data-stu-id="f5fc0-372">(PatchRequest as PatchRequest2).Operations.ElementAt(0).Value.Count: 1</span></span>
  * <span data-ttu-id="f5fc0-373">(PatchRequest som PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Referens: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646</span><span class="sxs-lookup"><span data-stu-id="f5fc0-373">(PatchRequest as PatchRequest2).Operations.ElementAt(0).Value.ElementAt(0).Reference: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646</span></span>
  * <span data-ttu-id="f5fc0-374">(PatchRequest som PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Värde: 2819c223-7f76-453a-919d-413861904646</span><span class="sxs-lookup"><span data-stu-id="f5fc0-374">(PatchRequest as PatchRequest2).Operations.ElementAt(0).Value.ElementAt(0).Value: 2819c223-7f76-453a-919d-413861904646</span></span>

6. <span data-ttu-id="f5fc0-375">toode etablera en användare från en butik identitet fronted av SCIM-tjänsten, Azure AD skickar en begäran som:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-375">toode-provision a user from an identity store fronted by an SCIM service, Azure AD sends a request such as:</span></span> 
  ````
    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  <span data-ttu-id="f5fc0-376">Om hello-tjänsten har skapats med hello vanlig infrastruktur för språket bibliotek som tillhandahålls av Microsoft för att implementera SCIM tjänster, översätts hello-begäran till en anropet toohello Delete-metoden för hello-leverantör.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-376">If hello service was built using hello Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, then hello request is translated into a call toohello Delete method of hello service’s provider.</span></span>   <span data-ttu-id="f5fc0-377">Den här metoden har den här signaturen:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-377">That method has this signature:</span></span> 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
  ````
  <span data-ttu-id="f5fc0-378">hello-objekt som angetts för hello värde för hello resourceIdentifier argumentet har egenskapsvärdena i hello exempel på en begäran toode-etablera en användare:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-378">hello object provided as hello value of hello resourceIdentifier argument has these property values in hello example of a request toode-provision a user:</span></span> 
  
  * <span data-ttu-id="f5fc0-379">ResourceIdentifier.Identifier: ”54D382A4-2050-4C03-94D1-E769F1D15682”</span><span class="sxs-lookup"><span data-stu-id="f5fc0-379">ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span></span>
  * <span data-ttu-id="f5fc0-380">ResourceIdentifier.SchemaIdentifier: ”urn: ietf:params:scim:schemas:extension:enterprise:2.0:User”</span><span class="sxs-lookup"><span data-stu-id="f5fc0-380">ResourceIdentifier.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"</span></span>

## <a name="group-provisioning-and-de-provisioning"></a><span data-ttu-id="f5fc0-381">Gruppen etablering och avetablering</span><span class="sxs-lookup"><span data-stu-id="f5fc0-381">Group provisioning and de-provisioning</span></span>
<span data-ttu-id="f5fc0-382">Följande bild visar hälsningsmeddelande att Azure AcD skickar tooa SCIM service toomanage hello livscykeln för en grupp i en annan identitet store hello.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-382">hello following illustration shows hello messages that Azure AcD sends tooa SCIM service toomanage hello lifecycle of a group in another identity store.</span></span>  <span data-ttu-id="f5fc0-383">Dessa meddelanden skiljer sig från hälsningsmeddelande som rör toousers på tre sätt:</span><span class="sxs-lookup"><span data-stu-id="f5fc0-383">Those messages differ from hello messages pertaining toousers in three ways:</span></span> 

* <span data-ttu-id="f5fc0-384">hello schemat för en gruppresurs identifieras som http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-384">hello schema of a group resource is identified as http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.</span></span>  
* <span data-ttu-id="f5fc0-385">Begäranden tooretrieve grupper fastställa hello medlemmar attributet är toobe uteslutas från alla resurser som finns i svaret toohello begäran.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-385">Requests tooretrieve groups stipulate that hello members attribute is toobe excluded from any resource provided in response toohello request.</span></span>  
* <span data-ttu-id="f5fc0-386">Begäranden toodetermine om ett referensattribut har ett visst värde är begäranden om hello medlemmar attribut.</span><span class="sxs-lookup"><span data-stu-id="f5fc0-386">Requests toodetermine whether a reference attribute has a certain value are requests about hello members attribute.</span></span>  

<span data-ttu-id="f5fc0-387">![][5]
*Bild 6: Grupp etablering och avetablering sekvens*</span><span class="sxs-lookup"><span data-stu-id="f5fc0-387">![][5]
*Figure 6: Group provisioning and de-provisioning sequence*</span></span>

## <a name="related-articles"></a><span data-ttu-id="f5fc0-388">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="f5fc0-388">Related articles</span></span>
* [<span data-ttu-id="f5fc0-389">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f5fc0-389">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="f5fc0-390">Automatisera användaren etablering/avetablering tooSaaS appar</span><span class="sxs-lookup"><span data-stu-id="f5fc0-390">Automate User Provisioning/Deprovisioning tooSaaS Apps</span></span>](active-directory-saas-app-provisioning.md)
* [<span data-ttu-id="f5fc0-391">Anpassa attributmappning för Användaretablering</span><span class="sxs-lookup"><span data-stu-id="f5fc0-391">Customizing Attribute Mappings for User Provisioning</span></span>](active-directory-saas-customizing-attribute-mappings.md)
* [<span data-ttu-id="f5fc0-392">Skriva uttryck för attributmappning</span><span class="sxs-lookup"><span data-stu-id="f5fc0-392">Writing Expressions for Attribute Mappings</span></span>](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [<span data-ttu-id="f5fc0-393">Omfångsfilter för Användaretablering</span><span class="sxs-lookup"><span data-stu-id="f5fc0-393">Scoping Filters for User Provisioning</span></span>](active-directory-saas-scoping-filters.md)
* [<span data-ttu-id="f5fc0-394">Kontot etablering meddelanden</span><span class="sxs-lookup"><span data-stu-id="f5fc0-394">Account Provisioning Notifications</span></span>](active-directory-saas-account-provisioning-notifications.md)
* [<span data-ttu-id="f5fc0-395">Lista över självstudier om hur tooIntegrate SaaS-appar</span><span class="sxs-lookup"><span data-stu-id="f5fc0-395">List of Tutorials on How tooIntegrate SaaS Apps</span></span>](active-directory-saas-tutorial-list.md)

<!--Image references-->
[0]: ./media/active-directory-scim-provisioning/scim-figure-1.PNG
[1]: ./media/active-directory-scim-provisioning/scim-figure-2a.PNG
[2]: ./media/active-directory-scim-provisioning/scim-figure-2b.PNG
[3]: ./media/active-directory-scim-provisioning/scim-figure-3.PNG
[4]: ./media/active-directory-scim-provisioning/scim-figure-4.PNG
[5]: ./media/active-directory-scim-provisioning/scim-figure-5.PNG
