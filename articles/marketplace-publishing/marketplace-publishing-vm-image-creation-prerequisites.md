---
title: "Tekniska krav för att skapa en avbildning av virtuell dator för Azure Marketplace | Microsoft Docs"
description: "Förstå kraven för att skapa och distribuera en avbildning av virtuell dator på Azure Marketplace för andra att köpa."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 63c16966-0304-4b17-a715-368a0a5ccb2c
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 04/29/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: af3e2ad623d8d7bfafe676411f9ae3fbee78aab8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="technical-prerequisites-for-creating-a-virtual-machine-image-for-the-azure-marketplace"></a><span data-ttu-id="fa477-103">Tekniska krav för att skapa en avbildning av virtuell dator för Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="fa477-103">Technical prerequisites for creating a virtual machine image for the Azure Marketplace</span></span>
<span data-ttu-id="fa477-104">Läs noggrant innan du börjar processen och förstå varför och där varje steg utförs.</span><span class="sxs-lookup"><span data-stu-id="fa477-104">Read the process thoroughly before beginning and understand where and why each step is performed.</span></span> <span data-ttu-id="fa477-105">Så mycket som möjligt du ska förbereda företagets information och annan information, hämta nödvändiga verktyg och skapa tekniska komponenter innan du börjar skapa erbjudande.</span><span class="sxs-lookup"><span data-stu-id="fa477-105">As much as possible, you should prepare your company information and other data, download necessary tools, and/or create technical components before beginning the offer creation process.</span></span> <span data-ttu-id="fa477-106">Dessa objekt bör vara klart från granska den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="fa477-106">These items should be clear from reviewing this article.</span></span>  

## <a name="download-needed-tools--applications"></a><span data-ttu-id="fa477-107">Hämta nödvändiga verktyg och program</span><span class="sxs-lookup"><span data-stu-id="fa477-107">Download needed tools & applications</span></span>
<span data-ttu-id="fa477-108">Du bör ha följande till hands innan du börjar:</span><span class="sxs-lookup"><span data-stu-id="fa477-108">You should have the following items ready before beginning the process:</span></span>

* <span data-ttu-id="fa477-109">Beroende på vilket operativsystem du inriktar dig, installera den [Azure PowerShell-cmdlets](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) eller [Linux kommandoradsgränssnittet verktyget](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) från den [Azure hämtar](https://azure.microsoft.com/downloads/) sidan.</span><span class="sxs-lookup"><span data-stu-id="fa477-109">Depending on which operating system you are targeting, install the [Azure PowerShell cmdlets](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) or [Linux command-line interface tool](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) from the [Azure Downloads](https://azure.microsoft.com/downloads/) page.</span></span>
* <span data-ttu-id="fa477-110">Installera Azure Lagringsutforskaren från CodePlex.</span><span class="sxs-lookup"><span data-stu-id="fa477-110">Install Azure Storage Explorer from CodePlex.</span></span>
* <span data-ttu-id="fa477-111">Hämta och installera verktyget certifikatutfärdare Test för Azure certifierade:</span><span class="sxs-lookup"><span data-stu-id="fa477-111">Download and install the Certification Test Tool for Azure Certified:</span></span>
  * <span data-ttu-id="fa477-112">[http://go.microsoft.com/fwlink/?LinkId=526913](http://go.microsoft.com/fwlink/?LinkID=526913).</span><span class="sxs-lookup"><span data-stu-id="fa477-112">[http://go.microsoft.com/fwlink/?LinkID=526913](http://go.microsoft.com/fwlink/?LinkID=526913).</span></span> <span data-ttu-id="fa477-113">Behöver du en Windows-baserad dator köra verktyget certifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="fa477-113">You need a Windows-based computer to run the certification tool.</span></span> <span data-ttu-id="fa477-114">Om du inte har en Windows-baserad dator som är tillgängliga kan du köra verktyget med hjälp av en Windows-baserad virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="fa477-114">If you do not have a Windows-based computer available, you can run the tool using a Windows-based VM in Azure.</span></span>

## <a name="platforms-supported"></a><span data-ttu-id="fa477-115">Plattformar som stöds</span><span class="sxs-lookup"><span data-stu-id="fa477-115">Platforms supported</span></span>
<span data-ttu-id="fa477-116">Du kan utveckla Azure-baserade virtuella datorer i Windows eller Linux.</span><span class="sxs-lookup"><span data-stu-id="fa477-116">You can develop Azure-based VMs on Windows or Linux.</span></span> <span data-ttu-id="fa477-117">Vissa delar av publiceringsprocessen – till exempel skapa en Azure-kompatibel virtuell hårddisk (VHD)--Använd olika verktyg och steg beroende på vilket operativsystem du använder:</span><span class="sxs-lookup"><span data-stu-id="fa477-117">Some elements of the publishing process--such as creating an Azure-compatible virtual hard disk (VHD)--use different tools and steps depending on which operating system you are using:</span></span>  

* <span data-ttu-id="fa477-118">Om du använder Linux finns i avsnittet ”Skapa en Azure-kompatibel virtuell Hårddisk (Linux-baserade)” för den [virtuella avbildningen publiceringsguide](marketplace-publishing-vm-image-creation.md).</span><span class="sxs-lookup"><span data-stu-id="fa477-118">If you are using Linux, refer to the “Create an Azure-compatible VHD (Linux-based)” section of the [Virtual machine image publishing guide](marketplace-publishing-vm-image-creation.md).</span></span>
* <span data-ttu-id="fa477-119">Om du använder Windows finns i avsnittet ”Skapa en Azure-kompatibel virtuell Hårddisk (Windows-baserade)” för den [virtuella avbildningen publiceringsguide](marketplace-publishing-vm-image-creation.md).</span><span class="sxs-lookup"><span data-stu-id="fa477-119">If you are using Windows, refer to the “Create an Azure-compatible VHD (Windows-based)” section of the [Virtual machine image publishing guide](marketplace-publishing-vm-image-creation.md).</span></span>

> [!NOTE]
> <span data-ttu-id="fa477-120">Du behöver åtkomst till en Windows-baserad dator till:</span><span class="sxs-lookup"><span data-stu-id="fa477-120">You need access to a Windows-based machine to:</span></span>
> 
> * <span data-ttu-id="fa477-121">Kör verktyget certifikatutfärdare validering.</span><span class="sxs-lookup"><span data-stu-id="fa477-121">Run the certification validation tool.</span></span>
> * <span data-ttu-id="fa477-122">Skapa VHD delad åtkomst signatur URL-Adressen för VHD certifikatutfärdare överföring.</span><span class="sxs-lookup"><span data-stu-id="fa477-122">Create the VHD shared access signature URL for the VHD certification submission.</span></span>
> 
> 

## <a name="develop-your-vhd"></a><span data-ttu-id="fa477-123">Utveckla den virtuella Hårddisken</span><span class="sxs-lookup"><span data-stu-id="fa477-123">Develop your VHD</span></span>
<span data-ttu-id="fa477-124">Du kan utveckla Azure virtuella hårddiskar i molnet eller lokalt:</span><span class="sxs-lookup"><span data-stu-id="fa477-124">You can develop Azure VHDs in the cloud or on-premises:</span></span>

* <span data-ttu-id="fa477-125">Molnbaserad utveckling innebär alla development steg utförs via fjärranslutning på en virtuell Hårddisk på Azure.</span><span class="sxs-lookup"><span data-stu-id="fa477-125">Cloud-based development means all development steps are performed remotely on a VHD resident on Azure.</span></span>
* <span data-ttu-id="fa477-126">Lokal utveckling kräver hämtning av en virtuell Hårddisk och utveckla den med hjälp av lokal infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="fa477-126">On-premises development requires downloading a VHD and developing it using on-premises infrastructure.</span></span> <span data-ttu-id="fa477-127">Även om det är möjligt, rekommenderas inte den.</span><span class="sxs-lookup"><span data-stu-id="fa477-127">Although this is possible, we do not recommend it.</span></span> <span data-ttu-id="fa477-128">Observera att utveckla för Windows eller SQL lokalt måste du ha relevanta lokalt licensnycklar.</span><span class="sxs-lookup"><span data-stu-id="fa477-128">Note that developing for Windows or SQL on-premises requires you to have the relevant on-premises license keys.</span></span> <span data-ttu-id="fa477-129">Du kan inte innehålla eller installera SQL Server när du har skapat en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="fa477-129">You cannot include or install SQL Server after creating a VM.</span></span> <span data-ttu-id="fa477-130">Erbjudandet måste också baseras på en godkända SQL-avbildning från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fa477-130">You must also base your offer on an approved SQL image from the Azure portal.</span></span> <span data-ttu-id="fa477-131">Om du väljer att utveckla lokalt, måste du utföra några steg annorlunda än om du skapar i molnet.</span><span class="sxs-lookup"><span data-stu-id="fa477-131">If you decide to develop on-premises, you must perform some steps differently than if you were developing in the cloud.</span></span> <span data-ttu-id="fa477-132">Du kan hitta relevant information i [skapa en lokal VM-avbildning](marketplace-publishing-vm-image-creation-on-premise.md).</span><span class="sxs-lookup"><span data-stu-id="fa477-132">You can find relevant information in [Create an on-premises VM image](marketplace-publishing-vm-image-creation-on-premise.md).</span></span>

[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
