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
> <span data-ttu-id="69354-104">Det här är för att arbeta med DC/OS-baserade ACS-kluster.</span><span class="sxs-lookup"><span data-stu-id="69354-104">This is for working with DC/OS-based ACS clusters.</span></span> <span data-ttu-id="69354-105">Det finns inget behov av toodo detta för Swarm-baserade ACS-kluster.</span><span class="sxs-lookup"><span data-stu-id="69354-105">There is no need toodo this for Swarm-based ACS clusters.</span></span>
> 
> 

<span data-ttu-id="69354-106">Första, [ansluta tooyour DC/OS-baserade ACS-kluster](../articles/container-service/container-service-connect.md).</span><span class="sxs-lookup"><span data-stu-id="69354-106">First, [connect tooyour DC/OS-based ACS cluster](../articles/container-service/container-service-connect.md).</span></span> <span data-ttu-id="69354-107">När du har gjort det, kan du installera hello DC/OS CLI på din klientdator med hello-kommandona nedan:</span><span class="sxs-lookup"><span data-stu-id="69354-107">Once you have done this, you can install hello DC/OS CLI on your client machine with hello commands below:</span></span>

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

<span data-ttu-id="69354-108">Om du använder en äldre version av Python, kan du få en del "InsecurePlatformWarnings".</span><span class="sxs-lookup"><span data-stu-id="69354-108">If you are using an old version of Python, you may notice some "InsecurePlatformWarnings".</span></span> <span data-ttu-id="69354-109">Du kan ignorera dem.</span><span class="sxs-lookup"><span data-stu-id="69354-109">You can safely ignore these.</span></span>

<span data-ttu-id="69354-110">I ordning tooget igång utan att starta om gränssnittet, kör du:</span><span class="sxs-lookup"><span data-stu-id="69354-110">In order tooget started without restarting your shell, run:</span></span>

```bash
source ~/.bashrc
```

<span data-ttu-id="69354-111">Det här steget är inte nödvändigt när du startar nya gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="69354-111">This step will not be necessary when you start new shells.</span></span>

<span data-ttu-id="69354-112">Nu kan du bekräfta att hello CLI är installerad:</span><span class="sxs-lookup"><span data-stu-id="69354-112">Now you can confirm that hello CLI is installed:</span></span>

```bash
dcos --help
```