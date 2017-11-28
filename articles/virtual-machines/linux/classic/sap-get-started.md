---
title: "aaaUsing SAP på Linux-datorer | Microsoft Docs"
description: "Läs om att använda SAP på virtuella Linux-datorer (VM:ar) i Microsoft Azure"
services: virtual-machines-linux,virtual-network,storage
documentationcenter: saponazure
author: MSSedusch
manager: timlt
editor: 
tags: azure-service-management
keywords: 
ms.assetid: f9cd93dc-71ad-48a4-8778-4e48aec484a6
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: campaign-page
ms.tgt_pltfrm: vm-linux
ms.workload: na
ms.date: 10/04/2016
ms.author: sedusch
ms.openlocfilehash: a805cdecb515239057e185a92bf5c4d4e707f72f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-sap-on-linux-virtual-machines-in-azure"></a><span data-ttu-id="5d269-103">Med hjälp av SAP på Linux-datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="5d269-103">Using SAP on Linux virtual machines in Azure</span></span>
<span data-ttu-id="5d269-104">Molntjänster är en term som används mycket som har blivit mer och mer betydelse inom hello IT-branschen, från små företag upp toolarge och multinationella företag.</span><span class="sxs-lookup"><span data-stu-id="5d269-104">Cloud Computing is a widely used term which is gaining more and more importance within hello IT industry, from small companies up toolarge and multinational corporations.</span></span> <span data-ttu-id="5d269-105">Microsoft Azure är hello Molnplattform tjänster från Microsoft, vilket ger ett brett spektrum av nya möjligheter.</span><span class="sxs-lookup"><span data-stu-id="5d269-105">Microsoft Azure is hello Cloud Services Platform from Microsoft which offers a wide spectrum of new possibilities.</span></span> <span data-ttu-id="5d269-106">Nu är kunder kan toorapidly etablera och avinstallation etablera program som molntjänster, så att de inte är begränsad tootechnical eller budgetering begränsningar.</span><span class="sxs-lookup"><span data-stu-id="5d269-106">Now customers are able toorapidly provision and de-provision applications as Cloud-Services, so they are not limited tootechnical or budgeting restrictions.</span></span> <span data-ttu-id="5d269-107">I stället för investera tid och budget i infrastrukturen för maskinvara, kan företag fokuserar på hello program, processer och dess fördelar för kunder och användare.</span><span class="sxs-lookup"><span data-stu-id="5d269-107">Instead of investing time and budget into hardware infrastructure, companies can focus on hello application, business processes and its benefits for customers and users.</span></span>

<span data-ttu-id="5d269-108">Microsoft erbjuder ett omfattande infrastruktur som en tjänst (IaaS)-plattform med Microsoft Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="5d269-108">With Microsoft Azure virtual machines, Microsoft offers a comprehensive Infrastructure as a Service (IaaS) platform.</span></span> <span data-ttu-id="5d269-109">SAP NetWeaver-baserade program stöds på virtuella Azure-datorer (IaaS).</span><span class="sxs-lookup"><span data-stu-id="5d269-109">SAP NetWeaver based applications are supported on Azure Virtual Machines (IaaS).</span></span> <span data-ttu-id="5d269-110">hello whitepapers nedan beskrivs hur tooplan och implementera SAP NetWeaver baserade program på Windows-datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="5d269-110">hello whitepapers below describe how tooplan and implement SAP NetWeaver based applications on Windows virtual machines in Azure.</span></span> <span data-ttu-id="5d269-111">Du kan också implementeras SAP NetWeaver baserat program på [virtuella Windows-datorer](../../windows/classic/sap-get-started.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5d269-111">You can also implement SAP NetWeaver based applications on [Windows virtual machines](../../windows/classic/sap-get-started.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-classic-sap-get-started](../../../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure-suse-linux-virtual-machines"></a><span data-ttu-id="5d269-112">SAP NetWeaver på Azure SUSE Linux virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="5d269-112">SAP NetWeaver on Azure SUSE Linux Virtual Machines</span></span>
<span data-ttu-id="5d269-113">Rubrik: aaaTesting SAP NetWeaver på Microsoft Azure SUSE Linux virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="5d269-113">title: aaaTesting SAP NetWeaver on Microsoft Azure SUSE Linux VMs</span></span>

<span data-ttu-id="5d269-114">Sammanfattning: Det finns inget officiella SAP-stöd för att köra SAP NetWeaver på virtuella Azure Linux-datorer vid denna tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="5d269-114">Summary: There is no official SAP support for running SAP NetWeaver on Azure Linux VMs at this point in time.</span></span> <span data-ttu-id="5d269-115">Dock kanske vill toodo vissa testning kunder eller överväga toorun SAP demo eller utbildning system på virtuella Azure Linux-datorer som behöver för att kontakta supporten för SAP.</span><span class="sxs-lookup"><span data-stu-id="5d269-115">Nevertheless customers might want toodo some testing or might consider toorun SAP demo or training systems on Azure Linux VMs as long as there is no need for contacting SAP support.</span></span> <span data-ttu-id="5d269-116">Den här artikeln bör hjälp med att konfigurera Azure SUSE Linux virtuella datorer för att köra SAP och ger vissa grundläggande tips i ordning tooavoid vanliga fallgropar.</span><span class="sxs-lookup"><span data-stu-id="5d269-116">This article should help setting up Azure SUSE Linux VMs for running SAP and gives some basic hints in order tooavoid common potential pitfalls.</span></span>

<span data-ttu-id="5d269-117">Uppdaterad: December 2015</span><span class="sxs-lookup"><span data-stu-id="5d269-117">Updated: December 2015</span></span>

[<span data-ttu-id="5d269-118">Den här artikeln hittar du här</span><span class="sxs-lookup"><span data-stu-id="5d269-118">This article can be found here</span></span>](../../virtual-machines-linux-sap-on-suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

