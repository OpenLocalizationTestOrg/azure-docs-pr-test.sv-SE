---
title: aaaManage ett Azure DB som Cosmos-konto via hello Azure Portal | Microsoft Docs
description: "Lär dig hur toomanage Azure Cosmos-DB-kontot via hello Azure-portalen. Hitta en vägledning om hur du använder hello Azure Portal tooview, kopiera, ta bort och åtkomst."
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
ms.openlocfilehash: 77ad953cf558a519674be08ad913a12202f69496
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-an-azure-cosmos-db-account"></a><span data-ttu-id="9bf03-105">Hur toomanage ett Azure DB som Cosmos-konto</span><span class="sxs-lookup"><span data-stu-id="9bf03-105">How toomanage an Azure Cosmos DB account</span></span>
<span data-ttu-id="9bf03-106">Lär dig hur tooset globalt konsekvensfel, arbeta med nycklar och ta bort ett Azure DB som Cosmos-konto i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9bf03-106">Learn how tooset global consistency, work with keys, and delete an Azure Cosmos DB account in hello Azure portal.</span></span>

## <span data-ttu-id="9bf03-107"><a id="consistency"></a>Hantera Azure Cosmos DB konsekvenskontroll inställningar</span><span class="sxs-lookup"><span data-stu-id="9bf03-107"><a id="consistency"></a>Manage Azure Cosmos DB consistency settings</span></span>
<span data-ttu-id="9bf03-108">Att välja rätt konsekvensnivå för hello beror på hello semantiken för ditt program.</span><span class="sxs-lookup"><span data-stu-id="9bf03-108">Selecting hello right consistency level depends on hello semantics of your application.</span></span> <span data-ttu-id="9bf03-109">Du bör du bekanta dig med hello tillgängliga konsekvensnivåer i Azure Cosmos-databasen genom att läsa [med konsekvenskontroll nivåer toomaximize tillgänglighet och prestanda i Azure Cosmos DB][consistency].</span><span class="sxs-lookup"><span data-stu-id="9bf03-109">You should familiarize yourself with hello available consistency levels in Azure Cosmos DB by reading [Using consistency levels toomaximize availability and performance in Azure Cosmos DB][consistency].</span></span> <span data-ttu-id="9bf03-110">Azure Cosmos-DB ger konsekvens, tillgänglighet och prestanda garantier, på alla nivåer för konsekvenskontroll som är tillgängliga för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="9bf03-110">Azure Cosmos DB provides consistency, availability, and performance guarantees, at every consistency level available for your database account.</span></span> <span data-ttu-id="9bf03-111">Konfigurera ditt konto med en konsekvenskontroll nivå av starka kräver att dina data är begränsat tooa enda Azure-region och globalt inte tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="9bf03-111">Configuring your database account with a consistency level of Strong requires that your data is confined tooa single Azure region and not globally available.</span></span> <span data-ttu-id="9bf03-112">På andra sidan hello, hello Avslappnad konsekvensnivåer - avgränsas föråldrad, session eller eventuell aktivera du tooassociate valfritt antal Azure-regioner med ditt konto.</span><span class="sxs-lookup"><span data-stu-id="9bf03-112">On hello other hand, hello relaxed consistency levels - bounded staleness, session or eventual enable you tooassociate any number of Azure regions with your database account.</span></span> <span data-ttu-id="9bf03-113">hello följande enkla steg visar hur tooselect hello standardnivån för konsekvens för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="9bf03-113">hello following simple steps show you how tooselect hello default consistency level for your database account.</span></span> 

### <a name="toospecify-hello-default-consistency-for-an-azure-cosmos-db-account"></a><span data-ttu-id="9bf03-114">toospecify hello standardkonsekvensen för ett konto i Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="9bf03-114">toospecify hello default consistency for an Azure Cosmos DB account</span></span>
1. <span data-ttu-id="9bf03-115">I hello [Azure-portalen](https://portal.azure.com/), komma åt ditt konto i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9bf03-115">In hello [Azure portal](https://portal.azure.com/), access your Azure Cosmos DB account.</span></span>
2. <span data-ttu-id="9bf03-116">I hello-kontoblad klickar du på **standard konsekvenskontroll**.</span><span class="sxs-lookup"><span data-stu-id="9bf03-116">In hello account blade, click **Default consistency**.</span></span>
3. <span data-ttu-id="9bf03-117">I hello **standard konsekvenskontroll** bladet, Välj hello nya konsekvensnivå och klickar på **spara**.</span><span class="sxs-lookup"><span data-stu-id="9bf03-117">In hello **Default Consistency** blade, select hello new consistency level and click **Save**.</span></span>
    <span data-ttu-id="9bf03-118">![Standard konsekvenskontroll session][5]</span><span class="sxs-lookup"><span data-stu-id="9bf03-118">![Default consistency session][5]</span></span>

## <span data-ttu-id="9bf03-119"><a id="keys"></a>Visa, kopiera och återskapa åtkomstnycklar</span><span class="sxs-lookup"><span data-stu-id="9bf03-119"><a id="keys"></a>View, copy, and regenerate access keys</span></span>
<span data-ttu-id="9bf03-120">När du skapar ett konto i Azure Cosmos DB genererar hello tjänsten två master åtkomstnycklar som kan användas för autentisering när hello Azure Cosmos DB konto används.</span><span class="sxs-lookup"><span data-stu-id="9bf03-120">When you create an Azure Cosmos DB account, hello service generates two master access keys that can be used for authentication when hello Azure Cosmos DB account is accessed.</span></span> <span data-ttu-id="9bf03-121">Genom att tillhandahålla två åtkomstnycklar kan Azure Cosmos DB du tooregenerate hello nycklarna utan avbrott tooyour Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="9bf03-121">By providing two access keys, Azure Cosmos DB enables you tooregenerate hello keys with no interruption tooyour Azure Cosmos DB account.</span></span> 

<span data-ttu-id="9bf03-122">I hello [Azure-portalen](https://portal.azure.com/), åtkomst hello **nycklar** bladet hello resurs-menyn på hello **Azure Cosmos DB konto** bladet tooview, kopiera och generera hello åtkomstnycklar som är används tooaccess Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="9bf03-122">In hello [Azure portal](https://portal.azure.com/), access hello **Keys** blade from hello resource menu on hello **Azure Cosmos DB account** blade tooview, copy, and regenerate hello access keys that are used tooaccess your Azure Cosmos DB account.</span></span>

![Skärmbild av Azure Portal, bladet nycklar](./media/manage-account/keys.png)

> [!NOTE]
> <span data-ttu-id="9bf03-124">Hej **nycklar** bladet innehåller också primära och sekundära anslutningssträngar som kan använda tooconnect tooyour konto från hello [Datamigreringsverktyg](import-data.md).</span><span class="sxs-lookup"><span data-stu-id="9bf03-124">hello **Keys** blade also includes primary and secondary connection strings that can be used tooconnect tooyour account from hello [Data Migration Tool](import-data.md).</span></span>
> 
> 

<span data-ttu-id="9bf03-125">Skrivskyddade nycklar är också tillgängliga på det här bladet.</span><span class="sxs-lookup"><span data-stu-id="9bf03-125">Read-only keys are also available on this blade.</span></span> <span data-ttu-id="9bf03-126">Läsning och frågor är skrivskyddade åtgärder stund skapar, tar bort, inte och ersätter.</span><span class="sxs-lookup"><span data-stu-id="9bf03-126">Reads and queries are read-only operations, while creates, deletes, and replaces are not.</span></span>

### <a name="copy-an-access-key-in-hello-azure-portal"></a><span data-ttu-id="9bf03-127">Kopiera en snabbtangent i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9bf03-127">Copy an access key in hello Azure Portal</span></span>
<span data-ttu-id="9bf03-128">På hello **nycklar** bladet, klickar du på hello **kopiera** knappen toohello höger i hello nyckel som du vill att toocopy.</span><span class="sxs-lookup"><span data-stu-id="9bf03-128">On hello **Keys** blade, click hello **Copy** button toohello right of hello key you wish toocopy.</span></span>

![Visa och kopiera snabbtangent i hello bladet nycklar i Azure Portal](./media/manage-account/copykeys.png)

### <a name="regenerate-access-keys"></a><span data-ttu-id="9bf03-130">Återskapa åtkomstnycklar</span><span class="sxs-lookup"><span data-stu-id="9bf03-130">Regenerate access keys</span></span>
<span data-ttu-id="9bf03-131">Du bör ändra hello nycklar tooyour Azure Cosmos DB åtkomstkonto regelbundet toohelp skydda dina anslutningar.</span><span class="sxs-lookup"><span data-stu-id="9bf03-131">You should change hello access keys tooyour Azure Cosmos DB account periodically toohelp keep your connections more secure.</span></span> <span data-ttu-id="9bf03-132">Två åtkomstnycklar tilldelas tooenable du toomaintain anslutningar toohello Azure DB som Cosmos-konto med hjälp av ena åtkomstnyckeln medan du återskapar hello andra snabbtangent.</span><span class="sxs-lookup"><span data-stu-id="9bf03-132">Two access keys are assigned tooenable you toomaintain connections toohello Azure Cosmos DB account using one access key while you regenerate hello other access key.</span></span>

> [!WARNING]
> <span data-ttu-id="9bf03-133">Återskapar åtkomstnycklarna påverkar alla program som är beroende av hello aktuella nyckeln.</span><span class="sxs-lookup"><span data-stu-id="9bf03-133">Regenerating your access keys affects any applications that are dependent on hello current key.</span></span> <span data-ttu-id="9bf03-134">Alla klienter som använder hello viktiga tooaccess hello Azure Cosmos DB åtkomstkonto måste vara uppdaterade toouse hello ny nyckel.</span><span class="sxs-lookup"><span data-stu-id="9bf03-134">All clients that use hello access key tooaccess hello Azure Cosmos DB account must be updated toouse hello new key.</span></span>
> 
> 

<span data-ttu-id="9bf03-135">Om du har program eller molntjänster med hjälp av hello Azure Cosmos DB konto förlorar du hello anslutningar om du återskapa nycklar, såvida inte du lanserar dina nycklar.</span><span class="sxs-lookup"><span data-stu-id="9bf03-135">If you have applications or cloud services using hello Azure Cosmos DB account, you will lose hello connections if you regenerate keys, unless you roll your keys.</span></span> <span data-ttu-id="9bf03-136">hello beskriver följande steg hello processen ingår i rullande dina nycklar.</span><span class="sxs-lookup"><span data-stu-id="9bf03-136">hello following steps outline hello process involved in rolling your keys.</span></span>

1. <span data-ttu-id="9bf03-137">Uppdatera hello snabbtangent i ditt program kod tooreference hello sekundära åtkomstnyckeln för hello Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="9bf03-137">Update hello access key in your application code tooreference hello secondary access key of hello Azure Cosmos DB account.</span></span>
2. <span data-ttu-id="9bf03-138">Återskapa hello primära åtkomstnyckeln för ditt Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="9bf03-138">Regenerate hello primary access key for your Azure Cosmos DB account.</span></span> <span data-ttu-id="9bf03-139">I hello [Azure Portal](https://portal.azure.com/), komma åt ditt konto i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9bf03-139">In hello [Azure Portal](https://portal.azure.com/), access your Azure Cosmos DB account.</span></span>
3. <span data-ttu-id="9bf03-140">I hello **Azure Cosmos-DB konto** bladet, klickar du på **nycklar**.</span><span class="sxs-lookup"><span data-stu-id="9bf03-140">In hello **Azure Cosmos DB Account** blade, click **Keys**.</span></span>
4. <span data-ttu-id="9bf03-141">På hello **nycklar** bladet hello generera knappen Klicka **Ok** tooconfirm som du vill toogenerate en ny nyckel.</span><span class="sxs-lookup"><span data-stu-id="9bf03-141">On hello **Keys** blade, click hello regenerate button, then click **Ok** tooconfirm that you want toogenerate a new key.</span></span>
    <span data-ttu-id="9bf03-142">![Återskapa åtkomstnycklar](./media/manage-account/regenerate-keys.png)</span><span class="sxs-lookup"><span data-stu-id="9bf03-142">![Regenerate access keys](./media/manage-account/regenerate-keys.png)</span></span>
5. <span data-ttu-id="9bf03-143">När du har verifierat hello nya nyckeln är tillgänglig för användning uppdatera (cirka 5 minuter efter återskapandet), hello snabbtangent i ditt program kod tooreference hello nya primärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="9bf03-143">Once you have verified that hello new key is available for use (approximately 5 minutes after regeneration), update hello access key in your application code tooreference hello new primary access key.</span></span>
6. <span data-ttu-id="9bf03-144">Återskapa hello sekundära åtkomstnyckeln.</span><span class="sxs-lookup"><span data-stu-id="9bf03-144">Regenerate hello secondary access key.</span></span>
   
    ![Återskapa åtkomstnycklar](./media/manage-account/regenerate-secondary-key.png)

> [!NOTE]
> <span data-ttu-id="9bf03-146">Det kan ta flera minuter innan en nyligen skapade nyckel kan använda tooaccess Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="9bf03-146">It can take several minutes before a newly generated key can be used tooaccess your Azure Cosmos DB account.</span></span>
> 
> 

## <a name="get-hello--connection-string"></a><span data-ttu-id="9bf03-147">Hämta hello anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="9bf03-147">Get hello  connection string</span></span>
<span data-ttu-id="9bf03-148">tooretrieve anslutningen sträng, hello följande:</span><span class="sxs-lookup"><span data-stu-id="9bf03-148">tooretrieve your connection string, do hello following:</span></span> 

1. <span data-ttu-id="9bf03-149">I hello [Azure-portalen](https://portal.azure.com), komma åt ditt konto i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9bf03-149">In hello [Azure portal](https://portal.azure.com), access your Azure Cosmos DB account.</span></span>
2. <span data-ttu-id="9bf03-150">Hello resurs menyn klickar du på **nycklar**.</span><span class="sxs-lookup"><span data-stu-id="9bf03-150">In hello resource menu, click **Keys**.</span></span>
3. <span data-ttu-id="9bf03-151">Klicka på hello **kopiera** knappen Nästa toohello **primära anslutningssträngen** eller **sekundära anslutningssträngen** rutan.</span><span class="sxs-lookup"><span data-stu-id="9bf03-151">Click hello **Copy** button next toohello **Primary Connection String** or **Secondary Connection String** box.</span></span> 

<span data-ttu-id="9bf03-152">Om du använder hello anslutningssträngen i hello [Migreringsverktyget för Azure Cosmos DB databasen](import-data.md), Lägg till hello databas namnet toohello slutet av hello anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="9bf03-152">If you are using hello connection string in hello [Azure Cosmos DB Database Migration Tool](import-data.md), append hello database name toohello end of hello connection string.</span></span> <span data-ttu-id="9bf03-153">`AccountEndpoint=< >;AccountKey=< >;Database=< >`.</span><span class="sxs-lookup"><span data-stu-id="9bf03-153">`AccountEndpoint=< >;AccountKey=< >;Database=< >`.</span></span>

## <span data-ttu-id="9bf03-154"><a id="delete"></a>Ta bort ett Azure DB som Cosmos-konto</span><span class="sxs-lookup"><span data-stu-id="9bf03-154"><a id="delete"></a> Delete an Azure Cosmos DB account</span></span>
<span data-ttu-id="9bf03-155">tooremove en Azure-Cosmos-DB kontot från hello Azure-portalen som du inte längre använder, högerklicka på hello kontonamn och klicka på **ta bort kontot**.</span><span class="sxs-lookup"><span data-stu-id="9bf03-155">tooremove an Azure Cosmos DB account from hello Azure Portal that you are no longer using, right-click hello account name, and click **Delete account**.</span></span>

![Hur toodelete en Cosmos-databas med Azure-konto i hello Azure-portalen](./media/manage-account/deleteaccount.png)

1. <span data-ttu-id="9bf03-157">I hello [Azure-portalen](https://portal.azure.com/), komma åt hello Azure Cosmos DB-konto som du vill toodelete.</span><span class="sxs-lookup"><span data-stu-id="9bf03-157">In hello [Azure portal](https://portal.azure.com/), access hello Azure Cosmos DB account you wish toodelete.</span></span>
2. <span data-ttu-id="9bf03-158">På hello **Azure Cosmos DB konto** bladet, högerklicka på hello-kontot och klicka sedan på **ta bort konto**.</span><span class="sxs-lookup"><span data-stu-id="9bf03-158">On hello **Azure Cosmos DB account** blade, right-click hello account, and then click **Delete Account**.</span></span> 
3. <span data-ttu-id="9bf03-159">Ange hello Azure Cosmos DB konto namnet tooconfirm som du vill toodelete hello konto på hello resulterande bekräftelse-bladet.</span><span class="sxs-lookup"><span data-stu-id="9bf03-159">On hello resulting confirmation blade, type hello Azure Cosmos DB account name tooconfirm that you want toodelete hello account.</span></span>
4. <span data-ttu-id="9bf03-160">Klicka på hello **ta bort** knappen.</span><span class="sxs-lookup"><span data-stu-id="9bf03-160">Click hello **Delete** button.</span></span>

![Hur toodelete en Cosmos-databas med Azure-konto i hello Azure-portalen](./media/manage-account/delete-account-confirm.png)

## <span data-ttu-id="9bf03-162"><a id="next"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9bf03-162"><a id="next"></a>Next steps</span></span>
<span data-ttu-id="9bf03-163">Lär dig hur för[komma igång med ditt konto i Azure Cosmos DB](http://go.microsoft.com/fwlink/p/?LinkId=402364).</span><span class="sxs-lookup"><span data-stu-id="9bf03-163">Learn how too[get started with your Azure Cosmos DB account](http://go.microsoft.com/fwlink/p/?LinkId=402364).</span></span>

<!--Image references-->
[5]: ./media/manage-account/documentdb_change_consistency-1.png

<!--Reference style links - using these makes hello source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: consistency-levels.md
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/cosmos-db/
