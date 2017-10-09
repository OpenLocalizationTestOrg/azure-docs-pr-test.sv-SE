---
title: aaaDevTest Labs begrepp | Microsoft Docs
description: "Lär dig hello grundläggande begrepp för DevTest Labs och hur det kan göra det enkelt toocreate, hantera och övervaka virtuella Azure-datorer"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 105919e8-3617-4ce3-a29f-a289fa608fb2
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
ms.openlocfilehash: d9f1d948002c4d3121e5bdd4e65eb8b54cd91f9c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="devtest-labs-concepts"></a>DevTest Labs-koncept
## <a name="overview"></a>Översikt
hello följande lista innehåller huvudbegrepp DevTest Labs och definitioner:

## <a name="labs"></a>Labbar
Ett labb är hello-infrastruktur som omfattar en grupp med resurser, t.ex. virtuella datorer (VM), som låter dig bättre hantera resurserna genom att ange gränser och kvoter.

## <a name="virtual-machine"></a>Virtuell dator
En Azure VM är en av flera typer av [på begäran skalbara datorresurser](https://docs.microsoft.com/azure/app-service-web/choose-web-site-cloud-service-vm) som Azure erbjuder. Azure virtuella datorer får du hello virtualiseringsflexibilitet utan toobuy och underhålla hello fysiska maskinvara som körs, även om du fortfarande behöver toomaintain hello VM genom att utföra vissa åtgärder, till exempel konfigurera, korrigera och installera hello programvara som körs på den.

[Översikt över Windows-datorer i Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-overview) ger information om vad du bör tänka på innan du skapar en virtuell dator, hur du skapar och hur du hanterar.

## <a name="claimable-vm"></a>Claimable VM
En Claimable Azure VM är en virtuell dator som är tillgängliga för användning av alla lab-användare med behörighet. En labb-administratör kan förbereda virtuella datorer med vissa grundläggande bilder och artefakter och spara dem tooa delade poolen. Lab-användare kan sedan begära en aktiv virtuell dator från hello pool när de behöver en med den specifika konfigurationen.

En virtuell dator som är claimable inte har tilldelats ursprungligen tooany viss användare, men kommer att visas i listan för varje användare under ”Claimable virtuella datorer”. När en virtuell dator har begärts av en användare är det flyttas upp tootheir ”Mina virtuella datorer” området och inte längre claimable av någon annan användare.

## <a name="environment"></a>Miljö
En miljö refererar tooa samling Azure-resurser i ett labb i DevTest Labs. [Det här blogginlägget](https://blogs.msdn.microsoft.com/devtestlab/2016/11/16/connect-2016-news-for-azure-devtest-labs-azure-resource-manager-template-based-environments-vm-auto-shutdown-and-more/) beskrivs hur toocreate flera Virtuella miljöer från Azure Resource Manager-mallar.

## <a name="base-images"></a>Grundläggande bilder
Grundläggande avbildningar är VM-avbildningar med alla hello verktyg och inställningar för förinstallerad konfigurerade tooquickly skapa en virtuell dator. Du kan etablera en virtuell dator genom att välja en befintlig base och lägger till en artefakt tooinstall test-agenten. Du kan sedan spara hello etablerad VM som en bas så att hello base kan användas utan att ha tooreinstall hello test agent för varje etablering av hello VM.

## <a name="artifacts"></a>Artefakter
Artefakter är används toodeploy och konfigurera ditt program när en virtuell dator har etablerats. Artefakter kan vara:

* Verktyg som du vill tooinstall på hello virtuell dator, till exempel agenter, Fiddler och Visual Studio.
* Åtgärder som du vill toorun på hello virtuell dator, t.ex att klona en repo.
* Program som du vill tootest.

Artefakter är [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) JSON-filer som innehåller instruktioner tooperform distribution och tillämpa konfigurationen.

## <a name="artifact-repositories"></a>Artefakt databaser
Artefakt databaser är git-databaser där artefakter checkas in. Artefakt databaser kan läggas toomultiple labs i din organisation att aktivera återanvändning och delning.

## <a name="formulas"></a>Formler
Formler i tillägg toobase bilder, tillhandahåller en mekanism för snabb etablering av virtuell dator. En formel i DevTest Labs är en lista över standard egenskapen värden som används för toocreate ett labb VM.
Med formler, virtuella datorer med samma in egenskaper - basavbildning, VM-storlek, virtuella nätverk och artefakter - hello skapas utan att behöva toospecify dessa egenskaper varje gång. När du skapar en virtuell dator från en formel hello standardvärdena kan användas som-har eller ändrats.

## <a name="policies"></a>Principer
Med hjälp av principer kontrollerar kostnaden i labbet. Du kan till exempel skapa en princip tooautomatically stänga av virtuella datorer baserat på ett definierat schema.

## <a name="caps"></a>Versaler
CAPS är en mekanism toominimize skräp i labbet. Exempelvis kan du ange en fjärrskrivbordsanslutning toorestrict hello antal virtuella datorer som kan skapas per användare eller i ett labb.

## <a name="security-levels"></a>Säkerhetsnivåer
Säkerhetsåtkomst bestäms av rollbaserad åtkomstkontroll (RBAC). toounderstand hur åt fungerar, hjälper det toounderstand hello skillnaderna mellan en behörighet, en roll och ett omfång som definieras av RBAC.

* Behörigheten - behörighet är definierad åtkomst tooa något (t.ex. läsbehörighet tooall virtuella datorer).
* Roll - en roll är en uppsättning behörigheter som kan grupperas och tilldelad tooa användare. Till exempel hello *prenumerationsägaren* rollen har åtkomst tooall resurser inom en prenumeration.
* Scope - ett scope är en nivå i hello hierarkin för en Azure-resurs, till exempel en resursgrupp, en enda lab eller hello hela prenumerationen.

Inom hello omfång DevTest Labs, det finns två typer av roller toodefine behörigheten: labbägaren och lab-användare.

* Labbägaren - en labbägaren har tooany komma åt resurser i hello testmiljön. Därför kan en labbägaren ändra principer, läsa och skriva alla virtuella datorer, ändra hello virtuella nätverk och så vidare.
* Lab-användare - lab-användare kan visa alla lab resurser, till exempel virtuella datorer, principer och virtuella nätverk, men det går inte att ändra principer eller virtuella datorer som skapats av andra användare.

toosee hur toocreate anpassade roller i DevTest Labs finns toohello artikel [bevilja användarbehörighet toospecific principer för labbet](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Eftersom scope är hierarkiska, när en användare har behörighet för ett visst område kan beviljas de automatiskt dessa behörighet för definitionsområdet var lägre nivå finns. Till exempel om en användare har tilldelats rollen som toohello prenumerationsägaren, har sedan de åtkomst tooall resurser i en prenumeration som omfattar alla virtuella datorer, alla virtuella nätverk och alla övningar. Därför ärver en prenumerationsägaren automatiskt labbägaren hello roll. Dock gäller inte hello motsatt. En labbägaren har åtkomst tooa lab, vilket är en omfattning som är lägre än hello prenumerationsnivån. En labbägaren kommer därför inte kan toosee virtuella datorer eller virtuella nätverk eller resurser som ligger utanför hello labbet.

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager-mallar
Alla hello begrepp som diskuteras i den här artikeln kan konfigureras med hjälp av Azure Resource Manager-mallar, vilket gör att du kan definiera hello/konfigurationen i Azure-lösningen och upprepade gånger distribuera i ett konsekvent tillstånd.

[Förstå hello struktur och syntaxen för Azure Resource Manager-mallar](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates#template-format) beskriver hello strukturen i en Azure Resource Manager-mall och hello egenskaper som är tillgängliga i hello olika avsnitt i en mall.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Nästa steg
[Skapa ett labb i DevTest Labs](devtest-lab-create-lab.md)
