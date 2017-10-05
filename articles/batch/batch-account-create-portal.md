---
title: "Skapa ett Batch-konto på Azure Portal | Microsoft Docs"
description: "Lär dig hur du skapar ett Azure Batch-konto på Azure Portal för att köra storskaliga parallella arbetsbelastningar i molnet"
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
ms.openlocfilehash: 520d1d42d35b25db1a35d4317e9eb616cf5de565
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-batch-account-with-the-azure-portal"></a><span data-ttu-id="2825a-103">Skapa ett Batch-konto med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2825a-103">Create a Batch account with the Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2825a-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2825a-104">Azure portal</span></span>](batch-account-create-portal.md)
> * [<span data-ttu-id="2825a-105">Batch Management .NET</span><span class="sxs-lookup"><span data-stu-id="2825a-105">Batch Management .NET</span></span>](batch-management-dotnet.md)
>
>

<span data-ttu-id="2825a-106">Lär dig hur du skapar ett Azure Batch-konto på [Azure Portal][azure_portal] och välj de kontoegenskaper som passar ditt beräkningsscenario.</span><span class="sxs-lookup"><span data-stu-id="2825a-106">Learn how to create an Azure Batch account in the [Azure portal][azure_portal], and choose the account properties that fit your compute scenario.</span></span> <span data-ttu-id="2825a-107">Lär dig var du hittar viktiga kontoegenskaper som snabbtangenter och konto-URL:er.</span><span class="sxs-lookup"><span data-stu-id="2825a-107">Learn where to find important account properties like access keys and account URLs.</span></span>

<span data-ttu-id="2825a-108">Bakgrundsinformation om Batch-konton och Batch-scenarier finns i [funktionsöversikten](batch-api-basics.md).</span><span class="sxs-lookup"><span data-stu-id="2825a-108">For background about Batch accounts and scenarios, see the [feature overview](batch-api-basics.md).</span></span>



## <a name="create-a-batch-account"></a><span data-ttu-id="2825a-109">Skapa ett Batch-konto</span><span class="sxs-lookup"><span data-stu-id="2825a-109">Create a Batch account</span></span>

<span data-ttu-id="2825a-110">Använd portalen för att skapa ett Batch-konto i ett av de två *poolallokeringslägena*: **Batch-tjänstläge** eller det nyare **användarprenumerationsläget**, som kräver mer konfiguration.</span><span class="sxs-lookup"><span data-stu-id="2825a-110">Use the portal to create a Batch account in one of the two *pool allocation modes*: **Batch service** mode or the newer **user subscription** mode, which requires more configuration.</span></span> <span data-ttu-id="2825a-111">Information om dessa två lägena finns i [funktionsöversikten](batch-api-basics.md#account).</span><span class="sxs-lookup"><span data-stu-id="2825a-111">For information about these two modes, see the [feature overview](batch-api-basics.md#account).</span></span> <span data-ttu-id="2825a-112">Mer information om funktioner i användarprenumerationsläget finns i det här [blogginlägget](https://blogs.technet.microsoft.com/windowshpc/2017/03/17/azure-batch-vnet-and-custom-image-support-for-virtual-machine-pools/).</span><span class="sxs-lookup"><span data-stu-id="2825a-112">For features of the user subscription mode, see also the [blog post](https://blogs.technet.microsoft.com/windowshpc/2017/03/17/azure-batch-vnet-and-custom-image-support-for-virtual-machine-pools/).</span></span>

## <a name="batch-service-mode"></a><span data-ttu-id="2825a-113">Batch-tjänstläge</span><span class="sxs-lookup"><span data-stu-id="2825a-113">Batch service mode</span></span>



1. <span data-ttu-id="2825a-114">Logga in på [Azure Portal][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="2825a-114">Sign in to the [Azure portal][azure_portal].</span></span>
2. <span data-ttu-id="2825a-115">Klicka på **Ny** > **Beräkna** > **Batch-tjänst**.</span><span class="sxs-lookup"><span data-stu-id="2825a-115">Click **New** > **Compute** > **Batch Service**.</span></span>

    ![Batch på Marketplace][marketplace_portal]
3. <span data-ttu-id="2825a-117">Bladet **Nytt Batch-konto** visas.</span><span class="sxs-lookup"><span data-stu-id="2825a-117">The **New Batch Account** blade is displayed.</span></span> <span data-ttu-id="2825a-118">Se beskrivningarna nedan av varje element på bladet.</span><span class="sxs-lookup"><span data-stu-id="2825a-118">See the descriptions below of each blade element.</span></span>

    ![Skapa ett Batch-konto][account_portal]

    <span data-ttu-id="2825a-120">a.</span><span class="sxs-lookup"><span data-stu-id="2825a-120">a.</span></span> <span data-ttu-id="2825a-121">**Kontonamn**: Det Batch-kontonamn som du väljer måste vara unikt i den Azure-region där kontot skapas (se **Plats** nedan).</span><span class="sxs-lookup"><span data-stu-id="2825a-121">**Account name**: The Batch account name you choose must be unique within the Azure region where the account is created (see **Location** below).</span></span> <span data-ttu-id="2825a-122">Kontonamnet får bara innehålla gemener eller siffror och måste vara mellan 3 och 24 tecken långt.</span><span class="sxs-lookup"><span data-stu-id="2825a-122">The account name may contain only lowercase characters or numbers, and must be 3-24 characters in length.</span></span>

    <span data-ttu-id="2825a-123">b.</span><span class="sxs-lookup"><span data-stu-id="2825a-123">b.</span></span> <span data-ttu-id="2825a-124">**Prenumeration**: Prenumerationen som Batch-kontot skapas i.</span><span class="sxs-lookup"><span data-stu-id="2825a-124">**Subscription**: The subscription in which to create the Batch account.</span></span> <span data-ttu-id="2825a-125">Om du bara har en prenumeration väljs den som standard.</span><span class="sxs-lookup"><span data-stu-id="2825a-125">If you have only one subscription, it is selected by default.</span></span>

    <span data-ttu-id="2825a-126">c.</span><span class="sxs-lookup"><span data-stu-id="2825a-126">c.</span></span> <span data-ttu-id="2825a-127">**Poolallokeringsläge**: Välj **Batch-tjänst**.</span><span class="sxs-lookup"><span data-stu-id="2825a-127">**Pool allocation mode**: Select **Batch service**.</span></span>

    <span data-ttu-id="2825a-128">c.</span><span class="sxs-lookup"><span data-stu-id="2825a-128">c.</span></span> <span data-ttu-id="2825a-129">**Resursgrupp**: Välj en befintlig resursgrupp för ditt nya Batch-konto. Du kan också skapa en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="2825a-129">**Resource group**: Select an existing resource group for your new Batch account, or optionally create a new one.</span></span>

    <span data-ttu-id="2825a-130">d.</span><span class="sxs-lookup"><span data-stu-id="2825a-130">d.</span></span> <span data-ttu-id="2825a-131">**Plats**: Azure-regionen som Batch-kontot skapas i.</span><span class="sxs-lookup"><span data-stu-id="2825a-131">**Location**: The Azure region in which to create the Batch account.</span></span> <span data-ttu-id="2825a-132">Endast de regioner som stöds av din prenumeration och resursgrupp visas som alternativ.</span><span class="sxs-lookup"><span data-stu-id="2825a-132">Only the regions supported by your subscription and resource group are displayed as options.</span></span>

    <span data-ttu-id="2825a-133">e.</span><span class="sxs-lookup"><span data-stu-id="2825a-133">e.</span></span> <span data-ttu-id="2825a-134">**Lagringskonto** (valfritt): Ett generellt Azure Storage-konto som du associerar med ditt Batch-konto.</span><span class="sxs-lookup"><span data-stu-id="2825a-134">**Storage account** (optional): A general-purpose Azure Storage account that you associate with your Batch account.</span></span> <span data-ttu-id="2825a-135">Detta rekommenderas för de flesta Batch-konton.</span><span class="sxs-lookup"><span data-stu-id="2825a-135">This is recommended for most Batch accounts.</span></span> <span data-ttu-id="2825a-136">Mer information finns i [Länkat Azure Storage-konto](#linked-azure-storage-account) senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="2825a-136">See [Linked Azure Storage account](#linked-azure-storage-account) later in this article for more details.</span></span>

4. <span data-ttu-id="2825a-137">Skapa kontot genom att klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="2825a-137">Click **Create** to create the account.</span></span>

   <span data-ttu-id="2825a-138">Portalen meddelar att distributionen pågår.</span><span class="sxs-lookup"><span data-stu-id="2825a-138">The portal indicates deployment is in progress.</span></span> <span data-ttu-id="2825a-139">När åtgärden har slutförts visas meddelandet **Distributionen är klar** i **Meddelanden**.</span><span class="sxs-lookup"><span data-stu-id="2825a-139">Upon completion, a **Deployments succeeded** notification appears in **Notifications**.</span></span>

## <a name="user-subscription-mode"></a><span data-ttu-id="2825a-140">Användarprenumerationsläge</span><span class="sxs-lookup"><span data-stu-id="2825a-140">User subscription mode</span></span>

### <a name="allow-azure-batch-to-access-the-subscription-one-time-operation"></a><span data-ttu-id="2825a-141">Tillåt att Azure Batch får åtkomst till prenumerationen (engångsåtgärd)</span><span class="sxs-lookup"><span data-stu-id="2825a-141">Allow Azure Batch to access the subscription (one-time operation)</span></span>
<span data-ttu-id="2825a-142">När du skapar ditt första Batch-konto i användarprenumerationsläge utför du följande steg för att registrera prenumerationen med Batch.</span><span class="sxs-lookup"><span data-stu-id="2825a-142">When creating your first Batch account in user subscription mode, perform the following steps to register your subscription with Batch.</span></span> <span data-ttu-id="2825a-143">(Om du redan har gjort detta går du vidare till nästa avsnitt.)</span><span class="sxs-lookup"><span data-stu-id="2825a-143">(If you previously did this, skip to the next section.)</span></span>

1. <span data-ttu-id="2825a-144">Logga in på [Azure Portal][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="2825a-144">Sign in to the [Azure portal][azure_portal].</span></span>

2. <span data-ttu-id="2825a-145">Klicka på **Fler tjänster** > **Prenumerationer** och klicka på den prenumeration som du vill använda för Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="2825a-145">Click **More Services** > **Subscriptions**, and click the subscription you want to use for the Batch account.</span></span>

3. <span data-ttu-id="2825a-146">På bladet **Prenumeration** klickar du på **Åtkomstkontroll (IAM)** > **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="2825a-146">In the **Subscription** blade, click **Access control (IAM)** > **Add**.</span></span>

    ![Åtkomstkontroll för prenumeration][subscription_access]

4. <span data-ttu-id="2825a-148">På bladet **Lägg till behörigheter** väljer du rollen **Deltagare** och söker efter Batch-API:t.</span><span class="sxs-lookup"><span data-stu-id="2825a-148">On the **Add permissions** blade, select the **Contributor** role, search for the Batch API.</span></span> <span data-ttu-id="2825a-149">Sök efter var och en av de här strängarna tills du hittar API:t:</span><span class="sxs-lookup"><span data-stu-id="2825a-149">Search for each of these strings until you find the API:</span></span>
    1. <span data-ttu-id="2825a-150">**MicrosoftAzureBatch**.</span><span class="sxs-lookup"><span data-stu-id="2825a-150">**MicrosoftAzureBatch**.</span></span>
    2. <span data-ttu-id="2825a-151">**Microsoft Azure Batch**.</span><span class="sxs-lookup"><span data-stu-id="2825a-151">**Microsoft Azure Batch**.</span></span> <span data-ttu-id="2825a-152">Nyare Azure AD-klientorganisationer kan använda det här namnet.</span><span class="sxs-lookup"><span data-stu-id="2825a-152">Newer Azure AD tenants may use this name.</span></span>
    3. <span data-ttu-id="2825a-153">**ddbf3205-c6bd-46ae-8127-60eb93363864** är id:t för API:t.</span><span class="sxs-lookup"><span data-stu-id="2825a-153">**ddbf3205-c6bd-46ae-8127-60eb93363864** is the ID for the Batch API.</span></span> 

5. <span data-ttu-id="2825a-154">När du har hittat Batch-API:t markerar du det och klickar på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="2825a-154">Once you find the Batch API, select it and click **Save**.</span></span>

    ![Lägg till Batch-behörigheter][add_permission]

### <a name="create-a-key-vault"></a><span data-ttu-id="2825a-156">Skapa ett nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="2825a-156">Create a key vault</span></span>
<span data-ttu-id="2825a-157">I användarprenumerationsläge krävs ett Azure-nyckelvalv som tillhör samma resursgrupp som Batch-kontot som ska skapas.</span><span class="sxs-lookup"><span data-stu-id="2825a-157">In user subscription mode, an Azure key vault is required that belongs to the same resource group as the Batch account to be created.</span></span> <span data-ttu-id="2825a-158">Kontrollera att resursgruppen finns i en region där Batch är [tillgängligt](https://azure.microsoft.com/regions/services/) och som din prenumeration stöder.</span><span class="sxs-lookup"><span data-stu-id="2825a-158">Make sure the resource group is in a region where Batch is [available](https://azure.microsoft.com/regions/services/) and which your subscription supports.</span></span>

1. <span data-ttu-id="2825a-159">På [Azure Portal][azure_portal] klickar du på **Nytt** > **Säkerhet + identitet** > **Nyckelvalv**.</span><span class="sxs-lookup"><span data-stu-id="2825a-159">In the [Azure portal][azure_portal], click **New** > **Security + Identity** > **Key Vault**.</span></span>

2. <span data-ttu-id="2825a-160">På bladet **Skapa nyckelvalv** anger du ett namn för nyckelvalvet och skapar en resursgrupp i den region som du vill använda för ditt Batch-konto.</span><span class="sxs-lookup"><span data-stu-id="2825a-160">In the **Create Key Vault** blade, enter a name for the key vault, and create a resource group in the region you want for your Batch account.</span></span> <span data-ttu-id="2825a-161">Lämna standardvärdena för resten av inställningarna och klicka sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="2825a-161">Leave the remaining settings at default values, then click **Create**.</span></span>

### <a name="create-a-batch-account"></a><span data-ttu-id="2825a-162">Skapa ett Batch-konto</span><span class="sxs-lookup"><span data-stu-id="2825a-162">Create a Batch account</span></span>

1. <span data-ttu-id="2825a-163">På [Azure Portal][azure_portal] klickar du på **Nytt** > **Beräkna** > **Batch-tjänst**.</span><span class="sxs-lookup"><span data-stu-id="2825a-163">In the [Azure portal][azure_portal], click **New** > **Compute** > **Batch Service**.</span></span>

    ![Batch på Marketplace][marketplace_portal]
3. <span data-ttu-id="2825a-165">Bladet **Nytt Batch-konto** visas.</span><span class="sxs-lookup"><span data-stu-id="2825a-165">The **New Batch Account** blade is displayed.</span></span> <span data-ttu-id="2825a-166">Se beskrivningarna nedan av varje element på bladet.</span><span class="sxs-lookup"><span data-stu-id="2825a-166">See the descriptions below of each blade element.</span></span>

    ![Skapa ett Batch-konto][account_portal_byos]

    <span data-ttu-id="2825a-168">a.</span><span class="sxs-lookup"><span data-stu-id="2825a-168">a.</span></span> <span data-ttu-id="2825a-169">**Kontonamn**: Det Batch-kontonamn som du väljer måste vara unikt i den Azure-region där kontot skapas (se **Plats** nedan).</span><span class="sxs-lookup"><span data-stu-id="2825a-169">**Account name**: The Batch account name you choose must be unique within the Azure region where the account is created (see **Location** below).</span></span> <span data-ttu-id="2825a-170">Kontonamnet får bara innehålla gemener eller siffror och måste vara mellan 3 och 24 tecken långt.</span><span class="sxs-lookup"><span data-stu-id="2825a-170">The account name may contain only lowercase characters or numbers, and must be 3-24 characters in length.</span></span>

    <span data-ttu-id="2825a-171">b.</span><span class="sxs-lookup"><span data-stu-id="2825a-171">b.</span></span> <span data-ttu-id="2825a-172">**Prenumeration**: Om du har mer än en prenumeration väljer du den prenumeration som du registrerade med Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2825a-172">**Subscription**: If you have more than one subscription, select the subscription that you registered with the Batch service.</span></span>

    <span data-ttu-id="2825a-173">c.</span><span class="sxs-lookup"><span data-stu-id="2825a-173">c.</span></span> <span data-ttu-id="2825a-174">**Poolallokeringsläge**: Välj **Användarprenumeration**.</span><span class="sxs-lookup"><span data-stu-id="2825a-174">**Pool allocation mode**: Select **User subscription**.</span></span>

    <span data-ttu-id="2825a-175">d.</span><span class="sxs-lookup"><span data-stu-id="2825a-175">d.</span></span> <span data-ttu-id="2825a-176">**Nyckelvalv**: Välj det nyckelvalv som du skapade för Batch-kontot i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2825a-176">**Key vault**: Select the key vault you created for your Batch account in the previous section.</span></span> <span data-ttu-id="2825a-177">Om du vill kan du skapa ett nytt nyckelvalv.</span><span class="sxs-lookup"><span data-stu-id="2825a-177">Optionally, create a new key vault.</span></span> <span data-ttu-id="2825a-178">När du har valt valvet markerar du kryssrutan för att ge Azure Batch åtkomst till nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="2825a-178">After selecting the vault, select the checkbox to grant Azure Batch access to the key vault.</span></span>

    <span data-ttu-id="2825a-179">c.</span><span class="sxs-lookup"><span data-stu-id="2825a-179">c.</span></span> <span data-ttu-id="2825a-180">**Resursgrupp**: Välj den resursgrupp där du skapade nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="2825a-180">**Resource group**: Select the resource group in which you  created the key vault.</span></span>

    <span data-ttu-id="2825a-181">d.</span><span class="sxs-lookup"><span data-stu-id="2825a-181">d.</span></span> <span data-ttu-id="2825a-182">**Plats**: Den Azure-region där du skapade nyckelvalvet för Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="2825a-182">**Location**: The Azure region in which you created the key vault for the Batch account.</span></span>

    <span data-ttu-id="2825a-183">e.</span><span class="sxs-lookup"><span data-stu-id="2825a-183">e.</span></span> <span data-ttu-id="2825a-184">**Lagringskonto** (valfritt): Ett generellt Azure Storage-konto som du associerar med ditt Batch-konto.</span><span class="sxs-lookup"><span data-stu-id="2825a-184">**Storage account** (optional): A general-purpose Azure Storage account that you associate with your Batch account.</span></span> <span data-ttu-id="2825a-185">Detta rekommenderas för de flesta Batch-konton.</span><span class="sxs-lookup"><span data-stu-id="2825a-185">This is recommended for most Batch accounts.</span></span> <span data-ttu-id="2825a-186">Mer information finns i [Länkat Azure Storage-konto](#linked-azure-storage-account) nedan.</span><span class="sxs-lookup"><span data-stu-id="2825a-186">See [Linked Azure Storage account](#linked-azure-storage-account) below for more details.</span></span>

4. <span data-ttu-id="2825a-187">Skapa kontot genom att klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="2825a-187">Click **Create** to create the account.</span></span>

   <span data-ttu-id="2825a-188">Portalen meddelar att distributionen pågår.</span><span class="sxs-lookup"><span data-stu-id="2825a-188">The portal indicates deployment is in progress.</span></span> <span data-ttu-id="2825a-189">När åtgärden har slutförts visas meddelandet **Distributionen är klar** i **Meddelanden**.</span><span class="sxs-lookup"><span data-stu-id="2825a-189">Upon completion, a **Deployments succeeded** notification appears in **Notifications**.</span></span>



## <a name="view-batch-account-properties"></a><span data-ttu-id="2825a-190">Visa egenskaper för ett Batch-konto</span><span class="sxs-lookup"><span data-stu-id="2825a-190">View Batch account properties</span></span>
<span data-ttu-id="2825a-191">När kontot har skapats kan du öppna bladet för **Batch-kontot** och komma åt kontots inställningar och egenskaper.</span><span class="sxs-lookup"><span data-stu-id="2825a-191">Once the account has been created, you can open the **Batch account blade** to access its settings and properties.</span></span> <span data-ttu-id="2825a-192">Du kan komma åt alla kontoinställningar och kontoegenskaper från den vänstra menyn på bladet för Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="2825a-192">You can access all account settings and properties by using the left menu of the Batch account blade.</span></span>

![Bladet för Batch-kontot på Azure Portal][account_blade]

* <span data-ttu-id="2825a-194">**Batch-kontots URL**: När du utvecklar ett program med [Batch-API:er](batch-apis-tools.md#azure-accounts-for-batch-development), behöver du ett konto-URL till dina Batch-resurser.</span><span class="sxs-lookup"><span data-stu-id="2825a-194">**Batch account URL**: When you develop an application with the [Batch APIs](batch-apis-tools.md#azure-accounts-for-batch-development), you'll need an account URL to access your Batch resources.</span></span> <span data-ttu-id="2825a-195">URL:en för ett Batch-konto har följande format:</span><span class="sxs-lookup"><span data-stu-id="2825a-195">A Batch account URL has the following format:</span></span>

    `https://<account_name>.<region>.batch.azure.com`

![Batch-kontots URL på portalen][account_url]

* <span data-ttu-id="2825a-197">**Snabbtangenter** (Batch-tjänstläge): För att autentisera åtkomsten till ditt Batch-konto från programmet behöver du en snabbtangent för kontot.</span><span class="sxs-lookup"><span data-stu-id="2825a-197">**Access keys** (Batch service mode): To authenticate access to your Batch account from your application, you'll need an account access key.</span></span> <span data-ttu-id="2825a-198">(Den här inställningen är inte tillgänglig i användarprenumerationsläge, där du använder Azure Active Directory-autentisering.)</span><span class="sxs-lookup"><span data-stu-id="2825a-198">(This setting is not available in user subscription mode, where you use Azure Active Directory authentication.)</span></span>

    <span data-ttu-id="2825a-199">Om du vill visa eller återskapa Batch-kontots åtkomstnycklar skriver du `keys` i rutan **Sök** på den vänstra menyn i bladet för Batch-kontot och väljer **Nycklar**.</span><span class="sxs-lookup"><span data-stu-id="2825a-199">To view or regenerate your Batch account's access keys, enter `keys` in the left menu **Search** box on the Batch account blade, then select **Keys**.</span></span>

    ![Batch-kontonycklar på Azure Portal][account_keys]

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="linked-azure-storage-account"></a><span data-ttu-id="2825a-201">Länkat Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="2825a-201">Linked Azure Storage account</span></span>

<span data-ttu-id="2825a-202">Om du vill kan du länka ett allmänt Azure Storage-konto till ditt Batch-konto.</span><span class="sxs-lookup"><span data-stu-id="2825a-202">You can optionally link a general-purpose Azure Storage account to your Batch account.</span></span> <span data-ttu-id="2825a-203">Funktionen [programpaket](batch-application-packages.md) i Batch använder Azure Blob Storage, precis som biblioteket [Batch File Conventions .NET](batch-task-output.md) gör.</span><span class="sxs-lookup"><span data-stu-id="2825a-203">The [application packages](batch-application-packages.md) feature of Batch uses Azure Blob storage, as does the [Batch File Conventions .NET](batch-task-output.md) library.</span></span> <span data-ttu-id="2825a-204">Med dessa valfria funktioner kan du distribuera de program som körs av Batch-aktiviteterna och spara de data som de genererar.</span><span class="sxs-lookup"><span data-stu-id="2825a-204">These optional features assist you in deploying the applications that your Batch tasks run, and persisting the data they produce.</span></span>

<span data-ttu-id="2825a-205">Vi rekommenderar att du skapar ett nytt lagringskonto för exklusiv användning av Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="2825a-205">We recommend that you create a new Storage account exclusively for use by your Batch account.</span></span>

![Skapa ett allmänt lagringskonto][storage_account]

> [!NOTE]
> <span data-ttu-id="2825a-207">Azure Batch stöder för närvarande endast den allmänna lagringskontotypen.</span><span class="sxs-lookup"><span data-stu-id="2825a-207">Azure Batch currently supports only the general-purpose Storage account type.</span></span> <span data-ttu-id="2825a-208">Den här kontotypen beskrivs i steg 5, [Skapa ett lagringskonto] (../storage/common/storage-create-storage-account.md#create-a-storage-account), i [Om Azure Storage-konton](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="2825a-208">This account type is described in step 5, [Create a storage account] (../storage/common/storage-create-storage-account.md#create-a-storage-account), in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>
>
>

> [!WARNING]
> <span data-ttu-id="2825a-209">Var noga när du återskapar åtkomstnycklarna för ett länkat Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="2825a-209">Be careful when regenerating the access keys of a linked Storage account.</span></span> <span data-ttu-id="2825a-210">Återskapa endast en Storage-kontonyckel och klicka på **Synkronisera nycklar** på bladet för det länkade Storage-kontot.</span><span class="sxs-lookup"><span data-stu-id="2825a-210">Regenerate only one Storage account key and click **Sync Keys** on the linked Storage account blade.</span></span> <span data-ttu-id="2825a-211">Vänta fem minuter så att nycklarna hinner distribueras till beräkningsnoderna i poolerna och återskapa och synkronisera sedan den andra nyckeln om det behövs.</span><span class="sxs-lookup"><span data-stu-id="2825a-211">Wait five minutes to allow the keys to propagate to the compute nodes in your pools, then regenerate and synchronize the other key if necessary.</span></span> <span data-ttu-id="2825a-212">Om du återskapar båda nycklarna samtidigt kan dina beräkningsnoder inte synkronisera någon av nycklarna och de förlorar åtkomst till Storage-kontot.</span><span class="sxs-lookup"><span data-stu-id="2825a-212">If you regenerate both keys at the same time, your compute nodes will not be able to synchronize either key, and they will lose access to the Storage account.</span></span>
>
>

<span data-ttu-id="2825a-213">![Återskapa lagringskontonycklar][4]</span><span class="sxs-lookup"><span data-stu-id="2825a-213">![Regenerating storage account keys][4]</span></span>

## <a name="batch-service-quotas-and-limits"></a><span data-ttu-id="2825a-214">Kvoter och begränsningar för Batch-tjänsten</span><span class="sxs-lookup"><span data-stu-id="2825a-214">Batch service quotas and limits</span></span>
<span data-ttu-id="2825a-215">Tänk på att, precis som din Azure-prenumeration och andra Azure-tjänster, så har Batch-konton vissa [kvoter och begränsningar](batch-quota-limit.md).</span><span class="sxs-lookup"><span data-stu-id="2825a-215">Please be aware that as with your Azure subscription and other Azure services, certain [quotas and limits](batch-quota-limit.md) apply to Batch accounts.</span></span> <span data-ttu-id="2825a-216">De aktuella kvoterna för ett Batch-konto visas på portalen i **kontoegenskaperna**.</span><span class="sxs-lookup"><span data-stu-id="2825a-216">Current quotas for a Batch account appear in the portal in the account **Properties**.</span></span>

![Kvoter för Batch-konto på Azure Portal][quotas]



<span data-ttu-id="2825a-218">Vidare kan du öka många av dessa kvoter genom att bara skicka en kostnadsfri begäran om produktsupport på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="2825a-218">Additionally, many of these quotas can be increased simply with a free product support request submitted in the Azure portal.</span></span> <span data-ttu-id="2825a-219">Information om hur du gör den här typen av begäran finns i [Kvoter och begränsningar för Azure Batch-tjänsten](batch-quota-limit.md).</span><span class="sxs-lookup"><span data-stu-id="2825a-219">See [Quotas and limits for the Azure Batch service](batch-quota-limit.md) for details on requesting quota increases.</span></span>

## <a name="other-batch-account-management-options"></a><span data-ttu-id="2825a-220">Andra alternativ för Batch-kontohantering</span><span class="sxs-lookup"><span data-stu-id="2825a-220">Other Batch account management options</span></span>
<span data-ttu-id="2825a-221">Förutom att använda Azure Portal kan du också skapa och hantera Batch-konton med följande:</span><span class="sxs-lookup"><span data-stu-id="2825a-221">In addition to using the Azure portal, you can also create and manage Batch accounts with the following:</span></span>

* [<span data-ttu-id="2825a-222">PowerShell-cmdlets för Batch</span><span class="sxs-lookup"><span data-stu-id="2825a-222">Batch PowerShell cmdlets</span></span>](batch-powershell-cmdlets-get-started.md)
* [<span data-ttu-id="2825a-223">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2825a-223">Azure CLI</span></span>](batch-cli-get-started.md)
* [<span data-ttu-id="2825a-224">Batch Management .NET</span><span class="sxs-lookup"><span data-stu-id="2825a-224">Batch Management .NET</span></span>](batch-management-dotnet.md)

## <a name="next-steps"></a><span data-ttu-id="2825a-225">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2825a-225">Next steps</span></span>
* <span data-ttu-id="2825a-226">Mer information om begrepp och funktioner relaterade till Batch-tjänsten finns i [funktionsöversikten för Batch](batch-api-basics.md).</span><span class="sxs-lookup"><span data-stu-id="2825a-226">See the [Batch feature overview](batch-api-basics.md) to learn more about Batch service concepts and features.</span></span> <span data-ttu-id="2825a-227">Artikeln beskriver de viktigaste Batch-resurserna, t.ex. pooler, beräkningsnoder, jobb och aktiviteter, och innehåller en översikt över funktioner i tjänsten som stöder storskaliga arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="2825a-227">The article discusses the primary Batch resources such as pools, compute nodes, jobs, and tasks, and provides an overview of the service's features that enable large-scale compute workload execution.</span></span>
* <span data-ttu-id="2825a-228">Lär dig hur du utvecklar ett enkelt Batch-aktiverat program med hjälp av [Batch .NET-klientbiblioteket](batch-dotnet-get-started.md) eller [Python](batch-python-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="2825a-228">Learn the basics of developing a Batch-enabled application using the [Batch .NET client library](batch-dotnet-get-started.md) or [Python](batch-python-tutorial.md).</span></span> <span data-ttu-id="2825a-229">Introduktionsartikeln beskriver steg för steg ett program som använder Batch-tjänsten för att köra en arbetsbelastning på flera beräkningsnoder och förklarar hur du använder Azure Storage för mellanlagring och hämtning av filer i arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="2825a-229">These introductory articles guide you through a working application that uses the Batch service to execute a workload on multiple compute nodes, and includes using Azure Storage for workload file staging and retrieval.</span></span>

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
