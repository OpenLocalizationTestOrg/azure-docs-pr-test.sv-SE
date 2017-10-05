---
title: "Med SAP på Linux-datorer | Microsoft Docs"
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
ms.openlocfilehash: 66eb53f99ce4b30ec283243deb3649c9ca897a2b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-sap-on-linux-virtual-machines-in-azure"></a><span data-ttu-id="9416e-103">Med hjälp av SAP på Linux-datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="9416e-103">Using SAP on Linux virtual machines in Azure</span></span>
<span data-ttu-id="9416e-104">Cloud Computing är en välanvänd term som ökat i vikt inom IT-branschen, från småföretag upp till stora multinationella bolag.</span><span class="sxs-lookup"><span data-stu-id="9416e-104">Cloud Computing is a widely used term which is gaining more and more importance within the IT industry, from small companies up to large and multinational corporations.</span></span> <span data-ttu-id="9416e-105">Microsoft Azure är Microsofts molntjänstplattform som erbjuder ett brett utbud av nya möjligheter.</span><span class="sxs-lookup"><span data-stu-id="9416e-105">Microsoft Azure is the Cloud Services Platform from Microsoft which offers a wide spectrum of new possibilities.</span></span> <span data-ttu-id="9416e-106">Kunder kan snabbt etablera och avetablera program som molntjänster så att de inte behöver hålla tillbaka på grund av tekniska eller budgetbegränsningar.</span><span class="sxs-lookup"><span data-stu-id="9416e-106">Now customers are able to rapidly provision and de-provision applications as Cloud-Services, so they are not limited to technical or budgeting restrictions.</span></span> <span data-ttu-id="9416e-107">Istället för att investera tid och budget i maskinvaruinfrastruktur, kan företag fokusera på programmet, verksamhetsprocesserna och dess fördelar för kunder och användare.</span><span class="sxs-lookup"><span data-stu-id="9416e-107">Instead of investing time and budget into hardware infrastructure, companies can focus on the application, business processes and its benefits for customers and users.</span></span>

<span data-ttu-id="9416e-108">Microsoft erbjuder ett omfattande infrastruktur som en tjänst (IaaS)-plattform med Microsoft Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="9416e-108">With Microsoft Azure virtual machines, Microsoft offers a comprehensive Infrastructure as a Service (IaaS) platform.</span></span> <span data-ttu-id="9416e-109">SAP NetWeaver-baserade program stöds på virtuella Azure-datorer (IaaS).</span><span class="sxs-lookup"><span data-stu-id="9416e-109">SAP NetWeaver based applications are supported on Azure Virtual Machines (IaaS).</span></span> <span data-ttu-id="9416e-110">Faktablad nedan beskrivs hur du planerar och implementerar SAP NetWeaver baserat program på Windows-datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="9416e-110">The whitepapers below describe how to plan and implement SAP NetWeaver based applications on Windows virtual machines in Azure.</span></span> <span data-ttu-id="9416e-111">Du kan också implementeras SAP NetWeaver baserat program på [virtuella Windows-datorer](../../virtual-machines-windows-classic-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9416e-111">You can also implement SAP NetWeaver based applications on [Windows virtual machines](../../virtual-machines-windows-classic-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-classic-sap-get-started](../../../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure-suse-linux-virtual-machines"></a><span data-ttu-id="9416e-112">SAP NetWeaver på Azure SUSE Linux virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="9416e-112">SAP NetWeaver on Azure SUSE Linux Virtual Machines</span></span>
<span data-ttu-id="9416e-113">Rubrik: Testa SAP NetWeaver på Microsoft Azure SUSE Linux virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="9416e-113">Title: Testing SAP NetWeaver on Microsoft Azure SUSE Linux VMs</span></span>

<span data-ttu-id="9416e-114">Sammanfattning: Det finns inget officiella SAP-stöd för att köra SAP NetWeaver på virtuella Azure Linux-datorer vid denna tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="9416e-114">Summary: There is no official SAP support for running SAP NetWeaver on Azure Linux VMs at this point in time.</span></span> <span data-ttu-id="9416e-115">Dock kanske vill göra vissa tester kunder eller överväga för att köra SAP demo eller utbildningssystem på virtuella Azure Linux-datorer så länge som det ingen behövs för att kontakta supporten för SAP.</span><span class="sxs-lookup"><span data-stu-id="9416e-115">Nevertheless customers might want to do some testing or might consider to run SAP demo or training systems on Azure Linux VMs as long as there is no need for contacting SAP support.</span></span> <span data-ttu-id="9416e-116">Den här artikeln bör hjälp med att konfigurera Azure SUSE Linux virtuella datorer för att köra SAP och ger vissa grundläggande tips för att undvika vanliga fallgropar.</span><span class="sxs-lookup"><span data-stu-id="9416e-116">This article should help setting up Azure SUSE Linux VMs for running SAP and gives some basic hints in order to avoid common potential pitfalls.</span></span>

<span data-ttu-id="9416e-117">Uppdaterad: December 2015</span><span class="sxs-lookup"><span data-stu-id="9416e-117">Updated: December 2015</span></span>

[<span data-ttu-id="9416e-118">Den här artikeln hittar du här</span><span class="sxs-lookup"><span data-stu-id="9416e-118">This article can be found here</span></span>](suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

