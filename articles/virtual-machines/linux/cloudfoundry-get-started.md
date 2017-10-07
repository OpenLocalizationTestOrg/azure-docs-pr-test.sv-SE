---
title: "aaaGetting igång med Cloud Foundry på Microsoft Azure | Microsoft Docs"
description: "Kör OSS eller spela en central molnet Foundry på Microsoft Azure"
services: virtual-machines-linux
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 2a15ffbf-9f86-41e4-b75b-eb44c1a2a7ab
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 01/19/2017
ms.author: seanmck
ms.openlocfilehash: 56300d5a0a75b5a9f46145a49ed3f5daa774375a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-foundry-on-azure"></a>Cloud Foundry på Azure

Molnet Foundry är en öppen källkod plattform som en-tjänst (PaaS) för att skapa, distribuera och använda 12-factor-program som utvecklats i olika språk och ramverk. Det här dokumentet beskriver hello alternativ som du kör molnet Foundry Azure och hur du kan komma igång.

## <a name="cloud-foundry-offerings"></a>Foundry molntjänster

Det finns två typer av molnet Foundry tillgängliga toorun i Azure: öppen källkod molnet Foundry (OSS CF) och spela en central molnet Foundry (PCF). OSS CF är en helt [öppen källkod](https://github.com/cloudfoundry) version av Cloud Foundry hanteras av hello molnet Foundry Foundation. Spela en central molnet Foundry är en enterprise-distribution för molnet från spela en central Software Inc. Vi titta på några av hello skillnaderna mellan två hello-erbjudanden.

### <a name="open-source-cloud-foundry"></a>Öppen källkod molnet Foundry

Du kan distribuera OSS molnet Foundry i Azure genom att först distribuera en BOSH director och distribuera moln Foundry, med hjälp av hello [instruktionerna på GitHub](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md). toolearn mer information om hur du använder OSS CF finns hello [dokumentationen](https://docs.cloudfoundry.org/) tillhandahålls av hello molnet Foundry Foundation.

Microsoft ger bästa stöd för OSS CF via hello följande community kanaler:

- #<a name="bosh-azure-cpi-channel-on-cloud-foundry-slackhttpsslackcloudfoundryorg"></a>bosh-azure-cpi kanal på [moln Foundry Slack](https://slack.cloudfoundry.org/)
- [CF bosh postlistan](https://lists.cloudfoundry.org/pipermail/cf-bosh)
- GitHub problem för hello [CPI](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/issues) och [service broker](https://github.com/Azure/meta-azure-service-broker/issues)

>[!NOTE]
> hello supportnivå för din Azure-resurser, till exempel hello virtuella datorer där du kör molnet Foundry baseras på ditt Azure-supportavtal. Bästa community-support gäller bara toohello moln Foundry-specifika komponenter.

### <a name="pivotal-cloud-foundry"></a>Spela en central molnet Foundry

Spela en central molnet Foundry innehåller hello samma kärnplattformen som hello OSS distribution, tillsammans med en uppsättning egna hanteringsverktyg och enterprise-stöd. Du måste skaffa en licens från Pivotal toorun PCF på Azure. Hej PCF erbjudande från hello Azure marketplace innehåller en 90-dagars testversion.

hello verktygen innehåller [spela en central Operations Manager](http://docs.pivotal.io/pivotalcf/customizing/), ett webbprogram som förenklar distribution och hantering av ett moln Foundry foundation och [spela en central appar Manager](https://docs.pivotal.io/pivotalcf/console/), ett webbprogram för att hantera användare och program.

Dessutom toohello support-kanaler för OSS CF ovan, en PCF-licens kan du toocontact Pivotal för support. Microsoft och Pivotal också har aktiverat stöd för arbetsflöden som gör att du toocontact antingen part för att få hjälp och har din förfrågan dirigeras korrekt beroende på var hello problemet ligger.

## <a name="azure-service-broker"></a>Azure Service Broker

Molnet Foundry uppmuntrar hello [”tolv faktor app”](https://12factor.net/) metod som främjar en ren avgränsning av tillståndslösa programprocesser och stateful säkerhetskopiering tjänster. [Tjänsten mäklare](https://docs.cloudfoundry.org/services/api.html) erbjuder ett konsekvent sätt tooprovision och binda stödjande services tooapplications. Hej [Azure service broker](https://github.com/Azure/meta-azure-service-broker) innehåller några av hello viktiga Azure-tjänster genom den här kanalen, inklusive Azure storage och SQL Azure.

Om du använder spela en central molnet Foundry hello service broker är också [tillgänglig som en panel](https://docs.pivotal.io/azure-sb/installing.html) från hello spela en central nätverk.

## <a name="related-resources"></a>Relaterade resurser

### <a name="visual-studio-team-services-plugin"></a>Visual Studio Team Services plugin-program

Molnet Foundry är väl lämpade tooagile programvaruutveckling, inklusive hello användning av kontinuerlig integration (KO) och kontinuerlig leverans (CD). Om du använder Visual Studio Team Services VSTS () toomanage projekt och vill tooset upp en CI/CD-pipeline mål moln Foundry, kan du använda hello [VSTS molnet Foundry build-tillägget](https://marketplace.visualstudio.com/items?itemName=ms-vsts.cloud-foundry-build-extension). hello plugin-programmet gör det enkelt tooconfigure och automatisera distributioner tooCloud Foundry, oavsett om körs i Azure eller en annan miljö.

## <a name="next-steps"></a>Nästa steg

- [Distribuera spela en central molnet Foundry från hello Azure Marketplace](https://azure.microsoft.com/en-us/marketplace/partners/pivotal/pivotal-cloud-foundryazure-pcf/)
- [Distribuera en app tooCloud Foundry i Azure](./cloudfoundry-deploy-your-first-app.md)
