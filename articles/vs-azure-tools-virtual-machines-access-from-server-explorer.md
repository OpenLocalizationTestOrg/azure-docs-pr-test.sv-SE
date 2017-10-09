---
title: "aaaAccessing Azure virtuella datorer från Server Explorer | Microsoft Docs"
description: "Få en översikt över hur tooview skapa och hantera virtuella Azure-datorer (VM) i Server Explorer i Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: eb3afde6-ba90-4308-9ac1-3cc29da4ede0
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: f8326aed105a64ca558f766d712cc68701f82c15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-azure-virtual-machines-from-server-explorer"></a><span data-ttu-id="da334-103">Åtkomst till virtuella Azure-datorer från Server Explorer</span><span class="sxs-lookup"><span data-stu-id="da334-103">Accessing Azure Virtual Machines from Server Explorer</span></span>
<span data-ttu-id="da334-104">Du kan visa information om dina virtuella datorer som finns i Azure med hjälp av Server Explorer i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="da334-104">By using Server Explorer in Visual Studio, you can display information about your virtual machines hosted by Azure.</span></span>

## <a name="accessing-virtual-machines-in-server-explorer"></a><span data-ttu-id="da334-105">Åtkomst till virtuella datorer i Server Explorer</span><span class="sxs-lookup"><span data-stu-id="da334-105">Accessing virtual machines in Server Explorer</span></span>
<span data-ttu-id="da334-106">Om du har virtuella datorer som finns i Azure, kan du komma åt dem i Server Explorer.</span><span class="sxs-lookup"><span data-stu-id="da334-106">If you have virtual machines hosted by Azure, you can access them in Server Explorer.</span></span> <span data-ttu-id="da334-107">Du måste först logga in tooyour Azure-prenumeration tooview dina mobila tjänster.</span><span class="sxs-lookup"><span data-stu-id="da334-107">You must first sign in tooyour Azure subscription tooview your mobile services.</span></span> <span data-ttu-id="da334-108">toosign, öppna hello snabbmenyn för hello Azure nod i Server Explorer och välj **ansluta tooMicrosoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="da334-108">toosign in, open hello shortcut menu for hello Azure node in Server Explorer, and choose **Connect tooMicrosoft Azure**.</span></span>

### <a name="tooget-information-about-your-virtual-machines"></a><span data-ttu-id="da334-109">tooget information om dina virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="da334-109">tooget information about your virtual machines</span></span>
1. <span data-ttu-id="da334-110">Välj en virtuell dator i Server Explorer, och välj hello F4 viktiga tooshow fönstret dess egenskaper.</span><span class="sxs-lookup"><span data-stu-id="da334-110">In Server Explorer, choose a virtual machine, and then choose hello F4 key tooshow its properties window.</span></span>
   
    <span data-ttu-id="da334-111">hello i den följande tabellen visar vilka egenskaper som är tillgängligt, men de är alla skrivskyddade.</span><span class="sxs-lookup"><span data-stu-id="da334-111">hello following table shows what properties are available, but they are all read-only.</span></span> <span data-ttu-id="da334-112">toochange dem, använda hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="da334-112">toochange them, use hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885).</span></span>
   
   | <span data-ttu-id="da334-113">Egenskap</span><span class="sxs-lookup"><span data-stu-id="da334-113">Property</span></span> | <span data-ttu-id="da334-114">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="da334-114">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="da334-115">DNS-namn</span><span class="sxs-lookup"><span data-stu-id="da334-115">DNS Name</span></span> |<span data-ttu-id="da334-116">hello URL med hello Internetadressen för hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="da334-116">hello URL with hello Internet address of hello virtual machine.</span></span> |
   | <span data-ttu-id="da334-117">Miljö</span><span class="sxs-lookup"><span data-stu-id="da334-117">Environment</span></span> |<span data-ttu-id="da334-118">Hello-värdet för den här egenskapen är alltid produktion för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="da334-118">For a virtual machine, hello value of this property is always Production.</span></span> |
   | <span data-ttu-id="da334-119">Namn</span><span class="sxs-lookup"><span data-stu-id="da334-119">Name</span></span> |<span data-ttu-id="da334-120">hello namnet på hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="da334-120">hello name of hello virtual machine.</span></span> |
   | <span data-ttu-id="da334-121">Storlek</span><span class="sxs-lookup"><span data-stu-id="da334-121">Size</span></span> |<span data-ttu-id="da334-122">hello storlek hello virtuell dator som visar hello mängden minne och diskutrymme som är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="da334-122">hello size of hello virtual machine, which reflects hello amount of memory and disk space that’s available.</span></span> <span data-ttu-id="da334-123">Mer information finns i How To: Konfigurera storlekar för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="da334-123">For more information, see How To: Configure Virtual Machine Sizes.</span></span> |
   | <span data-ttu-id="da334-124">Status</span><span class="sxs-lookup"><span data-stu-id="da334-124">Status</span></span> |<span data-ttu-id="da334-125">Värden är Start, igång, stoppas, Stoppad och hämtar Status.</span><span class="sxs-lookup"><span data-stu-id="da334-125">Values include Starting, Started, Stopping, Stopped, and Retrieving Status.</span></span> <span data-ttu-id="da334-126">Om hämtning av Status visas är hello aktuell status okänd.</span><span class="sxs-lookup"><span data-stu-id="da334-126">If Retrieving Status appears, hello current status is unknown.</span></span> <span data-ttu-id="da334-127">hello värden för den här egenskapen skiljer sig från hello-värden som används på hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="da334-127">hello values for this property differ from hello values that are used on hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885).</span></span> |
   | <span data-ttu-id="da334-128">Prenumerations-ID</span><span class="sxs-lookup"><span data-stu-id="da334-128">SubscriptionID</span></span> |<span data-ttu-id="da334-129">hello prenumerations-ID för ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="da334-129">hello subscription ID for your Azure account.</span></span> <span data-ttu-id="da334-130">Du kan visa den här informationen på hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885) genom att visa hello egenskaperna för en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="da334-130">You can show this information on hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885) by viewing hello properties for a subscription.</span></span> |
2. <span data-ttu-id="da334-131">Välj en slutpunkt-nod och sedan visar hello **egenskaper** fönster.</span><span class="sxs-lookup"><span data-stu-id="da334-131">Choose an endpoint node, and then view hello **Properties** window.</span></span>
3. <span data-ttu-id="da334-132">hello följande tabell beskrivs hello tillgängliga egenskaper för slutpunkter, men de är skrivskyddad.</span><span class="sxs-lookup"><span data-stu-id="da334-132">hello following table describes hello available properties of endpoints, but they are read-only.</span></span> <span data-ttu-id="da334-133">tooadd eller redigera hello-slutpunkter för en virtuell dator använda hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="da334-133">tooadd or edit hello endpoints for a virtual machine, use hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885).</span></span> 
   
   | <span data-ttu-id="da334-134">Egenskap</span><span class="sxs-lookup"><span data-stu-id="da334-134">Property</span></span> | <span data-ttu-id="da334-135">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="da334-135">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="da334-136">Namn</span><span class="sxs-lookup"><span data-stu-id="da334-136">Name</span></span> |<span data-ttu-id="da334-137">En identifierare för hello slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="da334-137">An identifier for hello endpoint.</span></span> |
   | <span data-ttu-id="da334-138">Privat Port</span><span class="sxs-lookup"><span data-stu-id="da334-138">Private Port</span></span> |<span data-ttu-id="da334-139">hello-port för nätverksprogram åtkomst interna tooyour.</span><span class="sxs-lookup"><span data-stu-id="da334-139">hello port for network access internal tooyour application.</span></span> |
   | <span data-ttu-id="da334-140">Protokoll</span><span class="sxs-lookup"><span data-stu-id="da334-140">Protocol</span></span> |<span data-ttu-id="da334-141">hello-protokollet som hello Transportskiktet för den här slutpunkten använder TCP eller UDP.</span><span class="sxs-lookup"><span data-stu-id="da334-141">hello protocol that hello transport layer for this endpoint uses, either TCP or UDP.</span></span> |
   | <span data-ttu-id="da334-142">Offentlig port</span><span class="sxs-lookup"><span data-stu-id="da334-142">Public Port</span></span> |<span data-ttu-id="da334-143">hello-port som används för allmän åtkomst tooyour program.</span><span class="sxs-lookup"><span data-stu-id="da334-143">hello port that’s used for public access tooyour application.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="da334-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="da334-144">Next steps</span></span>
<span data-ttu-id="da334-145">toolearn mer information om hur du använder Azure roller i Visual Studio finns [med hjälp av fjärrskrivbord med Azure-roller](vs-azure-tools-remote-desktop-roles.md).</span><span class="sxs-lookup"><span data-stu-id="da334-145">toolearn more about using Azure roles in Visual Studio, see [Using Remote Desktop with Azure Roles](vs-azure-tools-remote-desktop-roles.md).</span></span>

