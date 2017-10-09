---
title: "aaaHow toomigrate från RemoteApp VNET-tooan Azure VNET | Microsoft Docs"
description: "Lär dig hur toomigrate från RemoteApp VNET-tooan Azure VNET"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: baea5d29-353b-48f8-b47f-806f2163e067
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: c0f8617556c6f1e33eca8322febf67ff33937ecd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomigrate-a-hybrid-collection-from-a-remoteapp-vnet-tooan-azure-vnet"></a><span data-ttu-id="8e72b-103">Hur toomigrate en hybridsamling från en RemoteApp VNET tooan Azure VNET</span><span class="sxs-lookup"><span data-stu-id="8e72b-103">How toomigrate a hybrid collection from a RemoteApp VNET tooan Azure VNET</span></span>
> [!IMPORTANT]
> <span data-ttu-id="8e72b-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="8e72b-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="8e72b-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="8e72b-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="8e72b-106">Goda nyheter!</span><span class="sxs-lookup"><span data-stu-id="8e72b-106">Good news!</span></span> <span data-ttu-id="8e72b-107">Vi har aktiverat toodeploy hybrid RemoteApp-samlingar direkt i din befintliga virtuella Azure-nätverk (Vnet) i stället för att skapa RemoteApp-specifika Vnet.</span><span class="sxs-lookup"><span data-stu-id="8e72b-107">We have enabled you toodeploy hybrid RemoteApp collections directly into your existing Azure virtual networks (VNETs) instead of creating RemoteApp-specific VNETs.</span></span> <span data-ttu-id="8e72b-108">Detta kan du dra nytta av hello senaste VNET-funktioner (till exempel ExpressRoute) och ge din hybrid samlingar direkt network access tooother Azure-tjänster och virtuella datorer distribueras toothat VNET.</span><span class="sxs-lookup"><span data-stu-id="8e72b-108">This lets you take advantage of hello latest VNET features (like ExpressRoute) and give your hybrid collections direct network access tooother Azure services and virtual machines deployed toothat VNET.</span></span>  <span data-ttu-id="8e72b-109">(Det här får du bättre prestanda och enklare konfiguration än VNET-till-VNET-konfigurationer).</span><span class="sxs-lookup"><span data-stu-id="8e72b-109">(This gets you better performance and easier setup than VNET-to-VNET configurations).</span></span>

<span data-ttu-id="8e72b-110">Anta att du redan har skapat en hybrid RemoteApp-samling som heter *OriginalCollection* med en RemoteApp-VNET kallas *RemoteAppVNET*.</span><span class="sxs-lookup"><span data-stu-id="8e72b-110">Let’s say that you’ve already created a hybrid RemoteApp collection called *OriginalCollection* with a RemoteApp VNET called *RemoteAppVNET*.</span></span> <span data-ttu-id="8e72b-111">Här följer hello steg toomigrate den tooa nytt Azure VNET kallas *AzureVNET*.</span><span class="sxs-lookup"><span data-stu-id="8e72b-111">Here are hello steps toomigrate it tooa new Azure VNET called *AzureVNET*.</span></span>

1. <span data-ttu-id="8e72b-112">På hello **nätverk** fliken i hello [hanteringsportalen](http://manage.windowsazure.com/), skapa ett virtuellt nätverk kallas *AzureVNET*med hjälp av hello samma plats, DNS-konfigurationen och adressutrymme (för minst en för hello *AzureVNET* undernät) som du använde för *RemoteAppVNET*.</span><span class="sxs-lookup"><span data-stu-id="8e72b-112">On hello **Networks** tab in hello [management portal](http://manage.windowsazure.com/), create a VNET called *AzureVNET*, using hello same location, DNS configuration, and address space (for at least one of hello *AzureVNET* subnets) as you used for *RemoteAppVNET*.</span></span>
2. <span data-ttu-id="8e72b-113">Konfigurera *AzureVNET* tooeither värd eller har network connectivity toohello Active Directory-distribution som *OriginalCollection* är domänen som är ansluten till.</span><span class="sxs-lookup"><span data-stu-id="8e72b-113">Configure *AzureVNET* tooeither host or have network connectivity toohello Active Directory deployment that *OriginalCollection* is domain joined to.</span></span>
3. <span data-ttu-id="8e72b-114">På hello **RemoteApp-program** fliken, skapa en ny RemoteApp-samling som heter *ny samling*.</span><span class="sxs-lookup"><span data-stu-id="8e72b-114">On hello **RemoteApps** tab, create a new RemoteApp collection called *New Collection*.</span></span> <span data-ttu-id="8e72b-115">(Använd hello **skapa med VNET** alternativet inte **Snabbregistrering**.)</span><span class="sxs-lookup"><span data-stu-id="8e72b-115">(Use hello **Create with VNET** option, not **Quick Create**.)</span></span>
4. <span data-ttu-id="8e72b-116">Konfigurera *NewCollection* toobe distribueras tooa undernät i *AzureVNET*.</span><span class="sxs-lookup"><span data-stu-id="8e72b-116">Configure *NewCollection* toobe deployed tooa subnet in *AzureVNET*.</span></span>
5. <span data-ttu-id="8e72b-117">Konfigurera *NewCollection* toouse hello samma avbildning och domänanslutningsinformationen som du använde för *OriginalCollection*.</span><span class="sxs-lookup"><span data-stu-id="8e72b-117">Configure *NewCollection* toouse hello same image and domain join information as you used for *OriginalCollection*.</span></span>
6. <span data-ttu-id="8e72b-118">Efter några timmar *NewCollection* visas i samlingslistan med aktiv.</span><span class="sxs-lookup"><span data-stu-id="8e72b-118">After a few hours, *NewCollection* will show up in your collection list with an Active state.</span></span>

<span data-ttu-id="8e72b-119">Nu om du inte behöver toomigrate användarinformation från hello ursprungliga samlingen toohello ny samling, göra de här stegen sedan:</span><span class="sxs-lookup"><span data-stu-id="8e72b-119">Now, if you DON’T need toomigrate any user information from hello original collection toohello new collection, do these steps next:</span></span>

1. <span data-ttu-id="8e72b-120">Ta bort *OriginalCollection*.</span><span class="sxs-lookup"><span data-stu-id="8e72b-120">Delete *OriginalCollection*.</span></span>
2. <span data-ttu-id="8e72b-121">Ta bort *RemoteAppVNET*.</span><span class="sxs-lookup"><span data-stu-id="8e72b-121">Delete *RemoteAppVNET*.</span></span>

<span data-ttu-id="8e72b-122">Och du är klar!</span><span class="sxs-lookup"><span data-stu-id="8e72b-122">And, you’re done!</span></span>

<span data-ttu-id="8e72b-123">Alternativt, om du behöver toomigrate användarinformation från hello ursprungliga samlingen toohello ny samling, göra de här stegen sedan:</span><span class="sxs-lookup"><span data-stu-id="8e72b-123">Alternately, if you DO need toomigrate user information from hello original collection toohello new collection, do these steps next:</span></span>

1. <span data-ttu-id="8e72b-124">Skicka ett e-postmeddelande för[ remoteappforum@microsoft.com ](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20user%20information%20migration) hello namnet på din ursprungliga samling och hello namnet på den nya samlingen med ditt Azure prenumerations-ID och be dem toomigrate din användarinformation.</span><span class="sxs-lookup"><span data-stu-id="8e72b-124">Send an email too[remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20user%20information%20migration) with your Azure subscription ID, hello name of your original collection, and hello name of your new collection, and ask them toomigrate your user information.</span></span>
2. <span data-ttu-id="8e72b-125">Inom 2 arbetsdagar flyttar hello RemoteApp-teamet hello användarlistan för åtkomst och alla användardokument och användarinställningar från hello ursprungliga samlingen toohello ny samling.</span><span class="sxs-lookup"><span data-stu-id="8e72b-125">Within 2 business days hello RemoteApp team will move hello user access list and all user documents and user settings from hello original collection toohello new collection.</span></span>
3. <span data-ttu-id="8e72b-126">Ta bort *OriginalCollection*.</span><span class="sxs-lookup"><span data-stu-id="8e72b-126">Delete *OriginalCollection*.</span></span>
4. <span data-ttu-id="8e72b-127">Ta bort *RemoteAppVNET*.</span><span class="sxs-lookup"><span data-stu-id="8e72b-127">Delete *RemoteAppVNET*.</span></span>

<span data-ttu-id="8e72b-128">Och du är nu klar!</span><span class="sxs-lookup"><span data-stu-id="8e72b-128">And now, you’re done!</span></span>

<span data-ttu-id="8e72b-129">Om du har några frågor eller behöver hjälp med särskilda e-postmeddelande till [ remoteappforum@microsoft.com ](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20VNET%20migration%20help).</span><span class="sxs-lookup"><span data-stu-id="8e72b-129">If you have any questions or need special assistance, please email [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20VNET%20migration%20help).</span></span>

