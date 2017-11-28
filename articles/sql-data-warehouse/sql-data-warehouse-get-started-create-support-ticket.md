---
title: "Så här skapar du ett supportärende för SQL Data Warehouse | Microsoft Docs"
description: "Så här skapar du ett supportärende i Azure SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: a91d1f53-03cb-464b-9d5b-4a9c1a194ed3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: 058ff1229acee5d03db7c0305c5565ae95a85758
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-a-support-ticket-for-sql-data-warehouse"></a><span data-ttu-id="c6f1b-103">Så här skapar du ett supportärende för SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="c6f1b-103">How to create a support ticket for SQL Data Warehouse</span></span>
<span data-ttu-id="c6f1b-104">Om du har problem med din SQL Data Warehouse, ber vi dig skapa ett supportärende så att våra tekniker kan hjälpa dig.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-104">If you are having any issues with your SQL Data Warehouse, please create a support ticket so that our engineering team can assist you.</span></span>

> [!NOTE] 
> <span data-ttu-id="c6f1b-105">Från och med 20/12/2016 stämmer inte hälsokontrollen för resurser i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-105">As of 12/20/2016, the resource health check in the Azure portal is not accurate.</span></span> <span data-ttu-id="c6f1b-106">Vi arbetar för att åtgärda problemet.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-106">We are actively working to fix this issue.</span></span> 


## <a name="create-a-support-ticket"></a><span data-ttu-id="c6f1b-107">Skapa ett supportärende</span><span class="sxs-lookup"><span data-stu-id="c6f1b-107">Create a support ticket</span></span>
1. <span data-ttu-id="c6f1b-108">Öppna [Azure-portalen][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="c6f1b-108">Open the [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="c6f1b-109">På startsidan klickar du på **hjälp + support**-panelen.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-109">On the Home screen, click the **Help + support** tile.</span></span>
   
    ![Hjälp + support](./media/sql-data-warehouse-get-started-create-support-ticket/help-support.png)
3. <span data-ttu-id="c6f1b-111">På Hjälp + Support-bladet, klickar du på **skapa supportbegäran**.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-111">On the Help + Support blade, click **Create support request**.</span></span>
   
    ![Ny supportbegäran](./media/sql-data-warehouse-get-started-create-support-ticket/create-support-request.png)
   
    <a name="request-quota-change"></a> 
4. <span data-ttu-id="c6f1b-113">Välj **typ av begäran**.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-113">Select the **Request Type**.</span></span>
   
    ![Typ av begäran](./media/sql-data-warehouse-get-started-create-support-ticket/request-type.png)
   
   > [!NOTE]
   > <span data-ttu-id="c6f1b-115">Varje SQL Server (t.ex. myserver.database.windows.net) har som standard en **DTU-kvot** på 45 000.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-115">By default, each SQL server (e.g. myserver.database.windows.net) has a **DTU Quota** of 45,000.</span></span> <span data-ttu-id="c6f1b-116">Kvoten är helt enkelt en säkerhetsgräns.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-116">This quota is simply a safety limit.</span></span> <span data-ttu-id="c6f1b-117">Du kan öka din kvot genom att skapa en supportbiljett och välja *Kvot* som typ av begäran.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-117">You can increase your quota by creating a support ticket and selecting *Quota* as the request type.</span></span> <span data-ttu-id="c6f1b-118">För att beräkna dina DTU-behov, multiplicerar du 7,5 med det totala antalet [DWU:er][DWU] du behöver.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-118">To calculate your DTU needs, multiply the 7.5 by the total [DWU][DWU] needed.</span></span> <span data-ttu-id="c6f1b-119">Om du till exempel skulle vilja vara värd för två DW6000 på en SQL Server, bör du begära en DTU-kvot på 90 000.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-119">For example, you would like to host two DW6000s on one SQL server, then you should request a DTU quota of 90,000.</span></span>  <span data-ttu-id="c6f1b-120">Du kan visa din aktuella DTU-förbrukning från SQL Server-bladet i portalen.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-120">You can view your current DTU consumption from the SQL server blade in the portal.</span></span> <span data-ttu-id="c6f1b-121">Både pausade och inte pausade databaser räknas i förhållande till DTU-kvoten.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-121">Both paused and un-paused databases count toward the DTU quota.</span></span> 
   > 
   > 
5. <span data-ttu-id="c6f1b-122">Välj den **prenumeration** som är värd för databasen som har problemen du ska rapportera.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-122">Select the **Subscription** that hosts the database with the problem you are reporting.</span></span>
   
    ![Prenumeration](./media/sql-data-warehouse-get-started-create-support-ticket/subscription.png)
6. <span data-ttu-id="c6f1b-124">Välj **SQL Data Warehouse** som resursen.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-124">Select **SQL Data Warehouse** as the Resource.</span></span>
   
    ![Resurs](./media/sql-data-warehouse-get-started-create-support-ticket/resource.png)
7. <span data-ttu-id="c6f1b-126">Välj din [Azure-supportplan][Azure support plan].</span><span class="sxs-lookup"><span data-stu-id="c6f1b-126">Select your [Azure support plan][Azure support plan].</span></span>
   
   * <span data-ttu-id="c6f1b-127">Stöd för **fakturerings-, kvot- och prenumerationhantering** finns tillgängligt på alla supportnivåer.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-127">**Billing, quota and subscription management** support is available at all support levels.</span></span>
   * <span data-ttu-id="c6f1b-128">**Reparations**-support tillhandahålls via [Utvecklare][Developer], [Standard][Standard], [Professional Direct][Professional Direct] eller [Premier][Premier] support.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-128">**Break-fix** support is provided through [Developer][Developer], [Standard][Standard], [Professional Direct][Professional Direct] or [Premier][Premier] support.</span></span> <span data-ttu-id="c6f1b-129">Reparationsärenden är problem som kunder upplever när de använder Azure och där det rimligen kan antas att Microsoft orsakade problemet.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-129">Break-fix issues are problems experienced by customers while using Azure where there is a reasonable expectation that Microsoft caused the problem.</span></span>
   * <span data-ttu-id="c6f1b-130">**Utvecklarhandledning** och **rådgivningstjänster** finns tillgängliga på supportnivåerna [Professional Direct][Professional Direct] och [Premier][Premier].</span><span class="sxs-lookup"><span data-stu-id="c6f1b-130">**Developer mentoring** and **advisory services** are available at the [Professional Direct][Professional Direct] and [Premier][Premier] support levels.</span></span> 
     
     <span data-ttu-id="c6f1b-131">Om du har en Premier-supportplan, kan du även rapportera SQL Data Warehouse-relaterade problem på [Microsoft Premier-onlineportalen][Microsoft Premier online portal].</span><span class="sxs-lookup"><span data-stu-id="c6f1b-131">If you have a Premier support plan, you can also report SQL Data Warehouse related issues on the [Microsoft Premier online portal][Microsoft Premier online portal].</span></span>  <span data-ttu-id="c6f1b-132">Se [Azure-supportplaner][Azure support plan] för att läsa mer om de olika supportplanerna, inklusive omfattning, svarstider, prissättning, osv.  Vanliga frågor och svar om Azure-support finns på [Vanliga frågor och svar om Azure-support][Azure support FAQs].</span><span class="sxs-lookup"><span data-stu-id="c6f1b-132">See [Azure support plans][Azure support plan] to learn more about the various support plans, including scope, response times, pricing, etc.  For frequently asked questions about Azure support, see [Azure support FAQs][Azure support FAQs].</span></span>  
     
     ![Supportplan](./media/sql-data-warehouse-get-started-create-support-ticket/support-plan.png)
8. <span data-ttu-id="c6f1b-134">Välj **problemtyp** och **kategori**.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-134">Select the **Problem Type** and **Category**.</span></span> <span data-ttu-id="c6f1b-135">I det här exemplet har vi valt ”Verktyg” som problemtyp och ”Klientverktyg” som kategori.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-135">In this example, we have chosen "Tools" as the Problem type and "Client tools" as the category.</span></span> 
   
    ![Problemtyp-kategori](./media/sql-data-warehouse-get-started-create-support-ticket/problem-type-category.png)
9. <span data-ttu-id="c6f1b-137">Beskriv problemet och välj vilken inverkan det har på verksamheten.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-137">Describe the problem and choose the level of business impact.</span></span>
   
    ![Problembeskrivning](./media/sql-data-warehouse-get-started-create-support-ticket/problem-description.png)
10. <span data-ttu-id="c6f1b-139">Din **kontaktinformation** för det här supportärendet fylls i automatiskt.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-139">Your **contact information** for this support ticket will be pre-filled.</span></span> <span data-ttu-id="c6f1b-140">Uppdatera den vid behov.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-140">Update this if necessary.</span></span>
    
    ![Kontaktuppgifter](./media/sql-data-warehouse-get-started-create-support-ticket/contact-info.png)
11. <span data-ttu-id="c6f1b-142">Klicka på **skapa** för att skicka in supportbegäran.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-142">Click **Create** to submit the support request.</span></span>

## <a name="monitor-a-support-ticket"></a><span data-ttu-id="c6f1b-143">Övervaka ett supportärende</span><span class="sxs-lookup"><span data-stu-id="c6f1b-143">Monitor a support ticket</span></span>
<span data-ttu-id="c6f1b-144">När du har skickat in supportbegäran, kommer Azure-teamet att kontakta dig.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-144">After you have submitted the support request, the Azure support team will contact you.</span></span> <span data-ttu-id="c6f1b-145">Du kan kontrollera status och detaljer för din begäran genom att klicka på **hantera supportärenden** på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="c6f1b-145">To check your request status and details, click **Manage support requests** on the dashboard.</span></span>

![Kontrollera status](./media/sql-data-warehouse-get-started-create-support-ticket/check-status.png)

## <a name="other-resources"></a><span data-ttu-id="c6f1b-147">Andra resurser</span><span class="sxs-lookup"><span data-stu-id="c6f1b-147">Other Resources</span></span>
<span data-ttu-id="c6f1b-148">Du kan dessutom ansluta till SQL Data Warehouse-communityn på [Stack Overflow][Stack Overflow] eller på [Azure SQL Data Warehouse MSDN-forumet][Azure SQL Data Warehouse MSDN forum].</span><span class="sxs-lookup"><span data-stu-id="c6f1b-148">Additionally, you can connect with the SQL Data Warehouse community on [Stack Overflow][Stack Overflow] or on the [Azure SQL Data Warehouse MSDN forum][Azure SQL Data Warehouse MSDN forum].</span></span>

<!--Image references--> 

<!--Article references--> 
[DWU]: ./sql-data-warehouse-overview-what-is.md

<!--MSDN references--> 

<!--Other web references--> 
[Azure portal]: https://portal.azure.com/
[Azure support plan]: https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/  
[Developer]: https://azure.microsoft.com/support/plans/developer/  
[Standard]: https://azure.microsoft.com/support/plans/standard/  
[Professional Direct]: https://azure.microsoft.com/support/plans/prodirect/  
[Premier]: https://azure.microsoft.com/support/plans/premier/  
[Azure support FAQs]: https://azure.microsoft.com/support/faq/
[Microsoft Premier online portal]: https://premier.microsoft.com/
[Stack Overflow]: https://stackoverflow.com/questions/tagged/azure-sqldw/
[Azure SQL Data Warehouse MSDN forum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/

