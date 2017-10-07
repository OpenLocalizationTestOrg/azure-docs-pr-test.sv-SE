---
title: "aaaHow tooCreate en Apptjänstmiljö v1"
description: "Beskrivningen av paketflödet skapas för en app service-miljö v1"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: 81bd32cf-7ae5-454b-a0d2-23b57b51af47
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/11/2017
ms.author: ccompy
ms.openlocfilehash: 95feb33854eee5bac02fa68b066e2fc10eb3fede
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-an-app-service-environment-v1"></a>Hur tooCreate en Apptjänstmiljö v1 

> [!NOTE]
> Den här artikeln handlar om hello Apptjänstmiljö v1. Det finns en nyare version av hello Apptjänst-miljö som är enklare toouse och körs på mer kraftfulla infrastruktur. Mer om hello nya versionen börjar med hello toolearn [introduktion toohello Apptjänstmiljö](../app-service/app-service-environment/intro.md).
> 

### <a name="overview"></a>Översikt
hello App Service miljö (ASE) är ett Premium-tjänstealternativ i Azure App Service som levererar en utökad konfiguration-funktion som inte är tillgänglig i hello flera stämplar. Hej ASE funktion i stort sett distribuerar hello Azure App Service till kundens virtuella nätverk. toogain en bättre förståelse av hello funktioner som erbjuds av Apptjänstmiljöer läsa hello [vad är en Apptjänstmiljö] [ WhatisASE] dokumentation.

### <a name="before-you-create-your-ase"></a>Innan du skapar din ASE
Det är viktigt toobe medveten om hello saker du inte kan ändra. Dessa aspekter som du inte kan ändra om din ASE när den har skapats är:

* Plats
* Prenumeration
* Resursgrupp
* Virtuella nätverk som används
* Undernät som används 
* Undernätets storlek

När väljer ett virtuellt nätverk och ange ett undernät, kontrollera att den är tillräckligt stor tooaccomodate eventuell tillväxt. 

### <a name="creating-an-app-service-environment-v1"></a>Skapa en App Service miljö v1
toocreate en Apptjänstmiljö v1 måste toosearch hello Azure Marketplace för ***Apptjänstmiljö v1***, eller genom att gå via Ny -> webb + mobil -> Apptjänst-miljö. toocreate en ASEv1:

1. Ange hello namnet på din ASE. hello-namn som har angetts för hello ASE ska användas för hello appar som har skapats i hello ASE. Om namnet på hello ASE är appsvcenvdemo vore hello underdomännamn. *appsvcenvdemo.p.azurewebsites.net*. Om du har därför skapat en app med namnet *mytestapp* sedan det vore adresserbara på *mytestapp.appsvcenvdemo.p.azurewebsites.net*. Du kan inte använda blanksteg i hello namnet på din ASE. Om du använder tecken med versaler i hello namn blir hello domännamn hello totala gemen version av det här namnet. Om du använder en ILB sedan ASE-namnet används inte i din underdomän men i stället anges uttryckligen under skapande av ASE
   
    ![][1]
2. Välj din prenumeration. hello-prenumeration som används för din ASE är också hello något som alla program i den ASE kommer att skapas med. Du kan inte placera din ASE i ett virtuellt nätverk som tillhör en annan prenumeration
3. Välj eller ange en ny resursgrupp. hello resursgruppens namn används för din ASE måste hello samma som används för din VNet. Om du väljer en befintlig VNet kommer hello val av resursgrupp för ditt ASE ska vara uppdaterade tooreflect som ditt VNet.
   
    ![][2]
4. Gör dina val för virtuellt nätverk och plats. Du kan välja toocreate ett nytt virtuellt nätverk eller välj ett befintligt virtuellt nätverk. Om du väljer ett nytt virtuellt nätverk sedan kan du ange ett namn och plats. hello nya VNet har hello adressintervallet 192.168.250.0/23 och ett undernät med namnet **standard** som har definierats som 192.168.250.0/24. Du också välja en befintlig klassiska eller Resource Manager VNet. hello VIP val avgör om din ASE kan nås direkt från hello internet (externa) eller om den använder en intern belastning belastningsutjämnare (ILB). Mer om dem läsa toolearn [med en Apptjänst-miljö med en intern belastningsutjämnare][ILBASE]. Om du väljer en VIP-typ av externa kan du välja hur många externa IP-adresser hello systemet skapas med för IPSSL ändamål. Om du väljer internt måste toospecify hello underdomän som din ASE ska använda. ASEs kan distribueras till virtuella nätverk som använder *antingen* offentligt adressintervall *eller* RFC1918 adressutrymmen (d.v.s. privata adresser). I ordning toouse ett virtuellt nätverk med ett offentligt adressintervall behöver du toocreate hello VNet i förväg. När du väljer en befintlig VNet måste toocreate ett nytt undernät under skapande av ASE. **Du kan inte använda ett befintligt undernät i hello-portalen. Du kan skapa en ASE med en befintlig undernät om du skapar din ASE med hjälp av en resource manager-mall.** toocreate en ASE från en mall använda hello information här [att skapa en Apptjänst-miljö från mallen] [ ILBAseTemplate] och här [skapar en ILB Apptjänst-miljö från mall] [ASEfromTemplate].

### <a name="details"></a>Information
En ASE skapas med 2 främre slutar och 2 anställda. hello främre slutar fungera som hello HTTP/HTTPS-slutpunkter och skicka trafik toohello anställda som är hello roller som värd för dina appar. Du kan justera hello kvantitet efter ASE har skapats och kan även konfigurera automatiska regler för de här resurspoolerna. Mer information kring manuell skalning, hantering och övervakning av en Apptjänst-miljö finns här: [hur tooconfigure en Apptjänst-miljö][ASEConfig] 

Endast hello en ASE kan finnas i hello undernät som används av hello ASE. hello undernät kan inte användas för något annat än hello ASE

### <a name="after-app-service-environment-v1-creation"></a>Efter att Apptjänstmiljö v1 har skapats
Efter att ASE har skapats kan du justera:

* Antalet främre slutar (lägsta: 2)
* Antal av de anställda (lägsta: 2)
* Antal IP-adresser som är tillgängliga för IP-SSL
* Compute-resurs-storlekar som används av hello främre slutar eller arbetare (klientdel minsta storlek är P2)

Det finns mer information kring manuell skalning, hantering och övervakning av Apptjänstmiljöer här: [hur tooconfigure en Apptjänst-miljö][ASEConfig] 

Mer information om autoskalning är en handbok här: [hur tooconfigure Autoskala för en Apptjänstmiljö][ASEAutoscale]

Det finns ytterligare beroenden som inte är tillgängliga för anpassning till exempel hello databasen och lagring. Dessa hanteras av Azure och medföljer hello system. hello system storage stöder upp too500 GB för hello hela Apptjänst-miljön och hello databasen justeras av Azure vid behov genom att hello skalan för hello system.

## <a name="getting-started"></a>Komma igång
Alla artiklar och hur-att datorns för Apptjänstmiljöer finns tillgängliga i hello [viktigt för Programtjänstmiljöer](../app-service/app-service-app-service-environments-readme.md).

tooget igång med Apptjänst-miljö v1 finns [introduktion toohello Apptjänstmiljö v1][WhatisASE]

Mer information om hello Azure Apptjänst-plattformen finns [Azure App Service][AzureAppService].

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-basecreateblade.png
[2]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-vnetcreation.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
[ILBAseTemplate]: http://azure.microsoft.com/documentation/templates/201-web-app-ase-create/
[ASEfromTemplate]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-create-ilb-ase-resourcemanager/
