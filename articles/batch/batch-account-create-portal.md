---
title: aaaCreate Batch-kontot i hello Azure-portalen | Microsoft Docs
description: "Lär dig hur toocreate ett Azure Batch konto i hello Azure portal toorun storskaliga parallella arbetsbelastningar i hello moln"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 3fbae545-245f-4c66-aee2-e25d7d5d36db
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2176f88ba0a1a3298023de8f520d46ef28a664b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-batch-account-with-hello-azure-portal"></a><span data-ttu-id="12911-103">Skapa ett batchkonto med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="12911-103">Create a Batch account with hello Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="12911-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="12911-104">Azure portal</span></span>](batch-account-create-portal.md)
> * [<span data-ttu-id="12911-105">Batch Management .NET</span><span class="sxs-lookup"><span data-stu-id="12911-105">Batch Management .NET</span></span>](batch-management-dotnet.md)
>
>

<span data-ttu-id="12911-106">Lär dig hur toocreate ett Azure Batch-kontot i hello [Azure-portalen][azure_portal], och välj Egenskaper för hello som passar din situation för beräkning.</span><span class="sxs-lookup"><span data-stu-id="12911-106">Learn how toocreate an Azure Batch account in hello [Azure portal][azure_portal], and choose hello account properties that fit your compute scenario.</span></span> <span data-ttu-id="12911-107">Lär dig där toofind viktiga egenskaper som kontot URL: er och åtkomstnycklar.</span><span class="sxs-lookup"><span data-stu-id="12911-107">Learn where toofind important account properties like access keys and account URLs.</span></span>

<span data-ttu-id="12911-108">Information om Batch-konton och scenarier finns hello [funktion översikt](batch-api-basics.md).</span><span class="sxs-lookup"><span data-stu-id="12911-108">For background about Batch accounts and scenarios, see hello [feature overview](batch-api-basics.md).</span></span>



## <a name="create-a-batch-account"></a><span data-ttu-id="12911-109">Skapa ett Batch-konto</span><span class="sxs-lookup"><span data-stu-id="12911-109">Create a Batch account</span></span>

<span data-ttu-id="12911-110">Använd hello portal toocreate ett Batch-konto i en hello två *poolens fördelning lägen*: **Batch-tjänsten** läge eller hello senare **användarens prenumeration** läge, som kräver mer konfiguration.</span><span class="sxs-lookup"><span data-stu-id="12911-110">Use hello portal toocreate a Batch account in one of hello two *pool allocation modes*: **Batch service** mode or hello newer **user subscription** mode, which requires more configuration.</span></span> <span data-ttu-id="12911-111">Information om dessa två lägen finns hello [funktion översikt](batch-api-basics.md#account).</span><span class="sxs-lookup"><span data-stu-id="12911-111">For information about these two modes, see hello [feature overview](batch-api-basics.md#account).</span></span> <span data-ttu-id="12911-112">Funktioner i hello användarläge prenumeration finns även hello [blogginlägget](https://blogs.technet.microsoft.com/windowshpc/2017/03/17/azure-batch-vnet-and-custom-image-support-for-virtual-machine-pools/).</span><span class="sxs-lookup"><span data-stu-id="12911-112">For features of hello user subscription mode, see also hello [blog post](https://blogs.technet.microsoft.com/windowshpc/2017/03/17/azure-batch-vnet-and-custom-image-support-for-virtual-machine-pools/).</span></span>

## <a name="batch-service-mode"></a><span data-ttu-id="12911-113">Batch-tjänstläge</span><span class="sxs-lookup"><span data-stu-id="12911-113">Batch service mode</span></span>



1. <span data-ttu-id="12911-114">Logga in toohello [Azure-portalen][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="12911-114">Sign in toohello [Azure portal][azure_portal].</span></span>
2. <span data-ttu-id="12911-115">Klicka på **Ny** > **Beräkna** > **Batch-tjänst**.</span><span class="sxs-lookup"><span data-stu-id="12911-115">Click **New** > **Compute** > **Batch Service**.</span></span>

    ![Batch i hello Marketplace][marketplace_portal]
3. <span data-ttu-id="12911-117">Hej **nytt Batchkonto** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="12911-117">hello **New Batch Account** blade is displayed.</span></span> <span data-ttu-id="12911-118">Se hello beskrivningarna nedan för varje element i bladet.</span><span class="sxs-lookup"><span data-stu-id="12911-118">See hello descriptions below of each blade element.</span></span>

    ![Skapa ett Batch-konto][account_portal]

    <span data-ttu-id="12911-120">a.</span><span class="sxs-lookup"><span data-stu-id="12911-120">a.</span></span> <span data-ttu-id="12911-121">**Kontonamn**: hello Batch-kontonamn du väljer måste vara unika inom hello Azure-region där hello kontot skapas (se **plats** nedan).</span><span class="sxs-lookup"><span data-stu-id="12911-121">**Account name**: hello Batch account name you choose must be unique within hello Azure region where hello account is created (see **Location** below).</span></span> <span data-ttu-id="12911-122">hello-kontonamnet får innehålla endast små bokstäver eller siffror och måste bestå av 3 till 24 tecken.</span><span class="sxs-lookup"><span data-stu-id="12911-122">hello account name may contain only lowercase characters or numbers, and must be 3-24 characters in length.</span></span>

    <span data-ttu-id="12911-123">b.</span><span class="sxs-lookup"><span data-stu-id="12911-123">b.</span></span> <span data-ttu-id="12911-124">**Prenumerationen**: hello prenumerationen i vilka toocreate hello Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="12911-124">**Subscription**: hello subscription in which toocreate hello Batch account.</span></span> <span data-ttu-id="12911-125">Om du bara har en prenumeration väljs den som standard.</span><span class="sxs-lookup"><span data-stu-id="12911-125">If you have only one subscription, it is selected by default.</span></span>

    <span data-ttu-id="12911-126">c.</span><span class="sxs-lookup"><span data-stu-id="12911-126">c.</span></span> <span data-ttu-id="12911-127">**Poolallokeringsläge**: Välj **Batch-tjänst**.</span><span class="sxs-lookup"><span data-stu-id="12911-127">**Pool allocation mode**: Select **Batch service**.</span></span>

    <span data-ttu-id="12911-128">c.</span><span class="sxs-lookup"><span data-stu-id="12911-128">c.</span></span> <span data-ttu-id="12911-129">**Resursgrupp**: Välj en befintlig resursgrupp för ditt nya Batch-konto. Du kan också skapa en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="12911-129">**Resource group**: Select an existing resource group for your new Batch account, or optionally create a new one.</span></span>

    <span data-ttu-id="12911-130">d.</span><span class="sxs-lookup"><span data-stu-id="12911-130">d.</span></span> <span data-ttu-id="12911-131">**Plats**: hello Azure-region i vilken toocreate hello Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="12911-131">**Location**: hello Azure region in which toocreate hello Batch account.</span></span> <span data-ttu-id="12911-132">Endast hello regioner som stöds av din prenumeration och resursgrupp visas som alternativ.</span><span class="sxs-lookup"><span data-stu-id="12911-132">Only hello regions supported by your subscription and resource group are displayed as options.</span></span>

    <span data-ttu-id="12911-133">e.</span><span class="sxs-lookup"><span data-stu-id="12911-133">e.</span></span> <span data-ttu-id="12911-134">**Lagringskonto** (valfritt): Ett generellt Azure Storage-konto som du associerar med ditt Batch-konto.</span><span class="sxs-lookup"><span data-stu-id="12911-134">**Storage account** (optional): A general-purpose Azure Storage account that you associate with your Batch account.</span></span> <span data-ttu-id="12911-135">Detta rekommenderas för de flesta Batch-konton.</span><span class="sxs-lookup"><span data-stu-id="12911-135">This is recommended for most Batch accounts.</span></span> <span data-ttu-id="12911-136">Mer information finns i [Länkat Azure Storage-konto](#linked-azure-storage-account) senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="12911-136">See [Linked Azure Storage account](#linked-azure-storage-account) later in this article for more details.</span></span>

4. <span data-ttu-id="12911-137">Klicka på **skapa** toocreate hello-konto.</span><span class="sxs-lookup"><span data-stu-id="12911-137">Click **Create** toocreate hello account.</span></span>

   <span data-ttu-id="12911-138">hello portal anger distribution pågår.</span><span class="sxs-lookup"><span data-stu-id="12911-138">hello portal indicates deployment is in progress.</span></span> <span data-ttu-id="12911-139">När åtgärden har slutförts visas meddelandet **Distributionen är klar** i **Meddelanden**.</span><span class="sxs-lookup"><span data-stu-id="12911-139">Upon completion, a **Deployments succeeded** notification appears in **Notifications**.</span></span>

## <a name="user-subscription-mode"></a><span data-ttu-id="12911-140">Användarprenumerationsläge</span><span class="sxs-lookup"><span data-stu-id="12911-140">User subscription mode</span></span>

### <a name="allow-azure-batch-tooaccess-hello-subscription-one-time-operation"></a><span data-ttu-id="12911-141">Tillåt Azure Batch tooaccess hello prenumeration (engångsåtgärd)</span><span class="sxs-lookup"><span data-stu-id="12911-141">Allow Azure Batch tooaccess hello subscription (one-time operation)</span></span>
<span data-ttu-id="12911-142">När du skapar din första Batch-kontot i användarläge för prenumerationen kan du utföra hello följande steg tooregister prenumerationen med Batch.</span><span class="sxs-lookup"><span data-stu-id="12911-142">When creating your first Batch account in user subscription mode, perform hello following steps tooregister your subscription with Batch.</span></span> <span data-ttu-id="12911-143">(Om du tidigare har gjort det, hoppar du över toohello nästa avsnitt.)</span><span class="sxs-lookup"><span data-stu-id="12911-143">(If you previously did this, skip toohello next section.)</span></span>

1. <span data-ttu-id="12911-144">Logga in toohello [Azure-portalen][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="12911-144">Sign in toohello [Azure portal][azure_portal].</span></span>

2. <span data-ttu-id="12911-145">Klicka på **fler tjänster** > **prenumerationer**, och klicka på hello-prenumeration som du vill använda toouse för hello Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="12911-145">Click **More Services** > **Subscriptions**, and click hello subscription you want toouse for hello Batch account.</span></span>

3. <span data-ttu-id="12911-146">I hello **prenumeration** bladet, klickar du på **åtkomstkontroll (IAM)** > **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="12911-146">In hello **Subscription** blade, click **Access control (IAM)** > **Add**.</span></span>

    ![Åtkomstkontroll för prenumeration][subscription_access]

4. <span data-ttu-id="12911-148">På hello **lägga till behörigheter** bladet, Välj hello **deltagare** roll, söka efter hello Batch-API.</span><span class="sxs-lookup"><span data-stu-id="12911-148">On hello **Add permissions** blade, select hello **Contributor** role, search for hello Batch API.</span></span> <span data-ttu-id="12911-149">Sök efter var och en av de här strängarna tills du hittar hello-API:</span><span class="sxs-lookup"><span data-stu-id="12911-149">Search for each of these strings until you find hello API:</span></span>
    1. <span data-ttu-id="12911-150">**MicrosoftAzureBatch**.</span><span class="sxs-lookup"><span data-stu-id="12911-150">**MicrosoftAzureBatch**.</span></span>
    2. <span data-ttu-id="12911-151">**Microsoft Azure Batch**.</span><span class="sxs-lookup"><span data-stu-id="12911-151">**Microsoft Azure Batch**.</span></span> <span data-ttu-id="12911-152">Nyare Azure AD-klientorganisationer kan använda det här namnet.</span><span class="sxs-lookup"><span data-stu-id="12911-152">Newer Azure AD tenants may use this name.</span></span>
    3. <span data-ttu-id="12911-153">**ddbf3205-c6bd-46ae-8127-60eb93363864** är hello-ID för hello Batch-API.</span><span class="sxs-lookup"><span data-stu-id="12911-153">**ddbf3205-c6bd-46ae-8127-60eb93363864** is hello ID for hello Batch API.</span></span> 

5. <span data-ttu-id="12911-154">När du har hittat hello Batch-API och på **spara**.</span><span class="sxs-lookup"><span data-stu-id="12911-154">Once you find hello Batch API, select it and click **Save**.</span></span>

    ![Lägg till Batch-behörigheter][add_permission]

### <a name="create-a-key-vault"></a><span data-ttu-id="12911-156">Skapa ett nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="12911-156">Create a key vault</span></span>
<span data-ttu-id="12911-157">I användarläge för prenumerationen, krävs en Azure key vault som tillhör toothe samma resursgrupp som hello Batch-kontot toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="12911-157">In user subscription mode, an Azure key vault is required that belongs toothe same resource group as hello Batch account toobe created.</span></span> <span data-ttu-id="12911-158">Se till att hello resursgrupp i en region där Batch är [tillgängliga](https://azure.microsoft.com/regions/services/) och som har stöd för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="12911-158">Make sure hello resource group is in a region where Batch is [available](https://azure.microsoft.com/regions/services/) and which your subscription supports.</span></span>

1. <span data-ttu-id="12911-159">I hello [Azure-portalen][azure_portal], klickar du på **ny** > **säkerhet + identitet** > **Key Vault** .</span><span class="sxs-lookup"><span data-stu-id="12911-159">In hello [Azure portal][azure_portal], click **New** > **Security + Identity** > **Key Vault**.</span></span>

2. <span data-ttu-id="12911-160">I hello **skapa Nyckelvalvet** bladet, ange ett namn för hello nyckelvalvet och skapa en resursgrupp i hello region som du vill använda för Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="12911-160">In hello **Create Key Vault** blade, enter a name for hello key vault, and create a resource group in hello region you want for your Batch account.</span></span> <span data-ttu-id="12911-161">Lämna hello återstående inställningar till standardvärden och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="12911-161">Leave hello remaining settings at default values, then click **Create**.</span></span>

### <a name="create-a-batch-account"></a><span data-ttu-id="12911-162">Skapa ett Batch-konto</span><span class="sxs-lookup"><span data-stu-id="12911-162">Create a Batch account</span></span>

1. <span data-ttu-id="12911-163">I hello [Azure-portalen][azure_portal], klickar du på **ny** > **Compute** > **Batchtjänsten**.</span><span class="sxs-lookup"><span data-stu-id="12911-163">In hello [Azure portal][azure_portal], click **New** > **Compute** > **Batch Service**.</span></span>

    ![Batch i hello Marketplace][marketplace_portal]
3. <span data-ttu-id="12911-165">Hej **nytt Batchkonto** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="12911-165">hello **New Batch Account** blade is displayed.</span></span> <span data-ttu-id="12911-166">Se hello beskrivningarna nedan för varje element i bladet.</span><span class="sxs-lookup"><span data-stu-id="12911-166">See hello descriptions below of each blade element.</span></span>

    ![Skapa ett Batch-konto][account_portal_byos]

    <span data-ttu-id="12911-168">a.</span><span class="sxs-lookup"><span data-stu-id="12911-168">a.</span></span> <span data-ttu-id="12911-169">**Kontonamn**: hello Batch-kontonamn du väljer måste vara unika inom hello Azure-region där hello kontot skapas (se **plats** nedan).</span><span class="sxs-lookup"><span data-stu-id="12911-169">**Account name**: hello Batch account name you choose must be unique within hello Azure region where hello account is created (see **Location** below).</span></span> <span data-ttu-id="12911-170">hello-kontonamnet får innehålla endast små bokstäver eller siffror och måste bestå av 3 till 24 tecken.</span><span class="sxs-lookup"><span data-stu-id="12911-170">hello account name may contain only lowercase characters or numbers, and must be 3-24 characters in length.</span></span>

    <span data-ttu-id="12911-171">b.</span><span class="sxs-lookup"><span data-stu-id="12911-171">b.</span></span> <span data-ttu-id="12911-172">**Prenumerationen**: Om du har mer än en prenumeration väljer du hello-prenumeration som du har registrerat med hello Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="12911-172">**Subscription**: If you have more than one subscription, select hello subscription that you registered with hello Batch service.</span></span>

    <span data-ttu-id="12911-173">c.</span><span class="sxs-lookup"><span data-stu-id="12911-173">c.</span></span> <span data-ttu-id="12911-174">**Poolallokeringsläge**: Välj **Användarprenumeration**.</span><span class="sxs-lookup"><span data-stu-id="12911-174">**Pool allocation mode**: Select **User subscription**.</span></span>

    <span data-ttu-id="12911-175">d.</span><span class="sxs-lookup"><span data-stu-id="12911-175">d.</span></span> <span data-ttu-id="12911-176">**Nyckelvalv**: Välj hello nyckelvalv som du skapade för Batch-kontot i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="12911-176">**Key vault**: Select hello key vault you created for your Batch account in hello previous section.</span></span> <span data-ttu-id="12911-177">Om du vill kan du skapa ett nytt nyckelvalv.</span><span class="sxs-lookup"><span data-stu-id="12911-177">Optionally, create a new key vault.</span></span> <span data-ttu-id="12911-178">När du har valt hello valvet, Välj hello kryssrutan toogrant Azure Batch åtkomst toohello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="12911-178">After selecting hello vault, select hello checkbox toogrant Azure Batch access toohello key vault.</span></span>

    <span data-ttu-id="12911-179">c.</span><span class="sxs-lookup"><span data-stu-id="12911-179">c.</span></span> <span data-ttu-id="12911-180">**Resursgruppen**: Välj hello resursgrupp som du skapade hello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="12911-180">**Resource group**: Select hello resource group in which you  created hello key vault.</span></span>

    <span data-ttu-id="12911-181">d.</span><span class="sxs-lookup"><span data-stu-id="12911-181">d.</span></span> <span data-ttu-id="12911-182">**Plats**: hello Azure-region där du skapade hello nyckelvalv för hello Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="12911-182">**Location**: hello Azure region in which you created hello key vault for hello Batch account.</span></span>

    <span data-ttu-id="12911-183">e.</span><span class="sxs-lookup"><span data-stu-id="12911-183">e.</span></span> <span data-ttu-id="12911-184">**Lagringskonto** (valfritt): Ett generellt Azure Storage-konto som du associerar med ditt Batch-konto.</span><span class="sxs-lookup"><span data-stu-id="12911-184">**Storage account** (optional): A general-purpose Azure Storage account that you associate with your Batch account.</span></span> <span data-ttu-id="12911-185">Detta rekommenderas för de flesta Batch-konton.</span><span class="sxs-lookup"><span data-stu-id="12911-185">This is recommended for most Batch accounts.</span></span> <span data-ttu-id="12911-186">Mer information finns i [Länkat Azure Storage-konto](#linked-azure-storage-account) nedan.</span><span class="sxs-lookup"><span data-stu-id="12911-186">See [Linked Azure Storage account](#linked-azure-storage-account) below for more details.</span></span>

4. <span data-ttu-id="12911-187">Klicka på **skapa** toocreate hello-konto.</span><span class="sxs-lookup"><span data-stu-id="12911-187">Click **Create** toocreate hello account.</span></span>

   <span data-ttu-id="12911-188">hello portal anger distribution pågår.</span><span class="sxs-lookup"><span data-stu-id="12911-188">hello portal indicates deployment is in progress.</span></span> <span data-ttu-id="12911-189">När åtgärden har slutförts visas meddelandet **Distributionen är klar** i **Meddelanden**.</span><span class="sxs-lookup"><span data-stu-id="12911-189">Upon completion, a **Deployments succeeded** notification appears in **Notifications**.</span></span>



## <a name="view-batch-account-properties"></a><span data-ttu-id="12911-190">Visa egenskaper för ett Batch-konto</span><span class="sxs-lookup"><span data-stu-id="12911-190">View Batch account properties</span></span>
<span data-ttu-id="12911-191">När hello-konto har skapats kan du öppna hello **Batch-kontoblad** tooaccess inställningar och egenskaper.</span><span class="sxs-lookup"><span data-stu-id="12911-191">Once hello account has been created, you can open hello **Batch account blade** tooaccess its settings and properties.</span></span> <span data-ttu-id="12911-192">Du kan komma åt alla egenskaper och inställningar med hjälp av hello vänstra menyn hello Batch-kontot bladet.</span><span class="sxs-lookup"><span data-stu-id="12911-192">You can access all account settings and properties by using hello left menu of hello Batch account blade.</span></span>

![Bladet för Batch-kontot på Azure Portal][account_blade]

* <span data-ttu-id="12911-194">**Batch-kontots URL**: när du utvecklar ett program med hello [Batch-API: er](batch-apis-tools.md#azure-accounts-for-batch-development), behöver du ett konto URL tooaccess Batch-resurser.</span><span class="sxs-lookup"><span data-stu-id="12911-194">**Batch account URL**: When you develop an application with hello [Batch APIs](batch-apis-tools.md#azure-accounts-for-batch-development), you'll need an account URL tooaccess your Batch resources.</span></span> <span data-ttu-id="12911-195">En URL för Batch-kontot har hello följande format:</span><span class="sxs-lookup"><span data-stu-id="12911-195">A Batch account URL has hello following format:</span></span>

    `https://<account_name>.<region>.batch.azure.com`

![Batch-kontots URL på portalen][account_url]

* <span data-ttu-id="12911-197">**Åtkomstnycklar** (Batch-tjänsten-läge): tooauthenticate åtkomst tooyour Batch-kontot från ditt program behöver du en åtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="12911-197">**Access keys** (Batch service mode): tooauthenticate access tooyour Batch account from your application, you'll need an account access key.</span></span> <span data-ttu-id="12911-198">(Den här inställningen är inte tillgänglig i användarprenumerationsläge, där du använder Azure Active Directory-autentisering.)</span><span class="sxs-lookup"><span data-stu-id="12911-198">(This setting is not available in user subscription mode, where you use Azure Active Directory authentication.)</span></span>

    <span data-ttu-id="12911-199">tooview eller generera åtkomstnycklar för Batch-kontot, ange `keys` hello vänstra menyn **Sök** på bladet för hello Batch-konto och sedan markera **nycklar**.</span><span class="sxs-lookup"><span data-stu-id="12911-199">tooview or regenerate your Batch account's access keys, enter `keys` in hello left menu **Search** box on hello Batch account blade, then select **Keys**.</span></span>

    ![Batch-kontonycklar på Azure Portal][account_keys]

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="linked-azure-storage-account"></a><span data-ttu-id="12911-201">Länkat Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="12911-201">Linked Azure Storage account</span></span>

<span data-ttu-id="12911-202">Alternativt kan du koppla en generell Azure Storage-konto tooyour Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="12911-202">You can optionally link a general-purpose Azure Storage account tooyour Batch account.</span></span> <span data-ttu-id="12911-203">Hej [programpaket](batch-application-packages.md) funktion i Batch använder Azure Blob storage hello [Batch filen konventioner .NET](batch-task-output.md) bibliotek.</span><span class="sxs-lookup"><span data-stu-id="12911-203">hello [application packages](batch-application-packages.md) feature of Batch uses Azure Blob storage, as does hello [Batch File Conventions .NET](batch-task-output.md) library.</span></span> <span data-ttu-id="12911-204">De här valfria funktionerna hjälpa dig distribuera hello-program som körs Batch-aktiviteter och spara hello data de producerar.</span><span class="sxs-lookup"><span data-stu-id="12911-204">These optional features assist you in deploying hello applications that your Batch tasks run, and persisting hello data they produce.</span></span>

<span data-ttu-id="12911-205">Vi rekommenderar att du skapar ett nytt lagringskonto för exklusiv användning av Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="12911-205">We recommend that you create a new Storage account exclusively for use by your Batch account.</span></span>

![Skapa ett allmänt lagringskonto][storage_account]

> [!NOTE]
> <span data-ttu-id="12911-207">Azure Batch stöder för närvarande endast hello allmänna lagringskontotypen.</span><span class="sxs-lookup"><span data-stu-id="12911-207">Azure Batch currently supports only hello general-purpose Storage account type.</span></span> <span data-ttu-id="12911-208">Den här kontotypen beskrivs i steg 5, [Skapa ett lagringskonto] (../storage/common/storage-create-storage-account.md#create-a-storage-account), i [Om Azure Storage-konton](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="12911-208">This account type is described in step 5, [Create a storage account] (../storage/common/storage-create-storage-account.md#create-a-storage-account), in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>
>
>

> [!WARNING]
> <span data-ttu-id="12911-209">Var försiktig när du återskapar hello snabbtangenter av länkade lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="12911-209">Be careful when regenerating hello access keys of a linked Storage account.</span></span> <span data-ttu-id="12911-210">Återskapa endast en lagringskontonyckel och klicka på **synkronisera nycklar** på hello länkade Storage-konto bladet.</span><span class="sxs-lookup"><span data-stu-id="12911-210">Regenerate only one Storage account key and click **Sync Keys** on hello linked Storage account blade.</span></span> <span data-ttu-id="12911-211">Vänta 5 minuter tooallow hello nycklar toopropagate toohello compute-noder i din pooler och sedan återskapa och synkronisera hello andra nyckeln om det behövs.</span><span class="sxs-lookup"><span data-stu-id="12911-211">Wait five minutes tooallow hello keys toopropagate toohello compute nodes in your pools, then regenerate and synchronize hello other key if necessary.</span></span> <span data-ttu-id="12911-212">Om du återskapa båda nycklarna på hello samma tid, compute-noderna inte kan toosynchronize antingen nyckel, och de kommer att förlora åtkomst toohello Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="12911-212">If you regenerate both keys at hello same time, your compute nodes will not be able toosynchronize either key, and they will lose access toohello Storage account.</span></span>
>
>

<span data-ttu-id="12911-213">![Återskapa lagringskontonycklar][4]</span><span class="sxs-lookup"><span data-stu-id="12911-213">![Regenerating storage account keys][4]</span></span>

## <a name="batch-service-quotas-and-limits"></a><span data-ttu-id="12911-214">Kvoter och begränsningar för Batch-tjänsten</span><span class="sxs-lookup"><span data-stu-id="12911-214">Batch service quotas and limits</span></span>
<span data-ttu-id="12911-215">Du är medveten om att som med din Azure-prenumeration och andra Azure-tjänster, vissa [kvoter och gränser](batch-quota-limit.md) gäller tooBatch konton.</span><span class="sxs-lookup"><span data-stu-id="12911-215">Please be aware that as with your Azure subscription and other Azure services, certain [quotas and limits](batch-quota-limit.md) apply tooBatch accounts.</span></span> <span data-ttu-id="12911-216">Aktuella kvoter för Batch-kontot visas i hello-portal i hello konto **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="12911-216">Current quotas for a Batch account appear in hello portal in hello account **Properties**.</span></span>

![Kvoter för Batch-konto på Azure Portal][quotas]



<span data-ttu-id="12911-218">Dessutom att många av dessa kvoter kan ökas bara med en kostnadsfri produkten supportbegäran skickade i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="12911-218">Additionally, many of these quotas can be increased simply with a free product support request submitted in hello Azure portal.</span></span> <span data-ttu-id="12911-219">Se [kvoter och gränser för hello Azure Batch-tjänsten](batch-quota-limit.md) information om begär kvoten ökar.</span><span class="sxs-lookup"><span data-stu-id="12911-219">See [Quotas and limits for hello Azure Batch service](batch-quota-limit.md) for details on requesting quota increases.</span></span>

## <a name="other-batch-account-management-options"></a><span data-ttu-id="12911-220">Andra alternativ för Batch-kontohantering</span><span class="sxs-lookup"><span data-stu-id="12911-220">Other Batch account management options</span></span>
<span data-ttu-id="12911-221">Dessutom toousing hello Azure-portalen, du kan också skapa och hantera Batch-konton med hello följande:</span><span class="sxs-lookup"><span data-stu-id="12911-221">In addition toousing hello Azure portal, you can also create and manage Batch accounts with hello following:</span></span>

* [<span data-ttu-id="12911-222">PowerShell-cmdletar för Batch</span><span class="sxs-lookup"><span data-stu-id="12911-222">Batch PowerShell cmdlets</span></span>](batch-powershell-cmdlets-get-started.md)
* [<span data-ttu-id="12911-223">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="12911-223">Azure CLI</span></span>](batch-cli-get-started.md)
* [<span data-ttu-id="12911-224">Batch Management .NET</span><span class="sxs-lookup"><span data-stu-id="12911-224">Batch Management .NET</span></span>](batch-management-dotnet.md)

## <a name="next-steps"></a><span data-ttu-id="12911-225">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="12911-225">Next steps</span></span>
* <span data-ttu-id="12911-226">Se hello [Batch funktionsöversikt](batch-api-basics.md) toolearn mer om Batch-koncept och funktioner.</span><span class="sxs-lookup"><span data-stu-id="12911-226">See hello [Batch feature overview](batch-api-basics.md) toolearn more about Batch service concepts and features.</span></span> <span data-ttu-id="12911-227">hello artikeln beskriver hello primära Batch resurser, till exempel pooler, compute-noder, jobb och uppgifter och ger en översikt över hello tjänstens funktioner som möjliggör körning av storskaliga beräkning arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="12911-227">hello article discusses hello primary Batch resources such as pools, compute nodes, jobs, and tasks, and provides an overview of hello service's features that enable large-scale compute workload execution.</span></span>
* <span data-ttu-id="12911-228">Hello grundläggande information om hur utvecklingen av ett Batch-aktiverade program som använder hello [Batch .NET-klientbibliotek](batch-dotnet-get-started.md) eller [Python](batch-python-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="12911-228">Learn hello basics of developing a Batch-enabled application using hello [Batch .NET client library](batch-dotnet-get-started.md) or [Python](batch-python-tutorial.md).</span></span> <span data-ttu-id="12911-229">De här inledande artiklarna leder dig igenom ett fungerande program som använder hello Batch-tjänsten tooexecute en arbetsbelastning på flera datornoder och användning av Azure Storage för arbetsbelastningen filen mellanlagrings- och hämtning.</span><span class="sxs-lookup"><span data-stu-id="12911-229">These introductory articles guide you through a working application that uses hello Batch service tooexecute a workload on multiple compute nodes, and includes using Azure Storage for workload file staging and retrieval.</span></span>

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "Återskapa lagringskontonycklar"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[quotas]: ./media/batch-account-create-portal/quotas.png
[subscription_access]: ./media/batch-account-create-portal/subscription_iam.png
[add_permission]: ./media/batch-account-create-portal/add_permission.png
[account_portal_byos]: ./media/batch-account-create-portal/batch_acct_portal_byos.png
