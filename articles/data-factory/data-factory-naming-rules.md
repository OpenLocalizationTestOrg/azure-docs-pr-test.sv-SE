---
title: "aaaRules för namngivning av enheter i Azure Data Factory | Microsoft Docs"
description: "Beskriver namnregler för enheter som Data Factory."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: bc5e801d-0b3b-48ec-9501-bb4146ea17f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: shlo
ms.openlocfilehash: 98c5fc5fc932b72b65894afad438b4dc321c8aca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory---naming-rules"></a><span data-ttu-id="81957-103">Azure Data Factory - namngivningsregler</span><span class="sxs-lookup"><span data-stu-id="81957-103">Azure Data Factory - naming rules</span></span>
<span data-ttu-id="81957-104">hello följande tabell innehåller namngivningsregler för Data Factory-artefakter.</span><span class="sxs-lookup"><span data-stu-id="81957-104">hello following table provides naming rules for Data Factory artifacts.</span></span>

| <span data-ttu-id="81957-105">Namn</span><span class="sxs-lookup"><span data-stu-id="81957-105">Name</span></span> | <span data-ttu-id="81957-106">Unika namn</span><span class="sxs-lookup"><span data-stu-id="81957-106">Name Uniqueness</span></span> | <span data-ttu-id="81957-107">Kontroller</span><span class="sxs-lookup"><span data-stu-id="81957-107">Validation Checks</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="81957-108">Data Factory</span><span class="sxs-lookup"><span data-stu-id="81957-108">Data Factory</span></span> |<span data-ttu-id="81957-109">Unikt över Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="81957-109">Unique across Microsoft Azure.</span></span> <span data-ttu-id="81957-110">Namn är inte skiftlägeskänsliga, det vill säga `MyDF` och `mydf` finns toohello samma data factory.</span><span class="sxs-lookup"><span data-stu-id="81957-110">Names are case-insensitive, that is, `MyDF` and `mydf` refer toohello same data factory.</span></span> |<ul><li><span data-ttu-id="81957-111">Varje datafabriken är bundet tooexactly en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="81957-111">Each data factory is tied tooexactly one Azure subscription.</span></span></li><li><span data-ttu-id="81957-112">Objektnamn måste börja med en bokstav eller en siffra och får bara innehålla bokstäver, siffror, och hello streck (-).</span><span class="sxs-lookup"><span data-stu-id="81957-112">Object names must start with a letter or a number, and can contain only letters, numbers, and hello dash (-) character.</span></span></li><li><span data-ttu-id="81957-113">Varje streck (-) måste föregås och följas av en bokstav eller en siffra.</span><span class="sxs-lookup"><span data-stu-id="81957-113">Every dash (-) character must be immediately preceded and followed by a letter or a number.</span></span> <span data-ttu-id="81957-114">Streck i följd är inte tillåtna i behållarnamn.</span><span class="sxs-lookup"><span data-stu-id="81957-114">Consecutive dashes are not permitted in container names.</span></span></li><li><span data-ttu-id="81957-115">Namnet kan innehålla 3-63 tecken.</span><span class="sxs-lookup"><span data-stu-id="81957-115">Name can be 3-63 characters long.</span></span></li></ul> |
| <span data-ttu-id="81957-116">Länkade tjänster/tabeller/Pipelines</span><span class="sxs-lookup"><span data-stu-id="81957-116">Linked Services/Tables/Pipelines</span></span> |<span data-ttu-id="81957-117">Unikt med i en data factory.</span><span class="sxs-lookup"><span data-stu-id="81957-117">Unique with in a data factory.</span></span> <span data-ttu-id="81957-118">Namn är inte skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="81957-118">Names are case-insensitive.</span></span> |<ul><li><span data-ttu-id="81957-119">Maximalt antal tecken i ett tabellnamn: 260.</span><span class="sxs-lookup"><span data-stu-id="81957-119">Maximum number of characters in a table name: 260.</span></span></li><li><span data-ttu-id="81957-120">Objektnamn måste börja med en bokstav, siffra eller ett understreck (_).</span><span class="sxs-lookup"><span data-stu-id="81957-120">Object names must start with a letter, number, or an underscore (_).</span></span></li><li><span data-ttu-id="81957-121">Följande tecken är inte tillåtna ”:”., ”+” ”,”?, ”/” ”, <” ”, >” ”, *”, ”%”, ”&” ”,:” ”,\\”</span><span class="sxs-lookup"><span data-stu-id="81957-121">Following characters are not allowed: “.”, “+”, “?”, “/”, “<”, ”>”,”*”,”%”,”&”,”:”,”\\”</span></span></li></ul> |
| <span data-ttu-id="81957-122">Resursgrupp</span><span class="sxs-lookup"><span data-stu-id="81957-122">Resource Group</span></span> |<span data-ttu-id="81957-123">Unikt över Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="81957-123">Unique across Microsoft Azure.</span></span> <span data-ttu-id="81957-124">Namn är inte skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="81957-124">Names are case-insensitive.</span></span> |<ul><li><span data-ttu-id="81957-125">Maximalt antal tecken: 1000.</span><span class="sxs-lookup"><span data-stu-id="81957-125">Maximum number of characters: 1000.</span></span></li><li><span data-ttu-id="81957-126">Namnet får innehålla bokstäver, siffror och hello följande tecken ”:-” ”, _” ”,,” och ””.</span><span class="sxs-lookup"><span data-stu-id="81957-126">Name can contain letters, digits, and hello following characters: “-”, “_”, “,” and “.”</span></span></li></ul> |

