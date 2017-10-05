---
title: "Appars krav för Azure RemoteApp | Microsoft Docs"
description: "Läs om krav för appar som du vill använda i Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 4427eef6-288a-49e1-97eb-fee67d99f26a
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 13d42df97ea2b090180f5865a4eac25945f9f34c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="app-requirements"></a><span data-ttu-id="5817b-103">Appkrav</span><span class="sxs-lookup"><span data-stu-id="5817b-103">App requirements</span></span>
> [!IMPORTANT]
> <span data-ttu-id="5817b-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="5817b-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="5817b-105">Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.</span><span class="sxs-lookup"><span data-stu-id="5817b-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="5817b-106">Azure RemoteApp stöder strömmande 32-bitars eller 64-bitars Windows-baserade program från en avbildning av Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="5817b-106">Azure RemoteApp supports streaming 32-bit or 64-bit Windows-based applications from a Windows Server 2012 R2 image.</span></span> <span data-ttu-id="5817b-107">De flesta 32-bitars eller 64-bitars Windows-baserade program som kör ”i befintligt skick” i Azure RemoteApp (Remote Desktop Services eller hette Terminal Services) miljö.</span><span class="sxs-lookup"><span data-stu-id="5817b-107">Most existing 32-bit or 64-bit Windows-based applications run "as is" in Azure RemoteApp (Remote Desktop Services or formerly known as Terminal Services) environment.</span></span> <span data-ttu-id="5817b-108">Men det är skillnad mellan kör och kör väl – vissa program fungerar korrekt och utföra, medan andra inte.</span><span class="sxs-lookup"><span data-stu-id="5817b-108">However, there is a difference between running and running well - some applications function correctly and perform well, while others do not.</span></span> <span data-ttu-id="5817b-109">Följande information ger vägledning för att utveckla program i en miljö med Remote Desktop Services och tester för att säkerställa kompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="5817b-109">The following information provides guidance for developing applications in a Remote Desktop Services environment and testing to ensure compatibility.</span></span>

<span data-ttu-id="5817b-110">Tips: Vi arbetar med att skapa arbetsexempel på appar du.</span><span class="sxs-lookup"><span data-stu-id="5817b-110">Tip: We're working on creating some working examples of apps for you.</span></span> <span data-ttu-id="5817b-111">Nya avsnitt som handlar om med hjälp av Microsoft Access, QuickBooks och App-V i RemoteApp visas.</span><span class="sxs-lookup"><span data-stu-id="5817b-111">You'll see new topics that discuss using Microsoft Access, QuickBooks, and App-V in RemoteApp.</span></span>

## <a name="requirements"></a><span data-ttu-id="5817b-112">Krav</span><span class="sxs-lookup"><span data-stu-id="5817b-112">Requirements</span></span>
<span data-ttu-id="5817b-113">Dessa tre krav om följt, för att köra väl i RemoteApp-programmet:</span><span class="sxs-lookup"><span data-stu-id="5817b-113">These three requirements, if followed, help your application run well in RemoteApp:</span></span>

1. <span data-ttu-id="5817b-114">Program som uppfyller alla [certifieringskraven för Windows-skrivbordsappar](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx) och följa [Fjärrskrivbordstjänster riktlinjer för programmering](https://msdn.microsoft.com/library/aa383490.aspx) har fullständig kompatibilitet med RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="5817b-114">Applications that meet all [Certification requirements for Windows desktop apps](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx) and adhere to [Remote Desktop Services programming guidelines](https://msdn.microsoft.com/library/aa383490.aspx) will have complete compatibility with RemoteApp.</span></span>
2. <span data-ttu-id="5817b-115">Program ska aldrig data lagras lokalt på bilden eller RemoteApp-instanser som kan gå förlorade.</span><span class="sxs-lookup"><span data-stu-id="5817b-115">Applications should never store data locally on the image or RemoteApp instances that can be lost.</span></span>  <span data-ttu-id="5817b-116">När du skapar en RemoteApp-samling kan klonas instanserna och är tillståndslösa och får endast innehålla program.</span><span class="sxs-lookup"><span data-stu-id="5817b-116">After you create a RemoteApp collection, the instances are cloned and are stateless and should only contain applications.</span></span> <span data-ttu-id="5817b-117">Lagra data i en extern källa eller i användarens profil.</span><span class="sxs-lookup"><span data-stu-id="5817b-117">Store data in an external source or within the user's profile.</span></span>
3. <span data-ttu-id="5817b-118">Anpassade avbildningar bör aldrig innehåller data som kan gå förlorade.</span><span class="sxs-lookup"><span data-stu-id="5817b-118">Custom images should never contain data that can be lost.</span></span>  

## <a name="testing-your-apps"></a><span data-ttu-id="5817b-119">Testa dina appar</span><span class="sxs-lookup"><span data-stu-id="5817b-119">Testing your apps</span></span>
<span data-ttu-id="5817b-120">Följ dessa steg för att testa program:</span><span class="sxs-lookup"><span data-stu-id="5817b-120">Use these steps to testing applications:</span></span>

1. <span data-ttu-id="5817b-121">Installera Windows Server 2012 R2 och ditt program</span><span class="sxs-lookup"><span data-stu-id="5817b-121">Install Windows Server 2012 R2 and your application</span></span>
2. <span data-ttu-id="5817b-122">Aktivera Fjärrskrivbord</span><span class="sxs-lookup"><span data-stu-id="5817b-122">Enable Remote Desktop</span></span>
3. <span data-ttu-id="5817b-123">Skapa två användarkonton, användare a och b, att både användarkonton läggs till i säkerhetsgruppen för fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="5817b-123">Create two user accounts, UserA and UserB, adding both user accounts to the Remote Desktop security group.</span></span>
4. <span data-ttu-id="5817b-124">Kontrollera kompatibiliteten för flera sessioner genom att skapa två samtidiga RDS-sessioner till datorn när du startar programmet.</span><span class="sxs-lookup"><span data-stu-id="5817b-124">Check multi-session compatibility by establishing two simultaneous RDS sessions to the PC while launching the application.</span></span>
5. <span data-ttu-id="5817b-125">Validera app-beteende</span><span class="sxs-lookup"><span data-stu-id="5817b-125">Validate app behavior</span></span>

## <a name="application-development-guidelines"></a><span data-ttu-id="5817b-126">Riktlinjer för utveckling av program</span><span class="sxs-lookup"><span data-stu-id="5817b-126">Application development guidelines</span></span>
<span data-ttu-id="5817b-127">Använd följande riktlinjer för att utveckla program för RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="5817b-127">Use the following guidelines for developing applications for RemoteApp.</span></span>

### <a name="multiple-users"></a><span data-ttu-id="5817b-128">Flera användare</span><span class="sxs-lookup"><span data-stu-id="5817b-128">Multiple users</span></span>
* <span data-ttu-id="5817b-129">Installera en [för en enskild användare ](https://msdn.microsoft.com/library/aa380661.aspx)kan bli ett problem i ett nätverk.</span><span class="sxs-lookup"><span data-stu-id="5817b-129">Installing an [application for a single user ](https://msdn.microsoft.com/library/aa380661.aspx)can create problems in a multiuser environment.</span></span>
* <span data-ttu-id="5817b-130">Program ska [lagra användarspecifik information](https://msdn.microsoft.com/library/aa383452.aspx) användarspecifika platser separat från globala information som gäller för alla användare.</span><span class="sxs-lookup"><span data-stu-id="5817b-130">Applications should [store user-specific information](https://msdn.microsoft.com/library/aa383452.aspx) in user-specific locations, separately from global information that applies to all users.</span></span>
* <span data-ttu-id="5817b-131">RemoteApp använder flera [namnområden för Kernelobjekt](https://msdn.microsoft.com/library/aa382954.aspx); en global namnrymd används främst av tjänster i klient/server-program.</span><span class="sxs-lookup"><span data-stu-id="5817b-131">RemoteApp uses multiple [namespaces for kernel objects](https://msdn.microsoft.com/library/aa382954.aspx); a global namespace is used primarily by services in client/server applications.</span></span>
* <span data-ttu-id="5817b-132">Det är inte säkert att anta att datornamnet eller [IP-adress](https://msdn.microsoft.com/library/aa382942.aspx) tilldelats datorn är associerade med en enskild användare eftersom flera användare kan ha loggat in samtidigt till en server som värd för fjärrskrivbordssession (RD Session Host).</span><span class="sxs-lookup"><span data-stu-id="5817b-132">It is not safe to assume that the computer name or the [IP address](https://msdn.microsoft.com/library/aa382942.aspx) assigned to the computer are associated with a single user because multiple users can be logged on simultaneously to a Remote Desktop Session Host (RD Session Host) server.</span></span>

### <a name="performance"></a><span data-ttu-id="5817b-133">Prestanda</span><span class="sxs-lookup"><span data-stu-id="5817b-133">Performance</span></span>
* <span data-ttu-id="5817b-134">Inaktivera [grafiska effekter](https://msdn.microsoft.com/library/aa380822.aspx) innan du lägger till en app i RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="5817b-134">Disable [graphic effects](https://msdn.microsoft.com/library/aa380822.aspx) before you add your app to RemoteApp.</span></span>
* <span data-ttu-id="5817b-135">Om du vill maximera CPU tillgänglighet för alla användare, antingen inaktivera [background uppgifter ](https://msdn.microsoft.com/library/aa380665.aspx) eller skapa effektiva bakgrundsaktiviteter som inte är resurskrävande.</span><span class="sxs-lookup"><span data-stu-id="5817b-135">To maximize CPU availability for all users, either disable [background tasks ](https://msdn.microsoft.com/library/aa380665.aspx) or create efficient background tasks that are not resource-intensive.</span></span>
* <span data-ttu-id="5817b-136">Du bör finjustera och balansera program [tråd användning](https://msdn.microsoft.com/library/aa383520.aspx) för miljöer med flera användare, med flera processorer.</span><span class="sxs-lookup"><span data-stu-id="5817b-136">You should tune and balance application [thread usage](https://msdn.microsoft.com/library/aa383520.aspx) for a multiuser, multiprocessor environment.</span></span>
* <span data-ttu-id="5817b-137">För att optimera prestanda, är det bra för program att [identifiera](https://msdn.microsoft.com/library/aa380798.aspx) om de körs i en klientsession.</span><span class="sxs-lookup"><span data-stu-id="5817b-137">To optimize performance, it is good practice for applications to [detect](https://msdn.microsoft.com/library/aa380798.aspx) whether they are running in a client session.</span></span>

