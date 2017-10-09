---
title: "aaaDeploy SAP S/4HANA eller BW/4HANA på en virtuell dator i Azure | Microsoft Docs"
description: "Distribuera SAP S/4HANA eller BW/4HANA på en virtuell dator i Azure"
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 44bbd2b6-a376-4b5c-b824-e76917117fa9
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/15/2016
ms.author: hermannd
ms.openlocfilehash: 7e57f7daa7667b7c7dbcb86f6892a54e4295b74c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-sap-s4hana-or-bw4hana-on-azure"></a><span data-ttu-id="06bee-103">Distribuera SAP S/4HANA eller BW/4HANA på Azure</span><span class="sxs-lookup"><span data-stu-id="06bee-103">Deploy SAP S/4HANA or BW/4HANA on Azure</span></span>
<span data-ttu-id="06bee-104">Den här artikeln beskriver hur toodeploy S/4HANA på Azure med hjälp av hello SAP Molnbibliotek installation (SAP CAL) 3.0.</span><span class="sxs-lookup"><span data-stu-id="06bee-104">This article describes how toodeploy S/4HANA on Azure by using hello SAP Cloud Appliance Library (SAP CAL) 3.0.</span></span> <span data-ttu-id="06bee-105">toodeploy andra SAP HANA-baserade lösningar, till exempel BW/4HANA följer hello samma steg.</span><span class="sxs-lookup"><span data-stu-id="06bee-105">toodeploy other SAP HANA-based solutions, such as BW/4HANA, follow hello same steps.</span></span>

> [!NOTE]
<span data-ttu-id="06bee-106">Mer information om hello SAP CAL gå toohello [SAP installation Molnbibliotek](https://cal.sap.com/) webbplats.</span><span class="sxs-lookup"><span data-stu-id="06bee-106">For more information about hello SAP CAL, go toohello [SAP Cloud Appliance Library](https://cal.sap.com/) website.</span></span> <span data-ttu-id="06bee-107">SAP har också en blogg om hello [SAP molnet installation biblioteket 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).</span><span class="sxs-lookup"><span data-stu-id="06bee-107">SAP also has a blog about hello [SAP Cloud Appliance Library 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).</span></span>

> [!NOTE]
<span data-ttu-id="06bee-108">Eftersom den 29 maj 2017 kan du använda hello Azure Resource Manager-distributionsmodellen dessutom toohello mindre önskade klassisk distribution modell toodeploy hello SAP CAL.</span><span class="sxs-lookup"><span data-stu-id="06bee-108">As of May 29, 2017, you can use hello Azure Resource Manager deployment model in addition toohello less-preferred classic deployment model toodeploy hello SAP CAL.</span></span> <span data-ttu-id="06bee-109">Vi rekommenderar att du använder hello nya Resource Manager-modellen och ignorera hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="06bee-109">We recommend that you use hello new Resource Manager deployment model and disregard hello classic deployment model.</span></span>

## <a name="step-by-step-process-toodeploy-hello-solution"></a><span data-ttu-id="06bee-110">Steg för steg toodeploy hello lösning</span><span class="sxs-lookup"><span data-stu-id="06bee-110">Step-by-step process toodeploy hello solution</span></span>

<span data-ttu-id="06bee-111">hello följande sekvens av skärmdumpar visar hur toodeploy S/4HANA på Azure med hjälp av hello SAP CAL.</span><span class="sxs-lookup"><span data-stu-id="06bee-111">hello following sequence of screenshots shows you how toodeploy S/4HANA on Azure by using hello SAP CAL.</span></span> <span data-ttu-id="06bee-112">hello processen fungerar hello samma sätt som andra lösningar, till exempel BW/4HANA.</span><span class="sxs-lookup"><span data-stu-id="06bee-112">hello process works hello same way for other solutions, such as BW/4HANA.</span></span>

<span data-ttu-id="06bee-113">Hej **lösningar** sidan visar några av hello SAP HANA för CAL-baserade lösningar som är tillgängliga på Azure.</span><span class="sxs-lookup"><span data-stu-id="06bee-113">hello **Solutions** page shows some of hello SAP CAL HANA-based solutions available on Azure.</span></span> <span data-ttu-id="06bee-114">**SAP S/4HANA 1610 FPS01, Fully-Activated installation** hello mellersta rad:</span><span class="sxs-lookup"><span data-stu-id="06bee-114">**SAP S/4HANA 1610 FPS01, Fully-Activated Appliance** is in hello middle row:</span></span>

![SAP CAL-lösningar](./media/cal-s4h/s4h-pic-1c.png)

### <a name="create-an-account-in-hello-sap-cal"></a><span data-ttu-id="06bee-116">Skapa ett konto i hello SAP CAL</span><span class="sxs-lookup"><span data-stu-id="06bee-116">Create an account in hello SAP CAL</span></span>
1. <span data-ttu-id="06bee-117">toosign i toohello SAP CAL för hello första gången använder din SAP-S-användare eller en annan användare som har registrerats med SAP.</span><span class="sxs-lookup"><span data-stu-id="06bee-117">toosign in toohello SAP CAL for hello first time, use your SAP S-User or other user registered with SAP.</span></span> <span data-ttu-id="06bee-118">Definiera ett SAP CAL-konto som används av hello SAP CAL toodeploy installationer på Azure.</span><span class="sxs-lookup"><span data-stu-id="06bee-118">Then define an SAP CAL account that is used by hello SAP CAL toodeploy appliances on Azure.</span></span> <span data-ttu-id="06bee-119">I hello konto definition behöver du:</span><span class="sxs-lookup"><span data-stu-id="06bee-119">In hello account definition, you need to:</span></span>

    <span data-ttu-id="06bee-120">a.</span><span class="sxs-lookup"><span data-stu-id="06bee-120">a.</span></span> <span data-ttu-id="06bee-121">Välj hello distributionsmodell i Azure (Resource Manager eller klassisk).</span><span class="sxs-lookup"><span data-stu-id="06bee-121">Select hello deployment model on Azure (Resource Manager or classic).</span></span>

    <span data-ttu-id="06bee-122">b.</span><span class="sxs-lookup"><span data-stu-id="06bee-122">b.</span></span> <span data-ttu-id="06bee-123">Ange din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="06bee-123">Enter your Azure subscription.</span></span> <span data-ttu-id="06bee-124">En SAP CAL-konto kan tilldelas tooone-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="06bee-124">An SAP CAL account can be assigned tooone subscription only.</span></span> <span data-ttu-id="06bee-125">Om du behöver mer än en prenumeration behöver toocreate en annan SAP CAL-konto.</span><span class="sxs-lookup"><span data-stu-id="06bee-125">If you need more than one subscription, you need toocreate another SAP CAL account.</span></span>

    <span data-ttu-id="06bee-126">c.</span><span class="sxs-lookup"><span data-stu-id="06bee-126">c.</span></span> <span data-ttu-id="06bee-127">Ge hello SAP CAL behörighet toodeploy till din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="06bee-127">Give hello SAP CAL permission toodeploy into your Azure subscription.</span></span>

    > [!NOTE]
    <span data-ttu-id="06bee-128">hello nästa steg visar hur toocreate en SAP-CAL-konto för Resource Manager distributioner.</span><span class="sxs-lookup"><span data-stu-id="06bee-128">hello next steps show how toocreate an SAP CAL account for Resource Manager deployments.</span></span> <span data-ttu-id="06bee-129">Om du redan har ett SAP CAL-konto som är länkade toohello klassiska distributionsmodellen du *måste* toofollow dessa steg toocreate ett nytt SAP CAL-konto.</span><span class="sxs-lookup"><span data-stu-id="06bee-129">If you already have an SAP CAL account that is linked toohello classic deployment model, you *need* toofollow these steps toocreate a new SAP CAL account.</span></span> <span data-ttu-id="06bee-130">hello nya SAP CAL-konto måste toodeploy i hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="06bee-130">hello new SAP CAL account needs toodeploy in hello Resource Manager model.</span></span>

2. <span data-ttu-id="06bee-131">Skapa ett nytt SAP CAL-konto.</span><span class="sxs-lookup"><span data-stu-id="06bee-131">Create a new SAP CAL account.</span></span> <span data-ttu-id="06bee-132">Hej **konton** sidan visas tre alternativ för Azure:</span><span class="sxs-lookup"><span data-stu-id="06bee-132">hello **Accounts** page shows three choices for Azure:</span></span> 

    <span data-ttu-id="06bee-133">a.</span><span class="sxs-lookup"><span data-stu-id="06bee-133">a.</span></span> <span data-ttu-id="06bee-134">**Microsoft Azure (klassisk)** är hello klassiska distributionsmodellen och är inte längre önskade.</span><span class="sxs-lookup"><span data-stu-id="06bee-134">**Microsoft Azure (classic)** is hello classic deployment model and is no longer preferred.</span></span>

    <span data-ttu-id="06bee-135">b.</span><span class="sxs-lookup"><span data-stu-id="06bee-135">b.</span></span> <span data-ttu-id="06bee-136">**Microsoft Azure** är hello nya Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="06bee-136">**Microsoft Azure** is hello new Resource Manager deployment model.</span></span>

    <span data-ttu-id="06bee-137">c.</span><span class="sxs-lookup"><span data-stu-id="06bee-137">c.</span></span> <span data-ttu-id="06bee-138">**Windows Azure som drivs av 21Vianet** är ett alternativ i Kina som använder hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="06bee-138">**Windows Azure operated by 21Vianet** is an option in China that uses hello classic deployment model.</span></span>

    <span data-ttu-id="06bee-139">Välj toodeploy i hello Resource Manager-modellen **Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="06bee-139">toodeploy in hello Resource Manager model, select **Microsoft Azure**.</span></span>

    ![Information om SAP CAL-konto](./media/cal-s4h/s4h-pic-2a.png)

3. <span data-ttu-id="06bee-141">Ange hello Azure **prenumerations-ID** som finns på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="06bee-141">Enter hello Azure **Subscription ID** that can be found on hello Azure portal.</span></span>

   ![SAP CAL-konton](./media/cal-s4h/s4h-pic3c.png)

4. <span data-ttu-id="06bee-143">tooauthorize hello SAP CAL toodeploy till hello Azure-prenumeration som du har definierat klickar du på **auktorisera**.</span><span class="sxs-lookup"><span data-stu-id="06bee-143">tooauthorize hello SAP CAL toodeploy into hello Azure subscription you defined, click **Authorize**.</span></span> <span data-ttu-id="06bee-144">hello följande sida visas i hello flik i webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="06bee-144">hello following page appears in hello browser tab:</span></span>

   ![Internet Explorer cloud services-inloggning](./media/cal-s4h/s4h-pic4c.png)

5. <span data-ttu-id="06bee-146">Om fler än en användare visas väljer du hello Microsoft-konto som är länkade toobe hello coadministrator av hello Azure-prenumeration du valt.</span><span class="sxs-lookup"><span data-stu-id="06bee-146">If more than one user is listed, choose hello Microsoft account that is linked toobe hello coadministrator of hello Azure subscription you selected.</span></span> <span data-ttu-id="06bee-147">hello följande sida visas i hello flik i webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="06bee-147">hello following page appears in hello browser tab:</span></span>

   ![Bekräftelse för Internet Explorer cloud services](./media/cal-s4h/s4h-pic5a.png)

6. <span data-ttu-id="06bee-149">Klicka på **acceptera**.</span><span class="sxs-lookup"><span data-stu-id="06bee-149">Click **Accept**.</span></span> <span data-ttu-id="06bee-150">Om hello auktorisering lyckas visar hello definition för SAP CAL-konto igen.</span><span class="sxs-lookup"><span data-stu-id="06bee-150">If hello authorization is successful, hello SAP CAL account definition displays again.</span></span> <span data-ttu-id="06bee-151">Efter en kort tid bekräftar ett meddelande att hello Auktoriseringen lyckades.</span><span class="sxs-lookup"><span data-stu-id="06bee-151">After a short time, a message confirms that hello authorization process was successful.</span></span>

7. <span data-ttu-id="06bee-152">tooassign hello nyskapad SAP CAL tooyour användarkonto, ange din **användar-ID** i hello hello rätt textrutan och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="06bee-152">tooassign hello newly created SAP CAL account tooyour user, enter your **User ID** in hello text box on hello right and click **Add**.</span></span>

   ![Toouser kontokoppling](./media/cal-s4h/s4h-pic8a.png)

8. <span data-ttu-id="06bee-154">tooassociate Klicka på ditt konto med hello användare att använda toosign i toohello SAP CAL **granska**.</span><span class="sxs-lookup"><span data-stu-id="06bee-154">tooassociate your account with hello user that you use toosign in toohello SAP CAL, click **Review**.</span></span> 
 
9. <span data-ttu-id="06bee-155">toocreate hello associationen mellan användaren och hello nyskapad SAP CAL-konto, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="06bee-155">toocreate hello association between your user and hello newly created SAP CAL account, click **Create**.</span></span>

   ![Användarassociationen tooSAP CAL-konto](./media/cal-s4h/s4h-pic9b.png)

<span data-ttu-id="06bee-157">Du har skapat ett SAP CAL-konto som kan:</span><span class="sxs-lookup"><span data-stu-id="06bee-157">You successfully created an SAP CAL account that is able to:</span></span>

- <span data-ttu-id="06bee-158">Använd hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="06bee-158">Use hello Resource Manager deployment model.</span></span>
- <span data-ttu-id="06bee-159">Distribuera SAP-system i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="06bee-159">Deploy SAP systems into your Azure subscription.</span></span>

<span data-ttu-id="06bee-160">Du kan nu starta toodeploy S/4HANA till din prenumeration för användare i Azure.</span><span class="sxs-lookup"><span data-stu-id="06bee-160">Now you can start toodeploy S/4HANA into your user subscription in Azure.</span></span>

> [!NOTE]
<span data-ttu-id="06bee-161">Innan du fortsätter kan du avgöra om du har Azure core kvoter för virtuella datorer i Azure H-serien.</span><span class="sxs-lookup"><span data-stu-id="06bee-161">Before you continue, determine whether you have Azure core quotas for Azure H-Series VMs.</span></span> <span data-ttu-id="06bee-162">Hello tillfället hello SAP CAL använder H-serien virtuella datorer i Azure toodeploy vissa hello SAP HANA-baserade lösningar.</span><span class="sxs-lookup"><span data-stu-id="06bee-162">At hello moment, hello SAP CAL uses H-Series VMs of Azure toodeploy some of hello SAP HANA-based solutions.</span></span> <span data-ttu-id="06bee-163">Din Azure-prenumeration kanske inte H-serien core kvoter för H-serien.</span><span class="sxs-lookup"><span data-stu-id="06bee-163">Your Azure subscription might not have any H-Series core quotas for H-Series.</span></span> <span data-ttu-id="06bee-164">I så fall kanske du måste toocontact Azure-supporten tooget en kvot på minst 16 H-serien kärnor.</span><span class="sxs-lookup"><span data-stu-id="06bee-164">If so, you might need toocontact Azure support tooget a quota of at least 16 H-Series cores.</span></span>

> [!NOTE]
<span data-ttu-id="06bee-165">När du distribuerar en lösning på Azure i hello SAP CAL kanske du upptäcker att du kan välja endast en Azure-region.</span><span class="sxs-lookup"><span data-stu-id="06bee-165">When you deploy a solution on Azure in hello SAP CAL, you might find that you can choose only one Azure region.</span></span> <span data-ttu-id="06bee-166">toodeploy i Azure-regioner än hello en föreslås av hello SAP CAL måste du toopurchase CAL prenumeration från SAP.</span><span class="sxs-lookup"><span data-stu-id="06bee-166">toodeploy into Azure regions other than hello one suggested by hello SAP CAL, you need toopurchase a CAL subscription from SAP.</span></span> <span data-ttu-id="06bee-167">Du måste kanske också tooopen ett meddelande med SAP toohave toodeliver din CAL-kontot är aktiverat i Azure-regioner än hello som ursprungligen förslag.</span><span class="sxs-lookup"><span data-stu-id="06bee-167">You also might need tooopen a message with SAP toohave your CAL account enabled toodeliver into Azure regions other than hello ones initially suggested.</span></span>

### <a name="deploy-a-solution"></a><span data-ttu-id="06bee-168">Distribuera en lösning</span><span class="sxs-lookup"><span data-stu-id="06bee-168">Deploy a solution</span></span>

<span data-ttu-id="06bee-169">Nu ska vi distribuera en lösning från hello **lösningar** sidan hello SAP CAL.</span><span class="sxs-lookup"><span data-stu-id="06bee-169">Let's deploy a solution from hello **Solutions** page of hello SAP CAL.</span></span> <span data-ttu-id="06bee-170">hello SAP CAL har två sekvenser toodeploy:</span><span class="sxs-lookup"><span data-stu-id="06bee-170">hello SAP CAL has two sequences toodeploy:</span></span>

- <span data-ttu-id="06bee-171">En grundläggande sekvens som använder en sida toodefine hello system toobe distribueras</span><span class="sxs-lookup"><span data-stu-id="06bee-171">A basic sequence that uses one page toodefine hello system toobe deployed</span></span>
- <span data-ttu-id="06bee-172">En avancerad sekvens som ger dig vissa alternativ på VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="06bee-172">An advanced sequence that gives you certain choices on VM sizes</span></span> 

<span data-ttu-id="06bee-173">Vi visar hello grundläggande sökvägen toodeployment här.</span><span class="sxs-lookup"><span data-stu-id="06bee-173">We demonstrate hello basic path toodeployment here.</span></span>

1. <span data-ttu-id="06bee-174">På hello **kontoinformation** sidan som du behöver:</span><span class="sxs-lookup"><span data-stu-id="06bee-174">On hello **Account Details** page, you need to:</span></span>

    <span data-ttu-id="06bee-175">a.</span><span class="sxs-lookup"><span data-stu-id="06bee-175">a.</span></span> <span data-ttu-id="06bee-176">Välj ett SAP CAL-konto.</span><span class="sxs-lookup"><span data-stu-id="06bee-176">Select an SAP CAL account.</span></span> <span data-ttu-id="06bee-177">(Använda ett konto som är associerade toodeploy med hello Resource Manager-distributionsmodellen.)</span><span class="sxs-lookup"><span data-stu-id="06bee-177">(Use an account that is associated toodeploy with hello Resource Manager deployment model.)</span></span>

    <span data-ttu-id="06bee-178">b.</span><span class="sxs-lookup"><span data-stu-id="06bee-178">b.</span></span> <span data-ttu-id="06bee-179">Ange en instans **namn**.</span><span class="sxs-lookup"><span data-stu-id="06bee-179">Enter an instance **Name**.</span></span>

    <span data-ttu-id="06bee-180">c.</span><span class="sxs-lookup"><span data-stu-id="06bee-180">c.</span></span> <span data-ttu-id="06bee-181">Välj en Azure **Region**.</span><span class="sxs-lookup"><span data-stu-id="06bee-181">Select an Azure **Region**.</span></span> <span data-ttu-id="06bee-182">hello SAP CAL föreslår en region.</span><span class="sxs-lookup"><span data-stu-id="06bee-182">hello SAP CAL suggests a region.</span></span> <span data-ttu-id="06bee-183">Om du behöver en annan Azure-region och du inte har en SAP CAL-prenumeration kan behöver du tooorder en CAL-prenumeration med SAP.</span><span class="sxs-lookup"><span data-stu-id="06bee-183">If you need another Azure region and you don't have an SAP CAL subscription, you need tooorder a CAL subscription with SAP.</span></span>

    <span data-ttu-id="06bee-184">d.</span><span class="sxs-lookup"><span data-stu-id="06bee-184">d.</span></span> <span data-ttu-id="06bee-185">Ange en **lösenord** för hello lösning av åtta eller nio tecken.</span><span class="sxs-lookup"><span data-stu-id="06bee-185">Enter a master **Password** for hello solution of eight or nine characters.</span></span> <span data-ttu-id="06bee-186">hello lösenordet används för hello administratörer av hello olika komponenter.</span><span class="sxs-lookup"><span data-stu-id="06bee-186">hello password is used for hello administrators of hello different components.</span></span>

   ![SAP CAL grundläggande läge: Skapa en instans](./media/cal-s4h/s4h-pic10a.png)

2. <span data-ttu-id="06bee-188">Klicka på **skapa**, och i hello meddelanderutan som visas, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="06bee-188">Click **Create**, and in hello message box that appears, click **OK**.</span></span>

   ![SAP CAL VM-storlekar som stöds](./media/cal-s4h/s4h-pic10b.png)

3. <span data-ttu-id="06bee-190">I hello **privat nyckel** dialogrutan klickar du på **Store** toostore hello privata nyckeln i hello SAP CAL.</span><span class="sxs-lookup"><span data-stu-id="06bee-190">In hello **Private Key** dialog box, click **Store** toostore hello private key in hello SAP CAL.</span></span> <span data-ttu-id="06bee-191">toouse lösenordsskydd för hello privata nyckel, klickar du på **hämta**.</span><span class="sxs-lookup"><span data-stu-id="06bee-191">toouse password protection for hello private key, click **Download**.</span></span> 

   ![SAP CAL privat nyckel](./media/cal-s4h/s4h-pic10c.png)

4. <span data-ttu-id="06bee-193">Läs hello SAP CAL **varning** meddelande och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="06bee-193">Read hello SAP CAL **Warning** message, and click **OK**.</span></span>

   ![SAP CAL-varning](./media/cal-s4h/s4h-pic10d.png)

    <span data-ttu-id="06bee-195">Nu hello distribution sker.</span><span class="sxs-lookup"><span data-stu-id="06bee-195">Now hello deployment takes place.</span></span> <span data-ttu-id="06bee-196">Efter en stund, beroende på hello storleken och komplexiteten för hello-lösning (hello SAP CAL ger en uppskattning), visas hello status som aktiv och redo för användning.</span><span class="sxs-lookup"><span data-stu-id="06bee-196">After some time, depending on hello size and complexity of hello solution (hello SAP CAL provides an estimate), hello status is shown as active and ready for use.</span></span>

5. <span data-ttu-id="06bee-197">toofind hello virtuella datorer som har samlats in med Hej andra associerade resurser i en resursgrupp, gå toohello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="06bee-197">toofind hello virtual machines collected with hello other associated resources in one resource group, go toohello Azure portal:</span></span> 

   ![SAP CAL-objekt som distribuerats i nya hello-portalen](./media/cal-s4h/sapcaldeplyment_portalview.png)

6. <span data-ttu-id="06bee-199">På hello SAP CAL-portalen hello status visas som **Active**.</span><span class="sxs-lookup"><span data-stu-id="06bee-199">On hello SAP CAL portal, hello status appears as **Active**.</span></span> <span data-ttu-id="06bee-200">tooconnect toohello lösning, klickar du på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="06bee-200">tooconnect toohello solution, click **Connect**.</span></span> <span data-ttu-id="06bee-201">Olika alternativ tooconnect toohello olika komponenter har distribuerats i den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="06bee-201">Different options tooconnect toohello different components are deployed within this solution.</span></span>

   ![SAP CAL-instanser](./media/cal-s4h/active_solution.png)

7. <span data-ttu-id="06bee-203">Innan du kan använda ett av hello alternativ tooconnect toohello distribuerade system, klickar du på **komma igång**.</span><span class="sxs-lookup"><span data-stu-id="06bee-203">Before you can use one of hello options tooconnect toohello deployed systems, click **Getting Started Guide**.</span></span> 

   ![Ansluta toohello instans](./media/cal-s4h/connect_to_solution.png)

    <span data-ttu-id="06bee-205">hello dokumentationen namn hello användare för varje hello anslutningsmetoder.</span><span class="sxs-lookup"><span data-stu-id="06bee-205">hello documentation names hello users for each of hello connectivity methods.</span></span> <span data-ttu-id="06bee-206">hello lösenorden för dessa användare ställs toohello lösenord du har definierat hello början av hello distributionsprocessen.</span><span class="sxs-lookup"><span data-stu-id="06bee-206">hello passwords for those users are set toohello master password you defined at hello beginning of hello deployment process.</span></span> <span data-ttu-id="06bee-207">I dokumentationen för hello andra mer funktionell användare visas med sina lösenord, som du kan använda toosign i toohello distribuerade system.</span><span class="sxs-lookup"><span data-stu-id="06bee-207">In hello documentation, other more functional users are listed with their passwords, which you can use toosign in toohello deployed system.</span></span> 

    <span data-ttu-id="06bee-208">Till exempel om du använder hello SAP grafiskt användargränssnitt som är förinstallerat på hello Windows Fjärrskrivbord datorn hello S/4 system kan se ut så här:</span><span class="sxs-lookup"><span data-stu-id="06bee-208">For example, if you use hello SAP GUI that's preinstalled on hello Windows Remote Desktop machine, hello S/4 system might look like this:</span></span>

   ![SM50 i hello förinstallerat SAP GUI](./media/cal-s4h/gui_sm50.png)

    <span data-ttu-id="06bee-210">Eller om du använder hello DBACockpit hello instans kan se ut så här:</span><span class="sxs-lookup"><span data-stu-id="06bee-210">Or if you use hello DBACockpit, hello instance might look like this:</span></span>

   ![SM50 i hello DBACockpit SAP GUI](./media/cal-s4h/dbacockpit.png)

<span data-ttu-id="06bee-212">En felfri SAP S/4 installation distribueras inom några timmar i Azure.</span><span class="sxs-lookup"><span data-stu-id="06bee-212">Within a few hours, a healthy SAP S/4 appliance is deployed in Azure.</span></span>

<span data-ttu-id="06bee-213">Om du har köpt en prenumeration för SAP CAL stöd SAP fullständigt för distribution via hello SAP CAL på Azure.</span><span class="sxs-lookup"><span data-stu-id="06bee-213">If you bought an SAP CAL subscription, SAP fully supports deployments through hello SAP CAL on Azure.</span></span> <span data-ttu-id="06bee-214">hello stöd kön är BC-VCM-klientåtkomstlicens.</span><span class="sxs-lookup"><span data-stu-id="06bee-214">hello support queue is BC-VCM-CAL.</span></span>







