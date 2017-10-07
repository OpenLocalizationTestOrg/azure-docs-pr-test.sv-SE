---
title: "aaaGeo distribuerade skala med Apptjänstmiljöer"
description: "Lär dig hur toohorizontally skala appar med geo-distribution med Traffic Manager och App-miljöer."
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: c1b05ca8-3703-4d87-a9ae-819d741787fb
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2016
ms.author: stefsch
ms.openlocfilehash: 9b441f637d8b7f679b3d83240baf99b8ee57e8f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="geo-distributed-scale-with-app-service-environments"></a>Geodistribuerad skalning med App Service Environment
## <a name="overview"></a>Översikt
Programscenarier som kräver storskalighet kan överskrida hello beräkning resurs kapacitet tillgängliga tooa distribution av en app.  Röstning program, är sport händelser och TV underhållning händelser alla exempel på scenarier som kräver mycket hög skala. Hög skalning kraven kan uppfyllas genom att skala ut appar, vågrätt med flera appdistributioner görs inom en enskild region, samt över regioner, toohandle extrema belastningskraven.

Apptjänstmiljöer finns en perfekt plattform för vågrät skala ut.  När en Apptjänst-miljö konfiguration har valts som kan stödja en känd förfrågningar kan utvecklare distribuera ytterligare Apptjänstmiljöer i ”kakmall” sätt tooattain en önskad belastning belastningen kapacitet.

Anta till exempel att en app som körs på en Apptjänstmiljö konfiguration har testats toohandle 20K begäranden per sekund (RPS).  Om hello önskade belastning läser in kapacitet är 100K RPS, Apptjänstmiljöer fem (5) kan skapas och konfigurerade tooensure hello-programmet kan hantera hello största planerade belastning.

Eftersom kunder normalt kommer åt appar som använder en anpassad (eller alternativa)-domän, utvecklare behöver begär en sätt toodistribute app för alla instanser av hello Apptjänst-miljö.  Ett bra sätt tooaccomplish är tooresolve hello en anpassad domän med hjälp av en [Azure Traffic Manager-profilen][AzureTrafficManagerProfile].  hello Traffic Manager-profil kan vara konfigurerade toopoint på alla hello enskilda Apptjänstmiljöer.  Traffic Manager hanterar automatiskt distribuerar kunder för alla hello Apptjänstmiljöer baserat på hello inställningar i hello Traffic Manager-profil för belastningsutjämning.  Den här metoden fungerar oavsett om alla hello Apptjänstmiljöer är distribuerat i en enda Azure region eller distribueras i hela världen över flera Azure-regioner.

Eftersom kunder åtkomst till appar via hello alternativa domän, inte dessutom kunder känner till hello antalet Apptjänstmiljöer som kör en app.  Därför kan utvecklare snabbt och enkelt lägga till och ta bort Apptjänstmiljöer baserat på observerade trafikbelastningen.

hello konceptuella diagrammet nedan visar en app vågrätt skala ut över tre Apptjänstmiljöer inom en enskild region.

![Konceptuell arkitektur][ConceptualArchitecture] 

hello resten av det här avsnittet går igenom hello tillvägagångssättet med att konfigurera en distribuerad topologi för hello sample-appen med hjälp av flera Apptjänstmiljöer.

## <a name="planning-hello-topology"></a>Planera hello topologi
Innan du bygga ut en distribuerad app storleken hjälper toohave några delar information i förväg.

* **Anpassad domän för hello app:** vad är hello domännamn kunder kommer att använda tooaccess hello app?  Hello exempel hello anpassade tillämpningsdomän är namn *www.scalableasedemo.com*
* **Traffic Manager-domän:** ett domännamn måste toobe valt när du skapar en [Azure Traffic Manager-profilen][AzureTrafficManagerProfile].  Det här namnet kommer att kombineras med hello *trafficmanager.net* tooregister en post för domänen som hanteras av Traffic Manager-suffix.  Hej exempelapp hello namn som väljs är *skalbara ase demo*.  Därför hello fullständiga domännamn som hanteras av Traffic Manager är *skalbara ase demo.trafficmanager.net*.
* **Strategin för skalning hello app storleken:** hello programmet storleken distribueras över flera Apptjänstmiljöer i en region?  Flera regioner?  En blanda och matcha-med båda metoderna?  hello beslut ska baseras på förväntningar där kunden trafik kommer kommer, samt hur väl hello resten av en app stöd för backend-infrastruktur kan skala.  Till exempel med ett 100% tillståndslösa program, kan en app massivt skalas med en kombination av flera Apptjänstmiljöer per Azure-region, multiplicerat med Apptjänstmiljöer distribution över flera Azure-regioner.  Med 15 + offentliga Azure-regioner tillgängliga toochoose från bygga kunder verkligen ett globalt storskaliga program storleken.  För hello sample-appen används för den här artikeln, har tre Apptjänstmiljöer skapats i en enda Azure region (södra centrala USA).
* **Namnkonventionen för hello Apptjänstmiljöer:** varje Apptjänst-miljön kräver ett unikt namn.  Utöver en eller två Apptjänstmiljöer är det bra toohave en namngivningskonvention toohelp identifiera varje Apptjänst-miljö.  En enkel namngivningskonvention användes för hello sample-appen.  hello namnen på hello tre Apptjänstmiljöer är *fe1ase*, *fe2ase*, och *fe3ase*.
* **Namnkonventionen för hello appar:** eftersom flera instanser av hello appen ska distribueras, krävs ett namn för varje instans av hello distribuerade appen.  En mindre kända men praktiskt funktion i Apptjänstmiljöer är att hello samma appnamn kan användas på flera Apptjänstmiljöer.  Eftersom varje Apptjänstmiljö har ett unikt domänsuffix, kan utvecklare välja toore Använd hello exakt samma appnamn i varje miljö.  En utvecklare kan till exempel ha appar med namnet på följande sätt: *myapp.foo1.p.azurewebsites.net*, *myapp.foo2.p.azurewebsites.net*, *myapp.foo3.p.azurewebsites.net*osv.  För hello exempelapp även om varje app-instansen har även ett unikt namn.  hello app instansnamn som används är *webfrontend1*, *webfrontend2*, och *webfrontend3*.

## <a name="setting-up-hello-traffic-manager-profile"></a>Konfigurera hello Traffic Manager-profilen
När flera instanser av en app distribueras på flera Apptjänstmiljöer kan hello enskilda app-instanser registreras med Traffic Manager.  För hello sample-appen en Traffic Manager profil krävs för *skalbara ase demo.trafficmanager.net* som kan vidarebefordra kunder tooany av hello följa distribuerade app-instanser:

* **webfrontend1.fe1ase.p.azurewebsites.NET:** en instans av hello sample-appen har distribuerats på hello första Apptjänstmiljö.
* **webfrontend2.fe2ase.p.azurewebsites.NET:** en instans av hello sample-appen har distribuerats på hello andra Apptjänst-miljö.
* **webfrontend3.fe3ase.p.azurewebsites.NET:** en instans av hello sample-appen har distribuerats på hello tredje Apptjänstmiljö.

Hej enklaste sättet tooregister flera Azure App Service-slutpunkter kan alla körs i hello **samma** Azure-regionen är med hello Powershell [stöd för Azure Resource Manager Traffic Manager] [ ARMTrafficManager].  

hello första steget är toocreate en Azure Traffic Manager-profil.  hello koden nedan visar hur hello profilen har skapats för hello sample-appen:

    $profile = New-AzureTrafficManagerProfile –Name scalableasedemo -ResourceGroupName yourRGNameHere -TrafficRoutingMethod Weighted -RelativeDnsName scalable-ase-demo -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"

Observera hur hello *RelativeDnsName* parameter har angetts för*skalbara ase demo*.  Detta är hur hello domännamn *skalbara ase demo.trafficmanager.net* som associeras med en Traffic Manager-profil.

Hej *TrafficRoutingMethod* parametern definierar hello princip för Traffic Manager kommer att använda toodetermine hur toospread kundbelastning över alla tillgängliga hello-slutpunkter för belastningsutjämning.  I det här exemplet hello *viktat* har valt metoden.  Detta resulterar i kundernas önskemål som sprids hello registrerade programmet slutpunkterna baserat på hello relativa vikten som är associerade med varje slutpunkt. 

Med hello profilen har skapats, varje app-instansen till toohello profil som en intern Azure-slutpunkt.  hello koden nedan hämtar en referens tooeach frontend-webbprogram och lägger sedan till varje app som en Traffic Manager-slutpunkt som hello *TargetResourceId* parameter.

    $webapp1 = Get-AzureRMWebApp -Name webfrontend1
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend1 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp1.Id –EndpointStatus Enabled –Weight 10

    $webapp2 = Get-AzureRMWebApp -Name webfrontend2
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend2 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp2.Id –EndpointStatus Enabled –Weight 10

    $webapp3 = Get-AzureRMWebApp -Name webfrontend3
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend3 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp3.Id –EndpointStatus Enabled –Weight 10

    Set-AzureTrafficManagerProfile –TrafficManagerProfile $profile

Observera hur det är ett anrop för*Lägg till AzureTrafficManagerEndpointConfig* för varje enskild app-instansen.  Hej *TargetResourceId* i varje Powershell-kommando refererar till ett av hello tre distribuerad app instanser.  hello Traffic Manager-profilen ska spridas belastningen för alla tre slutpunkterna som registrerats i hello profilen.

Alla tre slutpunkterna använda hello hello samma värde (10) för hello *vikt* parameter.  Detta resulterar i Traffic Manager sprids kundönskemål i alla tre app instanser jämnt. 

## <a name="pointing-hello-apps-custom-domain-at-hello-traffic-manager-domain"></a>Pekar hello App anpassad domän på hello Traffic Manager-domän
hello sista steget behövs är toopoint hello domänen hello app på hello Traffic Manager-domän.  För hello exempelapp innebär detta att peka *www.scalableasedemo.com* på *skalbara ase demo.trafficmanager.net*.  Det här steget måste toobe slutfördes med hello domänregistrator som hanterar hello anpassade domäner.  

Använda din registrator domän hanteringsverktyg, skapa en CNAME-poster måste toobe domänen som pekar hello på hello Traffic Manager-domän.  hello bilden nedan visas ett exempel på hur den här CNAME-konfigurationen ser ut:

![CNAME-post för den anpassade domänen][CNAMEforCustomDomain] 

Kom ihåg att varje enskild app-instansen måste toohave hello domänen registrerats med den samt men som inte omfattas i det här avsnittet.  Om en begäran gör det tooan app-instansen och hello programmet saknar hello domänen som är registrerade med hello app, annars misslyckas hello begäran.  

I det här exemplet hello domänen är *www.scalableasedemo.com*, och varje instans av programmet har hello anpassade domäner som är kopplade till den.

![Egen domän][CustomDomain] 

En sammanfattning av registreringen av en anpassad domän med Azure Apptjänst-appar finns i följande artikel hello [registrera anpassade domäner][RegisterCustomDomain].

## <a name="trying-out-hello-distributed-topology"></a>Testar hello distribuerad topologi
hello slutresultatet av hello Traffic Manager- och DNS är som begär för *www.scalableasedemo.com* flödar via hello följande ordning:

1. En webbläsare eller en enhet blir en DNS-sökning *www.scalableasedemo.com*
2. hello CNAME-post hos hello domänregistrator gör hello DNS-sökning toobe omdirigeras tooAzure Traffic Manager.
3. En DNS-sökning görs för *skalbara ase demo.trafficmanager.net* mot en hello Azure Traffic Manager DNS-servrar.
4. Baserat på hello princip för belastningsutjämning (hello *TrafficRoutingMethod* parameter används tidigare när du skapar hello Traffic Manager-profil), Traffic Manager kommer väljer ett av hello konfigurerade slutpunkter och returnera hello FQDN för som slutpunkten toohello webbläsare och enheter.
5. Eftersom hello FQDN för hello endpoint hello-Url för en app-instans som körs på en Apptjänst-miljö, hello webbläsare och enheter kommer att fråga en Microsoft Azure DNS-server tooresolve hello FQDN tooan IP-adress. 
6. hello webbläsare och enheter skickar hello HTTP/S begäran toohello IP-adress.  
7. hello begäran anländer till en hello app-instanser som körs på en av hello Apptjänstmiljöer.

hello konsolen bilden nedan visas en DNS-sökning för hello exempel appens anpassade domäner har lösa tooan app-instansen körs på en av hello tre exempel Apptjänstmiljöer (i det här fallet hello hello tre Apptjänstmiljöer sekund):

![DNS-sökning][DNSLookup] 

## <a name="additional-links-and-information"></a>Information och ytterligare länkar
Alla artiklar och hur-att datorns för Apptjänstmiljöer finns tillgängliga i hello [viktigt för Programtjänstmiljöer](../app-service/app-service-app-service-environments-readme.md).

Dokumentation om hello Powershell [stöd för Azure Resource Manager Traffic Manager][ARMTrafficManager].  

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[AzureTrafficManagerProfile]: ../traffic-manager/traffic-manager-manage-profiles.md
[ARMTrafficManager]: ../traffic-manager/traffic-manager-powershell-arm.md
[RegisterCustomDomain]: app-service-web-tutorial-custom-domain.md


<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-geo-distributed-scale/ConceptualArchitecture-1.png
[CNAMEforCustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CNAMECustomDomain-1.png
[DNSLookup]:  ./media/app-service-app-service-environment-geo-distributed-scale/DNSLookup-1.png
[CustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CustomDomain-1.png 
