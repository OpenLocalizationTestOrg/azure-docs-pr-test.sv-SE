---
title: aaaDeploy QuickBooks i Azure RemoteApp | Microsoft Docs
description: "Lär dig hur tooshare QuickBooks med Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: c5d00753-77c0-4f0d-a5df-9451b46a31d3
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: c21b1ac311449be2281f9ce157828260e856f55d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-do-you-deploy-quickbooks-in-azure-remoteapp"></a>Hur distribuerar du QuickBooks i Azure RemoteApp?
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Använd följande information tooshare QuickBooks som en app i Azure RemoteApp hello.

Du kan dela QuickBooks 2015 Enterprise med Azure RemoteApp i en hybrid eller molnet samling. hello företagsfil måste finnas på en virtuell dator som kör QuickBooks databasserver som är separat från hello Azure RemoteApp-servrar. Lagrar aldrig hello företagsfil på Azure RemoteApp-avbildning - dataförlust förväntas om du gör detta. Endast QuickBooks Enterprise stöder värd hello QuickBooks-filen på en extern resurs med QuickBooks databasserver som är tillgänglig via vanliga Windows-nätverk.   

> [!IMPORTANT]
> hello QuickBooks databasserver som är värd för hello företagsfil måste finnas på en separat virtuell dator i hello samma virtuella nätverk som hello Azure RemoteApp-samling.  
> 
> 

## <a name="steps-toodeploy-quickbooks"></a>Steg toodeploy QuickBooks
1. Skapa en virtuell dator i Azure och installera QuickBooks QuickBooks-databasservern och placera hello företagsfil i en Azure VM.  Se till att tooproperly konfigurera brandväggens regler.
2. Installera QuickBooks på en [anpassad avbildning](remoteapp-imageoptions.md) och skapa en [Azure RemoteApp-samlingen](remoteapp-collections.md), moln eller hybrid inom hello exakt samma virtuella nätverk där hello VM värd hello QuickBooks databasserver med företagets filer finns. 
3. [Publicera](remoteapp-publish.md) QuickBooks app toousers
4. Starta hello Azure RemoteApp-värdbaserad QuickBooks klienten, navigera använder standard Windows nätverks-toohello VM hello QuickBooks databasen värdservern och öppna hello företagsfil. 

## <a name="documentation-references"></a>Dokumentationen referenser
* QuickBooks [konfigurationer som stöds](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)
* QuickBooks [distributionsalternativ](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)

Du kan också kontrollera ut presentationen Ignite [grunderna för Microsoft Azure RemoteApp-hantering och Administration](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) -framåt too1:02:45 tooget toohello QuickBooks del.

## <a name="deployment-architecture"></a>Arkitektur för distribution
![QuickBooks + Azure RemoteApp-distribution](./media/remoteapp-quickbooks/ra-quickbooks.png)

