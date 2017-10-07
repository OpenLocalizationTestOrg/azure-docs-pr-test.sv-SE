---
title: "aaaAzure compute-alternativ - molntjänster | Microsoft Docs"
description: "Lär dig mer om Azure compute värd alternativ och hur de fungerar: Apptjänst molntjänster och virtuella datorer"
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
ms.assetid: ed7ad348-6018-41bb-a27d-523accd90305
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: e7f109a54c61cc2f37644d39a61d2d932a374587
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="should-i-choose-cloud-services-or-something-else"></a>Ska jag välja molntjänster eller något annat?
Är Azure Cloud Services hello valet för dig? Azure tillhandahåller olika modeller som värd för program som körs. Var och en innehåller en annan uppsättning tjänster, så vilket som du väljer beror på exakt vad du skulle toodo.

[!INCLUDE [compute-table](../../includes/compute-options-table.md)]

<a name="tellmecs"></a>

## <a name="tell-me-about-cloud-services"></a>Berätta om molntjänster
Molntjänster är ett exempel på [plattform som en tjänst](https://azure.microsoft.com/overview/what-is-paas/) (PaaS). Som [Apptjänst](../app-service-web/app-service-web-overview.md), den här tekniken är utformad toosupport program som är skalbara, tillförlitliga och billig toooperate. Precis som en Apptjänst är värd för virtuella datorer, så är för molntjänster, men har du större kontroll över hello virtuella datorer. Du kan installera en egen programvara på virtuella datorer för molnet tjänsten och du kan fjärranslutna i dem.

![cs_diagram](./media/cloud-services-choose-me/diagram.png)

Mer kontroll innebär också mindre användarvänlighet. Om du inte behöver hello ytterligare alternativ, det är normalt snabbare och enklare tooget ett webbprogram och körs i Web Apps i Apptjänst jämförs tooCloud tjänster.

Det finns två typer av Molntjänsten roller. hello endast skillnaden mellan hello två är hur din roll finns på hello virtuell dator.

* **Webbroll**  
Distribuerar och är värd för din app via IIS automatiskt.

* **Arbetsroll**  
Använder inte IIS och kör din app fristående.

Ett enkelt program kan till exempel använda bara en enda webbrollen, betjänar en webbplats. Ett mer komplext program kan använda en webbserver-rollen toohandle inkommande begäranden från användare, skickar sedan dessa begäranden på tooa worker-rollen för bearbetning. (Den här kommunikationen kan använda [Service Bus](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md) eller [Azure köer](../storage/common/storage-introduction.md).)

Som hello föregående bild föreslås alla hello virtuella datorer i ett enda program som körs i hello samma molntjänst. Användare åtkomst hello program via en offentlig IP-adress, med förfrågningar automatiskt belastningsutjämnade över hello programmets virtuella datorer. hello plattform [skalas och distribuerar](cloud-services-how-to-scale.md) hello virtuella datorer i ett Cloud Services-program på ett sätt som förhindrar en enskild felpunkt maskinvara.

Även om program som körs i virtuella datorer, är det viktigt toounderstand att molntjänster tillhandahåller PaaS, inte IaaS. Här är ett sätt toothink om den: med IaaS, till exempel Azure virtuella datorer du först skapa och konfigurera ditt program körs i hello-miljö och sedan distribuera programmet till den här miljön. Du är ansvarig för att hantera mycket världen, gör saker, till exempel distribuera nya korrigeringsfil versioner av hello operativsystemet på varje virtuell dator. I PaaS är det däremot som om hello-miljö finns redan. Du har toodo är att distribuera ditt program. Hello-plattform som den körs på, inklusive distribution av nya versioner av operativsystemet hello hanteras åt dig.

## <a name="scaling-and-management"></a>Skalning och hantering
Med Cloud Services, kan du inte skapa virtuella datorer. I stället kan du ange en konfigurationsfil som talar om Azure hur många av var du vill, t.ex **tre webbrollsinstanser** och **två worker rollinstanser**, och hello plattform skapar dem åt dig.  Du fortfarande välja [hur stor](cloud-services-sizes-specs.md) de säkerhetskopiera virtuella datorer bör vara, men du inte uttryckligen skapa dem själv. Om ditt program måste toohandle belastningen, du kan be för flera virtuella datorer och Azure skapar dessa instanser. Du kan stänga av dessa instanser och stoppa betalar för dem om hello belastningen minskar.

Ett program för molntjänster görs normalt tillgängliga toousers via en tvåstegsprocess. En utvecklare första [överföringar hello programmet](cloud-services-how-to-create-deploy.md) toohello plattform uppbyggnadsområde. När hello utvecklare är klar toomake hello program live använder de hello Azure portal tooswap mellanlagring till produktionen. Detta [växla mellan mellanlagrings- och](cloud-services-nodejs-stage-application.md) kan göras utan avbrott, vilket gör att ett program som körs att uppgraderade tooa nya versionen utan att störa sina användare.

## <a name="monitoring"></a>Övervakning
Cloud Services ger också övervakning. Som Azure Virtual Machines, den identifierar en misslyckad fysisk server och startar om hello virtuella datorer som kördes på servern på en ny dator. Men molntjänster identifierar också misslyckade virtuella datorer och program, inte bara maskinvarufel. Till skillnad från virtuella datorer, den har en agent i rollerna webb- och arbetsroller och det är därför kan toostart nya virtuella datorer och programinstanser när fel uppstår.

Hej PaaS uppbyggnad molntjänster har andra effekter för. En av de viktigaste hello är att program som bygger på den här tekniken ska skrivas toorun korrekt när alla webb eller arbetare rollinstansen misslyckas. tooachieve en molntjänster program inte bör Underhåll detta tillstånd i hello filsystemet på sin egen virtuella datorer. Till skillnad från virtuella datorer som skapades med Azure Virtual Machines, skrivningar görs tooCloud Services virtuella datorer inte är beständig; Det finns inget som en datadisk för virtuella datorer. I stället en molntjänster program bör uttryckligen skriva alla tillstånd tooSQL databas, blobbar, tabeller eller annan extern lagring. Bygga program sätt gör dem lättare tooscale och mer motståndskraftiga toofailure både viktiga mål av molntjänster.

## <a name="next-steps"></a>Nästa steg
[Skapa en cloud service-app i .NET](cloud-services-dotnet-get-started.md)  
[Skapa en cloud service-app i Node.js](cloud-services-nodejs-develop-deploy-app.md)  
[Skapa en cloud service-app i PHP](../cloud-services-php-create-web-role.md)  
[Skapa en cloud service-app i Python](cloud-services-python-ptvs.md)

