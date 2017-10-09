---
title: "aaaTechnical förutsättningar för att skapa en avbildning av virtuell dator för hello Azure Marketplace | Microsoft Docs"
description: "Förstå hello kraven för att skapa och distribuera en virtuell dator på en bild-toohello Azure Marketplace för andra toopurchase."
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
ms.openlocfilehash: fcae4d9e052581e3c1dfe7962e6d50ec31040419
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="technical-prerequisites-for-creating-a-virtual-machine-image-for-hello-azure-marketplace"></a><span data-ttu-id="2b2df-103">Tekniska krav för att skapa en avbildning av virtuell dator för hello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="2b2df-103">Technical prerequisites for creating a virtual machine image for hello Azure Marketplace</span></span>
<span data-ttu-id="2b2df-104">Läs hello processen noggrant innan du börjar och förstå varför och där varje steg utförs.</span><span class="sxs-lookup"><span data-stu-id="2b2df-104">Read hello process thoroughly before beginning and understand where and why each step is performed.</span></span> <span data-ttu-id="2b2df-105">Så mycket som möjligt du ska förbereda företagets information och andra data, hämta nödvändiga verktyg och skapa tekniska komponenter innan du börjar hello erbjudande skapas.</span><span class="sxs-lookup"><span data-stu-id="2b2df-105">As much as possible, you should prepare your company information and other data, download necessary tools, and/or create technical components before beginning hello offer creation process.</span></span> <span data-ttu-id="2b2df-106">Dessa objekt bör vara klart från granska den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="2b2df-106">These items should be clear from reviewing this article.</span></span>  

## <a name="download-needed-tools--applications"></a><span data-ttu-id="2b2df-107">Hämta nödvändiga verktyg och program</span><span class="sxs-lookup"><span data-stu-id="2b2df-107">Download needed tools & applications</span></span>
<span data-ttu-id="2b2df-108">Du bör ha följande till hands innan du börjar hello hello:</span><span class="sxs-lookup"><span data-stu-id="2b2df-108">You should have hello following items ready before beginning hello process:</span></span>

* <span data-ttu-id="2b2df-109">Beroende på vilket operativsystem du inriktar dig, installera hello [Azure PowerShell-cmdlets](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) eller [Linux kommandoradsgränssnittet verktyget](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) från hello [Azure hämtar](https://azure.microsoft.com/downloads/) sidan.</span><span class="sxs-lookup"><span data-stu-id="2b2df-109">Depending on which operating system you are targeting, install hello [Azure PowerShell cmdlets](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) or [Linux command-line interface tool](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) from hello [Azure Downloads](https://azure.microsoft.com/downloads/) page.</span></span>
* <span data-ttu-id="2b2df-110">Installera Azure Lagringsutforskaren från CodePlex.</span><span class="sxs-lookup"><span data-stu-id="2b2df-110">Install Azure Storage Explorer from CodePlex.</span></span>
* <span data-ttu-id="2b2df-111">Hämta och installera hello certifikatutfärdare-verktyget för Azure certifierade:</span><span class="sxs-lookup"><span data-stu-id="2b2df-111">Download and install hello Certification Test Tool for Azure Certified:</span></span>
  * <span data-ttu-id="2b2df-112">[http://go.microsoft.com/fwlink/?LinkId=526913](http://go.microsoft.com/fwlink/?LinkID=526913).</span><span class="sxs-lookup"><span data-stu-id="2b2df-112">[http://go.microsoft.com/fwlink/?LinkID=526913](http://go.microsoft.com/fwlink/?LinkID=526913).</span></span> <span data-ttu-id="2b2df-113">Du behöver ett Windows-baserad dator toorun hello certifikatutfärdare verktyg.</span><span class="sxs-lookup"><span data-stu-id="2b2df-113">You need a Windows-based computer toorun hello certification tool.</span></span> <span data-ttu-id="2b2df-114">Om du inte har en Windows-baserad dator som är tillgängliga kan du köra hello verktyget med en Windows-baserad virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="2b2df-114">If you do not have a Windows-based computer available, you can run hello tool using a Windows-based VM in Azure.</span></span>

## <a name="platforms-supported"></a><span data-ttu-id="2b2df-115">Plattformar som stöds</span><span class="sxs-lookup"><span data-stu-id="2b2df-115">Platforms supported</span></span>
<span data-ttu-id="2b2df-116">Du kan utveckla Azure-baserade virtuella datorer i Windows eller Linux.</span><span class="sxs-lookup"><span data-stu-id="2b2df-116">You can develop Azure-based VMs on Windows or Linux.</span></span> <span data-ttu-id="2b2df-117">Vissa delar av hello publiceringsprocessen – till exempel skapa en Azure-kompatibel virtuell hårddisk (VHD)--Använd olika verktyg och steg beroende på vilket operativsystem du använder:</span><span class="sxs-lookup"><span data-stu-id="2b2df-117">Some elements of hello publishing process--such as creating an Azure-compatible virtual hard disk (VHD)--use different tools and steps depending on which operating system you are using:</span></span>  

* <span data-ttu-id="2b2df-118">Om du använder Linux finns toohello ”skapa en Azure-kompatibel virtuell Hårddisk (Linux-baserade)” avsnittet av hello [virtuella avbildningen publiceringsguide](marketplace-publishing-vm-image-creation.md).</span><span class="sxs-lookup"><span data-stu-id="2b2df-118">If you are using Linux, refer toohello “Create an Azure-compatible VHD (Linux-based)” section of hello [Virtual machine image publishing guide](marketplace-publishing-vm-image-creation.md).</span></span>
* <span data-ttu-id="2b2df-119">Om du använder Windows finns toohello ”skapa en Azure-kompatibel virtuell Hårddisk (Windows-baserade)” avsnittet av hello [virtuella avbildningen publiceringsguide](marketplace-publishing-vm-image-creation.md).</span><span class="sxs-lookup"><span data-stu-id="2b2df-119">If you are using Windows, refer toohello “Create an Azure-compatible VHD (Windows-based)” section of hello [Virtual machine image publishing guide](marketplace-publishing-vm-image-creation.md).</span></span>

> [!NOTE]
> <span data-ttu-id="2b2df-120">Du behöver komma åt tooa Windows-baserade datorn till:</span><span class="sxs-lookup"><span data-stu-id="2b2df-120">You need access tooa Windows-based machine to:</span></span>
> 
> * <span data-ttu-id="2b2df-121">Kör verktyget för validering av hello certifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="2b2df-121">Run hello certification validation tool.</span></span>
> * <span data-ttu-id="2b2df-122">Skapa hello VHD delad åtkomst signatur URL för hello VHD certifikatutfärdare ansökan.</span><span class="sxs-lookup"><span data-stu-id="2b2df-122">Create hello VHD shared access signature URL for hello VHD certification submission.</span></span>
> 
> 

## <a name="develop-your-vhd"></a><span data-ttu-id="2b2df-123">Utveckla den virtuella Hårddisken</span><span class="sxs-lookup"><span data-stu-id="2b2df-123">Develop your VHD</span></span>
<span data-ttu-id="2b2df-124">Du kan utveckla Azure virtuella hårddiskar i hello molnet eller lokalt:</span><span class="sxs-lookup"><span data-stu-id="2b2df-124">You can develop Azure VHDs in hello cloud or on-premises:</span></span>

* <span data-ttu-id="2b2df-125">Molnbaserad utveckling innebär alla development steg utförs via fjärranslutning på en virtuell Hårddisk på Azure.</span><span class="sxs-lookup"><span data-stu-id="2b2df-125">Cloud-based development means all development steps are performed remotely on a VHD resident on Azure.</span></span>
* <span data-ttu-id="2b2df-126">Lokal utveckling kräver hämtning av en virtuell Hårddisk och utveckla den med hjälp av lokal infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="2b2df-126">On-premises development requires downloading a VHD and developing it using on-premises infrastructure.</span></span> <span data-ttu-id="2b2df-127">Även om det är möjligt, rekommenderas inte den.</span><span class="sxs-lookup"><span data-stu-id="2b2df-127">Although this is possible, we do not recommend it.</span></span> <span data-ttu-id="2b2df-128">Observera att utveckla för Windows eller SQL lokalt kräver du toohave hello relevanta lokalt licensnycklar.</span><span class="sxs-lookup"><span data-stu-id="2b2df-128">Note that developing for Windows or SQL on-premises requires you toohave hello relevant on-premises license keys.</span></span> <span data-ttu-id="2b2df-129">Du kan inte innehålla eller installera SQL Server när du har skapat en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="2b2df-129">You cannot include or install SQL Server after creating a VM.</span></span> <span data-ttu-id="2b2df-130">Erbjudandet måste också baseras på en godkända SQL-avbildning från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2b2df-130">You must also base your offer on an approved SQL image from hello Azure portal.</span></span> <span data-ttu-id="2b2df-131">Om du väljer toodevelop på lokalt, måste du utföra några steg annorlunda än om du skapar i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="2b2df-131">If you decide toodevelop on-premises, you must perform some steps differently than if you were developing in hello cloud.</span></span> <span data-ttu-id="2b2df-132">Du kan hitta relevant information i [skapa en lokal VM-avbildning](marketplace-publishing-vm-image-creation-on-premise.md).</span><span class="sxs-lookup"><span data-stu-id="2b2df-132">You can find relevant information in [Create an on-premises VM image](marketplace-publishing-vm-image-creation-on-premise.md).</span></span>

[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
