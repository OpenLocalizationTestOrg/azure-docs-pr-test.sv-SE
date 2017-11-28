---
title: "aaaManage din första API i Azure API Management | Microsoft Docs"
description: "Lär dig hur toocreate API: er, lägga till åtgärder och kom igång med API-hantering."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 51b7df8b-1c43-43c6-90c9-0aa24f48206b
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 7d43f33aa359c4d1e605e9fb41e43d323ca6a777
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-first-api-in-azure-api-management"></a><span data-ttu-id="e09ea-103">Hantera ditt första API i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="e09ea-103">Manage your first API in Azure API Management</span></span>
## <span data-ttu-id="e09ea-104"><a name="overview"> </a>Översikt</span><span class="sxs-lookup"><span data-stu-id="e09ea-104"><a name="overview"> </a>Overview</span></span>
<span data-ttu-id="e09ea-105">Den här guiden visar hur tooquickly Kom igång med Azure API Management och göra din första API-anrop.</span><span class="sxs-lookup"><span data-stu-id="e09ea-105">This guide shows you how tooquickly get started in using Azure API Management and make your first API call.</span></span>

## <span data-ttu-id="e09ea-106"><a name="concepts"> </a>Vad är Azure API Management?</span><span class="sxs-lookup"><span data-stu-id="e09ea-106"><a name="concepts"> </a>What is Azure API Management?</span></span>
<span data-ttu-id="e09ea-107">Du kan använda Azure API Management tootake alla serverdelar och starta en komplett API-programmet baseras på den.</span><span class="sxs-lookup"><span data-stu-id="e09ea-107">You can use Azure API Management tootake any backend and launch a full-fledged API program based on it.</span></span>

<span data-ttu-id="e09ea-108">Här följer exempel på några vanliga scenarier:</span><span class="sxs-lookup"><span data-stu-id="e09ea-108">Common scenarios include:</span></span>

* <span data-ttu-id="e09ea-109">**Skydda den mobila infrastrukturen** genom att hantera åtkomsten med API-nycklar, förhindra DOS-attacker med hjälp av begränsningar och använda avancerade säkerhetsprinciper som JWT-tokenvalidering.</span><span class="sxs-lookup"><span data-stu-id="e09ea-109">**Securing mobile infrastructure** by gating access with API keys, preventing DOS attacks by using throttling, or using advanced security policies like JWT token validation.</span></span>
* <span data-ttu-id="e09ea-110">**Aktivera ISV partner ekosystem** genom att erbjuda snabb partner onboarding via hello developer portal och bygga ett API facade toodecouple från interna implementeringar som inte är mogna för partner förbrukning.</span><span class="sxs-lookup"><span data-stu-id="e09ea-110">**Enabling ISV partner ecosystems** by offering fast partner onboarding through hello developer portal and building an API facade toodecouple from internal implementations that are not ripe for partner consumption.</span></span>
* <span data-ttu-id="e09ea-111">**Ett internt API-program körs** genom att erbjuda en central plats för hello organisation toocommunicate om hello tillgänglighet och senaste ändras tooAPIs, gating åtkomst baserat på organisationskonton, alla baserat på en skyddad kanal mellan hello API-gateway och hello backend.</span><span class="sxs-lookup"><span data-stu-id="e09ea-111">**Running an internal API program** by offering a centralized location for hello organization toocommunicate about hello availability and latest changes tooAPIs, gating access based on organizational accounts, all based on a secured channel between hello API gateway and hello backend.</span></span>

<span data-ttu-id="e09ea-112">hello system består av hello följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="e09ea-112">hello system is made up of hello following components:</span></span>

* <span data-ttu-id="e09ea-113">Hej **API gateway** är hello slutpunkt som:</span><span class="sxs-lookup"><span data-stu-id="e09ea-113">hello **API gateway** is hello endpoint that:</span></span>
  
  * <span data-ttu-id="e09ea-114">Accepterar API-anrop och skickar dem tooyour serverdelar.</span><span class="sxs-lookup"><span data-stu-id="e09ea-114">Accepts API calls and routes them tooyour backends.</span></span>
  * <span data-ttu-id="e09ea-115">Verifierar API-nycklar, JWT-token, certifikat och andra autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="e09ea-115">Verifies API keys, JWT tokens, certificates, and other credentials.</span></span>
  * <span data-ttu-id="e09ea-116">Tillämpar användningskvoter och hastighetsbegränsningar.</span><span class="sxs-lookup"><span data-stu-id="e09ea-116">Enforces usage quotas and rate limits.</span></span>
  * <span data-ttu-id="e09ea-117">Transformerar ditt API på hello direkt utan kod ändringar.</span><span class="sxs-lookup"><span data-stu-id="e09ea-117">Transforms your API on hello fly without code modifications.</span></span>
  * <span data-ttu-id="e09ea-118">Cachelagrar backend-svar om detta konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="e09ea-118">Caches backend responses where set up.</span></span>
  * <span data-ttu-id="e09ea-119">Loggar metadata i anrop för analysändamål.</span><span class="sxs-lookup"><span data-stu-id="e09ea-119">Logs call metadata for analytics purposes.</span></span>
* <span data-ttu-id="e09ea-120">Hej **publisher portal** är hello administrationsgränssnittet där du ställa in API-program.</span><span class="sxs-lookup"><span data-stu-id="e09ea-120">hello **publisher portal** is hello administrative interface where you set up your API program.</span></span> <span data-ttu-id="e09ea-121">Använd portalen om du vill:</span><span class="sxs-lookup"><span data-stu-id="e09ea-121">Use it to:</span></span>
  
  * <span data-ttu-id="e09ea-122">Definiera eller importera API-schemat.</span><span class="sxs-lookup"><span data-stu-id="e09ea-122">Define or import API schema.</span></span>
  * <span data-ttu-id="e09ea-123">Paketera API:er till produkter.</span><span class="sxs-lookup"><span data-stu-id="e09ea-123">Package APIs into products.</span></span>
  * <span data-ttu-id="e09ea-124">Konfigurera principer som kvoter eller omformningar på hello API: er.</span><span class="sxs-lookup"><span data-stu-id="e09ea-124">Set up policies like quotas or transformations on hello APIs.</span></span>
  * <span data-ttu-id="e09ea-125">Få insikter från analyser.</span><span class="sxs-lookup"><span data-stu-id="e09ea-125">Get insights from analytics.</span></span>
  * <span data-ttu-id="e09ea-126">Hantera användare.</span><span class="sxs-lookup"><span data-stu-id="e09ea-126">Manage users.</span></span>
* <span data-ttu-id="e09ea-127">Hej **utvecklarportalen** fungerar som hello huvudsakliga webben för utvecklare, där de kan:</span><span class="sxs-lookup"><span data-stu-id="e09ea-127">hello **developer portal** serves as hello main web presence for developers, where they can:</span></span>
  
  * <span data-ttu-id="e09ea-128">Få tillgång till API-dokumentation.</span><span class="sxs-lookup"><span data-stu-id="e09ea-128">Read API documentation.</span></span>
  * <span data-ttu-id="e09ea-129">Prova att använda en API via interaktiva hello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="e09ea-129">Try out an API via hello interactive console.</span></span>
  * <span data-ttu-id="e09ea-130">Skapa ett konto och prenumerera tooget API-nycklar.</span><span class="sxs-lookup"><span data-stu-id="e09ea-130">Create an account and subscribe tooget API keys.</span></span>
  * <span data-ttu-id="e09ea-131">Komma åt analyser om deras egen användning.</span><span class="sxs-lookup"><span data-stu-id="e09ea-131">Access analytics on their own usage.</span></span>

## <span data-ttu-id="e09ea-132"><a name="create-service-instance"> </a>Skapa en API Management-instans</span><span class="sxs-lookup"><span data-stu-id="e09ea-132"><a name="create-service-instance"> </a>Create an API Management instance</span></span>
> [!NOTE]
> <span data-ttu-id="e09ea-133">toocomplete den här självstudiekursen kommer du behöver ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="e09ea-133">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="e09ea-134">Om du inte har något konto kan du skapa ett kostnadsfritt konto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="e09ea-134">If you don't have an account, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="e09ea-135">Mer information finns i [kostnadsfri utvärderingsversion av Azure][Azure Free Trial].</span><span class="sxs-lookup"><span data-stu-id="e09ea-135">For details, see [Azure Free Trial][Azure Free Trial].</span></span>
> 
> 

<span data-ttu-id="e09ea-136">hello första steget i att arbeta med API-hantering är toocreate en instans av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="e09ea-136">hello first step in working with API Management is toocreate a service instance.</span></span> <span data-ttu-id="e09ea-137">Logga in toohello [Azure Portal] [ Azure Portal] och på **ny**, **webb + mobilt**, **API Management**.</span><span class="sxs-lookup"><span data-stu-id="e09ea-137">Sign in toohello [Azure Portal][Azure Portal] and click **New**, **Web + Mobile**, **API Management**.</span></span>

![Ny API Management-instans][api-management-create-instance-menu]

<span data-ttu-id="e09ea-139">För **namn**, ange en unik underordnade domän namnet toouse för hello tjänst-URL.</span><span class="sxs-lookup"><span data-stu-id="e09ea-139">For **Name**, specify a unique sub-domain name toouse for hello service URL.</span></span>

<span data-ttu-id="e09ea-140">Välj önskad hello **prenumeration**, **resursgruppen** och **plats** för service-instans.</span><span class="sxs-lookup"><span data-stu-id="e09ea-140">Choose hello desired **Subscription**, **Resource group** and **Location** for your service instance.</span></span>

<span data-ttu-id="e09ea-141">Ange **Contoso AB** för hello **organisationsnamn**, och ange din e-postadress i hello **administratör e-post** fältet.</span><span class="sxs-lookup"><span data-stu-id="e09ea-141">Enter **Contoso Ltd.** for hello **Organization Name**, and enter your email address in hello **Administrator E-Mail** field.</span></span>

> [!NOTE]
> <span data-ttu-id="e09ea-142">E-postadressen används för meddelanden från hello API Management-systemet.</span><span class="sxs-lookup"><span data-stu-id="e09ea-142">This email address is used for notifications from hello API Management system.</span></span> <span data-ttu-id="e09ea-143">Mer information finns i [hur tooconfigure meddelanden och e-mallar i Azure API Management][How tooconfigure notifications and email templates in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="e09ea-143">For more information, see [How tooconfigure notifications and email templates in Azure API Management][How tooconfigure notifications and email templates in Azure API Management].</span></span>
> 
> 

![Ny API Management-tjänst][api-management-create-instance-step1]

<span data-ttu-id="e09ea-145">API Management-tjänstinstanser är tillgängliga på tre nivåer: Developer, Standard och Premium.</span><span class="sxs-lookup"><span data-stu-id="e09ea-145">API Management service instances are available in three tiers: Developer, Standard, and Premium.</span></span>

> [!NOTE]
> <span data-ttu-id="e09ea-146">hello Developer nivå är för utveckling, testning och API pilotprogram där hög tillgänglighet inte är ett problem.</span><span class="sxs-lookup"><span data-stu-id="e09ea-146">hello Developer Tier is for development, testing, and pilot API programs where high availability is not a concern.</span></span> <span data-ttu-id="e09ea-147">I hello Standard och Premium-nivåer, kan du skala din enhet antal toohandle mer trafik.</span><span class="sxs-lookup"><span data-stu-id="e09ea-147">In hello Standard and Premium tiers, you can scale your reserved unit count toohandle more traffic.</span></span> <span data-ttu-id="e09ea-148">hello Standard och Premium nivåer ge din API Management-tjänsten med hello de flesta processorkraft och prestanda.</span><span class="sxs-lookup"><span data-stu-id="e09ea-148">hello Standard and Premium tiers provide your API Management service with hello most processing power and performance.</span></span> <span data-ttu-id="e09ea-149">Du kan genomföra den här självstudiekursen med valfri nivå.</span><span class="sxs-lookup"><span data-stu-id="e09ea-149">You can complete this tutorial by using any tier.</span></span> <span data-ttu-id="e09ea-150">Mer information om API Management-nivåer finns i avsnittet om [API Management-priser][API Management pricing].</span><span class="sxs-lookup"><span data-stu-id="e09ea-150">For more information about API Management tiers, see [API Management pricing][API Management pricing].</span></span>
> 
> 

<span data-ttu-id="e09ea-151">Klicka på **skapa** toostart etablering service-instans.</span><span class="sxs-lookup"><span data-stu-id="e09ea-151">Click **Create** toostart provisioning your service instance.</span></span>

![Ny API Management-tjänst][api-management-instance-created]

<span data-ttu-id="e09ea-153">När du har skapat hello tjänstinstans hello nästa steg är toocreate eller importerar en API.</span><span class="sxs-lookup"><span data-stu-id="e09ea-153">Once hello service instance is created, hello next step is toocreate or import an API.</span></span>

## <span data-ttu-id="e09ea-154"><a name="create-api"> </a>Importera ett API</span><span class="sxs-lookup"><span data-stu-id="e09ea-154"><a name="create-api"> </a>Import an API</span></span>
<span data-ttu-id="e09ea-155">Ett API består av en uppsättning åtgärder som kan anropas från ett klientprogram.</span><span class="sxs-lookup"><span data-stu-id="e09ea-155">An API consists of a set of operations that can be invoked from a client application.</span></span> <span data-ttu-id="e09ea-156">API: et är via proxy tooexisting webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="e09ea-156">API operations are proxied tooexisting web services.</span></span>

<span data-ttu-id="e09ea-157">API:er kan skapas (och åtgärder kan läggas till) manuellt eller importeras.</span><span class="sxs-lookup"><span data-stu-id="e09ea-157">APIs can be created (and operations can be added) manually, or they can be imported.</span></span> <span data-ttu-id="e09ea-158">I den här självstudiekursen kommer vi importera hello API för en exempel Kalkylatorn-webbtjänst som tillhandahålls av Microsoft och finns på Azure.</span><span class="sxs-lookup"><span data-stu-id="e09ea-158">In this tutorial, we will import hello API for a sample calculator web service provided by Microsoft and hosted on Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="e09ea-159">Anvisningar för att skapa en API och manuellt lägga till operations finns [hur toocreate API: er](api-management-howto-create-apis.md) och [hur tooadd operations tooan API](api-management-howto-add-operations.md).</span><span class="sxs-lookup"><span data-stu-id="e09ea-159">For guidance on creating an API and manually adding operations, see [How toocreate APIs](api-management-howto-create-apis.md) and [How tooadd operations tooan API](api-management-howto-add-operations.md).</span></span>
> 
> 

<span data-ttu-id="e09ea-160">API: er konfigureras från hello publisher-portalen.</span><span class="sxs-lookup"><span data-stu-id="e09ea-160">APIs are configured from hello publisher portal.</span></span> <span data-ttu-id="e09ea-161">tooreach, klickar du på **Publisher portal** hello service verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="e09ea-161">tooreach it, click **Publisher portal** from hello service toolbar.</span></span>

![Utgivarportalen][api-management-management-console]

<span data-ttu-id="e09ea-163">tooimport hello Kalkylatorn API, klickar du på **API: er** från hello **API Management** menyn på hello vänster och klicka sedan på **Import API**.</span><span class="sxs-lookup"><span data-stu-id="e09ea-163">tooimport hello calculator API, click **APIs** from hello **API Management** menu on hello left, and then click **Import API**.</span></span>

![Knappen Importera API][api-management-import-api]

<span data-ttu-id="e09ea-165">Utför hello följande steg tooconfigure hello Kalkylatorn API:</span><span class="sxs-lookup"><span data-stu-id="e09ea-165">Perform hello following steps tooconfigure hello calculator API:</span></span>

1. <span data-ttu-id="e09ea-166">Klicka på **från URL: en**, ange **http://calcapi.cloudapp.net/calcapi.json** till hello **specifikation dokumentet URL** text och på hello **Swagger**  knappen.</span><span class="sxs-lookup"><span data-stu-id="e09ea-166">Click **From URL**, enter **http://calcapi.cloudapp.net/calcapi.json** into hello **Specification document URL** text box, and click hello **Swagger** radio button.</span></span>
2. <span data-ttu-id="e09ea-167">Typen **beräkna** till hello **URL för Web API-suffix** textruta.</span><span class="sxs-lookup"><span data-stu-id="e09ea-167">Type **calc** into hello **Web API URL suffix** text box.</span></span>
3. <span data-ttu-id="e09ea-168">Klicka i hello **produkter (valfritt)** och väljer **Starter**.</span><span class="sxs-lookup"><span data-stu-id="e09ea-168">Click in hello **Products (optional)** box and choose **Starter**.</span></span>
4. <span data-ttu-id="e09ea-169">Klicka på **spara** tooimport hello API.</span><span class="sxs-lookup"><span data-stu-id="e09ea-169">Click **Save** tooimport hello API.</span></span>

![Lägga till ett nytt API][api-management-import-new-api]

> [!NOTE]
> <span data-ttu-id="e09ea-171">**API Management** stöder för närvarande både 1.2- och 2.0-versionen av Swagger-dokument för importändamål.</span><span class="sxs-lookup"><span data-stu-id="e09ea-171">**API Management** currently supports both 1.2 and 2.0 version of Swagger document for import.</span></span> <span data-ttu-id="e09ea-172">Egenskaperna `host`, `basePath` och `schemes` är valfria enligt [Swagger 2.0-specifikationen](http://swagger.io/specification), men ditt Swagger 2.0-dokument **MÅSTE** innehålla dessa egenskaper för att importeras.</span><span class="sxs-lookup"><span data-stu-id="e09ea-172">Make sure that, even though [Swagger 2.0 specification](http://swagger.io/specification) declares that `host`, `basePath`, and `schemes` properties are optional, your Swagger 2.0 document **MUST** contain those properties; otherwise it won't get imported.</span></span> 
> 
> 

<span data-ttu-id="e09ea-173">När du har importerat hello API visas hello sammanfattningssidan hello-API: t i hello publisher-portalen.</span><span class="sxs-lookup"><span data-stu-id="e09ea-173">Once hello API is imported, hello summary page for hello API is displayed in hello publisher portal.</span></span>

![API-sammanfattning][api-management-imported-api-summary]

<span data-ttu-id="e09ea-175">hello API-avsnitt har flera flikar.</span><span class="sxs-lookup"><span data-stu-id="e09ea-175">hello API section has several tabs.</span></span> <span data-ttu-id="e09ea-176">Hej **sammanfattning** fliken visas information om hello API och grundläggande mått.</span><span class="sxs-lookup"><span data-stu-id="e09ea-176">hello **Summary** tab displays basic metrics and information about hello API.</span></span> <span data-ttu-id="e09ea-177">Hej [inställningar](api-management-howto-create-apis.md#configure-api-settings) fliken har konfigurerats att använda tooview och redigera hello API.</span><span class="sxs-lookup"><span data-stu-id="e09ea-177">hello [Settings](api-management-howto-create-apis.md#configure-api-settings) tab is used tooview and edit hello configuration for an API.</span></span> <span data-ttu-id="e09ea-178">Hej [Operations](api-management-howto-add-operations.md) är operations används toomanage hello API: er.</span><span class="sxs-lookup"><span data-stu-id="e09ea-178">hello [Operations](api-management-howto-add-operations.md) tab is used toomanage hello API's operations.</span></span> <span data-ttu-id="e09ea-179">Hej **säkerhet** flik kan vara används tooconfigure gateway-autentisering för hello backend-servern med hjälp av grundläggande autentisering eller [ömsesidig autentisering](api-management-howto-mutual-certificates.md), och tooconfigure [ Auktoriseringen av användaren med hjälp av OAuth 2.0](api-management-howto-oauth2.md).</span><span class="sxs-lookup"><span data-stu-id="e09ea-179">hello **Security** tab can be used tooconfigure gateway authentication for hello backend server by using Basic authentication or [mutual certificate authentication](api-management-howto-mutual-certificates.md), and tooconfigure [user authorization by using OAuth 2.0](api-management-howto-oauth2.md).</span></span>  <span data-ttu-id="e09ea-180">Hej **problem** är används tooview problem som rapporteras av hello utvecklare som använder dina API: er.</span><span class="sxs-lookup"><span data-stu-id="e09ea-180">hello **Issues** tab is used tooview issues reported by hello developers who are using your APIs.</span></span> <span data-ttu-id="e09ea-181">Hej **produkter** är används tooconfigure hello produkter som innehåller detta API.</span><span class="sxs-lookup"><span data-stu-id="e09ea-181">hello **Products** tab is used tooconfigure hello products that contain this API.</span></span>

<span data-ttu-id="e09ea-182">Som standard medföljer två exempelprodukter varje API Management-instans:</span><span class="sxs-lookup"><span data-stu-id="e09ea-182">By default, each API Management instance comes with two sample products:</span></span>

* <span data-ttu-id="e09ea-183">**Starter**</span><span class="sxs-lookup"><span data-stu-id="e09ea-183">**Starter**</span></span>
* <span data-ttu-id="e09ea-184">**Obegränsat**</span><span class="sxs-lookup"><span data-stu-id="e09ea-184">**Unlimited**</span></span>

<span data-ttu-id="e09ea-185">I den här självstudiekursen lades hello grundläggande Kalkylatorn API toohello Starter produkten när hello API importerades.</span><span class="sxs-lookup"><span data-stu-id="e09ea-185">In this tutorial, hello Basic Calculator API was added toohello Starter product when hello API was imported.</span></span>

<span data-ttu-id="e09ea-186">Utvecklare måste först prenumerera tooa produkt som ger dem åtkomst tooit i ordning toomake anrop tooan API.</span><span class="sxs-lookup"><span data-stu-id="e09ea-186">In order toomake calls tooan API, developers must first subscribe tooa product that gives them access tooit.</span></span> <span data-ttu-id="e09ea-187">Utvecklare kan prenumerera på tooproducts i hello developer-portalen eller administratörer kan prenumerera på utvecklare tooproducts hello publisher-portalen.</span><span class="sxs-lookup"><span data-stu-id="e09ea-187">Developers can subscribe tooproducts in hello developer portal, or administrators can subscribe developers tooproducts in hello publisher portal.</span></span> <span data-ttu-id="e09ea-188">Du är administratör sedan du skapade hello API Management-instans i hello föregående steg i självstudiekursen hello så att du redan prenumererar på tooevery produkten som standard.</span><span class="sxs-lookup"><span data-stu-id="e09ea-188">You are an administrator since you created hello API Management instance in hello previous steps in hello tutorial, so you are already subscribed tooevery product by default.</span></span>

## <span data-ttu-id="e09ea-189"><a name="call-operation"></a>Anropa en åtgärd från hello developer-portalen</span><span class="sxs-lookup"><span data-stu-id="e09ea-189"><a name="call-operation"> </a>Call an operation from hello developer portal</span></span>
<span data-ttu-id="e09ea-190">Åtgärder kan anropas direkt från hello developer-portalen, vilket ger ett bekvämt sätt tooview och testa hello driften av ett API.</span><span class="sxs-lookup"><span data-stu-id="e09ea-190">Operations can be called directly from hello developer portal, which provides a convenient way tooview and test hello operations of an API.</span></span> <span data-ttu-id="e09ea-191">I den här självstudiekursen steg ska du anropa hello grundläggande Kalkylatorn API: er **lägga till två heltal** igen.</span><span class="sxs-lookup"><span data-stu-id="e09ea-191">In this tutorial step, you will call hello Basic Calculator API's **Add two integers** operation.</span></span> <span data-ttu-id="e09ea-192">Klicka på **Developer-portalen** hello-menyn på hello övre högra hello publisher portalen.</span><span class="sxs-lookup"><span data-stu-id="e09ea-192">Click **Developer portal** from hello menu at hello top right of hello publisher portal.</span></span>

![Utvecklarportalen][api-management-developer-portal-menu]

<span data-ttu-id="e09ea-194">Klicka på **API: er** hello översta menyn och klicka sedan på **grundläggande Kalkylatorn** toosee hello tillgängliga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="e09ea-194">Click **APIs** from hello top menu, and then click **Basic Calculator** toosee hello available operations.</span></span>

![Utvecklarportalen][api-management-developer-portal-calc-api]

<span data-ttu-id="e09ea-196">Observera hello exempel beskrivningar och parametrarna som har importerats tillsammans med hello API och åtgärder som att tillhandahålla dokumentation för hello utvecklare som använder den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="e09ea-196">Note hello sample descriptions and parameters that were imported along with hello API and operations, providing documentation for hello developers that will use this operation.</span></span> <span data-ttu-id="e09ea-197">Dessa beskrivningar kan även läggas till om åtgärder läggs till manuellt.</span><span class="sxs-lookup"><span data-stu-id="e09ea-197">These descriptions can also be added when operations are added manually.</span></span>

<span data-ttu-id="e09ea-198">toocall hello **lägga till två heltal** åtgärden, klickar du på **prova**.</span><span class="sxs-lookup"><span data-stu-id="e09ea-198">toocall hello **Add two integers** operation, click **Try it**.</span></span>

![Prova][api-management-developer-portal-calc-api-console]

<span data-ttu-id="e09ea-200">Du kan ange vissa värden för hello parametrar eller hålla hello standardvärden och klicka sedan på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="e09ea-200">You can enter some values for hello parameters or keep hello defaults, and then click **Send**.</span></span>

![HTTP Get][api-management-invoke-get]

<span data-ttu-id="e09ea-202">När en åtgärd har anropats hello developer-portalen visar hello **svarsstatusen**, hello **svarshuvuden**, och en **svar innehåll**.</span><span class="sxs-lookup"><span data-stu-id="e09ea-202">After an operation is invoked, hello developer portal displays hello **Response status**, hello **Response headers**, and any **Response content**.</span></span>

![Svar][api-management-invoke-get-response]

## <span data-ttu-id="e09ea-204"><a name="view-analytics"> </a>Visa analys</span><span class="sxs-lookup"><span data-stu-id="e09ea-204"><a name="view-analytics"> </a>View analytics</span></span>
<span data-ttu-id="e09ea-205">tooview analytics för grundläggande Kalkylatorn växla tillbaka toohello publisher portal genom att välja **hantera** hello-menyn på hello uppifrån höger om hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="e09ea-205">tooview analytics for Basic Calculator, switch back toohello publisher portal by selecting **Manage** from hello menu at hello top right of hello developer portal.</span></span>

![Hantera][api-management-manage-menu]

<span data-ttu-id="e09ea-207">hello standardvyn för hello publisher portal är hello **instrumentpanelen**, vilket ger en översikt över API Management-instans.</span><span class="sxs-lookup"><span data-stu-id="e09ea-207">hello default view for hello publisher portal is hello **Dashboard**, which provides an overview of your API Management instance.</span></span>

![Instrumentpanel][api-management-dashboard]

<span data-ttu-id="e09ea-209">Hovra hello muspekaren över hello diagram för **grundläggande Kalkylatorn** toosee hello specifika mått för hello användning av hello API för en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="e09ea-209">Hover hello mouse over hello chart for **Basic Calculator** toosee hello specific metrics for hello usage of hello API for a given time period.</span></span>

> [!NOTE]
> <span data-ttu-id="e09ea-210">Om du inte ser några rader i diagrammet, växla tillbaka toohello developer-portalen och vissa anrop till hello API, vänta en stund och sedan gå tillbaka toohello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="e09ea-210">If you don't see any lines on your chart, switch back toohello developer portal and make some calls into hello API, wait a few moments, and then come back toohello dashboard.</span></span>
> 
> 

<span data-ttu-id="e09ea-211">Klicka på **visa information** tooview hello sammanfattningssidan för hello API, inklusive en större version av hello visas mått.</span><span class="sxs-lookup"><span data-stu-id="e09ea-211">Click **View Details** tooview hello summary page for hello API, including a larger version of hello displayed metrics.</span></span>

![Analys][api-management-mouse-over]

![Sammanfattning][api-management-api-summary-metrics]

<span data-ttu-id="e09ea-214">För detaljerad mätvärden och rapporter, klickar du på **Analytics** från hello **API Management** menyn hello vänster.</span><span class="sxs-lookup"><span data-stu-id="e09ea-214">For detailed metrics and reports, click **Analytics** from hello **API Management** menu on hello left.</span></span>

![Översikt][api-management-analytics-overview]

<span data-ttu-id="e09ea-216">Hej **Analytics** avsnitt har hello följande fyra flikar:</span><span class="sxs-lookup"><span data-stu-id="e09ea-216">hello **Analytics** section has hello following four tabs:</span></span>

* <span data-ttu-id="e09ea-217">**En överblick över** ger totala användningen och hälsotillstånd mått samt hello översta utvecklare, produkter, övre API: er och översta åtgärder.</span><span class="sxs-lookup"><span data-stu-id="e09ea-217">**At a glance** provides overall usage and health metrics, as well as hello top developers, top products, top APIs, and top operations.</span></span>
* <span data-ttu-id="e09ea-218">**Användning** innehåller detaljerad information om API-anrop och bandbredd, inklusive en geografisk representation.</span><span class="sxs-lookup"><span data-stu-id="e09ea-218">**Usage** provides an in-depth look at API calls and bandwidth, including a geographical representation.</span></span>
* <span data-ttu-id="e09ea-219">**Hälsa** fokuserar på statuskoder, lyckade cachelagringsåtgärder, svarstider och API- och tjänstsvarstider.</span><span class="sxs-lookup"><span data-stu-id="e09ea-219">**Health** focuses on status codes, cache success rates, response times, and API and service response times.</span></span>
* <span data-ttu-id="e09ea-220">**Aktiviteten** innehåller rapporter som detaljnivån om hello specifik aktivitet efter utvecklare, produkt, API och åtgärden.</span><span class="sxs-lookup"><span data-stu-id="e09ea-220">**Activity** provides reports that drill down on hello specific activity by developer, product, API, and operation.</span></span>

## <span data-ttu-id="e09ea-221"><a name="next-steps"> </a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e09ea-221"><a name="next-steps"> </a>Next steps</span></span>
* <span data-ttu-id="e09ea-222">Lär dig hur för[skydda din API med hastighetsbegränsningar](api-management-howto-product-with-rules.md).</span><span class="sxs-lookup"><span data-stu-id="e09ea-222">Learn how too[Protect your API with rate limits](api-management-howto-product-with-rules.md).</span></span>

[Azure Free Trial]: http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=api_management_hero_a

[Create an API Management instance]: #create-service-instance
[Create an API]: #create-api
[Add an operation]: #add-operation
[Add hello new API tooa product]: #add-api-to-product
[Subscribe toohello product that contains hello API]: #subscribe
[Call an operation from hello Developer Portal]: #call-operation
[View analytics]: #view-analytics
[Next steps]: #next-steps


[How toomanage developer accounts in Azure API Management]: api-management-howto-create-or-invite-developers.md
[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[How tooconfigure notifications and email templates in Azure API Management]: api-management-howto-configure-notifications.md
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md
[API Management pricing]: http://azure.microsoft.com/pricing/details/api-management/

[Azure Portal]: https://portal.azure.com/

[api-management-management-console]: ./media/api-management-get-started/api-management-management-console.png
[api-management-create-instance-menu]: ./media/api-management-get-started/api-management-create-instance-menu.png
[api-management-create-instance-step1]: ./media/api-management-get-started/api-management-create-instance-step1.png
[api-management-create-instance-step2]: ./media/api-management-get-started/api-management-create-instance-step2.png
[api-management-instance-created]: ./media/api-management-get-started/api-management-instance-created.png
[api-management-import-api]: ./media/api-management-get-started/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-get-started/api-management-import-new-api.png
[api-management-imported-api-summary]: ./media/api-management-get-started/api-management-imported-api-summary.png
[api-management-calc-operations]: ./media/api-management-get-started/api-management-calc-operations.png
[api-management-list-products]: ./media/api-management-get-started/api-management-list-products.png
[api-management-add-api-to-product]: ./media/api-management-get-started/api-management-add-api-to-product.png
[api-management-add-myechoapi-to-product]: ./media/api-management-get-started/api-management-add-myechoapi-to-product.png
[api-management-api-added-to-product]: ./media/api-management-get-started/api-management-api-added-to-product.png
[api-management-developers]: ./media/api-management-get-started/api-management-developers.png
[api-management-add-subscription]: ./media/api-management-get-started/api-management-add-subscription.png
[api-management-add-subscription-window]: ./media/api-management-get-started/api-management-add-subscription-window.png
[api-management-subscription-added]: ./media/api-management-get-started/api-management-subscription-added.png
[api-management-developer-portal-menu]: ./media/api-management-get-started/api-management-developer-portal-menu.png
[api-management-developer-portal-calc-api]: ./media/api-management-get-started/api-management-developer-portal-calc-api.png
[api-management-developer-portal-calc-api-console]: ./media/api-management-get-started/api-management-developer-portal-calc-api-console.png
[api-management-invoke-get]: ./media/api-management-get-started/api-management-invoke-get.png
[api-management-invoke-get-response]: ./media/api-management-get-started/api-management-invoke-get-response.png
[api-management-manage-menu]: ./media/api-management-get-started/api-management-manage-menu.png
[api-management-dashboard]: ./media/api-management-get-started/api-management-dashboard.png

[api-management-add-response]: ./media/api-management-get-started/api-management-add-response.png
[api-management-add-response-window]: ./media/api-management-get-started/api-management-add-response-window.png
[api-management-developer-key]: ./media/api-management-get-started/api-management-developer-key.png
[api-management-mouse-over]: ./media/api-management-get-started/api-management-mouse-over.png
[api-management-api-summary-metrics]: ./media/api-management-get-started/api-management-api-summary-metrics.png
[api-management-analytics-overview]: ./media/api-management-get-started/api-management-analytics-overview.png
[api-management-analytics-usage]: ./media/api-management-get-started/api-management-analytics-usage.png
[api-management-]: ./media/api-management-get-started/api-management-.png
[api-management-]: ./media/api-management-get-started/api-management-.png
