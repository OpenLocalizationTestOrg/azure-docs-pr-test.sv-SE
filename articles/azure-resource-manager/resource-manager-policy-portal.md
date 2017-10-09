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
# <a name="use-azure-portal-tooassign-and-manage-resource-policies"></a>Använda Azure portal tooassign och hantera principer för företagsresurser
hello Azure-portalen kan du tooassign principer tooresource resursgrupper och prenumerationer. hello användargränssnittet gör det enkelt tooselect hello princip som du vill tooassign och ange parametervärden för denna princip toocustomize hello principinställningar. 

tooassign en princip hello-portalen hello principdefinitionen måste redan finnas i din prenumeration. Din prenumeration har flera inbyggda principdefinitioner som är klara för du tooassign tooresource grupper eller prenumerationer. Du kan se dessa inbyggda principer och eventuella anpassade principer som du har definierat när du använder hello portal tooassign principer. En introduktion toopolicies och hur toodefine anpassad princip finns [resurs uppgifter](resource-manager-policy.md).

Principer ärvs av alla underordnade resurser. Så om en princip är tillämpade tooa resursgrupp, är det tillämpliga tooall hello resurser i resursgruppen. I den här artikeln hello termen **omfång** refererar toohello resursgrupp eller prenumeration som är tilldelad hello princip. 

Principer utvärderas när du skapar och uppdaterar resurser (PLACERA och korrigering operations).

## <a name="assign-a-policy"></a>Tilldela en princip

1. tooassign princip-tooeither en resursgrupp eller prenumeration, Välj den resursgrupp eller prenumeration. Markera hello inställningar **principer**.

   ![Välj principer](./media/resource-manager-policy-portal/select-policies.png)

2. Välj toocreate en principtilldelning för detta scope **Lägg till tilldelning**.

   ![Lägg till tilldelning](./media/resource-manager-policy-portal/add-assignment.png)

3. Välj hello-princip som du vill tooassign. Även om du inte har lagt till någon princip definitioner tooyour prenumeration kan se du hello inbyggda principer som är tillgängliga för tilldelning. De här inbyggda täcker många vanliga scenarier.

   ![Välj definition](./media/resource-manager-policy-portal/select-definition.png)

4. När du har valt en princip, visas en beskrivning av principen för hello och eventuella parametrar för grupprincipobjekt. Hello följande bild visar exempelvis hello **tillåtna platser** som krävs för hello-princip som hindrar hello tillgängliga platser.

   ![Visa parametrar](./media/resource-manager-policy-portal/show-parameters.png)

5. Välj hello värden toospecify för hello principparametrar (t.ex hello platser som kan användas för distribution) via hello användargränssnitt.

   ![Välj parametervärden](./media/resource-manager-policy-portal/select-parameters.png)

6. Ange värden för hello andra parametrar. hello scope tilldelas automatiskt baserat på hello bladet som du valde när du startar hello tilldelning av principer. Välj **OK** när du är klar.

   ![Definiera parametrar](./media/resource-manager-policy-portal/define-parameters.png)

  Du har tilldelat hello princip toohello angivna omfattningen.

## <a name="view-policy-assignments"></a>Visa principtilldelningar

Efter att en princip syns i hello listan med principer för detta omfång. Hej **information** fliken visas en sammanfattning av hello tilldelning av principer.

![Visa information](./media/resource-manager-policy-portal/show-details.png)

Hej **tilldelningsregel** visar hello JSON för hello principdefinitionen.

![Visa tilldelningsregel](./media/resource-manager-policy-portal/show-assignment-rule.png)

## <a name="change-an-existing-policy-assignment"></a>Ändra en befintlig principtilldelning

toochange en princip, Välj **Redigera tilldelning** eller **ta bort**

![Redigera eller ta bort tilldelning](./media/resource-manager-policy-portal/edit-delete-policy.png)

## <a name="assign-custom-policies"></a>Tilldela anpassade principer

Om du har definierat anpassade principer i din prenumeration, kan dessa principer tilldelas hello-portalen. Dessa principer inleds med **[Custom]**

![anpassade principer](./media/resource-manager-policy-portal/show-custom-policy.png)

## <a name="next-steps"></a>Nästa steg
* toolearn hello JSON-syntax för att definiera principer, se [resurs uppgifter](resource-manager-policy.md).
* Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).
* hello princip schema har publicerats på [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json). 

