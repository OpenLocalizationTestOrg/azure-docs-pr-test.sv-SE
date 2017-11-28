---
title: "aaaDeploy SAP IDES EHP7 SP3 för SAP ERP 6.0 i Azure | Microsoft Docs"
description: "Distribuera SAP IDES EHP7 SP3 för SAP ERP 6.0 på Azure"
services: virtual-machines-windows
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 626c1523-1026-478f-bd8a-22c83b869231
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/16/2016
ms.author: hermannd
ms.openlocfilehash: 26d88c7b48a91d35602464c4f89ca7a30502c4b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-sap-ides-ehp7-sp3-for-sap-erp-60-on-azure"></a><span data-ttu-id="b5a84-103">Distribuera SAP IDES EHP7 SP3 för SAP ERP 6.0 på Azure</span><span class="sxs-lookup"><span data-stu-id="b5a84-103">Deploy SAP IDES EHP7 SP3 for SAP ERP 6.0 on Azure</span></span>
<span data-ttu-id="b5a84-104">Den här artikeln beskriver hur toodeploy SAP IDES system körs med SQL Server och hello Windows-operativsystemet på Azure via hello SAP Molnbibliotek installation (SAP CAL) 3.0.</span><span class="sxs-lookup"><span data-stu-id="b5a84-104">This article describes how toodeploy an SAP IDES system running with SQL Server and hello Windows operating system on Azure via hello SAP Cloud Appliance Library (SAP CAL) 3.0.</span></span> <span data-ttu-id="b5a84-105">hello skärmdumpar visar hello steg för steg.</span><span class="sxs-lookup"><span data-stu-id="b5a84-105">hello screenshots show hello step-by-step process.</span></span> <span data-ttu-id="b5a84-106">Följ toodeploy en annan lösning hello samma steg.</span><span class="sxs-lookup"><span data-stu-id="b5a84-106">toodeploy a different solution, follow hello same steps.</span></span>

<span data-ttu-id="b5a84-107">toostart med hello SAP CAL, gå toohello [SAP installation Molnbibliotek](https://cal.sap.com/) webbplats.</span><span class="sxs-lookup"><span data-stu-id="b5a84-107">toostart with hello SAP CAL, go toohello [SAP Cloud Appliance Library](https://cal.sap.com/) website.</span></span> <span data-ttu-id="b5a84-108">SAP har också en blogg om hello nya [SAP molnet installation biblioteket 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).</span><span class="sxs-lookup"><span data-stu-id="b5a84-108">SAP also has a blog about hello new [SAP Cloud Appliance Library 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).</span></span> 

> [!NOTE]
<span data-ttu-id="b5a84-109">Eftersom den 29 maj 2017 kan du använda hello Azure Resource Manager-distributionsmodellen dessutom toohello mindre önskade klassisk distribution modell toodeploy hello SAP CAL.</span><span class="sxs-lookup"><span data-stu-id="b5a84-109">As of May 29, 2017, you can use hello Azure Resource Manager deployment model in addition toohello less-preferred classic deployment model toodeploy hello SAP CAL.</span></span> <span data-ttu-id="b5a84-110">Vi rekommenderar att du använder hello nya Resource Manager-modellen och ignorera hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="b5a84-110">We recommend that you use hello new Resource Manager deployment model and disregard hello classic deployment model.</span></span>

<span data-ttu-id="b5a84-111">Om du redan skapat ett SAP CAL-konto som använder hello klassiska modellen, *toocreate måste en annan SAP CAL-konto*.</span><span class="sxs-lookup"><span data-stu-id="b5a84-111">If you already created an SAP CAL account that uses hello classic model, *you need toocreate another SAP CAL account*.</span></span> <span data-ttu-id="b5a84-112">Det här kontot måste tooexclusively distribuera till Azure med hjälp av hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="b5a84-112">This account needs tooexclusively deploy into Azure by using hello Resource Manager model.</span></span>

<span data-ttu-id="b5a84-113">När du loggar in toohello SAP CAL hello första sidan normalt leder dig toohello **lösningar** sidan.</span><span class="sxs-lookup"><span data-stu-id="b5a84-113">After you sign in toohello SAP CAL, hello first page usually leads you toohello **Solutions** page.</span></span> <span data-ttu-id="b5a84-114">hello-lösningar som erbjuds via hello SAP CAL ökar stadigt, så du kan behöva tooscroll mycket lite toofind hello lösningen som du söker.</span><span class="sxs-lookup"><span data-stu-id="b5a84-114">hello solutions offered on hello SAP CAL are steadily increasing, so you might need tooscroll quite a bit toofind hello solution you want.</span></span> <span data-ttu-id="b5a84-115">hello markerade Windows-baserade SAP IDES lösning som är tillgänglig enbart på Azure visar hello distributionsprocessen:</span><span class="sxs-lookup"><span data-stu-id="b5a84-115">hello highlighted Windows-based SAP IDES solution that is available exclusively on Azure demonstrates hello deployment process:</span></span>

![SAP CAL-lösningar](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic1.jpg)

### <a name="create-an-account-in-hello-sap-cal"></a><span data-ttu-id="b5a84-117">Skapa ett konto i hello SAP CAL</span><span class="sxs-lookup"><span data-stu-id="b5a84-117">Create an account in hello SAP CAL</span></span>
1. <span data-ttu-id="b5a84-118">toosign i toohello SAP CAL för hello första gången använder din SAP-S-användare eller en annan användare som har registrerats med SAP.</span><span class="sxs-lookup"><span data-stu-id="b5a84-118">toosign in toohello SAP CAL for hello first time, use your SAP S-User or other user registered with SAP.</span></span> <span data-ttu-id="b5a84-119">Definiera ett SAP CAL-konto som används av hello SAP CAL toodeploy installationer på Azure.</span><span class="sxs-lookup"><span data-stu-id="b5a84-119">Then define an SAP CAL account that is used by hello SAP CAL toodeploy appliances on Azure.</span></span> <span data-ttu-id="b5a84-120">I hello konto definition behöver du:</span><span class="sxs-lookup"><span data-stu-id="b5a84-120">In hello account definition, you need to:</span></span>

    <span data-ttu-id="b5a84-121">a.</span><span class="sxs-lookup"><span data-stu-id="b5a84-121">a.</span></span> <span data-ttu-id="b5a84-122">Välj hello distributionsmodell i Azure (Resource Manager eller klassisk).</span><span class="sxs-lookup"><span data-stu-id="b5a84-122">Select hello deployment model on Azure (Resource Manager or classic).</span></span>

    <span data-ttu-id="b5a84-123">b.</span><span class="sxs-lookup"><span data-stu-id="b5a84-123">b.</span></span> <span data-ttu-id="b5a84-124">Ange din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b5a84-124">Enter your Azure subscription.</span></span> <span data-ttu-id="b5a84-125">En SAP CAL-konto kan tilldelas tooone-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b5a84-125">An SAP CAL account can be assigned tooone subscription only.</span></span> <span data-ttu-id="b5a84-126">Om du behöver mer än en prenumeration behöver toocreate en annan SAP CAL-konto.</span><span class="sxs-lookup"><span data-stu-id="b5a84-126">If you need more than one subscription, you need toocreate another SAP CAL account.</span></span>
    
    <span data-ttu-id="b5a84-127">c.</span><span class="sxs-lookup"><span data-stu-id="b5a84-127">c.</span></span> <span data-ttu-id="b5a84-128">Ge hello SAP CAL behörighet toodeploy till din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b5a84-128">Give hello SAP CAL permission toodeploy into your Azure subscription.</span></span>

    > [!NOTE]
    <span data-ttu-id="b5a84-129">hello nästa steg visar hur toocreate en SAP-CAL-konto för Resource Manager distributioner.</span><span class="sxs-lookup"><span data-stu-id="b5a84-129">hello next steps show how toocreate an SAP CAL account for Resource Manager deployments.</span></span> <span data-ttu-id="b5a84-130">Om du redan har ett SAP CAL-konto som är länkade toohello klassiska distributionsmodellen du *måste* toofollow dessa steg toocreate ett nytt SAP CAL-konto.</span><span class="sxs-lookup"><span data-stu-id="b5a84-130">If you already have an SAP CAL account that is linked toohello classic deployment model, you *need* toofollow these steps toocreate a new SAP CAL account.</span></span> <span data-ttu-id="b5a84-131">hello nya SAP CAL-konto måste toodeploy i hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="b5a84-131">hello new SAP CAL account needs toodeploy in hello Resource Manager model.</span></span>

2. <span data-ttu-id="b5a84-132">toocreate en ny SAP-CAL-konto hello **konton** sidan visas två alternativ för Azure:</span><span class="sxs-lookup"><span data-stu-id="b5a84-132">toocreate a new SAP CAL account, hello **Accounts** page shows two choices for Azure:</span></span> 

    <span data-ttu-id="b5a84-133">a.</span><span class="sxs-lookup"><span data-stu-id="b5a84-133">a.</span></span> <span data-ttu-id="b5a84-134">**Microsoft Azure (klassisk)** är hello klassiska distributionsmodellen och är inte längre önskade.</span><span class="sxs-lookup"><span data-stu-id="b5a84-134">**Microsoft Azure (classic)** is hello classic deployment model and is no longer preferred.</span></span>

    <span data-ttu-id="b5a84-135">b.</span><span class="sxs-lookup"><span data-stu-id="b5a84-135">b.</span></span> <span data-ttu-id="b5a84-136">**Microsoft Azure** är hello nya Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="b5a84-136">**Microsoft Azure** is hello new Resource Manager deployment model.</span></span>

    ![SAP CAL-konton](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic-2a.PNG)

    <span data-ttu-id="b5a84-138">Välj toodeploy i hello Resource Manager-modellen **Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="b5a84-138">toodeploy in hello Resource Manager model, select **Microsoft Azure**.</span></span>

    ![SAP CAL-konton](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic3c.PNG)

3. <span data-ttu-id="b5a84-140">Ange hello Azure **prenumerations-ID** som finns på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b5a84-140">Enter hello Azure **Subscription ID** that can be found on hello Azure portal.</span></span> 

    ![SAP CAL prenumerations-ID](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic3c.PNG)

4. <span data-ttu-id="b5a84-142">tooauthorize hello SAP CAL toodeploy till hello Azure-prenumeration som du har definierat klickar du på **auktorisera**.</span><span class="sxs-lookup"><span data-stu-id="b5a84-142">tooauthorize hello SAP CAL toodeploy into hello Azure subscription you defined, click **Authorize**.</span></span> <span data-ttu-id="b5a84-143">hello följande sida visas i hello flik i webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="b5a84-143">hello following page appears in hello browser tab:</span></span>

    ![Internet Explorer cloud services-inloggning](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic4c.PNG)

5. <span data-ttu-id="b5a84-145">Om fler än en användare visas väljer du hello Microsoft-konto som är länkade toobe hello coadministrator av hello Azure-prenumeration du valt.</span><span class="sxs-lookup"><span data-stu-id="b5a84-145">If more than one user is listed, choose hello Microsoft account that is linked toobe hello coadministrator of hello Azure subscription you selected.</span></span> <span data-ttu-id="b5a84-146">hello följande sida visas i hello flik i webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="b5a84-146">hello following page appears in hello browser tab:</span></span>

    ![Bekräftelse för Internet Explorer cloud services](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic5a.PNG)

6. <span data-ttu-id="b5a84-148">Klicka på **acceptera**.</span><span class="sxs-lookup"><span data-stu-id="b5a84-148">Click **Accept**.</span></span> <span data-ttu-id="b5a84-149">Om hello auktorisering lyckas visar hello definition för SAP CAL-konto igen.</span><span class="sxs-lookup"><span data-stu-id="b5a84-149">If hello authorization is successful, hello SAP CAL account definition displays again.</span></span> <span data-ttu-id="b5a84-150">Efter en kort tid bekräftar ett meddelande att hello Auktoriseringen lyckades.</span><span class="sxs-lookup"><span data-stu-id="b5a84-150">After a short time, a message confirms that hello authorization process was successful.</span></span>

7. <span data-ttu-id="b5a84-151">tooassign hello nyskapad SAP CAL tooyour användarkonto, ange din **användar-ID** i hello hello rätt textrutan och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="b5a84-151">tooassign hello newly created SAP CAL account tooyour user, enter your **User ID** in hello text box on hello right and click **Add**.</span></span> 

    ![Toouser kontokoppling](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic8a.PNG)

8. <span data-ttu-id="b5a84-153">tooassociate Klicka på ditt konto med hello användare att använda toosign i toohello SAP CAL **granska**.</span><span class="sxs-lookup"><span data-stu-id="b5a84-153">tooassociate your account with hello user that you use toosign in toohello SAP CAL, click **Review**.</span></span> 

9. <span data-ttu-id="b5a84-154">toocreate hello associationen mellan användaren och hello nyskapad SAP CAL-konto, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="b5a84-154">toocreate hello association between your user and hello newly created SAP CAL account, click **Create**.</span></span>

    ![Användarassociationen tooaccount](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic9b.PNG)

<span data-ttu-id="b5a84-156">Du har skapat ett SAP CAL-konto som kan:</span><span class="sxs-lookup"><span data-stu-id="b5a84-156">You successfully created an SAP CAL account that is able to:</span></span>

- <span data-ttu-id="b5a84-157">Använd hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="b5a84-157">Use hello Resource Manager deployment model.</span></span>
- <span data-ttu-id="b5a84-158">Distribuera SAP-system i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b5a84-158">Deploy SAP systems into your Azure subscription.</span></span>

> [!NOTE]
<span data-ttu-id="b5a84-159">Innan du kan distribuera hello SAP IDES lösning baserad på Windows och SQL Server måste kanske du måste toosign för en SAP CAL-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b5a84-159">Before you can deploy hello SAP IDES solution based on Windows and SQL Server, you might need toosign up for an SAP CAL subscription.</span></span> <span data-ttu-id="b5a84-160">I annat fall hello-lösning kan visas som **låst** på översiktssidan för hello.</span><span class="sxs-lookup"><span data-stu-id="b5a84-160">Otherwise, hello solution might show up as **Locked** on hello overview page.</span></span>

### <a name="deploy-a-solution"></a><span data-ttu-id="b5a84-161">Distribuera en lösning</span><span class="sxs-lookup"><span data-stu-id="b5a84-161">Deploy a solution</span></span>
1. <span data-ttu-id="b5a84-162">När du har skapat ett SAP CAL-konto, Välj **hello SAP IDES lösning på Windows och SQL Server** lösning.</span><span class="sxs-lookup"><span data-stu-id="b5a84-162">After you set up an SAP CAL account, select **hello SAP IDES solution on Windows and SQL Server** solution.</span></span> <span data-ttu-id="b5a84-163">Klicka på **skapa instans**, och bekräfta hello villkor för användning och villkor.</span><span class="sxs-lookup"><span data-stu-id="b5a84-163">Click **Create Instance**, and confirm hello usage and terms conditions.</span></span> 

2. <span data-ttu-id="b5a84-164">På hello **grundläggande läge: skapa instans** sidan som du behöver:</span><span class="sxs-lookup"><span data-stu-id="b5a84-164">On hello **Basic Mode: Create Instance** page, you need to:</span></span>

    <span data-ttu-id="b5a84-165">a.</span><span class="sxs-lookup"><span data-stu-id="b5a84-165">a.</span></span> <span data-ttu-id="b5a84-166">Ange en instans **namn**.</span><span class="sxs-lookup"><span data-stu-id="b5a84-166">Enter an instance **Name**.</span></span>

    <span data-ttu-id="b5a84-167">b.</span><span class="sxs-lookup"><span data-stu-id="b5a84-167">b.</span></span> <span data-ttu-id="b5a84-168">Välj en Azure **Region**.</span><span class="sxs-lookup"><span data-stu-id="b5a84-168">Select an Azure **Region**.</span></span> <span data-ttu-id="b5a84-169">Du kan behöva en SAP CAL prenumeration tooget som erbjuds av flera Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="b5a84-169">You might need an SAP CAL subscription tooget multiple Azure regions offered.</span></span>

    <span data-ttu-id="b5a84-170">c.</span><span class="sxs-lookup"><span data-stu-id="b5a84-170">c.</span></span>  <span data-ttu-id="b5a84-171">Ange hello master **lösenord** för hello lösning, som visas:</span><span class="sxs-lookup"><span data-stu-id="b5a84-171">Enter hello master **Password** for hello solution, as shown:</span></span>

    ![SAP CAL grundläggande läge: Skapa en instans](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic10a.png)

3. <span data-ttu-id="b5a84-173">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b5a84-173">Click **Create**.</span></span> <span data-ttu-id="b5a84-174">Efter en stund, beroende på hello storleken och komplexiteten för hello-lösning (hello SAP CAL ger en uppskattning), visas hello status som aktiv och redo att användas:</span><span class="sxs-lookup"><span data-stu-id="b5a84-174">After some time, depending on hello size and complexity of hello solution (hello SAP CAL provides an estimate), hello status is shown as active and ready for use:</span></span> 

    ![SAP CAL-instanser](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic12a.png)

4. <span data-ttu-id="b5a84-176">toofind hello resursgruppen och alla objekt som har skapats av hello SAP CAL, gå toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b5a84-176">toofind hello resource group and all its objects that were created by hello SAP CAL, go toohello Azure portal.</span></span> <span data-ttu-id="b5a84-177">hello virtuella datorn finns från och med hello samma namn som har angetts i hello SAP CAL-instans.</span><span class="sxs-lookup"><span data-stu-id="b5a84-177">hello virtual machine can be found starting with hello same instance name that was given in hello SAP CAL.</span></span>

    ![Resursen gruppobjekt](./media/cal-ides-erp6-ehp7-sp3-sql/ides_resource_group.PNG)

5. <span data-ttu-id="b5a84-179">Gå toohello distribueras instanser på hello SAP CAL-portalen och klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="b5a84-179">On hello SAP CAL portal, go toohello deployed instances and click **Connect**.</span></span> <span data-ttu-id="b5a84-180">hello följande popup-fönster visas:</span><span class="sxs-lookup"><span data-stu-id="b5a84-180">hello following pop-up window appears:</span></span> 

    ![Ansluta toohello instans](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic14a.PNG)

6. <span data-ttu-id="b5a84-182">Innan du kan använda ett av hello alternativ tooconnect toohello distribuerade system, klickar du på **komma igång**.</span><span class="sxs-lookup"><span data-stu-id="b5a84-182">Before you can use one of hello options tooconnect toohello deployed systems, click **Getting Started Guide**.</span></span> <span data-ttu-id="b5a84-183">hello dokumentationen namn hello användare för varje hello anslutningsmetoder.</span><span class="sxs-lookup"><span data-stu-id="b5a84-183">hello documentation names hello users for each of hello connectivity methods.</span></span> <span data-ttu-id="b5a84-184">hello lösenorden för dessa användare ställs toohello lösenord du har definierat hello början av hello distributionsprocessen.</span><span class="sxs-lookup"><span data-stu-id="b5a84-184">hello passwords for those users are set toohello master password you defined at hello beginning of hello deployment process.</span></span> <span data-ttu-id="b5a84-185">I dokumentationen för hello andra mer funktionell användare visas med sina lösenord, som du kan använda toosign i toohello distribuerade system.</span><span class="sxs-lookup"><span data-stu-id="b5a84-185">In hello documentation, other more functional users are listed with their passwords, which you can use toosign in toohello deployed system.</span></span>

    ![Välkommen till SAP-dokumentation](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic15.jpg)

<span data-ttu-id="b5a84-187">En felfri systemet SAP IDES distribueras inom några timmar i Azure.</span><span class="sxs-lookup"><span data-stu-id="b5a84-187">Within a few hours, a healthy SAP IDES system is deployed in Azure.</span></span>

<span data-ttu-id="b5a84-188">Om du har köpt en prenumeration för SAP CAL stöd SAP fullständigt för distribution via hello SAP CAL på Azure.</span><span class="sxs-lookup"><span data-stu-id="b5a84-188">If you bought an SAP CAL subscription, SAP fully supports deployments through hello SAP CAL on Azure.</span></span> <span data-ttu-id="b5a84-189">hello stöd kön är BC-VCM-klientåtkomstlicens.</span><span class="sxs-lookup"><span data-stu-id="b5a84-189">hello support queue is BC-VCM-CAL.</span></span>

