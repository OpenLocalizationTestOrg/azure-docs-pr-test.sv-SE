---
title: aaaUse Windows klient-avbildningar i Azure | Microsoft Docs
description: "Hur toouse prenumeration på Visual Studio fördelar toodeploy Windows 7, Windows 8 eller Windows 10 i Azure för utveckling och testning scenarier"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 91c3880a-cede-44f1-ae25-f8f9f5b6eaa4
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: iainfou
ms.openlocfilehash: 4ac7c3d9872673030f4ea0f0ab38625dd9d9c1b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-windows-client-in-azure-for-devtest-scenarios"></a>Använda Windows-klienten i Azure för utveckling och testning scenarier
Du kan använda Windows 7, Windows 8 eller Windows 10 i Azure för utveckling och testning scenarier som du har en lämplig Visual Studio (tidigare MSDN)-prenumeration. Den här artikeln beskrivs hello behörighetskraven för att köra Windows-klienten i Azure och användning av hello Azure-galleriet bilder.

## <a name="subscription-eligibility"></a>Prenumerationen är behörig
Aktiva Visual Studio-prenumeranter (personer har skaffat en licens för Visual Studio-prenumeration) kan använda Windows-klient för utveckling och testning. Windows-klient kan användas på maskinvara och Azure virtuella datorer som körs i en typ av Azure-prenumeration. Windows-klienter kanske inte distribuerade tooor användas i Azure för normal produktion eller användas av personer som inte är aktiva Visual Studio-prenumeranter.

För din bekvämlighet vi har gjort vissa Windows 10-avbildningar tillgängliga från hello Azure-galleriet inom [berättigade utveckling och testning erbjuder](#eligible-offers). Visual Studio-prenumeranter inom någon typ av erbjudandet kan också [på lämpligt sätt förbereda och skapa](prepare-for-upload-vhd-image.md) en 64-bitars Windows 7, Windows 8 eller Windows 10-avbildning och sedan [överför tooAzure](upload-generalized-managed.md). hello används fortfarande begränsat toodev och testning av aktiva Visual Studio-prenumeranter.

## <a name="eligible-offers"></a>Tillgängliga erbjudanden
följande tabell information hello hello erbjuder-ID som är berättigade toodeploy Windows 10 via hello Azure-galleriet. hello Windows 10-avbildningar är bara synliga toohello följande erbjudanden. Visual Studio-prenumeranter som behöver toorun Windows-klienter i ett annat erbjudandetypen kräver att du för[på lämpligt sätt förbereda och skapa](prepare-for-upload-vhd-image.md) en 64-bitars Windows 7, Windows 8 eller Windows 10-avbildning och [överför tooAzure](upload-generalized-managed.md).

| Erbjudandets namn | Erbjudandets nummer | Tillgängliga avbildningar |
|:--- |:---:|:---:|
| [Betala per användning utveckling och testning](https://azure.microsoft.com/offers/ms-azr-0023p/) |0023P |Windows 10 |
| [Visual Studio Enterprise (MPN)-prenumeranter](https://azure.microsoft.com/offers/ms-azr-0029p/) |0029P |Windows 10 |
| [Visual Studio Professional-prenumeranter](https://azure.microsoft.com/offers/ms-azr-0059p/) |0059P |Windows 10 |
| [Visual Studio Test Professional-prenumeranter](https://azure.microsoft.com/offers/ms-azr-0060p/) |0060P |Windows 10 |
| [Visual Studio Premium med MSDN (förmåner)](https://azure.microsoft.com/offers/ms-azr-0061p/) |0061P |Windows 10 |
| [Visual Studio Enterprise-prenumeranter](https://azure.microsoft.com/offers/ms-azr-0063p/) |0063P |Windows 10 |
| [Visual Studio Enterprise (BizSpark)-prenumeranter](https://azure.microsoft.com/offers/ms-azr-0064p/) |0064P |Windows 10 |
| [Utveckling och testning Enterprise](https://azure.microsoft.com/ofers/ms-azr-0148p/) |0148P |Windows 10 |

## <a name="check-your-azure-subscription"></a>Kontrollera din Azure-prenumeration
Om du inte vet ditt erbjudande-ID, kan du skaffa det genom hello Azure-portalen i något av följande två sätt:  

- Hej, prenumerationer' bladet:

  ![Erbjuder ID-information från hello Azure-portalen](./media/client-images/offer-id-azure-portal.png) 

- Klicka på **fakturering** och klicka sedan på ditt prenumerations-ID. hello erbjudande ID visas i hello fakturering bladet.

Du kan också visa hello erbjudande ID från hello [fliken 'Abonnemang'](http://account.windowsazure.com/Subscriptions) hello Azure-kontoportalen:

![Erbjuder ID-information från hello Azure-kontoportalen](./media/client-images/offer-id-azure-account-portal.png) 

## <a name="next-steps"></a>Nästa steg
Du kan nu distribuera dina virtuella datorer med hjälp av [PowerShell](quick-create-powershell.md), [Resource Manager-mallar](ps-template.md), eller [Visual Studio](../../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

