---
title: "aaaAzure moln Shell (förhandsgranskning) översikt | Microsoft Docs"
description: "Översikt över hello Azure Cloud-gränssnittet."
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: juluk
ms.openlocfilehash: 45c6c85b167a90947a333f44a9186e2c01b4fa7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-azure-cloud-shell-preview"></a>Översikt över Azure-molnet Shell (förhandsgranskning)
Azure Cloud-gränssnittet är en interaktiv, webbläsare-tillgängliga shell för att hantera Azure-resurser.

![](media/overview-pic.png)

## <a name="features"></a>Funktioner
### <a name="browser-based-shell-experience"></a>Shell-webbläsarbaserad upplevelse
Moln Shell aktiverar åtkomst tooa webbläsarbaserad kommandoradsverktyget erfarenheterna med Azure-hanteringsuppgifter i åtanke. Utnyttja molnet Shell toowork obunden från en lokal dator på ett sätt som endast hello molnet kan ge.

### <a name="pre-configured-azure-workstation"></a>Förkonfigurerade Azure arbetsstation
Molnet Shell finns förinstallerat med populära kommandoradsverktyg och språkstöd så att du kan arbeta snabbare.

[Visa hello fullständig verktygsuppsättning lista för Azure Cloud Shell här.](features.md#tools)

### <a name="automatic-authentication"></a>Automatisk autentisering
Molnet Shell autentiserar på ett säkert sätt automatiskt på varje session för direktåtkomst tooyour resurser via hello Azure CLI 2.0.

### <a name="connect-your-azure-file-storage"></a>Anslut Azure File storage
Molnet Shell datorer är tillfälliga och därför kräver ett Azure file share toobe monterade som `clouddrive` toopersist $Home-katalogen.
Startas för första uppmanar molnet Shell toocreate en resursgrupp, storage-konto, och filresursen för din räkning. Detta är ett enstaka steg och bifogas automatiskt för alla sessioner. 

#### <a name="create-new-storage"></a>Skapa nya lagringsenheter
![](media/basic-storage.png)

Ett lokalt redundant lagringskonto (LRS) kan skapas för din räkning med en Azure-filresurs som innehåller en disk på 5 GB standardavbildningen. hello filresurs monterar som `clouddrive` dela interaktion med hello diskavbildning för filen som används toosync och bevara $Home-katalogen. Vanliga lagringskostnader gäller.

Tre resurser kommer att skapas för din räkning:
1. En resursgrupp med namnet:`cloud-shell-storage-<region>`
2. Lagringskontonamnet:`cs<uniqueGuid>`
3. Filresurs med namnet:`cs-<user>-<domain>-com-uniqueGuid`

> [!Note]
> Alla filer i katalogen $Home, till exempel SSH-nycklar finns kvar i din användare diskavbildning som lagras i monterade filresursen. Tillämpa metodtips när du sparar filer i din $Home katalog och monterade filresurs.

#### <a name="use-existing-resources"></a>Använda befintliga resurser
![](media/advanced-storage.png)

Ett avancerat alternativ finns även vilket gör att du tooassociate befintliga resurser tooCloud Shell. Klicka på ”Visa avancerade inställningar” tooselect ytterligare alternativ när hello lagring installationsprogrammet Kommandotolken kan. Nedrullningsbara listorna filtreras för molntjänster Shell regionen och lokalt/globalt-redundant storage-konton.

[Läs mer om molntjänster Shell lagring, uppdatera filresurser och ladda upp/hämta filer.] (beständighet-shell-storage.md)

## <a name="concepts"></a>Koncept
* Molnet Shell körs på en tillfällig virtuell dator på en per session, per användare
* Molnet Shell timeout efter 20 minuter utan interaktiva aktivitet
* Molnet Shell kan endast användas med en filresurs som ansluten
* Molnet Shell tilldelas en dator per konto
* Behörigheter har angetts som en vanlig Linux-användare

[Läs mer om alla moln Shell-funktioner.](features.md)

## <a name="examples"></a>Exempel
* Skapa eller redigera skript tooautomate Azure management
* Samtidigt hantera resurser via Azure-portalen och Azure CLI 2.0
* Testkör Azure CLI 2.0

[Prova att använda de här exemplen på hello molnet Shell Snabbstart.](quickstart.md)

## <a name="pricing"></a>Prissättning
hello-dator som är värd molnet Shell är ledig, med ett krav för en monterad Azure fil dela toopersist $Home-katalogen. Vanliga lagringskostnader gäller.

## <a name="supported-browsers"></a>Webbläsare som stöds
Molnet Shell rekommenderas för Chrome, kant och Safari. När molnet Shell stöds för Chrome, Firefox, Safari, Internet Explorer eller Edge, är moln-gränssnittet ämne toospecific webbläsarinställningar.

## <a name="troubleshooting"></a>Felsökning
1. När du använder en prenumeration på Azure Active Directory, det går inte att skapa lagring på grund av tooError: 400 DisallowedOperation. tooresolve, Använd en Azure-prenumeration kan skapa lagringsresurser. AD-prenumerationer är inte toocreate Azure-resurser.

Vissa kända begränsningar finns [begränsningar i molnet Shell](limitations.md).
