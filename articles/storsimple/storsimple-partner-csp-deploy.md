---
title: "aaaMicrosoft Azure StorSimple och molnet providern programmet översikt över lösningar med | Microsoft Docs"
description: "En översikt över hur hello StorSimple- och CSP för StorSimple-partner."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/08/2017
ms.author: alkohli
ms.openlocfilehash: b5d999f2fbb9a27e7404ff454957b29dbef56af6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array-for-cloud-solution-provider-program"></a><span data-ttu-id="12b69-103">Distribuera virtuella StorSimple-matrisen för Cloud Solution Provider Program</span><span class="sxs-lookup"><span data-stu-id="12b69-103">Deploy StorSimple Virtual Array for Cloud Solution Provider Program</span></span>

## <a name="overview"></a><span data-ttu-id="12b69-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="12b69-104">Overview</span></span>

<span data-ttu-id="12b69-105">StorSimple virtuell matris kan distribueras av hello Cloud Solution Providers (CSP) partners för sina kunder.</span><span class="sxs-lookup"><span data-stu-id="12b69-105">StorSimple Virtual Array can be deployed by hello Cloud Solution Provider (CSP) partners for their customers.</span></span> <span data-ttu-id="12b69-106">En CSP-partner kan skapa en StorSimple Device Manager-tjänst.</span><span class="sxs-lookup"><span data-stu-id="12b69-106">A CSP partner can create a StorSimple Device Manager service.</span></span> <span data-ttu-id="12b69-107">Den här tjänsten kan sedan använda toodeploy och hantera virtuella StorSimple-matris och hello associerade resurser, volymer och säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="12b69-107">This service can then be used toodeploy and manage StorSimple Virtual Array and hello associated shares, volumes, and backups.</span></span>

<span data-ttu-id="12b69-108">Den här artikeln beskrivs hur en CSP-partner kan lägga till en kund eller en ny prenumeration tooan befintliga kund och skapa sedan en service toodeploy en virtuell StorSimple-matris i CSP.</span><span class="sxs-lookup"><span data-stu-id="12b69-108">This article describes how a CSP partner can add a customer or a new subscription tooan existing customer and then create a service toodeploy a StorSimple Virtual Array in CSP.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="12b69-109">Krav</span><span class="sxs-lookup"><span data-stu-id="12b69-109">Prerequisites</span></span>

<span data-ttu-id="12b69-110">Innan du börjar bör du se till att:</span><span class="sxs-lookup"><span data-stu-id="12b69-110">Before you begin, ensure that:</span></span>

- <span data-ttu-id="12b69-111">Du är registrerad under hello CSP-programmet.</span><span class="sxs-lookup"><span data-stu-id="12b69-111">You are enrolled under hello CSP program.</span></span>
- <span data-ttu-id="12b69-112">Du har en giltig [Partnercenter](http://partnercenter.microsoft.com/) inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="12b69-112">You have valid [Partner Center](http://partnercenter.microsoft.com/) login credentials.</span></span> <span data-ttu-id="12b69-113">Aktivera toosign toohello Partner portal tooadd nya kunder hello autentiseringsuppgifter, söka efter kunder eller navigera tooa kundkontot hello Partner instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="12b69-113">hello credentials enable you toosign in toohello Partner portal tooadd new customers, search for customers, or navigate tooa customer account from hello Partner dashboard.</span></span> <span data-ttu-id="12b69-114">hello CSP kan fungera som en StorSimple-administratör åt hello kunden i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="12b69-114">hello CSP can function as a StorSimple administrator on behalf of hello customer in hello Azure portal.</span></span>
                             
## <a name="add-a-customer"></a><span data-ttu-id="12b69-115">Lägg till en kund</span><span class="sxs-lookup"><span data-stu-id="12b69-115">Add a customer</span></span>

<span data-ttu-id="12b69-116">Om du lägger till en kund, skapas automatiskt en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="12b69-116">If you add a customer, a subscription is automatically created.</span></span> <span data-ttu-id="12b69-117">tooadd en kund (och automatiskt skapa en prenumeration) utför hello följa stegen i hello Partner-portalen.</span><span class="sxs-lookup"><span data-stu-id="12b69-117">tooadd a customer (and automatically create a subscription), perform hello following steps in hello Partner portal.</span></span>

1. <span data-ttu-id="12b69-118">Gå toohello [Partnercenter](http://partnercenter.microsoft.com/) och logga in med dina inloggningsuppgifter för CSP.</span><span class="sxs-lookup"><span data-stu-id="12b69-118">Go toohello [Partner Center](http://partnercenter.microsoft.com/) and sign in using your CSP credentials.</span></span> <span data-ttu-id="12b69-119">Klicka på **instrumentpanelen**.</span><span class="sxs-lookup"><span data-stu-id="12b69-119">Click **Dashboard**.</span></span>

     ![Instrumentpanelen Partner Center](./media/storsimple-partner-csp-deploy/image1.png)
                              
2. <span data-ttu-id="12b69-121">I hello vänstra fönsterrutan klickar du på **kunder**.</span><span class="sxs-lookup"><span data-stu-id="12b69-121">In hello left-pane, click **Customers**.</span></span> <span data-ttu-id="12b69-122">I hello högra rutan, klickar du på **Lägg till kunder**.</span><span class="sxs-lookup"><span data-stu-id="12b69-122">In hello right-pane, click **Add customers**.</span></span> <span data-ttu-id="12b69-123">Ange information om hello hello kunden.</span><span class="sxs-lookup"><span data-stu-id="12b69-123">Enter hello details of hello customer.</span></span> <span data-ttu-id="12b69-124">Klicka på **nästa: prenumerationer** toocreate en kundprenumeration.</span><span class="sxs-lookup"><span data-stu-id="12b69-124">Click **Next: Subscriptions** toocreate a customer subscription.</span></span>

    ![Lägg till kund](./media/storsimple-partner-csp-deploy/image2.png)

3.  <span data-ttu-id="12b69-126">Välj **Microsoft Azure** erbjuder.</span><span class="sxs-lookup"><span data-stu-id="12b69-126">Select **Microsoft Azure** offer.</span></span> <span data-ttu-id="12b69-127">Rulla toohello längst ned på sidan hello och klicka på **granska**.</span><span class="sxs-lookup"><span data-stu-id="12b69-127">Scroll toohello bottom of hello page and click **Review**.</span></span>

    ![Granska information om prenumerationen](./media/storsimple-partner-csp-deploy/image3.png)
                              
4. <span data-ttu-id="12b69-129">Granska hello information och klickar på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="12b69-129">Review hello information and click **Submit**.</span></span>

    ![Skicka prenumeration](./media/storsimple-partner-csp-deploy/image4.png)

5. <span data-ttu-id="12b69-131">Spara hello bekräftelse information för framtida bruk.</span><span class="sxs-lookup"><span data-stu-id="12b69-131">Save hello confirmation information for future reference.</span></span>

    ![Spara-bekräftelse](./media/storsimple-partner-csp-deploy/image5.png)

6. <span data-ttu-id="12b69-133">Sök eller navigera toohello kund som du just lagt till.</span><span class="sxs-lookup"><span data-stu-id="12b69-133">Find or navigate toohello customer you just added.</span></span> <span data-ttu-id="12b69-134">Klicka på hello **företagsnamn** toodrill ned hello detaljer.</span><span class="sxs-lookup"><span data-stu-id="12b69-134">Click hello **Company name** toodrill down into hello details.</span></span>

    ![Sök efter hello kunden](./media/storsimple-partner-csp-deploy/image6.png)  

7. <span data-ttu-id="12b69-136">Välj i hello vänstra fönsterrutan **Service management**.</span><span class="sxs-lookup"><span data-stu-id="12b69-136">In hello left-pane, select **Service management**.</span></span> <span data-ttu-id="12b69-137">I hello högra rutan under **administrera tjänster**, klickar du på **Microsoft Azure Management Portal** toosign i som Azure-administratör för din kund.</span><span class="sxs-lookup"><span data-stu-id="12b69-137">In hello right-pane, under **Administer services**, click **Microsoft Azure Management Portal** toosign in as an Azure administrator for your customer.</span></span>

    ![Logga in tooAzure portal](./media/storsimple-partner-csp-deploy/image9.png)

8. <span data-ttu-id="12b69-139">toocreate en StorSimple-Enhetshanteraren, klicka på **+ ny** och Sök eller navigera för**StorSimple virtuell enhet serien**.</span><span class="sxs-lookup"><span data-stu-id="12b69-139">toocreate a StorSimple Device Manager, click **+ New** and search for or navigate too**StorSimple Virtual Device Series**.</span></span> <span data-ttu-id="12b69-140">Mer information finns för[distribuera en tjänst för StorSimple Enhetshanteraren](storsimple-virtual-array-manage-service.md).</span><span class="sxs-lookup"><span data-stu-id="12b69-140">For more information, go too[Deploy a StorSimple Device Manager service](storsimple-virtual-array-manage-service.md).</span></span>

    ![Skapa StorSimple enheten Manager-tjänsten](./media/storsimple-partner-csp-deploy/image8.png)


## <a name="add-a-subscription"></a><span data-ttu-id="12b69-142">Lägga till en prenumeration</span><span class="sxs-lookup"><span data-stu-id="12b69-142">Add a subscription</span></span>

<span data-ttu-id="12b69-143">I vissa fall kan du kanske har en befintlig kund, och du behöver tooadd en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="12b69-143">In some instances, you may have an existing customer, and you need tooadd a subscription.</span></span> <span data-ttu-id="12b69-144">tooadd en prenumeration tooan befintliga kund, utföra hello följa stegen i hello Partner-portalen.</span><span class="sxs-lookup"><span data-stu-id="12b69-144">tooadd a subscription tooan existing customer, perform hello following steps in hello Partner portal.</span></span>

1. <span data-ttu-id="12b69-145">Gå toohello [Partnercenter](http://partnercenter.microsoft.com/) och logga in med dina inloggningsuppgifter för CSP.</span><span class="sxs-lookup"><span data-stu-id="12b69-145">Go toohello [Partner Center](http://partnercenter.microsoft.com/) and sign in using your CSP credentials.</span></span> <span data-ttu-id="12b69-146">Klicka på **instrumentpanelen**.</span><span class="sxs-lookup"><span data-stu-id="12b69-146">Click **Dashboard**.</span></span>

     ![Instrumentpanelen Partner Center](./media/storsimple-partner-csp-deploy/image1.png)
                              
2. <span data-ttu-id="12b69-148">I hello vänstra fönsterrutan klickar du på **kunder**.</span><span class="sxs-lookup"><span data-stu-id="12b69-148">In hello left-pane, click **Customers**.</span></span> <span data-ttu-id="12b69-149">Sök eller navigera toohello kund som du vill tooadd en prenumeration på.</span><span class="sxs-lookup"><span data-stu-id="12b69-149">Find or navigate toohello customer you want tooadd a subscription to.</span></span> <span data-ttu-id="12b69-150">Klicka på hello ![Expandera kryssikonen](./media/storsimple-partner-csp-deploy/expand_pane_icon.png) ikonen tooexpand hello rad för hello företagsnamn för kunden.</span><span class="sxs-lookup"><span data-stu-id="12b69-150">Click hello ![Expand check icon](./media/storsimple-partner-csp-deploy/expand_pane_icon.png) icon tooexpand hello row for hello company name for your customer.</span></span> <span data-ttu-id="12b69-151">I hello information klickar du på **lägga till prenumerationer**.</span><span class="sxs-lookup"><span data-stu-id="12b69-151">In hello details, click **Add subscriptions**.</span></span>

    ![Kunder](./media/storsimple-partner-csp-deploy/image10.png)

3. <span data-ttu-id="12b69-153">Kontrollera **Microsoft Azure** för hello **uppifrån erbjudanden** i hello prenumeration och klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="12b69-153">Check **Microsoft Azure** for hello **Top offers** in hello subscription and click **Submit**.</span></span> <span data-ttu-id="12b69-154">Detta skapar en ny prenumeration.</span><span class="sxs-lookup"><span data-stu-id="12b69-154">This creates a new subscription.</span></span>

    ![Lägg till ny prenumeration](./media/storsimple-partner-csp-deploy/image11.png)

6. <span data-ttu-id="12b69-156">När en ny prenumeration skapas, klickar du på **<--kunder** i hello vänstra fönsterrutan tooreturn toohello **kunder** sidan.</span><span class="sxs-lookup"><span data-stu-id="12b69-156">After a new subscription is created, click **<-- Customers** in hello left-pane tooreturn toohello **Customers** page.</span></span> <span data-ttu-id="12b69-157">Sök efter hello kund som du just har skapat en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="12b69-157">Search for hello customer for whom you just created a subscription.</span></span> <span data-ttu-id="12b69-158">Klicka på hello **företagsnamn** toodrill ned hello detaljer.</span><span class="sxs-lookup"><span data-stu-id="12b69-158">Click hello **Company name** toodrill down into hello details.</span></span>

    ![Sök efter hello kunden](./media/storsimple-partner-csp-deploy/image6.png)  

7. <span data-ttu-id="12b69-160">Välj i hello vänstra fönsterrutan **Service management**.</span><span class="sxs-lookup"><span data-stu-id="12b69-160">In hello left-pane, select **Service management**.</span></span> <span data-ttu-id="12b69-161">I hello högra rutan under **administrera tjänster**, klickar du på **Microsoft Azure Management Portal** toosign i som Azure-administratör för din kund.</span><span class="sxs-lookup"><span data-stu-id="12b69-161">In hello right-pane, under **Administer services**, click **Microsoft Azure Management Portal** toosign in as an Azure administrator for your customer.</span></span>

    ![Logga in tooAzure portal](./media/storsimple-partner-csp-deploy/image9.png)

8. <span data-ttu-id="12b69-163">toocreate en StorSimple-Enhetshanteraren, klicka på **+ ny** och Sök eller navigera för**StorSimple virtuell enhet serien**.</span><span class="sxs-lookup"><span data-stu-id="12b69-163">toocreate a StorSimple Device Manager, click **+ New** and search for or navigate too**StorSimple Virtual Device Series**.</span></span> <span data-ttu-id="12b69-164">Mer information finns för[distribuera en tjänst för StorSimple Enhetshanteraren](storsimple-virtual-array-manage-service.md).</span><span class="sxs-lookup"><span data-stu-id="12b69-164">For more information, go too[Deploy a StorSimple Device Manager service](storsimple-virtual-array-manage-service.md).</span></span>

    ![Skapa StorSimple enheten Manager-tjänsten](./media/storsimple-partner-csp-deploy/image8.png)

## <a name="next-steps"></a><span data-ttu-id="12b69-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="12b69-166">Next steps</span></span>

- <span data-ttu-id="12b69-167">Om du har fler frågor angående hello StorSimple i CSP gå för[StorSimple i CSP: vanliga frågor och](storsimple-partner-csp-faq.md).</span><span class="sxs-lookup"><span data-stu-id="12b69-167">If you have more questions regarding hello StorSimple in CSP, go too[StorSimple in CSP: Frequently asked questions](storsimple-partner-csp-faq.md).</span></span>
- <span data-ttu-id="12b69-168">Om du är klar toodeploy din StorSimple, gå för[distribuera din StorSimple i CSP](storsimple-partner-csp-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="12b69-168">If you are ready toodeploy your StorSimple, go too[Deploy your StorSimple in CSP](storsimple-partner-csp-deploy.md).</span></span>
