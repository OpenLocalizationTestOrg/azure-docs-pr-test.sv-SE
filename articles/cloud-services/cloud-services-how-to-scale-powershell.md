---
title: "aaaScale en Azure-molntjänst i Windows PowerShell | Microsoft Docs"
description: "(klassisk) Lär dig hur toouse PowerShell tooscale en webbroll eller arbetsrollen in eller ut i Azure."
services: cloud-services
documentationcenter: 
author: mmccrory
manager: timlt
editor: 
ms.assetid: ee37dd8c-6714-4c61-adb8-03d6bbf76c9a
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: mmccrory
ms.openlocfilehash: cfac6660e84f8ae24e4e9bdd5bf2016fb9cd7045
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooscale-a-cloud-service-in-powershell"></a>Hur tooscale ett moln-tjänsten i PowerShell

Du kan använda Windows PowerShell tooscale en webbroll eller arbetsrollen in eller ut genom att lägga till eller ta bort instanser.  

## <a name="log-in-tooazure"></a>Logga in tooAzure

Du måste logga in innan du kan utföra några åtgärder på din prenumeration med hjälp av PowerShell:

```powershell
Add-AzureAccount
```

Om du har flera prenumerationer som är kopplade till ditt konto kan behöva du toochange hello aktuell prenumeration beroende på där Molntjänsten finns. toocheck hello aktuell prenumeration, kör:

```powershell
Get-AzureSubscription -Current
```

Om du behöver toochange hello aktuell prenumeration, kör:

```powershell
Set-AzureSubscription -SubscriptionId <subscription_id>
```

## <a name="check-hello-current-instance-count-for-your-role"></a>Kontrollera hello aktuella instanser för din roll

toocheck hello aktuell status för din roll, kör:

```powershell
Get-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>'
```

Du bör få tillbaka information om hello rollen, inklusive dess aktuella OS-version och instans antal. I det här fallet har hello rollen en enda instans.

![Information om hello roll](./media/cloud-services-how-to-scale-powershell/get-azure-role.png)

## <a name="scale-out-hello-role-by-adding-more-instances"></a>Skala ut hello roll genom att lägga till flera instanser

tooscale ut din roll pass hello önskat antal instanser som hello **antal** parametern toohello **Set AzureRole** cmdlet:

```powershell
Set-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>' -Slot <target_slot> -Count <desired_instances>
```

hello cmdlet block tillfälligt medan hello nya instanser etablerad och igång. Under denna tid om du öppnar en ny PowerShell-fönster och anropet **Get-AzureRole** enligt tidigare, visas hello nya mål-instanser. Och om du inspektera hello Rollstatus i hello portal bör du se hello ny instans skulle startas:

![VM-instans med början i portalen](./media/cloud-services-how-to-scale-powershell/role-instance-starting.png)

När hello nya instanser har startat kan returnera hello-cmdlet har:

![Rollen instans öka lyckades](./media/cloud-services-how-to-scale-powershell/set-azure-role-success.png)

## <a name="scale-in-hello-role-by-removing-instances"></a>Skala hello roll genom att ta bort instanser

Du kan skala i en roll genom att ta bort instanser i hello samma sätt. Ange hello **antal** parameter på **Set AzureRole** toohello antalet instanser som du vill toohave när hello skalan i åtgärden har slutförts.

## <a name="next-steps"></a>Nästa steg

Det är inte möjligt tooconfigure Autoskala för molntjänster från PowerShell. toodo som finns i [hur tooauto skala en tjänst i molnet](cloud-services-how-to-scale-portal.md).
