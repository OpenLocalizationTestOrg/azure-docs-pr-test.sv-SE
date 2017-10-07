---
title: "aaaMigrate virtuellt Azure-nätverk (klassiskt) från en tillhörighetsgrupp tooa region | Microsoft Docs"
description: "Lär dig hur toomigrate ett virtuellt nätverk (klassiskt) från en tillhörighet gruppera tooa region."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 84febcb9-bb8b-4e79-ab91-865ad9de41cb
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: e3a5c0f21e748912e29e2e8d03f4295720e63637
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-a-virtual-network-classic-from-an-affinity-group-tooa-region"></a>Migrera ett virtuellt nätverk (klassiska) från en tillhörighetsgrupp tooa region

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

Tillhörighetsgrupper se till att resurserna skapas i samma tillhörighetsgrupp fysiskt finns på servrar som är nära varandra att aktivera dessa resurser toocommunicate snabbare hello. I hello tidigare var tillhörighetsgrupper ett krav för att skapa virtuella nätverk (klassiska). Då gick hello network manager-tjänsten som hanteras av virtuellt nätverk (klassiskt) fungerar bara i en uppsättning fysiska servrar eller skalningsenhet. Arkitektur förbättringar har ökat hello omfattning nätverket management tooa region.

På grund av förbättringarna arkitektur finns tillhörighetsgrupper inte längre rekommenderas eller krävs för virtuella nätverk (klassiska). hello användningen av tillhörighetsgrupper för virtuella nätverk (klassiskt) ersätts av regioner. Virtuella nätverk (klassiskt) som är associerade med regioner kallas regionala virtuella nätverk.

Vi rekommenderar att du inte använder tillhörighetsgrupper i allmänhet. Utöver hello virtuella nätverket kravet tillhörighetsgrupper har också viktig toouse tooensure resurser, till exempel beräkning (klassisk) och lagring (klassisk), placerades nära varandra. Men med hello aktuella Azure-nätverksarkitekturen behövs kraven placering inte längre.

> [!IMPORTANT]
> Det är tekniskt möjligt toocreate ett virtuellt nätverk som är associerad med en tillhörighetsgrupp, men det finns inga tvingande orsak toodo så. Många funktioner för virtuella nätverk, till exempel nätverkssäkerhetsgrupper, är bara tillgängliga när du använder ett regionalt virtuellt nätverk och är inte tillgängliga för virtuella nätverk som är associerade med tillhörighetsgrupper.
> 
> 

## <a name="edit-hello-network-configuration-file"></a>Redigera konfigurationsfilen för hello nätverk

1. Exportera konfigurationsfilen för hello nätverk. toolearn hur tooexport en nätverkskonfiguration med PowerShell eller hello Azure-kommandoradsgränssnittet (CLI) version 1.0, se [konfigurera ett virtuellt nätverk med en konfigurationsfil för nätverket](virtual-networks-using-network-configuration-file.md#export).
2. Redigera konfigurationsfilen för hello nätverk, ersätter **AffinityGroup** med **plats**. Du anger en Azure [region](https://azure.microsoft.com/regions) för **plats**.
   
   > [!NOTE]
   > Hej **plats** hello region som du angav för hello tillhörighetsgrupp som är kopplad till ditt virtuella nätverk (klassiska). Om ditt virtuella nätverk (klassiska) som är associerad med en tillhörighetsgrupp som finns i västra USA när du migrerar, till exempel din **plats** måste peka tooWest USA. 
   > 
   > 
   
    Redigera hello följande rader i konfigurationsfilen nätverk, ersätter hello värden med dina egna: 
   
    **Gammalt värde:** \<VirtualNetworkSitename = ”VNetUSWest” AffinityGroup = ”VNetDemoAG”\> 
   
    **Nytt värde:** \<VirtualNetworkSitename = ”VNetUSWest” sökväg = ”USA, västra”\>
3. Spara dina ändringar och [importera](virtual-networks-using-network-configuration-file.md#import) hello tooAzure för konfiguration av nätverk.

> [!NOTE]
> Migreringen medför några driftavbrott tooyour tjänster.
> 
> 

## <a name="what-toodo-if-you-have-a-vm-classic-in-an-affinity-group"></a>Vilka toodo om du har en virtuell dator (klassisk) i en tillhörighetsgrupp
Virtuella datorer (klassiskt) som används för närvarande i en tillhörighetsgrupp behöver inte toobe tas bort från hello tillhörighetsgrupp. När en virtuell dator har distribuerats, är det enda skalningsenhet för distribuerade tooa. Tillhörighetsgrupper kan begränsa hello uppsättning tillgängliga VM-storlekar för en ny VM-distribution, men en befintlig virtuell dator som har distribuerats redan är begränsad toohello uppsättning VM storlekar i hello skalningsenhet i vilka hello VM distribueras. Eftersom hello VM är redan distribuerad tooa skalningsenhet, har ta bort en virtuell dator från en tillhörighetsgrupp ingen effekt på hello VM.
