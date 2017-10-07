---
title: "aaaConfigure CHAP för StorSimple 8000-serieenhet | Microsoft Docs"
description: "Beskriver hur tooconfigure hello CHAP Challenge Handshake Authentication Protocol () på en StorSimple-enhet."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 467044d7-7885-4382-90bd-3148dbbd341f
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 272ef2c184f56ad262e55410357494c72e45cf83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-chap-for-your-storsimple-device"></a><span data-ttu-id="5fb2f-103">Konfigurera CHAP för din StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="5fb2f-103">Configure CHAP for your StorSimple device</span></span>
<span data-ttu-id="5fb2f-104">Den här självstudiekursen beskrivs hur tooconfigure CHAP för din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-104">This tutorial explains how tooconfigure CHAP for your StorSimple device.</span></span> <span data-ttu-id="5fb2f-105">hello proceduren som beskrivs i den här artikeln gäller tooStorSimple 8000-serien samt 1200 StorSimple-enheter.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-105">hello procedure detailed in this article applies tooStorSimple 8000 series as well as StorSimple 1200 devices.</span></span>

<span data-ttu-id="5fb2f-106">CHAP står för Challenge Handshake Authentication Protocol.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-106">CHAP stands for Challenge Handshake Authentication Protocol.</span></span> <span data-ttu-id="5fb2f-107">Det är ett autentiseringsschema som används av servrar toovalidate hello identiteten för fjärrklienter.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-107">It is an authentication scheme used by servers toovalidate hello identity of remote clients.</span></span> <span data-ttu-id="5fb2f-108">hello verifiering baseras på en delad lösenord eller hemlighet.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-108">hello verification is based on a shared password or secret.</span></span> <span data-ttu-id="5fb2f-109">CHAP kan vara enkelriktade (enkelriktad) eller ömsesidig (dubbelriktad).</span><span class="sxs-lookup"><span data-stu-id="5fb2f-109">CHAP can be one-way (unidirectional) or mutual (bidirectional).</span></span> <span data-ttu-id="5fb2f-110">Enkelriktade CHAP är när hello mål autentiserar en initierare.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-110">One-way CHAP is when hello target authenticates an initiator.</span></span> <span data-ttu-id="5fb2f-111">Ömsesidig eller omvänd CHAP hello däremot måste hello mål autentisera hello initieraren och sedan hello initieraren autentisera hello mål.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-111">Mutual or reverse CHAP, on hello other hand, requires that hello target authenticate hello initiator and then hello initiator authenticate hello target.</span></span> <span data-ttu-id="5fb2f-112">Initieraren autentisering kan genomföras utan target autentisering.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-112">Initiator authentication can be implemented without target authentication.</span></span> <span data-ttu-id="5fb2f-113">Mål-autentisering kan dock implementeras endast om initieraren authentication implementeras också.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-113">However, target authentication can be implemented only if initiator authentication is also implemented.</span></span> 

<span data-ttu-id="5fb2f-114">Som bästa praxis rekommenderar vi att du använder CHAP tooenhance iSCSI-säkerhet.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-114">As a best practice, we recommend that you use CHAP tooenhance iSCSI security.</span></span>

> [!NOTE]
> <span data-ttu-id="5fb2f-115">Tänk på att IPSEC inte stöds för närvarande på StorSimple-enheter.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-115">Keep in mind that IPSEC is not currently supported on StorSimple devices.</span></span>
> 
> 

<span data-ttu-id="5fb2f-116">hello CHAP-inställningar på hello StorSimple-enhet kan konfigureras i hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="5fb2f-116">hello CHAP settings on hello StorSimple device can be configured in hello following ways:</span></span>

* <span data-ttu-id="5fb2f-117">Enkelriktade eller envägs-autentisering</span><span class="sxs-lookup"><span data-stu-id="5fb2f-117">Unidirectional or one-way authentication</span></span>
* <span data-ttu-id="5fb2f-118">Dubbelriktad eller ömsesidig eller omvänd autentisering</span><span class="sxs-lookup"><span data-stu-id="5fb2f-118">Bidirectional or mutual or reverse authentication</span></span>

<span data-ttu-id="5fb2f-119">I dessa fall måste hello portal hello enhet och hello serverprogramvara iSCSI-initieraren toobe konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-119">In each of these cases, hello portal for hello device and hello server iSCSI initiator software needs toobe configured.</span></span> <span data-ttu-id="5fb2f-120">hello beskrivs detaljerade anvisningar för den här konfigurationen i hello följa kursen.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-120">hello detailed steps for this configuration are described in hello following tutorial.</span></span>

## <a name="unidirectional-or-one-way-authentication"></a><span data-ttu-id="5fb2f-121">Enkelriktade eller envägs-autentisering</span><span class="sxs-lookup"><span data-stu-id="5fb2f-121">Unidirectional or one-way authentication</span></span>
<span data-ttu-id="5fb2f-122">I enkelriktad autentisering autentiserar hello målet hello initieraren.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-122">In unidirectional authentication, hello target authenticates hello initiator.</span></span> <span data-ttu-id="5fb2f-123">Den här autentiseringen måste du konfigurera inställningar för hello CHAP-initieraren på hello StorSimple-enhet och hello iSCSI-initieraren programvara på hello värden.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-123">This authentication requires that you configure hello CHAP initiator settings on hello StorSimple device and hello iSCSI Initiator software on hello host.</span></span> <span data-ttu-id="5fb2f-124">Hej detaljerade anvisningar för din StorSimple-enhet och Windows-värd beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-124">hello detailed procedures for your StorSimple device and Windows host are described next.</span></span>

#### <a name="tooconfigure-your-device-for-one-way-authentication"></a><span data-ttu-id="5fb2f-125">tooconfigure din enhet för enkelriktad autentisering</span><span class="sxs-lookup"><span data-stu-id="5fb2f-125">tooconfigure your device for one-way authentication</span></span>
1. <span data-ttu-id="5fb2f-126">I hello klassiska Azure-portalen på hello **enheter** klickar du på hello **konfigurera** fliken.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-126">In hello Azure classic portal, on hello **Devices** page, click hello **Configure** tab.</span></span>
   
    ![CHAP-initieraren](./media/storsimple-configure-chap/IC740943.png)
2. <span data-ttu-id="5fb2f-128">Rulla ned på den här sidan och i hello **CHAP-initieraren** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="5fb2f-128">Scroll down on this page, and in hello **CHAP Initiator** section:</span></span>
   
   1. <span data-ttu-id="5fb2f-129">Ange ett användarnamn för CHAP-initieraren.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-129">Provide a user name for your CHAP initiator.</span></span>
   2. <span data-ttu-id="5fb2f-130">Ange ett lösenord för CHAP-initieraren.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-130">Supply a password for your CHAP initiator.</span></span>
      
    > [!IMPORTANT]
    > <span data-ttu-id="5fb2f-131">hello CHAP-användarnamn måste innehålla färre än 233 tecken.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-131">hello CHAP user name must contain fewer than 233 characters.</span></span> <span data-ttu-id="5fb2f-132">hello CHAP-lösenord måste vara mellan 12 och 16 tecken.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-132">hello CHAP password must be between 12 and 16 characters.</span></span> <span data-ttu-id="5fb2f-133">Ett längre användarnamn eller lösenord resulterar i ett autentiseringsfel på hello Windows-värd.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-133">A longer user name or password will result in an authentication failure on hello Windows host.</span></span>
   
   3. <span data-ttu-id="5fb2f-134">Bekräfta hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-134">Confirm hello password.</span></span>
3. <span data-ttu-id="5fb2f-135">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-135">Click **Save**.</span></span> <span data-ttu-id="5fb2f-136">Att kommer visas ett bekräftelsemeddelande.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-136">A confirmation message will be displayed.</span></span> <span data-ttu-id="5fb2f-137">Klicka på **OK** toosave hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-137">Click **OK** toosave hello changes.</span></span>

#### <a name="tooconfigure-one-way-authentication-on-hello-windows-host-server"></a><span data-ttu-id="5fb2f-138">tooconfigure enkelriktad autentisering på Windows hello värdservern</span><span class="sxs-lookup"><span data-stu-id="5fb2f-138">tooconfigure one-way authentication on hello Windows host server</span></span>
1. <span data-ttu-id="5fb2f-139">Starta hello iSCSI-initieraren på värdservern för hello Windows.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-139">On hello Windows host server, start hello iSCSI Initiator.</span></span>
2. <span data-ttu-id="5fb2f-140">I hello **iSCSI-Initieraregenskaper** fönstret utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5fb2f-140">In hello **iSCSI Initiator Properties** window, perform hello following steps:</span></span>
   
   1. <span data-ttu-id="5fb2f-141">Klicka på hello **identifiering** fliken.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-141">Click hello **Discovery** tab.</span></span>
      
       ![iSCSI-initieraregenskaper](./media/storsimple-configure-chap/IC740944.png)
   2. <span data-ttu-id="5fb2f-143">Klicka på **identifiera Portal**.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-143">Click **Discover Portal**.</span></span>
3. <span data-ttu-id="5fb2f-144">I hello **identifiera målportal** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="5fb2f-144">In hello **Discover Target Portal** dialog box:</span></span>
   
   1. <span data-ttu-id="5fb2f-145">Ange hello IP-adressen för din enhet.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-145">Specify hello IP address of your device.</span></span>
   2. <span data-ttu-id="5fb2f-146">Klicka på **avancerade**.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-146">Click **Advanced**.</span></span>
      
       ![Identifiera målportal](./media/storsimple-configure-chap/IC740945.png)
4. <span data-ttu-id="5fb2f-148">I hello **avancerade inställningar** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="5fb2f-148">In hello **Advanced Settings** dialog box:</span></span>
   
   1. <span data-ttu-id="5fb2f-149">Välj hello **aktivera CHAP inloggning** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-149">Select hello **Enable CHAP log on** check box.</span></span>
   2. <span data-ttu-id="5fb2f-150">I hello **namn** fält, ange hello användarnamnet som angetts för hello CHAP-initieraren i hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-150">In hello **Name** field, supply hello user name that you specified for hello CHAP Initiator in hello classic portal.</span></span>
   3. <span data-ttu-id="5fb2f-151">I hello **mål hemlighet** fält, ange hello lösenordet du angav för hello CHAP-initieraren i hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-151">In hello **Target secret** field, supply hello password that you specified for hello CHAP Initiator in hello classic portal.</span></span>
   4. <span data-ttu-id="5fb2f-152">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-152">Click **OK**.</span></span>
      
       ![Avancerade inställningar som är allmänt](./media/storsimple-configure-chap/IC740946.png)
5. <span data-ttu-id="5fb2f-154">På hello **mål** för hello **iSCSI-Initieraregenskaper** fönstret hello enhetens status borde visas som **ansluten**.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-154">On hello **Targets** tab of hello **iSCSI Initiator Properties** window, hello device status should appear as **Connected**.</span></span> <span data-ttu-id="5fb2f-155">Om du använder en enhet med StorSimple 1200 sedan monteras varje volym som en iSCSI-mål som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-155">If you are using a StorSimple 1200 device, then each volume will be mounted as an iSCSI target as shown below.</span></span> <span data-ttu-id="5fb2f-156">Steg 3 – 4 måste därför toobe upprepas för varje volym.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-156">Hence, steps  3-4 will need toobe repeated for each volume.</span></span>
   
    ![Volymerna monterade som separata mål](./media/storsimple-configure-chap/chap4.png)
   
   > [!IMPORTANT]
   > <span data-ttu-id="5fb2f-158">Om du ändrar hello iSCSI-namn används hello nytt namn för den nya iSCSI-sessioner.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-158">If you change hello iSCSI name, hello new name will be used for new iSCSI sessions.</span></span> <span data-ttu-id="5fb2f-159">Nya inställningar används inte för befintliga sessioner förrän du logga ut och logga in igen.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-159">New settings are not used for existing sessions until you log off and log on again.</span></span>
   > 
   > 

<span data-ttu-id="5fb2f-160">Mer information om hur du konfigurerar CHAP på värdservern med Windows hello gå för[ytterligare överväganden](#additional-considerations).</span><span class="sxs-lookup"><span data-stu-id="5fb2f-160">For more information about configuring CHAP on hello Windows host server, go too[Additional considerations](#additional-considerations).</span></span>

## <a name="bidirectional-or-mutual-authentication"></a><span data-ttu-id="5fb2f-161">Dubbelriktad eller ömsesidig autentisering</span><span class="sxs-lookup"><span data-stu-id="5fb2f-161">Bidirectional or mutual authentication</span></span>
<span data-ttu-id="5fb2f-162">I dubbelriktad autentisering hello mål autentiserar hello initieraren och sedan hello initieraren hello mål.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-162">In bidirectional authentication, hello target authenticates hello initiator and then hello initiator authenticates hello target.</span></span> <span data-ttu-id="5fb2f-163">Detta kräver hello tooconfigure hello CHAP-initieraren användarinställningar, samt hello omvänd CHAP-inställningar på enheten hello och iSCSI-initieraren programvara på hello värden.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-163">This requires hello user tooconfigure hello CHAP initiator settings, as well as hello reverse CHAP settings on hello device and iSCSI Initiator software on hello host.</span></span> <span data-ttu-id="5fb2f-164">hello följande procedurer beskrivs hello steg tooconfigure ömsesidig autentisering på hello enheten och på Windows hello-värd.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-164">hello following procedures describe hello steps tooconfigure mutual authentication on hello device and on hello Windows host.</span></span>

#### <a name="tooconfigure-your-device-for-mutual-authentication"></a><span data-ttu-id="5fb2f-165">tooconfigure enheten för ömsesidig autentisering</span><span class="sxs-lookup"><span data-stu-id="5fb2f-165">tooconfigure your device for mutual authentication</span></span>
1. <span data-ttu-id="5fb2f-166">I hello klassiska Azure-portalen på hello **enheter** klickar du på hello **konfigurera** fliken.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-166">In hello Azure classic portal, on hello **Devices** page, click hello **Configure** tab.</span></span>
   
    ![CHAP-målet](./media/storsimple-configure-chap/IC740948.png)
2. <span data-ttu-id="5fb2f-168">Rulla ned på den här sidan och i hello **CHAP Target** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="5fb2f-168">Scroll down on this page, and in hello **CHAP Target** section:</span></span>
   
   1. <span data-ttu-id="5fb2f-169">Ange en **omvänd CHAP-användarnamn** för din enhet.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-169">Provide a **Reverse CHAP user name** for your device.</span></span>
   2. <span data-ttu-id="5fb2f-170">Ange en **omvänd CHAP-lösenord** för din enhet.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-170">Supply a **Reverse CHAP password** for your device.</span></span>
   3. <span data-ttu-id="5fb2f-171">Bekräfta hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-171">Confirm hello password.</span></span>
3. <span data-ttu-id="5fb2f-172">I hello **CHAP-initieraren** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="5fb2f-172">In hello **CHAP Initiator** section:</span></span>
   
   1. <span data-ttu-id="5fb2f-173">Ange en **användarnamn** för din enhet.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-173">Provide a **user name** for your device.</span></span>
   2. <span data-ttu-id="5fb2f-174">Ange en **lösenord** för din enhet.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-174">Provide a **password** for your device.</span></span>
   3. <span data-ttu-id="5fb2f-175">Bekräfta hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-175">Confirm hello password.</span></span>
4. <span data-ttu-id="5fb2f-176">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-176">Click **Save**.</span></span> <span data-ttu-id="5fb2f-177">Att kommer visas ett bekräftelsemeddelande.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-177">A confirmation message will be displayed.</span></span> <span data-ttu-id="5fb2f-178">Klicka på **OK** toosave hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-178">Click **OK** toosave hello changes.</span></span>

#### <a name="tooconfigure-bidirectional-authentication-on-hello-windows-host-server"></a><span data-ttu-id="5fb2f-179">tooconfigure dubbelriktad autentisering på Windows hello värdservern</span><span class="sxs-lookup"><span data-stu-id="5fb2f-179">tooconfigure bidirectional authentication on hello Windows host server</span></span>
1. <span data-ttu-id="5fb2f-180">Starta hello iSCSI-initieraren på värdservern för hello Windows.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-180">On hello Windows host server, start hello iSCSI Initiator.</span></span>
2. <span data-ttu-id="5fb2f-181">I hello **iSCSI-Initieraregenskaper** -fönstret klickar du på hello **Configuration** fliken.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-181">In hello **iSCSI Initiator Properties** window, click hello **Configuration** tab.</span></span>
3. <span data-ttu-id="5fb2f-182">Klicka på **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-182">Click **CHAP**.</span></span>
4. <span data-ttu-id="5fb2f-183">I hello **iSCSI Initiator ömsesidig CHAP Secret** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="5fb2f-183">In hello **iSCSI Initiator Mutual CHAP Secret** dialog box:</span></span>
   
   1. <span data-ttu-id="5fb2f-184">Typen hello **omvänd CHAP-lösenord** som du konfigurerade i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-184">Type hello **Reverse CHAP Password** that you configured in hello Azure classic portal.</span></span>
   2. <span data-ttu-id="5fb2f-185">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-185">Click **OK**.</span></span>
      
       ![iSCSI-initieraren ömsesidig CHAP-hemlighet](./media/storsimple-configure-chap/IC740949.png)
5. <span data-ttu-id="5fb2f-187">Klicka på hello **mål** fliken.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-187">Click hello **Targets** tab.</span></span>
6. <span data-ttu-id="5fb2f-188">Klicka på hello **Anslut** knappen.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-188">Click hello **Connect** button.</span></span> 
7. <span data-ttu-id="5fb2f-189">I hello **ansluta tooTarget** dialogrutan klickar du på **Avancerat**.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-189">In hello **Connect tooTarget** dialog box, click **Advanced**.</span></span>
8. <span data-ttu-id="5fb2f-190">I hello **avancerade egenskaper** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="5fb2f-190">In hello **Advanced Properties** dialog box:</span></span>
   
   1. <span data-ttu-id="5fb2f-191">Välj hello **aktivera CHAP inloggning** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-191">Select hello **Enable CHAP log on** check box.</span></span>
   2. <span data-ttu-id="5fb2f-192">I hello **namn** fält, ange hello användarnamnet som angetts för hello CHAP-initieraren i hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-192">In hello **Name** field, supply hello user name that you specified for hello CHAP Initiator in hello classic portal.</span></span>
   3. <span data-ttu-id="5fb2f-193">I hello **mål hemlighet** fält, ange hello lösenordet du angav för hello CHAP-initieraren i hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-193">In hello **Target secret** field, supply hello password that you specified for hello CHAP Initiator in hello classic portal.</span></span>
   4. <span data-ttu-id="5fb2f-194">Välj hello **utför ömsesidig autentisering** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-194">Select hello **Perform mutual authentication** check box.</span></span>
      
       ![Avancerade inställningar ömsesidig autentisering](./media/storsimple-configure-chap/IC740950.png)
   5. <span data-ttu-id="5fb2f-196">Klicka på **OK** toocomplete hello CHAP-konfiguration</span><span class="sxs-lookup"><span data-stu-id="5fb2f-196">Click **OK** toocomplete hello CHAP configuration</span></span>

<span data-ttu-id="5fb2f-197">Mer information om hur du konfigurerar CHAP på värdservern med Windows hello gå för[ytterligare överväganden](#additional-considerations).</span><span class="sxs-lookup"><span data-stu-id="5fb2f-197">For more information about configuring CHAP on hello Windows host server, go too[Additional considerations](#additional-considerations).</span></span>

## <a name="additional-considerations"></a><span data-ttu-id="5fb2f-198">Annat som är bra att tänka på</span><span class="sxs-lookup"><span data-stu-id="5fb2f-198">Additional considerations</span></span>
<span data-ttu-id="5fb2f-199">Hej **Snabbanslut** funktionen har inte stöd för anslutningar som har aktiverat CHAP.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-199">hello **Quick Connect** feature does not support connections that have CHAP enabled.</span></span> <span data-ttu-id="5fb2f-200">När CHAP är aktiverat, kontrollera att du använder hello **Anslut** knapp som finns på hello **mål** fliken tooconnect tooa mål.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-200">When CHAP is enabled, make sure that you use hello **Connect** button that is available on hello **Targets** tab tooconnect tooa target.</span></span>

![Ansluta tootarget](./media/storsimple-configure-chap/IC740947.png)

<span data-ttu-id="5fb2f-202">I hello **ansluta tooTarget** dialogruta som presenteras, Välj hello **lägga till den här anslutningen toohello lista över favoritmål** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-202">In hello **Connect tooTarget** dialog box that is presented, select hello **Add this connection toohello list of Favorite Targets** check box.</span></span> <span data-ttu-id="5fb2f-203">Detta säkerställer att varje gång hello datorn startas om ett försök görs toorestore hello anslutning toohello iSCSI-favorit mål.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-203">This ensures that every time hello computer restarts, an attempt is made toorestore hello connection toohello iSCSI favorite targets.</span></span>

## <a name="errors-during-configuration"></a><span data-ttu-id="5fb2f-204">Fel under konfigurationen</span><span class="sxs-lookup"><span data-stu-id="5fb2f-204">Errors during configuration</span></span>
<span data-ttu-id="5fb2f-205">Om CHAP-konfigurationen är felaktig så kan du förmodligen toosee en **autentiseringsfel** felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-205">If your CHAP configuration is incorrect, then you are likely toosee an **Authentication failure** error message.</span></span>

## <a name="verification-of-chap-configuration"></a><span data-ttu-id="5fb2f-206">Verifiering av CHAP-konfiguration</span><span class="sxs-lookup"><span data-stu-id="5fb2f-206">Verification of CHAP configuration</span></span>
<span data-ttu-id="5fb2f-207">Du kan verifiera att CHAP används genom att slutföra hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-207">You can verify that CHAP is being used by completing hello following steps.</span></span>

#### <a name="tooverify-your-chap-configuration"></a><span data-ttu-id="5fb2f-208">tooverify CHAP-konfiguration</span><span class="sxs-lookup"><span data-stu-id="5fb2f-208">tooverify your CHAP configuration</span></span>
1. <span data-ttu-id="5fb2f-209">Klicka på **favorit mål**.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-209">Click **Favorite Targets**.</span></span>
2. <span data-ttu-id="5fb2f-210">Välj hello målobjekt som du har aktiverat autentisering.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-210">Select hello target for which you enabled authentication.</span></span>
3. <span data-ttu-id="5fb2f-211">Klicka på **information**.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-211">Click **Details**.</span></span>
   
    ![iSCSI-initieraren egenskaper favorit mål](./media/storsimple-configure-chap/IC740951.png)
4. <span data-ttu-id="5fb2f-213">I hello **information om favorit mål** dialogrutan hello kommentar i hello **autentisering** fältet.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-213">In hello **Favorite Target Details** dialog box, note hello entry in hello **Authentication** field.</span></span> <span data-ttu-id="5fb2f-214">Om hello konfigurationen lyckades, ska det stå **CHAP**.</span><span class="sxs-lookup"><span data-stu-id="5fb2f-214">If hello configuration was successful, it should say **CHAP**.</span></span>
   
    ![Information om favorit mål](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a><span data-ttu-id="5fb2f-216">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5fb2f-216">Next steps</span></span>
* <span data-ttu-id="5fb2f-217">Lär dig mer om [StorSimple-säkerhet](storsimple-security.md).</span><span class="sxs-lookup"><span data-stu-id="5fb2f-217">Learn more about [StorSimple security](storsimple-security.md).</span></span>
* <span data-ttu-id="5fb2f-218">Lär dig mer om [med hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="5fb2f-218">Learn more about [using hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

