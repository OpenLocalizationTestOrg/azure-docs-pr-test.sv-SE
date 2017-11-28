---
title: "aaaConfigure CHAP för StorSimple 8000-serieenhet | Microsoft Docs"
description: "Beskriver hur tooconfigure hello CHAP Challenge Handshake Authentication Protocol () på en StorSimple-enhet."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: TBD
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: 3351184b0317da7e3deae398bc0d63c3e5bd930f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-chap-for-your-storsimple-device"></a><span data-ttu-id="b030d-103">Konfigurera CHAP för din StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="b030d-103">Configure CHAP for your StorSimple device</span></span>

<span data-ttu-id="b030d-104">Den här självstudiekursen beskrivs hur tooconfigure CHAP för din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="b030d-104">This tutorial explains how tooconfigure CHAP for your StorSimple device.</span></span> <span data-ttu-id="b030d-105">hello proceduren som beskrivs i den här artikeln gäller tooStorSimple 8000-serien enheter.</span><span class="sxs-lookup"><span data-stu-id="b030d-105">hello procedure detailed in this article applies tooStorSimple 8000 series devices.</span></span>

<span data-ttu-id="b030d-106">CHAP står för Challenge Handshake Authentication Protocol.</span><span class="sxs-lookup"><span data-stu-id="b030d-106">CHAP stands for Challenge Handshake Authentication Protocol.</span></span> <span data-ttu-id="b030d-107">Det är ett autentiseringsschema som används av servrar toovalidate hello identiteten för fjärrklienter.</span><span class="sxs-lookup"><span data-stu-id="b030d-107">It is an authentication scheme used by servers toovalidate hello identity of remote clients.</span></span> <span data-ttu-id="b030d-108">hello verifiering baseras på en delad lösenord eller hemlighet.</span><span class="sxs-lookup"><span data-stu-id="b030d-108">hello verification is based on a shared password or secret.</span></span> <span data-ttu-id="b030d-109">CHAP kan vara ett sätt (enkelriktad) eller ömsesidig (dubbelriktad).</span><span class="sxs-lookup"><span data-stu-id="b030d-109">CHAP can be one way (unidirectional) or mutual (bidirectional).</span></span> <span data-ttu-id="b030d-110">Ett sätt är CHAP när hello mål autentiserar en initierare.</span><span class="sxs-lookup"><span data-stu-id="b030d-110">One way CHAP is when hello target authenticates an initiator.</span></span> <span data-ttu-id="b030d-111">I ömsesidig eller omvänd CHAP hello mål autentiserar hello initieraren och sedan hello initieraren hello mål.</span><span class="sxs-lookup"><span data-stu-id="b030d-111">In mutual or reverse CHAP, hello target authenticates hello initiator and then hello initiator authenticates hello target.</span></span> <span data-ttu-id="b030d-112">Initieraren autentisering kan genomföras utan target autentisering.</span><span class="sxs-lookup"><span data-stu-id="b030d-112">Initiator authentication can be implemented without target authentication.</span></span> <span data-ttu-id="b030d-113">Mål-autentisering kan dock implementeras endast om initieraren authentication implementeras också.</span><span class="sxs-lookup"><span data-stu-id="b030d-113">However, target authentication can be implemented only if initiator authentication is also implemented.</span></span>

<span data-ttu-id="b030d-114">Som bästa praxis rekommenderar vi att du använder CHAP tooenhance iSCSI-säkerhet.</span><span class="sxs-lookup"><span data-stu-id="b030d-114">As a best practice, we recommend that you use CHAP tooenhance iSCSI security.</span></span>

> [!NOTE]
> <span data-ttu-id="b030d-115">Tänk på att IPSEC inte stöds för närvarande på StorSimple-enheter.</span><span class="sxs-lookup"><span data-stu-id="b030d-115">Keep in mind that IPSEC is not currently supported on StorSimple devices.</span></span>

<span data-ttu-id="b030d-116">hello CHAP-inställningar på hello StorSimple-enhet kan konfigureras i hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b030d-116">hello CHAP settings on hello StorSimple device can be configured in hello following ways:</span></span>

* <span data-ttu-id="b030d-117">Enkelriktade eller envägs-autentisering</span><span class="sxs-lookup"><span data-stu-id="b030d-117">Unidirectional or one-way authentication</span></span>
* <span data-ttu-id="b030d-118">Dubbelriktad eller ömsesidig eller omvänd autentisering</span><span class="sxs-lookup"><span data-stu-id="b030d-118">Bidirectional or mutual or reverse authentication</span></span>

<span data-ttu-id="b030d-119">I dessa fall måste hello portal hello enhet och hello serverprogramvara iSCSI-initieraren toobe konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="b030d-119">In each of these cases, hello portal for hello device and hello server iSCSI initiator software needs toobe configured.</span></span> <span data-ttu-id="b030d-120">hello beskrivs detaljerade anvisningar för den här konfigurationen i hello följa kursen.</span><span class="sxs-lookup"><span data-stu-id="b030d-120">hello detailed steps for this configuration are described in hello following tutorial.</span></span>

## <a name="unidirectional-or-one-way-authentication"></a><span data-ttu-id="b030d-121">Enkelriktade eller envägs-autentisering</span><span class="sxs-lookup"><span data-stu-id="b030d-121">Unidirectional or one-way authentication</span></span>

<span data-ttu-id="b030d-122">I enkelriktad autentisering autentiserar hello målet hello initieraren.</span><span class="sxs-lookup"><span data-stu-id="b030d-122">In unidirectional authentication, hello target authenticates hello initiator.</span></span> <span data-ttu-id="b030d-123">Den här autentiseringen måste du konfigurera inställningar för hello CHAP-initieraren på hello StorSimple-enhet och hello iSCSI-initieraren programvara på hello värden.</span><span class="sxs-lookup"><span data-stu-id="b030d-123">This authentication requires that you configure hello CHAP initiator settings on hello StorSimple device and hello iSCSI Initiator software on hello host.</span></span> <span data-ttu-id="b030d-124">Hej detaljerade anvisningar för din StorSimple-enhet och Windows-värd beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="b030d-124">hello detailed procedures for your StorSimple device and Windows host are described next.</span></span>

#### <a name="tooconfigure-your-device-for-one-way-authentication"></a><span data-ttu-id="b030d-125">tooconfigure din enhet för enkelriktad autentisering</span><span class="sxs-lookup"><span data-stu-id="b030d-125">tooconfigure your device for one-way authentication</span></span>

1. <span data-ttu-id="b030d-126">Gå tooyour StorSimple enheten Manager-tjänsten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b030d-126">In hello Azure portal, go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="b030d-127">Klicka på **enheter** och markera och klicka på en enhet som du vill tooconfigure CHAP för.</span><span class="sxs-lookup"><span data-stu-id="b030d-127">Click **Devices** and select and click a device you wish tooconfigure CHAP for.</span></span> <span data-ttu-id="b030d-128">Gå för**Enhetsinställningar > säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="b030d-128">Go too**Device settings > Security**.</span></span> <span data-ttu-id="b030d-129">I hello **säkerhetsinställningar** bladet, klickar du på **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="b030d-129">In hello **Security settings** blade, click **CHAP**.</span></span>
   
    ![CHAP-initieraren](./media/storsimple-8000-configure-chap/configure-chap5.png)
2. <span data-ttu-id="b030d-131">I hello **CHAP** bladet och i hello **CHAP-initieraren** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="b030d-131">In hello **CHAP** blade, and in hello **CHAP Initiator** section:</span></span>
   
   1. <span data-ttu-id="b030d-132">Ange ett användarnamn för CHAP-initieraren.</span><span class="sxs-lookup"><span data-stu-id="b030d-132">Provide a user name for your CHAP initiator.</span></span>
   2. <span data-ttu-id="b030d-133">Ange ett lösenord för CHAP-initieraren.</span><span class="sxs-lookup"><span data-stu-id="b030d-133">Supply a password for your CHAP initiator.</span></span>
      
    > [!IMPORTANT]
    > <span data-ttu-id="b030d-134">hello CHAP-användarnamn måste innehålla färre än 233 tecken.</span><span class="sxs-lookup"><span data-stu-id="b030d-134">hello CHAP user name must contain fewer than 233 characters.</span></span> <span data-ttu-id="b030d-135">hello CHAP-lösenord måste vara mellan 12 och 16 tecken.</span><span class="sxs-lookup"><span data-stu-id="b030d-135">hello CHAP password must be between 12 and 16 characters.</span></span> <span data-ttu-id="b030d-136">Ett längre användarnamn eller lösenord resulterar i ett autentiseringsfel på hello Windows-värd.</span><span class="sxs-lookup"><span data-stu-id="b030d-136">A longer user name or password results in an authentication failure on hello Windows host.</span></span>
   
   3. <span data-ttu-id="b030d-137">Bekräfta hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="b030d-137">Confirm hello password.</span></span>

       ![CHAP-initieraren](./media/storsimple-8000-configure-chap/configure-chap6.png)
3. <span data-ttu-id="b030d-139">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b030d-139">Click **Save**.</span></span> <span data-ttu-id="b030d-140">Ett bekräftelsemeddelande visas.</span><span class="sxs-lookup"><span data-stu-id="b030d-140">A confirmation message is displayed.</span></span> <span data-ttu-id="b030d-141">Klicka på **OK** toosave hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="b030d-141">Click **OK** toosave hello changes.</span></span>

#### <a name="tooconfigure-one-way-authentication-on-hello-windows-host-server"></a><span data-ttu-id="b030d-142">tooconfigure enkelriktad autentisering på Windows hello värdservern</span><span class="sxs-lookup"><span data-stu-id="b030d-142">tooconfigure one-way authentication on hello Windows host server</span></span>
1. <span data-ttu-id="b030d-143">Starta hello iSCSI-initieraren på värdservern för hello Windows.</span><span class="sxs-lookup"><span data-stu-id="b030d-143">On hello Windows host server, start hello iSCSI Initiator.</span></span>
2. <span data-ttu-id="b030d-144">I hello **iSCSI-Initieraregenskaper** fönstret utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b030d-144">In hello **iSCSI Initiator Properties** window, perform hello following steps:</span></span>
   
   1. <span data-ttu-id="b030d-145">Klicka på hello **identifiering** fliken.</span><span class="sxs-lookup"><span data-stu-id="b030d-145">Click hello **Discovery** tab.</span></span>
      
       ![iSCSI-initieraregenskaper](./media/storsimple-configure-chap/IC740944.png)
   2. <span data-ttu-id="b030d-147">Klicka på **identifiera Portal**.</span><span class="sxs-lookup"><span data-stu-id="b030d-147">Click **Discover Portal**.</span></span>
3. <span data-ttu-id="b030d-148">I hello **identifiera målportal** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="b030d-148">In hello **Discover Target Portal** dialog box:</span></span>
   
   1. <span data-ttu-id="b030d-149">Ange hello IP-adressen för din enhet.</span><span class="sxs-lookup"><span data-stu-id="b030d-149">Specify hello IP address of your device.</span></span>
   2. <span data-ttu-id="b030d-150">Klicka på **avancerade**.</span><span class="sxs-lookup"><span data-stu-id="b030d-150">Click **Advanced**.</span></span>
      
       ![Identifiera målportal](./media/storsimple-configure-chap/IC740945.png)
4. <span data-ttu-id="b030d-152">I hello **avancerade inställningar** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="b030d-152">In hello **Advanced Settings** dialog box:</span></span>
   
   1. <span data-ttu-id="b030d-153">Välj hello **aktivera CHAP inloggning** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="b030d-153">Select hello **Enable CHAP log on** check box.</span></span>
   2. <span data-ttu-id="b030d-154">I hello **namn** fält, ange hello användarnamnet som angetts för hello CHAP-initieraren i hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="b030d-154">In hello **Name** field, supply hello user name that you specified for hello CHAP Initiator in hello classic portal.</span></span>
   3. <span data-ttu-id="b030d-155">I hello **mål hemlighet** fält, ange hello lösenordet du angav för hello CHAP-initieraren i hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="b030d-155">In hello **Target secret** field, supply hello password that you specified for hello CHAP Initiator in hello classic portal.</span></span>
   4. <span data-ttu-id="b030d-156">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b030d-156">Click **OK**.</span></span>
      
       ![Avancerade inställningar som är allmänt](./media/storsimple-configure-chap/IC740946.png)
5. <span data-ttu-id="b030d-158">På hello **mål** för hello **iSCSI-Initieraregenskaper** fönstret hello enhetens status borde visas som **ansluten**.</span><span class="sxs-lookup"><span data-stu-id="b030d-158">On hello **Targets** tab of hello **iSCSI Initiator Properties** window, hello device status should appear as **Connected**.</span></span> <span data-ttu-id="b030d-159">Om du använder en enhet med StorSimple 1200 monterade volymerna som ett iSCSI-mål.</span><span class="sxs-lookup"><span data-stu-id="b030d-159">If you are using a StorSimple 1200 device, then each volume is mounted as an iSCSI target.</span></span> <span data-ttu-id="b030d-160">Steg 3 – 4 måste därför toobe upprepas för varje volym.</span><span class="sxs-lookup"><span data-stu-id="b030d-160">Hence, steps 3-4 will need toobe repeated for each volume.</span></span>
   
    ![Volymerna monterade som separata mål](./media/storsimple-configure-chap/chap4.png)
   
   > [!IMPORTANT]
   > <span data-ttu-id="b030d-162">Om du ändrar hello iSCSI-namn används hello nytt namn för den nya iSCSI-sessioner.</span><span class="sxs-lookup"><span data-stu-id="b030d-162">If you change hello iSCSI name, hello new name is used for new iSCSI sessions.</span></span> <span data-ttu-id="b030d-163">Nya inställningar används inte för befintliga sessioner förrän du logga ut och logga in igen.</span><span class="sxs-lookup"><span data-stu-id="b030d-163">New settings are not used for existing sessions until you log off and log on again.</span></span>

<span data-ttu-id="b030d-164">Mer information om hur du konfigurerar CHAP på värdservern med Windows hello gå för[ytterligare överväganden](#additional-considerations).</span><span class="sxs-lookup"><span data-stu-id="b030d-164">For more information about configuring CHAP on hello Windows host server, go too[Additional considerations](#additional-considerations).</span></span>

## <a name="bidirectional-or-mutual-authentication"></a><span data-ttu-id="b030d-165">Dubbelriktad eller ömsesidig autentisering</span><span class="sxs-lookup"><span data-stu-id="b030d-165">Bidirectional or mutual authentication</span></span>

<span data-ttu-id="b030d-166">I dubbelriktad autentisering hello mål autentiserar hello initieraren och sedan hello initieraren hello mål.</span><span class="sxs-lookup"><span data-stu-id="b030d-166">In bidirectional authentication, hello target authenticates hello initiator and then hello initiator authenticates hello target.</span></span> <span data-ttu-id="b030d-167">Den här proceduren kräver hello tooconfigure hello CHAP-initieraren användarinställningar, omvänd CHAP-inställningar på hello enhet och iSCSI-initieraren programvara på hello värden.</span><span class="sxs-lookup"><span data-stu-id="b030d-167">This procedure requires hello user tooconfigure hello CHAP initiator settings, reverse CHAP settings on hello device, and iSCSI Initiator software on hello host.</span></span> <span data-ttu-id="b030d-168">hello följande procedurer beskrivs hello steg tooconfigure ömsesidig autentisering på hello enheten och på Windows hello-värd.</span><span class="sxs-lookup"><span data-stu-id="b030d-168">hello following procedures describe hello steps tooconfigure mutual authentication on hello device and on hello Windows host.</span></span>

#### <a name="tooconfigure-your-device-for-mutual-authentication"></a><span data-ttu-id="b030d-169">tooconfigure enheten för ömsesidig autentisering</span><span class="sxs-lookup"><span data-stu-id="b030d-169">tooconfigure your device for mutual authentication</span></span>

1. <span data-ttu-id="b030d-170">Gå tooyour StorSimple enheten Manager-tjänsten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b030d-170">In hello Azure portal, go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="b030d-171">Klicka på **enheter** och markera och klicka på en enhet som du vill tooconfigure CHAP för.</span><span class="sxs-lookup"><span data-stu-id="b030d-171">Click **Devices** and select and click a device you wish tooconfigure CHAP for.</span></span> <span data-ttu-id="b030d-172">Gå för**Enhetsinställningar > säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="b030d-172">Go too**Device settings > Security**.</span></span> <span data-ttu-id="b030d-173">I hello **säkerhetsinställningar** bladet, klickar du på **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="b030d-173">In hello **Security settings** blade, click **CHAP**.</span></span>
   
    ![CHAP-målet](./media/storsimple-8000-configure-chap/configure-chap5.png)
2. <span data-ttu-id="b030d-175">Rulla ned på den här sidan och i hello **CHAP Target** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="b030d-175">Scroll down on this page, and in hello **CHAP Target** section:</span></span>
   
   1. <span data-ttu-id="b030d-176">Ange en **omvänd CHAP-användarnamn** för din enhet.</span><span class="sxs-lookup"><span data-stu-id="b030d-176">Provide a **Reverse CHAP user name** for your device.</span></span>
   2. <span data-ttu-id="b030d-177">Ange en **omvänd CHAP-lösenord** för din enhet.</span><span class="sxs-lookup"><span data-stu-id="b030d-177">Supply a **Reverse CHAP password** for your device.</span></span>
   3. <span data-ttu-id="b030d-178">Bekräfta hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="b030d-178">Confirm hello password.</span></span>
3. <span data-ttu-id="b030d-179">I hello **CHAP-initieraren** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="b030d-179">In hello **CHAP Initiator** section:</span></span>
   
   1. <span data-ttu-id="b030d-180">Ange en **användarnamn** för din enhet.</span><span class="sxs-lookup"><span data-stu-id="b030d-180">Provide a **user name** for your device.</span></span>
   2. <span data-ttu-id="b030d-181">Ange en **lösenord** för din enhet.</span><span class="sxs-lookup"><span data-stu-id="b030d-181">Provide a **password** for your device.</span></span>
   3. <span data-ttu-id="b030d-182">Bekräfta hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="b030d-182">Confirm hello password.</span></span>

       ![CHAP-initieraren](./media/storsimple-8000-configure-chap/configure-chap11.png)
4. <span data-ttu-id="b030d-184">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b030d-184">Click **Save**.</span></span> <span data-ttu-id="b030d-185">Ett bekräftelsemeddelande visas.</span><span class="sxs-lookup"><span data-stu-id="b030d-185">A confirmation message is displayed.</span></span> <span data-ttu-id="b030d-186">Klicka på **OK** toosave hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="b030d-186">Click **OK** toosave hello changes.</span></span>

#### <a name="tooconfigure-bidirectional-authentication-on-hello-windows-host-server"></a><span data-ttu-id="b030d-187">tooconfigure dubbelriktad autentisering på Windows hello värdservern</span><span class="sxs-lookup"><span data-stu-id="b030d-187">tooconfigure bidirectional authentication on hello Windows host server</span></span>

1. <span data-ttu-id="b030d-188">Starta hello iSCSI-initieraren på värdservern för hello Windows.</span><span class="sxs-lookup"><span data-stu-id="b030d-188">On hello Windows host server, start hello iSCSI Initiator.</span></span>
2. <span data-ttu-id="b030d-189">I hello **iSCSI-Initieraregenskaper** -fönstret klickar du på hello **Configuration** fliken.</span><span class="sxs-lookup"><span data-stu-id="b030d-189">In hello **iSCSI Initiator Properties** window, click hello **Configuration** tab.</span></span>
3. <span data-ttu-id="b030d-190">Klicka på **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="b030d-190">Click **CHAP**.</span></span>
4. <span data-ttu-id="b030d-191">I hello **iSCSI Initiator ömsesidig CHAP Secret** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="b030d-191">In hello **iSCSI Initiator Mutual CHAP Secret** dialog box:</span></span>
   
   1. <span data-ttu-id="b030d-192">Typen hello **omvänd CHAP-lösenord** som du konfigurerade i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b030d-192">Type hello **Reverse CHAP Password** that you configured in hello Azure portal.</span></span>
   2. <span data-ttu-id="b030d-193">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b030d-193">Click **OK**.</span></span>
      
       ![iSCSI-initieraren ömsesidig CHAP-hemlighet](./media/storsimple-configure-chap/IC740949.png)
5. <span data-ttu-id="b030d-195">Klicka på hello **mål** fliken.</span><span class="sxs-lookup"><span data-stu-id="b030d-195">Click hello **Targets** tab.</span></span>
6. <span data-ttu-id="b030d-196">Klicka på hello **Anslut** knappen.</span><span class="sxs-lookup"><span data-stu-id="b030d-196">Click hello **Connect** button.</span></span> 
7. <span data-ttu-id="b030d-197">I hello **ansluta tooTarget** dialogrutan klickar du på **Avancerat**.</span><span class="sxs-lookup"><span data-stu-id="b030d-197">In hello **Connect tooTarget** dialog box, click **Advanced**.</span></span>
8. <span data-ttu-id="b030d-198">I hello **avancerade egenskaper** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="b030d-198">In hello **Advanced Properties** dialog box:</span></span>
   
   1. <span data-ttu-id="b030d-199">Välj hello **aktivera CHAP inloggning** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="b030d-199">Select hello **Enable CHAP log on** check box.</span></span>
   2. <span data-ttu-id="b030d-200">I hello **namn** fält, ange hello användarnamnet som angetts för hello CHAP-initieraren i hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="b030d-200">In hello **Name** field, supply hello user name that you specified for hello CHAP Initiator in hello classic portal.</span></span>
   3. <span data-ttu-id="b030d-201">I hello **mål hemlighet** fält, ange hello lösenordet du angav för hello CHAP-initieraren i hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="b030d-201">In hello **Target secret** field, supply hello password that you specified for hello CHAP Initiator in hello classic portal.</span></span>
   4. <span data-ttu-id="b030d-202">Välj hello **utför ömsesidig autentisering** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="b030d-202">Select hello **Perform mutual authentication** check box.</span></span>
      
       ![Avancerade inställningar ömsesidig autentisering](./media/storsimple-configure-chap/IC740950.png)
   5. <span data-ttu-id="b030d-204">Klicka på **OK** toocomplete hello CHAP-konfiguration</span><span class="sxs-lookup"><span data-stu-id="b030d-204">Click **OK** toocomplete hello CHAP configuration</span></span>

<span data-ttu-id="b030d-205">Mer information om hur du konfigurerar CHAP på värdservern med Windows hello gå för[ytterligare överväganden](#additional-considerations).</span><span class="sxs-lookup"><span data-stu-id="b030d-205">For more information about configuring CHAP on hello Windows host server, go too[Additional considerations](#additional-considerations).</span></span>

## <a name="additional-considerations"></a><span data-ttu-id="b030d-206">Annat som är bra att tänka på</span><span class="sxs-lookup"><span data-stu-id="b030d-206">Additional considerations</span></span>

<span data-ttu-id="b030d-207">Hej **Snabbanslut** funktionen har inte stöd för anslutningar som har aktiverat CHAP.</span><span class="sxs-lookup"><span data-stu-id="b030d-207">hello **Quick Connect** feature does not support connections that have CHAP enabled.</span></span> <span data-ttu-id="b030d-208">När CHAP är aktiverat, kontrollera att du använder hello **Anslut** knapp som finns på hello **mål** fliken tooconnect tooa mål.</span><span class="sxs-lookup"><span data-stu-id="b030d-208">When CHAP is enabled, make sure that you use hello **Connect** button that is available on hello **Targets** tab tooconnect tooa target.</span></span>

![Ansluta tootarget](./media/storsimple-configure-chap/IC740947.png)

<span data-ttu-id="b030d-210">I hello **ansluta tooTarget** dialogruta som presenteras, Välj hello **lägga till den här anslutningen toohello lista över favoritmål** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="b030d-210">In hello **Connect tooTarget** dialog box that is presented, select hello **Add this connection toohello list of Favorite Targets** check box.</span></span> <span data-ttu-id="b030d-211">Det här valet garanterar att varje gång hello datorn startas om ett försök görs toorestore hello anslutning toohello iSCSI-favorit mål.</span><span class="sxs-lookup"><span data-stu-id="b030d-211">This selection ensures that every time hello computer restarts, an attempt is made toorestore hello connection toohello iSCSI favorite targets.</span></span>

## <a name="errors-during-configuration"></a><span data-ttu-id="b030d-212">Fel under konfigurationen</span><span class="sxs-lookup"><span data-stu-id="b030d-212">Errors during configuration</span></span>

<span data-ttu-id="b030d-213">Om CHAP-konfigurationen är felaktig så kan du förmodligen toosee en **autentiseringsfel** felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="b030d-213">If your CHAP configuration is incorrect, then you are likely toosee an **Authentication failure** error message.</span></span>

## <a name="verification-of-chap-configuration"></a><span data-ttu-id="b030d-214">Verifiering av CHAP-konfiguration</span><span class="sxs-lookup"><span data-stu-id="b030d-214">Verification of CHAP configuration</span></span>

<span data-ttu-id="b030d-215">Du kan verifiera att CHAP används genom att slutföra hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="b030d-215">You can verify that CHAP is being used by completing hello following steps.</span></span>

#### <a name="tooverify-your-chap-configuration"></a><span data-ttu-id="b030d-216">tooverify CHAP-konfiguration</span><span class="sxs-lookup"><span data-stu-id="b030d-216">tooverify your CHAP configuration</span></span>
1. <span data-ttu-id="b030d-217">Klicka på **favorit mål**.</span><span class="sxs-lookup"><span data-stu-id="b030d-217">Click **Favorite Targets**.</span></span>
2. <span data-ttu-id="b030d-218">Välj hello målobjekt som du har aktiverat autentisering.</span><span class="sxs-lookup"><span data-stu-id="b030d-218">Select hello target for which you enabled authentication.</span></span>
3. <span data-ttu-id="b030d-219">Klicka på **information**.</span><span class="sxs-lookup"><span data-stu-id="b030d-219">Click **Details**.</span></span>
   
    ![iSCSI-initieraren egenskaper favorit mål](./media/storsimple-configure-chap/IC740951.png)
4. <span data-ttu-id="b030d-221">I hello **information om favorit mål** dialogrutan hello kommentar i hello **autentisering** fältet.</span><span class="sxs-lookup"><span data-stu-id="b030d-221">In hello **Favorite Target Details** dialog box, note hello entry in hello **Authentication** field.</span></span> <span data-ttu-id="b030d-222">Om hello konfigurationen lyckades, ska det stå **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="b030d-222">If hello configuration was successful, it should say **CHAP**.</span></span>
   
    ![Information om favorit mål](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a><span data-ttu-id="b030d-224">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b030d-224">Next steps</span></span>

* <span data-ttu-id="b030d-225">Lär dig mer om [StorSimple-säkerhet](storsimple-8000-security.md).</span><span class="sxs-lookup"><span data-stu-id="b030d-225">Learn more about [StorSimple security](storsimple-8000-security.md).</span></span>
* <span data-ttu-id="b030d-226">Lär dig mer om [med hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="b030d-226">Learn more about [using hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

