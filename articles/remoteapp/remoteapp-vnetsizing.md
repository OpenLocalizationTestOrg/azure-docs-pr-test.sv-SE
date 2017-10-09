---
title: "information om aaaSizing för ett VNET i Azure RemoteApp | Microsoft Docs"
description: "Lär dig mer om krav för Azure RemoteApp körs med ett VNET-hello IP-adress"
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
ms.openlocfilehash: f98b831af32c41740b258d122b3e18765be08d97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sizing-information-for-a-vnet-in-azure-remoteapp"></a><span data-ttu-id="04459-103">Storleksinformation för ett VNET i Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="04459-103">Sizing information for a VNET in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="04459-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="04459-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="04459-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="04459-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="04459-106">När du använder Azure RemoteApp med ett virtuellt nätverk (VNET) använder RemoteApp IP-adresser inom hello undernät.</span><span class="sxs-lookup"><span data-stu-id="04459-106">When you use Azure RemoteApp with a virtual network (VNET), RemoteApp uses IP addresses within hello subnet.</span></span> <span data-ttu-id="04459-107">Baserat på hello skalan för RemoteApp-tjänsten måste tooensure att ditt undernät har tillräckligt med IP-adresser som är tillgängliga för RemoteApp-virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="04459-107">Based on hello scale of your RemoteApp service, you need tooensure that your subnet has enough IP addresses available for RemoteApp virtual machines.</span></span> <span data-ttu-id="04459-108">Den här vägledningen storlek är inte perfekt baserat på hur RemoteApp dynamiskt startar och roterar av virtuella datorer i en samling, hjälper den dig att beräkna undernätets intervall.</span><span class="sxs-lookup"><span data-stu-id="04459-108">While this sizing guidance isn’t perfect given how RemoteApp dynamically spins up and spins down virtual machines within a collection, it will help you estimate your subnet range.</span></span> <span data-ttu-id="04459-109">Detta är särskilt viktigt eftersom när en RemoteApp-tjänsten är placerad i ett VNET, kan du öka hello undernätets storlek utan att ta bort RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="04459-109">This is especially important as, once a RemoteApp service is placed in a VNET, you cannot increase hello subnet size without removing RemoteApp.</span></span>

<span data-ttu-id="04459-110">Du bör ha 100 IP-adresser som är tillgängliga för varje RemoteApp-samling som du vill toorun med högsta kapacitet.</span><span class="sxs-lookup"><span data-stu-id="04459-110">For each RemoteApp collection that you want toorun at maximum capacity, you should have 100 IP addresses available.</span></span> <span data-ttu-id="04459-111">Om du har en RemoteApp-samlingen i hello standardplan och du vill toohave hello maximalt 500 användare, bör du ha 100 IP-adresser för samlingen.</span><span class="sxs-lookup"><span data-stu-id="04459-111">For example, if you have one RemoteApp collection in hello Standard plan and you want toohave hello maximum 500 users, you should have 100 IP addresses for that collection.</span></span> <span data-ttu-id="04459-112">Dessutom måste 100 IP-adresser för en RemoteApp-samling i hello grundläggande planen som har 800 användare.</span><span class="sxs-lookup"><span data-stu-id="04459-112">Similarly, you need 100 IP addresses for a RemoteApp collection in hello Basic plan that has 800 users.</span></span> <span data-ttu-id="04459-113">Om du planerar toohave färre användare (mindre än hello maximala), kan du minska hello IP-adresser som behövs per samling.</span><span class="sxs-lookup"><span data-stu-id="04459-113">If you plan toohave fewer users (less than hello maximum), you can reduce hello IP addresses needed per collection.</span></span> <span data-ttu-id="04459-114">hello minsta undernät kravet är 30 IP-adresser (/ 27).</span><span class="sxs-lookup"><span data-stu-id="04459-114">hello minimum subnet size requirement is 30 IP addresses (/27).</span></span>

<span data-ttu-id="04459-115">Kolla hello följande information toomake att ditt virtuella nätverk har konfigurerats och fungerar propertly:</span><span class="sxs-lookup"><span data-stu-id="04459-115">Check out hello following information toomake sure your VNET is configured and working propertly:</span></span>

* [<span data-ttu-id="04459-116">Migrera från en personlig VNET tooan Azure VNET</span><span class="sxs-lookup"><span data-stu-id="04459-116">Migrate from a personal VNET tooan Azure VNET</span></span>](remoteapp-migratevnet.md)
* [<span data-ttu-id="04459-117">Validera hello Azure VNET toouse med Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="04459-117">Validate hello Azure VNET toouse with Azure RemoteApp</span></span>](remoteapp-vnet.md)

