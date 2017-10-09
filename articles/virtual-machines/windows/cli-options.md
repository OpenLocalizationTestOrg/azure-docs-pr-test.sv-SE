---
title: "aaaUsing hello Azure CLI på Windows | Microsoft Docs"
description: "Med hjälp av hello Azure CLI i Windows"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/14/2017
ms.author: nepeters
ms.openlocfilehash: edca4a2701a7dfc0fc54df5649e8a5e7afc95490
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-cli-on-windows"></a>Med hjälp av hello Azure CLI i Windows

hello Azure-kommandoradsgränssnittet (CLI) tillhandahåller en kommandorad och skriptmiljö för att skapa och hantera Azure-resurser. hello Azure CLI är tillgänglig för macOS, Linux och Windows-operativsystem. I dessa operativsystem kan hello CLI-kommandona är identiska, men operativsystemet specifika skriptsyntax kan skilja sig.

Det här dokumentet information hello sätt hello Azure CLI kan installeras och körs på Windows och information för varje syntaktiska överväganden. Djupgående Azure CLI-dokumentationen finns [Azure CLI dokumentationen]( https://docs.microsoft.com/en-us/cli/azure/overview).

## <a name="windows-subsystem-for-linux"></a>Windows-undersystem för Linux

hello Windows undersystem för Linux (WSL) tillhandahåller en Ubuntu Linux-miljö på årsdagar för Windows 10 och senare versioner. När aktiverat tillhandahåller WSL en inbyggd Bash-upplevelse, som kan användas för att skapa och köra Azure CLI-skript. Eftersom WSL tillhandahåller en inbyggd Bash, kan Azure CLI-skript delas mellan macOS, Linux och Windows utan modifiering.

toouse hello Azure CLI i WSL, Slutför följande hello.

|Aktivitet | Instruktioner |
|---|---|
| Aktivera WSL | [Installera WSL dokumentation](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide) |
| Installera hello Azure CLI |[Installera hello CLI på WSL/Ubuntu 14.04](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2#ubuntu)|

## <a name="powershell"></a>PowerShell

hello Azure CLI kan köras inbyggt i Windows. I den här konfigurationen hello Azure CLI-paketet är installerat på hello Windows-operativsystem och kommandon kan köras från PowerShell. I den här konfigurationen, kan Azure CLI-kommandon och skript köras på någon version av Windows, men det krävs specifika skriptsyntax för plattformen. Därför kan skript inte nödvändigtvis delas mellan macOS, Linux och Windows utan modifiering.

toouse hello Azure CLI i Windows, installationspaket hello använder dessa instruktioner [installera hello CLI på Windows](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2#windows).

## <a name="docker-image"></a>Docker-bild

När du använder Docker för Windows, kan en Docker-avbildning starta som innehåller hello Azure CLI. Den här avbildningen baseras på Linux och innehåller en inbyggd Bash-upplevelse.  När du använder Docker för Windows och hello Azure CLI bild, skript toobe delas mellan macOS, Linux och Windows. 

toouse hello Azure CLI på Docker för Windows, se till att Docker för Windows körs och kör följande kommando hello.

```bash
docker run -it azuresdk/azure-cli-python:latest bash
```

När slutfört, ett Bash-sessionen startas som förinstallerats hello Azure CLI-verktygen.

## <a name="next-steps"></a>Nästa steg

[CLI-exempel för virtuella Azure-datorer](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[CLI prover för Azure-Webbappar](../../app-service-web/app-service-cli-samples.md)

[CLI prover för Azure SQL](../../sql-database/sql-database-cli-samples.md)
