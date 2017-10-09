---
title: "aaaApplication eller användarspecifik Marathon-tjänst | Microsoft Docs"
description: "Skapa en program- eller användarspecifik Marathon-tjänst"
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Behållare, Marathon, Micro-tjänster, DC/OS, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/12/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 1e6f69ed64e113a3a059788a71ddb57b6d3ad8da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-or-user-specific-marathon-service"></a>Skapa en program- eller användarspecifik Marathon-tjänst
Genom Azure Container Service tillhandahålls en uppsättning huvudservrar där Apache Mesos och Marathon förkonfigureras. Det kan vara används tooorchestrate dina program på hello kluster, men det är bästa inte toouse hello huvudservrarna för det här ändamålet. Till exempel modifiera hello konfigurationen av Marathon kräver logga in på hello huvudservrar sig själva och göra ändringar--detta uppmanar unika huvudservrar som är lite annorlunda från hello som standard och måste toobe få och hanterade oberoende av varandra. Dessutom kanske inte hello-konfiguration som krävs av ett team hello bästa konfigurationen för ett annat team.

I den här artikeln förklarar vi hur tooadd en program- eller användarspecifik Marathon-tjänst.

Eftersom den här tjänsten kommer att tillhöra tooa användare eller grupp, de är kostnadsfria tooconfigure den på något sätt som de önskar. Dessutom säkerställer Azure Container Service att hello tjänsten fortsätter toorun. Om det inte går att hello-tjänsten, Azure Container Service startar om den åt dig. De flesta hello tid märker du inte ens det varit driftstopp.

## <a name="prerequisites"></a>Krav
[Distribuera en instans av Azure Container Service](container-service-deployment.md) orchestrator Skriv DC/OS och [se till att klienten kan ansluta tooyour klustret](../container-service-connect.md). Dessutom hello följande steg.

[!INCLUDE [install hello DC/OS CLI](../../../includes/container-service-install-dcos-cli-include.md)]

## <a name="create-an-application-or-user-specific-marathon-service"></a>Skapa en program- eller användarspecifik Marathon-tjänst
Börja med att skapa en JSON-konfigurationsfil som definierar hello programtjänsten som du vill toocreate hello namn. Vi använder här `marathon-alice` som hello framework namn. Spara hello-fil med något som liknar `marathon-alice.json`:

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

Använd sedan hello DC/OS CLI tooinstall hello Marathon-instansen med hello-alternativ som har angetts i konfigurationsfilen:

```bash
dcos package install --options=marathon-alice.json marathon
```

Du bör nu se ditt `marathon-alice` tjänsten som körs i hello tjänstefliken i DC/OS-Gränssnittet. hello Gränssnittet är `http://<hostname>/service/marathon-alice/` om du vill tooaccess den direkt.

## <a name="set-hello-dcos-cli-tooaccess-hello-service"></a>Ange hello DC/OS CLI tooaccess hello-tjänsten
Du kan också konfigurera den här tjänsten för DC/OS CLI-tooaccess genom att ange hello `marathon.url` egenskapen toopoint toohello `marathon-alice` instans enligt följande:

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

Du kan kontrollera vilken instans av Marathon som CLI fungerar med hello `dcos config show` kommando. Du kan återställa toousing Marathon-huvudtjänsten med kommandot hello `dcos config unset marathon.url`.

