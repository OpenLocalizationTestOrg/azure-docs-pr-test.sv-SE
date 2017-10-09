---
title: "aaaDeploy din första app tooCloud Foundry på Microsoft Azure | Microsoft Docs"
description: "Distribuera ett program tooCloud Foundry på Azure"
services: virtual-machines-linux
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 8fa04a58-56ad-4e6c-bef4-d02c80d4b60f
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/14/2017
ms.author: seanmck
ms.openlocfilehash: 878da38f6eabe32a339f02aa0ead811d6e5af9a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-first-app-toocloud-foundry-on-microsoft-azure"></a>Distribuera din första app tooCloud Foundry på Microsoft Azure

[Molnet Foundry](http://cloudfoundry.org) är en programplattform för populära öppen källkod tillgängliga på Microsoft Azure. I den här artikeln visar vi hur toodeploy och hantera ett program på molnet Foundry i en Azure-miljö.

## <a name="create-a-cloud-foundry-environment"></a>Skapa en moln Foundry-miljö

Det finns flera alternativ för att skapa en miljö med molnet Foundry i Azure:

- Använd hello [spela en central molnet Foundry erbjudande] [ pcf-azuremarketplace] i hello Azure Marketplace toocreate en standard miljö som innehåller PCF Ops Manager och hello Azure Service Broker. Du kan hitta [fullständiga] [ pcf-azuremarketplace-pivotaldocs] för att distribuera hello marketplace erbjuder i hello spela en central dokumentation.
- Skapa en anpassad miljö genom [manuellt distribuera spela en central molnet Foundry][pcf-custom].
- [Distribuera hello öppen källkod molnet Foundry paket direkt] [ oss-cf-bosh] genom att ställa in en [BOSH](http://bosh.io) chef, en virtuell dator som samordnar hello distribution av hello molnet Foundry miljö.

> [!IMPORTANT] 
> Om du distribuerar PCF från hello Azure Marketplace anteckna hello SYSTEMDOMAINURL och hello administratörsautentiseringsuppgifter krävs tooaccess hello spela en central appar Manager, som beskrivs i distributionsguiden för hello marketplace. De är nödvändiga toocomplete den här kursen. Hej SYSTEMDOMAINURL är i hello formuläret https://system för marketplace-distributioner. *ip-adress*. cf.pcfazure.com.

## <a name="connect-toohello-cloud-controller"></a>Ansluta toohello moln-styrenhet

hello molnet domänkontrollant är hello primära post punkt tooa moln Foundry miljö för att distribuera och hantera program. hello core molnet Controller API (CCAPI) är en REST-API, men den är tillgänglig via olika verktyg. I det här fallet vi interagerar med det via hello [moln Foundry CLI][cf-cli]. Du kan installera hello CLI på Linux, MacOS eller Windows, men om du föredrar att inte tooinstall den, den är tillgänglig förinstallerat i hello [Azure Cloud Shell][cloudshell-docs].

toolog, lägga `api` toohello SYSTEMDOMAINURL som du fick från hello marketplace-distributionen. Eftersom hello standard distributionen använder ett självsignerat certifikat, bör du också inkludera hello `skip-ssl-validation` växla.

```bash
cf login -a https://api.SYSTEMDOMAINURL --skip-ssl-validation
```

Du kan ange toolog i toohello moln-styrenhet. Använd hello admin kontoautentiseringsuppgifter som du har köpt från hello marketplace-distributionsstegen.

Molnet Foundry ger *organisationer* och *blanksteg* som namnområden tooisolate hello team och miljöer i en delad distribution. Hej PCF marketplace distribution innehåller hello standard *system* org och en uppsättning blanksteg skapade toocontain hello baskomponenterna som hello autoskalning service och hello Azure service broker. Välj hello just nu *system* utrymme.


## <a name="create-an-org-and-space"></a>Skapa en organisations och utrymme

Om du skriver `cf apps`, visas en uppsättning systemprogram som har distribuerats i hello systemutrymme i hello system organisationen. 

Du bör ha hello *system* org som reserverats för systemprogram, så du måste skapa en organisations och utrymme toohouse våra exempelprogrammet.

```bash
cf create-org myorg
cf create-space dev -o myorg
```

Använd hello mål kommandot tooswitch toohello nya org och utrymme:

```bash
cf target -o testorg -s dev
```

Nu när du distribuerar ett program kan skapas det automatiskt i hello nya org och blanksteg. tooconfirm som det finns för närvarande inga appar i hello nya org/utrymme, Skriv `cf apps` igen.

> [!NOTE] 
> Mer information om organisationer blanksteg och hur de kan användas för rollbaserad åtkomstkontroll (RBAC) finns hello [moln Foundry dokumentationen][cf-orgs-spaces-docs].

## <a name="deploy-an-application"></a>Distribuera ett program

Nu ska vi använda ett exempelprogram för molntjänster Foundry kallas Hello källan moln, vilket är skriven i Java och baserat på hello [Vårversionen Framework](http://spring.io) och [Vårversionen Start](http://projects.spring.io/spring-boot/).

### <a name="clone-hello-hello-spring-cloud-repository"></a>Klona hello Hello källan molnet databasen

hello Hello källan molnet exempelprogrammet finns på GitHub. Klona den tooyour miljö och ändra till nya hello-katalogen:

```bash
git clone https://github.com/cloudfoundry-samples/hello-spring-cloud
cd hello-spring-cloud
```

### <a name="build-hello-application"></a>Skapa hello program

Build hello appen med [Apache Maven](http://maven.apache.org).

```bash
mvn clean package
```

### <a name="deploy-hello-application-with-cf-push"></a>Distribuera hello program med cf push

Du kan distribuera de flesta program tooCloud Foundry med hello `push` kommando:

```bash
cf push
```

När du *push* ett program, molnet Foundry identifierar hello typ av program (i det här fallet en Java-app) och identifierar dess beroenden (i det här fallet hello källan framework). Den sedan paket allt krävs toorun din kod i en fristående behållaren avbildning som kallas en *droplet*. Slutligen molnet Foundry scheman hello program på en av hello tillgängliga datorer i din miljö och skapar en URL där du kan nå den, som finns i hello hello kommandots utdata.

![Utdata från cf push-kommando][cf-push-output]

toosee hello hello källan moln program, öppna hello angivna URL: en i webbläsaren:

![Standardgränssnitt för Hello källan moln][hello-spring-cloud-basic]

> [!NOTE] 
> Mer om vad som händer under toolearn `cf push`, finns [hur program mellanlagras] [ cf-push-docs] i hello molnet Foundry-dokumentationen.

## <a name="view-application-logs"></a>Visa programloggar

Du kan använda hello molnet Foundry CLI tooview loggar för ett program med namnet:

```bash
cf logs hello-spring-cloud
```

Som standard loggas hello kommando använder *pilslut*, vilket visar nya loggar som de är skrivna. toosee nya loggar visas uppdatera hello hello-källan-cloud app i hello webbläsare.

tooview loggar som redan har skrivits, lägga till hello `recent` växel:

```bash
cf logs --recent hello-spring-cloud
```

## <a name="scale-hello-application"></a>Skala hello program

Som standard `cf push` skapar endast en instans av programmet. tooensure hög tillgänglighet och aktivera skala ut för högre genomströmning, vanligtvis du toorun mer än en instans av dina program. Du kan enkelt skala ut redan distribuerade program med hjälp av hello `scale` kommando:

```bash
cf scale -i 2 hello-spring-cloud
```

Körs hello `cf app` kommandot på hello program visar att molnet Foundry skapar en annan instans av programmet hello. När programmet hello har börjat startar automatiskt moln Foundry trafik tooit för belastningsutjämning.


## <a name="next-steps"></a>Nästa steg

- [Läs hello molnet Foundry-dokumentation][cloudfoundry-docs]
- [Konfigurera hello Visual Studio Team Services plugin-programmet för molnet Foundry][vsts-plugin]
- [Konfigurera hello Microsoft Log Analytics munstycket för molnet Foundry][loganalytics-nozzle]

<!-- LINKS -->

[pcf-azuremarketplace]: https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry
[pcf-custom]: https://docs.pivotal.io/pivotalcf/1-10/customizing/azure.html
[oss-cf-bosh]: https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs
[pcf-azuremarketplace-pivotaldocs]: https://docs.pivotal.io/pivotalcf/customizing/pcf_azure.html
[cf-cli]: https://github.com/cloudfoundry/cli
[cloudshell-docs]: https://docs.microsoft.com/azure/cloud-shell/overview
[cf-orgs-spaces-docs]: https://docs.cloudfoundry.org/concepts/roles.html
[spring-boot]: https://projects.spring.io/spring-boot/
[spring-framework]: http://spring.io
[cf-push-docs]: https://docs.cloudfoundry.org/concepts/how-applications-are-staged.html
[cloudfoundry-docs]: https://docs.cloudfoundry.org
[vsts-plugin]: https://github.com/Microsoft/vsts-cloudfoundry
[loganalytics-nozzle]: https://github.com/Azure/oms-log-analytics-firehose-nozzle

<!-- IMAGES -->
[cf-push-output]: ./media/cloudfoundry-deploy-your-first-app/cf-push-output.png
[hello-spring-cloud-basic]: ./media/cloudfoundry-deploy-your-first-app/hello-spring-cloud-basic.png