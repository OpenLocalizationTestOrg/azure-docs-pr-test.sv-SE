---
title: "Skala en Azure-molntjänst i Windows PowerShell | Microsoft Docs"
description: "(klassisk) Lär dig hur du använder PowerShell för att skala en webbroll och en arbetsroll in eller ut i Azure."
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
ms.openlocfilehash: a7ae8ff202d403dff19b8c9a6a09492235db27ac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-scale-a-cloud-service-in-powershell"></a><span data-ttu-id="da97d-103">Så här skalar en tjänst i molnet i PowerShell</span><span class="sxs-lookup"><span data-stu-id="da97d-103">How to scale a cloud service in PowerShell</span></span>

<span data-ttu-id="da97d-104">Du kan använda Windows PowerShell för att skala en webbroll eller arbetsrollen in eller ut genom att lägga till eller ta bort instanser.</span><span class="sxs-lookup"><span data-stu-id="da97d-104">You can use Windows PowerShell to scale a web role or worker role in or out by adding or removing instances.</span></span>  

## <a name="log-in-to-azure"></a><span data-ttu-id="da97d-105">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="da97d-105">Log in to Azure</span></span>

<span data-ttu-id="da97d-106">Du måste logga in innan du kan utföra några åtgärder på din prenumeration med hjälp av PowerShell:</span><span class="sxs-lookup"><span data-stu-id="da97d-106">Before you can perform any operations on your subscription through PowerShell, you must log in:</span></span>

```powershell
Add-AzureAccount
```

<span data-ttu-id="da97d-107">Om du har flera prenumerationer som är kopplade till ditt konto kan behöva du ändra den aktuella prenumerationen beroende på där Molntjänsten finns.</span><span class="sxs-lookup"><span data-stu-id="da97d-107">If you have multiple subscriptions associated with your account, you may need to change the current subscription depending on where your cloud service resides.</span></span> <span data-ttu-id="da97d-108">Om du vill kontrollera den aktuella prenumerationen, kör du:</span><span class="sxs-lookup"><span data-stu-id="da97d-108">To check the current subscription, run:</span></span>

```powershell
Get-AzureSubscription -Current
```

<span data-ttu-id="da97d-109">Om du behöver ändra den aktuella prenumerationen kör:</span><span class="sxs-lookup"><span data-stu-id="da97d-109">If you need to change the current subscription, run:</span></span>

```powershell
Set-AzureSubscription -SubscriptionId <subscription_id>
```

## <a name="check-the-current-instance-count-for-your-role"></a><span data-ttu-id="da97d-110">Kontrollera den aktuella instanser för din roll</span><span class="sxs-lookup"><span data-stu-id="da97d-110">Check the current instance count for your role</span></span>

<span data-ttu-id="da97d-111">Om du vill kontrollera det aktuella tillståndet för din roll, kör du:</span><span class="sxs-lookup"><span data-stu-id="da97d-111">To check the current state of your role, run:</span></span>

```powershell
Get-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>'
```

<span data-ttu-id="da97d-112">Du bör få tillbaka information om rollen, inklusive dess aktuella OS-version och instans antal.</span><span class="sxs-lookup"><span data-stu-id="da97d-112">You should get back information about the role, including its current OS version and instance count.</span></span> <span data-ttu-id="da97d-113">I det här fallet har rollen en enda instans.</span><span class="sxs-lookup"><span data-stu-id="da97d-113">In this case, the role has a single instance.</span></span>

![Information om rollen](./media/cloud-services-how-to-scale-powershell/get-azure-role.png)

## <a name="scale-out-the-role-by-adding-more-instances"></a><span data-ttu-id="da97d-115">Skala ut rollen genom att lägga till flera instanser</span><span class="sxs-lookup"><span data-stu-id="da97d-115">Scale out the role by adding more instances</span></span>

<span data-ttu-id="da97d-116">Om du vill skala upp din roll skicka antalet instanser som den **antal** parametern till den **Set AzureRole** cmdlet:</span><span class="sxs-lookup"><span data-stu-id="da97d-116">To scale out your role, pass the desired number of instances as the **Count** parameter to the **Set-AzureRole** cmdlet:</span></span>

```powershell
Set-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>' -Slot <target_slot> -Count <desired_instances>
```

<span data-ttu-id="da97d-117">Cmdlet-block tillfälligt när nya instanser etablerad och igång.</span><span class="sxs-lookup"><span data-stu-id="da97d-117">The cmdlet blocks momentarily while the new instances are provisioned and started.</span></span> <span data-ttu-id="da97d-118">Under denna tid om du öppnar en ny PowerShell-fönster och anropet **Get-AzureRole** enligt tidigare, visas antal för nya mål-instans.</span><span class="sxs-lookup"><span data-stu-id="da97d-118">During this time, if you open a new PowerShell window and call **Get-AzureRole** as shown earlier, you will see the new target instance count.</span></span> <span data-ttu-id="da97d-119">Och om du inspektera Rollstatus i portalen, bör du se den nya instansen skulle startas:</span><span class="sxs-lookup"><span data-stu-id="da97d-119">And if you inspect the role status in the portal, you should see the new instance starting up:</span></span>

![VM-instans med början i portalen](./media/cloud-services-how-to-scale-powershell/role-instance-starting.png)

<span data-ttu-id="da97d-121">När nya instanser har startat, returnerar cmdleten har:</span><span class="sxs-lookup"><span data-stu-id="da97d-121">Once the new instances have started, the cmdlet will return successfully:</span></span>

![Rollen instans öka lyckades](./media/cloud-services-how-to-scale-powershell/set-azure-role-success.png)

## <a name="scale-in-the-role-by-removing-instances"></a><span data-ttu-id="da97d-123">Skala i rollen genom att ta bort instanser</span><span class="sxs-lookup"><span data-stu-id="da97d-123">Scale in the role by removing instances</span></span>

<span data-ttu-id="da97d-124">Du kan skala i en roll genom att ta bort instanser på samma sätt.</span><span class="sxs-lookup"><span data-stu-id="da97d-124">You can scale in a role by removing instances in the same way.</span></span> <span data-ttu-id="da97d-125">Ange den **antal** parameter på **Set AzureRole** till antal instanser som du vill ha när skalan i åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="da97d-125">Set the **Count** parameter on **Set-AzureRole** to the number of instances you want to have after the scale in operation is complete.</span></span>

## <a name="next-steps"></a><span data-ttu-id="da97d-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="da97d-126">Next steps</span></span>

<span data-ttu-id="da97d-127">Det går inte att konfigurera Autoskala för molntjänster från PowerShell.</span><span class="sxs-lookup"><span data-stu-id="da97d-127">It is not possible to configure auto-scale for cloud services from PowerShell.</span></span> <span data-ttu-id="da97d-128">För att göra det, se [så att automatiskt skala en tjänst i molnet](cloud-services-how-to-scale-portal.md).</span><span class="sxs-lookup"><span data-stu-id="da97d-128">To do that, see [How to auto scale a cloud service](cloud-services-how-to-scale-portal.md).</span></span>
