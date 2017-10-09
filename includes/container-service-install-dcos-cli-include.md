---
title: aaaInstall hello DC/OS CLI | Microsoft Docs
description: Installera hello DC/OS CLI.
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Behållare, Micro-tjänster, DC/OS, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/10/2016
ms.author: rogardle
ms.openlocfilehash: b077c05beff9a5638486ea5efe9df31089e32701
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
> [!NOTE]
> Det här är för att arbeta med DC/OS-baserade ACS-kluster. Det finns inget behov av toodo detta för Swarm-baserade ACS-kluster.
> 
> 

Första, [ansluta tooyour DC/OS-baserade ACS-kluster](../articles/container-service/container-service-connect.md). När du har gjort det, kan du installera hello DC/OS CLI på din klientdator med hello-kommandona nedan:

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

Om du använder en äldre version av Python, kan du få en del "InsecurePlatformWarnings". Du kan ignorera dem.

I ordning tooget igång utan att starta om gränssnittet, kör du:

```bash
source ~/.bashrc
```

Det här steget är inte nödvändigt när du startar nya gränssnitt.

Nu kan du bekräfta att hello CLI är installerad:

```bash
dcos --help
```