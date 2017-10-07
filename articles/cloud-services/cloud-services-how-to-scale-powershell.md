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
# <a name="how-tooscale-a-cloud-service-in-powershell"></a><span data-ttu-id="ac991-103">Hur tooscale ett moln-tjänsten i PowerShell</span><span class="sxs-lookup"><span data-stu-id="ac991-103">How tooscale a cloud service in PowerShell</span></span>

<span data-ttu-id="ac991-104">Du kan använda Windows PowerShell tooscale en webbroll eller arbetsrollen in eller ut genom att lägga till eller ta bort instanser.</span><span class="sxs-lookup"><span data-stu-id="ac991-104">You can use Windows PowerShell tooscale a web role or worker role in or out by adding or removing instances.</span></span>  

## <a name="log-in-tooazure"></a><span data-ttu-id="ac991-105">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="ac991-105">Log in tooAzure</span></span>

<span data-ttu-id="ac991-106">Du måste logga in innan du kan utföra några åtgärder på din prenumeration med hjälp av PowerShell:</span><span class="sxs-lookup"><span data-stu-id="ac991-106">Before you can perform any operations on your subscription through PowerShell, you must log in:</span></span>

```powershell
Add-AzureAccount
```

<span data-ttu-id="ac991-107">Om du har flera prenumerationer som är kopplade till ditt konto kan behöva du toochange hello aktuell prenumeration beroende på där Molntjänsten finns.</span><span class="sxs-lookup"><span data-stu-id="ac991-107">If you have multiple subscriptions associated with your account, you may need toochange hello current subscription depending on where your cloud service resides.</span></span> <span data-ttu-id="ac991-108">toocheck hello aktuell prenumeration, kör:</span><span class="sxs-lookup"><span data-stu-id="ac991-108">toocheck hello current subscription, run:</span></span>

```powershell
Get-AzureSubscription -Current
```

<span data-ttu-id="ac991-109">Om du behöver toochange hello aktuell prenumeration, kör:</span><span class="sxs-lookup"><span data-stu-id="ac991-109">If you need toochange hello current subscription, run:</span></span>

```powershell
Set-AzureSubscription -SubscriptionId <subscription_id>
```

## <a name="check-hello-current-instance-count-for-your-role"></a><span data-ttu-id="ac991-110">Kontrollera hello aktuella instanser för din roll</span><span class="sxs-lookup"><span data-stu-id="ac991-110">Check hello current instance count for your role</span></span>

<span data-ttu-id="ac991-111">toocheck hello aktuell status för din roll, kör:</span><span class="sxs-lookup"><span data-stu-id="ac991-111">toocheck hello current state of your role, run:</span></span>

```powershell
Get-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>'
```

<span data-ttu-id="ac991-112">Du bör få tillbaka information om hello rollen, inklusive dess aktuella OS-version och instans antal.</span><span class="sxs-lookup"><span data-stu-id="ac991-112">You should get back information about hello role, including its current OS version and instance count.</span></span> <span data-ttu-id="ac991-113">I det här fallet har hello rollen en enda instans.</span><span class="sxs-lookup"><span data-stu-id="ac991-113">In this case, hello role has a single instance.</span></span>

![Information om hello roll](./media/cloud-services-how-to-scale-powershell/get-azure-role.png)

## <a name="scale-out-hello-role-by-adding-more-instances"></a><span data-ttu-id="ac991-115">Skala ut hello roll genom att lägga till flera instanser</span><span class="sxs-lookup"><span data-stu-id="ac991-115">Scale out hello role by adding more instances</span></span>

<span data-ttu-id="ac991-116">tooscale ut din roll pass hello önskat antal instanser som hello **antal** parametern toohello **Set AzureRole** cmdlet:</span><span class="sxs-lookup"><span data-stu-id="ac991-116">tooscale out your role, pass hello desired number of instances as hello **Count** parameter toohello **Set-AzureRole** cmdlet:</span></span>

```powershell
Set-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>' -Slot <target_slot> -Count <desired_instances>
```

<span data-ttu-id="ac991-117">hello cmdlet block tillfälligt medan hello nya instanser etablerad och igång.</span><span class="sxs-lookup"><span data-stu-id="ac991-117">hello cmdlet blocks momentarily while hello new instances are provisioned and started.</span></span> <span data-ttu-id="ac991-118">Under denna tid om du öppnar en ny PowerShell-fönster och anropet **Get-AzureRole** enligt tidigare, visas hello nya mål-instanser.</span><span class="sxs-lookup"><span data-stu-id="ac991-118">During this time, if you open a new PowerShell window and call **Get-AzureRole** as shown earlier, you will see hello new target instance count.</span></span> <span data-ttu-id="ac991-119">Och om du inspektera hello Rollstatus i hello portal bör du se hello ny instans skulle startas:</span><span class="sxs-lookup"><span data-stu-id="ac991-119">And if you inspect hello role status in hello portal, you should see hello new instance starting up:</span></span>

![VM-instans med början i portalen](./media/cloud-services-how-to-scale-powershell/role-instance-starting.png)

<span data-ttu-id="ac991-121">När hello nya instanser har startat kan returnera hello-cmdlet har:</span><span class="sxs-lookup"><span data-stu-id="ac991-121">Once hello new instances have started, hello cmdlet will return successfully:</span></span>

![Rollen instans öka lyckades](./media/cloud-services-how-to-scale-powershell/set-azure-role-success.png)

## <a name="scale-in-hello-role-by-removing-instances"></a><span data-ttu-id="ac991-123">Skala hello roll genom att ta bort instanser</span><span class="sxs-lookup"><span data-stu-id="ac991-123">Scale in hello role by removing instances</span></span>

<span data-ttu-id="ac991-124">Du kan skala i en roll genom att ta bort instanser i hello samma sätt.</span><span class="sxs-lookup"><span data-stu-id="ac991-124">You can scale in a role by removing instances in hello same way.</span></span> <span data-ttu-id="ac991-125">Ange hello **antal** parameter på **Set AzureRole** toohello antalet instanser som du vill toohave när hello skalan i åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="ac991-125">Set hello **Count** parameter on **Set-AzureRole** toohello number of instances you want toohave after hello scale in operation is complete.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ac991-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ac991-126">Next steps</span></span>

<span data-ttu-id="ac991-127">Det är inte möjligt tooconfigure Autoskala för molntjänster från PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ac991-127">It is not possible tooconfigure auto-scale for cloud services from PowerShell.</span></span> <span data-ttu-id="ac991-128">toodo som finns i [hur tooauto skala en tjänst i molnet](cloud-services-how-to-scale-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ac991-128">toodo that, see [How tooauto scale a cloud service](cloud-services-how-to-scale-portal.md).</span></span>
