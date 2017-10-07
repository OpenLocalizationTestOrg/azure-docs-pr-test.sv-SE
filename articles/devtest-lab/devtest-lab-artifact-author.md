---
title: "aaaCreate anpassade artefakter för den virtuella datorn med DevTest Labs | Microsoft Docs"
description: "Lär dig hur tooauthor egna artefakter för hjälp med DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 32dcdc61-ec23-4a01-b731-78c029ea5316
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2017
ms.author: tarcher
ms.openlocfilehash: 2bd603bc1241ca6b669a3a276a677729514f0df2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-custom-artifacts-for-your-devtest-labs-vm"></a>Skapa anpassade artefakter för den virtuella datorn med DevTest Labs
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/how-to-author-custom-artifacts/player]
> 
> 

## <a name="overview"></a>Översikt
**Artefakter** är används toodeploy och konfigurera ditt program när en virtuell dator har etablerats. En artefakt består av en artefakt definitionsfilen och andra filer som lagras i en mapp i en git-lagringsplats. Artefakt definitionsfiler består av JSON och uttryck som du kan använda toospecify vad du vill tooinstall på en virtuell dator. Du kan till exempel definiera hello namnet på en artefakt, kommandot toorun och parametrar som är tillgängliga när hello kommandot körs. Du kan se tooother skriptfiler i definitionsfilen för hello artefakt efter namn.

## <a name="artifact-definition-file-format"></a>Artefakt format för paketdefinitionsfiler
hello visar följande exempel hello avsnitt som utgör hello grundläggande struktur för en definitionsfil:

    {
      "$schema": "https://raw.githubusercontent.com/Azure/azure-devtestlab/master/schemas/2016-11-28/dtlArtifacts.json",
      "title": "",
      "description": "",
      "iconUri": "",
      "targetOsType": "",
      "parameters": {
        "<parameterName>": {
          "type": "",
          "displayName": "",
          "description": ""
        }
      },
      "runCommand": {
        "commandToExecute": ""
      }
    }

| Elementnamn | Krävs? | Beskrivning |
| --- | --- | --- |
| $schema |Nej |Sökväg till hello JSON-schema som hjälper till att testa hello giltighet hello definitionsfilen. |
| Rubrik |Ja |Namnet på hello artefakt som visas i hello labbet. |
| description |Ja |Beskrivning av hello artefakt som visas i hello labbet. |
| iconUri |Nej |URI för hello-ikonen som visas i hello labbet. |
| targetOsType |Ja |Operativsystemet på hello VM där artefakt är installerad. Alternativ som stöds är: Windows och Linux. |
| parameters |Nej |Värden som tillhandahålls när artefakt installationskommando körs på en dator. På så sätt kan anpassa din artefakt. |
| Kör kommando |Ja |Artefakt installationskommando som körs på en virtuell dator. |

### <a name="artifact-parameters"></a>Artefakt parametrar
I hello parametrar i definitionsfilen för hello kan ange du vilka värden som en användare kan ange när du installerar en artefakt. Du kan se toothese värden i hello artefakt installationskommando.

Du kan definiera parametrar med hello följande struktur:

    "parameters": {
        "<parameterName>": {
          "type": "<type-of-parameter-value>",
          "displayName": "<display-name-of-parameter>",
          "description": "<description-of-parameter>"
        }
      }

| Elementnamn | Krävs? | Beskrivning |
| --- | --- | --- |
| typ |Ja |Typ av parametervärdet. Se följande lista för hello tillåtna typer hello: |
| Visningsnamn |Ja |Namnet på hello parameter visas tooa användaren i hello labbet. | |
| description |Ja |Beskrivning av hello-parameter som visas i hello labbet. |

hello tillåtna typer är:

* strängen – en giltig JSON-sträng
* int – alla giltiga JSON-heltal
* bool – alla giltiga JSON-booleska
* matrisen – en giltig JSON-matris

## <a name="artifact-expressions-and-functions"></a>Artefakt uttryck och funktioner
Du kan använda uttryck och funktioner tooconstruct hello artefakt installationskommando.
Uttryck är omges av hakparenteser ([och]) och utvärderas när hello artefakt installeras. Uttryck kan finnas var som helst i ett strängvärde i JSON och returnerar alltid en annan JSON-värde. Om du behöver toouse en teckensträng som börjar med en hakparentes [, måste du använda två hakparenteser [[.
Normalt använder du uttryck med funktioner tooconstruct ett värde. Precis som i JavaScript är-funktionsanrop som formaterade som functionName(arg1,arg2,arg3).

hello visas nedan vanliga funktioner:

* parameters(parameterName) - returnerar ett parametervärde som tillhandahålls vid hello artefakt kommandot körs.
* sammanfoga (arg1, arg2, arg3,...) - kombinerar flera strängvärden. Den här funktionen kan ta valfritt antal argument.

följande exempel visar hur hello toouse uttryck och funktioner tooconstruct ett värde:

    runCommand": {
         "commandToExecute": "[concat('powershell.exe -ExecutionPolicy bypass \"& ./startChocolatey.ps1'
    , ' -RawPackagesList ', parameters('packages')
    , ' -Username ', parameters('installUsername')
    , ' -Password ', parameters('installPassword'))]"
    }

## <a name="create-a-custom-artifact"></a>Skapa en anpassad artefakt
Skapa din egen artefakt genom att följa dessa steg:

1. Installera en JSON-redigerare - du behöver en JSON-redigerare toowork med artefakt definitionsfilerna. Vi rekommenderar att du använder [Visual Studio Code](https://code.visualstudio.com/), som är tillgänglig för Windows, Linux och OS X.
2. Hämta en exempel-artifactfile.json - checka ut hello artefakter som skapats av Azure DevTest Labs-teamet på vår [GitHub-lagringsplatsen](https://github.com/Azure/azure-devtestlab), där vi har skapat ett omfattande Meddelandebibliotek artefakter som hjälper dig skapa dina egna artefakter. Hämta en artefakt definitionsfilen och göra ändringar tooit toocreate egna artefakter.
3. Genom att utnyttja IntelliSense - utnyttjar IntelliSense toosee giltiga element som kan använda tooconstruct en artefakt definitionsfilen. Du kan också se hello olika alternativ för värden i ett element. Till exempel IntelliSense visar du hello två val av Windows eller Linux när du redigerar hello **targetOsType** element.
4. Store hello artefakt i en [git-lagringsplats](devtest-lab-add-artifact-repo.md).
   
   1. Skapa en separat katalog för varje artefakt där hello katalognamnet är hello samma som hello artefakt namn.
   2. Lagra hello artefakt-definitionsfil (artifactfile.json) i hello-katalogen som du skapade.
   3. Store hello skripten från hello artefakt installationskommando.
      
      Här är ett exempel på hur en artefakt-mappen kan se ut:
      
      ![Exempel för artefakt git repo](./media/devtest-lab-artifact-author/git-repo.png)
5. Lägga till hello artefakter databasen toohello lab: finns toohello artikel [lägga till en Git-lagringsplats för artefakter och mallar](devtest-lab-add-artifact-repo.md).

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-articles"></a>Relaterade artiklar
* [Hur toodiagnose artefakt fel i DevTest Labs](devtest-lab-troubleshoot-artifact-failure.md)
* [Ansluta till en VM-tooexisting AD-domän med hjälp av en resource manager-mall i Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[lägga till ett Git artefakt databasen tooa labb](devtest-lab-add-artifact-repo.md).

