---
title: Azure Application Insights skorstenar
description: "Lär dig hur du kan använda skorstenar för att identifiera hur kunder interagerar med programmet."
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
ms.openlocfilehash: 59c4dfafc102b26e3b9873f433065715f4aec9ec
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="discover-how-customers-are-using-your-application-with-the-application-insights-funnels"></a><span data-ttu-id="c6120-103">Identifiera hur kunder använder ditt program med Application Insights skorstenar</span><span class="sxs-lookup"><span data-stu-id="c6120-103">Discover how customers are using your application with the Application Insights Funnels</span></span>

<span data-ttu-id="c6120-104">Förstå kundupplevelsen är av yttersta vikt för företaget.</span><span class="sxs-lookup"><span data-stu-id="c6120-104">Understanding customer experience is of the utmost importance to your business.</span></span> <span data-ttu-id="c6120-105">Om ditt program innefattar flera faser, kommer du behöver veta om de flesta kunder fortskrider genom hela processen, eller om de avslutar processen vid något tillfälle.</span><span class="sxs-lookup"><span data-stu-id="c6120-105">If your application involves multiple stages, you will need to know if most customers are progressing through the entire process, or if they are ending the process at some point.</span></span> <span data-ttu-id="c6120-106">Förlopp i en serie steg i ett webbprogram kallas ”Trattens”.</span><span class="sxs-lookup"><span data-stu-id="c6120-106">The progression through a series of steps in a web application is known as a "funnel".</span></span> <span data-ttu-id="c6120-107">Du kan använda Application Insights skorstenar och få insikter om dina användare och övervaka stegvisa konvertering priser.</span><span class="sxs-lookup"><span data-stu-id="c6120-107">You can use the Application Insights Funnels to gain insights into your users and monitor step-by-step conversion rates.</span></span> 

## <a name="get-started-with-the-funnels-blade"></a><span data-ttu-id="c6120-108">Kom igång med bladet skorstenar</span><span class="sxs-lookup"><span data-stu-id="c6120-108">Get started with the Funnels blade</span></span>
<span data-ttu-id="c6120-109">Det enklaste sättet att lära dig om skorstenar är att gå via ett exempel.</span><span class="sxs-lookup"><span data-stu-id="c6120-109">The easiest way to learn about Funnels is to walk though an example.</span></span> <span data-ttu-id="c6120-110">Följande bilder visar stegen ägarna av en e-handel tar att lära dig hur kunderna samverkar med deras webbprogram.</span><span class="sxs-lookup"><span data-stu-id="c6120-110">The following illustrations demonstrate the steps owners of an e-commerce business would take to learn how their customers interact with their web application.</span></span>  

### <a name="create-your-funnel"></a><span data-ttu-id="c6120-111">Skapa din tratten</span><span class="sxs-lookup"><span data-stu-id="c6120-111">Create your funnel</span></span>
<span data-ttu-id="c6120-112">Innan du skapar din Trattens måste du bestämma om du vill att besvara frågan.</span><span class="sxs-lookup"><span data-stu-id="c6120-112">Before you create your funnel, you need to decide on the question you want to answer.</span></span> <span data-ttu-id="c6120-113">Du kanske vill veta hur många kunder visa startsidan-Klicka på en annons.</span><span class="sxs-lookup"><span data-stu-id="c6120-113">For example, you might want to know how many customers viewing your home page click on an advertisement.</span></span> <span data-ttu-id="c6120-114">I det här exemplet vill ägarna av företaget Fabrikam Fiber veta procentandelen kunder som gör ett inköp när du lägger till objekt till deras kundvagn under den senaste månaden.</span><span class="sxs-lookup"><span data-stu-id="c6120-114">In this example, the owners of the Fabrikam Fiber company want to know the percentage of customers who make a purchase after adding items to their shopping cart during the last month.</span></span>

<span data-ttu-id="c6120-115">Här är de steg som de vidtar för att skapa deras tratten.</span><span class="sxs-lookup"><span data-stu-id="c6120-115">Here are the steps they take to create their funnel.</span></span>

1. <span data-ttu-id="c6120-116">Klicka på knappen Nytt i bladet skorstenar.</span><span class="sxs-lookup"><span data-stu-id="c6120-116">Click the New button on the Funnels blade.</span></span>
1. <span data-ttu-id="c6120-117">Ange tidsintervall för ”senaste månad” från den **tidsintervall** listrutan.</span><span class="sxs-lookup"><span data-stu-id="c6120-117">Select the time range of "Last month" from the **Time Range** drop-down.</span></span> 
1. <span data-ttu-id="c6120-118">Välj den **produktsidan** händelse från den **steg 1** listrutan.</span><span class="sxs-lookup"><span data-stu-id="c6120-118">Select the **Product page** event from the **Step 1** drop-down list.</span></span> 
1. <span data-ttu-id="c6120-119">Välj den **Lägg till i kundvagn** händelse från den **steg 2** listrutan.</span><span class="sxs-lookup"><span data-stu-id="c6120-119">Select the **Add to shopping cart** event from the **Step 2** drop-down list.</span></span>
1. <span data-ttu-id="c6120-120">Välj den **Klicka på Köp** händelse från den **steg3** listrutan.</span><span class="sxs-lookup"><span data-stu-id="c6120-120">Select the **Click purchase** event from the **Step 3** drop-down list.</span></span>
1. <span data-ttu-id="c6120-121">Lägg till ett namn i tratten och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="c6120-121">Add a name to the funnel and click **Save**.</span></span>

<span data-ttu-id="c6120-122">Följande bild visar data bladet skorstenar genererar.</span><span class="sxs-lookup"><span data-stu-id="c6120-122">The following illustration demonstrates the data the Funnels blade generates.</span></span> <span data-ttu-id="c6120-123">Härifrån Fabrikam ser ägare att 22.7% av sina kunder som lagts till ett objekt till deras kundvagn slutfört köpet under den senaste veckan.</span><span class="sxs-lookup"><span data-stu-id="c6120-123">From here the Fabrikam owners can see that during the last week, 22.7% of their customers who added an item to their shopping cart completed the purchase.</span></span> <span data-ttu-id="c6120-124">De kan också se att 1% av kunderna klickat på en annons innan besöka produktsidan och 20% av sina kunder loggas när du har slutfört sin inköp.</span><span class="sxs-lookup"><span data-stu-id="c6120-124">They can also see that 1% of the customers clicked an advertisement before visiting the product page, and 20% of their customers signed out after completing their purchase.</span></span>


![Skorstenar bladet med data](./media/app-insights-understand-usage-patterns/funnel1.png)

## <a name="next-steps"></a><span data-ttu-id="c6120-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c6120-126">Next steps</span></span>
  * [<span data-ttu-id="c6120-127">Översikt över användning</span><span class="sxs-lookup"><span data-stu-id="c6120-127">Usage overview</span></span>](app-insights-usage-overview.md)
  * [<span data-ttu-id="c6120-128">Användare, sessioner och händelser</span><span class="sxs-lookup"><span data-stu-id="c6120-128">Users, Sessions, and Events</span></span>](app-insights-usage-segmentation.md)
  * [<span data-ttu-id="c6120-129">Kvarhållning</span><span class="sxs-lookup"><span data-stu-id="c6120-129">Retention</span></span>](app-insights-usage-retention.md)
  * [<span data-ttu-id="c6120-130">Arbetsböcker</span><span class="sxs-lookup"><span data-stu-id="c6120-130">Workbooks</span></span>](app-insights-usage-workbooks.md)
  * [<span data-ttu-id="c6120-131">Lägg till användarkontext</span><span class="sxs-lookup"><span data-stu-id="c6120-131">Add user context</span></span>](app-insights-usage-send-user-context.md)
