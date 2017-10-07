---
title: aaaAzure Application Insights skorstenar
description: "Lär dig hur du kan använda skorstenar toodiscover hur kunder interagerar med programmet."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: bwren
ms.openlocfilehash: 2a6125cf596570cfaee30bb3ff757916e90d7676
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="discover-how-customers-are-using-your-application-with-hello-application-insights-funnels"></a><span data-ttu-id="b0480-103">Identifiera hur kunder använder ditt program med hello Application Insights skorstenar</span><span class="sxs-lookup"><span data-stu-id="b0480-103">Discover how customers are using your application with hello Application Insights Funnels</span></span>

<span data-ttu-id="b0480-104">Förstå kundupplevelsen är hello yttersta vikt tooyour verksamhet.</span><span class="sxs-lookup"><span data-stu-id="b0480-104">Understanding customer experience is of hello utmost importance tooyour business.</span></span> <span data-ttu-id="b0480-105">Om ditt program innefattar flera faser, behöver tooknow om de flesta kunder fortskrider via hello hela processen, eller om de avslutande hello processen vid något tillfälle.</span><span class="sxs-lookup"><span data-stu-id="b0480-105">If your application involves multiple stages, you will need tooknow if most customers are progressing through hello entire process, or if they are ending hello process at some point.</span></span> <span data-ttu-id="b0480-106">hello förlopp i en serie steg i ett webbprogram kallas ”Trattens”.</span><span class="sxs-lookup"><span data-stu-id="b0480-106">hello progression through a series of steps in a web application is known as a "funnel".</span></span> <span data-ttu-id="b0480-107">Du kan använda hello Application Insights skorstenar toogain insikter om dina användare och övervaka stegvisa konvertering priser.</span><span class="sxs-lookup"><span data-stu-id="b0480-107">You can use hello Application Insights Funnels toogain insights into your users and monitor step-by-step conversion rates.</span></span> 

## <a name="get-started-with-hello-funnels-blade"></a><span data-ttu-id="b0480-108">Kom igång med hello skorstenar bladet</span><span class="sxs-lookup"><span data-stu-id="b0480-108">Get started with hello Funnels blade</span></span>
<span data-ttu-id="b0480-109">hello enklaste sättet toolearn om skorstenar är toowalk även om ett exempel.</span><span class="sxs-lookup"><span data-stu-id="b0480-109">hello easiest way toolearn about Funnels is toowalk though an example.</span></span> <span data-ttu-id="b0480-110">hello visar följande bilder hello steg ägarna av en e-handel tar toolearn hur kunderna samverkar med deras webbprogram.</span><span class="sxs-lookup"><span data-stu-id="b0480-110">hello following illustrations demonstrate hello steps owners of an e-commerce business would take toolearn how their customers interact with their web application.</span></span>  

### <a name="create-your-funnel"></a><span data-ttu-id="b0480-111">Skapa din tratten</span><span class="sxs-lookup"><span data-stu-id="b0480-111">Create your funnel</span></span>
<span data-ttu-id="b0480-112">Innan du skapar din Trattens måste toodecide hello frågan ska tooanswer.</span><span class="sxs-lookup"><span data-stu-id="b0480-112">Before you create your funnel, you need toodecide on hello question you want tooanswer.</span></span> <span data-ttu-id="b0480-113">Du kanske exempelvis vill tooknow hur många kunder visa startsidan-Klicka på en annons.</span><span class="sxs-lookup"><span data-stu-id="b0480-113">For example, you might want tooknow how many customers viewing your home page click on an advertisement.</span></span> <span data-ttu-id="b0480-114">I det här exemplet vill hello ägare av hello Fabrikam Fiber företagets tooknow hello procentandelen kunder som gör ett inköp när du lägger till objekt tootheir kundvagn under hello senaste månaden.</span><span class="sxs-lookup"><span data-stu-id="b0480-114">In this example, hello owners of hello Fabrikam Fiber company want tooknow hello percentage of customers who make a purchase after adding items tootheir shopping cart during hello last month.</span></span>

<span data-ttu-id="b0480-115">Här är hello åtgärder de toocreate sina tratten.</span><span class="sxs-lookup"><span data-stu-id="b0480-115">Here are hello steps they take toocreate their funnel.</span></span>

1. <span data-ttu-id="b0480-116">Klicka på hello-knappen på hello skorstenar bladet på nytt.</span><span class="sxs-lookup"><span data-stu-id="b0480-116">Click hello New button on hello Funnels blade.</span></span>
1. <span data-ttu-id="b0480-117">Välj hello tidsintervall för ”senaste månad” Hej **tidsintervall** listrutan.</span><span class="sxs-lookup"><span data-stu-id="b0480-117">Select hello time range of "Last month" from hello **Time Range** drop-down.</span></span> 
1. <span data-ttu-id="b0480-118">Välj hello **produktsidan** händelse från hello **steg 1** listrutan.</span><span class="sxs-lookup"><span data-stu-id="b0480-118">Select hello **Product page** event from hello **Step 1** drop-down list.</span></span> 
1. <span data-ttu-id="b0480-119">Välj hello **Lägg till tooshopping kundvagn** händelse från hello **steg 2** listrutan.</span><span class="sxs-lookup"><span data-stu-id="b0480-119">Select hello **Add tooshopping cart** event from hello **Step 2** drop-down list.</span></span>
1. <span data-ttu-id="b0480-120">Välj hello **Klicka på Köp** händelse från hello **steg3** listrutan.</span><span class="sxs-lookup"><span data-stu-id="b0480-120">Select hello **Click purchase** event from hello **Step 3** drop-down list.</span></span>
1. <span data-ttu-id="b0480-121">Lägga till ett namn toohello Trattens och på **spara**.</span><span class="sxs-lookup"><span data-stu-id="b0480-121">Add a name toohello funnel and click **Save**.</span></span>

<span data-ttu-id="b0480-122">hello följande bild visar hello data hello skorstenar bladet genererar.</span><span class="sxs-lookup"><span data-stu-id="b0480-122">hello following illustration demonstrates hello data hello Funnels blade generates.</span></span> <span data-ttu-id="b0480-123">Här hello Se Fabrikam ägare att 22.7% av sina kunder som lagts till ett objekt tootheir shopping under hello föregående vecka, kundvagn slutförda hello inköp.</span><span class="sxs-lookup"><span data-stu-id="b0480-123">From here hello Fabrikam owners can see that during hello last week, 22.7% of their customers who added an item tootheir shopping cart completed hello purchase.</span></span> <span data-ttu-id="b0480-124">De kan också se att 1% av hello kunder klickat på en annons innan besöker hello produktsidan och 20% av sina kunder loggas när du har slutfört sin inköp.</span><span class="sxs-lookup"><span data-stu-id="b0480-124">They can also see that 1% of hello customers clicked an advertisement before visiting hello product page, and 20% of their customers signed out after completing their purchase.</span></span>


![Skorstenar bladet med data](./media/app-insights-understand-usage-patterns/funnel1.png)

## <a name="next-steps"></a><span data-ttu-id="b0480-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b0480-126">Next steps</span></span>
  * [<span data-ttu-id="b0480-127">Översikt över användning</span><span class="sxs-lookup"><span data-stu-id="b0480-127">Usage overview</span></span>](app-insights-usage-overview.md)
  * [<span data-ttu-id="b0480-128">Användare, sessioner och händelser</span><span class="sxs-lookup"><span data-stu-id="b0480-128">Users, Sessions, and Events</span></span>](app-insights-usage-segmentation.md)
  * [<span data-ttu-id="b0480-129">Kvarhållning</span><span class="sxs-lookup"><span data-stu-id="b0480-129">Retention</span></span>](app-insights-usage-retention.md)
  * [<span data-ttu-id="b0480-130">Arbetsböcker</span><span class="sxs-lookup"><span data-stu-id="b0480-130">Workbooks</span></span>](app-insights-usage-workbooks.md)
  * [<span data-ttu-id="b0480-131">Lägg till användarkontext</span><span class="sxs-lookup"><span data-stu-id="b0480-131">Add user context</span></span>](app-insights-usage-send-user-context.md)
