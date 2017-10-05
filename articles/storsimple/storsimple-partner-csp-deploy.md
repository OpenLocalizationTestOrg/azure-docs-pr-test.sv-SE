---
title: "Microsoft Azure StorSimple och Molnlösningsleverantör Programöversikt | Microsoft Docs"
description: "En översikt över hur StorSimple- och CSP för StorSimple-partner."
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
ms.openlocfilehash: c8cb51093142146fc7d43b51a62d949f6cc38988
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-storsimple-virtual-array-for-cloud-solution-provider-program"></a><span data-ttu-id="47e68-103">Distribuera virtuella StorSimple-matrisen för Cloud Solution Provider Program</span><span class="sxs-lookup"><span data-stu-id="47e68-103">Deploy StorSimple Virtual Array for Cloud Solution Provider Program</span></span>

## <a name="overview"></a><span data-ttu-id="47e68-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="47e68-104">Overview</span></span>

<span data-ttu-id="47e68-105">StorSimple virtuell matris kan distribueras med partners Cloud Solution Providers (CSP) för sina kunder.</span><span class="sxs-lookup"><span data-stu-id="47e68-105">StorSimple Virtual Array can be deployed by the Cloud Solution Provider (CSP) partners for their customers.</span></span> <span data-ttu-id="47e68-106">En CSP-partner kan skapa en StorSimple Device Manager-tjänst.</span><span class="sxs-lookup"><span data-stu-id="47e68-106">A CSP partner can create a StorSimple Device Manager service.</span></span> <span data-ttu-id="47e68-107">Den här tjänsten kan sedan användas för att distribuera och hantera virtuella StorSimple-matris och associerade resurser, volymer och säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="47e68-107">This service can then be used to deploy and manage StorSimple Virtual Array and the associated shares, volumes, and backups.</span></span>

<span data-ttu-id="47e68-108">Den här artikeln beskrivs hur en CSP-partner kan lägga till en kund eller en ny prenumeration till en befintlig kund och sedan skapa en tjänst för att distribuera en virtuell StorSimple-matris i CSP.</span><span class="sxs-lookup"><span data-stu-id="47e68-108">This article describes how a CSP partner can add a customer or a new subscription to an existing customer and then create a service to deploy a StorSimple Virtual Array in CSP.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="47e68-109">Krav</span><span class="sxs-lookup"><span data-stu-id="47e68-109">Prerequisites</span></span>

<span data-ttu-id="47e68-110">Innan du börjar bör du se till att:</span><span class="sxs-lookup"><span data-stu-id="47e68-110">Before you begin, ensure that:</span></span>

- <span data-ttu-id="47e68-111">Du är registrerad i CSP-programmet.</span><span class="sxs-lookup"><span data-stu-id="47e68-111">You are enrolled under the CSP program.</span></span>
- <span data-ttu-id="47e68-112">Du har en giltig [Partnercenter](http://partnercenter.microsoft.com/) inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="47e68-112">You have valid [Partner Center](http://partnercenter.microsoft.com/) login credentials.</span></span> <span data-ttu-id="47e68-113">Autentiseringsuppgifterna kan du logga in på partnerportalen för att lägga till nya kunder, söka efter kunder eller navigera till ett kundkonto från Partner-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="47e68-113">The credentials enable you to sign in to the Partner portal to add new customers, search for customers, or navigate to a customer account from the Partner dashboard.</span></span> <span data-ttu-id="47e68-114">CSP: N kan fungera som en StorSimple-administratör för kunden i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="47e68-114">The CSP can function as a StorSimple administrator on behalf of the customer in the Azure portal.</span></span>
                             
## <a name="add-a-customer"></a><span data-ttu-id="47e68-115">Lägg till en kund</span><span class="sxs-lookup"><span data-stu-id="47e68-115">Add a customer</span></span>

<span data-ttu-id="47e68-116">Om du lägger till en kund, skapas automatiskt en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="47e68-116">If you add a customer, a subscription is automatically created.</span></span> <span data-ttu-id="47e68-117">Utför följande steg i partnerportalen för att lägga till en kund (och automatiskt skapa en prenumeration).</span><span class="sxs-lookup"><span data-stu-id="47e68-117">To add a customer (and automatically create a subscription), perform the following steps in the Partner portal.</span></span>

1. <span data-ttu-id="47e68-118">Gå till den [Partnercenter](http://partnercenter.microsoft.com/) och logga in med dina inloggningsuppgifter för CSP.</span><span class="sxs-lookup"><span data-stu-id="47e68-118">Go to the [Partner Center](http://partnercenter.microsoft.com/) and sign in using your CSP credentials.</span></span> <span data-ttu-id="47e68-119">Klicka på **instrumentpanelen**.</span><span class="sxs-lookup"><span data-stu-id="47e68-119">Click **Dashboard**.</span></span>

     ![Instrumentpanelen Partner Center](./media/storsimple-partner-csp-deploy/image1.png)
                              
2. <span data-ttu-id="47e68-121">I den vänstra rutan klickar du på **kunder**.</span><span class="sxs-lookup"><span data-stu-id="47e68-121">In the left-pane, click **Customers**.</span></span> <span data-ttu-id="47e68-122">I den högra rutan, klickar du på **Lägg till kunder**.</span><span class="sxs-lookup"><span data-stu-id="47e68-122">In the right-pane, click **Add customers**.</span></span> <span data-ttu-id="47e68-123">Ange informationen för kunden.</span><span class="sxs-lookup"><span data-stu-id="47e68-123">Enter the details of the customer.</span></span> <span data-ttu-id="47e68-124">Klicka på **nästa: prenumerationer** att skapa en kundprenumeration.</span><span class="sxs-lookup"><span data-stu-id="47e68-124">Click **Next: Subscriptions** to create a customer subscription.</span></span>

    ![Lägg till kund](./media/storsimple-partner-csp-deploy/image2.png)

3.  <span data-ttu-id="47e68-126">Välj **Microsoft Azure** erbjuder.</span><span class="sxs-lookup"><span data-stu-id="47e68-126">Select **Microsoft Azure** offer.</span></span> <span data-ttu-id="47e68-127">Bläddra längst ned på sidan och klicka på **granska**.</span><span class="sxs-lookup"><span data-stu-id="47e68-127">Scroll to the bottom of the page and click **Review**.</span></span>

    ![Granska information om prenumerationen](./media/storsimple-partner-csp-deploy/image3.png)
                              
4. <span data-ttu-id="47e68-129">Granska informationen och klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="47e68-129">Review the information and click **Submit**.</span></span>

    ![Skicka prenumeration](./media/storsimple-partner-csp-deploy/image4.png)

5. <span data-ttu-id="47e68-131">Spara bekräftelse-information för framtida bruk.</span><span class="sxs-lookup"><span data-stu-id="47e68-131">Save the confirmation information for future reference.</span></span>

    ![Spara-bekräftelse](./media/storsimple-partner-csp-deploy/image5.png)

6. <span data-ttu-id="47e68-133">Sök eller navigera till kunden du just lagt till.</span><span class="sxs-lookup"><span data-stu-id="47e68-133">Find or navigate to the customer you just added.</span></span> <span data-ttu-id="47e68-134">Klicka på den **företagsnamn** och öka detaljnivån till informationen.</span><span class="sxs-lookup"><span data-stu-id="47e68-134">Click the **Company name** to drill down into the details.</span></span>

    ![Sök efter kunden](./media/storsimple-partner-csp-deploy/image6.png)  

7. <span data-ttu-id="47e68-136">I den vänstra rutan, Välj **Service management**.</span><span class="sxs-lookup"><span data-stu-id="47e68-136">In the left-pane, select **Service management**.</span></span> <span data-ttu-id="47e68-137">I den högra rutan under **administrera tjänster**, klickar du på **Microsoft Azure Management Portal** att logga in som administratör Azure för kunden.</span><span class="sxs-lookup"><span data-stu-id="47e68-137">In the right-pane, under **Administer services**, click **Microsoft Azure Management Portal** to sign in as an Azure administrator for your customer.</span></span>

    ![Logga in på Azure-portalen](./media/storsimple-partner-csp-deploy/image9.png)

8. <span data-ttu-id="47e68-139">Klicka för att skapa en StorSimple-Enhetshanteraren **+ ny** och söka efter eller navigera till **StorSimple virtuell enhet serien**.</span><span class="sxs-lookup"><span data-stu-id="47e68-139">To create a StorSimple Device Manager, click **+ New** and search for or navigate to **StorSimple Virtual Device Series**.</span></span> <span data-ttu-id="47e68-140">Mer information finns på [distribuera en tjänst för StorSimple Enhetshanteraren](storsimple-virtual-array-manage-service.md).</span><span class="sxs-lookup"><span data-stu-id="47e68-140">For more information, go to [Deploy a StorSimple Device Manager service](storsimple-virtual-array-manage-service.md).</span></span>

    ![Skapa StorSimple enheten Manager-tjänsten](./media/storsimple-partner-csp-deploy/image8.png)


## <a name="add-a-subscription"></a><span data-ttu-id="47e68-142">Lägga till en prenumeration</span><span class="sxs-lookup"><span data-stu-id="47e68-142">Add a subscription</span></span>

<span data-ttu-id="47e68-143">I vissa fall kan du kanske har en befintlig kund och du måste lägga till en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="47e68-143">In some instances, you may have an existing customer, and you need to add a subscription.</span></span> <span data-ttu-id="47e68-144">Utför följande steg om du vill lägga till en prenumeration till en befintlig kund i partnerportalen.</span><span class="sxs-lookup"><span data-stu-id="47e68-144">To add a subscription to an existing customer, perform the following steps in the Partner portal.</span></span>

1. <span data-ttu-id="47e68-145">Gå till den [Partnercenter](http://partnercenter.microsoft.com/) och logga in med dina inloggningsuppgifter för CSP.</span><span class="sxs-lookup"><span data-stu-id="47e68-145">Go to the [Partner Center](http://partnercenter.microsoft.com/) and sign in using your CSP credentials.</span></span> <span data-ttu-id="47e68-146">Klicka på **instrumentpanelen**.</span><span class="sxs-lookup"><span data-stu-id="47e68-146">Click **Dashboard**.</span></span>

     ![Instrumentpanelen Partner Center](./media/storsimple-partner-csp-deploy/image1.png)
                              
2. <span data-ttu-id="47e68-148">I den vänstra rutan klickar du på **kunder**.</span><span class="sxs-lookup"><span data-stu-id="47e68-148">In the left-pane, click **Customers**.</span></span> <span data-ttu-id="47e68-149">Sök eller navigera till kunden som du vill lägga till en prenumeration på.</span><span class="sxs-lookup"><span data-stu-id="47e68-149">Find or navigate to the customer you want to add a subscription to.</span></span> <span data-ttu-id="47e68-150">Klicka på den ![Expandera kryssikonen](./media/storsimple-partner-csp-deploy/expand_pane_icon.png) ikon för att expandera raden för företagsnamnet för kunden.</span><span class="sxs-lookup"><span data-stu-id="47e68-150">Click the ![Expand check icon](./media/storsimple-partner-csp-deploy/expand_pane_icon.png) icon to expand the row for the company name for your customer.</span></span> <span data-ttu-id="47e68-151">Klicka på information, **lägga till prenumerationer**.</span><span class="sxs-lookup"><span data-stu-id="47e68-151">In the details, click **Add subscriptions**.</span></span>

    ![Kunder](./media/storsimple-partner-csp-deploy/image10.png)

3. <span data-ttu-id="47e68-153">Kontrollera **Microsoft Azure** för den **uppifrån erbjudanden** i prenumerationen och på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="47e68-153">Check **Microsoft Azure** for the **Top offers** in the subscription and click **Submit**.</span></span> <span data-ttu-id="47e68-154">Detta skapar en ny prenumeration.</span><span class="sxs-lookup"><span data-stu-id="47e68-154">This creates a new subscription.</span></span>

    ![Lägg till ny prenumeration](./media/storsimple-partner-csp-deploy/image11.png)

6. <span data-ttu-id="47e68-156">När en ny prenumeration skapas, klickar du på **<--kunder** i den vänstra rutan att återgå till den **kunder** sidan.</span><span class="sxs-lookup"><span data-stu-id="47e68-156">After a new subscription is created, click **<-- Customers** in the left-pane to return to the **Customers** page.</span></span> <span data-ttu-id="47e68-157">Söka efter kunder som du just har skapat en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="47e68-157">Search for the customer for whom you just created a subscription.</span></span> <span data-ttu-id="47e68-158">Klicka på den **företagsnamn** och öka detaljnivån till informationen.</span><span class="sxs-lookup"><span data-stu-id="47e68-158">Click the **Company name** to drill down into the details.</span></span>

    ![Sök efter kunden](./media/storsimple-partner-csp-deploy/image6.png)  

7. <span data-ttu-id="47e68-160">I den vänstra rutan, Välj **Service management**.</span><span class="sxs-lookup"><span data-stu-id="47e68-160">In the left-pane, select **Service management**.</span></span> <span data-ttu-id="47e68-161">I den högra rutan under **administrera tjänster**, klickar du på **Microsoft Azure Management Portal** att logga in som administratör Azure för kunden.</span><span class="sxs-lookup"><span data-stu-id="47e68-161">In the right-pane, under **Administer services**, click **Microsoft Azure Management Portal** to sign in as an Azure administrator for your customer.</span></span>

    ![Logga in på Azure-portalen](./media/storsimple-partner-csp-deploy/image9.png)

8. <span data-ttu-id="47e68-163">Klicka för att skapa en StorSimple-Enhetshanteraren **+ ny** och söka efter eller navigera till **StorSimple virtuell enhet serien**.</span><span class="sxs-lookup"><span data-stu-id="47e68-163">To create a StorSimple Device Manager, click **+ New** and search for or navigate to **StorSimple Virtual Device Series**.</span></span> <span data-ttu-id="47e68-164">Mer information finns på [distribuera en tjänst för StorSimple Enhetshanteraren](storsimple-virtual-array-manage-service.md).</span><span class="sxs-lookup"><span data-stu-id="47e68-164">For more information, go to [Deploy a StorSimple Device Manager service](storsimple-virtual-array-manage-service.md).</span></span>

    ![Skapa StorSimple enheten Manager-tjänsten](./media/storsimple-partner-csp-deploy/image8.png)

## <a name="next-steps"></a><span data-ttu-id="47e68-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="47e68-166">Next steps</span></span>

- <span data-ttu-id="47e68-167">Om du har fler frågor om den virtuella StorSimple i CSP, gå till [StorSimple i CSP: vanliga frågor och](storsimple-partner-csp-faq.md).</span><span class="sxs-lookup"><span data-stu-id="47e68-167">If you have more questions regarding the StorSimple in CSP, go to [StorSimple in CSP: Frequently asked questions](storsimple-partner-csp-faq.md).</span></span>
- <span data-ttu-id="47e68-168">Om du är redo att distribuera din StorSimple, gå till [distribuera din StorSimple i CSP](storsimple-partner-csp-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="47e68-168">If you are ready to deploy your StorSimple, go to [Deploy your StorSimple in CSP](storsimple-partner-csp-deploy.md).</span></span>
