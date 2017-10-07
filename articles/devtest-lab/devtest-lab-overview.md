---
title: aaaAbout Azure DevTest Labs | Microsoft Docs
description: "Lär dig hur DevTest Labs kan göra det enkelt toocreate, hantera och övervaka virtuella Azure-datorer"
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
ms.openlocfilehash: 5e5aa683f80144a2f6872c7fa328016120ea6253
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="about-azure-devtest-labs"></a><span data-ttu-id="60eaf-103">Om Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="60eaf-103">About Azure DevTest Labs</span></span>
## <a name="overview"></a><span data-ttu-id="60eaf-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="60eaf-104">Overview</span></span>
<span data-ttu-id="60eaf-105">Utvecklare och testare söker toosolve hello fördröjningar i Skapa och hantera sina miljöer genom att gå toohello moln.</span><span class="sxs-lookup"><span data-stu-id="60eaf-105">Developers and testers are looking toosolve hello delays in creating and managing their environments by going toohello cloud.</span></span>  <span data-ttu-id="60eaf-106">Azure löser hello problem i miljön fördröjningar och tillåter självbetjäning i en ny effektiv kostnad-struktur.</span><span class="sxs-lookup"><span data-stu-id="60eaf-106">Azure solves hello problem of environment delays and allows self-service within a new cost efficient structure.</span></span>  <span data-ttu-id="60eaf-107">Utvecklare och testare fortfarande behöver dock toospend mycket tid konfigurera sina egna hanteras miljöer.</span><span class="sxs-lookup"><span data-stu-id="60eaf-107">However, developers and testers still need toospend considerable time configuring their self-served environments.</span></span> <span data-ttu-id="60eaf-108">Dessutom är beslutsfattare osäker på hur tooleverage hello molnet toomaximize sina kostnadsbesparingar utan att lägga till så mycket bearbeta omkostnader.</span><span class="sxs-lookup"><span data-stu-id="60eaf-108">Also, decision makers are uncertain about how tooleverage hello cloud toomaximize their cost savings without adding too much process overhead.</span></span>

<span data-ttu-id="60eaf-109">Azure DevTest Labs är en tjänst som hjälper utvecklare och testare skapa snabbt miljöer i Azure under minimera skräp och kontrollera kostnaden.</span><span class="sxs-lookup"><span data-stu-id="60eaf-109">Azure DevTest Labs is a service that helps developers and testers quickly create environments in Azure while minimizing waste and controlling cost.</span></span> <span data-ttu-id="60eaf-110">Du kan testa hello senaste versionen av programmet genom att snabbt etablera Windows och Linux-miljöer med hjälp av återanvändbara mallar och artefakter.</span><span class="sxs-lookup"><span data-stu-id="60eaf-110">You can test hello latest version of your application by quickly provisioning Windows and Linux environments using reusable templates and artifacts.</span></span> <span data-ttu-id="60eaf-111">Enkelt integrera din pipeline för distribution med DevTest Labs tooprovision på begäran-miljöer.</span><span class="sxs-lookup"><span data-stu-id="60eaf-111">Easily integrate your deployment pipeline with DevTest Labs tooprovision on-demand environments.</span></span> <span data-ttu-id="60eaf-112">Skala upp din belastningen testar genom att etablera flera test agenter och skapa företablerad miljöer för utbildning och demonstrationer.</span><span class="sxs-lookup"><span data-stu-id="60eaf-112">Scale up your load testing by provisioning multiple test agents, and create pre-provisioned environments for training and demos.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/What-is-Azure-DevTest-Labs/player]
> 
> 

<span data-ttu-id="60eaf-113">DevTest Labs ger följande fördelar i hur du skapar, konfigurerar och hanterar developer- och testmiljöer i hello molnet hello</span><span class="sxs-lookup"><span data-stu-id="60eaf-113">DevTest Labs provides hello following benefits in creating, configuring, and managing developer and test environments in hello cloud</span></span>

## <a name="worry-free-self-service"></a><span data-ttu-id="60eaf-114">Problemfri självbetjäning</span><span class="sxs-lookup"><span data-stu-id="60eaf-114">Worry-free self-service</span></span>
<span data-ttu-id="60eaf-115">DevTest Labs gör det enklare toocontrol kostnader genom att låta dig tooset principer på ditt labb -, till exempel antalet virtuella datorer (VM) per användare och antalet virtuella datorer per labb.</span><span class="sxs-lookup"><span data-stu-id="60eaf-115">DevTest Labs makes it easier toocontrol costs by allowing you tooset policies on your lab - such as number of virtual machines (VM) per user and number of VMs per lab.</span></span> <span data-ttu-id="60eaf-116">DevTest Labs också kan du toocreate principer tooautomatically Stäng och starta virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="60eaf-116">DevTest Labs also enables you toocreate policies tooautomatically shut down and start VMs.</span></span>

## <a name="quickly-get-tooready-to-test"></a><span data-ttu-id="60eaf-117">Snabbt tooready-test</span><span class="sxs-lookup"><span data-stu-id="60eaf-117">Quickly get tooready-to-test</span></span>
<span data-ttu-id="60eaf-118">DevTest Labs kan toocreate företablerad miljöer med allt gruppen måste toostart utveckling och testning av applikationer.</span><span class="sxs-lookup"><span data-stu-id="60eaf-118">DevTest Labs enables you toocreate pre-provisioned environments with everything your team needs toostart developing and testing applications.</span></span> <span data-ttu-id="60eaf-119">Bara anspråk hello miljöer där hello senaste fungerande version av programmet har installerats och hämta arbeta direkt.</span><span class="sxs-lookup"><span data-stu-id="60eaf-119">Simply claim hello environments where hello last good build of your application is installed and get working right away.</span></span> <span data-ttu-id="60eaf-120">Eller Använd behållare för att skapa en miljö även snabbare och smidigare.</span><span class="sxs-lookup"><span data-stu-id="60eaf-120">Or, use containers for even faster and leaner environment creation.</span></span>

## <a name="create-once-use-everywhere"></a><span data-ttu-id="60eaf-121">Skapa en gång, använd överallt</span><span class="sxs-lookup"><span data-stu-id="60eaf-121">Create once, use everywhere</span></span>
<span data-ttu-id="60eaf-122">Samla in och dela miljö mallar och artefakter i din grupp eller organisation – allt i källkontrollen - toocreate utvecklare och testmiljöer enkelt.</span><span class="sxs-lookup"><span data-stu-id="60eaf-122">Capture and share environment templates and artifacts within your team or organization - all in source control - toocreate developer and test environments easily.</span></span>

## <a name="integrates-with-your-existing-toolchain"></a><span data-ttu-id="60eaf-123">Integreras med din befintliga verktygskedja</span><span class="sxs-lookup"><span data-stu-id="60eaf-123">Integrates with your existing toolchain</span></span>
<span data-ttu-id="60eaf-124">Utnyttja fördefinierade plugin-program eller våra API tooprovision utveckling och testning miljöer direkt från din önskade kontinuerlig integration (KO)-verktyget integrerad utvecklingsmiljö (IDE) eller automatiserade versionen pipeline.</span><span class="sxs-lookup"><span data-stu-id="60eaf-124">Leverage pre-made plug-ins or our API tooprovision Dev/Test environments directly from your preferred continuous integration (CI) tool, integrated development environment (IDE), or automated release pipeline.</span></span> <span data-ttu-id="60eaf-125">Du kan också använda vår omfattande kommandoradsverktyget.</span><span class="sxs-lookup"><span data-stu-id="60eaf-125">You can also use our comprehensive command-line tool.</span></span>


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="60eaf-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="60eaf-126">Next steps</span></span>
[<span data-ttu-id="60eaf-127">DevTest Labs-koncept</span><span class="sxs-lookup"><span data-stu-id="60eaf-127">DevTest Labs concepts</span></span>](devtest-lab-concepts.md)

