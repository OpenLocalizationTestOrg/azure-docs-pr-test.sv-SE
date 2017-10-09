---
title: "aaaAzure portal för resursprinciper | Microsoft Docs"
description: "Beskriver hur toouse Azure portal toocreate och hantera principer för hanteraren för filserverresurser. Principer kan tillämpas på hello prenumerationen eller resursen grupper."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: tomfitz
ms.openlocfilehash: ce6413386317e532b63761a24458b85c996af4a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-portal-tooassign-and-manage-resource-policies"></a><span data-ttu-id="1265e-104">Använda Azure portal tooassign och hantera principer för företagsresurser</span><span class="sxs-lookup"><span data-stu-id="1265e-104">Use Azure portal tooassign and manage resource policies</span></span>
<span data-ttu-id="1265e-105">hello Azure-portalen kan du tooassign principer tooresource resursgrupper och prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="1265e-105">hello Azure portal enables you tooassign resource policies tooresource groups and subscriptions.</span></span> <span data-ttu-id="1265e-106">hello användargränssnittet gör det enkelt tooselect hello princip som du vill tooassign och ange parametervärden för denna princip toocustomize hello principinställningar.</span><span class="sxs-lookup"><span data-stu-id="1265e-106">hello user interface makes it easy tooselect hello policy that you want tooassign, and specify parameter values for that policy toocustomize hello policy settings.</span></span> 

<span data-ttu-id="1265e-107">tooassign en princip hello-portalen hello principdefinitionen måste redan finnas i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1265e-107">tooassign a policy through hello portal, hello policy definition must already exist in your subscription.</span></span> <span data-ttu-id="1265e-108">Din prenumeration har flera inbyggda principdefinitioner som är klara för du tooassign tooresource grupper eller prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="1265e-108">Your subscription has several built-in policy definitions that are ready for you tooassign tooresource groups or subscriptions.</span></span> <span data-ttu-id="1265e-109">Du kan se dessa inbyggda principer och eventuella anpassade principer som du har definierat när du använder hello portal tooassign principer.</span><span class="sxs-lookup"><span data-stu-id="1265e-109">You see these built-in policies and any custom policies you have defined when using hello portal tooassign policies.</span></span> <span data-ttu-id="1265e-110">En introduktion toopolicies och hur toodefine anpassad princip finns [resurs uppgifter](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="1265e-110">For an introduction toopolicies and how toodefine customized policy, see [Resource policy overview](resource-manager-policy.md).</span></span>

<span data-ttu-id="1265e-111">Principer ärvs av alla underordnade resurser.</span><span class="sxs-lookup"><span data-stu-id="1265e-111">Policies are inherited by all child resources.</span></span> <span data-ttu-id="1265e-112">Så om en princip är tillämpade tooa resursgrupp, är det tillämpliga tooall hello resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="1265e-112">So, if a policy is applied tooa resource group, it is applicable tooall hello resources in that resource group.</span></span> <span data-ttu-id="1265e-113">I den här artikeln hello termen **omfång** refererar toohello resursgrupp eller prenumeration som är tilldelad hello princip.</span><span class="sxs-lookup"><span data-stu-id="1265e-113">In this article, hello term **scope** refers toohello resource group or subscription that is assigned hello policy.</span></span> 

<span data-ttu-id="1265e-114">Principer utvärderas när du skapar och uppdaterar resurser (PLACERA och korrigering operations).</span><span class="sxs-lookup"><span data-stu-id="1265e-114">Policies are evaluated when creating and updating resources (PUT and PATCH operations).</span></span>

## <a name="assign-a-policy"></a><span data-ttu-id="1265e-115">Tilldela en princip</span><span class="sxs-lookup"><span data-stu-id="1265e-115">Assign a policy</span></span>

1. <span data-ttu-id="1265e-116">tooassign princip-tooeither en resursgrupp eller prenumeration, Välj den resursgrupp eller prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1265e-116">tooassign a policy tooeither a resource group or subscription, select that resource group or subscription.</span></span> <span data-ttu-id="1265e-117">Markera hello inställningar **principer**.</span><span class="sxs-lookup"><span data-stu-id="1265e-117">In hello settings, select **Policies**.</span></span>

   ![Välj principer](./media/resource-manager-policy-portal/select-policies.png)

2. <span data-ttu-id="1265e-119">Välj toocreate en principtilldelning för detta scope **Lägg till tilldelning**.</span><span class="sxs-lookup"><span data-stu-id="1265e-119">toocreate a policy assignment for this scope, select **Add assignment**.</span></span>

   ![Lägg till tilldelning](./media/resource-manager-policy-portal/add-assignment.png)

3. <span data-ttu-id="1265e-121">Välj hello-princip som du vill tooassign.</span><span class="sxs-lookup"><span data-stu-id="1265e-121">Select hello policy you want tooassign.</span></span> <span data-ttu-id="1265e-122">Även om du inte har lagt till någon princip definitioner tooyour prenumeration kan se du hello inbyggda principer som är tillgängliga för tilldelning.</span><span class="sxs-lookup"><span data-stu-id="1265e-122">Even if you have not added any policy definitions tooyour subscription, you see hello built-in policies that are available for assignment.</span></span> <span data-ttu-id="1265e-123">De här inbyggda täcker många vanliga scenarier.</span><span class="sxs-lookup"><span data-stu-id="1265e-123">These built-in policies cover many common scenarios.</span></span>

   ![Välj definition](./media/resource-manager-policy-portal/select-definition.png)

4. <span data-ttu-id="1265e-125">När du har valt en princip, visas en beskrivning av principen för hello och eventuella parametrar för grupprincipobjekt.</span><span class="sxs-lookup"><span data-stu-id="1265e-125">After selecting a policy, you see a description of hello policy, and any parameters for that policy.</span></span> <span data-ttu-id="1265e-126">Hello följande bild visar exempelvis hello **tillåtna platser** som krävs för hello-princip som hindrar hello tillgängliga platser.</span><span class="sxs-lookup"><span data-stu-id="1265e-126">For example, hello following image shows hello **Allowed locations** parameter, which is required for hello policy that restricts hello available locations.</span></span>

   ![Visa parametrar](./media/resource-manager-policy-portal/show-parameters.png)

5. <span data-ttu-id="1265e-128">Välj hello värden toospecify för hello principparametrar (t.ex hello platser som kan användas för distribution) via hello användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="1265e-128">Through hello user interface, select hello values toospecify for hello policy parameters (such as hello locations that can be used for deployment).</span></span>

   ![Välj parametervärden](./media/resource-manager-policy-portal/select-parameters.png)

6. <span data-ttu-id="1265e-130">Ange värden för hello andra parametrar.</span><span class="sxs-lookup"><span data-stu-id="1265e-130">Provide values for hello other parameters.</span></span> <span data-ttu-id="1265e-131">hello scope tilldelas automatiskt baserat på hello bladet som du valde när du startar hello tilldelning av principer.</span><span class="sxs-lookup"><span data-stu-id="1265e-131">hello scope is automatically assigned based on hello blade you selected when starting hello policy assignment.</span></span> <span data-ttu-id="1265e-132">Välj **OK** när du är klar.</span><span class="sxs-lookup"><span data-stu-id="1265e-132">Select **OK** when done.</span></span>

   ![Definiera parametrar](./media/resource-manager-policy-portal/define-parameters.png)

  <span data-ttu-id="1265e-134">Du har tilldelat hello princip toohello angivna omfattningen.</span><span class="sxs-lookup"><span data-stu-id="1265e-134">You have assigned hello policy toohello specified scope.</span></span>

## <a name="view-policy-assignments"></a><span data-ttu-id="1265e-135">Visa principtilldelningar</span><span class="sxs-lookup"><span data-stu-id="1265e-135">View policy assignments</span></span>

<span data-ttu-id="1265e-136">Efter att en princip syns i hello listan med principer för detta omfång.</span><span class="sxs-lookup"><span data-stu-id="1265e-136">After assigning a policy, you see it in hello list of policies for that scope.</span></span> <span data-ttu-id="1265e-137">Hej **information** fliken visas en sammanfattning av hello tilldelning av principer.</span><span class="sxs-lookup"><span data-stu-id="1265e-137">hello **Details** tab shows a summary of hello policy assignment.</span></span>

![Visa information](./media/resource-manager-policy-portal/show-details.png)

<span data-ttu-id="1265e-139">Hej **tilldelningsregel** visar hello JSON för hello principdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="1265e-139">hello **Assignment rule** tab shows hello JSON for hello policy definition.</span></span>

![Visa tilldelningsregel](./media/resource-manager-policy-portal/show-assignment-rule.png)

## <a name="change-an-existing-policy-assignment"></a><span data-ttu-id="1265e-141">Ändra en befintlig principtilldelning</span><span class="sxs-lookup"><span data-stu-id="1265e-141">Change an existing policy assignment</span></span>

<span data-ttu-id="1265e-142">toochange en princip, Välj **Redigera tilldelning** eller **ta bort**</span><span class="sxs-lookup"><span data-stu-id="1265e-142">toochange a policy, select **Edit assignment** or **Delete**</span></span>

![Redigera eller ta bort tilldelning](./media/resource-manager-policy-portal/edit-delete-policy.png)

## <a name="assign-custom-policies"></a><span data-ttu-id="1265e-144">Tilldela anpassade principer</span><span class="sxs-lookup"><span data-stu-id="1265e-144">Assign custom policies</span></span>

<span data-ttu-id="1265e-145">Om du har definierat anpassade principer i din prenumeration, kan dessa principer tilldelas hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="1265e-145">If you have defined custom policies in your subscription, those policies are available for assignment through hello portal.</span></span> <span data-ttu-id="1265e-146">Dessa principer inleds med **[Custom]**</span><span class="sxs-lookup"><span data-stu-id="1265e-146">Those policies are prefaced with **[Custom]**</span></span>

![anpassade principer](./media/resource-manager-policy-portal/show-custom-policy.png)

## <a name="next-steps"></a><span data-ttu-id="1265e-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1265e-148">Next steps</span></span>
* <span data-ttu-id="1265e-149">toolearn hello JSON-syntax för att definiera principer, se [resurs uppgifter](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="1265e-149">toolearn about hello JSON syntax for defining policies, see [Resource policy overview](resource-manager-policy.md).</span></span>
* <span data-ttu-id="1265e-150">Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="1265e-150">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>
* <span data-ttu-id="1265e-151">hello princip schema har publicerats på [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json).</span><span class="sxs-lookup"><span data-stu-id="1265e-151">hello policy schema is published at [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json).</span></span> 

