---
title: "aaaCreate en Azure Search-tjänst i hello portal | Microsoft Docs"
description: "Etablera en Azure Search-tjänst i hello-portalen."
services: search
manager: jhubbard
author: HeidiSteen
documentationcenter: 
ms.assetid: c8c88922-69aa-4099-b817-60f7b54e62df
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: heidist
ms.openlocfilehash: f1c7197a1467e32bd4b360b165c9059e6bb0e496
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-search-service-in-hello-portal"></a>Skapa en Azure Search-tjänst i hello-portalen

Den här artikeln förklarar hur toocreate eller etablera ett Azure Search-tjänst i hello-portalen. PowerShell instruktioner finns i [hantera Azure Search med PowerShell](search-manage-powershell.md).

## <a name="subscribe-free-or-paid"></a>Prenumerera (ledigt eller betald)

[Öppna ett kostnadsfritt Azure-konto](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) och använda kostnadsfria krediter tootry ut betald Azure-tjänster. Efter att krediten är slut, hålla hello konto och fortsätta toouse kostnadsfria Azure-tjänster, till exempel Websites. Ditt kreditkort debiteras aldrig såvida du inte uttryckligen ändrar dina inställningar och be toobe debiteras.

Du kan också [aktivera MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). En MSDN-prenumeration ger dig krediter varje månad som du kan använda för Azure-betaltjänster. 

## <a name="find-azure-search"></a>Hitta Azure Search
1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Klicka hello plustecken (”+”) i hello övre vänstra hörnet.
3. Välj **webb + mobilt** > **Azure Search**.

![](./media/search-create-service-portal/find-search2.png)

## <a name="name-hello-service-and-url-endpoint"></a>Namnet hello-tjänsten och URL-slutpunkt

Ett tjänstnamn är en del av hello URL-slutpunkt mot vilken API-anrop har utfärdats. Skriv ditt namn i hello **URL** fältet. 

Kraven för tjänsten:
   * 2 och 60 tecken
   * gemena bokstäver, siffror eller bindestreck (”-”)
   * Inga streck (”-”) som hello första 2 tecken eller sista tecken
   * streck i följd (”--”)

## <a name="select-a-subscription"></a>Välj en prenumeration
Om du har mer än en prenumeration väljer du en som också har data eller file storage-tjänster. Azure Search kan automatisk identifiering av Azure Table och Blob storage, SQL Database och Azure Cosmos DB för indexering *indexerare*, men endast för tjänster i hello samma prenumeration.

## <a name="select-a-resource-group"></a>Välj en resursgrupp
En resursgrupp är en samling Azure-tjänster och resurser som används tillsammans. Till exempel om du använder Azure Search tooindex en SQL-databas, sedan båda tjänsterna bör vara en del av hello samma resursgrupp.

> [!TIP]
> En resursgrupp också tar du bort hello tjänster i den. För prototyp projekt använder flera tjänster, placerar dem i hello underlättar samma resursgrupp Rensa när hello projektet är över. 

## <a name="select-a-hosting-location"></a>Välj en värdplats 
Azure Search kan finnas i datacenter runt hälsningsmeddelande som en Azure-tjänst. Observera att [priser kan skilja sig](https://azure.microsoft.com/pricing/details/search/) efter geografi.

## <a name="select-a-pricing-tier-sku"></a>Välj en prisnivå (SKU)
[Azure Search är för närvarande finns flera prisnivåer](https://azure.microsoft.com/pricing/details/search/): ledigt, Basic eller Standard. Varje nivå har sin egen [kapacitet och gränser](search-limits-quotas-capacity.md). Se [Välj en prisnivå nivå eller SKU](search-sku-tier.md) anvisningar.

I den här genomgången, har vi valt hello standardnivån för vår tjänst.

## <a name="create-your-service"></a>Skapa din tjänst

Kom ihåg toopin service toohello instrumentpanelen för enkel åtkomst när du loggar in.

![](./media/search-create-service-portal/new-service2.png)

## <a name="scale-your-service"></a>Skala din tjänst
Det kan ta några minuter toocreate en tjänst (15 minuter eller mer beroende på hello nivån). När tjänsten har etablerats du skala det toomeet dina behov. Eftersom du har valt hello standardnivån för din Azure Search-tjänst kan du skala din tjänst i två dimensioner: repliker och partitioner. Hade du valt hello grundläggande nivån, du kan bara lägga till repliker. Skalning är inte tillgängligt om du har etablerat hello kostnadsfria tjänsten.

***Partitioner*** Tillåt din service toostore och söka igenom flera dokument.

***Repliker*** Tillåt service-toohandle en högre belastning av sökfrågor.

> [!Important]
> En tjänst måste ha [2 repliker för skrivskyddade SLA och 3 repliker för läsning och skrivning SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

1. Gå tooyour search-tjänsten bladet i hello Azure-portalen.
2. Hello vänster-navigeringsfönstret och välj **inställningar** > **skala**.
3. Använd hello slidebar tooadd repliker eller partitioner.

![](./media/search-create-service-portal/settings-scale.png)

> [!Note] 
> Varje nivå har olika [gränser](search-limits-quotas-capacity.md) på hello Totalt antal Search-enheter som tillåts i en enskild tjänst (repliker * partitioner = Totalt antal Search-enheter).

## <a name="when-tooadd-a-second-service"></a>När tooadd en andra tjänst

En stor majoritet av kunder använder en tjänst som etablerats på en nivå som ger hello [Högerklicka balans mellan resurser](search-sku-tier.md). En tjänst kan vara värd för flera index, ämne toohello [begränsningarna hello-nivå som du väljer](search-capacity-planning.md), med varje index isolerade från varandra. I Azure Search-begäranden och kan bara vara riktad tooone index, minimera hello risken för oavsiktligt eller avsiktligt datahämtning från andra index i hello samma tjänst.

Även om de flesta kunder använder en tjänst, kan det vara nödvändigt om operativa krav inkludera hello följande service redundans:

+ Katastrofåterställning (data center avbrott). Azure Search ger inte några omedelbara redundans i ett avbrott hello-händelse. Vägledning och rekommendationer finns [tjänsten administration](search-manage.md).
+ Din undersökning av multitenans modellering har fastställt att ytterligare tjänster hello optimal design. Mer information finns i [Design för multitenans](search-modeling-multitenant-saas-applications.md).
+ Du kan behöva en instans av Azure Search för globalt distribuerade program i flera regioner toominimize fördröjningen för programmets internationell trafik.

> [!NOTE]
> Du kan särskilja indexering och frågar arbetsbelastningar; i Azure Search Därför skulle du skapa flera tjänster för åtskilda arbetsbelastningar aldrig. Ett index efterfrågas alltid på hello tjänst där den skapades (du kan inte skapa ett index i en tjänst och kopiera den tooanother).
>

En andra tjänst krävs inte för hög tillgänglighet. Hög tillgänglighet för frågor uppnås när du använder 2 eller flera repliker i hello samma tjänst. Replik uppdateringar är sekventiella, vilket innebär att minst en fungerar när en tjänstuppdatering lyfts. Läs mer om drifttid [serviceavtal](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

## <a name="next-steps"></a>Nästa steg
När du etablerar en Azure Search-tjänsten är du redo för[definiera ett index](search-what-is-an-index.md) så att du kan ladda upp och söka efter data.

tooaccess hello tjänst från kod eller skript, ange hello URL (*Tjänstenamn*. search.windows.net) och en nyckel. Admin nycklar ger fullständig åtkomst; frågan nycklar bevilja läsåtkomst. Se [hur toouse Azure Sök i .NET](search-howto-dotnet-sdk.md) tooget igång.

Se [bygga och fråga ditt första index](search-get-started-portal.md) en snabb genomgång av portal-baserade.

