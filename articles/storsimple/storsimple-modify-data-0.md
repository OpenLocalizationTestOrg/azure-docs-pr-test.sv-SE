---
title: "aaaModify hello DATA 0-inställningar på en StorSimple-enhet | Microsoft Docs"
description: "Lär dig hur toouse Windows PowerShell för StorSimple tooreconfigure hello DATA 0-nätverksgränssnittet på din StorSimple-enhet."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 58e3d509-f425-4a7f-b650-d496a7c85193
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: caec51c3344d953299253301c2a0d7577d553c6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="modify-hello-data-0-network-interface-settings-on-your-storsimple-device"></a><span data-ttu-id="62f59-103">Ändra hello DATA 0 inställningar för nätverksgränssnittet på din StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="62f59-103">Modify hello DATA 0 network interface settings on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="62f59-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="62f59-104">Overview</span></span>
<span data-ttu-id="62f59-105">Microsoft Azure StorSimple-enheten har sex nätverksgränssnitt från DATA 0 tooDATA 5.</span><span class="sxs-lookup"><span data-stu-id="62f59-105">Your Microsoft Azure StorSimple device has six network interfaces, from DATA 0 tooDATA 5.</span></span> <span data-ttu-id="62f59-106">hello DATA 0 gränssnittet är alltid konfigurerad via Windows PowerShell-gränssnittet för hello eller hello seriekonsolen och är automatiskt moln-aktiverat.</span><span class="sxs-lookup"><span data-stu-id="62f59-106">hello DATA 0 interface is always configured through hello Windows PowerShell interface or hello serial console, and is automatically cloud-enabled.</span></span> <span data-ttu-id="62f59-107">Observera att du kan konfigurera DATA 0-nätverksgränssnittet via hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="62f59-107">Note that you cannot configure DATA 0 network interface through hello Azure classic portal.</span></span> 

<span data-ttu-id="62f59-108">hello DATA 0 gränssnittet konfigureras först via hello installationsguiden under första distributionen av hello StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="62f59-108">hello DATA 0 interface is first configured through hello setup wizard during initial deployment of hello StorSimple device.</span></span> <span data-ttu-id="62f59-109">När hello enhet är i ett arbetsläge kan du behöva tooreconfigure DATA 0 inställningar.</span><span class="sxs-lookup"><span data-stu-id="62f59-109">When hello device is in an operational mode, you may need tooreconfigure DATA 0 settings.</span></span> <span data-ttu-id="62f59-110">Den här kursen erbjuder två metoder toomodify DATA 0 nätverksinställningar, både via Windows PowerShell för StorSimple.</span><span class="sxs-lookup"><span data-stu-id="62f59-110">This tutorial provides two methods toomodify DATA 0 network settings, both through Windows PowerShell for StorSimple.</span></span>

<span data-ttu-id="62f59-111">När du har läst den här självstudiekursen kommer du att kunna:</span><span class="sxs-lookup"><span data-stu-id="62f59-111">After reading this tutorial, you will be able to:</span></span>

* <span data-ttu-id="62f59-112">Ändra DATA 0 nätverk via hello installationsguiden</span><span class="sxs-lookup"><span data-stu-id="62f59-112">Modify DATA 0 network setting through hello setup wizard</span></span>
* <span data-ttu-id="62f59-113">Ändra DATA 0 nätverksinställningar via hello `Set-HcsNetInterface` cmdlet</span><span class="sxs-lookup"><span data-stu-id="62f59-113">Modify DATA 0 network settings through hello `Set-HcsNetInterface` cmdlet</span></span>

## <a name="modify-data-0-network-settings-through-setup-wizard"></a><span data-ttu-id="62f59-114">Ändra DATA 0 nätverksinställningar via installationsguiden</span><span class="sxs-lookup"><span data-stu-id="62f59-114">Modify DATA 0 network settings through setup wizard</span></span>
<span data-ttu-id="62f59-115">Du kan konfigurera om DATA 0 nätverksinställningar genom att ansluta toohello Windows PowerShell-gränssnittet av StorSimple-enheten och starta en session för installationsprogrammet guiden.</span><span class="sxs-lookup"><span data-stu-id="62f59-115">You can reconfigure DATA 0 network settings by connecting toohello Windows PowerShell interface of your StorSimple device and launching a setup wizard session.</span></span> <span data-ttu-id="62f59-116">Utför följande steg toomodify DATA 0 hello inställningar:</span><span class="sxs-lookup"><span data-stu-id="62f59-116">Perform hello following steps toomodify DATA 0 settings:</span></span>

#### <a name="toomodify-data-0-network-settings-through-setup-wizard"></a><span data-ttu-id="62f59-117">toomodify DATA 0 nätverksinställningar via installationsguiden</span><span class="sxs-lookup"><span data-stu-id="62f59-117">toomodify DATA 0 network settings through setup wizard</span></span>
1. <span data-ttu-id="62f59-118">I menyn för seriekonsolen av hello väljer du alternativ 1, **logga in med fullständig åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="62f59-118">In hello serial console menu, choose option 1, **Log in with full access**.</span></span> <span data-ttu-id="62f59-119">När du uppmanas ange hello **enhetens administratörslösenord**.</span><span class="sxs-lookup"><span data-stu-id="62f59-119">When prompted provide hello **device administrator password**.</span></span> <span data-ttu-id="62f59-120">hello standardlösenordet är `Password1`.</span><span class="sxs-lookup"><span data-stu-id="62f59-120">hello default password is `Password1`.</span></span>
2. <span data-ttu-id="62f59-121">I hello kommandotolk, skriver du:</span><span class="sxs-lookup"><span data-stu-id="62f59-121">At hello command prompt, type:</span></span>
   
    `Invoke-HcsSetupWizard`
3. <span data-ttu-id="62f59-122">En installationsguide kommer visas toohelp som du konfigurerar hello DATA 0-gränssnittet på enheten.</span><span class="sxs-lookup"><span data-stu-id="62f59-122">A setup wizard will appear toohelp you configure hello DATA 0 interface of your device.</span></span> <span data-ttu-id="62f59-123">Ange nya värden för hello IP-adress, -gateway och nätmask.</span><span class="sxs-lookup"><span data-stu-id="62f59-123">Provide new values for hello IP address, gateway, and netmask.</span></span>

> [!NOTE]
> <span data-ttu-id="62f59-124">hello fast domänkontrollanter IP-adresser måste konfigureras via hello toobe **konfigurera** sidan av hello StorSimple-enhet i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="62f59-124">hello fixed controllers IPs will need toobe reconfigured through hello **Configure** page of hello StorSimple device in hello Azure classic portal.</span></span> <span data-ttu-id="62f59-125">Mer information finns för[ändra nätverksgränssnitt](storsimple-modify-device-config.md#modify-network-interfaces).</span><span class="sxs-lookup"><span data-stu-id="62f59-125">For more information, go too[Modify network interfaces](storsimple-modify-device-config.md#modify-network-interfaces).</span></span>
> 
> 

## <a name="modify-data-0-network-settings-through-set-hcsnetinterface-cmdlet"></a><span data-ttu-id="62f59-126">Ändra DATA 0 nätverksinställningar via cmdlet Set-HcsNetInterface</span><span class="sxs-lookup"><span data-stu-id="62f59-126">Modify DATA 0 network settings through Set-HcsNetInterface cmdlet</span></span>
<span data-ttu-id="62f59-127">Ett annat sätt tooreconfigure DATA 0-nätverksgränssnittet är via hello hello `Set-HcsNetInterface` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="62f59-127">An alternate way tooreconfigure DATA 0 network interface is through hello use of  hello `Set-HcsNetInterface` cmdlet.</span></span> <span data-ttu-id="62f59-128">hello cmdlet körs från hello Windows PowerShell-gränssnittet för din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="62f59-128">hello cmdlet is executed from hello Windows PowerShell interface of your StorSimple device.</span></span> <span data-ttu-id="62f59-129">När du använder den här proceduren kan hello styrenhets-fästa IP-adresser även konfigureras här.</span><span class="sxs-lookup"><span data-stu-id="62f59-129">When using this procedure, hello controller fixed IPs can also be configured here.</span></span> <span data-ttu-id="62f59-130">Utför följande steg toomodify hello DATA 0 hello inställningar:</span><span class="sxs-lookup"><span data-stu-id="62f59-130">Perform hello following steps toomodify hello DATA 0 settings:</span></span> 

#### <a name="toomodify-data-0-network-settings-through-hello-set-hcsnetinterface-cmdlet"></a><span data-ttu-id="62f59-131">toomodify DATA 0 nätverksinställningar via hello Set-HcsNetInterface cmdlet</span><span class="sxs-lookup"><span data-stu-id="62f59-131">toomodify DATA 0 network settings through hello Set-HcsNetInterface cmdlet</span></span>
1. <span data-ttu-id="62f59-132">I menyn för seriekonsolen av hello väljer du alternativ 1, **logga in med fullständig åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="62f59-132">In hello serial console menu, choose option 1, **Log in with full access**.</span></span> <span data-ttu-id="62f59-133">När du uppmanas ange hello enhetens administratörslösenord.</span><span class="sxs-lookup"><span data-stu-id="62f59-133">When prompted provide hello device administrator password.</span></span> <span data-ttu-id="62f59-134">hello standardlösenordet är `Password1`.</span><span class="sxs-lookup"><span data-stu-id="62f59-134">hello default password is `Password1`.</span></span>
2. <span data-ttu-id="62f59-135">I hello kommandotolk, skriver du:</span><span class="sxs-lookup"><span data-stu-id="62f59-135">At hello command prompt, type:</span></span>
   
    `Set-HCSNetInterface -InterfaceAlias Data0 -IPv4Address <> -IPv4Netmask <> -IPv4Gateway <> -Controller0IPv4Address <> -Controller1IPv4Address <> -IsiScsiEnabled 1 -IsCloudEnabled 1`
   
    <span data-ttu-id="62f59-136">Skriv följande värden för DATA 0 hello i hello vinkel hakparenteser:</span><span class="sxs-lookup"><span data-stu-id="62f59-136">In hello angled brackets, type hello following values for DATA 0:</span></span>
   
   * <span data-ttu-id="62f59-137">IPv4-adress</span><span class="sxs-lookup"><span data-stu-id="62f59-137">IPv4 address</span></span>
   * <span data-ttu-id="62f59-138">IPv4-gateway</span><span class="sxs-lookup"><span data-stu-id="62f59-138">IPv4 gateway</span></span>
   * <span data-ttu-id="62f59-139">IPv4-nätmask</span><span class="sxs-lookup"><span data-stu-id="62f59-139">IPv4 subnet mask</span></span>
   * <span data-ttu-id="62f59-140">Fast IPv4-adressen för styrenhet 0</span><span class="sxs-lookup"><span data-stu-id="62f59-140">Fixed IPv4 address for Controller 0</span></span>
   * <span data-ttu-id="62f59-141">Fast IPv4-adressen för styrenhet 1</span><span class="sxs-lookup"><span data-stu-id="62f59-141">Fixed IPv4 address for Controller 1</span></span>
     
     <span data-ttu-id="62f59-142">Mer information om hello använder denna cmdlet finns för[Windows PowerShell för StorSimple cmdlet-referens](https://technet.microsoft.com/library/dn688161.aspx).</span><span class="sxs-lookup"><span data-stu-id="62f59-142">For more information on hello use of this cmdlet, go too[Windows PowerShell for StorSimple cmdlet reference](https://technet.microsoft.com/library/dn688161.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="62f59-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="62f59-143">Next steps</span></span>
* <span data-ttu-id="62f59-144">tooconfigure nätverksgränssnitt än DATA 0, kan du använda hello [konfigurationssidan i hello klassiska Azure-portalen](storsimple-modify-device-config.md).</span><span class="sxs-lookup"><span data-stu-id="62f59-144">tooconfigure network interfaces other than DATA 0, you can use hello [Configure page in hello Azure classic portal](storsimple-modify-device-config.md).</span></span> 
* <span data-ttu-id="62f59-145">Om du får problem när du konfigurerar din nätverksgränssnitt finns för[felsöka distributionsproblem](storsimple-troubleshoot-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="62f59-145">If you experience any issues when configuring your network interfaces, refer too[Troubleshoot deployment issues](storsimple-troubleshoot-deployment.md).</span></span>

