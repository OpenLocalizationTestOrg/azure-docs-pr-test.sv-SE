---
title: "Skapa anpassade artefakter för den virtuella datorn med DevTest Labs | Microsoft Docs"
description: "Lär dig hur du skapar egna artefakter för användning med DevTest Labs"
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
ms.openlocfilehash: 2412033daa1d97860dd9f380178622b1ddc590c0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-custom-artifacts-for-your-devtest-labs-vm"></a>Skapa anpassade artefakter för den virtuella datorn med DevTest Labs
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/how-to-author-custom-artifacts/player]
> 
> 

## <a name="overview"></a>Översikt
**Artefakter** används för att distribuera och konfigurera ditt program när en virtuell dator har etablerats. En artefakt består av en artefakt definitionsfilen och andra filer som lagras i en mapp i en git-lagringsplats. Artefakt definitionsfiler består av JSON och uttryck som du kan använda för att ange vad du vill installera på en virtuell dator. Du kan till exempel definiera namnet på en artefakt kommando som ska köras och parametrar som är tillgängliga när du kör kommandot. Du kan referera till andra filer i definitionsfilen artefakt efter namn.

## <a name="artifact-definition-file-format"></a>Artefakt format för paketdefinitionsfiler
I följande exempel visas de avsnitt som utgör den grundläggande strukturen i en definitionsfil:

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
| $schema |Nej |Plats för JSON-schemafilen som hjälper till att testa giltighet definitionsfilen. |
| Rubrik |Ja |Namnet på den artefakt som visas i labbet. |
| Beskrivning |Ja |Beskrivning av artefakt som visas i labbet. |
| iconUri |Nej |URI för den ikon som visas i labbet. |
| targetOsType |Ja |Operativsystemet på den virtuella datorn där artefakt är installerad. Alternativ som stöds är: Windows och Linux. |
| Parametrar |Nej |Värden som tillhandahålls när artefakt installationskommando körs på en dator. På så sätt kan anpassa din artefakt. |
| Kör kommando |Ja |Artefakt installationskommando som körs på en virtuell dator. |

### <a name="artifact-parameters"></a>Artefakt parametrar
I avsnittet parametrar i definitionsfilen anger du vilka värden som en användare kan ange när du installerar en artefakt. Du kan referera till dessa värden i kommandot artefakt install.

Du kan definiera parametrar med följande struktur:

    "parameters": {
        "<parameterName>": {
          "type": "<type-of-parameter-value>",
          "displayName": "<display-name-of-parameter>",
          "description": "<description-of-parameter>"
        }
      }

| Elementnamn | Krävs? | Beskrivning |
| --- | --- | --- |
| typ |Ja |Typ av parametervärdet. Se följande lista för tillåtna typer: |
| Visningsnamn |Ja |Namnet på den parameter som visas för en användare i labbet. | |
| Beskrivning |Ja |Beskrivning av den parameter som visas i labbet. |

De tillåtna typerna är:

* strängen – en giltig JSON-sträng
* int – alla giltiga JSON-heltal
* bool – alla giltiga JSON-booleska
* matrisen – en giltig JSON-matris

## <a name="artifact-expressions-and-functions"></a>Artefakt uttryck och funktioner
Du kan använda uttryck och funktioner för att konstruera artefakten installationskommando.
Uttryck är omges av hakparenteser ([och]) och utvärderas när artefakten installeras. Uttryck kan finnas var som helst i ett strängvärde i JSON och returnerar alltid en annan JSON-värde. Om du behöver använda en teckensträng som börjar med en hakparentes [, måste du använda två hakparenteser [[.
Normalt använder du uttryck med funktioner för att konstruera ett värde. Precis som i JavaScript är-funktionsanrop som formaterade som functionName(arg1,arg2,arg3).

I följande lista visas vanliga funktioner:

* parameters(parameterName) - returnerar ett parametervärde som tillhandahålls vid artefakt kommandot körs.
* sammanfoga (arg1, arg2, arg3,...) - kombinerar flera strängvärden. Den här funktionen kan ta valfritt antal argument.

I följande exempel visas hur du använder uttryck och funktioner för att konstruera ett värde:

    runCommand": {
         "commandToExecute": "[concat('powershell.exe -ExecutionPolicy bypass \"& ./startChocolatey.ps1'
    , ' -RawPackagesList ', parameters('packages')
    , ' -Username ', parameters('installUsername')
    , ' -Password ', parameters('installPassword'))]"
    }

## <a name="create-a-custom-artifact"></a>Skapa en anpassad artefakt
Skapa din egen artefakt genom att följa dessa steg:

1. Installera en JSON-redigerare, behöver du en JSON-redigerare för att arbeta med artefakt definitionsfiler. Vi rekommenderar att du använder [Visual Studio Code](https://code.visualstudio.com/), som är tillgänglig för Windows, Linux och OS X.
2. Hämta en exempel-artifactfile.json - utcheckning artefakter som skapats av Azure DevTest Labs-teamet på vår [GitHub-lagringsplatsen](https://github.com/Azure/azure-devtestlab), där vi har skapat ett omfattande Meddelandebibliotek artefakter som hjälper dig skapa dina egna artefakter. Hämta en artefakt definitionsfilen och göra ändringar i den för att skapa egna artefakter.
3. Genom att utnyttja IntelliSense - utnyttjar IntelliSense Se giltiga element som kan användas för att konstruera en artefakt definitionsfilen. Du kan också se de olika alternativen för värden i ett element. Till exempel IntelliSense visar du två val av Windows eller Linux när du redigerar den **targetOsType** element.
4. Lagra artefakt i en [git-lagringsplats](devtest-lab-add-artifact-repo.md).
   
   1. Skapa en separat katalog för varje artefakt där katalognamnet är detsamma som namnet artefakt.
   2. Lagra artefakt-definitionsfil (artifactfile.json) i den katalog som du skapade.
   3. Lagra de skript som refereras till från installationskommando artefakt.
      
      Här är ett exempel på hur en artefakt-mappen kan se ut:
      
      ![Exempel för artefakt git repo](./media/devtest-lab-artifact-author/git-repo.png)
5. Lägg till artefakter databasen till labbet - finns i artikel [lägga till en Git-lagringsplats för artefakter och mallar](devtest-lab-add-artifact-repo.md).

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-articles"></a>Relaterade artiklar
* [Felsökning av artefakt fel i DevTest Labs](devtest-lab-troubleshoot-artifact-failure.md)
* [Anslut en virtuell dator till befintliga AD-domän med hjälp av en resource manager-mall i Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a>Nästa steg
* Lär dig hur du [lägga till en Git-artefaktcentrallagret i ett labb](devtest-lab-add-artifact-repo.md).

