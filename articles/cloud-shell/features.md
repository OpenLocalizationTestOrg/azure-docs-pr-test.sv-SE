---
title: "funktioner för aaaAzure moln Shell (förhandsversion) | Microsoft Docs"
description: "Översikt över funktioner i Azure Cloud Shell"
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: juluk
ms.openlocfilehash: 65482ca6caeac01dda18a6b12eabe943e3d68a96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="features-and-tools-for-azure-cloud-shell"></a>Funktioner och verktyg för Azure-molnet Shell
Azure Cloud-gränssnittet är ett webbaserat gränssnitt upplevelse toomanage och utveckla Azure-resurser.

Molnet Shell erbjuder en webbläsare tillgängligt, förkonfigurerade shell-upplevelse för att hantera Azure-resurser utan hello arbetet med att installera, versionshantering, och underhålla en dator själv.

Molnet Shell etablerar datorer på grundval av per begäran och därför datortillståndet kommer inte att spara mellan sessioner. Eftersom moln-gränssnittet är utformat för interaktiva sessioner, avsluta tankar automatiskt efter 20 minuters inaktivitet shell.

## <a name="bash-in-cloud-shell"></a>Bash i molnet Shell
### <a name="tools"></a>Verktyg
|Kategori   |Namn   |
|---|---|
|Shell-tolken för Linux|Bash<br> del               |
|Azure-verktyg            |[Azure CLI 2.0](https://github.com/Azure/azure-cli) och [1.0](https://github.com/Azure/azure-xplat-cli)<br> [AzCopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy)<br> [Batch Shipyard](https://github.com/Azure/batch-shipyard)     |
|Textredigerare           |VIM<br> nano<br> emacs       |
|Källkontrollen         |Git                    |
|Skapa verktyg            |Kontrollera<br> maven<br> npm<br> PIP         |
|Behållare             |[Docker CLI](https://github.com/docker/cli)/[Docker-dator](https://github.com/docker/machine)<br> [Kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/)<br> [Utkast](https://github.com/Azure/draft)<br> [DC/OS CLI](https://github.com/dcos/dcos-cli)         |
|Databaser              |MySQL-klient<br> PostgreSql-klient<br> [SQLCMD-verktyget](https://docs.microsoft.com/sql/tools/sqlcmd-utility)<br> [MSSQL-scripter](https://github.com/Microsoft/sql-xplat-cli) |
|Annat                  |iPython klienten<br> [Molnet Foundry CLI](https://github.com/cloudfoundry/cli)<br> |

### <a name="language-support"></a>Språkstöd
|Språk   |Version   |
|---|---|
|.NET       |1.01       |
|Go         |1.7        |
|Java       |1.8        |
|Node.js    |6.9.4      |
|Python     |2.7 och 3.5 (standard)|

## <a name="secure-automatic-authentication"></a>Säker automatisk autentisering
Moln-gränssnittet autentiserar på ett säkert sätt och automatiskt kontoåtkomst för hello Azure CLI 2.0.

## <a name="azure-files-persistence"></a>Azure filer beständiga
Eftersom molnet Shell fördelas på grundval av per begäran med hjälp av en temporär dator är filer utanför din $Home och datorn tillstånd inte beständiga mellan sessioner.
toopersist filer mellan sessioner, molnet Shell får du genom att bifoga en fil som Azure dela startas för första.
När du är klar molnet Shell koppla automatiskt lagringen för alla framtida sessioner.

[Mer information om hur du kopplar Azure file resurser tooCloud Shell.](persisting-shell-storage.md)

## <a name="next-steps"></a>Nästa steg
[Molnet Shell Snabbstart](quickstart.md) <br>
[Lär dig mer om Azure CLI 2.0](https://docs.microsoft.com/cli/azure/) <br>