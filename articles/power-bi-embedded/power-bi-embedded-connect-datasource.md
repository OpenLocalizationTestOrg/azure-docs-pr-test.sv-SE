---
title: "aaaMicrosoft Power BI Embedded - anslutande tooa-datakälla"
description: "Power BI Embedded, ansluta toodata källor"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 2a4caeb3-255d-4215-9554-0ca8e3568c13
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 01/06/2017
ms.author: asaxton
ms.openlocfilehash: b1aad6e638104716d90f7e1d060eefcbc9daedbc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooa-data-source"></a><span data-ttu-id="02991-103">Ansluta tooa datakällan</span><span class="sxs-lookup"><span data-stu-id="02991-103">Connect tooa data source</span></span>
<span data-ttu-id="02991-104">Med **Power BI Embedded**, du kan bädda in rapporter i din egen app.</span><span class="sxs-lookup"><span data-stu-id="02991-104">With **Power BI Embedded**, you can embed reports into your own app.</span></span> <span data-ttu-id="02991-105">När du bäddar in en Power BI-rapport i din app hello rapporten ansluter toohello underliggande data av **importera** en kopia av hello data eller av **ansluta direkt** datakälla toohello med  **DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="02991-105">When you embed a Power BI report into your app, hello report connects toohello underlying data by **importing** a copy of hello data or by **connecting directly** toohello data source using **DirectQuery**.</span></span>

<span data-ttu-id="02991-106">Här följer hello skillnaderna mellan **importera** och **DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="02991-106">Here are hello differences between using **Import** and **DirectQuery**.</span></span>

| <span data-ttu-id="02991-107">Importera</span><span class="sxs-lookup"><span data-stu-id="02991-107">Import</span></span> | <span data-ttu-id="02991-108">DirectQuery</span><span class="sxs-lookup"><span data-stu-id="02991-108">DirectQuery</span></span> |
| --- | --- |
| <span data-ttu-id="02991-109">Tabeller, kolumner *och data* importeras eller kopieras till hello rapporten dataset.</span><span class="sxs-lookup"><span data-stu-id="02991-109">Tables, columns, *and data* are imported or copied into hello report's dataset.</span></span> <span data-ttu-id="02991-110">toosee ändras uppstod toohello underliggande data, du måste uppdatera eller importera en fullständig, aktuell datauppsättning igen.</span><span class="sxs-lookup"><span data-stu-id="02991-110">toosee changes that occurred toohello underlying data, you must refresh, or import, a complete, current dataset again.</span></span> |<span data-ttu-id="02991-111">Endast *tabeller och kolumner* importeras eller kopieras till hello rapporten dataset.</span><span class="sxs-lookup"><span data-stu-id="02991-111">Only *tables and columns* are imported or copied into hello report's dataset.</span></span> <span data-ttu-id="02991-112">Du kan alltid visa hello aktuella data.</span><span class="sxs-lookup"><span data-stu-id="02991-112">You always view hello most current data.</span></span> |

<span data-ttu-id="02991-113">Med Power BI Embedded, du kan använda DirectQuery med datakällor i molnet men inte lokala datakällor just nu.</span><span class="sxs-lookup"><span data-stu-id="02991-113">With Power BI Embedded, you can use DirectQuery with cloud data sources but not on-premises data sources at this time.</span></span>

> [!NOTE]
> <span data-ttu-id="02991-114">hello stöds lokala Data Gateway inte med Power BI Embedded just nu.</span><span class="sxs-lookup"><span data-stu-id="02991-114">hello On-Premises Data Gateway is not supported with Power BI Embedded at this time.</span></span> <span data-ttu-id="02991-115">Det innebär att du inte kan använda DirectQuery med lokala datakällor.</span><span class="sxs-lookup"><span data-stu-id="02991-115">This means you cannot use DirectQuery with on-premises data sources.</span></span>

## <a name="supported-data-sources"></a><span data-ttu-id="02991-116">Datakällor som stöds</span><span class="sxs-lookup"><span data-stu-id="02991-116">Supported data sources</span></span>

<span data-ttu-id="02991-117">**DirectQuery**</span><span class="sxs-lookup"><span data-stu-id="02991-117">**DirectQuery**</span></span>
* <span data-ttu-id="02991-118">Azure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="02991-118">Azure SQL database</span></span>
* <span data-ttu-id="02991-119">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="02991-119">Azure SQL Data Warehouse</span></span>

<span data-ttu-id="02991-120">**Importera**</span><span class="sxs-lookup"><span data-stu-id="02991-120">**Import**</span></span>

<span data-ttu-id="02991-121">Du kan importera med hjälp av alla hello tillgängliga datakällor i Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="02991-121">You can import using all of hello available data sources within Power BI Desktop.</span></span> <span data-ttu-id="02991-122">Du kommer **inte** vara kan toorefresh data i Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="02991-122">You will **not** be able toorefresh that data within Power BI Embedded.</span></span> <span data-ttu-id="02991-123">Du måste tooupload ändringar tooyour PBIX filen tooPower BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="02991-123">You will have tooupload changes tooyour PBIX file tooPower BI Embedded.</span></span> <span data-ttu-id="02991-124">Detta är på grund av toono tillgänglig gateway.</span><span class="sxs-lookup"><span data-stu-id="02991-124">This is due toono available gateway.</span></span> 

## <a name="benefits-of-using-directquery"></a><span data-ttu-id="02991-125">Fördelarna med att använda DirectQuery</span><span class="sxs-lookup"><span data-stu-id="02991-125">Benefits of using DirectQuery</span></span>
<span data-ttu-id="02991-126">Det finns två primära fördelar när du använder **DirectQuery**:</span><span class="sxs-lookup"><span data-stu-id="02991-126">There are two primary benefits when using **DirectQuery**:</span></span>

* <span data-ttu-id="02991-127">**DirectQuery** kan du skapa visualiseringar över mycket stora datamängder, om det annars skulle vara unfeasible toofirst importera alla hello data.</span><span class="sxs-lookup"><span data-stu-id="02991-127">**DirectQuery** lets you build visualizations over very large datasets, where it otherwise would be unfeasible toofirst import all of hello data.</span></span>
* <span data-ttu-id="02991-128">Underliggande data ändras kan kräva en uppdatering av data och för vissa rapporter hello måste toodisplay aktuella data kan kräva stora dataöverföringar, vilket gör att importera data unfeasible.</span><span class="sxs-lookup"><span data-stu-id="02991-128">Underlying data changes can require a refresh of data, and for some reports, hello need toodisplay current data can require large data transfers, making re-importing data unfeasible.</span></span> <span data-ttu-id="02991-129">Däremot **DirectQuery** rapporter alltid använda aktuella data.</span><span class="sxs-lookup"><span data-stu-id="02991-129">By contrast, **DirectQuery** reports always use current data.</span></span>

## <a name="limitations-of-directquery"></a><span data-ttu-id="02991-130">Begränsningar av DirectQuery</span><span class="sxs-lookup"><span data-stu-id="02991-130">Limitations of DirectQuery</span></span>
   <span data-ttu-id="02991-131">Det finns några begränsningar toousing **DirectQuery**:</span><span class="sxs-lookup"><span data-stu-id="02991-131">There are a few limitations toousing **DirectQuery**:</span></span>

* <span data-ttu-id="02991-132">Alla tabeller måste komma från en enskild databas.</span><span class="sxs-lookup"><span data-stu-id="02991-132">All tables must come from a single database.</span></span>
* <span data-ttu-id="02991-133">Om hello frågan är alltför komplex, inträffar ett fel.</span><span class="sxs-lookup"><span data-stu-id="02991-133">If hello query is overly complex, an error will occur.</span></span> <span data-ttu-id="02991-134">tooremedy hello fel du måste refactor hello frågan så att den är mindre komplex.</span><span class="sxs-lookup"><span data-stu-id="02991-134">tooremedy hello error you must refactor hello query so it is less complex.</span></span> <span data-ttu-id="02991-135">Om hello fråga måste vara komplex, behöver du tooimport hello data istället för att använda **DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="02991-135">If hello query must be complex, you will need tooimport hello data instead of using **DirectQuery**.</span></span>
* <span data-ttu-id="02991-136">Relationen filtrering är begränsad tooa riktning i stället för i båda riktningarna.</span><span class="sxs-lookup"><span data-stu-id="02991-136">Relationship filtering is limited tooa single direction, rather than both directions.</span></span>
* <span data-ttu-id="02991-137">Du kan inte ändra hello datatypen för en kolumn.</span><span class="sxs-lookup"><span data-stu-id="02991-137">You cannot change hello data type of a column.</span></span>
* <span data-ttu-id="02991-138">Som standard begränsningar DAX-uttryck som tillåts i mått.</span><span class="sxs-lookup"><span data-stu-id="02991-138">By default, limitations are placed on DAX expressions allowed in measures.</span></span> <span data-ttu-id="02991-139">Se [DirectQuery åtgärder och](#measures).</span><span class="sxs-lookup"><span data-stu-id="02991-139">See [DirectQuery and measures](#measures).</span></span>

<a name="measures"/>

## <a name="directquery-and-measures"></a><span data-ttu-id="02991-140">DirectQuery och åtgärder</span><span class="sxs-lookup"><span data-stu-id="02991-140">DirectQuery and measures</span></span>
<span data-ttu-id="02991-141">tooensure frågor skickas toohello underliggande datakällan har acceptabel prestanda, begränsningar gäller för åtgärder.</span><span class="sxs-lookup"><span data-stu-id="02991-141">tooensure queries sent toohello underlying data source have acceptable performance, limitations are imposed on measures.</span></span> <span data-ttu-id="02991-142">När du använder **Power BI Desktop**, avancerade användare kan välja toobypass den här begränsningen genom att välja **fil > Alternativ och inställningar > alternativ**.</span><span class="sxs-lookup"><span data-stu-id="02991-142">When using **Power BI Desktop**, advanced users can choose toobypass this limitation by choosing **File > Options and settings > Options**.</span></span> <span data-ttu-id="02991-143">I hello **alternativ** dialogrutan Välj **DirectQuery**, och välj alternativet för hello **tillåta obegränsad åtgärder i DirectQuery-läge**.</span><span class="sxs-lookup"><span data-stu-id="02991-143">In hello **Options** dialog, choose **DirectQuery**, and select hello option **Allow unrestricted measures in DirectQuery mode**.</span></span> <span data-ttu-id="02991-144">När alternativet väljs, kan du använda DAX-uttryck som gäller för ett mått.</span><span class="sxs-lookup"><span data-stu-id="02991-144">When that option is selected, any DAX expression that is valid for a measure can be used.</span></span> <span data-ttu-id="02991-145">Användare måste vara medvetna om; men att vissa uttryck som utför mycket bra när du importerar hello data kan resultera i mycket långsamt frågor toohello backend datakälla när i **DirectQuery** läge.</span><span class="sxs-lookup"><span data-stu-id="02991-145">Users must be aware; however, that some expressions that perform very well when hello data is imported may result in very slow queries toohello backend source when in **DirectQuery** mode.</span></span> 

## <a name="see-also"></a><span data-ttu-id="02991-146">Se även</span><span class="sxs-lookup"><span data-stu-id="02991-146">See Also</span></span>
* [<span data-ttu-id="02991-147">Kom igång med Microsoft Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="02991-147">Get started with Microsoft Power BI Embedded</span></span>](power-bi-embedded-get-started.md)
* [<span data-ttu-id="02991-148">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="02991-148">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)

<span data-ttu-id="02991-149">Fler frågor?</span><span class="sxs-lookup"><span data-stu-id="02991-149">More questions?</span></span> [<span data-ttu-id="02991-150">Försök hello Power BI-communityn</span><span class="sxs-lookup"><span data-stu-id="02991-150">Try hello Power BI Community</span></span>](http://community.powerbi.com/)

