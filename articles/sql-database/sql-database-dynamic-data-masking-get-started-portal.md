---
title: 'Azure-portalen: SQL-databas dynamisk datamaskning | Microsoft Docs'
description: "Hur tooget igång med SQL Database dynamisk datamaskering i hello Azure-portalen"
services: sql-database
documentationcenter: 
author: ronitr
manager: jhubbard
editor: 
ms.assetid: "2"
ms.service: sql-database
ms.custom: security
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 11/22/2016
ms.author: ronitr; ronmat
ms.openlocfilehash: 5c5f74682a15d42eceb78c3ca0705401e1ac0ae7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-sql-database-dynamic-data-masking-with-hello-azure-portal"></a><span data-ttu-id="6de9b-103">Kom igång med SQL Database dynamisk datamaskering med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6de9b-103">Get started with SQL Database dynamic data masking with hello Azure Portal</span></span>

<span data-ttu-id="6de9b-104">Det här avsnittet beskrivs hur du tooimplement [dynamisk datamaskning](sql-database-dynamic-data-masking-get-started.md) med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6de9b-104">This topic shows you how tooimplement [dynamic data masking](sql-database-dynamic-data-masking-get-started.md) with hello Azure portal.</span></span> <span data-ttu-id="6de9b-105">Du kan också implementeras dynamiska datamaskering med [Azure SQL Database-cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx) eller hello [REST API](https://msdn.microsoft.com/library/dn505719.aspx).</span><span class="sxs-lookup"><span data-stu-id="6de9b-105">You can also implement dynamic data masking using [Azure SQL Database cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx) or hello [REST API](https://msdn.microsoft.com/library/dn505719.aspx).</span></span>


## <a name="set-up-dynamic-data-masking-for-your-database-using-hello-azure-portal"></a><span data-ttu-id="6de9b-106">Konfigurera dynamisk datamaskning för databasen med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6de9b-106">Set up dynamic data masking for your database using hello Azure Portal</span></span>
1. <span data-ttu-id="6de9b-107">Starta hello Azure-portalen på [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6de9b-107">Launch hello Azure Portal at [https://portal.azure.com](https://portal.azure.com).</span></span>
2. <span data-ttu-id="6de9b-108">Navigera toohello inställningsbladet för hello-databas som innehåller hello känsliga data som du vill toomask.</span><span class="sxs-lookup"><span data-stu-id="6de9b-108">Navigate toohello settings blade of hello database that includes hello sensitive data you want toomask.</span></span>
3. <span data-ttu-id="6de9b-109">Klicka på hello **dynamisk Datamaskering** panelen som startar hello **dynamisk Datamaskering** configuration-bladet.</span><span class="sxs-lookup"><span data-stu-id="6de9b-109">Click hello **Dynamic Data Masking** tile which launches hello **Dynamic Data Masking** configuration blade.</span></span>
   
   * <span data-ttu-id="6de9b-110">Du kan också bläddra ned toohello **Operations** avsnittet och klicka på **dynamisk Datamaskering**.</span><span class="sxs-lookup"><span data-stu-id="6de9b-110">Alternatively, you can scroll down toohello **Operations** section and click **Dynamic Data Masking**.</span></span>
     
     ![Navigeringsfönstret](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)<br/><br/>
4. <span data-ttu-id="6de9b-112">I hello **dynamisk Datamaskering** configuration bladet kan du se vissa databaskolumner som hello rekommendationer motorn har flaggats för maskering.</span><span class="sxs-lookup"><span data-stu-id="6de9b-112">In hello **Dynamic Data Masking** configuration blade you may see some database columns that hello recommendations engine has flagged for masking.</span></span> <span data-ttu-id="6de9b-113">Order tooaccept hello rekommendationer, klicka bara på **Lägg till Mask** för en eller flera kolumner och en mask skapas baserat på hello standardtypen för den här kolumnen.</span><span class="sxs-lookup"><span data-stu-id="6de9b-113">In order tooaccept hello recommendations, just click **Add Mask** for one or more columns and a mask will be created based on hello default type for this column.</span></span> <span data-ttu-id="6de9b-114">Du kan ändra hello maskering funktionen genom att klicka på hello maskningsregel och redigera hello maskering fältet format tooa annat valfritt format.</span><span class="sxs-lookup"><span data-stu-id="6de9b-114">You can change hello masking function by clicking on hello masking rule and editing hello masking field format tooa different format of your choice.</span></span> <span data-ttu-id="6de9b-115">Vara säker på att tooclick **spara** toosave dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="6de9b-115">Be sure tooclick **Save** toosave your settings.</span></span>
   
    ![Navigeringsfönstret](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)<br/><br/>
5. <span data-ttu-id="6de9b-117">tooadd en mask för alla kolumner i databasen, hello överst i hello **dynamisk Datamaskering** configuration bladet och klickar på **Lägg till Mask** tooopen hello **Lägg till regel för maskering** konfiguration av bladet</span><span class="sxs-lookup"><span data-stu-id="6de9b-117">tooadd a mask for any column in your database, at hello top of hello **Dynamic Data Masking** configuration blade click **Add Mask** tooopen hello **Add Masking Rule** configuration blade</span></span>
   
    ![Navigeringsfönstret](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)<br/><br/>
6. <span data-ttu-id="6de9b-119">Välj hello **schemat**, **tabell** och **kolumnen** toodefine hello anges fält som ska maskeras.</span><span class="sxs-lookup"><span data-stu-id="6de9b-119">Select hello **Schema**, **Table** and **Column** toodefine hello designated field that will be masked.</span></span>
7. <span data-ttu-id="6de9b-120">Välj en **maskering cellformatet** hello listan över känsliga data maskering kategorier.</span><span class="sxs-lookup"><span data-stu-id="6de9b-120">Choose a **Masking Field Format** from hello list of sensitive data masking categories.</span></span>
   
    ![Navigeringsfönstret](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)<br/><br/>        
8. <span data-ttu-id="6de9b-122">Klicka på **spara** i hello datamaskning bladet tooupdate hello regeluppsättning av maskering regler i hello dynamisk datamaskering princip.</span><span class="sxs-lookup"><span data-stu-id="6de9b-122">Click **Save** in hello data masking rule blade tooupdate hello set of masking rules in hello dynamic data masking policy.</span></span>
9. <span data-ttu-id="6de9b-123">Typen hello SQL-användare eller AAD identiteter som ska uteslutas från maskering och ha åtkomst toohello avmaskerad känsliga data.</span><span class="sxs-lookup"><span data-stu-id="6de9b-123">Type hello SQL users or AAD identities that should be excluded from masking, and have access toohello unmasked sensitive data.</span></span> <span data-ttu-id="6de9b-124">Detta bör vara en semikolonavgränsad lista med användare.</span><span class="sxs-lookup"><span data-stu-id="6de9b-124">This should be a semicolon-separated list of users.</span></span> <span data-ttu-id="6de9b-125">Observera att användare med administratörsbehörighet alltid har åtkomst till toohello ursprungliga omaskerat data.</span><span class="sxs-lookup"><span data-stu-id="6de9b-125">Note that users with administrator privileges always have access toohello original unmasked data.</span></span>
   
    ![Navigeringsfönstret](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)
   
   > [!TIP]
   > <span data-ttu-id="6de9b-127">toomake det så hello programnivå kan visa känsliga data för privilegierad användare, lägga till hello SQL-användaren eller AAD-identitet hello program använder tooquery hello databas.</span><span class="sxs-lookup"><span data-stu-id="6de9b-127">toomake it so hello application layer can display sensitive data for application privileged users, add hello SQL user or AAD identity hello application uses tooquery hello database.</span></span> <span data-ttu-id="6de9b-128">Vi rekommenderar starkt att den här listan innehåller ett minimalt antal Privilegierade användare toominimize exponering av hello känsliga data.</span><span class="sxs-lookup"><span data-stu-id="6de9b-128">It is highly recommended that this list contain a minimal number of privileged users toominimize exposure of hello sensitive data.</span></span>
   > 
   > 
10. <span data-ttu-id="6de9b-129">Klicka på **spara** i hello datamaskning bladet toosave hello nya eller uppdaterade maskering Konfigurationsprincip.</span><span class="sxs-lookup"><span data-stu-id="6de9b-129">Click **Save** in hello data masking configuration blade toosave hello new or updated masking policy.</span></span>


## <a name="next-steps"></a><span data-ttu-id="6de9b-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6de9b-130">Next steps</span></span>

* <span data-ttu-id="6de9b-131">En översikt över dynamisk datamaskning finns [dynamisk datamaskning](sql-database-dynamic-data-masking-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="6de9b-131">For an overview of dynamic data masking, see [dynamic data masking](sql-database-dynamic-data-masking-get-started.md).</span></span>
* <span data-ttu-id="6de9b-132">Du kan också implementeras dynamiska datamaskering med [Azure SQL Database-cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx) eller hello [REST API](https://msdn.microsoft.com/library/dn505719.aspx).</span><span class="sxs-lookup"><span data-stu-id="6de9b-132">You can also implement dynamic data masking using [Azure SQL Database cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx) or hello [REST API](https://msdn.microsoft.com/library/dn505719.aspx).</span></span>
