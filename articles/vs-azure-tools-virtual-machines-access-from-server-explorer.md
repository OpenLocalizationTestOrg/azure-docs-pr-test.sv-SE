---
title: "Åtkomst till virtuella Azure-datorer från Server Explorer | Microsoft Docs"
description: "Få en översikt över hur du visar skapa och hantera virtuella Azure-datorer (VM) i Server Explorer i Visual Studio."
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
ms.openlocfilehash: fcbb00cc2f00691e25ea84333e8c418b08210a67
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="accessing-azure-virtual-machines-from-server-explorer"></a><span data-ttu-id="ccf64-103">Åtkomst till virtuella Azure-datorer från Server Explorer</span><span class="sxs-lookup"><span data-stu-id="ccf64-103">Accessing Azure Virtual Machines from Server Explorer</span></span>
<span data-ttu-id="ccf64-104">Du kan visa information om dina virtuella datorer som finns i Azure med hjälp av Server Explorer i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ccf64-104">By using Server Explorer in Visual Studio, you can display information about your virtual machines hosted by Azure.</span></span>

## <a name="accessing-virtual-machines-in-server-explorer"></a><span data-ttu-id="ccf64-105">Åtkomst till virtuella datorer i Server Explorer</span><span class="sxs-lookup"><span data-stu-id="ccf64-105">Accessing virtual machines in Server Explorer</span></span>
<span data-ttu-id="ccf64-106">Om du har virtuella datorer som finns i Azure, kan du komma åt dem i Server Explorer.</span><span class="sxs-lookup"><span data-stu-id="ccf64-106">If you have virtual machines hosted by Azure, you can access them in Server Explorer.</span></span> <span data-ttu-id="ccf64-107">Du måste först logga in på Azure-prenumerationen att visa dina mobila tjänster.</span><span class="sxs-lookup"><span data-stu-id="ccf64-107">You must first sign in to your Azure subscription to view your mobile services.</span></span> <span data-ttu-id="ccf64-108">Öppna snabbmenyn för noden Azure i Server Explorer för att logga in och välj **Anslut till Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="ccf64-108">To sign in, open the shortcut menu for the Azure node in Server Explorer, and choose **Connect to Microsoft Azure**.</span></span>

### <a name="to-get-information-about-your-virtual-machines"></a><span data-ttu-id="ccf64-109">Att hämta information om dina virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="ccf64-109">To get information about your virtual machines</span></span>
1. <span data-ttu-id="ccf64-110">Välj en virtuell dator i Server Explorer och klicka på F4 för att visa dess egenskapsfönstret.</span><span class="sxs-lookup"><span data-stu-id="ccf64-110">In Server Explorer, choose a virtual machine, and then choose the F4 key to show its properties window.</span></span>
   
    <span data-ttu-id="ccf64-111">I följande tabell visas vilka egenskaper som är tillgängliga, men de är alla skrivskyddade.</span><span class="sxs-lookup"><span data-stu-id="ccf64-111">The following table shows what properties are available, but they are all read-only.</span></span> <span data-ttu-id="ccf64-112">Du kan ändra dem i [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="ccf64-112">To change them, use the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885).</span></span>
   
   | <span data-ttu-id="ccf64-113">Egenskap</span><span class="sxs-lookup"><span data-stu-id="ccf64-113">Property</span></span> | <span data-ttu-id="ccf64-114">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ccf64-114">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="ccf64-115">DNS-namn</span><span class="sxs-lookup"><span data-stu-id="ccf64-115">DNS Name</span></span> |<span data-ttu-id="ccf64-116">URL: en med Internet-adressen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ccf64-116">The URL with the Internet address of the virtual machine.</span></span> |
   | <span data-ttu-id="ccf64-117">Miljö</span><span class="sxs-lookup"><span data-stu-id="ccf64-117">Environment</span></span> |<span data-ttu-id="ccf64-118">Värdet för den här egenskapen är alltid produktion för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ccf64-118">For a virtual machine, the value of this property is always Production.</span></span> |
   | <span data-ttu-id="ccf64-119">Namn</span><span class="sxs-lookup"><span data-stu-id="ccf64-119">Name</span></span> |<span data-ttu-id="ccf64-120">Namnet på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ccf64-120">The name of the virtual machine.</span></span> |
   | <span data-ttu-id="ccf64-121">Storlek</span><span class="sxs-lookup"><span data-stu-id="ccf64-121">Size</span></span> |<span data-ttu-id="ccf64-122">Storleken på den virtuella datorn, som visar den totala mängden minne och diskutrymme som är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="ccf64-122">The size of the virtual machine, which reflects the amount of memory and disk space that’s available.</span></span> <span data-ttu-id="ccf64-123">Mer information finns i How To: Konfigurera storlekar för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ccf64-123">For more information, see How To: Configure Virtual Machine Sizes.</span></span> |
   | <span data-ttu-id="ccf64-124">Status</span><span class="sxs-lookup"><span data-stu-id="ccf64-124">Status</span></span> |<span data-ttu-id="ccf64-125">Värden är Start, igång, stoppas, Stoppad och hämtar Status.</span><span class="sxs-lookup"><span data-stu-id="ccf64-125">Values include Starting, Started, Stopping, Stopped, and Retrieving Status.</span></span> <span data-ttu-id="ccf64-126">Om hämtning av Status visas är aktuell status okänd.</span><span class="sxs-lookup"><span data-stu-id="ccf64-126">If Retrieving Status appears, the current status is unknown.</span></span> <span data-ttu-id="ccf64-127">Värdena för den här egenskapen skiljer sig från de värden som används på den [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="ccf64-127">The values for this property differ from the values that are used on the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885).</span></span> |
   | <span data-ttu-id="ccf64-128">Prenumerations-ID</span><span class="sxs-lookup"><span data-stu-id="ccf64-128">SubscriptionID</span></span> |<span data-ttu-id="ccf64-129">Prenumerations-ID för ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="ccf64-129">The subscription ID for your Azure account.</span></span> <span data-ttu-id="ccf64-130">Du kan visa den här informationen på den [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885) genom att visa egenskaperna för en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ccf64-130">You can show this information on the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885) by viewing the properties for a subscription.</span></span> |
2. <span data-ttu-id="ccf64-131">Välj en slutpunkt-nod och sedan visa den **egenskaper** fönster.</span><span class="sxs-lookup"><span data-stu-id="ccf64-131">Choose an endpoint node, and then view the **Properties** window.</span></span>
3. <span data-ttu-id="ccf64-132">I följande tabell beskrivs tillgängliga egenskaper för slutpunkter, men de är skrivskyddad.</span><span class="sxs-lookup"><span data-stu-id="ccf64-132">The following table describes the available properties of endpoints, but they are read-only.</span></span> <span data-ttu-id="ccf64-133">Lägg till eller redigera slutpunkterna för en virtuell dator genom att använda den [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="ccf64-133">To add or edit the endpoints for a virtual machine, use the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885).</span></span> 
   
   | <span data-ttu-id="ccf64-134">Egenskap</span><span class="sxs-lookup"><span data-stu-id="ccf64-134">Property</span></span> | <span data-ttu-id="ccf64-135">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ccf64-135">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="ccf64-136">Namn</span><span class="sxs-lookup"><span data-stu-id="ccf64-136">Name</span></span> |<span data-ttu-id="ccf64-137">En identifierare för slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="ccf64-137">An identifier for the endpoint.</span></span> |
   | <span data-ttu-id="ccf64-138">Privat Port</span><span class="sxs-lookup"><span data-stu-id="ccf64-138">Private Port</span></span> |<span data-ttu-id="ccf64-139">Port för nätverksåtkomst som är interna för ditt program.</span><span class="sxs-lookup"><span data-stu-id="ccf64-139">The port for network access internal to your application.</span></span> |
   | <span data-ttu-id="ccf64-140">Protokoll</span><span class="sxs-lookup"><span data-stu-id="ccf64-140">Protocol</span></span> |<span data-ttu-id="ccf64-141">Det protokoll som används för Transportskiktet för den här slutpunkten TCP eller UDP.</span><span class="sxs-lookup"><span data-stu-id="ccf64-141">The protocol that the transport layer for this endpoint uses, either TCP or UDP.</span></span> |
   | <span data-ttu-id="ccf64-142">Offentlig port</span><span class="sxs-lookup"><span data-stu-id="ccf64-142">Public Port</span></span> |<span data-ttu-id="ccf64-143">Den port som används för offentlig åtkomst till ditt program.</span><span class="sxs-lookup"><span data-stu-id="ccf64-143">The port that’s used for public access to your application.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ccf64-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ccf64-144">Next steps</span></span>
<span data-ttu-id="ccf64-145">Mer information om hur du använder Azure roller i Visual Studio finns [med hjälp av fjärrskrivbord med Azure-roller](vs-azure-tools-remote-desktop-roles.md).</span><span class="sxs-lookup"><span data-stu-id="ccf64-145">To learn more about using Azure roles in Visual Studio, see [Using Remote Desktop with Azure Roles](vs-azure-tools-remote-desktop-roles.md).</span></span>

