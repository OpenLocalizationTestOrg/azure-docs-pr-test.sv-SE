---
title: "aaaGuide toocreating en datatjänst för hello Marketplace | Microsoft Docs"
description: "Detaljerade anvisningar hur toocreate, certifiera och distribuera en Data-tjänst för att köpa på hello Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 96194198-6991-43b4-918e-ee337e250339
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 0220d357ae0ec89e7d4f6399605850e57c646f73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-service-publishing-guide-for-hello-azure-marketplace"></a><span data-ttu-id="f8bab-103">Data Service-publiceringsguide för hello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="f8bab-103">Data Service Publishing Guide for hello Azure Marketplace</span></span>
> [!IMPORTANT]
> <span data-ttu-id="f8bab-104">**Just nu vi inte längre onboarding några nya Data Service-utgivare. Nya dataservices kommer inte godkännas för lista.**</span><span class="sxs-lookup"><span data-stu-id="f8bab-104">**At this time we are no longer onboarding any new Data Service publishers. New dataservices will not get approved for listing.**</span></span> <span data-ttu-id="f8bab-105">Om du har ett SaaS-affärsprogram som toopublish på AppSource hittar du mer information [här](https://appsource.microsoft.com/partners).</span><span class="sxs-lookup"><span data-stu-id="f8bab-105">If you have a SaaS business application you would like toopublish on AppSource you can find more information [here](https://appsource.microsoft.com/partners).</span></span> <span data-ttu-id="f8bab-106">Om du har en IaaS-program eller utvecklare tjänst du skulle t.ex toopublish på Azure Marketplace hittar du mer information [här](https://azure.microsoft.com/marketplace/programs/certified/).</span><span class="sxs-lookup"><span data-stu-id="f8bab-106">If you have an IaaS applications or developer service you would like toopublish on Azure Marketplace you can find more information [here](https://azure.microsoft.com/marketplace/programs/certified/).</span></span>
> 
> 

<span data-ttu-id="f8bab-107">När du har slutfört steg 1, för hello [konto skapas och registrering](marketplace-publishing-accounts-creation-registration.md), vi Interaktiv du via hello [allmänna icke-tekniska](marketplace-publishing-pre-requisites.md) och [tekniska krav](marketplace-publishing-data-service-creation-prerequisites.md) av en datatjänst erbjuder på Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="f8bab-107">After completing hello step 1, [Account Creation and Registration](marketplace-publishing-accounts-creation-registration.md), we guided you through hello [General Non-Technical](marketplace-publishing-pre-requisites.md) and [Technical Requirements](marketplace-publishing-data-service-creation-prerequisites.md) of a Data Service offer on Azure Marketplace.</span></span> <span data-ttu-id="f8bab-108">Nu går du igenom hello steg för att skapa ett Data tjänsterbjudande på hello [Publiceringsportal] [ link-pubportal] för hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="f8bab-108">Now we will walk you through hello steps for creating a Data Service offer on hello [Publishing Portal][link-pubportal] for hello Azure Marketplace.</span></span>

## <a name="1----login-toohello-publishing-portal"></a><span data-ttu-id="f8bab-109">1.    Inloggningen toohello Publishing Portal.</span><span class="sxs-lookup"><span data-stu-id="f8bab-109">1.    Login toohello Publishing Portal.</span></span>
<span data-ttu-id="f8bab-110">Gå för[https://publish.windowsazure.com](https://publish.windowsazure.com.)</span><span class="sxs-lookup"><span data-stu-id="f8bab-110">Go too[https://publish.windowsazure.com](https://publish.windowsazure.com.)</span></span>

<span data-ttu-id="f8bab-111">**För första gången inloggningen tooPublishing Portal, använder du hello samma konto som företagets säljare profilen har registrerats i Developer Center.**</span><span class="sxs-lookup"><span data-stu-id="f8bab-111">**For first time login tooPublishing Portal, use hello same account with which your company’s Seller Profile was registered in Developer Center.**</span></span>  <span data-ttu-id="f8bab-112">(Senare du kan lägga till anställda på företaget som medadministratör i hello Publiceringsportal).</span><span class="sxs-lookup"><span data-stu-id="f8bab-112">(Later you can add any employee of your company as a co-admin in hello Publishing Portal).</span></span>

<span data-ttu-id="f8bab-113">Klicka på hello **publicera en datatjänster** panelen om det här är hello första inloggningen till hello publishing portal.</span><span class="sxs-lookup"><span data-stu-id="f8bab-113">Click on hello **Publish a Data Services** tile if this is hello first login into hello publishing portal.</span></span>

## <a name="2----choose-data-services-in-hello-navigation-menu-on-hello-left-side"></a><span data-ttu-id="f8bab-114">2.    Välj **datatjänster** i hello navigationsmenyn hello vänster sida.</span><span class="sxs-lookup"><span data-stu-id="f8bab-114">2.    Choose **Data Services** in hello navigation menu on hello left side.</span></span>
  ![Rita](media/marketplace-publishing-data-service-creation/pubportal-main-nav.png)

## <a name="3----create-a-new-data-service"></a><span data-ttu-id="f8bab-116">3.    Skapa en ny datatjänst</span><span class="sxs-lookup"><span data-stu-id="f8bab-116">3.    Create a New Data Service</span></span>
<span data-ttu-id="f8bab-117">Fyll i hello rubrik för din nya erbjuder för Data-tjänsten och klicka på ”+” på hello rätt.</span><span class="sxs-lookup"><span data-stu-id="f8bab-117">Fill in hello title for your new Data Service Offer and click on “+” on hello right.</span></span>

  ![Rita](media/marketplace-publishing-data-service-creation/step-3.png)

## <a name="4----review-hello-sub-menu-under-hello-newly-created-data-service-in-hello-navigation-menu"></a><span data-ttu-id="f8bab-119">4.    Granska hello undermeny under hello nyskapad datatjänst i hello navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="f8bab-119">4.    Review hello sub-menu under hello newly created Data Service in hello navigation menu.</span></span>
<span data-ttu-id="f8bab-120">Klicka på hello **genomgången** och granska alla nödvändiga steg behövs toopublish korrekt hello datatjänst på hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="f8bab-120">Click on hello **Walkthrough** tab and review all necessary steps needed toopublish properly hello Data Service on hello Azure Marketplace.</span></span>

> [!TIP]
> <span data-ttu-id="f8bab-121">Du kan alltid klickar du på hello länkar i hello ”genomgången” sidan eller Använd flikarna på hello datatjänst erbjudande undermeny hello vänster.</span><span class="sxs-lookup"><span data-stu-id="f8bab-121">You always can click on hello links in hello “Walkthrough” page or use tabs on hello Data Service offer’s sub-menu on hello left side.</span></span>
> 
> 

## <a name="5----create-a-new-plan"></a><span data-ttu-id="f8bab-122">5.    Skapa en ny Plan.</span><span class="sxs-lookup"><span data-stu-id="f8bab-122">5.    Create a new Plan.</span></span>
### <a name="offers-plans-transactions"></a><span data-ttu-id="f8bab-123">Erbjudanden planer, transaktioner.</span><span class="sxs-lookup"><span data-stu-id="f8bab-123">Offers, Plans, transactions.</span></span>
<span data-ttu-id="f8bab-124">Varje erbjudande kan ha flera planer, men måste ha minst en (1) Plan.</span><span class="sxs-lookup"><span data-stu-id="f8bab-124">Each Offer can have multiple Plans, but must have at least one (1) Plan.</span></span> <span data-ttu-id="f8bab-125">När slutanvändarna prenumererar tooyour erbjudande prenumerera de för en hello erbjudande Plan.</span><span class="sxs-lookup"><span data-stu-id="f8bab-125">When end-users subscribe tooyour offer they subscribe for one of hello offer’s Plan.</span></span> <span data-ttu-id="f8bab-126">Varje plan definierar hur slutanvändarna kommer att kunna toouse din tjänst.</span><span class="sxs-lookup"><span data-stu-id="f8bab-126">Each plan defines how end-users will be able toouse your service.</span></span>

<span data-ttu-id="f8bab-127">För närvarande månatlig prenumeration transaktion baserat modellen stöd för Azure Marketplace för Data Services, d.v.s. slutanvändare betalar månadsavgift enligt toohello priset för hello specifik plan de prenumererar tooand kommer att kunna tooconsume varje månad antal transaktionen som definieras av hello plan.</span><span class="sxs-lookup"><span data-stu-id="f8bab-127">Currently Azure Marketplace support only Monthly Subscription Transaction Based model for Data Services, i.e. end-users will pay monthly fee according toohello price of hello specific plan they subscribed tooand will be able tooconsume each month number of transaction defined by hello plan.</span></span>

<span data-ttu-id="f8bab-128">Varje transaktion som vanligtvis definieras som antalet poster som returnerar Data Service baserat på hello fråga skickas toohello Service.</span><span class="sxs-lookup"><span data-stu-id="f8bab-128">Each Transaction usually defined as number of records your Data Service will return based on hello query sent toohello Service.</span></span> <span data-ttu-id="f8bab-129">hello standardvärdet är 100.</span><span class="sxs-lookup"><span data-stu-id="f8bab-129">hello default is 100.</span></span> <span data-ttu-id="f8bab-130">Antal transaktioner returnerade tooeach frågan kommer att antalet poster dividerat med 100 och avrundat toohello närmaste heltal.</span><span class="sxs-lookup"><span data-stu-id="f8bab-130">Number of transactions returned tooeach query will be number of records divided by 100 and rounded up toohello closest integer.</span></span>

<span data-ttu-id="f8bab-131">Det är Azure Marketplace-tjänsten layer ansvar toomonitor (mätaren) antal transaktioner som används av varje fråga.</span><span class="sxs-lookup"><span data-stu-id="f8bab-131">It’s Azure Marketplace Service layer responsibility toomonitor (meter) number of transactions consumed by each query.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f8bab-132">Slutanvändare som nått gränsen för hello transaktion under hello månad kommer att blockeras från fortsätter toouse hello service till slutet av deras månatlig prenumeration cykel.</span><span class="sxs-lookup"><span data-stu-id="f8bab-132">End-Users which reached hello transaction limit during hello month will be blocked from continuing toouse hello service until end of their monthly subscription cycle.</span></span>
> 
> <span data-ttu-id="f8bab-133">hello plan eller någon av hello planer kan (men måste inte) innehåller obegränsat antal transaktioner.</span><span class="sxs-lookup"><span data-stu-id="f8bab-133">hello plan or one of hello plans can (but not must) include unlimited number of transactions.</span></span>
> 
> 

### <a name="create-a-plan"></a><span data-ttu-id="f8bab-134">Skapa en plan.</span><span class="sxs-lookup"><span data-stu-id="f8bab-134">Create a plan.</span></span>
1. <span data-ttu-id="f8bab-135">Klicka på **”+”** nästa toohello ”Lägg till en ny plan”.</span><span class="sxs-lookup"><span data-stu-id="f8bab-135">Click on **“+”** next toohello “Add a new plan”.</span></span>
2. <span data-ttu-id="f8bab-136">Välj ett av alternativen hello: **obegränsad** eller **begränsad** användning för den här planen.</span><span class="sxs-lookup"><span data-stu-id="f8bab-136">Choose one of hello options: **Unlimited** or **Limited** usage for this plan.</span></span>  <span data-ttu-id="f8bab-137">Om begränsad ange hello antalet transaktion hello planen kan tooconsume i en månad.</span><span class="sxs-lookup"><span data-stu-id="f8bab-137">If Limited then provide hello number of transaction hello plan will allow tooconsume in a month.</span></span>
   
    ![Rita](media/marketplace-publishing-data-service-creation/step-5.1.png)  
   
    <span data-ttu-id="f8bab-139">Publicera Portal kommer också föreslå ”identifieraren för Cacheuppdateringsplanen”, som kommer att använda toocommunicate toohello slutanvändare hello namnet på hello plan i hello UI och används också av hello marknadsplatsen Service tooidentify hello Plan.</span><span class="sxs-lookup"><span data-stu-id="f8bab-139">Publishing Portal will also suggest “Plan Identifier”, which will be used toocommunicate toohello end-users hello name of hello plan in hello UI and also used by hello Market Place Service tooidentify hello Plan.</span></span> <span data-ttu-id="f8bab-140">Du kan ändra hello ”planera identifierare” om du vill.</span><span class="sxs-lookup"><span data-stu-id="f8bab-140">You can change hello “Plan Identifier” if you want.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="f8bab-141">Hej ”planera identifierare” måste vara unika inom hello omfånget för varje erbjudande.</span><span class="sxs-lookup"><span data-stu-id="f8bab-141">hello “Plan Identifier” must be unique within hello scope of each offer.</span></span> <span data-ttu-id="f8bab-142">Så många andra identifierare som används i hello Publishing Portal planera identifierare låses efter hello första publishing tooproduction och du inte kommer att kunna toochange den här identifieraren.</span><span class="sxs-lookup"><span data-stu-id="f8bab-142">As many other Identifiers used in hello Publishing Portal Plan identifier will be locked after hello first publishing tooproduction and you will not be able toochange this identifier.</span></span>
   > 
   > 
3. <span data-ttu-id="f8bab-143">Klicka på tooaccept valet.</span><span class="sxs-lookup"><span data-stu-id="f8bab-143">Click tooaccept your choice.</span></span>
4. <span data-ttu-id="f8bab-144">Du kommer att få ytterligare några frågor om nyskapade planen.</span><span class="sxs-lookup"><span data-stu-id="f8bab-144">Then you will be asked few additional questions regarding your newly created Plan.</span></span>
   
    ![Rita](media/marketplace-publishing-data-service-creation/step-5.2.png)

| <span data-ttu-id="f8bab-146">Fråga</span><span class="sxs-lookup"><span data-stu-id="f8bab-146">Question</span></span> | <span data-ttu-id="f8bab-147">Betydelse</span><span class="sxs-lookup"><span data-stu-id="f8bab-147">Significance</span></span> |
| --- | --- |
| <span data-ttu-id="f8bab-148">**Den här planen är ledig och tillgänglig över hela världen?**</span><span class="sxs-lookup"><span data-stu-id="f8bab-148">**This Plan is free and available world-wide?**</span></span> |<span data-ttu-id="f8bab-149">Du kan skapa en plan för helt kostnadsfri-för-kostnad.</span><span class="sxs-lookup"><span data-stu-id="f8bab-149">You can create a completely free-of-charge plan.</span></span> <span data-ttu-id="f8bab-150">Om dess hello bara planera för det här erbjudandet – innebär det att du publicerar ”kostnadsfritt erbjuda” i hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="f8bab-150">If it’s hello only plan for this offer – it means that you are publishing “Free Offer” in hello Marketplace.</span></span> <span data-ttu-id="f8bab-151">Om det är bara för ett (få) Plan, hello den ger dig en alternativet toooffer slutanvändare toolearn mer om din tjänst med ett relativt litet antal transaktioner per månad.</span><span class="sxs-lookup"><span data-stu-id="f8bab-151">If it’s only for one (of few) Plan, hello it gives you an option toooffer end-users toolearn more about your service with a relatively small number of transactions per month.</span></span>  <span data-ttu-id="f8bab-152">Om hello svaret är ”Ja”, och inga ytterligare frågor.</span><span class="sxs-lookup"><span data-stu-id="f8bab-152">If hello answer is "Yes," then no further questions will be asked.</span></span> |

> [!NOTE]
> <span data-ttu-id="f8bab-153">Slutanvändare kan alltid uppgradera toohello betald planer.</span><span class="sxs-lookup"><span data-stu-id="f8bab-153">End users can always upgrade toohello paid plans.</span></span>
> 
> 

| <span data-ttu-id="f8bab-154">Fråga</span><span class="sxs-lookup"><span data-stu-id="f8bab-154">Question</span></span> | <span data-ttu-id="f8bab-155">Betydelse</span><span class="sxs-lookup"><span data-stu-id="f8bab-155">Significance</span></span> |
| --- | --- |
| <span data-ttu-id="f8bab-156">**Finns kostnadsfri utvärderingsversion?**</span><span class="sxs-lookup"><span data-stu-id="f8bab-156">**Is free trial available?**</span></span> |<span data-ttu-id="f8bab-157">Du kan välja mellan ”Nej utvärderingsversion” alls eller ge en alternativ toouse din Plan för ”en månad”.</span><span class="sxs-lookup"><span data-stu-id="f8bab-157">You can choose between “No Trial” at all or give an option toouse your Plan for “One Month”.</span></span> <span data-ttu-id="f8bab-158">Utgivare som toouse det här alternativet tooprovide slutanvändare hello möjligheten toounderstand hello fördelarna med hello kostnadsfritt erbjuda i en månad.</span><span class="sxs-lookup"><span data-stu-id="f8bab-158">Publishers like toouse this option tooprovide end-users hello possibility toounderstand hello benefits of hello offer for free for one month.</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="f8bab-159">Slutanvändare ska kunna toopurchase en kostnadsfri utvärderingsversion om de har etablerat betalningsalternativ t.ex. kreditkort, enterprise-avtal.</span><span class="sxs-lookup"><span data-stu-id="f8bab-159">End-users will only be able toopurchase a free trial if they have established payment instrument e.g. credit card, enterprise agreement.</span></span>
> 
> <span data-ttu-id="f8bab-160">Efter en månad för kostnadsfri utvärderingsversion av hello kommer Azure Marketplace startas debitering kunder hello priset för hello datum för hello prenumerationen om hello kunden initierade hello prenumeration annullering.</span><span class="sxs-lookup"><span data-stu-id="f8bab-160">After one month of hello free trial, Azure Marketplace will start charging customers hello price as of hello date of hello subscription, unless hello customer initiated hello subscription cancellation.</span></span> <span data-ttu-id="f8bab-161">Inget särskilt meddelande anges toohello slutanvändare.</span><span class="sxs-lookup"><span data-stu-id="f8bab-161">No special notification will be provided toohello end-users.</span></span>
> 
> 

| <span data-ttu-id="f8bab-162">Fråga</span><span class="sxs-lookup"><span data-stu-id="f8bab-162">Question</span></span> | <span data-ttu-id="f8bab-163">Betydelse</span><span class="sxs-lookup"><span data-stu-id="f8bab-163">Significance</span></span> |
| --- | --- |
| <span data-ttu-id="f8bab-164">**Den här planen kräver en befordran kod toopurchase?**</span><span class="sxs-lookup"><span data-stu-id="f8bab-164">**This plan requires a promotion code toopurchase?**</span></span> |<span data-ttu-id="f8bab-165">Utgivare har en alternativet toolimit åtkomst tootheir serviceplaner genom att tillhandahålla en särskild kod som kallas ”A Promocode” toospecific kunder.</span><span class="sxs-lookup"><span data-stu-id="f8bab-165">Publishers have an option toolimit access tootheir Service Plans by providing a special code, called “A Promocode” toospecific customers.</span></span> <span data-ttu-id="f8bab-166">Slutanvändare som har den här Promocode kommer att kunna toosubscribe toohello Plan.</span><span class="sxs-lookup"><span data-stu-id="f8bab-166">Only end-users which will have this Promocode will be able toosubscribe toohello Plan.</span></span> <span data-ttu-id="f8bab-167">Om du väljer ”Nej”, så att du godkänner alla från hello region där hello erbjuder är tillgänglig (se [Marketplace Marketing Content guiden](marketplace-publishing-push-to-staging.md) för mer information) kommer att kunna toosubscribe toothis plan.</span><span class="sxs-lookup"><span data-stu-id="f8bab-167">If you choose “No”, then you agree that everyone from hello region where hello offer is available (See [Marketplace Marketing Content Guide](marketplace-publishing-push-to-staging.md) for more details) will be able toosubscribe toothis plan.</span></span> <span data-ttu-id="f8bab-168">Inga ytterligare frågor blir ombedd.</span><span class="sxs-lookup"><span data-stu-id="f8bab-168">No further questions will be asked.</span></span> |
| <span data-ttu-id="f8bab-169">**Dessutom dölja den här planen som inte har en giltig befordran koden?**</span><span class="sxs-lookup"><span data-stu-id="f8bab-169">**Also hide this plan from anyone who doesn’t have a valid promotion code?**</span></span> |<span data-ttu-id="f8bab-170">Om hello svaret toohello föregående fråga är ”Yes” har hello utgivare en alternativet toocompletely ta bort den här planen från som visas i hello Användargränssnittet för hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="f8bab-170">If hello answer toohello previous question is “Yes” hello Publisher has an option toocompletely remove this plan from appearing in hello UI of hello Marketplace.</span></span> <span data-ttu-id="f8bab-171">Det innebär kunder inte ser den här planen hello erbjudande information på sidan.</span><span class="sxs-lookup"><span data-stu-id="f8bab-171">It means, customers will not see this plan in hello Offer’s details page.</span></span> <span data-ttu-id="f8bab-172">Slutanvändare som tar emot en promocode toopurchase, kommer att kunna toosubscribe tooit med hjälp av den här promocode.</span><span class="sxs-lookup"><span data-stu-id="f8bab-172">End-users which will receive a promocode toopurchase it, will be able toosubscribe tooit using this promocode.</span></span> |

## <a name="6----create-your-marketplace-marketing-content"></a><span data-ttu-id="f8bab-173">6.    Skapa din Marketplace marknadsföring innehåll</span><span class="sxs-lookup"><span data-stu-id="f8bab-173">6.    Create your Marketplace marketing content</span></span>
<span data-ttu-id="f8bab-174">För hur tooprovide information krävs i **marknadsföring, prissättning, Support och kategorier** flikar besök [Marketplace Marketing Content guiden](marketplace-publishing-push-to-staging.md) som är vanliga tooall artefakter som publicerats i hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="f8bab-174">For How tooprovide information required in **Marketing, Pricing, Support and Categories** tabs please visit [Marketplace Marketing Content Guide](marketplace-publishing-push-to-staging.md) which is common tooall artifacts published in hello Azure Marketplace.</span></span>  

## <a name="7----connect-your-offer-tooyour-service-sql-azure-based-or-web-service-based"></a><span data-ttu-id="f8bab-175">7.    Anslut din erbjudande tooyour (SQL Azure-baserat eller webbtjänst baserat).</span><span class="sxs-lookup"><span data-stu-id="f8bab-175">7.    Connect your offer tooyour Service (SQL Azure based or Web Service based).</span></span>
<span data-ttu-id="f8bab-176">Klicka på hello **datatjänster** undermeny.</span><span class="sxs-lookup"><span data-stu-id="f8bab-176">Click on hello **Data Services** sub-menu.</span></span>

<span data-ttu-id="f8bab-177">På hello övre hälften av hello sidan blir du ombedd tooprovide hello erbjudande **Namespace**.</span><span class="sxs-lookup"><span data-stu-id="f8bab-177">On hello upper half of hello page you’ll be asked tooprovide hello offer’s **Namespace**.</span></span>  

  ![Rita](media/marketplace-publishing-data-service-creation/step-7.png)

<span data-ttu-id="f8bab-179">hello nedan fråga definierar hur hello Publisher kommer tooexpose nyskapad erbjudande tooAzure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="f8bab-179">hello below question will define how hello Publisher is going tooexpose newly created offer tooAzure Marketplace.</span></span> <span data-ttu-id="f8bab-180">(Mer information finns hello [Data Services tekniska nödvändiga Guide](marketplace-publishing-data-service-creation-prerequisites.md)).</span><span class="sxs-lookup"><span data-stu-id="f8bab-180">(For more details see hello [Data Services Technical Prerequisite Guide](marketplace-publishing-data-service-creation-prerequisites.md)).</span></span>

  ![Rita](media/marketplace-publishing-data-service-creation/step-7.2.png)

<span data-ttu-id="f8bab-182">**Publicera hello baserat databasen service**</span><span class="sxs-lookup"><span data-stu-id="f8bab-182">**Publishing hello Database based service**</span></span>

<span data-ttu-id="f8bab-183">Klicka på **databasen**.</span><span class="sxs-lookup"><span data-stu-id="f8bab-183">Click on **Database**.</span></span> <span data-ttu-id="f8bab-184">hello följande sida visas:</span><span class="sxs-lookup"><span data-stu-id="f8bab-184">hello following page will appear:</span></span>

  ![Rita](media/marketplace-publishing-data-service-creation/step-7.3.png)

<span data-ttu-id="f8bab-186">toocreate en CSDL-mappning för hello Dataset baserat på hello SQL Azure DB:</span><span class="sxs-lookup"><span data-stu-id="f8bab-186">toocreate a CSDL mapping for hello Dataset based on hello SQL Azure DB:</span></span>

  ![Rita](media/marketplace-publishing-data-service-creation/step-7.4.png)

<span data-ttu-id="f8bab-188">Och sedan för varje tabell</span><span class="sxs-lookup"><span data-stu-id="f8bab-188">And then for each table</span></span>

  ![Rita](media/marketplace-publishing-data-service-creation/step-7.5.png)

  ![Rita](media/marketplace-publishing-data-service-creation/step-7.6.png)

<span data-ttu-id="f8bab-191">Om webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="f8bab-191">If Web Service</span></span>

  ![Rita](media/marketplace-publishing-data-service-creation/step-7.7.png)

> [!IMPORTANT]
> <span data-ttu-id="f8bab-193">Läs [mappning av en befintlig web service tooOData via CSDL](marketplace-publishing-data-service-creation-odata-mapping.md) detaljerade anvisningar och exempel för att skapa en CSDL-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="f8bab-193">Read [Mapping an existing web service tooOData through CSDL](marketplace-publishing-data-service-creation-odata-mapping.md) for detailed instructions and examples for creating a CSDL Web Service.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="f8bab-194">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f8bab-194">Next Steps</span></span>
<span data-ttu-id="f8bab-195">Nu när du har skapat ditt Data tjänsterbjudande, se till att du har slutfört hello instruktionerna i hello [Marketplace Marketing Content guiden](marketplace-publishing-push-to-staging.md) innan du går framåt för[testa Data Service i Förproduktion](marketplace-publishing-data-service-test-in-staging.md).</span><span class="sxs-lookup"><span data-stu-id="f8bab-195">Now that you've created your Data Service offer, please ensure that you complete hello instructions in hello [Marketplace Marketing Content Guide](marketplace-publishing-push-to-staging.md) before you move forward too[Testing your Data Service in Staging](marketplace-publishing-data-service-test-in-staging.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="f8bab-196">Se även</span><span class="sxs-lookup"><span data-stu-id="f8bab-196">See Also</span></span>
* [<span data-ttu-id="f8bab-197">Komma igång: Hur toopublish ett erbjudande toohello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="f8bab-197">Getting Started: How toopublish an offer toohello Azure Marketplace</span></span>](marketplace-publishing-getting-started.md)
* <span data-ttu-id="f8bab-198">Om du vill förstå hello övergripande processen för OData-mappning och ändamål, den här artikeln [mappning för OData-tjänsten](marketplace-publishing-data-service-creation-odata-mapping.md) tooreview definitioner, strukturer och instruktioner.</span><span class="sxs-lookup"><span data-stu-id="f8bab-198">If you are interested in understanding hello overall OData mapping process and purpose, read this article [Data Service OData Mapping](marketplace-publishing-data-service-creation-odata-mapping.md) tooreview definitions, structures, and instructions.</span></span>
* <span data-ttu-id="f8bab-199">Om du är intresserad av utbildning och förstå hello specifika noder och deras parametrar kan den här artikeln [mappa Data Service OData-noder](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) definitioner och förklaringar, exempel och Använd case kontext.</span><span class="sxs-lookup"><span data-stu-id="f8bab-199">If you are interested in learning and understanding hello specific nodes and their parameters, read this article [Data Service OData Mapping Nodes](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) for definitions and explanations, examples, and use case context.</span></span>
* <span data-ttu-id="f8bab-200">Om du vill granska exempel den här artikeln [mappa Data Service OData-exempel](marketplace-publishing-data-service-creation-odata-mapping-examples.md) toosee exempelkoden och förstå kodsyntax och kontext.</span><span class="sxs-lookup"><span data-stu-id="f8bab-200">If you are interested in reviewing examples, read this article [Data Service OData Mapping Examples](marketplace-publishing-data-service-creation-odata-mapping-examples.md) toosee sample code and understand code syntax and context.</span></span>

[link-pubportal]:https://publish.windowsazure.com
