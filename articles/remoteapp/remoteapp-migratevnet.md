---
title: "aaaHow toomigrate från RemoteApp VNET-tooan Azure VNET | Microsoft Docs"
description: "Lär dig hur toomigrate från RemoteApp VNET-tooan Azure VNET"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: baea5d29-353b-48f8-b47f-806f2163e067
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: c0f8617556c6f1e33eca8322febf67ff33937ecd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomigrate-a-hybrid-collection-from-a-remoteapp-vnet-tooan-azure-vnet"></a>Hur toomigrate en hybridsamling från en RemoteApp VNET tooan Azure VNET
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Goda nyheter! Vi har aktiverat toodeploy hybrid RemoteApp-samlingar direkt i din befintliga virtuella Azure-nätverk (Vnet) i stället för att skapa RemoteApp-specifika Vnet. Detta kan du dra nytta av hello senaste VNET-funktioner (till exempel ExpressRoute) och ge din hybrid samlingar direkt network access tooother Azure-tjänster och virtuella datorer distribueras toothat VNET.  (Det här får du bättre prestanda och enklare konfiguration än VNET-till-VNET-konfigurationer).

Anta att du redan har skapat en hybrid RemoteApp-samling som heter *OriginalCollection* med en RemoteApp-VNET kallas *RemoteAppVNET*. Här följer hello steg toomigrate den tooa nytt Azure VNET kallas *AzureVNET*.

1. På hello **nätverk** fliken i hello [hanteringsportalen](http://manage.windowsazure.com/), skapa ett virtuellt nätverk kallas *AzureVNET*med hjälp av hello samma plats, DNS-konfigurationen och adressutrymme (för minst en för hello *AzureVNET* undernät) som du använde för *RemoteAppVNET*.
2. Konfigurera *AzureVNET* tooeither värd eller har network connectivity toohello Active Directory-distribution som *OriginalCollection* är domänen som är ansluten till.
3. På hello **RemoteApp-program** fliken, skapa en ny RemoteApp-samling som heter *ny samling*. (Använd hello **skapa med VNET** alternativet inte **Snabbregistrering**.)
4. Konfigurera *NewCollection* toobe distribueras tooa undernät i *AzureVNET*.
5. Konfigurera *NewCollection* toouse hello samma avbildning och domänanslutningsinformationen som du använde för *OriginalCollection*.
6. Efter några timmar *NewCollection* visas i samlingslistan med aktiv.

Nu om du inte behöver toomigrate användarinformation från hello ursprungliga samlingen toohello ny samling, göra de här stegen sedan:

1. Ta bort *OriginalCollection*.
2. Ta bort *RemoteAppVNET*.

Och du är klar!

Alternativt, om du behöver toomigrate användarinformation från hello ursprungliga samlingen toohello ny samling, göra de här stegen sedan:

1. Skicka ett e-postmeddelande för[ remoteappforum@microsoft.com ](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20user%20information%20migration) hello namnet på din ursprungliga samling och hello namnet på den nya samlingen med ditt Azure prenumerations-ID och be dem toomigrate din användarinformation.
2. Inom 2 arbetsdagar flyttar hello RemoteApp-teamet hello användarlistan för åtkomst och alla användardokument och användarinställningar från hello ursprungliga samlingen toohello ny samling.
3. Ta bort *OriginalCollection*.
4. Ta bort *RemoteAppVNET*.

Och du är nu klar!

Om du har några frågor eller behöver hjälp med särskilda e-postmeddelande till [ remoteappforum@microsoft.com ](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20VNET%20migration%20help).

