---
title: aaaDeploying stora distributioner
description: "Lär dig hur toodeploy stora distributioner med hello Azure Toolkit för Eclipse."
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
ms.openlocfilehash: 6b1d2a7a5e49c78154fc856a221e64ca8dcfbe9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploying-large-deployments"></a>Distribuera stora distributioner
Om din distribution är för stor toobe i hello approot standardmappen, du kan använda en resurs för lokal lagring som hello distribution rotmappen för din JDK och programservrar.

## <a name="toouse-a-local-storage-resource-as-hello-deployment-root-folder-for-large-deployments"></a>toouse en resurs för lokal lagring som hello distribution rotmapp för stora distributioner
1. Skapa en ny resurs för lokal lagring. hello namnet på hello resursen spelar ingen roll. Lagringsresurser definieras på hello rollnivå. hello snabbaste sättet tooaccess hello lokal lagring konfigurationsdialogruta, där du kan skapa en ny resurs i lokal lagring är med hjälp av följande hello: Högerklicka på hello roll i hello **Projektutforskaren** visa (expandera din Azure-projekt nod om du inte ser hello rollen), klickar du på **Azure**, och klicka sedan på **lokal lagring**. Inom hello **lokal lagring** dialogrutan klickar du på **Lägg till** toocreate en ny resurs för lokal lagring.

2. Ange hello önskad storlek tooat minst 2 048 MB (något mindre kan orsaka hello samma storlek problem som du kan stöta på i hello approot).

3. Se till att **Rensa hello innehållet när hälsningspaket rollinstansen återanvänds** är markerat; detta förhindrar att hello distribution Startlogik körs i konflikt med befintliga filer i hello resurs när hello roll instansen återanvänds.

4. Se till att hello **miljö variabeln lagra hello resursens sökväg efter distributionen** toohello sträng har värdet **DEPLOYROOT**. Lokal lagring resurs dialogen ser liknande toohello följande.

   ![][ic667943]

Alternativt, om du använder **DEPLOYROOT** som hello *namn* av din lokala resursen och du inte ändra hello genereras automatiskt Miljövariabelnamnet (som anges för **DEPLOYROOT_PATH** i så fall), som fungerar för ditt program samt.

Mer information om hur du skapar en resurs för lokal lagring finns på [egenskaper för lokal lagring][Local storage properties].

## <a name="see-also"></a>Se även
[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]

[Skapa ett Hello World-program för Azure i Eclipse][Creating a Hello World Application for Azure in Eclipse]

[Installera hello Azure Toolkit för Eclipse][Installing hello Azure Toolkit for Eclipse] 

Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Local storage properties]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->
