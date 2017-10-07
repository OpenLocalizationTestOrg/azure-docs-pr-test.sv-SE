---
title: "aaaAzure översikt över Functions-Runtime | Microsoft Docs"
description: "Översikt över hello Azure Functions Runtime Preview"
services: functions
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 05/08/2017
ms.author: anwestg
ms.openlocfilehash: 8ce3e5037731d499c330b395c89c90109d18d65b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-runtime-overview"></a>Översikt över Azure Functions-Runtime

hello Azure Functions-Runtime är en ny metod för du tootake utnyttja hello enkelhet och flexibilitet i hello Azure Functions programming modellen på lokalt. Bygger på hello öppna samma källa rötter eftersom Azure Functions, Azure Functions-Runtime är distribuerade lokalt tooprovide nästan identisk development upplevelse som hello tjänst i molnet.

![Azure Functions-Runtime Preview-portalen][1]

hello Azure Functions-Runtime kan du tooexperience Azure Functions innan du genomför toohello moln. På så sätt kan kan hello kod tillgångar som du skapar sedan tas toohello moln när du migrerar.  hello runtime öppnas också nya alternativ för dig, till exempel med hjälp av ledig datorkraft som hello lokala datorer toorun batch processer över natten. Du kan också använda enheter inom din organisation tooconditionally skicka data tooother system, både lokalt och i hello molnet.

hello Azure Functions-Runtime består av två delar:
* Azure Functions-Runtime Ledningsroll
* Azure Functions-Runtime Worker-rollen

## <a name="azure-functions-management-role"></a>Azure Functions roll

hello Azure Functions Ledningsroll innehåller en värd för hello hanteringen av dina funktioner lokala. Den här rollen utför hello följande uppgifter:

* Att vara värd för hello Azure Functions-hanteringsportalen som är hello hello samma som visas i hello [Azure-portalen](https://portal.azure.com). Det här kan du utveckla dina funktioner i hello samma sätt som i hello Azure-portalen.
* Distribuera funktioner på flera funktioner arbetare.
* Tillhandahåller en publishing slutpunkt så att du kan publicera dina funktioner direkt från Microsoft Visual Studio.

## <a name="azure-functions-worker-role"></a>Azure Functions Worker-rollen

hello Azure Functions arbetsroller distribueras i Windows-behållare och det är där Funktionskoden körs.  Du kan distribuera flera arbetsroller i organisationen och är viktiga sätt där kunder kan göra att använda ledig datorkraft.

## <a name="minimum-requirements"></a>Minimikrav

tooget igång med hello Azure Functions-Runtime måste du ha en dator med **Windows Server 2016 eller Windows 10 skapare Update** med åtkomst tooa **SQL Server** instans.

## <a name="next-steps"></a>Nästa steg

Installera hello [Azure Functions-Runtime preview](https://aka.ms/azafr)

<!--Image references-->
[1]: ./media/functions-runtime-overview/AzureFunctionsRuntime_Portal.png
