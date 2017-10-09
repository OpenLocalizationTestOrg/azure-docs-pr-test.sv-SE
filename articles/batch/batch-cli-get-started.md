---
title: "aaaGet igång med Azure CLI för Batch | Microsoft Docs"
description: "Få en snabb introduktion toohello Batch-kommandon i Azure CLI för att hantera Azure Batch-tjänsten resurser"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: fcd76587-1827-4bc8-a84d-bba1cd980d85
ms.service: batch
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 07/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 14f28311ecb16c8097d0d304a4ad89de282a2e9b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-batch-resources-with-azure-cli"></a>Hantera Batch-resurser med Azure CLI

hello Azure CLI 2.0 är Azures nya kommandoradsverktyget upplevelsen för att hantera Azure-resurser. Den kan användas i Mac OS, Linux och Windows. Azure CLI 2.0 är optimerad för att hantera och administrera Azure-resurser från hello Kommandotolken. Du kan använda hello Azure CLI toomanage Azure Batch-konton och toomanage resurser, till exempel pooler, jobb och uppgifter. Med hello Azure CLI, du kan skapa ett skript många hello samma uppgifter som du vill utföra med hello Batch-API: er, Azure-portalen och Batch-PowerShell-cmdlets.

Den här artikeln innehåller en översikt över hur du använder [Azure CLI version 2.0](https://docs.microsoft.com/cli/azure/overview) med Batch. Se [Kom igång med Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) en översikt över hello CLI med Azure.

Microsoft rekommenderar att du använder hello senaste versionen av hello Azure CLI version 2.0. Mer information om version 2.0 finns i [Azure Command Line 2.0 now generally available](https://azure.microsoft.com/blog/announcing-general-availability-of-vm-storage-and-network-azure-cli-2-0/) (på engelska).

## <a name="set-up-hello-azure-cli"></a>Ställ in hello Azure CLI

tooinstall hello Azure CLI, följ hello steg som beskrivs i [installera hello Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli.md).

> [!TIP]
> Vi rekommenderar att du uppdaterar installationen av Azure CLI ofta tootake nytta av tjänsteuppdateringar och förbättringar.
> 
> 

## <a name="command-help"></a>Kommandohjälp

Du kan visa hjälptext som visas för alla kommandon i hello Azure CLI genom att lägga till `-h` toohello kommando. Utelämna andra alternativ. Exempel:

* tooget hjälp för hello `az` kommandot, ange:`az -h`
* tooget en lista över alla Batch-kommandon i hello CLI, använder du:`az batch -h`
* tooget hjälp om hur du skapar ett Batch-konto, ange:`az batch account create -h`

När osäkra, använda hello `-h` kommandoradsalternativet tooget hjälp med några Azure CLI-kommando.

> [!NOTE]
> Tidigare versioner av hello Azure CLI används `azure` toopreface kommandot CLI. I version 2.0 har alla kommandon nu `az` som prefix. Vara säker på att tooupdate skript toouse hello nya syntaxen med version 2.0.
>
>  

Dessutom finns toohello Azure CLI-referensdokumentationen för information om [Azure CLI-kommandona för Batch](https://docs.microsoft.com/cli/azure/batch). 

## <a name="log-in-and-authenticate"></a>Logga in och autentisera

toouse hello Azure CLI med Batch, du behöver toolog i och autentisera. Det finns två enkla steg toofollow:

1. **Logga in på Azure.** Logga in på Azure ger du åtkomst till tooAzure Resource Manager-kommandon, inklusive [Batch Management-tjänsten](batch-management-dotnet.md) kommandon.  
2. **Logga in på Batch-kontot**. Logga in på din Batch-konto får du åtkomst till tooBatch kommandon.   

### <a name="log-in-tooazure"></a>Logga in tooAzure

Det finns några olika sätt toolog i Azure, beskrivs i detalj i [logga in med Azure CLI 2.0](https://docs.microsoft.com/cli/azure/authenticate-azure-cli):

1. [Logga in interaktivt](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#interactive-log-in). Logga in interaktivt när du använder Azure CLI-kommandona själv hello-kommandoraden.
2. [Logga in med ett huvudnamn för tjänsten](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#logging-in-with-a-service-principal). Logga in med ett huvudnamn för tjänsten när du använder Azure CLI-kommandon från ett skript eller ett program.

Hello enligt den här artikeln visar vi hur toolog till Azure interaktivt. Typen [az inloggningen](https://docs.microsoft.com/cli/azure/#login) på hello kommandoraden:

```azurecli
# Log in tooAzure and authenticate interactively.
az login
```

Hej `az login` kommandot returnerar en token som du kan använda tooauthenticate, som visas här. Följ hello anvisningarna tooopen en webbsida och skicka hello token tooAzure:

![Logga in tooAzure](./media/batch-cli-get-started/az-login.png)

hello-exempel som anges i hello [exempel kommandoskript](#sample-shell-scripts) avsnittet också visa hur toostart Azure CLI-session genom att logga in på Azure interaktivt. När du har loggat in kan du anropa kommandon toowork med Batch Management-resurser, inklusive Batch-konton, nycklar, programpaket och kvoter.  

### <a name="log-in-tooyour-batch-account"></a>Logga in tooyour Batch-kontot

toouse hello Azure CLI toomanage Batch resurser, till exempel pooler, jobb och uppgifter, du behöver toolog i Batch-kontot och autentisera. toolog i toohello Batch-tjänsten använder hello [az batch kontoinloggning](https://docs.microsoft.com/cli/azure/batch/account#login) kommando. 

Det finns två alternativ för att autentisera mot Batch-kontot:

- **Med autentisering via Azure Active Directory (Azure AD).** 

    Autentisera med Azure AD är hello standard när du använder hello Azure CLI med Batch och rekommenderas för de flesta scenarier. 
    
    När du loggar in tooAzure interaktivt, enligt beskrivningen i föregående avsnitt i hello cachelagras dina autentiseringsuppgifter så hello Azure CLI kan logga in dig tooyour Batch-kontot med dessa samma autentiseringsuppgifter. Om du loggar in med ett huvudnamn för tjänsten tooAzure är också dessa autentiseringsuppgifter används toolog i tooyour Batch-kontot.

    En fördel med Azure AD är att den innehåller rollbaserad åtkomstkontroll (RBAC). Med RBAC beror en användares behörighet på den tilldelade rollen snarare än om de har hello nycklar. Du kan hantera RBAC-roller och låta Azure AD hantera åtkomst och autentisering i stället för att hantera nycklar.  

    Autentisera med Azure AD krävs om du har skapat ditt Azure Batch-konto med dess poolen allokering läge ange too'User prenumeration '. 

    toolog i tooyour Batch-konto med Azure AD, anropa hello [az batch kontoinloggning](https://docs.microsoft.com/cli/azure/batch/account#login) kommando: 

    ```azurecli
    az batch account login -g myresource group -n mybatchaccount
    ```

- **Autentisering med delad nyckel.**

    [Autentisering med delad nyckel](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service#authentication-via-shared-key) använder ditt konto åtkomstnycklar tooauthenticate Azure CLI-kommandona för hello Batch-tjänsten.

    Om du skapar Azure CLI skript tooautomate anropa Batch-kommandon, kan du använda autentisering med delad nyckel eller en Azure AD-tjänstens huvudnamn. I vissa situationer kan det vara enklare att använda autentisering med delad nyckel än att skapa ett huvudnamn för tjänsten.  

    toolog i med hjälp av autentisering med delad nyckel, inkludera hello `--shared-key-auth` alternativ på kommandoraden för hello:

    ```azurecli
    az batch account login -g myresourcegroup -n mybatchaccount --shared-key-auth
    ```

hello-exempel som anges i hello [exempel kommandoskript](#sample-shell-scripts) avsnittet visar hur toolog i Batch-kontot med hello Azure CLI med Azure AD och delad nyckel.

## <a name="use-azure-batch-cli-templates-and-file-transfer-preview"></a>Använda Azure Batch CLI-mallar och filöverföring (förhandsversion)

Du kan använda hello Azure CLI toorun Batch jobb slutpunkt till slutpunkt utan att skriva kod. Mallen kommandofiler stöd för att skapa pooler, jobb och aktiviteter med hello Azure CLI. Du kan också använda hello Azure CLI tooupload jobbet indatafiler toohello Azure Storage-konto som är associerade med hello Batch-kontot och hämta jobb utgående filer från den. Mer information finns i [Använda Azure Batch CLI-mallar och filöverföring (förhandsversion)](batch-cli-templates.md).

## <a name="sample-shell-scripts"></a>Exempel på kommandoskript

hello exempelskript som anges i följande tabell visar hello hur toouse Azure CLI-kommandon med hello Batch-tjänsten och Batch-tjänsten tooaccomplish vanliga hanteringsuppgifter. Dessa exempel på skript omfattar många av hello kommandon i hello Azure CLI för Batch. 

| Skript | Anteckningar |
|---|---|
| [Skapa ett Batch-konto](./scripts/batch-cli-sample-create-account.md) | Skapar ett Batch-konto och kopplar det till ett lagringskonto. |
| [Lägga till ett program](./scripts/batch-cli-sample-add-application.md) | Lägger till ett program och laddar upp paketerade binärfiler.|
| [Hantera Batch-pooler](./scripts/batch-cli-sample-manage-pool.md) | Visar hur du skapar, ändrar storlek på och hanterar pooler. |
| [Köra ett jobb och aktiviteter med Batch](./scripts/batch-cli-sample-run-job.md) | Visar hur du kör ett jobb och lägger till aktiviteter. |

## <a name="json-files-for-resource-creation"></a>JSON-filer för resursskapande

När du skapar Batch-resurser som pooler och jobb kan ange du en JSON-fil som innehåller hello ny resurs konfiguration i stället för att ange dess parametrar som kommandoradsalternativ. Exempel:

```azurecli
az batch pool create my_batch_pool.json
```

Du kan skapa de flesta Batch-resurser med hjälp av endast kommandoradsalternativ, vissa funktioner som kräver att du anger en JSON-formaterad fil som innehåller information om hello resursen. Du måste till exempel använda en JSON-fil om du vill toospecify resursfiler för en start-aktivitet.

toosee hello JSON syntax krävs toocreate en resurs finns toohello [Batch REST API-referensen] [ rest_api] dokumentation. Varje ”Lägg till *resurstypen*” hello REST API-referensen innehåller exempel på JSON-skript för att skapa den här resursen. Du kan använda dessa exempel JSON-skript som en mall för JSON-filer toouse med hello Azure CLI. Till exempel toosee hello JSON-syntax för att skapa en pool, referera för[Lägg till ett poolen tooan][rest_add_pool].

Ett exempelskript som anger en JSON-fil finns i [Köra ett jobb och aktiviteter med Batch](./scripts/batch-cli-sample-run-job.md).

> [!NOTE]
> Om du anger en JSON-fil när du skapar en resurs, ignoreras andra parametrar som du anger på hello kommandoraden för den här resursen.
> 
> 

## <a name="efficient-queries-for-batch-resources"></a>Effektiva frågor för Batch-resurser

Varje Batch-resurstyp stöder ett `list`-kommando som frågar Batch-kontot och visar en lista över resurser av den typen. Du kan till exempel ange hello pooler i ditt konto och hello aktiviteter i ett projekt:

```azurecli
az batch pool list
az batch task list --job-id job001
```

När du frågar hello Batch-tjänsten med en `list` åtgärd, som du kan ange en OData-satsen toolimit hello mängden data som returneras. Eftersom alla filtrering sker serversidan, korsar endast hello data som du begär hello-överföring. Använd dessa satser toosave bandbredd (och därför tid) när du utför Liståtgärder.

hello beskrivs följande tabell hello OData-satser stöds av hello Batch-tjänsten:

| Sats | Beskrivning |
|---|---|
| `--select-clause [select-clause]` | Returnerar en delmängd av egenskaperna för varje entitet. |
| `--filter-clause [filter-clause]` | Returnerar endast enheter som matchar hello angivna OData-uttrycket. |
| `--expand-clause [expand-clause]` | Hämtar hello entitet informationen i ett enda underliggande REST-anrop. hello Expandera satsen för närvarande stöder endast hello `stats` egenskapen. |

För ett exempel på skript som visar hur toouse en OData-sats, se [köra ett jobb och aktiviteter med Batch](./scripts/batch-cli-sample-run-job.md).

Mer information om hur du utför effektiv listan frågor med OData-satser finns [fråga hello Azure Batch-tjänsten effektivt](batch-efficient-list-queries.md).

## <a name="troubleshooting-tips"></a>Felsökningstips

hello följande tips kan hjälpa dig när du felsöker problem med Azure CLI:

* Använd `-h` tooget **hjälptext** för alla CLI-kommandon
* Använd `-v` och `-vv` toodisplay **utförlig** kommandot utdata. När hello `-vv` flaggan ingår, hello Azure CLI visar hello faktiska REST-begäranden och -svar. Växlarna är användbara för att visa fullständiga utdata vid fel.
* Du kan visa **kommandot utdata som JSON** med hello `--json` alternativet. Till exempel visar `az batch pool show pool001 --json` pool001:s egenskaper i JSON-format. Du kan sedan kopiera och ändra den här utdata toouse i en `--json-file` (se [JSON-filer](#json-files) tidigare i den här artikeln).
<!---Loc Comment: Please, check link [JSON files] since it's not redirecting tooany location.--->
* Hej [Batch-forum] [ batch_forum] övervakas av Batch-gruppmedlemmar. Du kan ställa frågor där om du får problem eller behöver hjälp med en viss åtgärd.

## <a name="next-steps"></a>Nästa steg

* Mer information om hello Azure CLI finns hello [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).
* Mer information om Batch-resurser finns i [Utveckla storskaliga parallella beräkningslösningar med Batch](batch-api-basics.md).
* Mer information om hur du använder Batch mallar toocreate pooler, jobb och uppgifter utan att skriva kod finns [Azure Batch CLI mallar och filöverföring (förhandsgranskning)](batch-cli-templates.md).

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
