---
title: "Storleksinformation för ett VNET i Azure RemoteApp | Microsoft Docs"
description: "Lär dig mer om krav för IP-adress för Azure RemoteApp körs med ett virtuellt nätverk"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: b6e1c4ba-0236-42b2-bced-69353bf211be
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 9375981db64ec4a1ae523e958423b5f5787cec33
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="sizing-information-for-a-vnet-in-azure-remoteapp"></a><span data-ttu-id="3f990-103">Storleksinformation för ett VNET i Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="3f990-103">Sizing information for a VNET in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="3f990-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="3f990-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="3f990-105">Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.</span><span class="sxs-lookup"><span data-stu-id="3f990-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="3f990-106">När du använder Azure RemoteApp med ett virtuellt nätverk (VNET) använder RemoteApp IP-adresser i undernätet.</span><span class="sxs-lookup"><span data-stu-id="3f990-106">When you use Azure RemoteApp with a virtual network (VNET), RemoteApp uses IP addresses within the subnet.</span></span> <span data-ttu-id="3f990-107">Baserat på skalan för RemoteApp-tjänsten, som du behöver se till att ditt undernät har tillräckligt med IP-adresser som är tillgängliga för RemoteApp-virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3f990-107">Based on the scale of your RemoteApp service, you need to ensure that your subnet has enough IP addresses available for RemoteApp virtual machines.</span></span> <span data-ttu-id="3f990-108">Den här vägledningen storlek är inte perfekt baserat på hur RemoteApp dynamiskt startar och roterar av virtuella datorer i en samling, hjälper den dig att beräkna undernätets intervall.</span><span class="sxs-lookup"><span data-stu-id="3f990-108">While this sizing guidance isn’t perfect given how RemoteApp dynamically spins up and spins down virtual machines within a collection, it will help you estimate your subnet range.</span></span> <span data-ttu-id="3f990-109">Detta är särskilt viktigt eftersom när en RemoteApp-tjänsten är placerad i ett VNET, kan du öka undernätets storlek utan att ta bort RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="3f990-109">This is especially important as, once a RemoteApp service is placed in a VNET, you cannot increase the subnet size without removing RemoteApp.</span></span>

<span data-ttu-id="3f990-110">Du bör ha 100 IP-adresser som är tillgängliga för varje RemoteApp-samling som du vill köra med högsta kapacitet.</span><span class="sxs-lookup"><span data-stu-id="3f990-110">For each RemoteApp collection that you want to run at maximum capacity, you should have 100 IP addresses available.</span></span> <span data-ttu-id="3f990-111">Om du har en RemoteApp-samlingen i Standard-plan och du vill ha maximal 500 användare, bör du ha 100 IP-adresser för samlingen.</span><span class="sxs-lookup"><span data-stu-id="3f990-111">For example, if you have one RemoteApp collection in the Standard plan and you want to have the maximum 500 users, you should have 100 IP addresses for that collection.</span></span> <span data-ttu-id="3f990-112">Dessutom behöver du 100 IP-adresser för RemoteApp-samlingen i den grundläggande planen som har 800 användare.</span><span class="sxs-lookup"><span data-stu-id="3f990-112">Similarly, you need 100 IP addresses for a RemoteApp collection in the Basic plan that has 800 users.</span></span> <span data-ttu-id="3f990-113">Om du planerar att ha färre användare (mindre än maximalt) kan du minska IP-adresser som behövs per samling.</span><span class="sxs-lookup"><span data-stu-id="3f990-113">If you plan to have fewer users (less than the maximum), you can reduce the IP addresses needed per collection.</span></span> <span data-ttu-id="3f990-114">Kravet på minsta undernät är 30 IP-adresser (/ 27).</span><span class="sxs-lookup"><span data-stu-id="3f990-114">The minimum subnet size requirement is 30 IP addresses (/27).</span></span>

<span data-ttu-id="3f990-115">Checka ut följande information för att kontrollera att ditt virtuella nätverk är konfigurerad och igång propertly:</span><span class="sxs-lookup"><span data-stu-id="3f990-115">Check out the following information to make sure your VNET is configured and working propertly:</span></span>

* [<span data-ttu-id="3f990-116">Migrera från ett personligt virtuellt nätverk till ett Azure VNET</span><span class="sxs-lookup"><span data-stu-id="3f990-116">Migrate from a personal VNET to an Azure VNET</span></span>](remoteapp-migratevnet.md)
* [<span data-ttu-id="3f990-117">Verifiera Azure VNET att använda med Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="3f990-117">Validate the Azure VNET to use with Azure RemoteApp</span></span>](remoteapp-vnet.md)

