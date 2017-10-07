---
title: "aaaMATLAB kluster på virtuella datorer | Microsoft Docs"
description: "Använda Microsoft Azure virtuella datorer toocreate MATLAB Computing Server för distribuerad kluster toorun beräkningsintensiva parallella MATLAB arbetsbelastningar"
services: virtual-machines-windows
documentationcenter: 
author: mscurrell
manager: timlt
editor: 
ms.assetid: e9980ce9-124a-41f1-b9ec-f444c8ea5c72
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Windows
ms.workload: infrastructure-services
ms.date: 05/09/2016
ms.author: markscu
ms.openlocfilehash: 266749dbdcfefac42c94b74aa612bfee18075a20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-matlab-distributed-computing-server-clusters-on-azure-vms"></a>Skapa MATLAB distribuerad datoranvändning serverkluster på Azure Virtual Machines
Använd Microsoft Azure virtuella datorer toocreate en eller flera MATLAB distribuerad datoranvändning serverkluster toorun beräkningsintensiva parallella MATLAB arbetsbelastningar. Installera programvaran MATLAB Computing Server för distribuerad på en VM-toouse som en grundläggande bild och Använd en mall för Azure quickstart eller Azure PowerShell-skript (tillgänglig på [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster)) toodeploy och hantera hello-kluster. Efter distributionen kan du ansluta toohello klustret toorun dina arbetsbelastningar.

## <a name="about-matlab-and-matlab-distributed-computing-server"></a>Om MATLAB och MATLAB distribuerad datoranvändning Server
Hej [MATLAB](http://www.mathworks.com/products/matlab/) plattformen är optimerad för att lösa problem med teknisk och vetenskaplig. MATLAB användare med stora simulering och databearbetningsuppgifter kan använda MathWorks parallell databearbetning produkter toospeed in sina beräkningsintensiva arbetsbelastningar genom att dra nytta av beräkningskluster och rutnätet tjänster. [Parallell Computing verktygslådan](http://www.mathworks.com/products/parallel-computing/) användarna MATLAB parallelize program och dra nytta av processorer med flera kärnor, GPU-kort, och beräkningsklustren. [MATLAB Computing Server för distribuerad](http://www.mathworks.com/products/distriben/) kan MATLAB användare tooutilize många datorer på ett beräkningskluster.

Med hjälp av Azure virtuella datorer kan du skapa MATLAB distribuerad datoranvändning serverkluster som har alla hello samma metoder tillgängliga toosubmit parallella arbete som lokala kluster, till exempel interaktiva jobb, batchjobb, oberoende uppgifter och kommunikation uppgifter. Använda Azure tillsammans med hello MATLAB plattform har många fördelar jämfört med tooprovisioning och med hjälp av traditionella lokala maskinvara: en uppsättning virtuella storlekar, skapa kluster på begäran så att du betalar bara för hello beräkningsresurser som du använder, och hello möjlighet tootest modeller i större skala.  

## <a name="prerequisites"></a>Krav
* **Klientdatorn** -behöver du en Windows-baserad klient datorn toocommunicate med Azure och hello MATLAB distribuerad datoranvändning serverkluster efter distributionen.
* **Azure PowerShell** -finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) tooinstall den på din klientdator.
* **Azure-prenumeration** -om du inte har en prenumeration kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/free/) på bara några minuter. Överväg att en prenumeration med användningsbaserad betalning eller andra köpalternativ för större kluster.
* **Kärnor kvoten** – du kan behöva tooincrease hello core kvoten toodeploy stora kluster eller mer än en MATLAB distribuerad datoranvändning serverkluster. tooincrease en kvot [öppna en supportbegäran online customer](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) utan kostnad.
* **MATLAB, parallella Computing verktygslådan och MATLAB Computing Server för distribuerad licenser** -hello skript förutsätter att hello [MathWorks finns Licenshanteraren](http://www.mathworks.com/products/parallel-computing/mathworks-hosted-license-manager/) används för alla licenser.  
* **MATLAB distribuerad datoranvändning serverprogramvara** -kommer att installeras på en virtuell dator som ska användas som hello VM basavbildning för hello kluster virtuella datorer.

## <a name="high-level-steps"></a>Hög nivå steg
toouse Azure virtuella datorer för dina MATLAB distribuerad datoranvändning serverkluster hello följande avancerade steg krävs. Detaljerade anvisningar finns i hello-dokumentationen som medföljer hello quickstart mallen och skripten på [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster).

1. **Skapa en grundläggande VM-avbildning**  

   * Hämta och installera MATLAB Computing Server för distribuerad programvara på den här virtuella datorn.

     > [!NOTE]
     > Den här processen kan ta några timmar, men du bara toodo den när du använder för varje version av MATLAB.   
     >
     >
2. **Skapa ett eller flera kluster**  

   * Använd hello angivna PowerShell-skript eller hello quickstart mallen toocreate ett kluster från hello grundläggande VM-avbildning.   
   * Hantera hello-kluster med hello angivna PowerShell-skript som gör att du toolist, pausa, återuppta och ta bort kluster.

## <a name="cluster-configurations"></a>Klusterkonfigurationer
För närvarande hello kluster Skapandeskriptet och mallen kan du toocreate en enda MATLAB Computing Server för distribuerad topologi. Om du vill skapa en eller flera ytterligare kluster med varje kluster med ett annat antal worker virtuella datorer, med olika storlekar på VM, och så vidare.

### <a name="matlab-client-and-cluster-in-azure"></a>MATLAB klient och kluster i Azure
hello MATLAB klientnod och MATLAB jobbschemat nod MATLAB Computing Server för distribuerad ”worker” noder är alla konfigurerade som virtuella Azure-datorer i ett virtuellt nätverk enligt hello följande bild.


* toouse hello kluster, ansluta med Remote Desktop toohello klientnod. hello klientnod kör hello MATLAB klienten.
* hello klientnoden har en filresurs som kan nås av alla anställda.
* MathWorks finns License Manager används för hello licens kontrollerar alla MATLAB programvara.
* Som standard skapas en MATLAB Computing Server för distribuerad arbetare per kärna på hello worker virtuella datorer, men du kan ange ett tal.

## <a name="use-an-azure-based-cluster"></a>Använda ett Azure-baserade kluster
Precis som med andra typer av MATLAB distribuerad datoranvändning serverkluster måste toouse hello Klusterhanterare för profilen i hello MATLAB klienten (på hello klienten VM) toocreate en MATLAB jobbschemat klustret profil.

![Hanteraren för profil](./media/matlab-mdcs-cluster/cluster_profile_manager.png)

## <a name="next-steps"></a>Nästa steg
* För detaljerade anvisningar toodeploy och hantera MATLAB distribuerad datoranvändning Server-kluster i Azure, se hello [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster) databasen som innehåller hello mallar och skript.
* Gå toohello [MathWorks plats](http://www.mathworks.com/) mer detaljerad dokumentation för MATLAB och MATLAB distribuerad datoranvändning Server.
