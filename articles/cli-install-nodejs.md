---
title: aaaInstall hello Azure CLI 1.0 | Microsoft Docs
description: "Installera hello Azure CLI 1.0 för Mac, Linux och Windows toostart med hjälp av Azure-tjänster"
editor: 
manager: timlt
documentationcenter: 
author: squillace
services: virtual-machines-linux,virtual-network,storage,azure-resource-manager
tags: azure-resource-manager,azure-service-management
ms.assetid: bdb776c8-7a76-4f3a-887c-236b4fffee10
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: command-line-interface
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: rasquill
ms.openlocfilehash: a8cd4e38fde6e4b17a768a7caecd280cd91a70f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-hello-azure-cli-10"></a>Installera hello Azure CLI 1.0
> [!div class="op_single_selector"]
> * [PowerShell](/powershell/azure/overview)
> * [Azure CLI 1.0](cli-install-nodejs.md)
> * [Azure CLI 2.0](/cli/azure/install-azure-cli)

> [!IMPORTANT]
> Det här avsnittet beskrivs hur tooinstall hello Azure CLI version 1.0, som bygger på nodeJs och stöder alla klassisk distribution API-anrop samt ett stort antal aktiviteter för Resource Manager distribution. Du bör använda hello [Azure CLI 2.0](/cli/azure/overview) för nya eller framtida CLI-distribution och hantering.

Snabbt installera hello Azure-kommandoradsgränssnittet (Azure CLI 1.0) toouse öppen källkod shell-baserade kommandon för att skapa och hantera resurser i Microsoft Azure. Har du flera alternativ tooinstall verktygen plattformsoberoende på datorn:

* **npm paketet** – kör npm (hello package manager för JavaScript) tooinstall hello senaste Azure CLI 1.0-paket på din distribution av Linux eller OS. Kräver node.js och npm på datorn.
* **Installer** -hämta ett installationsprogram för enkel installation på Mac- eller Windows.
* **Dockerbehållare** – börja använda hello senaste CLI i en klar och kör dockerbehållare. Kräver Docker värden på datorn.

Fler alternativ och bakgrunden finns hello projektet databasen på [GitHub](https://github.com/azure/azure-xplat-cli).

När du har installerat hello Azure CLI 1.0 [ansluta till din Azure-prenumeration](xplat-cli-connect.md) och kör hello **azure** kommandon från din kommandoradsgränssnittet (Bash, Terminal, Kommandotolken och så vidare) toowork med Azure-resurser.

## <a name="option-1-install-an-npm-package"></a>Alternativ 1: Installera npm-paket
tooinstall hello CLI från ett npm-paket, kontrollera att du har hämtat och installerat hello [senaste Node.js och npm](https://nodejs.org/en/download/package-manager/). Kör sedan **installera npm** tooinstall hello azure cli paketet:

```bash
npm install -g azure-cli
```

På Linux-distributioner måste du kanske toouse **sudo** toosuccessfully kör hello **npm** kommandot på följande sätt:

```bash
sudo npm install -g azure-cli
```

> [!NOTE]
> Om du behöver tooinstall eller uppdatera Node.js och npm på Linux-distribution eller OS, rekommenderar vi att du installerar hello senaste Node.js LTS versionen (4.x). Om du använder en äldre version, kan du hämta installationsfel.

Om du vill hämta hello senaste Linux [tar-filen] [ linux-installer] hello npm paketet lokalt. Installera sedan hello hämtade npm-paketet enligt följande (på Linux-distributioner måste du kanske toouse **sudo**):

```bash
npm install -g <path toodownloaded tar file>
```

## <a name="option-2-use-an-installer"></a>Alternativ 2: Använda ett installationsprogram
Om du använder en dator med Mac- eller Windows kan hello följande CLI installationsprogram hämtas:

* [Mac OS X-installationsprogrammet][mac-installer]
* [Windows MSI][windows-installer]

> [!TIP]
> I Windows, kan du också hämta hello [installationsprogram för webbplattform](https://go.microsoft.com/?linkid=9828653) tooinstall hello CLI. Installationsprogrammet kan du hello alternativet tooinstall ytterligare Azure SDK och kommandoradsverktygen när du har installerat hello CLI.

## <a name="option-3-use-a-docker-container"></a>Alternativ 3: Använda en dockerbehållare
Om du har konfigurerat datorn som en [Docker](https://docs.docker.com/engine/understanding-docker/) värden som du kan köra hello senaste Azure CLI 1.0 i en dockerbehållare. Kör hello följande kommando (på Linux-distributioner måste du kanske toouse **sudo**):

```bash
docker run -it microsoft/azure-cli
```

## <a name="run-azure-cli-10-commands"></a>Kör kommandon för Azure CLI 1.0
När du har installerat hello Azure CLI 1.0 kör hello **azure** från kommandoraden användargränssnittet (Bash, Terminal, Kommandotolken och så vidare). Till exempel toorun hello hjälpkommando skriver du hello följande:

```azurecli
azure help
```

> [!NOTE]
> På vissa Linux-distributioner kan du få ett felmeddelande liknande för`/usr/bin/env: ‘node’: No such file or directory`. Det här felet kommer från nya installationer av Node.js som installeras på /usr/bin/nodejs. toofix, skapa en symbolisk länk för/usr/bin/nod genom att köra det här kommandot:

```bash
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

toosee hello version av hello Azure CLI 1.0 du installerat typen hello följande:

```azurecli
azure --version
```

Nu är du klar! alla tooaccess hello CLI-kommandon toowork med egna resurser, [ansluta tooyour Azure-prenumeration från hello Azure CLI](xplat-cli-connect.md).

> [!NOTE]
> När du börjar använda Azure CLI, visas ett meddelande som frågar om du vill tooallow Microsoft toocollect användningsinformation. Det är frivilligt att delta. Om du väljer tooparticipate måste du stoppa när som helst genom att köra `azure telemetry --disable`. tooenable delta när som helst köra `azure telemetry --enable`.

## <a name="update-hello-cli"></a>Uppdatera hello CLI
Microsoft släpper ofta uppdaterade versioner av hello Azure CLI. Installera om hello CLI med hello installer för ditt operativsystem eller köra hello senaste dockerbehållare. Om du har hello senaste Node.js och npm installerad, uppdatera genom att skriva följande hello (på Linux-distributioner måste du kanske toouse **sudo**).

```bash
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a>Aktivera flikavslutande
Flikslutförande av CLI-kommandon stöds för Mac- och Linux.

tooenable i zsh, kör:

```bash
echo '. <(azure --completion)' >> .zshrc
```

tooenable i bash, kör:

```bash
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```


## <a name="next-steps"></a>Nästa steg
* [Ansluta från hello CLI tooyour Azure-prenumeration](xplat-cli-connect.md) toocreate och hantera Azure-resurser.
* toolearn mer om hello Azure CLI, ladda ned källkoden, rapportera problem, eller bidra toohello projekt, besök hello [GitHub-lagringsplatsen för hello Azure CLI](https://github.com/azure/azure-xplat-cli).
* Om du har frågor om hur du använder hello Azure CLI eller Azure besöka hello [Azure forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).


[mac-installer]: http://aka.ms/mac-azure-cli
[windows-installer]: http://aka.ms/webpi-azure-cli
[linux-installer]: http://aka.ms/linux-azure-cli
[cliasm]: /cli/azure/get-started-with-az-cli2
[cliarm]: ./virtual-machines/azure-cli-arm-commands.md
