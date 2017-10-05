---
title: "Azure portal för resursprinciper | Microsoft Docs"
description: "Beskriver hur du använder Azure-portalen för att skapa och hantera principer för hanteraren för filserverresurser. Principer kan tillämpas på grupperna prenumerationen eller resursen."
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
ms.openlocfilehash: 868b2cc1559053057d17b34c03e2e31347f399bf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-portal-to-assign-and-manage-resource-policies"></a><span data-ttu-id="af3f7-104">Använd Azure-portalen för att tilldela och hantera principer för företagsresurser</span><span class="sxs-lookup"><span data-stu-id="af3f7-104">Use Azure portal to assign and manage resource policies</span></span>
<span data-ttu-id="af3f7-105">Azure-portalen kan du tilldela resursprinciper resursgrupper och prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="af3f7-105">The Azure portal enables you to assign resource policies to resource groups and subscriptions.</span></span> <span data-ttu-id="af3f7-106">Användargränssnittet gör det enkelt att välja den princip som du vill tilldela och ange parametervärden att anpassa inställningarna för principen.</span><span class="sxs-lookup"><span data-stu-id="af3f7-106">The user interface makes it easy to select the policy that you want to assign, and specify parameter values for that policy to customize the policy settings.</span></span> 

<span data-ttu-id="af3f7-107">Om du vill tilldela en princip via portalen finnas principdefinitionen redan i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="af3f7-107">To assign a policy through the portal, the policy definition must already exist in your subscription.</span></span> <span data-ttu-id="af3f7-108">Din prenumeration har flera definitioner av inbyggd princip som du vill tilldela till resursgrupper eller prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="af3f7-108">Your subscription has several built-in policy definitions that are ready for you to assign to resource groups or subscriptions.</span></span> <span data-ttu-id="af3f7-109">Du kan se dessa inbyggda principer och eventuella anpassade principer som du har definierat när du använder portalen för att tilldela principer.</span><span class="sxs-lookup"><span data-stu-id="af3f7-109">You see these built-in policies and any custom policies you have defined when using the portal to assign policies.</span></span> <span data-ttu-id="af3f7-110">En introduktion till principer och hur du definierar en anpassad princip finns [resurs uppgifter](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="af3f7-110">For an introduction to policies and how to define customized policy, see [Resource policy overview](resource-manager-policy.md).</span></span>

<span data-ttu-id="af3f7-111">Principer ärvs av alla underordnade resurser.</span><span class="sxs-lookup"><span data-stu-id="af3f7-111">Policies are inherited by all child resources.</span></span> <span data-ttu-id="af3f7-112">Om en princip används för en resursgrupp, är det så gäller för alla resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="af3f7-112">So, if a policy is applied to a resource group, it is applicable to all the resources in that resource group.</span></span> <span data-ttu-id="af3f7-113">I den här artikeln termen **omfång** refererar till den resursgrupp eller prenumeration som är tilldelade principen.</span><span class="sxs-lookup"><span data-stu-id="af3f7-113">In this article, the term **scope** refers to the resource group or subscription that is assigned the policy.</span></span> 

<span data-ttu-id="af3f7-114">Principer utvärderas när du skapar och uppdaterar resurser (PLACERA och korrigering operations).</span><span class="sxs-lookup"><span data-stu-id="af3f7-114">Policies are evaluated when creating and updating resources (PUT and PATCH operations).</span></span>

## <a name="assign-a-policy"></a><span data-ttu-id="af3f7-115">Tilldela en princip</span><span class="sxs-lookup"><span data-stu-id="af3f7-115">Assign a policy</span></span>

1. <span data-ttu-id="af3f7-116">Om du vill tilldela en princip till en resursgrupp eller prenumeration väljer du den resursgrupp eller prenumeration.</span><span class="sxs-lookup"><span data-stu-id="af3f7-116">To assign a policy to either a resource group or subscription, select that resource group or subscription.</span></span> <span data-ttu-id="af3f7-117">I inställningar, väljer **principer**.</span><span class="sxs-lookup"><span data-stu-id="af3f7-117">In the settings, select **Policies**.</span></span>

   ![Välj principer](./media/resource-manager-policy-portal/select-policies.png)

2. <span data-ttu-id="af3f7-119">Om du vill skapa en principtilldelning för detta scope, Välj **Lägg till tilldelning**.</span><span class="sxs-lookup"><span data-stu-id="af3f7-119">To create a policy assignment for this scope, select **Add assignment**.</span></span>

   ![Lägg till tilldelning](./media/resource-manager-policy-portal/add-assignment.png)

3. <span data-ttu-id="af3f7-121">Välj den princip som du vill tilldela.</span><span class="sxs-lookup"><span data-stu-id="af3f7-121">Select the policy you want to assign.</span></span> <span data-ttu-id="af3f7-122">Även om du inte har lagt till alla principdefinitioner till din prenumeration kan du se de inbyggda principer som är tillgängliga för tilldelning.</span><span class="sxs-lookup"><span data-stu-id="af3f7-122">Even if you have not added any policy definitions to your subscription, you see the built-in policies that are available for assignment.</span></span> <span data-ttu-id="af3f7-123">De här inbyggda täcker många vanliga scenarier.</span><span class="sxs-lookup"><span data-stu-id="af3f7-123">These built-in policies cover many common scenarios.</span></span>

   ![Välj definition](./media/resource-manager-policy-portal/select-definition.png)

4. <span data-ttu-id="af3f7-125">När du har valt en princip, finns en beskrivning av principen och eventuella parametrar för grupprincipobjekt.</span><span class="sxs-lookup"><span data-stu-id="af3f7-125">After selecting a policy, you see a description of the policy, and any parameters for that policy.</span></span> <span data-ttu-id="af3f7-126">Till exempel följande bild visar den **tillåtna platser** som krävs för den princip som begränsar tillgängliga platser.</span><span class="sxs-lookup"><span data-stu-id="af3f7-126">For example, the following image shows the **Allowed locations** parameter, which is required for the policy that restricts the available locations.</span></span>

   ![Visa parametrar](./media/resource-manager-policy-portal/show-parameters.png)

5. <span data-ttu-id="af3f7-128">Markera värdena du anger för principparametrar (t.ex de platser som kan användas för distributionen) via användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="af3f7-128">Through the user interface, select the values to specify for the policy parameters (such as the locations that can be used for deployment).</span></span>

   ![Välj parametervärden](./media/resource-manager-policy-portal/select-parameters.png)

6. <span data-ttu-id="af3f7-130">Ange värden för de andra parametrarna.</span><span class="sxs-lookup"><span data-stu-id="af3f7-130">Provide values for the other parameters.</span></span> <span data-ttu-id="af3f7-131">Omfånget tilldelas automatiskt baserat på bladet som du valde när du startar principtilldelningen.</span><span class="sxs-lookup"><span data-stu-id="af3f7-131">The scope is automatically assigned based on the blade you selected when starting the policy assignment.</span></span> <span data-ttu-id="af3f7-132">Välj **OK** när du är klar.</span><span class="sxs-lookup"><span data-stu-id="af3f7-132">Select **OK** when done.</span></span>

   ![Definiera parametrar](./media/resource-manager-policy-portal/define-parameters.png)

  <span data-ttu-id="af3f7-134">Principen har tilldelats det specificerade omfånget.</span><span class="sxs-lookup"><span data-stu-id="af3f7-134">You have assigned the policy to the specified scope.</span></span>

## <a name="view-policy-assignments"></a><span data-ttu-id="af3f7-135">Visa principtilldelningar</span><span class="sxs-lookup"><span data-stu-id="af3f7-135">View policy assignments</span></span>

<span data-ttu-id="af3f7-136">Efter att en princip, ser du den i listan med principer för detta omfång.</span><span class="sxs-lookup"><span data-stu-id="af3f7-136">After assigning a policy, you see it in the list of policies for that scope.</span></span> <span data-ttu-id="af3f7-137">Den **information** fliken visas en sammanfattning av tilldelning av principer.</span><span class="sxs-lookup"><span data-stu-id="af3f7-137">The **Details** tab shows a summary of the policy assignment.</span></span>

![Visa information](./media/resource-manager-policy-portal/show-details.png)

<span data-ttu-id="af3f7-139">Den **tilldelningsregel** visar JSON för principdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="af3f7-139">The **Assignment rule** tab shows the JSON for the policy definition.</span></span>

![Visa tilldelningsregel](./media/resource-manager-policy-portal/show-assignment-rule.png)

## <a name="change-an-existing-policy-assignment"></a><span data-ttu-id="af3f7-141">Ändra en befintlig principtilldelning</span><span class="sxs-lookup"><span data-stu-id="af3f7-141">Change an existing policy assignment</span></span>

<span data-ttu-id="af3f7-142">Om du vill ändra en princip, Välj **Redigera tilldelning** eller **ta bort**</span><span class="sxs-lookup"><span data-stu-id="af3f7-142">To change a policy, select **Edit assignment** or **Delete**</span></span>

![Redigera eller ta bort tilldelning](./media/resource-manager-policy-portal/edit-delete-policy.png)

## <a name="assign-custom-policies"></a><span data-ttu-id="af3f7-144">Tilldela anpassade principer</span><span class="sxs-lookup"><span data-stu-id="af3f7-144">Assign custom policies</span></span>

<span data-ttu-id="af3f7-145">Om du har definierat anpassade principer i din prenumeration, är dessa principer tillgängliga för tilldelning via portalen.</span><span class="sxs-lookup"><span data-stu-id="af3f7-145">If you have defined custom policies in your subscription, those policies are available for assignment through the portal.</span></span> <span data-ttu-id="af3f7-146">Dessa principer inleds med **[Custom]**</span><span class="sxs-lookup"><span data-stu-id="af3f7-146">Those policies are prefaced with **[Custom]**</span></span>

![anpassade principer](./media/resource-manager-policy-portal/show-custom-policy.png)

## <a name="next-steps"></a><span data-ttu-id="af3f7-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="af3f7-148">Next steps</span></span>
* <span data-ttu-id="af3f7-149">Läs om JSON-syntax för att definiera principer i [resurs uppgifter](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="af3f7-149">To learn about the JSON syntax for defining policies, see [Resource policy overview](resource-manager-policy.md).</span></span>
* <span data-ttu-id="af3f7-150">Vägledning för hur företag kan använda resurshanteraren för att effektivt hantera prenumerationer finns i [Azure enterprise scaffold - förebyggande prenumerationsåtgärder](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="af3f7-150">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>
* <span data-ttu-id="af3f7-151">Princip-schemat har publicerats på [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json).</span><span class="sxs-lookup"><span data-stu-id="af3f7-151">The policy schema is published at [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json).</span></span> 

