---
title: "aaaHow toocreate ett supportärende för SQL Data Warehouse | Microsoft Docs"
description: "Hur biljett toocreate stöd i Azure SQL Data Warehouse."
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
ms.openlocfilehash: 72f7eac82112fb7f1bfb05abca4ce40aeb3c828c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-support-ticket-for-sql-data-warehouse"></a><span data-ttu-id="4b84f-103">Hur toocreate en biljett för SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="4b84f-103">How toocreate a support ticket for SQL Data Warehouse</span></span>
<span data-ttu-id="4b84f-104">Om du har problem med din SQL Data Warehouse, ber vi dig skapa ett supportärende så att våra tekniker kan hjälpa dig.</span><span class="sxs-lookup"><span data-stu-id="4b84f-104">If you are having any issues with your SQL Data Warehouse, please create a support ticket so that our engineering team can assist you.</span></span>

> [!NOTE] 
> <span data-ttu-id="4b84f-105">Från och med 2016/12/20 stämmer inte hello resurs hälsokontroll i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4b84f-105">As of 12/20/2016, hello resource health check in hello Azure portal is not accurate.</span></span> <span data-ttu-id="4b84f-106">Vi arbetar aktivt toofix problemet.</span><span class="sxs-lookup"><span data-stu-id="4b84f-106">We are actively working toofix this issue.</span></span> 


## <a name="create-a-support-ticket"></a><span data-ttu-id="4b84f-107">Skapa ett supportärende</span><span class="sxs-lookup"><span data-stu-id="4b84f-107">Create a support ticket</span></span>
1. <span data-ttu-id="4b84f-108">Öppna hello [Azure-portalen][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="4b84f-108">Open hello [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="4b84f-109">Klicka på startsidan för hello hello **hjälp + support** panelen.</span><span class="sxs-lookup"><span data-stu-id="4b84f-109">On hello Home screen, click hello **Help + support** tile.</span></span>
   
    ![Hjälp + support](./media/sql-data-warehouse-get-started-create-support-ticket/help-support.png)
3. <span data-ttu-id="4b84f-111">Klicka på hello hjälp + Support-bladet, **skapa supportbegäran**.</span><span class="sxs-lookup"><span data-stu-id="4b84f-111">On hello Help + Support blade, click **Create support request**.</span></span>
   
    ![Ny supportbegäran](./media/sql-data-warehouse-get-started-create-support-ticket/create-support-request.png)
   
    <a name="request-quota-change"></a> 
4. <span data-ttu-id="4b84f-113">Välj hello **begära typen**.</span><span class="sxs-lookup"><span data-stu-id="4b84f-113">Select hello **Request Type**.</span></span>
   
    ![Typ av begäran](./media/sql-data-warehouse-get-started-create-support-ticket/request-type.png)
   
   > [!NOTE]
   > <span data-ttu-id="4b84f-115">Varje SQL Server (t.ex. myserver.database.windows.net) har som standard en **DTU-kvot** på 45 000.</span><span class="sxs-lookup"><span data-stu-id="4b84f-115">By default, each SQL server (e.g. myserver.database.windows.net) has a **DTU Quota** of 45,000.</span></span> <span data-ttu-id="4b84f-116">Kvoten är helt enkelt en säkerhetsgräns.</span><span class="sxs-lookup"><span data-stu-id="4b84f-116">This quota is simply a safety limit.</span></span> <span data-ttu-id="4b84f-117">Du kan öka din kvot genom att skapa ett supportärende och välja *kvot* som hello typ av begäran.</span><span class="sxs-lookup"><span data-stu-id="4b84f-117">You can increase your quota by creating a support ticket and selecting *Quota* as hello request type.</span></span> <span data-ttu-id="4b84f-118">toocalculate din DTU måste, multiplicera hello 7.5 av hello totala [DWU] [ DWU] behövs.</span><span class="sxs-lookup"><span data-stu-id="4b84f-118">toocalculate your DTU needs, multiply hello 7.5 by hello total [DWU][DWU] needed.</span></span> <span data-ttu-id="4b84f-119">Till exempel som toohost två DW6000s på en SQL-server kan du begära en DTU-kvot på 90,000.</span><span class="sxs-lookup"><span data-stu-id="4b84f-119">For example, you would like toohost two DW6000s on one SQL server, then you should request a DTU quota of 90,000.</span></span>  <span data-ttu-id="4b84f-120">Du kan visa din aktuella DTU-förbrukning från hello SQL server-bladet i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="4b84f-120">You can view your current DTU consumption from hello SQL server blade in hello portal.</span></span> <span data-ttu-id="4b84f-121">Både pausas och icke pausas räknas in hello DTU-kvot.</span><span class="sxs-lookup"><span data-stu-id="4b84f-121">Both paused and un-paused databases count toward hello DTU quota.</span></span> 
   > 
   > 
5. <span data-ttu-id="4b84f-122">Välj hello **prenumeration** att värdar hello databasen med hello problem som du rapporterar.</span><span class="sxs-lookup"><span data-stu-id="4b84f-122">Select hello **Subscription** that hosts hello database with hello problem you are reporting.</span></span>
   
    ![Prenumeration](./media/sql-data-warehouse-get-started-create-support-ticket/subscription.png)
6. <span data-ttu-id="4b84f-124">Välj **SQL Data Warehouse** som hello resurs.</span><span class="sxs-lookup"><span data-stu-id="4b84f-124">Select **SQL Data Warehouse** as hello Resource.</span></span>
   
    ![Resurs](./media/sql-data-warehouse-get-started-create-support-ticket/resource.png)
7. <span data-ttu-id="4b84f-126">Välj din [Azure-supportplan][Azure support plan].</span><span class="sxs-lookup"><span data-stu-id="4b84f-126">Select your [Azure support plan][Azure support plan].</span></span>
   
   * <span data-ttu-id="4b84f-127">Stöd för **fakturerings-, kvot- och prenumerationhantering** finns tillgängligt på alla supportnivåer.</span><span class="sxs-lookup"><span data-stu-id="4b84f-127">**Billing, quota and subscription management** support is available at all support levels.</span></span>
   * <span data-ttu-id="4b84f-128">**Reparations**-support tillhandahålls via [Utvecklare][Developer], [Standard][Standard], [Professional Direct][Professional Direct] eller [Premier][Premier] support.</span><span class="sxs-lookup"><span data-stu-id="4b84f-128">**Break-fix** support is provided through [Developer][Developer], [Standard][Standard], [Professional Direct][Professional Direct] or [Premier][Premier] support.</span></span> <span data-ttu-id="4b84f-129">Reparations problem är problem som kunder upplever när du använder Azure där det finns en rimligen kan antas att Microsoft orsakade hello problem.</span><span class="sxs-lookup"><span data-stu-id="4b84f-129">Break-fix issues are problems experienced by customers while using Azure where there is a reasonable expectation that Microsoft caused hello problem.</span></span>
   * <span data-ttu-id="4b84f-130">**Utvecklarhandledning** och **rådgivningstjänster** finns på hello [Professional Direct] [ Professional Direct] och [Premier] [ Premier] supportnivåer.</span><span class="sxs-lookup"><span data-stu-id="4b84f-130">**Developer mentoring** and **advisory services** are available at hello [Professional Direct][Professional Direct] and [Premier][Premier] support levels.</span></span> 
     
     <span data-ttu-id="4b84f-131">Om du har en Premier-supportplan kan du även rapportera SQL Data Warehouse problem på hello [Microsoft Premier-onlineportalen][Microsoft Premier online portal].</span><span class="sxs-lookup"><span data-stu-id="4b84f-131">If you have a Premier support plan, you can also report SQL Data Warehouse related issues on hello [Microsoft Premier online portal][Microsoft Premier online portal].</span></span>  <span data-ttu-id="4b84f-132">Se [Azure-supportplaner] [ Azure support plan] toolearn mer om hello olika stöder planer, inklusive omfattning, svarstider, prissättning, osv.  Vanliga frågor och svar om Azure-support finns på [Vanliga frågor och svar om Azure-support][Azure support FAQs].</span><span class="sxs-lookup"><span data-stu-id="4b84f-132">See [Azure support plans][Azure support plan] toolearn more about hello various support plans, including scope, response times, pricing, etc.  For frequently asked questions about Azure support, see [Azure support FAQs][Azure support FAQs].</span></span>  
     
     ![Supportplan](./media/sql-data-warehouse-get-started-create-support-ticket/support-plan.png)
8. <span data-ttu-id="4b84f-134">Välj hello **problemtyp** och **kategori**.</span><span class="sxs-lookup"><span data-stu-id="4b84f-134">Select hello **Problem Type** and **Category**.</span></span> <span data-ttu-id="4b84f-135">I det här exemplet har vi valt hello problemtyp ”verktygen” och ”klientverktyg” som hello kategori.</span><span class="sxs-lookup"><span data-stu-id="4b84f-135">In this example, we have chosen "Tools" as hello Problem type and "Client tools" as hello category.</span></span> 
   
    ![Problemtyp-kategori](./media/sql-data-warehouse-get-started-create-support-ticket/problem-type-category.png)
9. <span data-ttu-id="4b84f-137">Beskriv problemet hello och välj hello nivå av företagsinverkan.</span><span class="sxs-lookup"><span data-stu-id="4b84f-137">Describe hello problem and choose hello level of business impact.</span></span>
   
    ![Problembeskrivning](./media/sql-data-warehouse-get-started-create-support-ticket/problem-description.png)
10. <span data-ttu-id="4b84f-139">Din **kontaktinformation** för det här supportärendet fylls i automatiskt.</span><span class="sxs-lookup"><span data-stu-id="4b84f-139">Your **contact information** for this support ticket will be pre-filled.</span></span> <span data-ttu-id="4b84f-140">Uppdatera den vid behov.</span><span class="sxs-lookup"><span data-stu-id="4b84f-140">Update this if necessary.</span></span>
    
    ![Kontaktuppgifter](./media/sql-data-warehouse-get-started-create-support-ticket/contact-info.png)
11. <span data-ttu-id="4b84f-142">Klicka på **skapa** toosubmit hello supportbegäran.</span><span class="sxs-lookup"><span data-stu-id="4b84f-142">Click **Create** toosubmit hello support request.</span></span>

## <a name="monitor-a-support-ticket"></a><span data-ttu-id="4b84f-143">Övervaka ett supportärende</span><span class="sxs-lookup"><span data-stu-id="4b84f-143">Monitor a support ticket</span></span>
<span data-ttu-id="4b84f-144">När du har skickat hello supportbegäran hello Azure-teamet kommer att kontakta dig.</span><span class="sxs-lookup"><span data-stu-id="4b84f-144">After you have submitted hello support request, hello Azure support team will contact you.</span></span> <span data-ttu-id="4b84f-145">toocheck din begärandestatus och information klickar du på **hantera supportärenden** på hello instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="4b84f-145">toocheck your request status and details, click **Manage support requests** on hello dashboard.</span></span>

![Kontrollera status](./media/sql-data-warehouse-get-started-create-support-ticket/check-status.png)

## <a name="other-resources"></a><span data-ttu-id="4b84f-147">Andra resurser</span><span class="sxs-lookup"><span data-stu-id="4b84f-147">Other Resources</span></span>
<span data-ttu-id="4b84f-148">Dessutom kan du ansluta med hello SQL Data Warehouse-communityn på [Stack Overflow] [ Stack Overflow] eller på hello [Azure SQL Data Warehouse MSDN-forum] [ Azure SQL Data Warehouse MSDN forum].</span><span class="sxs-lookup"><span data-stu-id="4b84f-148">Additionally, you can connect with hello SQL Data Warehouse community on [Stack Overflow][Stack Overflow] or on hello [Azure SQL Data Warehouse MSDN forum][Azure SQL Data Warehouse MSDN forum].</span></span>

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

