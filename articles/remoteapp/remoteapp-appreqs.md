---
title: "aaaApp krav för Azure RemoteApp | Microsoft Docs"
description: "Lär dig mer om hello krav för appar som du vill toouse i Azure RemoteApp"
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
ms.openlocfilehash: 3fa2bcdaab457a6fbee8ac52a81d1c4154bbdce1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="app-requirements"></a><span data-ttu-id="8f12b-103">Appkrav</span><span class="sxs-lookup"><span data-stu-id="8f12b-103">App requirements</span></span>
> [!IMPORTANT]
> <span data-ttu-id="8f12b-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="8f12b-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="8f12b-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="8f12b-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="8f12b-106">Azure RemoteApp stöder strömmande 32-bitars eller 64-bitars Windows-baserade program från en avbildning av Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="8f12b-106">Azure RemoteApp supports streaming 32-bit or 64-bit Windows-based applications from a Windows Server 2012 R2 image.</span></span> <span data-ttu-id="8f12b-107">De flesta 32-bitars eller 64-bitars Windows-baserade program som kör ”i befintligt skick” i Azure RemoteApp (Remote Desktop Services eller hette Terminal Services) miljö.</span><span class="sxs-lookup"><span data-stu-id="8f12b-107">Most existing 32-bit or 64-bit Windows-based applications run "as is" in Azure RemoteApp (Remote Desktop Services or formerly known as Terminal Services) environment.</span></span> <span data-ttu-id="8f12b-108">Men det är skillnad mellan kör och kör väl – vissa program fungerar korrekt och utföra, medan andra inte.</span><span class="sxs-lookup"><span data-stu-id="8f12b-108">However, there is a difference between running and running well - some applications function correctly and perform well, while others do not.</span></span> <span data-ttu-id="8f12b-109">hello följande information ger vägledning för att utveckla program i en miljö med Remote Desktop Services och testa tooensure kompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="8f12b-109">hello following information provides guidance for developing applications in a Remote Desktop Services environment and testing tooensure compatibility.</span></span>

<span data-ttu-id="8f12b-110">Tips: Vi arbetar med att skapa arbetsexempel på appar du.</span><span class="sxs-lookup"><span data-stu-id="8f12b-110">Tip: We're working on creating some working examples of apps for you.</span></span> <span data-ttu-id="8f12b-111">Nya avsnitt som handlar om med hjälp av Microsoft Access, QuickBooks och App-V i RemoteApp visas.</span><span class="sxs-lookup"><span data-stu-id="8f12b-111">You'll see new topics that discuss using Microsoft Access, QuickBooks, and App-V in RemoteApp.</span></span>

## <a name="requirements"></a><span data-ttu-id="8f12b-112">Krav</span><span class="sxs-lookup"><span data-stu-id="8f12b-112">Requirements</span></span>
<span data-ttu-id="8f12b-113">Dessa tre krav om följt, för att köra väl i RemoteApp-programmet:</span><span class="sxs-lookup"><span data-stu-id="8f12b-113">These three requirements, if followed, help your application run well in RemoteApp:</span></span>

1. <span data-ttu-id="8f12b-114">Program som uppfyller alla [certifieringskraven för Windows-skrivbordsappar](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx) och för att följa[Fjärrskrivbordstjänster riktlinjer för programmering](https://msdn.microsoft.com/library/aa383490.aspx) har fullständig kompatibilitet med RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="8f12b-114">Applications that meet all [Certification requirements for Windows desktop apps](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx) and adhere too[Remote Desktop Services programming guidelines](https://msdn.microsoft.com/library/aa383490.aspx) will have complete compatibility with RemoteApp.</span></span>
2. <span data-ttu-id="8f12b-115">Program ska aldrig data lagras lokalt på hello bild- eller RemoteApp-instanser som kan gå förlorade.</span><span class="sxs-lookup"><span data-stu-id="8f12b-115">Applications should never store data locally on hello image or RemoteApp instances that can be lost.</span></span>  <span data-ttu-id="8f12b-116">När du skapar en RemoteApp-samling kan klonas hello instanser och är tillståndslösa och får endast innehålla program.</span><span class="sxs-lookup"><span data-stu-id="8f12b-116">After you create a RemoteApp collection, hello instances are cloned and are stateless and should only contain applications.</span></span> <span data-ttu-id="8f12b-117">Lagra data i en extern källa eller inom hello användarprofil.</span><span class="sxs-lookup"><span data-stu-id="8f12b-117">Store data in an external source or within hello user's profile.</span></span>
3. <span data-ttu-id="8f12b-118">Anpassade avbildningar bör aldrig innehåller data som kan gå förlorade.</span><span class="sxs-lookup"><span data-stu-id="8f12b-118">Custom images should never contain data that can be lost.</span></span>  

## <a name="testing-your-apps"></a><span data-ttu-id="8f12b-119">Testa dina appar</span><span class="sxs-lookup"><span data-stu-id="8f12b-119">Testing your apps</span></span>
<span data-ttu-id="8f12b-120">Använd dessa steg tootesting program:</span><span class="sxs-lookup"><span data-stu-id="8f12b-120">Use these steps tootesting applications:</span></span>

1. <span data-ttu-id="8f12b-121">Installera Windows Server 2012 R2 och ditt program</span><span class="sxs-lookup"><span data-stu-id="8f12b-121">Install Windows Server 2012 R2 and your application</span></span>
2. <span data-ttu-id="8f12b-122">Aktivera Fjärrskrivbord</span><span class="sxs-lookup"><span data-stu-id="8f12b-122">Enable Remote Desktop</span></span>
3. <span data-ttu-id="8f12b-123">Skapa två användarkonton, användare a och b, som att lägga till både konton toohello fjärrskrivbord säkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="8f12b-123">Create two user accounts, UserA and UserB, adding both user accounts toohello Remote Desktop security group.</span></span>
4. <span data-ttu-id="8f12b-124">Kontrollera kompatibiliteten för flera sessioner genom att skapa två samtidiga RDS sessioner toohello dator när du startar programmet hello.</span><span class="sxs-lookup"><span data-stu-id="8f12b-124">Check multi-session compatibility by establishing two simultaneous RDS sessions toohello PC while launching hello application.</span></span>
5. <span data-ttu-id="8f12b-125">Validera app-beteende</span><span class="sxs-lookup"><span data-stu-id="8f12b-125">Validate app behavior</span></span>

## <a name="application-development-guidelines"></a><span data-ttu-id="8f12b-126">Riktlinjer för utveckling av program</span><span class="sxs-lookup"><span data-stu-id="8f12b-126">Application development guidelines</span></span>
<span data-ttu-id="8f12b-127">Använd följande riktlinjer för hur du utvecklar program för RemoteApp hello.</span><span class="sxs-lookup"><span data-stu-id="8f12b-127">Use hello following guidelines for developing applications for RemoteApp.</span></span>

### <a name="multiple-users"></a><span data-ttu-id="8f12b-128">Flera användare</span><span class="sxs-lookup"><span data-stu-id="8f12b-128">Multiple users</span></span>
* <span data-ttu-id="8f12b-129">Installera en [för en enskild användare ](https://msdn.microsoft.com/library/aa380661.aspx)kan bli ett problem i ett nätverk.</span><span class="sxs-lookup"><span data-stu-id="8f12b-129">Installing an [application for a single user ](https://msdn.microsoft.com/library/aa380661.aspx)can create problems in a multiuser environment.</span></span>
* <span data-ttu-id="8f12b-130">Program ska [lagra användarspecifik information](https://msdn.microsoft.com/library/aa383452.aspx) användarspecifika platser separat från globala information som gäller tooall användare.</span><span class="sxs-lookup"><span data-stu-id="8f12b-130">Applications should [store user-specific information](https://msdn.microsoft.com/library/aa383452.aspx) in user-specific locations, separately from global information that applies tooall users.</span></span>
* <span data-ttu-id="8f12b-131">RemoteApp använder flera [namnområden för Kernelobjekt](https://msdn.microsoft.com/library/aa382954.aspx); en global namnrymd används främst av tjänster i klient/server-program.</span><span class="sxs-lookup"><span data-stu-id="8f12b-131">RemoteApp uses multiple [namespaces for kernel objects](https://msdn.microsoft.com/library/aa382954.aspx); a global namespace is used primarily by services in client/server applications.</span></span>
* <span data-ttu-id="8f12b-132">Det är inte säker tooassume som hello datornamn eller hello [IP-adress](https://msdn.microsoft.com/library/aa382942.aspx) tilldelade toohello dator som är associerade med en enskild användare eftersom flera användare kan ha loggat in samtidigt tooa värd för fjärrskrivbordssession (RD Session Host) server.</span><span class="sxs-lookup"><span data-stu-id="8f12b-132">It is not safe tooassume that hello computer name or hello [IP address](https://msdn.microsoft.com/library/aa382942.aspx) assigned toohello computer are associated with a single user because multiple users can be logged on simultaneously tooa Remote Desktop Session Host (RD Session Host) server.</span></span>

### <a name="performance"></a><span data-ttu-id="8f12b-133">Prestanda</span><span class="sxs-lookup"><span data-stu-id="8f12b-133">Performance</span></span>
* <span data-ttu-id="8f12b-134">Inaktivera [grafiska effekter](https://msdn.microsoft.com/library/aa380822.aspx) innan du lägger till din app tooRemoteApp.</span><span class="sxs-lookup"><span data-stu-id="8f12b-134">Disable [graphic effects](https://msdn.microsoft.com/library/aa380822.aspx) before you add your app tooRemoteApp.</span></span>
* <span data-ttu-id="8f12b-135">toomaximize CPU tillgänglighet för alla användare, antingen inaktivera [background uppgifter ](https://msdn.microsoft.com/library/aa380665.aspx) eller skapa effektiva bakgrundsaktiviteter som inte är resurskrävande.</span><span class="sxs-lookup"><span data-stu-id="8f12b-135">toomaximize CPU availability for all users, either disable [background tasks ](https://msdn.microsoft.com/library/aa380665.aspx) or create efficient background tasks that are not resource-intensive.</span></span>
* <span data-ttu-id="8f12b-136">Du bör finjustera och balansera program [tråd användning](https://msdn.microsoft.com/library/aa383520.aspx) för miljöer med flera användare, med flera processorer.</span><span class="sxs-lookup"><span data-stu-id="8f12b-136">You should tune and balance application [thread usage](https://msdn.microsoft.com/library/aa383520.aspx) for a multiuser, multiprocessor environment.</span></span>
* <span data-ttu-id="8f12b-137">toooptimize prestanda, är det bra för program för[identifiera](https://msdn.microsoft.com/library/aa380798.aspx) om de körs i en klientsession.</span><span class="sxs-lookup"><span data-stu-id="8f12b-137">toooptimize performance, it is good practice for applications too[detect](https://msdn.microsoft.com/library/aa380798.aspx) whether they are running in a client session.</span></span>

