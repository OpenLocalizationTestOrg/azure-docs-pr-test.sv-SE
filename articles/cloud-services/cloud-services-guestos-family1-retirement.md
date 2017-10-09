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
# <a name="guest-os-family-1-retirement-notice"></a>Gäst-OS-familjen 1 pensionering meddelande
Hej tillbakadragningen av OS-familjen 1 först har meddelat den 1 juni 2013.

**2 september 2014** hello Azure gästoperativsystemet (gäst-OS) familj 1.x, som är baserat på hello Windows Server 2008-operativsystem, officiellt har dragits tillbaka. Alla försök toodeploy nya tjänster eller uppgradera befintliga tjänster använder familj 1 misslyckas med ett felmeddelande som talar om att hello gäst-OS-familjen 1 har tagits bort.

**3 november 2014** utökat stöd för gäst-OS-familjen 1 avslutades och fullständigt har återkallats. Alla tjänster fortfarande på familj 1 påverkas. Vi kan stoppa tjänsterna när som helst. Det är inte säkert dina tjänster fortsätter toorun om du manuellt uppgradera dem själv.

Om du har fler frågor finns hello [Cloud Services forum](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) eller [kontakta Azure-supporten](https://azure.microsoft.com/support/options/).

## <a name="are-you-affected"></a>Påverkas du?
Dina molntjänster påverkas om någon av hello följande gäller:

1. Du har värdet ”osFamily =” 1 ”explicit i hello ServiceConfiguration.cscfg filen för Molntjänsten.
2. Du har inte ett värde för osFamily explicit i hello ServiceConfiguration.cscfg filen för Molntjänsten. För närvarande används hello hello standardvärdet ”1” i det här fallet.
3. hello Azure-portalen visar gästoperativsystemet family värdet som ”Windows Server 2008”.

toofind vilka av dina molntjänster som körs som OS-familjen, kan du köra hello följande skript i Azure PowerShell, även om du måste [konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs) första. Mer information om hello skript finns [Azure gäst OS-familjen 1 slutet av livslängd: juni 2014](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx).

```Powershell
foreach($subscription in Get-AzureSubscription) {
    Select-AzureSubscription -SubscriptionName $subscription.SubscriptionName

    $deployments=get-azureService | get-azureDeployment -ErrorAction Ignore | where {$_.SdkVersion -NE ""}

    $deployments | ft @{Name="SubscriptionName";Expression={$subscription.SubscriptionName}}, ServiceName, SdkVersion, Slot, @{Name="osFamily";Expression={(select-xml -content $_.configuration -xpath "/ns:ServiceConfiguration/@osFamily" -namespace $namespace).node.value }}, osVersion, Status, URL
}
```

Dina molntjänster som påverkas av OS-familjen 1 pensionering om hello osFamily-kolumn i utdata för hello-skriptet är tom eller innehåller en ”1”.

## <a name="recommendations-if-you-are-affected"></a>Rekommendationer om du påverkas
Vi rekommenderar att du migrerar din molntjänst roller tooone hello stöds gäst OS-familjer:

**Gästoperativsystem family 4.x** -Windows Server 2012 R2 *(rekommenderas)*

1. Se till att ditt program med .NET framework 4.0, 4.5 och 4.5.1 SDK 2.1 eller senare.
2. Ange hello osFamily-attributet för ”4” i hello ServiceConfiguration.cscfg fil och distribuera din tjänst i molnet.

**Gästoperativsystem family 3.x** -Windows Server 2012

1. Se till att ditt program med .NET framework 4.0 eller 4.5 SDK 1,8 eller senare.
2. Ange hello osFamily-attributet för ”3” i hello ServiceConfiguration.cscfg fil och distribuera din tjänst i molnet.

**Gästoperativsystem family 2.x** -Windows Server 2008 R2

1. Kontrollera att ditt program använder SDK 1.3 och senare med .NET framework 3.5 eller 4.0.
2. Ange hello osFamily-attributet för ”2” i hello ServiceConfiguration.cscfg fil och distribuera din tjänst i molnet.

## <a name="extended-support-for-guest-os-family-1-ended-nov-3-2014"></a>Utökad Support för gäst-OS-familjen 1 avslutades 3 Nov 2014
Molntjänster på gäst-OS-familjen 1 stöds inte längre. Migrering av familj 1 så snart som möjligt tooavoid tjänst avbrott.  

## <a name="next-steps"></a>Nästa steg
Granska hello senaste [Gästoperativsystem släpper](cloud-services-guestos-update-matrix.md).
