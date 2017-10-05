---
title: Distribuera QuickBooks i Azure RemoteApp | Microsoft Docs
description: "Lär dig hur du delar QuickBooks med Azure RemoteApp."
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
ms.openlocfilehash: bbfac45220f6ef36c9951daae0ced1974e8ddabb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-do-you-deploy-quickbooks-in-azure-remoteapp"></a>Hur distribuerar du QuickBooks i Azure RemoteApp?
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.
> 
> 

Använd följande information för att dela QuickBooks som en app i Azure RemoteApp.

Du kan dela QuickBooks 2015 Enterprise med Azure RemoteApp i en hybrid eller molnet samling. Filen för företagets måste finnas på en virtuell dator som kör QuickBooks databasserver som är separat från Azure RemoteApp-servrar. Lagrar aldrig företagsfil på Azure RemoteApp-avbildning - dataförlust förväntas om du gör detta. Endast QuickBooks Enterprise har stöd för värd för filen QuickBooks på en extern resurs med QuickBooks databasserver som är tillgänglig via vanliga Windows-nätverk.   

> [!IMPORTANT]
> QuickBooks databasservern som är värd för filen för företagets måste finnas på en separat virtuell dator i samma virtuella nätverk som Azure RemoteApp-samling.  
> 
> 

## <a name="steps-to-deploy-quickbooks"></a>Steg för att distribuera QuickBooks
1. Skapa en virtuell dator i Azure och installera QuickBooks QuickBooks-databasservern och placera filen för företagets på en Azure VM.  Se till att konfigurera brandväggsregler.
2. Installera QuickBooks på en [anpassad avbildning](remoteapp-imageoptions.md) och skapa en [Azure RemoteApp-samlingen](remoteapp-collections.md), moln eller hybrid i exakt samma virtuella nätverk där den virtuella datorn som värd för databasservern QuickBooks med företagets filer finns. 
3. [Publicera](remoteapp-publish.md) QuickBooks app till användare
4. Starta Azure RemoteApp-värdbaserad QuickBooks klienten, navigera med hjälp av Windows-nätverk som standard till den virtuella datorn som värd för databasservern QuickBooks och öppna filen för företagets. 

## <a name="documentation-references"></a>Dokumentationen referenser
* QuickBooks [konfigurationer som stöds](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)
* QuickBooks [distributionsalternativ](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)

Du kan också kontrollera ut presentationen Ignite [grunderna för Microsoft Azure RemoteApp-hantering och Administration](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) -spola framåt till 1:02:45 till QuickBooks del.

## <a name="deployment-architecture"></a>Arkitektur för distribution
![QuickBooks + Azure RemoteApp-distribution](./media/remoteapp-quickbooks/ra-quickbooks.png)

