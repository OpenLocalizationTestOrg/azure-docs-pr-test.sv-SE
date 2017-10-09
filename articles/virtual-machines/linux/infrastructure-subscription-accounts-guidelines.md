---
title: "aaaSubscription och ta hänsyn till Linux virtuella datorer i Azure | Microsoft Docs"
description: "Läs mer om hello viktiga design och implementeringslösning riktlinjer för prenumerationer och konton i Azure."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 19343826-7eef-42a1-98be-4ec65b0f377a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9025a40783c008310ebd0f674deb4a9001ae974a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-subscription-and-accounts-guidelines-for-linux-vms"></a>Riktlinjer för Azure-prenumeration och konton för virtuella Linux-datorer

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

Den här artikeln fokuserar på att förstå hur tooapproach prenumeration och konto för hantering av din miljö och användarbas växer.

## <a name="implementation-guidelines-for-subscriptions-and-accounts"></a>Implementeringsriktlinjer för prenumerationer och konton
Beslut:

* Vilken uppsättning av prenumerationer och konton behöver du toohost din IT-arbetsbelastning eller infrastrukturen?
* Hur toobreak ned hello hierarkin toofit din organisation?

Aktiviteter:

* Definiera logiska organisationens hierarki som du vill att toomanage från en prenumerationsnivå.
* toomatch den här logiska hierarkin hello konton som krävs och prenumerationer under varje konto.
* Skapa hello uppsättning prenumerationer och konton med hjälp av din namngivningskonvention.

## <a name="subscriptions-and-accounts"></a>Prenumeration och konton
toowork med Azure, behöver du en eller flera Azure-prenumerationer. Resurser som virtuella datorer (VM) eller virtuella nätverk finns i dessa prenumerationer.

* Enterprise-kunder har vanligtvis ett Enterprise-registreringen, vilket är hello översta resurs i hello hierarki och associerade tooone eller fler konton.
* Hello översta resursen är för konsumenter och kunder utan en Enterprise-registrering hello-konto.
* Prenumerationer är associerade tooaccounts och det kan finnas en eller flera prenumerationer per konto. Azure poster faktureringsinformation på hello prenumerationsnivå.

På grund av toohello högst två hierarkinivåer i hello konto eller prenumeration på relationen är det viktigt tooalign hello namngivningskonvention konton och prenumerationer toohello fakturering behov. Till exempel om ett globalt företag använder Azure, kan de välja toohave ett konto per region och har prenumerationer hanteras på hello regionsnivån:

![](media/virtual-machines-common-infrastructure-service-guidelines/sub01.png)

Du kan exempelvis använda hello följande struktur:

![](media/virtual-machines-common-infrastructure-service-guidelines/sub02.png)

Om en region beslutar toohave mer än en prenumeration associerade tooa viss grupp, hello namngivningskonvention bör omfatta en sätt tooencode hello extra data på hello konto eller hello prenumerationsnamn. Den här organisationen kan massaging fakturering toogenerate hello nya nivåer i hierarkin under fakturering rapporter:

![](media/virtual-machines-common-infrastructure-service-guidelines/sub03.png)

hello organisation kan se ut som följande exempel hello:

![](media/virtual-machines-common-infrastructure-service-guidelines/sub04.png)

Vi ger detaljerad fakturering via en hämtningsbar fil för ett enskilt konto eller för alla konton i ett enterprise-avtal.

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

