---
title: "Hantera ditt första API i Azure API Management | Microsoft Docs"
description: "Lär dig hur du skapar API:er, lägger till åtgärder och kommer igång med API Management."
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
ms.openlocfilehash: 6e76d1ee08f804637999ef2ebf5d25becf6a0408
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-your-first-api-in-azure-api-management"></a><span data-ttu-id="70d80-103">Hantera ditt första API i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="70d80-103">Manage your first API in Azure API Management</span></span>
## <span data-ttu-id="70d80-104"><a name="overview"> </a>Översikt</span><span class="sxs-lookup"><span data-stu-id="70d80-104"><a name="overview"> </a>Overview</span></span>
<span data-ttu-id="70d80-105">Den här guiden hjälper dig att snabbt komma igång med Azure API Management och att göra ditt första API-anrop.</span><span class="sxs-lookup"><span data-stu-id="70d80-105">This guide shows you how to quickly get started in using Azure API Management and make your first API call.</span></span>

## <span data-ttu-id="70d80-106"><a name="concepts"> </a>Vad är Azure API Management?</span><span class="sxs-lookup"><span data-stu-id="70d80-106"><a name="concepts"> </a>What is Azure API Management?</span></span>
<span data-ttu-id="70d80-107">Med Azure API Management kan du välja valfri serverdel och köra ett fullvärdigt API-program baserat på den.</span><span class="sxs-lookup"><span data-stu-id="70d80-107">You can use Azure API Management to take any backend and launch a full-fledged API program based on it.</span></span>

<span data-ttu-id="70d80-108">Här följer exempel på några vanliga scenarier:</span><span class="sxs-lookup"><span data-stu-id="70d80-108">Common scenarios include:</span></span>

* <span data-ttu-id="70d80-109">**Skydda den mobila infrastrukturen** genom att hantera åtkomsten med API-nycklar, förhindra DOS-attacker med hjälp av begränsningar och använda avancerade säkerhetsprinciper som JWT-tokenvalidering.</span><span class="sxs-lookup"><span data-stu-id="70d80-109">**Securing mobile infrastructure** by gating access with API keys, preventing DOS attacks by using throttling, or using advanced security policies like JWT token validation.</span></span>
* <span data-ttu-id="70d80-110">**Aktivera ISV-partnerekosystem** genom att erbjuda snabb partnerintegrering via utvecklarportalen och genom att bygga en API-fasad som är åtskild från interna implementeringar som inte är redo för partneranvändning än.</span><span class="sxs-lookup"><span data-stu-id="70d80-110">**Enabling ISV partner ecosystems** by offering fast partner onboarding through the developer portal and building an API facade to decouple from internal implementations that are not ripe for partner consumption.</span></span>
* <span data-ttu-id="70d80-111">**Kör ett internt API-program** genom att erbjuda en central plats för organisationen där man kan kommunicera kring tillgänglighet och de senaste ändringarna i API:er samt hantera åtkomsten baserat på organisationskonton – allt via en säker kanal mellan API-gatewayen och serverdelen.</span><span class="sxs-lookup"><span data-stu-id="70d80-111">**Running an internal API program** by offering a centralized location for the organization to communicate about the availability and latest changes to APIs, gating access based on organizational accounts, all based on a secured channel between the API gateway and the backend.</span></span>

<span data-ttu-id="70d80-112">Systemet består av följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="70d80-112">The system is made up of the following components:</span></span>

* <span data-ttu-id="70d80-113">**API-gatewayen** är slutpunkten som:</span><span class="sxs-lookup"><span data-stu-id="70d80-113">The **API gateway** is the endpoint that:</span></span>
  
  * <span data-ttu-id="70d80-114">Accepterar API-anrop och dirigerar dem till serverdelen.</span><span class="sxs-lookup"><span data-stu-id="70d80-114">Accepts API calls and routes them to your backends.</span></span>
  * <span data-ttu-id="70d80-115">Verifierar API-nycklar, JWT-token, certifikat och andra autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="70d80-115">Verifies API keys, JWT tokens, certificates, and other credentials.</span></span>
  * <span data-ttu-id="70d80-116">Tillämpar användningskvoter och hastighetsbegränsningar.</span><span class="sxs-lookup"><span data-stu-id="70d80-116">Enforces usage quotas and rate limits.</span></span>
  * <span data-ttu-id="70d80-117">Transformerar ditt API direkt utan kodändringar.</span><span class="sxs-lookup"><span data-stu-id="70d80-117">Transforms your API on the fly without code modifications.</span></span>
  * <span data-ttu-id="70d80-118">Cachelagrar backend-svar om detta konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="70d80-118">Caches backend responses where set up.</span></span>
  * <span data-ttu-id="70d80-119">Loggar metadata i anrop för analysändamål.</span><span class="sxs-lookup"><span data-stu-id="70d80-119">Logs call metadata for analytics purposes.</span></span>
* <span data-ttu-id="70d80-120">**Utgivarportalen** är det administrativa gränssnittet där du konfigurerar ditt API-program.</span><span class="sxs-lookup"><span data-stu-id="70d80-120">The **publisher portal** is the administrative interface where you set up your API program.</span></span> <span data-ttu-id="70d80-121">Använd portalen om du vill:</span><span class="sxs-lookup"><span data-stu-id="70d80-121">Use it to:</span></span>
  
  * <span data-ttu-id="70d80-122">Definiera eller importera API-schemat.</span><span class="sxs-lookup"><span data-stu-id="70d80-122">Define or import API schema.</span></span>
  * <span data-ttu-id="70d80-123">Paketera API:er till produkter.</span><span class="sxs-lookup"><span data-stu-id="70d80-123">Package APIs into products.</span></span>
  * <span data-ttu-id="70d80-124">Konfigurera principer som kvoter eller transformationer i API:erna.</span><span class="sxs-lookup"><span data-stu-id="70d80-124">Set up policies like quotas or transformations on the APIs.</span></span>
  * <span data-ttu-id="70d80-125">Få insikter från analyser.</span><span class="sxs-lookup"><span data-stu-id="70d80-125">Get insights from analytics.</span></span>
  * <span data-ttu-id="70d80-126">Hantera användare.</span><span class="sxs-lookup"><span data-stu-id="70d80-126">Manage users.</span></span>
* <span data-ttu-id="70d80-127">**utvecklarportalen** är en fundamental webbportal för utvecklare, där de kan:</span><span class="sxs-lookup"><span data-stu-id="70d80-127">The **developer portal** serves as the main web presence for developers, where they can:</span></span>
  
  * <span data-ttu-id="70d80-128">Få tillgång till API-dokumentation.</span><span class="sxs-lookup"><span data-stu-id="70d80-128">Read API documentation.</span></span>
  * <span data-ttu-id="70d80-129">Testa ett API via den interaktiva konsolen.</span><span class="sxs-lookup"><span data-stu-id="70d80-129">Try out an API via the interactive console.</span></span>
  * <span data-ttu-id="70d80-130">Skapa ett konto och börja prenumerera på API-nycklar.</span><span class="sxs-lookup"><span data-stu-id="70d80-130">Create an account and subscribe to get API keys.</span></span>
  * <span data-ttu-id="70d80-131">Komma åt analyser om deras egen användning.</span><span class="sxs-lookup"><span data-stu-id="70d80-131">Access analytics on their own usage.</span></span>

## <span data-ttu-id="70d80-132"><a name="create-service-instance"> </a>Skapa en API Management-instans</span><span class="sxs-lookup"><span data-stu-id="70d80-132"><a name="create-service-instance"> </a>Create an API Management instance</span></span>
> [!NOTE]
> <span data-ttu-id="70d80-133">Du behöver ett Azure-konto för att slutföra den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="70d80-133">To complete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="70d80-134">Om du inte har något konto kan du skapa ett kostnadsfritt konto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="70d80-134">If you don't have an account, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="70d80-135">Mer information finns i [kostnadsfri utvärderingsversion av Azure][Azure Free Trial].</span><span class="sxs-lookup"><span data-stu-id="70d80-135">For details, see [Azure Free Trial][Azure Free Trial].</span></span>
> 
> 

<span data-ttu-id="70d80-136">Det första steget när du arbetar med API Management är att skapa en tjänstinstans.</span><span class="sxs-lookup"><span data-stu-id="70d80-136">The first step in working with API Management is to create a service instance.</span></span> <span data-ttu-id="70d80-137">Logga in på [Azure Portal][Azure Portal] och klicka på **Nytt**, **Web + Mobile**, **API Management**.</span><span class="sxs-lookup"><span data-stu-id="70d80-137">Sign in to the [Azure Portal][Azure Portal] and click **New**, **Web + Mobile**, **API Management**.</span></span>

![Ny API Management-instans][api-management-create-instance-menu]

<span data-ttu-id="70d80-139">För **Namn** anger du ett unikt underdomännamn som ska användas för tjänst-URL:en.</span><span class="sxs-lookup"><span data-stu-id="70d80-139">For **Name**, specify a unique sub-domain name to use for the service URL.</span></span>

<span data-ttu-id="70d80-140">Välj önskad **prenumeration**, **resursgrupp** och **plats** för din tjänstinstans.</span><span class="sxs-lookup"><span data-stu-id="70d80-140">Choose the desired **Subscription**, **Resource group** and **Location** for your service instance.</span></span>

<span data-ttu-id="70d80-141">Ange **Contoso Ltd.**</span><span class="sxs-lookup"><span data-stu-id="70d80-141">Enter **Contoso Ltd.**</span></span> <span data-ttu-id="70d80-142">som **organisationsnamn** och ange din e-postadress i fältet **Administratörens e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="70d80-142">for the **Organization Name**, and enter your email address in the **Administrator E-Mail** field.</span></span>

> [!NOTE]
> <span data-ttu-id="70d80-143">Den här e-postadressen används för meddelanden från API Management-systemet.</span><span class="sxs-lookup"><span data-stu-id="70d80-143">This email address is used for notifications from the API Management system.</span></span> <span data-ttu-id="70d80-144">Mer information finns i [Konfigurera meddelanden och e-postmallar i Azure API Management][How to configure notifications and email templates in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="70d80-144">For more information, see [How to configure notifications and email templates in Azure API Management][How to configure notifications and email templates in Azure API Management].</span></span>
> 
> 

![Ny API Management-tjänst][api-management-create-instance-step1]

<span data-ttu-id="70d80-146">API Management-tjänstinstanser är tillgängliga på tre nivåer: Developer, Standard och Premium.</span><span class="sxs-lookup"><span data-stu-id="70d80-146">API Management service instances are available in three tiers: Developer, Standard, and Premium.</span></span>

> [!NOTE]
> <span data-ttu-id="70d80-147">Developer-nivån är avsedd för utveckling, testning och pilotprojekt av API-program där hög tillgänglighet inte är något problem.</span><span class="sxs-lookup"><span data-stu-id="70d80-147">The Developer Tier is for development, testing, and pilot API programs where high availability is not a concern.</span></span> <span data-ttu-id="70d80-148">På Standard- och Premium-nivåerna kan du skala antalet reserverade enheter och hantera mer trafik.</span><span class="sxs-lookup"><span data-stu-id="70d80-148">In the Standard and Premium tiers, you can scale your reserved unit count to handle more traffic.</span></span> <span data-ttu-id="70d80-149">API Management-tjänsten har mest processorkraft och prestanda på Standard- och Premium-nivåerna.</span><span class="sxs-lookup"><span data-stu-id="70d80-149">The Standard and Premium tiers provide your API Management service with the most processing power and performance.</span></span> <span data-ttu-id="70d80-150">Du kan genomföra den här självstudiekursen med valfri nivå.</span><span class="sxs-lookup"><span data-stu-id="70d80-150">You can complete this tutorial by using any tier.</span></span> <span data-ttu-id="70d80-151">Mer information om API Management-nivåer finns i avsnittet om [API Management-priser][API Management pricing].</span><span class="sxs-lookup"><span data-stu-id="70d80-151">For more information about API Management tiers, see [API Management pricing][API Management pricing].</span></span>
> 
> 

<span data-ttu-id="70d80-152">Klicka på **skapa** för att börja etablera din serviceinstance.</span><span class="sxs-lookup"><span data-stu-id="70d80-152">Click **Create** to start provisioning your service instance.</span></span>

![Ny API Management-tjänst][api-management-instance-created]

<span data-ttu-id="70d80-154">När tjänstinstansen har skapats är nästa steg att skapa eller importera ett API.</span><span class="sxs-lookup"><span data-stu-id="70d80-154">Once the service instance is created, the next step is to create or import an API.</span></span>

## <span data-ttu-id="70d80-155"><a name="create-api"> </a>Importera ett API</span><span class="sxs-lookup"><span data-stu-id="70d80-155"><a name="create-api"> </a>Import an API</span></span>
<span data-ttu-id="70d80-156">Ett API består av en uppsättning åtgärder som kan anropas från ett klientprogram.</span><span class="sxs-lookup"><span data-stu-id="70d80-156">An API consists of a set of operations that can be invoked from a client application.</span></span> <span data-ttu-id="70d80-157">API-åtgärder körs via en proxy till befintliga webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="70d80-157">API operations are proxied to existing web services.</span></span>

<span data-ttu-id="70d80-158">API:er kan skapas (och åtgärder kan läggas till) manuellt eller importeras.</span><span class="sxs-lookup"><span data-stu-id="70d80-158">APIs can be created (and operations can be added) manually, or they can be imported.</span></span> <span data-ttu-id="70d80-159">I den här självstudien ska vi importera API:et för ett exempel på en webbaserad kalkylatortjänst som tillhandahålls av Microsoft och som finns i Azure.</span><span class="sxs-lookup"><span data-stu-id="70d80-159">In this tutorial, we will import the API for a sample calculator web service provided by Microsoft and hosted on Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="70d80-160">Anvisningar för hur du skapar ett API och lägger till åtgärder manuellt finns i [Skapa API:er](api-management-howto-create-apis.md) och [Lägga till åtgärder till ett API](api-management-howto-add-operations.md).</span><span class="sxs-lookup"><span data-stu-id="70d80-160">For guidance on creating an API and manually adding operations, see [How to create APIs](api-management-howto-create-apis.md) and [How to add operations to an API](api-management-howto-add-operations.md).</span></span>
> 
> 

<span data-ttu-id="70d80-161">API: er konfigureras från utgivarportalen.</span><span class="sxs-lookup"><span data-stu-id="70d80-161">APIs are configured from the publisher portal.</span></span> <span data-ttu-id="70d80-162">För att nå den, klickar du på **utgivarportalen** i serviceverktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="70d80-162">To reach it, click **Publisher portal** from the service toolbar.</span></span>

![Utgivarportalen][api-management-management-console]

<span data-ttu-id="70d80-164">Du importerar kalkylator-API:et genom att klicka på **API:er** på **API Management**-menyn till vänster och sedan på **Importera API**.</span><span class="sxs-lookup"><span data-stu-id="70d80-164">To import the calculator API, click **APIs** from the **API Management** menu on the left, and then click **Import API**.</span></span>

![Knappen Importera API][api-management-import-api]

<span data-ttu-id="70d80-166">Konfigurera kalkylator-API:et genom att följa stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="70d80-166">Perform the following steps to configure the calculator API:</span></span>

1. <span data-ttu-id="70d80-167">Klicka på **Från URL**, ange **http://calcapi.cloudapp.net/calcapi.json** i textrutan **URL till specifikationsdokument** och klicka på alternativknappen för **Swagger**.</span><span class="sxs-lookup"><span data-stu-id="70d80-167">Click **From URL**, enter **http://calcapi.cloudapp.net/calcapi.json** into the **Specification document URL** text box, and click the **Swagger** radio button.</span></span>
2. <span data-ttu-id="70d80-168">Skriv **calc** i textrutan **URL-suffix för webb-API**.</span><span class="sxs-lookup"><span data-stu-id="70d80-168">Type **calc** into the **Web API URL suffix** text box.</span></span>
3. <span data-ttu-id="70d80-169">Klicka i rutan **Produkter (valfritt)** och välj **Starter**.</span><span class="sxs-lookup"><span data-stu-id="70d80-169">Click in the **Products (optional)** box and choose **Starter**.</span></span>
4. <span data-ttu-id="70d80-170">Klicka på **Spara** för att importera API:et.</span><span class="sxs-lookup"><span data-stu-id="70d80-170">Click **Save** to import the API.</span></span>

![Lägga till ett nytt API][api-management-import-new-api]

> [!NOTE]
> <span data-ttu-id="70d80-172">**API Management** stöder för närvarande både 1.2- och 2.0-versionen av Swagger-dokument för importändamål.</span><span class="sxs-lookup"><span data-stu-id="70d80-172">**API Management** currently supports both 1.2 and 2.0 version of Swagger document for import.</span></span> <span data-ttu-id="70d80-173">Egenskaperna `host`, `basePath` och `schemes` är valfria enligt [Swagger 2.0-specifikationen](http://swagger.io/specification), men ditt Swagger 2.0-dokument **MÅSTE** innehålla dessa egenskaper för att importeras.</span><span class="sxs-lookup"><span data-stu-id="70d80-173">Make sure that, even though [Swagger 2.0 specification](http://swagger.io/specification) declares that `host`, `basePath`, and `schemes` properties are optional, your Swagger 2.0 document **MUST** contain those properties; otherwise it won't get imported.</span></span> 
> 
> 

<span data-ttu-id="70d80-174">När du har importerat API:et visas sammanfattningssidan för API:et på utgivarportalen.</span><span class="sxs-lookup"><span data-stu-id="70d80-174">Once the API is imported, the summary page for the API is displayed in the publisher portal.</span></span>

![API-sammanfattning][api-management-imported-api-summary]

<span data-ttu-id="70d80-176">API-avsnittet innehåller flera flikar.</span><span class="sxs-lookup"><span data-stu-id="70d80-176">The API section has several tabs.</span></span> <span data-ttu-id="70d80-177">Fliken **Sammanfattning** innehåller grundläggande mätvärden och information om API:et.</span><span class="sxs-lookup"><span data-stu-id="70d80-177">The **Summary** tab displays basic metrics and information about the API.</span></span> <span data-ttu-id="70d80-178">Fliken [Inställningar](api-management-howto-create-apis.md#configure-api-settings) används för att visa och redigera konfigurationen för ett API.</span><span class="sxs-lookup"><span data-stu-id="70d80-178">The [Settings](api-management-howto-create-apis.md#configure-api-settings) tab is used to view and edit the configuration for an API.</span></span> <span data-ttu-id="70d80-179">Fliken [Åtgärder](api-management-howto-add-operations.md) används för att hantera API-åtgärderna.</span><span class="sxs-lookup"><span data-stu-id="70d80-179">The [Operations](api-management-howto-add-operations.md) tab is used to manage the API's operations.</span></span> <span data-ttu-id="70d80-180">Fliken **Säkerhet** kan användas för att konfigurera gateway-autentisering för backend-servern med grundläggande autentisering eller [ömsesidig certifikatautentisering](api-management-howto-mutual-certificates.md), och för att konfigurera [användarauktorisering med hjälp av OAuth 2.0](api-management-howto-oauth2.md).</span><span class="sxs-lookup"><span data-stu-id="70d80-180">The **Security** tab can be used to configure gateway authentication for the backend server by using Basic authentication or [mutual certificate authentication](api-management-howto-mutual-certificates.md), and to configure [user authorization by using OAuth 2.0](api-management-howto-oauth2.md).</span></span>  <span data-ttu-id="70d80-181">Fliken **Problem** används för att visa problem som har rapporterats av utvecklare som använder dina API:er.</span><span class="sxs-lookup"><span data-stu-id="70d80-181">The **Issues** tab is used to view issues reported by the developers who are using your APIs.</span></span> <span data-ttu-id="70d80-182">Fliken **Produkter** används för att konfigurera produkterna som innehåller API:et.</span><span class="sxs-lookup"><span data-stu-id="70d80-182">The **Products** tab is used to configure the products that contain this API.</span></span>

<span data-ttu-id="70d80-183">Som standard medföljer två exempelprodukter varje API Management-instans:</span><span class="sxs-lookup"><span data-stu-id="70d80-183">By default, each API Management instance comes with two sample products:</span></span>

* <span data-ttu-id="70d80-184">**Starter**</span><span class="sxs-lookup"><span data-stu-id="70d80-184">**Starter**</span></span>
* <span data-ttu-id="70d80-185">**Obegränsat**</span><span class="sxs-lookup"><span data-stu-id="70d80-185">**Unlimited**</span></span>

<span data-ttu-id="70d80-186">I den här självstudien lades Basic Calculator-API:et till i Starter-produkten när API:et importerades.</span><span class="sxs-lookup"><span data-stu-id="70d80-186">In this tutorial, the Basic Calculator API was added to the Starter product when the API was imported.</span></span>

<span data-ttu-id="70d80-187">Innan utvecklare kan göra anrop till ett API måste de prenumerera på en produkt som ger dem åtkomst till API:et.</span><span class="sxs-lookup"><span data-stu-id="70d80-187">In order to make calls to an API, developers must first subscribe to a product that gives them access to it.</span></span> <span data-ttu-id="70d80-188">Utvecklare kan prenumerera på produkter via utvecklarportalen, eller så kan en administratör registrera utvecklare för produktprenumerationer från utgivarportalen.</span><span class="sxs-lookup"><span data-stu-id="70d80-188">Developers can subscribe to products in the developer portal, or administrators can subscribe developers to products in the publisher portal.</span></span> <span data-ttu-id="70d80-189">Du är administratör eftersom du skapade API Management-instansen i föregående steg i den här självstudiekursen, och som administratör prenumererar du redan på alla produkter som standard.</span><span class="sxs-lookup"><span data-stu-id="70d80-189">You are an administrator since you created the API Management instance in the previous steps in the tutorial, so you are already subscribed to every product by default.</span></span>

## <span data-ttu-id="70d80-190"><a name="call-operation"> </a>Anropa en åtgärd från utvecklarportalen</span><span class="sxs-lookup"><span data-stu-id="70d80-190"><a name="call-operation"> </a>Call an operation from the developer portal</span></span>
<span data-ttu-id="70d80-191">Du kan anropa åtgärder direkt från utvecklarportalen, vilket är ett enkelt sätt att visa och testa åtgärderna i ett API.</span><span class="sxs-lookup"><span data-stu-id="70d80-191">Operations can be called directly from the developer portal, which provides a convenient way to view and test the operations of an API.</span></span> <span data-ttu-id="70d80-192">I det här steget i självstudiekursen ska du anropa åtgärden **Lägg till två heltal** i Basic Calculator-API:et.</span><span class="sxs-lookup"><span data-stu-id="70d80-192">In this tutorial step, you will call the Basic Calculator API's **Add two integers** operation.</span></span> <span data-ttu-id="70d80-193">Klicka på **utvecklarportalen** på menyn längst upp till höger på utgivarportalen.</span><span class="sxs-lookup"><span data-stu-id="70d80-193">Click **Developer portal** from the menu at the top right of the publisher portal.</span></span>

![Utvecklarportalen][api-management-developer-portal-menu]

<span data-ttu-id="70d80-195">Klicka på **API:er** på den översta menyn och klicka sedan på **Basic Calculator** för att visa de tillgängliga åtgärderna.</span><span class="sxs-lookup"><span data-stu-id="70d80-195">Click **APIs** from the top menu, and then click **Basic Calculator** to see the available operations.</span></span>

![Utvecklarportalen][api-management-developer-portal-calc-api]

<span data-ttu-id="70d80-197">Observera exempelbeskrivningarna och exempelparametrarna som importerades tillsammans med API:et och åtgärderna. Beskrivningarna fungerar som en referens för utvecklare som ska använda åtgärden.</span><span class="sxs-lookup"><span data-stu-id="70d80-197">Note the sample descriptions and parameters that were imported along with the API and operations, providing documentation for the developers that will use this operation.</span></span> <span data-ttu-id="70d80-198">Dessa beskrivningar kan även läggas till om åtgärder läggs till manuellt.</span><span class="sxs-lookup"><span data-stu-id="70d80-198">These descriptions can also be added when operations are added manually.</span></span>

<span data-ttu-id="70d80-199">Anropa åtgärden **Lägg till två heltal** genom att klicka på **Prova**.</span><span class="sxs-lookup"><span data-stu-id="70d80-199">To call the **Add two integers** operation, click **Try it**.</span></span>

![Prova][api-management-developer-portal-calc-api-console]

<span data-ttu-id="70d80-201">Ange egna värden för parametrarna eller behåll standardvärdena och klicka sedan på **Skicka**.</span><span class="sxs-lookup"><span data-stu-id="70d80-201">You can enter some values for the parameters or keep the defaults, and then click **Send**.</span></span>

![HTTP Get][api-management-invoke-get]

<span data-ttu-id="70d80-203">När en åtgärd har anropats visas **svarsstatus**, **svarshuvuden** och eventuellt **svarsinnehåll** på utvecklarportalen.</span><span class="sxs-lookup"><span data-stu-id="70d80-203">After an operation is invoked, the developer portal displays the **Response status**, the **Response headers**, and any **Response content**.</span></span>

![Svar][api-management-invoke-get-response]

## <span data-ttu-id="70d80-205"><a name="view-analytics"> </a>Visa analys</span><span class="sxs-lookup"><span data-stu-id="70d80-205"><a name="view-analytics"> </a>View analytics</span></span>
<span data-ttu-id="70d80-206">Om du vill visa analysinformation för Basic Calculator går du tillbaka till utgivarportalen genom att välja **Hantera** på menyn längst upp till höger på utvecklarportalen.</span><span class="sxs-lookup"><span data-stu-id="70d80-206">To view analytics for Basic Calculator, switch back to the publisher portal by selecting **Manage** from the menu at the top right of the developer portal.</span></span>

![Hantera][api-management-manage-menu]

<span data-ttu-id="70d80-208">Standardvyn för utgivarportalen är **Instrumentpanel**, som innehåller en översikt över API Management-instansen.</span><span class="sxs-lookup"><span data-stu-id="70d80-208">The default view for the publisher portal is the **Dashboard**, which provides an overview of your API Management instance.</span></span>

![Instrumentpanel][api-management-dashboard]

<span data-ttu-id="70d80-210">Hovra med musen över diagrammet för **Basic Calculator** så ser du de specifika mätvärdena relaterade till användningen av API:et under en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="70d80-210">Hover the mouse over the chart for **Basic Calculator** to see the specific metrics for the usage of the API for a given time period.</span></span>

> [!NOTE]
> <span data-ttu-id="70d80-211">Om du inte ser några rader i diagrammet växlar du tillbaka till utvecklarportalen gör några anrop till API:et, väntar en stund och går sedan tillbaka till instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="70d80-211">If you don't see any lines on your chart, switch back to the developer portal and make some calls into the API, wait a few moments, and then come back to the dashboard.</span></span>
> 
> 

<span data-ttu-id="70d80-212">Klicka på **Visa detaljer** om du vill visa sammanfattningssidan för API:et, inklusive en mer omfattande version av mätvärdena som visas.</span><span class="sxs-lookup"><span data-stu-id="70d80-212">Click **View Details** to view the summary page for the API, including a larger version of the displayed metrics.</span></span>

![Analys][api-management-mouse-over]

![Sammanfattning][api-management-api-summary-metrics]

<span data-ttu-id="70d80-215">Om du vill visa detaljerade mätvärden och rapporter klickar du på **Analys** på **API Management**-menyn till vänster.</span><span class="sxs-lookup"><span data-stu-id="70d80-215">For detailed metrics and reports, click **Analytics** from the **API Management** menu on the left.</span></span>

![Översikt][api-management-analytics-overview]

<span data-ttu-id="70d80-217">Avsnittet **Analys** innehåller följande fyra flikar:</span><span class="sxs-lookup"><span data-stu-id="70d80-217">The **Analytics** section has the following four tabs:</span></span>

* <span data-ttu-id="70d80-218">**Översikt** innehåller mätvärden om användning och hälsotillstånd, samt de vanligaste utvecklarna, produkterna, API:erna och åtgärderna.</span><span class="sxs-lookup"><span data-stu-id="70d80-218">**At a glance** provides overall usage and health metrics, as well as the top developers, top products, top APIs, and top operations.</span></span>
* <span data-ttu-id="70d80-219">**Användning** innehåller detaljerad information om API-anrop och bandbredd, inklusive en geografisk representation.</span><span class="sxs-lookup"><span data-stu-id="70d80-219">**Usage** provides an in-depth look at API calls and bandwidth, including a geographical representation.</span></span>
* <span data-ttu-id="70d80-220">**Hälsa** fokuserar på statuskoder, lyckade cachelagringsåtgärder, svarstider och API- och tjänstsvarstider.</span><span class="sxs-lookup"><span data-stu-id="70d80-220">**Health** focuses on status codes, cache success rates, response times, and API and service response times.</span></span>
* <span data-ttu-id="70d80-221">**Aktivitet** innehåller rapporter som visar information om den specifika aktiviteten efter utvecklare, produkt, API och åtgärd.</span><span class="sxs-lookup"><span data-stu-id="70d80-221">**Activity** provides reports that drill down on the specific activity by developer, product, API, and operation.</span></span>

## <span data-ttu-id="70d80-222"><a name="next-steps"> </a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="70d80-222"><a name="next-steps"> </a>Next steps</span></span>
* <span data-ttu-id="70d80-223">Läs mer om hur du [skyddar ditt API med hastighetsbegränsningar](api-management-howto-product-with-rules.md).</span><span class="sxs-lookup"><span data-stu-id="70d80-223">Learn how to [Protect your API with rate limits](api-management-howto-product-with-rules.md).</span></span>

[Azure Free Trial]: http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=api_management_hero_a

[Create an API Management instance]: #create-service-instance
[Create an API]: #create-api
[Add an operation]: #add-operation
[Add the new API to a product]: #add-api-to-product
[Subscribe to the product that contains the API]: #subscribe
[Call an operation from the Developer Portal]: #call-operation
[View analytics]: #view-analytics
[Next steps]: #next-steps


[How to manage developer accounts in Azure API Management]: api-management-howto-create-or-invite-developers.md
[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[How to configure notifications and email templates in Azure API Management]: api-management-howto-configure-notifications.md
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
