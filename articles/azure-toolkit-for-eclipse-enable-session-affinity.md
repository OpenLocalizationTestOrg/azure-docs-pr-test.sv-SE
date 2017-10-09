---
title: "aaaEnable Session tillhörighet med hello Azure Toolkit för Eclipse"
description: "Lär dig hur tooenable session tillhörighet med hello Azure Toolkit för Eclipse."
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
ms.openlocfilehash: 523e728c58bda95e7af4b242e831694eb6d75cb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-session-affinity"></a><span data-ttu-id="94d0d-103">Aktivera Session tillhörighet</span><span class="sxs-lookup"><span data-stu-id="94d0d-103">Enable Session Affinity</span></span>
<span data-ttu-id="94d0d-104">Du kan aktivera HTTP-session tillhörighet eller ”Fäst sessioner”, för rollerna inom hello Azure Toolkit för Eclipse.</span><span class="sxs-lookup"><span data-stu-id="94d0d-104">Within hello Azure Toolkit for Eclipse, you can enable HTTP session affinity, or "sticky sessions", for your roles.</span></span> <span data-ttu-id="94d0d-105">hello följande bild visar hello **belastningsutjämning** Egenskaper dialogrutan används tooenable hello session tillhörighet funktionen:</span><span class="sxs-lookup"><span data-stu-id="94d0d-105">hello following image shows hello **Load Balancing** properties dialog used tooenable hello session affinity feature:</span></span>

![][ic719492]

## <a name="tooenable-session-affinity-for-your-role"></a><span data-ttu-id="94d0d-106">tooenable session tillhörighet för din roll</span><span class="sxs-lookup"><span data-stu-id="94d0d-106">tooenable session affinity for your role</span></span>
1. <span data-ttu-id="94d0d-107">Högerklicka på hello roll i Eclipses Projektutforskaren, klicka på **Azure**, och klicka sedan på **belastningsutjämning**.</span><span class="sxs-lookup"><span data-stu-id="94d0d-107">Right-click hello role in Eclipse's Project Explorer, click **Azure**, and then click **Load Balancing**.</span></span>

2. <span data-ttu-id="94d0d-108">I hello **egenskaper för belastningsutjämning för WorkerRole1** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="94d0d-108">In hello **Properties for WorkerRole1 Load Balancing** dialog:</span></span>

   <span data-ttu-id="94d0d-109">a.</span><span class="sxs-lookup"><span data-stu-id="94d0d-109">a.</span></span> <span data-ttu-id="94d0d-110">Kontrollera **Aktivera HTTP-session tillhörighet (Fäst sessioner) för den här rollen.**</span><span class="sxs-lookup"><span data-stu-id="94d0d-110">Check **Enable HTTP session affinity (sticky sessions) for this role.**</span></span>

   <span data-ttu-id="94d0d-111">b.</span><span class="sxs-lookup"><span data-stu-id="94d0d-111">b.</span></span> <span data-ttu-id="94d0d-112">För **inkommande slutpunkt toouse**, till exempel väljer en indataslutpunkten toouse **http (offentliga: 80, privat: 8080)**.</span><span class="sxs-lookup"><span data-stu-id="94d0d-112">For **Input endpoint toouse**, select an input endpoint toouse, for example, **http (public:80, private:8080)**.</span></span> <span data-ttu-id="94d0d-113">Programmet måste använda den här slutpunkten som sin HTTP-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="94d0d-113">Your application must use this endpoint as its HTTP endpoint.</span></span> <span data-ttu-id="94d0d-114">Du kan aktivera flera slutpunkter för din roll, men du kan välja endast en av dem toosupport Fäst sessioner.</span><span class="sxs-lookup"><span data-stu-id="94d0d-114">You can enable multiple endpoints for your role, but you can select only one of them toosupport sticky sessions.</span></span>

   <span data-ttu-id="94d0d-115">c.</span><span class="sxs-lookup"><span data-stu-id="94d0d-115">c.</span></span> <span data-ttu-id="94d0d-116">Återskapa ditt program.</span><span class="sxs-lookup"><span data-stu-id="94d0d-116">Rebuild your application.</span></span>

<span data-ttu-id="94d0d-117">När du har aktiverat, om du har mer än en rollinstans, HTTP-begäranden som kommer från en viss klient kommer att fortsätta hanteras av hello samma instans av serverroll.</span><span class="sxs-lookup"><span data-stu-id="94d0d-117">Once enabled, if you have more than one role instance, HTTP requests coming from a particular client will continue being handled by hello same role instance.</span></span>

<span data-ttu-id="94d0d-118">hello Eclipse Toolkit gör detta genom att installera en IIS specialmoduler kallas routning av programbegäran (ARR) till var och en av dina rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="94d0d-118">hello Eclipse Toolkit enables this by installing a special IIS module called Application Request Routing (ARR) into each of your role instances.</span></span> <span data-ttu-id="94d0d-119">ARR dras om HTTP-begäranden toohello rätt rollinstans.</span><span class="sxs-lookup"><span data-stu-id="94d0d-119">ARR reroutes HTTP requests toohello appropriate role instance.</span></span> <span data-ttu-id="94d0d-120">hello toolkit konfigurerar automatiskt hello markerade slutpunkten så att hello inkommande HTTP-trafik är första routade toohello ARR-programvara.</span><span class="sxs-lookup"><span data-stu-id="94d0d-120">hello toolkit automatically reconfigures hello selected endpoint so that hello incoming HTTP traffic is first routed toohello ARR software.</span></span> <span data-ttu-id="94d0d-121">hello toolkit skapas också en ny intern slutpunkt som Java-server är konfigurerad toolisten till.</span><span class="sxs-lookup"><span data-stu-id="94d0d-121">hello toolkit also creates a new internal endpoint that your Java server is configured toolisten to.</span></span> <span data-ttu-id="94d0d-122">Det är hello-slutpunkt som används av ARR tooreroute hello HTTP-trafik toohello rätt rollinstans.</span><span class="sxs-lookup"><span data-stu-id="94d0d-122">That is hello endpoint used by ARR tooreroute hello HTTP traffic toohello appropriate role instance.</span></span> <span data-ttu-id="94d0d-123">På så sätt kan fungerar varje instans av serverroll i flera instanser distributionen som en omvänd proxy för alla hello andra instanser aktiverar Fäst sessioner.</span><span class="sxs-lookup"><span data-stu-id="94d0d-123">This way, each role instance in your multi-instance deployment serves as a reverse proxy for all hello other instances, enabling sticky sessions.</span></span>

## <a name="notes-about-session-affinity"></a><span data-ttu-id="94d0d-124">Information om mappning mellan session</span><span class="sxs-lookup"><span data-stu-id="94d0d-124">Notes about session affinity</span></span>
* <span data-ttu-id="94d0d-125">Sessionen tillhörighet fungerar inte i hello beräkningsemulatorn.</span><span class="sxs-lookup"><span data-stu-id="94d0d-125">Session affinity does not work in hello compute emulator.</span></span> <span data-ttu-id="94d0d-126">hello inställningar kan användas i hello beräkningsemulatorn utan att störa build-processen eller compute emulator körning, men själva hello-funktionen fungerar inte i hello beräkningsemulatorn.</span><span class="sxs-lookup"><span data-stu-id="94d0d-126">hello settings can be applied in hello compute emulator without interfering with your build process or compute emulator execution, but hello feature itself does not function within hello compute emulator.</span></span>

* <span data-ttu-id="94d0d-127">Aktivera session tillhörighet resulterar i ökad hello mängden diskutrymme som används i din distribution i Azure som ytterligare programvara ska hämtas och installeras i dina rollinstanser när tjänsten startas i hello Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="94d0d-127">Enabling session affinity will result in an increase in hello amount of disk space taken up by your deployment in Azure, as additional software will be downloaded and installed into your role instances when your service is started in hello Azure cloud.</span></span>

* <span data-ttu-id="94d0d-128">hello tid tooinitialize varje roll tar längre tid.</span><span class="sxs-lookup"><span data-stu-id="94d0d-128">hello time tooinitialize each role will take longer.</span></span>

* <span data-ttu-id="94d0d-129">En intern slutpunkt toofunction som en trafik rerouter som nämns ovan, läggs.</span><span class="sxs-lookup"><span data-stu-id="94d0d-129">An internal endpoint, toofunction as a traffic rerouter as mentioned above, will be added.</span></span>


## <a name="see-also"></a><span data-ttu-id="94d0d-130">Se även</span><span class="sxs-lookup"><span data-stu-id="94d0d-130">See Also</span></span>
<span data-ttu-id="94d0d-131">[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="94d0d-131">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="94d0d-132">[Skapa ett Hello World-program för Azure i Eclipse][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="94d0d-132">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="94d0d-133">[Installera hello Azure Toolkit för Eclipse][Installing hello Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="94d0d-133">[Installing hello Azure Toolkit for Eclipse][Installing hello Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="94d0d-134">Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="94d0d-134">For more information about using Azure with Java, see hello [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[How tooMaintain Session Data with Session Affinity]: http://go.microsoft.com/fwlink/?LinkID=699539
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->
