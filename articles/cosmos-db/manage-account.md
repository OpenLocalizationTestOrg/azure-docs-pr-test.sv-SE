---
title: Hantera en Azure DB som Cosmos-konto via Azure Portal | Microsoft Docs
description: "Lär dig hur du hanterar Azure DB som Cosmos-konto via Azure Portal. Hitta en vägledning om hur du använder Azure-portalen visa, kopiera, ta bort och ha åtkomst till konton."
keywords: Azure-portalen documentdb, azure, Microsoft azure
services: cosmos-db
documentationcenter: 
author: kirillg
manager: jhubbard
editor: cgronlun
ms.assetid: 00fc172f-f86c-44ca-8336-11998dcab45c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: kirillg
ms.openlocfilehash: a0c6ec8d490e1adacc96758971ab91d8eaeab45c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-an-azure-cosmos-db-account"></a><span data-ttu-id="f895d-105">Så här hanterar du ett konto i Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="f895d-105">How to manage an Azure Cosmos DB account</span></span>
<span data-ttu-id="f895d-106">Lär dig mer om att ställa in globalt konsekvensfel, fungerar med nycklar och ta bort ett Azure DB som Cosmos-konto i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f895d-106">Learn how to set global consistency, work with keys, and delete an Azure Cosmos DB account in the Azure portal.</span></span>

## <span data-ttu-id="f895d-107"><a id="consistency"></a>Hantera Azure Cosmos DB konsekvenskontroll inställningar</span><span class="sxs-lookup"><span data-stu-id="f895d-107"><a id="consistency"></a>Manage Azure Cosmos DB consistency settings</span></span>
<span data-ttu-id="f895d-108">Att välja rätt konsekvensnivå beror på semantiken för ditt program.</span><span class="sxs-lookup"><span data-stu-id="f895d-108">Selecting the right consistency level depends on the semantics of your application.</span></span> <span data-ttu-id="f895d-109">Du bör du bekanta dig med tillgängliga konsekvensnivåer i Azure Cosmos-databasen genom att läsa [maximerar tillgänglighet och prestanda i Azure Cosmos-databasen med hjälp av konsekvensnivåer][consistency].</span><span class="sxs-lookup"><span data-stu-id="f895d-109">You should familiarize yourself with the available consistency levels in Azure Cosmos DB by reading [Using consistency levels to maximize availability and performance in Azure Cosmos DB][consistency].</span></span> <span data-ttu-id="f895d-110">Azure Cosmos-DB ger konsekvens, tillgänglighet och prestanda garantier, på alla nivåer för konsekvenskontroll som är tillgängliga för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="f895d-110">Azure Cosmos DB provides consistency, availability, and performance guarantees, at every consistency level available for your database account.</span></span> <span data-ttu-id="f895d-111">Konfigurera ditt konto med en konsekvenskontroll nivå av starka kräver att data är begränsat till en enda Azure region och globalt inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="f895d-111">Configuring your database account with a consistency level of Strong requires that your data is confined to a single Azure region and not globally available.</span></span> <span data-ttu-id="f895d-112">Å andra sidan, Avslappnad konsekvensnivåer - avgränsas föråldrad, session eller eventuell aktivera du associera valfritt antal Azure-regioner med ditt konto.</span><span class="sxs-lookup"><span data-stu-id="f895d-112">On the other hand, the relaxed consistency levels - bounded staleness, session or eventual enable you to associate any number of Azure regions with your database account.</span></span> <span data-ttu-id="f895d-113">Följande enkla steg visar hur du väljer standardnivån för konsekvens för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="f895d-113">The following simple steps show you how to select the default consistency level for your database account.</span></span> 

### <a name="to-specify-the-default-consistency-for-an-azure-cosmos-db-account"></a><span data-ttu-id="f895d-114">Ange standardkonsekvensen för ett konto i Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="f895d-114">To specify the default consistency for an Azure Cosmos DB account</span></span>
1. <span data-ttu-id="f895d-115">I den [Azure-portalen](https://portal.azure.com/), komma åt ditt konto i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f895d-115">In the [Azure portal](https://portal.azure.com/), access your Azure Cosmos DB account.</span></span>
2. <span data-ttu-id="f895d-116">I bladet konto klickar du på **standard konsekvenskontroll**.</span><span class="sxs-lookup"><span data-stu-id="f895d-116">In the account blade, click **Default consistency**.</span></span>
3. <span data-ttu-id="f895d-117">I den **standard konsekvenskontroll** bladet välj ny konsekvensnivå och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="f895d-117">In the **Default Consistency** blade, select the new consistency level and click **Save**.</span></span>
    <span data-ttu-id="f895d-118">![Standard konsekvenskontroll session][5]</span><span class="sxs-lookup"><span data-stu-id="f895d-118">![Default consistency session][5]</span></span>

## <span data-ttu-id="f895d-119"><a id="keys"></a>Visa, kopiera och återskapa åtkomstnycklar</span><span class="sxs-lookup"><span data-stu-id="f895d-119"><a id="keys"></a>View, copy, and regenerate access keys</span></span>
<span data-ttu-id="f895d-120">När du skapar ett konto i Azure Cosmos DB genererar tjänsten två master åtkomstnycklar som kan användas för autentisering vid åtkomst av Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="f895d-120">When you create an Azure Cosmos DB account, the service generates two master access keys that can be used for authentication when the Azure Cosmos DB account is accessed.</span></span> <span data-ttu-id="f895d-121">Genom att tillhandahålla två åtkomstnycklar kan Azure Cosmos DB du återskapa nycklarna utan avbrott i Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="f895d-121">By providing two access keys, Azure Cosmos DB enables you to regenerate the keys with no interruption to your Azure Cosmos DB account.</span></span> 

<span data-ttu-id="f895d-122">I den [Azure-portalen](https://portal.azure.com/), åtkomst av **nycklar** bladet på menyn resurs på den **Azure Cosmos DB konto** bladet för att visa, kopiera och återskapa åtkomstnycklar som används för att komma åt ditt konto i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f895d-122">In the [Azure portal](https://portal.azure.com/), access the **Keys** blade from the resource menu on the **Azure Cosmos DB account** blade to view, copy, and regenerate the access keys that are used to access your Azure Cosmos DB account.</span></span>

![Skärmbild av Azure Portal, bladet nycklar](./media/manage-account/keys.png)

> [!NOTE]
> <span data-ttu-id="f895d-124">Den **nycklar** bladet innehåller också primära och sekundära anslutningssträngar som kan användas för att ansluta till ditt konto från den [Datamigreringsverktyg](import-data.md).</span><span class="sxs-lookup"><span data-stu-id="f895d-124">The **Keys** blade also includes primary and secondary connection strings that can be used to connect to your account from the [Data Migration Tool](import-data.md).</span></span>
> 
> 

<span data-ttu-id="f895d-125">Skrivskyddade nycklar är också tillgängliga på det här bladet.</span><span class="sxs-lookup"><span data-stu-id="f895d-125">Read-only keys are also available on this blade.</span></span> <span data-ttu-id="f895d-126">Läsning och frågor är skrivskyddade åtgärder stund skapar, tar bort, inte och ersätter.</span><span class="sxs-lookup"><span data-stu-id="f895d-126">Reads and queries are read-only operations, while creates, deletes, and replaces are not.</span></span>

### <a name="copy-an-access-key-in-the-azure-portal"></a><span data-ttu-id="f895d-127">Kopiera en snabbtangent i Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f895d-127">Copy an access key in the Azure Portal</span></span>
<span data-ttu-id="f895d-128">På den **nycklar** bladet, klickar du på den **kopiera** knappen till höger om den nyckel som du vill kopiera.</span><span class="sxs-lookup"><span data-stu-id="f895d-128">On the **Keys** blade, click the **Copy** button to the right of the key you wish to copy.</span></span>

![Visa och kopiera snabbtangent i bladet nycklar i Azure-portalen](./media/manage-account/copykeys.png)

### <a name="regenerate-access-keys"></a><span data-ttu-id="f895d-130">Återskapa åtkomstnycklar</span><span class="sxs-lookup"><span data-stu-id="f895d-130">Regenerate access keys</span></span>
<span data-ttu-id="f895d-131">Du bör ändra åtkomstnycklarna till ditt Azure DB som Cosmos-konto med jämna mellanrum för att skydda dina anslutningar.</span><span class="sxs-lookup"><span data-stu-id="f895d-131">You should change the access keys to your Azure Cosmos DB account periodically to help keep your connections more secure.</span></span> <span data-ttu-id="f895d-132">Två åtkomstnycklar tilldelas så att du kan upprätthålla anslutningar till Azure Cosmos DB kontot den ena åtkomstnyckeln medan du återskapar den andra åtkomstnyckeln.</span><span class="sxs-lookup"><span data-stu-id="f895d-132">Two access keys are assigned to enable you to maintain connections to the Azure Cosmos DB account using one access key while you regenerate the other access key.</span></span>

> [!WARNING]
> <span data-ttu-id="f895d-133">Återskapar åtkomstnycklarna påverkar alla program som är beroende av den aktuella nyckeln.</span><span class="sxs-lookup"><span data-stu-id="f895d-133">Regenerating your access keys affects any applications that are dependent on the current key.</span></span> <span data-ttu-id="f895d-134">Alla klienter som använder snabbtangenten för att få åtkomst till Azure DB som Cosmos-kontot måste uppdateras för att använda den nya nyckeln.</span><span class="sxs-lookup"><span data-stu-id="f895d-134">All clients that use the access key to access the Azure Cosmos DB account must be updated to use the new key.</span></span>
> 
> 

<span data-ttu-id="f895d-135">Om du har program eller molntjänster med hjälp av Azure DB som Cosmos-konto kan förlorar du anslutningar om du återskapa nycklar, såvida inte du lanserar dina nycklar.</span><span class="sxs-lookup"><span data-stu-id="f895d-135">If you have applications or cloud services using the Azure Cosmos DB account, you will lose the connections if you regenerate keys, unless you roll your keys.</span></span> <span data-ttu-id="f895d-136">Följande steg beskriver processen ingår i rullande dina nycklar.</span><span class="sxs-lookup"><span data-stu-id="f895d-136">The following steps outline the process involved in rolling your keys.</span></span>

1. <span data-ttu-id="f895d-137">Uppdatera åtkomstnyckeln i din programkod för att referera till den sekundära åtkomstnyckeln för kontot Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f895d-137">Update the access key in your application code to reference the secondary access key of the Azure Cosmos DB account.</span></span>
2. <span data-ttu-id="f895d-138">Återskapa den primära åtkomstnyckeln för ditt Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="f895d-138">Regenerate the primary access key for your Azure Cosmos DB account.</span></span> <span data-ttu-id="f895d-139">I den [Azure Portal](https://portal.azure.com/), komma åt ditt konto i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f895d-139">In the [Azure Portal](https://portal.azure.com/), access your Azure Cosmos DB account.</span></span>
3. <span data-ttu-id="f895d-140">I den **Azure Cosmos-DB konto** bladet, klickar du på **nycklar**.</span><span class="sxs-lookup"><span data-stu-id="f895d-140">In the **Azure Cosmos DB Account** blade, click **Keys**.</span></span>
4. <span data-ttu-id="f895d-141">På den **nycklar** bladet, klickar du på knappen Generera, och klicka sedan på **Ok** att bekräfta att du vill skapa en ny nyckel.</span><span class="sxs-lookup"><span data-stu-id="f895d-141">On the **Keys** blade, click the regenerate button, then click **Ok** to confirm that you want to generate a new key.</span></span>
    <span data-ttu-id="f895d-142">![Återskapa åtkomstnycklar](./media/manage-account/regenerate-keys.png)</span><span class="sxs-lookup"><span data-stu-id="f895d-142">![Regenerate access keys](./media/manage-account/regenerate-keys.png)</span></span>
5. <span data-ttu-id="f895d-143">När du har verifierat att den nya nyckeln är tillgänglig för användning (cirka 5 minuter efter återskapandet), uppdatera åtkomstnyckeln i din programkod för att referera till den nya primärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="f895d-143">Once you have verified that the new key is available for use (approximately 5 minutes after regeneration), update the access key in your application code to reference the new primary access key.</span></span>
6. <span data-ttu-id="f895d-144">Återskapa den sekundära åtkomstnyckeln.</span><span class="sxs-lookup"><span data-stu-id="f895d-144">Regenerate the secondary access key.</span></span>
   
    ![Återskapa åtkomstnycklar](./media/manage-account/regenerate-secondary-key.png)

> [!NOTE]
> <span data-ttu-id="f895d-146">Det kan ta flera minuter innan en nyligen skapade nyckeln kan användas för att komma åt Azure DB som Cosmos-kontot.</span><span class="sxs-lookup"><span data-stu-id="f895d-146">It can take several minutes before a newly generated key can be used to access your Azure Cosmos DB account.</span></span>
> 
> 

## <a name="get-the--connection-string"></a><span data-ttu-id="f895d-147">Hämta anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="f895d-147">Get the  connection string</span></span>
<span data-ttu-id="f895d-148">Hämta din anslutningssträng genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="f895d-148">To retrieve your connection string, do the following:</span></span> 

1. <span data-ttu-id="f895d-149">I den [Azure-portalen](https://portal.azure.com), komma åt ditt konto i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f895d-149">In the [Azure portal](https://portal.azure.com), access your Azure Cosmos DB account.</span></span>
2. <span data-ttu-id="f895d-150">Resurs-menyn klickar du på **nycklar**.</span><span class="sxs-lookup"><span data-stu-id="f895d-150">In the resource menu, click **Keys**.</span></span>
3. <span data-ttu-id="f895d-151">Klicka på den **kopiera** knappen bredvid den **primära anslutningssträngen** eller **sekundära anslutningssträngen** rutan.</span><span class="sxs-lookup"><span data-stu-id="f895d-151">Click the **Copy** button next to the **Primary Connection String** or **Secondary Connection String** box.</span></span> 

<span data-ttu-id="f895d-152">Om du använder anslutningssträngen i den [Migreringsverktyget för Azure Cosmos DB databasen](import-data.md), Lägg till namnet på databasen i slutet av anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="f895d-152">If you are using the connection string in the [Azure Cosmos DB Database Migration Tool](import-data.md), append the database name to the end of the connection string.</span></span> <span data-ttu-id="f895d-153">`AccountEndpoint=< >;AccountKey=< >;Database=< >`.</span><span class="sxs-lookup"><span data-stu-id="f895d-153">`AccountEndpoint=< >;AccountKey=< >;Database=< >`.</span></span>

## <span data-ttu-id="f895d-154"><a id="delete"></a>Ta bort ett Azure DB som Cosmos-konto</span><span class="sxs-lookup"><span data-stu-id="f895d-154"><a id="delete"></a> Delete an Azure Cosmos DB account</span></span>
<span data-ttu-id="f895d-155">Om du vill ta bort ett Azure DB som Cosmos-konto från Azure-portalen som du inte längre använder högerklickar du på namnet på kontot och klicka på **ta bort kontot**.</span><span class="sxs-lookup"><span data-stu-id="f895d-155">To remove an Azure Cosmos DB account from the Azure Portal that you are no longer using, right-click the account name, and click **Delete account**.</span></span>

![Hur du tar bort ett Azure DB som Cosmos-konto i Azure Portal](./media/manage-account/deleteaccount.png)

1. <span data-ttu-id="f895d-157">I den [Azure-portalen](https://portal.azure.com/), få åtkomst till Azure DB som Cosmos-kontot som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="f895d-157">In the [Azure portal](https://portal.azure.com/), access the Azure Cosmos DB account you wish to delete.</span></span>
2. <span data-ttu-id="f895d-158">På den **Azure Cosmos DB konto** bladet högerklickar du på kontot och klicka sedan på **ta bort konto**.</span><span class="sxs-lookup"><span data-stu-id="f895d-158">On the **Azure Cosmos DB account** blade, right-click the account, and then click **Delete Account**.</span></span> 
3. <span data-ttu-id="f895d-159">Skriv namnet på Azure Cosmos DB och bekräfta att du vill ta bort kontot på bladet resulterande bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="f895d-159">On the resulting confirmation blade, type the Azure Cosmos DB account name to confirm that you want to delete the account.</span></span>
4. <span data-ttu-id="f895d-160">Klicka på den **ta bort** knappen.</span><span class="sxs-lookup"><span data-stu-id="f895d-160">Click the **Delete** button.</span></span>

![Hur du tar bort ett Azure DB som Cosmos-konto i Azure Portal](./media/manage-account/delete-account-confirm.png)

## <span data-ttu-id="f895d-162"><a id="next"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f895d-162"><a id="next"></a>Next steps</span></span>
<span data-ttu-id="f895d-163">Lär dig hur du [komma igång med ditt konto i Azure Cosmos DB](http://go.microsoft.com/fwlink/p/?LinkId=402364).</span><span class="sxs-lookup"><span data-stu-id="f895d-163">Learn how to [get started with your Azure Cosmos DB account](http://go.microsoft.com/fwlink/p/?LinkId=402364).</span></span>

<!--Image references-->
[5]: ./media/manage-account/documentdb_change_consistency-1.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: consistency-levels.md
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/cosmos-db/
