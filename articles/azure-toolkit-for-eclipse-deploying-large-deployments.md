---
title: Distribuera stora distributioner
description: "Lär dig hur du distribuerar stora distributioner som använder Azure Toolkit för Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 5e18bace-5df0-4af8-ad86-6151ea8bd823
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: e12e379e2b6727653e2377b1760c3745596a1e9c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploying-large-deployments"></a>Distribuera stora distributioner
Om din distribution är för stort för att finnas i approot standardmappen, du kan använda en resurs för lokal lagring som distributionen rotmappen för din JDK och programservrar.

## <a name="to-use-a-local-storage-resource-as-the-deployment-root-folder-for-large-deployments"></a>Att använda en lokal lagring-resurs som distributionen rotmapp för stora distributioner
1. Skapa en ny resurs för lokal lagring. Namnet på resursen spelar ingen roll. Lagringsresurser definieras på rollnivå. Det snabbaste sättet att komma åt konfigurationsdialogruta lokal lagring, där du kan skapa en ny resurs i lokal lagring är med hjälp av följande steg: Högerklicka på rollen i den **Projektutforskaren** visa (expandera din Azure-projekt Klicka på noden om du inte ser rollen), **Azure**, och klicka sedan på **lokal lagring**. I den **lokal lagring** dialogrutan klickar du på **Lägg till** att skapa en ny resurs för lokal lagring.

2. Ange önskad storlek på minst 2 048 MB (något mindre kan ge samma fil storlek problem som du skulle uppstå i approot).

3. Se till att **rensa innehållet när instansen återanvänds** är markerat; detta förhindrar att distributionens Startlogik körs i konflikt med befintliga filer i resursen när instansen är återanvänds.

4. Se till att den **miljövariabeln lagra resursens sökväg efter distributionen** värdet strängen **DEPLOYROOT**. Lokal lagring resurs dialogen ser ut ungefär så här.

   ![][ic667943]

Alternativt, om du använder **DEPLOYROOT** som den *namn* av din lokala resursen och du inte ändra genereras automatiskt miljövariabeln (som kommer att anges som **DEPLOYROOT_ SÖKVÄGEN** i så fall), som fungerar för ditt program samt.

Mer information om hur du skapar en resurs för lokal lagring finns på [egenskaper för lokal lagring][Local storage properties].

## <a name="see-also"></a>Se även
[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]

[Skapa ett Hello World-program för Azure i Eclipse][Creating a Hello World Application for Azure in Eclipse]

[Installera Azure Toolkit för Eclipse][Installing the Azure Toolkit for Eclipse] 

Mer information om hur du använder Azure med Java finns det [Azure Java Developer Center][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Local storage properties]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->
