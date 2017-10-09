---
title: "Lägg märke till aaaGuest OS-familjen 1 pensionering | Microsoft Docs"
description: "Innehåller information om när hello Azure gäst OS-familjen 1 pensionering inträffade och hur toodetermine påverkas"
services: cloud-services
documentationcenter: na
author: raiye
manager: timlt
editor: 
ms.assetid: 37b422e9-0713-4a81-a942-f553ef478064
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 5/21/2017
ms.author: raiye
ms.openlocfilehash: fa8b904c6560dbbe4982c301f818c7a5cbc4eacb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="guest-os-family-1-retirement-notice"></a><span data-ttu-id="71d72-103">Gäst-OS-familjen 1 pensionering meddelande</span><span class="sxs-lookup"><span data-stu-id="71d72-103">Guest OS Family 1 retirement notice</span></span>
<span data-ttu-id="71d72-104">Hej tillbakadragningen av OS-familjen 1 först har meddelat den 1 juni 2013.</span><span class="sxs-lookup"><span data-stu-id="71d72-104">hello retirement of OS Family 1 was first announced on June 1, 2013.</span></span>

<span data-ttu-id="71d72-105">**2 september 2014** hello Azure gästoperativsystemet (gäst-OS) familj 1.x, som är baserat på hello Windows Server 2008-operativsystem, officiellt har dragits tillbaka.</span><span class="sxs-lookup"><span data-stu-id="71d72-105">**Sept 2, 2014** hello Azure Guest operating system (Guest OS) Family 1.x, which is based on hello Windows Server 2008 operating system, was officially retired.</span></span> <span data-ttu-id="71d72-106">Alla försök toodeploy nya tjänster eller uppgradera befintliga tjänster använder familj 1 misslyckas med ett felmeddelande som talar om att hello gäst-OS-familjen 1 har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="71d72-106">All attempts toodeploy new services or upgrade existing services using Family 1 will fail with an error message informing you that hello Guest OS Family 1 has been retired.</span></span>

<span data-ttu-id="71d72-107">**3 november 2014** utökat stöd för gäst-OS-familjen 1 avslutades och fullständigt har återkallats.</span><span class="sxs-lookup"><span data-stu-id="71d72-107">**November 3, 2014** Extended support for Guest OS Family 1 ended and it is fully retired.</span></span> <span data-ttu-id="71d72-108">Alla tjänster fortfarande på familj 1 påverkas.</span><span class="sxs-lookup"><span data-stu-id="71d72-108">All services still on Family 1 will be impacted.</span></span> <span data-ttu-id="71d72-109">Vi kan stoppa tjänsterna när som helst.</span><span class="sxs-lookup"><span data-stu-id="71d72-109">We may stop those services at any time.</span></span> <span data-ttu-id="71d72-110">Det är inte säkert dina tjänster fortsätter toorun om du manuellt uppgradera dem själv.</span><span class="sxs-lookup"><span data-stu-id="71d72-110">There is no guarantee your services will continue toorun unless you manually upgrade them yourself.</span></span>

<span data-ttu-id="71d72-111">Om du har fler frågor finns hello [Cloud Services forum](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) eller [kontakta Azure-supporten](https://azure.microsoft.com/support/options/).</span><span class="sxs-lookup"><span data-stu-id="71d72-111">If you have additional questions, visit hello [Cloud Services Forums](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) or [contact Azure support](https://azure.microsoft.com/support/options/).</span></span>

## <a name="are-you-affected"></a><span data-ttu-id="71d72-112">Påverkas du?</span><span class="sxs-lookup"><span data-stu-id="71d72-112">Are you affected?</span></span>
<span data-ttu-id="71d72-113">Dina molntjänster påverkas om någon av hello följande gäller:</span><span class="sxs-lookup"><span data-stu-id="71d72-113">Your Cloud Services are affected if any one of hello following applies:</span></span>

1. <span data-ttu-id="71d72-114">Du har värdet ”osFamily =” 1 ”explicit i hello ServiceConfiguration.cscfg filen för Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="71d72-114">You have a value of "osFamily = "1" explicitly specified in hello ServiceConfiguration.cscfg file for your Cloud Service.</span></span>
2. <span data-ttu-id="71d72-115">Du har inte ett värde för osFamily explicit i hello ServiceConfiguration.cscfg filen för Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="71d72-115">You do not have a value for osFamily explicitly specified in hello ServiceConfiguration.cscfg file for your Cloud Service.</span></span> <span data-ttu-id="71d72-116">För närvarande används hello hello standardvärdet ”1” i det här fallet.</span><span class="sxs-lookup"><span data-stu-id="71d72-116">Currently, hello system uses hello default value of "1" in this case.</span></span>
3. <span data-ttu-id="71d72-117">hello Azure-portalen visar gästoperativsystemet family värdet som ”Windows Server 2008”.</span><span class="sxs-lookup"><span data-stu-id="71d72-117">hello Azure portal lists your Guest Operating System family value as "Windows Server 2008".</span></span>

<span data-ttu-id="71d72-118">toofind vilka av dina molntjänster som körs som OS-familjen, kan du köra hello följande skript i Azure PowerShell, även om du måste [konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs) första.</span><span class="sxs-lookup"><span data-stu-id="71d72-118">toofind which of your cloud services are running which OS Family, you can run hello following script in Azure PowerShell, though you must [set up Azure PowerShell](/powershell/azureps-cmdlets-docs) first.</span></span> <span data-ttu-id="71d72-119">Mer information om hello skript finns [Azure gäst OS-familjen 1 slutet av livslängd: juni 2014](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx).</span><span class="sxs-lookup"><span data-stu-id="71d72-119">For more information on hello script, see [Azure Guest OS Family 1 End of Life: June 2014](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx).</span></span>

```Powershell
foreach($subscription in Get-AzureSubscription) {
    Select-AzureSubscription -SubscriptionName $subscription.SubscriptionName

    $deployments=get-azureService | get-azureDeployment -ErrorAction Ignore | where {$_.SdkVersion -NE ""}

    $deployments | ft @{Name="SubscriptionName";Expression={$subscription.SubscriptionName}}, ServiceName, SdkVersion, Slot, @{Name="osFamily";Expression={(select-xml -content $_.configuration -xpath "/ns:ServiceConfiguration/@osFamily" -namespace $namespace).node.value }}, osVersion, Status, URL
}
```

<span data-ttu-id="71d72-120">Dina molntjänster som påverkas av OS-familjen 1 pensionering om hello osFamily-kolumn i utdata för hello-skriptet är tom eller innehåller en ”1”.</span><span class="sxs-lookup"><span data-stu-id="71d72-120">Your cloud services will be impacted by OS Family 1 retirement if hello osFamily column in hello script output is empty or contains a "1".</span></span>

## <a name="recommendations-if-you-are-affected"></a><span data-ttu-id="71d72-121">Rekommendationer om du påverkas</span><span class="sxs-lookup"><span data-stu-id="71d72-121">Recommendations if you are affected</span></span>
<span data-ttu-id="71d72-122">Vi rekommenderar att du migrerar din molntjänst roller tooone hello stöds gäst OS-familjer:</span><span class="sxs-lookup"><span data-stu-id="71d72-122">We recommend you migrate your Cloud Service roles tooone of hello supported Guest OS Families:</span></span>

<span data-ttu-id="71d72-123">**Gästoperativsystem family 4.x** -Windows Server 2012 R2 *(rekommenderas)*</span><span class="sxs-lookup"><span data-stu-id="71d72-123">**Guest OS family 4.x** - Windows Server 2012 R2 *(recommended)*</span></span>

1. <span data-ttu-id="71d72-124">Se till att ditt program med .NET framework 4.0, 4.5 och 4.5.1 SDK 2.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="71d72-124">Ensure that your application is using SDK 2.1 or later with .NET framework 4.0, 4.5 or 4.5.1.</span></span>
2. <span data-ttu-id="71d72-125">Ange hello osFamily-attributet för ”4” i hello ServiceConfiguration.cscfg fil och distribuera din tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="71d72-125">Set hello osFamily attribute too“4” in hello ServiceConfiguration.cscfg file, and redeploy your cloud service.</span></span>

<span data-ttu-id="71d72-126">**Gästoperativsystem family 3.x** -Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="71d72-126">**Guest OS family 3.x** - Windows Server 2012</span></span>

1. <span data-ttu-id="71d72-127">Se till att ditt program med .NET framework 4.0 eller 4.5 SDK 1,8 eller senare.</span><span class="sxs-lookup"><span data-stu-id="71d72-127">Ensure that your application is using SDK 1.8 or later with .NET framework 4.0 or 4.5.</span></span>
2. <span data-ttu-id="71d72-128">Ange hello osFamily-attributet för ”3” i hello ServiceConfiguration.cscfg fil och distribuera din tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="71d72-128">Set hello osFamily attribute too“3” in hello ServiceConfiguration.cscfg file, and redeploy your cloud service.</span></span>

<span data-ttu-id="71d72-129">**Gästoperativsystem family 2.x** -Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="71d72-129">**Guest OS family 2.x** - Windows Server 2008 R2</span></span>

1. <span data-ttu-id="71d72-130">Kontrollera att ditt program använder SDK 1.3 och senare med .NET framework 3.5 eller 4.0.</span><span class="sxs-lookup"><span data-stu-id="71d72-130">Ensure that your application is using SDK 1.3 and above with .NET framework 3.5 or 4.0.</span></span>
2. <span data-ttu-id="71d72-131">Ange hello osFamily-attributet för ”2” i hello ServiceConfiguration.cscfg fil och distribuera din tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="71d72-131">Set hello osFamily attribute too"2" in hello ServiceConfiguration.cscfg file, and redeploy your cloud service.</span></span>

## <a name="extended-support-for-guest-os-family-1-ended-nov-3-2014"></a><span data-ttu-id="71d72-132">Utökad Support för gäst-OS-familjen 1 avslutades 3 Nov 2014</span><span class="sxs-lookup"><span data-stu-id="71d72-132">Extended Support for Guest OS Family 1 ended Nov 3, 2014</span></span>
<span data-ttu-id="71d72-133">Molntjänster på gäst-OS-familjen 1 stöds inte längre.</span><span class="sxs-lookup"><span data-stu-id="71d72-133">Cloud services on Guest OS family 1 are no longer supported.</span></span> <span data-ttu-id="71d72-134">Migrering av familj 1 så snart som möjligt tooavoid tjänst avbrott.</span><span class="sxs-lookup"><span data-stu-id="71d72-134">Migrate off family 1 as soon as possible tooavoid service disruption.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="71d72-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="71d72-135">Next steps</span></span>
<span data-ttu-id="71d72-136">Granska hello senaste [Gästoperativsystem släpper](cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="71d72-136">Review hello latest [Guest OS releases](cloud-services-guestos-update-matrix.md).</span></span>
