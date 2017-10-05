---
title: Om Azure DevTest Labs | Microsoft Docs
description: "Lär dig hur DevTest Labs kan göra det enkelt att skapa, hantera och övervaka virtuella Azure-datorer"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 1b9eed3b-c69a-4c49-a36e-f388efea6f39
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: tarcher
ms.openlocfilehash: 62e2d214d6d685c7f27c8c45cae161eb25ed1cbd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="about-azure-devtest-labs"></a><span data-ttu-id="e674a-103">Om Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="e674a-103">About Azure DevTest Labs</span></span>
## <a name="overview"></a><span data-ttu-id="e674a-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="e674a-104">Overview</span></span>
<span data-ttu-id="e674a-105">Utvecklare och testare vill lösa fördröjningar i Skapa och hantera sina miljöer genom att gå till molnet.</span><span class="sxs-lookup"><span data-stu-id="e674a-105">Developers and testers are looking to solve the delays in creating and managing their environments by going to the cloud.</span></span>  <span data-ttu-id="e674a-106">Azure löser problemet med fördröjningar i miljön och tillåter självbetjäning i en ny effektiv kostnad-struktur.</span><span class="sxs-lookup"><span data-stu-id="e674a-106">Azure solves the problem of environment delays and allows self-service within a new cost efficient structure.</span></span>  <span data-ttu-id="e674a-107">Utvecklare och testare behöver dock fortfarande att tillbringar mycket tid konfigurera sina egna hanteras miljöer.</span><span class="sxs-lookup"><span data-stu-id="e674a-107">However, developers and testers still need to spend considerable time configuring their self-served environments.</span></span> <span data-ttu-id="e674a-108">Dessutom är beslutsfattare osäker på hur man utnyttjar molnet om du vill maximera sina kostnadsbesparingar utan att lägga till kostnader för mycket processen.</span><span class="sxs-lookup"><span data-stu-id="e674a-108">Also, decision makers are uncertain about how to leverage the cloud to maximize their cost savings without adding too much process overhead.</span></span>

<span data-ttu-id="e674a-109">Azure DevTest Labs är en tjänst som hjälper utvecklare och testare skapa snabbt miljöer i Azure under minimera skräp och kontrollera kostnaden.</span><span class="sxs-lookup"><span data-stu-id="e674a-109">Azure DevTest Labs is a service that helps developers and testers quickly create environments in Azure while minimizing waste and controlling cost.</span></span> <span data-ttu-id="e674a-110">Du kan testa den senaste versionen av programmet genom att snabbt etablera Windows- och Linux-miljöer med återanvändningsbara mallar och artefakter.</span><span class="sxs-lookup"><span data-stu-id="e674a-110">You can test the latest version of your application by quickly provisioning Windows and Linux environments using reusable templates and artifacts.</span></span> <span data-ttu-id="e674a-111">Enkelt integrera din pipeline för distribution med DevTest Labs att etablera på begäran-miljöer.</span><span class="sxs-lookup"><span data-stu-id="e674a-111">Easily integrate your deployment pipeline with DevTest Labs to provision on-demand environments.</span></span> <span data-ttu-id="e674a-112">Skala upp din belastningen testar genom att etablera flera test agenter och skapa företablerad miljöer för utbildning och demonstrationer.</span><span class="sxs-lookup"><span data-stu-id="e674a-112">Scale up your load testing by provisioning multiple test agents, and create pre-provisioned environments for training and demos.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/What-is-Azure-DevTest-Labs/player]
> 
> 

<span data-ttu-id="e674a-113">DevTest Labs ger följande fördelar i hur du skapar, konfigurerar och hanterar developer- och testmiljöer i molnet</span><span class="sxs-lookup"><span data-stu-id="e674a-113">DevTest Labs provides the following benefits in creating, configuring, and managing developer and test environments in the cloud</span></span>

## <a name="worry-free-self-service"></a><span data-ttu-id="e674a-114">Problemfri självbetjäning</span><span class="sxs-lookup"><span data-stu-id="e674a-114">Worry-free self-service</span></span>
<span data-ttu-id="e674a-115">DevTest Labs gör det enklare att styra kostnader genom att du kan ange principer på ditt labb, till exempel antalet virtuella datorer (VM) per användare och antalet virtuella datorer per labb.</span><span class="sxs-lookup"><span data-stu-id="e674a-115">DevTest Labs makes it easier to control costs by allowing you to set policies on your lab - such as number of virtual machines (VM) per user and number of VMs per lab.</span></span> <span data-ttu-id="e674a-116">DevTest Labs kan du skapa principer för att automatiskt stänga och starta virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e674a-116">DevTest Labs also enables you to create policies to automatically shut down and start VMs.</span></span>

## <a name="quickly-get-to-ready-to-test"></a><span data-ttu-id="e674a-117">Hitta snabbt redo att testa</span><span class="sxs-lookup"><span data-stu-id="e674a-117">Quickly get to ready-to-test</span></span>
<span data-ttu-id="e674a-118">DevTest Labs kan du skapa företablerad miljöer med allt gruppen måste börja utveckla och testa program.</span><span class="sxs-lookup"><span data-stu-id="e674a-118">DevTest Labs enables you to create pre-provisioned environments with everything your team needs to start developing and testing applications.</span></span> <span data-ttu-id="e674a-119">Bara anspråk i miljöer där senaste fungerande version av programmet har installerats och hämta arbeta direkt.</span><span class="sxs-lookup"><span data-stu-id="e674a-119">Simply claim the environments where the last good build of your application is installed and get working right away.</span></span> <span data-ttu-id="e674a-120">Eller Använd behållare för att skapa en miljö även snabbare och smidigare.</span><span class="sxs-lookup"><span data-stu-id="e674a-120">Or, use containers for even faster and leaner environment creation.</span></span>

## <a name="create-once-use-everywhere"></a><span data-ttu-id="e674a-121">Skapa en gång, använd överallt</span><span class="sxs-lookup"><span data-stu-id="e674a-121">Create once, use everywhere</span></span>
<span data-ttu-id="e674a-122">Samla in och dela miljö mallar och artefakter i din grupp eller organisation - alla i källkontrollen - för att skapa utvecklare och testmiljöer enkelt.</span><span class="sxs-lookup"><span data-stu-id="e674a-122">Capture and share environment templates and artifacts within your team or organization - all in source control - to create developer and test environments easily.</span></span>

## <a name="integrates-with-your-existing-toolchain"></a><span data-ttu-id="e674a-123">Integreras med din befintliga verktygskedja</span><span class="sxs-lookup"><span data-stu-id="e674a-123">Integrates with your existing toolchain</span></span>
<span data-ttu-id="e674a-124">Utnyttja fördefinierade plugin-program eller våra API för att etablera miljöer för utveckling och testning direkt från din önskade kontinuerlig integration (KO)-verktyget integrerad utvecklingsmiljö (IDE) eller automatiserade versionen pipeline.</span><span class="sxs-lookup"><span data-stu-id="e674a-124">Leverage pre-made plug-ins or our API to provision Dev/Test environments directly from your preferred continuous integration (CI) tool, integrated development environment (IDE), or automated release pipeline.</span></span> <span data-ttu-id="e674a-125">Du kan också använda vår omfattande kommandoradsverktyget.</span><span class="sxs-lookup"><span data-stu-id="e674a-125">You can also use our comprehensive command-line tool.</span></span>


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="e674a-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e674a-126">Next steps</span></span>
[<span data-ttu-id="e674a-127">DevTest Labs-koncept</span><span class="sxs-lookup"><span data-stu-id="e674a-127">DevTest Labs concepts</span></span>](devtest-lab-concepts.md)

