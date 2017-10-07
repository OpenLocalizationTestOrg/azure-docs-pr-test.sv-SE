---
title: "aaaConstructing filtersträngar för hello tabelldesignern | Microsoft Docs"
description: "Hur du skapar filtersträngar för hello tabelldesign"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: a1a10ea1-687a-4ee1-a952-6b24c2fe1a22
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: 48b38d27b97936064daa875e41881d51546bc11f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="constructing-filter-strings-for-hello-table-designer"></a><span data-ttu-id="8f2cf-103">Hur du skapar Filtersträngar för hello tabelldesign</span><span class="sxs-lookup"><span data-stu-id="8f2cf-103">Constructing Filter Strings for hello Table Designer</span></span>
## <a name="overview"></a><span data-ttu-id="8f2cf-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="8f2cf-104">Overview</span></span>
<span data-ttu-id="8f2cf-105">toofilter data i en Azure-tabell som visas i hello Visual Studio **tabelldesignern**, du skapar en Filtersträng och ange det i hello filterfältet.</span><span class="sxs-lookup"><span data-stu-id="8f2cf-105">toofilter data in an Azure table that is displayed in hello Visual Studio **Table Designer**, you construct a filter string and enter it into hello filter field.</span></span> <span data-ttu-id="8f2cf-106">hello filter Strängsyntaxen definieras av hello WCF Data Services och är liknande tooa SQL WHERE-sats, men toohello tabelltjänsten via en HTTP-begäran skickas.</span><span class="sxs-lookup"><span data-stu-id="8f2cf-106">hello filter string syntax is defined by hello WCF Data Services and is similar tooa SQL WHERE clause, but is sent toohello Table service via an HTTP request.</span></span> <span data-ttu-id="8f2cf-107">Hej **tabelldesignern** handtag hello rätt kodning för dig, så toofilter på önskad egenskapsvärde, du behöver endast ange hello egenskapsnamn, jämförelseoperator värde, och du kan också filtrera booleska operatorn i hello fält.</span><span class="sxs-lookup"><span data-stu-id="8f2cf-107">hello **Table Designer** handles hello proper encoding for you, so toofilter on a desired property value, you need only enter hello property name, comparison operator, criteria value, and optionally, Boolean operator in hello filter field.</span></span> <span data-ttu-id="8f2cf-108">Du behöver inte tooinclude hello $filter frågealternativet precis som om du man skapar en tabell URL tooquery hello via hello [Storage Services REST API-referens](http://go.microsoft.com/fwlink/p/?LinkId=400447).</span><span class="sxs-lookup"><span data-stu-id="8f2cf-108">You do not need tooinclude hello $filter query option as you would if you were constructing a URL tooquery hello table via hello [Storage Services REST API Reference](http://go.microsoft.com/fwlink/p/?LinkId=400447).</span></span>

<span data-ttu-id="8f2cf-109">hello WCF Data Services baseras på hello [Open Data Protocol](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData).</span><span class="sxs-lookup"><span data-stu-id="8f2cf-109">hello WCF Data Services are based on hello [Open Data Protocol](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData).</span></span> <span data-ttu-id="8f2cf-110">Mer information om hello filter system frågealternativet (**$filter**), se hello [konventioner för OData-URI-specifikationen](http://go.microsoft.com/fwlink/p/?LinkId=214806).</span><span class="sxs-lookup"><span data-stu-id="8f2cf-110">For details on hello filter system query option (**$filter**), see hello [OData URI Conventions specification](http://go.microsoft.com/fwlink/p/?LinkId=214806).</span></span>

## <a name="comparison-operators"></a><span data-ttu-id="8f2cf-111">Jämförelseoperatorer</span><span class="sxs-lookup"><span data-stu-id="8f2cf-111">Comparison Operators</span></span>
<span data-ttu-id="8f2cf-112">hello stöds följande logiska operatorer för alla egenskapstyper av:</span><span class="sxs-lookup"><span data-stu-id="8f2cf-112">hello following logical operators are supported for all property types:</span></span>

| <span data-ttu-id="8f2cf-113">Logisk operator</span><span class="sxs-lookup"><span data-stu-id="8f2cf-113">Logical operator</span></span> | <span data-ttu-id="8f2cf-114">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8f2cf-114">Description</span></span> | <span data-ttu-id="8f2cf-115">Exempel Filtersträngen</span><span class="sxs-lookup"><span data-stu-id="8f2cf-115">Example filter string</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8f2cf-116">EQ</span><span class="sxs-lookup"><span data-stu-id="8f2cf-116">eq</span></span> |<span data-ttu-id="8f2cf-117">Lika med</span><span class="sxs-lookup"><span data-stu-id="8f2cf-117">Equal</span></span> |<span data-ttu-id="8f2cf-118">Stad eq 'Redmond'</span><span class="sxs-lookup"><span data-stu-id="8f2cf-118">City eq 'Redmond'</span></span> |
| <span data-ttu-id="8f2cf-119">gt</span><span class="sxs-lookup"><span data-stu-id="8f2cf-119">gt</span></span> |<span data-ttu-id="8f2cf-120">Större än</span><span class="sxs-lookup"><span data-stu-id="8f2cf-120">Greater than</span></span> |<span data-ttu-id="8f2cf-121">Priset gt 20</span><span class="sxs-lookup"><span data-stu-id="8f2cf-121">Price gt 20</span></span> |
| <span data-ttu-id="8f2cf-122">ge</span><span class="sxs-lookup"><span data-stu-id="8f2cf-122">ge</span></span> |<span data-ttu-id="8f2cf-123">Större än eller lika för</span><span class="sxs-lookup"><span data-stu-id="8f2cf-123">Greater than or equal too</span></span>|<span data-ttu-id="8f2cf-124">Pris-ge 10</span><span class="sxs-lookup"><span data-stu-id="8f2cf-124">Price ge 10</span></span> |
| <span data-ttu-id="8f2cf-125">lt</span><span class="sxs-lookup"><span data-stu-id="8f2cf-125">lt</span></span> |<span data-ttu-id="8f2cf-126">Mindre än</span><span class="sxs-lookup"><span data-stu-id="8f2cf-126">Less than</span></span> |<span data-ttu-id="8f2cf-127">Priset lt 20</span><span class="sxs-lookup"><span data-stu-id="8f2cf-127">Price lt 20</span></span> |
| <span data-ttu-id="8f2cf-128">le</span><span class="sxs-lookup"><span data-stu-id="8f2cf-128">le</span></span> |<span data-ttu-id="8f2cf-129">Mindre än eller lika med</span><span class="sxs-lookup"><span data-stu-id="8f2cf-129">Less than or equal</span></span> |<span data-ttu-id="8f2cf-130">Priset le 100</span><span class="sxs-lookup"><span data-stu-id="8f2cf-130">Price le 100</span></span> |
| <span data-ttu-id="8f2cf-131">ne</span><span class="sxs-lookup"><span data-stu-id="8f2cf-131">ne</span></span> |<span data-ttu-id="8f2cf-132">Inte lika med</span><span class="sxs-lookup"><span data-stu-id="8f2cf-132">Not equal</span></span> |<span data-ttu-id="8f2cf-133">Stad ne ”London”</span><span class="sxs-lookup"><span data-stu-id="8f2cf-133">City ne 'London'</span></span> |
| <span data-ttu-id="8f2cf-134">och</span><span class="sxs-lookup"><span data-stu-id="8f2cf-134">and</span></span> |<span data-ttu-id="8f2cf-135">Och</span><span class="sxs-lookup"><span data-stu-id="8f2cf-135">And</span></span> |<span data-ttu-id="8f2cf-136">Priset le 200 och pris gt 3.5</span><span class="sxs-lookup"><span data-stu-id="8f2cf-136">Price le 200 and Price gt 3.5</span></span> |
| <span data-ttu-id="8f2cf-137">eller</span><span class="sxs-lookup"><span data-stu-id="8f2cf-137">or</span></span> |<span data-ttu-id="8f2cf-138">Eller</span><span class="sxs-lookup"><span data-stu-id="8f2cf-138">Or</span></span> |<span data-ttu-id="8f2cf-139">Priset le 3.5 eller pris gt 200</span><span class="sxs-lookup"><span data-stu-id="8f2cf-139">Price le 3.5 or Price gt 200</span></span> |
| <span data-ttu-id="8f2cf-140">inte</span><span class="sxs-lookup"><span data-stu-id="8f2cf-140">not</span></span> |<span data-ttu-id="8f2cf-141">inte</span><span class="sxs-lookup"><span data-stu-id="8f2cf-141">Not</span></span> |<span data-ttu-id="8f2cf-142">inte isAvailable</span><span class="sxs-lookup"><span data-stu-id="8f2cf-142">not isAvailable</span></span> |

<span data-ttu-id="8f2cf-143">När man skapar en Filtersträng är hello följande regler viktiga:</span><span class="sxs-lookup"><span data-stu-id="8f2cf-143">When constructing a filter string, hello following rules are important:</span></span>

* <span data-ttu-id="8f2cf-144">Använd hello logiska operatorer toocompare ett värde för egenskapen tooa.</span><span class="sxs-lookup"><span data-stu-id="8f2cf-144">Use hello logical operators toocompare a property tooa value.</span></span> <span data-ttu-id="8f2cf-145">Observera att det inte är möjligt toocompare ett tooa dynamiska egenskapsvärde; en sida av hello uttrycket måste vara en konstant.</span><span class="sxs-lookup"><span data-stu-id="8f2cf-145">Note that it is not possible toocompare a property tooa dynamic value; one side of hello expression must be a constant.</span></span>
* <span data-ttu-id="8f2cf-146">Alla delar av hello filtersträngen är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="8f2cf-146">All parts of hello filter string are case-sensitive.</span></span>
* <span data-ttu-id="8f2cf-147">hello konstant värde måste vara av hello samma datatyp som hello egenskap för hello tooreturn giltig filterresultaten.</span><span class="sxs-lookup"><span data-stu-id="8f2cf-147">hello constant value must be of hello same data type as hello property in order for hello filter tooreturn valid results.</span></span> <span data-ttu-id="8f2cf-148">Läs mer om stöds egenskapstyperna [hello förstå tabelltjänst-datamodellen](http://go.microsoft.com/fwlink/p/?LinkId=400448).</span><span class="sxs-lookup"><span data-stu-id="8f2cf-148">For more information about supported property types, see [Understanding hello Table Service Data Model](http://go.microsoft.com/fwlink/p/?LinkId=400448).</span></span>

## <a name="filtering-on-string-properties"></a><span data-ttu-id="8f2cf-149">Filtrering på egenskaperna för anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="8f2cf-149">Filtering on String Properties</span></span>
<span data-ttu-id="8f2cf-150">När du filtrerar på egenskaperna för anslutningssträngen omslutas hello strängkonstant enkla citattecken.</span><span class="sxs-lookup"><span data-stu-id="8f2cf-150">When you filter on string properties, enclose hello string constant in single quotation marks.</span></span>

<span data-ttu-id="8f2cf-151">följande exempel filter på hello hello **PartitionKey** och **RowKey** egenskaper; ytterligare icke-nyckeltyp egenskaper kan också läggas toohello Filtersträngen:</span><span class="sxs-lookup"><span data-stu-id="8f2cf-151">hello following example filters on hello **PartitionKey** and **RowKey** properties; additional non-key properties could also be added toohello filter string:</span></span>

    PartitionKey eq 'Partition1' and RowKey eq '00001'

<span data-ttu-id="8f2cf-152">Du kan sätta varje filteruttrycket inom parentes, även om det inte krävs:</span><span class="sxs-lookup"><span data-stu-id="8f2cf-152">You can enclose each filter expression in parentheses, although it is not required:</span></span>

    (PartitionKey eq 'Partition1') and (RowKey eq '00001')

<span data-ttu-id="8f2cf-153">Observera att hello tabelltjänsten inte har stöd för jokertecken används i frågor och de stöds inte i hello tabelldesignern antingen.</span><span class="sxs-lookup"><span data-stu-id="8f2cf-153">Note that hello Table service does not support wildcard queries, and they are not supported in hello Table Designer either.</span></span> <span data-ttu-id="8f2cf-154">Du kan dock utföra matchning med jämförelseoperatorer på hello önskade prefix prefix.</span><span class="sxs-lookup"><span data-stu-id="8f2cf-154">However, you can perform prefix matching by using comparison operators on hello desired prefix.</span></span> <span data-ttu-id="8f2cf-155">hello returnerar följande exempel entiteter med ett efternamn egenskapen börjar med hello bokstaven ”A”:</span><span class="sxs-lookup"><span data-stu-id="8f2cf-155">hello following example returns entities with a LastName property beginning with hello letter 'A':</span></span>

    LastName ge 'A' and LastName lt 'B'

## <a name="filtering-on-numeric-properties"></a><span data-ttu-id="8f2cf-156">Filtrering på numeriska egenskaper</span><span class="sxs-lookup"><span data-stu-id="8f2cf-156">Filtering on Numeric Properties</span></span>
<span data-ttu-id="8f2cf-157">toofilter på ett heltal eller flyttal, ange hello många utan citattecken.</span><span class="sxs-lookup"><span data-stu-id="8f2cf-157">toofilter on an integer or floating-point number, specify hello number without quotation marks.</span></span>

<span data-ttu-id="8f2cf-158">Det här exemplet returnerar alla entiteter med ålder egenskapen vars värde är större än 30:</span><span class="sxs-lookup"><span data-stu-id="8f2cf-158">This example returns all entities with an Age property whose value is greater than 30:</span></span>

    Age gt 30

<span data-ttu-id="8f2cf-159">Det här exemplet returnerar alla entiteter med egenskapen AmountDue vars värde är mindre än eller lika med too100.25:</span><span class="sxs-lookup"><span data-stu-id="8f2cf-159">This example returns all entities with an AmountDue property whose value is less than or equal too100.25:</span></span>

    AmountDue le 100.25

## <a name="filtering-on-boolean-properties"></a><span data-ttu-id="8f2cf-160">Filtrering på booleska egenskaper</span><span class="sxs-lookup"><span data-stu-id="8f2cf-160">Filtering on Boolean Properties</span></span>
<span data-ttu-id="8f2cf-161">toofilter på ett booleskt värde som anger **SANT** eller **FALSKT** utan citattecken.</span><span class="sxs-lookup"><span data-stu-id="8f2cf-161">toofilter on a Boolean value, specify **true** or **false** without quotation marks.</span></span>

<span data-ttu-id="8f2cf-162">hello följande exempel returneras alla entiteter där hello IsActive egenskapen har angetts för**SANT**:</span><span class="sxs-lookup"><span data-stu-id="8f2cf-162">hello following example returns all entities where hello IsActive property is set too**true**:</span></span>

    IsActive eq true

<span data-ttu-id="8f2cf-163">Du kan också skriva det här filteruttrycket utan hello logisk operator.</span><span class="sxs-lookup"><span data-stu-id="8f2cf-163">You can also write this filter expression without hello logical operator.</span></span> <span data-ttu-id="8f2cf-164">I följande exempel hello, hello tabelltjänsten också returneras alla entiteter där IsActive är **SANT**:</span><span class="sxs-lookup"><span data-stu-id="8f2cf-164">In hello following example, hello Table service will also return all entities where IsActive is **true**:</span></span>

    IsActive

<span data-ttu-id="8f2cf-165">alla enheter om IsActive är false, kan du använda inte hello tooreturn operator:</span><span class="sxs-lookup"><span data-stu-id="8f2cf-165">tooreturn all entities where IsActive is false, you can use hello not operator:</span></span>

    not IsActive

## <a name="filtering-on-datetime-properties"></a><span data-ttu-id="8f2cf-166">Filtrering på datum/tid-egenskaper</span><span class="sxs-lookup"><span data-stu-id="8f2cf-166">Filtering on DateTime Properties</span></span>
<span data-ttu-id="8f2cf-167">toofilter på ett DateTime-värde, ange hello **datetime** följt av hello tidsvärdet konstant inom enkla citattecken.</span><span class="sxs-lookup"><span data-stu-id="8f2cf-167">toofilter on a DateTime value, specify hello **datetime** keyword, followed by hello date/time constant in single quotation marks.</span></span> <span data-ttu-id="8f2cf-168">hello tidsvärdet konstanten måste vara i kombinerade UTC-format, enligt beskrivningen i [formatering DateTime egenskapsvärden](http://go.microsoft.com/fwlink/p/?LinkId=400449).</span><span class="sxs-lookup"><span data-stu-id="8f2cf-168">hello date/time constant must be in combined UTC format, as described in [Formatting DateTime Property Values](http://go.microsoft.com/fwlink/p/?LinkId=400449).</span></span>

<span data-ttu-id="8f2cf-169">hello följande exempel returnerar entiteter där hello CustomerSince egenskapen är lika tooJuly 10, 2008:</span><span class="sxs-lookup"><span data-stu-id="8f2cf-169">hello following example returns entities where hello CustomerSince property is equal tooJuly 10, 2008:</span></span>

    CustomerSince eq datetime'2008-07-10T00:00:00Z'
