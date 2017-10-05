---
title: "Distribuera SAP S/4HANA eller BW/4HANA på en virtuell dator i Azure | Microsoft Docs"
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
ms.openlocfilehash: 4788fa14a6c49d39b5a3096a69b6738f4a5d8cca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-sap-s4hana-or-bw4hana-on-azure"></a><span data-ttu-id="61448-103">Distribuera SAP S/4HANA eller BW/4HANA på Azure</span><span class="sxs-lookup"><span data-stu-id="61448-103">Deploy SAP S/4HANA or BW/4HANA on Azure</span></span>
<span data-ttu-id="61448-104">Den här artikeln beskriver hur du distribuerar S/4HANA på Azure med hjälp av SAP-installation Molnbibliotek (SAP CAL) 3.0.</span><span class="sxs-lookup"><span data-stu-id="61448-104">This article describes how to deploy S/4HANA on Azure by using the SAP Cloud Appliance Library (SAP CAL) 3.0.</span></span> <span data-ttu-id="61448-105">Följ samma steg för att distribuera andra SAP HANA-baserade lösningar, till exempel BW/4HANA.</span><span class="sxs-lookup"><span data-stu-id="61448-105">To deploy other SAP HANA-based solutions, such as BW/4HANA, follow the same steps.</span></span>

> [!NOTE]
<span data-ttu-id="61448-106">Mer information om SAP-CAL går du till den [SAP installation Molnbibliotek](https://cal.sap.com/) webbplats.</span><span class="sxs-lookup"><span data-stu-id="61448-106">For more information about the SAP CAL, go to the [SAP Cloud Appliance Library](https://cal.sap.com/) website.</span></span> <span data-ttu-id="61448-107">SAP har också en blogg om den [SAP molnet installation biblioteket 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).</span><span class="sxs-lookup"><span data-stu-id="61448-107">SAP also has a blog about the [SAP Cloud Appliance Library 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).</span></span>

> [!NOTE]
<span data-ttu-id="61448-108">Du kan använda Azure Resource Manager-distributionsmodellen utöver den önskade mindre klassiska distributionsmodellen för att distribuera SAP-CAL från och med den 29 maj 2017.</span><span class="sxs-lookup"><span data-stu-id="61448-108">As of May 29, 2017, you can use the Azure Resource Manager deployment model in addition to the less-preferred classic deployment model to deploy the SAP CAL.</span></span> <span data-ttu-id="61448-109">Vi rekommenderar att du använder den nya Resource Manager-distributionsmodellen och bortse från den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="61448-109">We recommend that you use the new Resource Manager deployment model and disregard the classic deployment model.</span></span>

## <a name="step-by-step-process-to-deploy-the-solution"></a><span data-ttu-id="61448-110">Steg för steg hur du distribuerar lösningen</span><span class="sxs-lookup"><span data-stu-id="61448-110">Step-by-step process to deploy the solution</span></span>

<span data-ttu-id="61448-111">Följande sekvens av skärmdumpar visar hur du distribuerar S/4HANA på Azure med hjälp av SAP-CAL.</span><span class="sxs-lookup"><span data-stu-id="61448-111">The following sequence of screenshots shows you how to deploy S/4HANA on Azure by using the SAP CAL.</span></span> <span data-ttu-id="61448-112">Processen fungerar på samma sätt som andra lösningar, till exempel BW/4HANA.</span><span class="sxs-lookup"><span data-stu-id="61448-112">The process works the same way for other solutions, such as BW/4HANA.</span></span>

<span data-ttu-id="61448-113">Den **lösningar** sidan visar några av de tillgängliga SAP HANA för CAL-baserade lösningarna på Azure.</span><span class="sxs-lookup"><span data-stu-id="61448-113">The **Solutions** page shows some of the SAP CAL HANA-based solutions available on Azure.</span></span> <span data-ttu-id="61448-114">**SAP S/4HANA 1610 FPS01, Fully-Activated installation** i mitten rad:</span><span class="sxs-lookup"><span data-stu-id="61448-114">**SAP S/4HANA 1610 FPS01, Fully-Activated Appliance** is in the middle row:</span></span>

![SAP CAL-lösningar](./media/cal-s4h/s4h-pic-1c.png)

### <a name="create-an-account-in-the-sap-cal"></a><span data-ttu-id="61448-116">Skapa ett konto i SAP-CAL</span><span class="sxs-lookup"><span data-stu-id="61448-116">Create an account in the SAP CAL</span></span>
1. <span data-ttu-id="61448-117">Om du vill logga in till SAP-CAL för första gången använder din SAP-S-användare eller en annan användare har registrerat hos SAP.</span><span class="sxs-lookup"><span data-stu-id="61448-117">To sign in to the SAP CAL for the first time, use your SAP S-User or other user registered with SAP.</span></span> <span data-ttu-id="61448-118">Definiera ett SAP CAL-konto som används av SAP-CAL för att distribuera installationer på Azure.</span><span class="sxs-lookup"><span data-stu-id="61448-118">Then define an SAP CAL account that is used by the SAP CAL to deploy appliances on Azure.</span></span> <span data-ttu-id="61448-119">I definitionen konto måste du:</span><span class="sxs-lookup"><span data-stu-id="61448-119">In the account definition, you need to:</span></span>

    <span data-ttu-id="61448-120">a.</span><span class="sxs-lookup"><span data-stu-id="61448-120">a.</span></span> <span data-ttu-id="61448-121">Välj distributionsmodell i Azure (Resource Manager eller klassisk).</span><span class="sxs-lookup"><span data-stu-id="61448-121">Select the deployment model on Azure (Resource Manager or classic).</span></span>

    <span data-ttu-id="61448-122">b.</span><span class="sxs-lookup"><span data-stu-id="61448-122">b.</span></span> <span data-ttu-id="61448-123">Ange din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="61448-123">Enter your Azure subscription.</span></span> <span data-ttu-id="61448-124">En SAP CAL-konto kan tilldelas till en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="61448-124">An SAP CAL account can be assigned to one subscription only.</span></span> <span data-ttu-id="61448-125">Om du behöver mer än en prenumeration måste du skapa en annan SAP CAL-konto.</span><span class="sxs-lookup"><span data-stu-id="61448-125">If you need more than one subscription, you need to create another SAP CAL account.</span></span>

    <span data-ttu-id="61448-126">c.</span><span class="sxs-lookup"><span data-stu-id="61448-126">c.</span></span> <span data-ttu-id="61448-127">Ge SAP CAL-behörighet att distribuera till din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="61448-127">Give the SAP CAL permission to deploy into your Azure subscription.</span></span>

    > [!NOTE]
    <span data-ttu-id="61448-128">Nästa steg visar hur du skapar ett SAP CAL-konto för Resource Manager distributioner.</span><span class="sxs-lookup"><span data-stu-id="61448-128">The next steps show how to create an SAP CAL account for Resource Manager deployments.</span></span> <span data-ttu-id="61448-129">Om du redan har ett SAP CAL-konto som är länkad till den klassiska distributionsmodellen du *måste* att följa dessa steg om du vill skapa ett nytt SAP CAL-konto.</span><span class="sxs-lookup"><span data-stu-id="61448-129">If you already have an SAP CAL account that is linked to the classic deployment model, you *need* to follow these steps to create a new SAP CAL account.</span></span> <span data-ttu-id="61448-130">Det nya SAP CAL-kontot måste distribueras i Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="61448-130">The new SAP CAL account needs to deploy in the Resource Manager model.</span></span>

2. <span data-ttu-id="61448-131">Skapa ett nytt SAP CAL-konto.</span><span class="sxs-lookup"><span data-stu-id="61448-131">Create a new SAP CAL account.</span></span> <span data-ttu-id="61448-132">Den **konton** sidan visas tre alternativ för Azure:</span><span class="sxs-lookup"><span data-stu-id="61448-132">The **Accounts** page shows three choices for Azure:</span></span> 

    <span data-ttu-id="61448-133">a.</span><span class="sxs-lookup"><span data-stu-id="61448-133">a.</span></span> <span data-ttu-id="61448-134">**Microsoft Azure (klassisk)** är den klassiska distributionsmodellen och är inte längre önskade.</span><span class="sxs-lookup"><span data-stu-id="61448-134">**Microsoft Azure (classic)** is the classic deployment model and is no longer preferred.</span></span>

    <span data-ttu-id="61448-135">b.</span><span class="sxs-lookup"><span data-stu-id="61448-135">b.</span></span> <span data-ttu-id="61448-136">**Microsoft Azure** är den nya Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="61448-136">**Microsoft Azure** is the new Resource Manager deployment model.</span></span>

    <span data-ttu-id="61448-137">c.</span><span class="sxs-lookup"><span data-stu-id="61448-137">c.</span></span> <span data-ttu-id="61448-138">**Windows Azure som drivs av 21Vianet** är ett alternativ i Kina som använder den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="61448-138">**Windows Azure operated by 21Vianet** is an option in China that uses the classic deployment model.</span></span>

    <span data-ttu-id="61448-139">Om du vill distribuera i Resource Manager-modellen, Välj **Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="61448-139">To deploy in the Resource Manager model, select **Microsoft Azure**.</span></span>

    ![Information om SAP CAL-konto](./media/cal-s4h/s4h-pic-2a.png)

3. <span data-ttu-id="61448-141">Ange Azure **prenumerations-ID** som finns på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="61448-141">Enter the Azure **Subscription ID** that can be found on the Azure portal.</span></span>

   ![SAP CAL-konton](./media/cal-s4h/s4h-pic3c.png)

4. <span data-ttu-id="61448-143">För att godkänna SAP-CAL att distribuera till Azure-prenumerationen du definierat klickar **auktorisera**.</span><span class="sxs-lookup"><span data-stu-id="61448-143">To authorize the SAP CAL to deploy into the Azure subscription you defined, click **Authorize**.</span></span> <span data-ttu-id="61448-144">Följande sida visas i fliken i webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="61448-144">The following page appears in the browser tab:</span></span>

   ![Internet Explorer cloud services-inloggning](./media/cal-s4h/s4h-pic4c.png)

5. <span data-ttu-id="61448-146">Om fler än en användare visas väljer du Microsoft-kontot som är kopplad till att coadministrator av Azure-prenumerationen du valt.</span><span class="sxs-lookup"><span data-stu-id="61448-146">If more than one user is listed, choose the Microsoft account that is linked to be the coadministrator of the Azure subscription you selected.</span></span> <span data-ttu-id="61448-147">Följande sida visas i fliken i webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="61448-147">The following page appears in the browser tab:</span></span>

   ![Bekräftelse för Internet Explorer cloud services](./media/cal-s4h/s4h-pic5a.png)

6. <span data-ttu-id="61448-149">Klicka på **acceptera**.</span><span class="sxs-lookup"><span data-stu-id="61448-149">Click **Accept**.</span></span> <span data-ttu-id="61448-150">Om tillståndet lyckas visar definition för SAP CAL-konto igen.</span><span class="sxs-lookup"><span data-stu-id="61448-150">If the authorization is successful, the SAP CAL account definition displays again.</span></span> <span data-ttu-id="61448-151">Efter en kort tid ett meddelande som bekräftar att Auktoriseringen lyckades.</span><span class="sxs-lookup"><span data-stu-id="61448-151">After a short time, a message confirms that the authorization process was successful.</span></span>

7. <span data-ttu-id="61448-152">Om du vill tilldela användaren det nyligen skapade SAP CAL-kontot, ange din **användar-ID** i rutan till höger och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="61448-152">To assign the newly created SAP CAL account to your user, enter your **User ID** in the text box on the right and click **Add**.</span></span>

   ![Kontot till användarassociationen](./media/cal-s4h/s4h-pic8a.png)

8. <span data-ttu-id="61448-154">Om du vill koppla ditt konto till den användare som du använder för att logga in på SAP-CAL, klickar du på **granska**.</span><span class="sxs-lookup"><span data-stu-id="61448-154">To associate your account with the user that you use to sign in to the SAP CAL, click **Review**.</span></span> 
 
9. <span data-ttu-id="61448-155">Om du vill skapa kopplingen mellan användaren och det nyligen skapade SAP CAL-kontot, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="61448-155">To create the association between your user and the newly created SAP CAL account, click **Create**.</span></span>

   ![Användaren till SAP CAL-kontokoppling](./media/cal-s4h/s4h-pic9b.png)

<span data-ttu-id="61448-157">Du har skapat ett SAP CAL-konto som kan:</span><span class="sxs-lookup"><span data-stu-id="61448-157">You successfully created an SAP CAL account that is able to:</span></span>

- <span data-ttu-id="61448-158">Använd Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="61448-158">Use the Resource Manager deployment model.</span></span>
- <span data-ttu-id="61448-159">Distribuera SAP-system i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="61448-159">Deploy SAP systems into your Azure subscription.</span></span>

<span data-ttu-id="61448-160">Nu kan du börja distribuera S/4HANA i din prenumeration för användare i Azure.</span><span class="sxs-lookup"><span data-stu-id="61448-160">Now you can start to deploy S/4HANA into your user subscription in Azure.</span></span>

> [!NOTE]
<span data-ttu-id="61448-161">Innan du fortsätter kan du avgöra om du har Azure core kvoter för virtuella datorer i Azure H-serien.</span><span class="sxs-lookup"><span data-stu-id="61448-161">Before you continue, determine whether you have Azure core quotas for Azure H-Series VMs.</span></span> <span data-ttu-id="61448-162">För tillfället använder SAP-CAL H-serien virtuella datorer i Azure för att distribuera vissa SAP HANA-baserade lösningar.</span><span class="sxs-lookup"><span data-stu-id="61448-162">At the moment, the SAP CAL uses H-Series VMs of Azure to deploy some of the SAP HANA-based solutions.</span></span> <span data-ttu-id="61448-163">Din Azure-prenumeration kanske inte H-serien core kvoter för H-serien.</span><span class="sxs-lookup"><span data-stu-id="61448-163">Your Azure subscription might not have any H-Series core quotas for H-Series.</span></span> <span data-ttu-id="61448-164">I så fall, kan du behöva kontakta Azure-supporten för att få en kvot på minst 16 H-serien kärnor.</span><span class="sxs-lookup"><span data-stu-id="61448-164">If so, you might need to contact Azure support to get a quota of at least 16 H-Series cores.</span></span>

> [!NOTE]
<span data-ttu-id="61448-165">När du distribuerar en lösning på Azure i SAP-CAL kanske du upptäcker att du kan välja endast en Azure-region.</span><span class="sxs-lookup"><span data-stu-id="61448-165">When you deploy a solution on Azure in the SAP CAL, you might find that you can choose only one Azure region.</span></span> <span data-ttu-id="61448-166">Om du vill distribuera till Azure-regioner än den föreslagna av SAP-CAL, måste du köpa en prenumeration för Fjärrskrivbordstjänster från SAP.</span><span class="sxs-lookup"><span data-stu-id="61448-166">To deploy into Azure regions other than the one suggested by the SAP CAL, you need to purchase a CAL subscription from SAP.</span></span> <span data-ttu-id="61448-167">Du kan behöva öppna ett meddelande med SAP till ditt CAL-konto som är aktiverad för att leverera i Azure-regioner än de som ursprungligen förslag.</span><span class="sxs-lookup"><span data-stu-id="61448-167">You also might need to open a message with SAP to have your CAL account enabled to deliver into Azure regions other than the ones initially suggested.</span></span>

### <a name="deploy-a-solution"></a><span data-ttu-id="61448-168">Distribuera en lösning</span><span class="sxs-lookup"><span data-stu-id="61448-168">Deploy a solution</span></span>

<span data-ttu-id="61448-169">Nu ska vi distribuera en lösning från den **lösningar** sidan i SAP-klientåtkomstlicens.</span><span class="sxs-lookup"><span data-stu-id="61448-169">Let's deploy a solution from the **Solutions** page of the SAP CAL.</span></span> <span data-ttu-id="61448-170">SAP-CAL har två sekvenser att distribuera:</span><span class="sxs-lookup"><span data-stu-id="61448-170">The SAP CAL has two sequences to deploy:</span></span>

- <span data-ttu-id="61448-171">En grundläggande sekvens som använder en sida för att definiera system som ska distribueras</span><span class="sxs-lookup"><span data-stu-id="61448-171">A basic sequence that uses one page to define the system to be deployed</span></span>
- <span data-ttu-id="61448-172">En avancerad sekvens som ger dig vissa alternativ på VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="61448-172">An advanced sequence that gives you certain choices on VM sizes</span></span> 

<span data-ttu-id="61448-173">Vi visar grundläggande sökvägen till den här distributionen.</span><span class="sxs-lookup"><span data-stu-id="61448-173">We demonstrate the basic path to deployment here.</span></span>

1. <span data-ttu-id="61448-174">På den **kontoinformation** sidan som du behöver:</span><span class="sxs-lookup"><span data-stu-id="61448-174">On the **Account Details** page, you need to:</span></span>

    <span data-ttu-id="61448-175">a.</span><span class="sxs-lookup"><span data-stu-id="61448-175">a.</span></span> <span data-ttu-id="61448-176">Välj ett SAP CAL-konto.</span><span class="sxs-lookup"><span data-stu-id="61448-176">Select an SAP CAL account.</span></span> <span data-ttu-id="61448-177">(Använda ett konto som är kopplad till distribuera med Resource Manager-distributionsmodellen.)</span><span class="sxs-lookup"><span data-stu-id="61448-177">(Use an account that is associated to deploy with the Resource Manager deployment model.)</span></span>

    <span data-ttu-id="61448-178">b.</span><span class="sxs-lookup"><span data-stu-id="61448-178">b.</span></span> <span data-ttu-id="61448-179">Ange en instans **namn**.</span><span class="sxs-lookup"><span data-stu-id="61448-179">Enter an instance **Name**.</span></span>

    <span data-ttu-id="61448-180">c.</span><span class="sxs-lookup"><span data-stu-id="61448-180">c.</span></span> <span data-ttu-id="61448-181">Välj en Azure **Region**.</span><span class="sxs-lookup"><span data-stu-id="61448-181">Select an Azure **Region**.</span></span> <span data-ttu-id="61448-182">SAP-CAL föreslår en region.</span><span class="sxs-lookup"><span data-stu-id="61448-182">The SAP CAL suggests a region.</span></span> <span data-ttu-id="61448-183">Om du behöver en annan Azure-region och du inte har en SAP CAL-prenumeration kan behöver du ordna en CAL-prenumeration med SAP.</span><span class="sxs-lookup"><span data-stu-id="61448-183">If you need another Azure region and you don't have an SAP CAL subscription, you need to order a CAL subscription with SAP.</span></span>

    <span data-ttu-id="61448-184">d.</span><span class="sxs-lookup"><span data-stu-id="61448-184">d.</span></span> <span data-ttu-id="61448-185">Ange en **lösenord** för lösning av åtta eller nio tecken.</span><span class="sxs-lookup"><span data-stu-id="61448-185">Enter a master **Password** for the solution of eight or nine characters.</span></span> <span data-ttu-id="61448-186">Lösenordet som används för administratörerna för de olika komponenterna.</span><span class="sxs-lookup"><span data-stu-id="61448-186">The password is used for the administrators of the different components.</span></span>

   ![SAP CAL grundläggande läge: Skapa en instans](./media/cal-s4h/s4h-pic10a.png)

2. <span data-ttu-id="61448-188">Klicka på **skapa**, och i meddelanderutan som visas på **OK**.</span><span class="sxs-lookup"><span data-stu-id="61448-188">Click **Create**, and in the message box that appears, click **OK**.</span></span>

   ![SAP CAL VM-storlekar som stöds](./media/cal-s4h/s4h-pic10b.png)

3. <span data-ttu-id="61448-190">I den **privat nyckel** dialogrutan klickar du på **lagra** att lagra den privata nyckeln i SAP-klientåtkomstlicens.</span><span class="sxs-lookup"><span data-stu-id="61448-190">In the **Private Key** dialog box, click **Store** to store the private key in the SAP CAL.</span></span> <span data-ttu-id="61448-191">Om du vill använda lösenordsskydd för den privata nyckeln **hämta**.</span><span class="sxs-lookup"><span data-stu-id="61448-191">To use password protection for the private key, click **Download**.</span></span> 

   ![SAP CAL privat nyckel](./media/cal-s4h/s4h-pic10c.png)

4. <span data-ttu-id="61448-193">Läsa SAP-CAL **varning** meddelande och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="61448-193">Read the SAP CAL **Warning** message, and click **OK**.</span></span>

   ![SAP CAL-varning](./media/cal-s4h/s4h-pic10d.png)

    <span data-ttu-id="61448-195">Nu sker distributionen.</span><span class="sxs-lookup"><span data-stu-id="61448-195">Now the deployment takes place.</span></span> <span data-ttu-id="61448-196">Efter en stund, beroende på storleken och komplexiteten för lösning (SAP-CAL ger en uppskattning), visas status som aktiv och redo för användning.</span><span class="sxs-lookup"><span data-stu-id="61448-196">After some time, depending on the size and complexity of the solution (the SAP CAL provides an estimate), the status is shown as active and ready for use.</span></span>

5. <span data-ttu-id="61448-197">Gå till Azure-portalen för att hitta de virtuella datorerna som samlats in med andra associerade resurser i en resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="61448-197">To find the virtual machines collected with the other associated resources in one resource group, go to the Azure portal:</span></span> 

   ![SAP CAL-objekt som distribuerats i den nya portalen](./media/cal-s4h/sapcaldeplyment_portalview.png)

6. <span data-ttu-id="61448-199">På portalen SAP CAL status visas som **Active**.</span><span class="sxs-lookup"><span data-stu-id="61448-199">On the SAP CAL portal, the status appears as **Active**.</span></span> <span data-ttu-id="61448-200">Anslut till lösningen **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="61448-200">To connect to the solution, click **Connect**.</span></span> <span data-ttu-id="61448-201">Olika alternativ för att ansluta till de olika komponenterna distribueras i den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="61448-201">Different options to connect to the different components are deployed within this solution.</span></span>

   ![SAP CAL-instanser](./media/cal-s4h/active_solution.png)

7. <span data-ttu-id="61448-203">Innan du kan använda något av alternativen för att ansluta till distribuerade system, klickar du på **komma igång**.</span><span class="sxs-lookup"><span data-stu-id="61448-203">Before you can use one of the options to connect to the deployed systems, click **Getting Started Guide**.</span></span> 

   ![Anslut till instansen](./media/cal-s4h/connect_to_solution.png)

    <span data-ttu-id="61448-205">I dokumentationen för att användarna för varje anslutningsmetoder.</span><span class="sxs-lookup"><span data-stu-id="61448-205">The documentation names the users for each of the connectivity methods.</span></span> <span data-ttu-id="61448-206">Lösenorden för dessa användare har angetts till master lösenordet som du angav i början av distributionsprocessen.</span><span class="sxs-lookup"><span data-stu-id="61448-206">The passwords for those users are set to the master password you defined at the beginning of the deployment process.</span></span> <span data-ttu-id="61448-207">I dokumentationen för visas andra mer funktionell användare med sina lösenord, som du kan använda för att logga in på det distribuerade systemet.</span><span class="sxs-lookup"><span data-stu-id="61448-207">In the documentation, other more functional users are listed with their passwords, which you can use to sign in to the deployed system.</span></span> 

    <span data-ttu-id="61448-208">Till exempel om du använder SAP-Användargränssnittet som är förinstallerat på datorn i Windows Remote Desktop S/4 system kan se ut så här:</span><span class="sxs-lookup"><span data-stu-id="61448-208">For example, if you use the SAP GUI that's preinstalled on the Windows Remote Desktop machine, the S/4 system might look like this:</span></span>

   ![SM50 i förinstallerade SAP-Gränssnittet](./media/cal-s4h/gui_sm50.png)

    <span data-ttu-id="61448-210">Eller om du använder DBACockpit instansen kan se ut så här:</span><span class="sxs-lookup"><span data-stu-id="61448-210">Or if you use the DBACockpit, the instance might look like this:</span></span>

   ![SM50 i det grafiska Användargränssnittet SAP DBACockpit](./media/cal-s4h/dbacockpit.png)

<span data-ttu-id="61448-212">En felfri SAP S/4 installation distribueras inom några timmar i Azure.</span><span class="sxs-lookup"><span data-stu-id="61448-212">Within a few hours, a healthy SAP S/4 appliance is deployed in Azure.</span></span>

<span data-ttu-id="61448-213">Om du har köpt en prenumeration för SAP CAL stöd SAP fullständigt för distribution via SAP-CAL på Azure.</span><span class="sxs-lookup"><span data-stu-id="61448-213">If you bought an SAP CAL subscription, SAP fully supports deployments through the SAP CAL on Azure.</span></span> <span data-ttu-id="61448-214">Stöd för kön är BC-VCM-klientåtkomstlicens.</span><span class="sxs-lookup"><span data-stu-id="61448-214">The support queue is BC-VCM-CAL.</span></span>







