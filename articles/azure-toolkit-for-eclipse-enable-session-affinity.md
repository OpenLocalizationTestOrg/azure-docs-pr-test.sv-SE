---
title: "Aktivera Session tillhörighet med verktyget Azure för Eclipse"
description: "Lär dig mer om att aktivera session tillhörighet med verktyget Azure för Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 88c595ec-7d85-40bd-9078-8d6be7b3f0fa
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: ab8623d6f9751ed6d71d9a5b1c0d5e939c442862
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="enable-session-affinity"></a><span data-ttu-id="8304f-103">Aktivera Session tillhörighet</span><span class="sxs-lookup"><span data-stu-id="8304f-103">Enable Session Affinity</span></span>
<span data-ttu-id="8304f-104">Du kan aktivera HTTP-session tillhörighet eller ”Fäst sessioner”, för din roller i Azure-verktygen för Eclipse.</span><span class="sxs-lookup"><span data-stu-id="8304f-104">Within the Azure Toolkit for Eclipse, you can enable HTTP session affinity, or "sticky sessions", for your roles.</span></span> <span data-ttu-id="8304f-105">Följande bild visar den **belastningsutjämning** egenskapsdialogrutan som används för att aktivera funktionen för mappning mellan sessionen:</span><span class="sxs-lookup"><span data-stu-id="8304f-105">The following image shows the **Load Balancing** properties dialog used to enable the session affinity feature:</span></span>

![][ic719492]

## <a name="to-enable-session-affinity-for-your-role"></a><span data-ttu-id="8304f-106">Så här aktiverar du session tillhörighet för din roll</span><span class="sxs-lookup"><span data-stu-id="8304f-106">To enable session affinity for your role</span></span>
1. <span data-ttu-id="8304f-107">Högerklicka på rollen i Eclipses Projektutforskaren, klicka på **Azure**, och klicka sedan på **belastningsutjämning**.</span><span class="sxs-lookup"><span data-stu-id="8304f-107">Right-click the role in Eclipse's Project Explorer, click **Azure**, and then click **Load Balancing**.</span></span>

2. <span data-ttu-id="8304f-108">I den **egenskaper för belastningsutjämning för WorkerRole1** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="8304f-108">In the **Properties for WorkerRole1 Load Balancing** dialog:</span></span>

   <span data-ttu-id="8304f-109">a.</span><span class="sxs-lookup"><span data-stu-id="8304f-109">a.</span></span> <span data-ttu-id="8304f-110">Kontrollera **Aktivera HTTP-session tillhörighet (Fäst sessioner) för den här rollen.**</span><span class="sxs-lookup"><span data-stu-id="8304f-110">Check **Enable HTTP session affinity (sticky sessions) for this role.**</span></span>

   <span data-ttu-id="8304f-111">b.</span><span class="sxs-lookup"><span data-stu-id="8304f-111">b.</span></span> <span data-ttu-id="8304f-112">För **indata slutpunkten för att använda**, Välj en slutpunkt för indata ska användas, till exempel **http (offentliga: 80, privat: 8080)**.</span><span class="sxs-lookup"><span data-stu-id="8304f-112">For **Input endpoint to use**, select an input endpoint to use, for example, **http (public:80, private:8080)**.</span></span> <span data-ttu-id="8304f-113">Programmet måste använda den här slutpunkten som sin HTTP-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="8304f-113">Your application must use this endpoint as its HTTP endpoint.</span></span> <span data-ttu-id="8304f-114">Du kan aktivera flera slutpunkter för din roll, men du kan bara markera ett av dem för att stödja Fäst sessioner.</span><span class="sxs-lookup"><span data-stu-id="8304f-114">You can enable multiple endpoints for your role, but you can select only one of them to support sticky sessions.</span></span>

   <span data-ttu-id="8304f-115">c.</span><span class="sxs-lookup"><span data-stu-id="8304f-115">c.</span></span> <span data-ttu-id="8304f-116">Återskapa ditt program.</span><span class="sxs-lookup"><span data-stu-id="8304f-116">Rebuild your application.</span></span>

<span data-ttu-id="8304f-117">När du har aktiverat, om du har mer än en rollinstans fortsätter HTTP-begäranden som kommer från en klient som hanteras av samma rollinstans.</span><span class="sxs-lookup"><span data-stu-id="8304f-117">Once enabled, if you have more than one role instance, HTTP requests coming from a particular client will continue being handled by the same role instance.</span></span>

<span data-ttu-id="8304f-118">Eclipse Toolkit gör detta genom att installera en IIS specialmoduler kallas routning av programbegäran (ARR) till var och en av dina rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="8304f-118">The Eclipse Toolkit enables this by installing a special IIS module called Application Request Routing (ARR) into each of your role instances.</span></span> <span data-ttu-id="8304f-119">ARR dras om HTTP-begäranden till rätt roll-instans.</span><span class="sxs-lookup"><span data-stu-id="8304f-119">ARR reroutes HTTP requests to the appropriate role instance.</span></span> <span data-ttu-id="8304f-120">Verktyget konfigurerar automatiskt den valda slutpunkten så att inkommande HTTP-trafik dirigeras först för ARR-programvaran.</span><span class="sxs-lookup"><span data-stu-id="8304f-120">The toolkit automatically reconfigures the selected endpoint so that the incoming HTTP traffic is first routed to the ARR software.</span></span> <span data-ttu-id="8304f-121">Verktyget skapar också en ny intern slutpunkt som din Java-server är konfigurerad för att lyssna på.</span><span class="sxs-lookup"><span data-stu-id="8304f-121">The toolkit also creates a new internal endpoint that your Java server is configured to listen to.</span></span> <span data-ttu-id="8304f-122">Det är den slutpunkt som används av ARR för att dirigera om HTTP-trafik till rätt roll-instans.</span><span class="sxs-lookup"><span data-stu-id="8304f-122">That is the endpoint used by ARR to reroute the HTTP traffic to the appropriate role instance.</span></span> <span data-ttu-id="8304f-123">På så sätt kan varje instans av serverroll i flera instanser distributionen fungerar som en omvänd proxy för alla andra instanser, aktivera Fäst sessioner.</span><span class="sxs-lookup"><span data-stu-id="8304f-123">This way, each role instance in your multi-instance deployment serves as a reverse proxy for all the other instances, enabling sticky sessions.</span></span>

## <a name="notes-about-session-affinity"></a><span data-ttu-id="8304f-124">Information om mappning mellan session</span><span class="sxs-lookup"><span data-stu-id="8304f-124">Notes about session affinity</span></span>
* <span data-ttu-id="8304f-125">Sessionen tillhörighet fungerar inte i beräkningsemulatorn.</span><span class="sxs-lookup"><span data-stu-id="8304f-125">Session affinity does not work in the compute emulator.</span></span> <span data-ttu-id="8304f-126">Inställningarna kan användas i beräkningsemulatorn utan att störa build-processen eller compute emulator körning, men själva funktionen fungerar inte i beräkningsemulatorn.</span><span class="sxs-lookup"><span data-stu-id="8304f-126">The settings can be applied in the compute emulator without interfering with your build process or compute emulator execution, but the feature itself does not function within the compute emulator.</span></span>

* <span data-ttu-id="8304f-127">Aktivera session tillhörighet resulterar i en ökning i mängden diskutrymme som används i din distribution i Azure som ytterligare programvara ska hämtas och installeras i dina rollinstanser när tjänsten startas i Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="8304f-127">Enabling session affinity will result in an increase in the amount of disk space taken up by your deployment in Azure, as additional software will be downloaded and installed into your role instances when your service is started in the Azure cloud.</span></span>

* <span data-ttu-id="8304f-128">Tid att initiera varje roll tar längre tid.</span><span class="sxs-lookup"><span data-stu-id="8304f-128">The time to initialize each role will take longer.</span></span>

* <span data-ttu-id="8304f-129">En intern slutpunkt att fungera som en trafik rerouter som nämns ovan, läggs.</span><span class="sxs-lookup"><span data-stu-id="8304f-129">An internal endpoint, to function as a traffic rerouter as mentioned above, will be added.</span></span>


## <a name="see-also"></a><span data-ttu-id="8304f-130">Se även</span><span class="sxs-lookup"><span data-stu-id="8304f-130">See Also</span></span>
<span data-ttu-id="8304f-131">[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="8304f-131">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="8304f-132">[Skapa ett Hello World-program för Azure i Eclipse][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="8304f-132">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="8304f-133">[Installera Azure Toolkit för Eclipse][Installing the Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="8304f-133">[Installing the Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="8304f-134">Mer information om hur du använder Azure med Java finns det [Azure Java Developer Center][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="8304f-134">For more information about using Azure with Java, see the [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[How to Maintain Session Data with Session Affinity]: http://go.microsoft.com/fwlink/?LinkID=699539
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->
