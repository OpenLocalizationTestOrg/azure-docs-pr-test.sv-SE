---
title: "aaaUse Azure Batch-API: er och verktyg toodevelop storskaliga parallellbearbetning lösningar | Microsoft Docs"
description: "Läs mer om hello API: er och verktyg som finns tillgängliga för utveckling av lösningar med hello Azure Batch-tjänsten."
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: get-started-article
ms.date: 03/08/2017
ms.author: tamram
ms.openlocfilehash: ca75a1a63b3e7e6b0805e79a63685bc49aaaca8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-batch-apis-and-tools"></a>Översikt över Batch-API:er och verktyg

Bearbetning av parallell arbetsbelastningar med Azure Batch normalt görs via programmering genom att använda en hello [Batch-API: er](#batch-development-apis). Klientprogrammet eller tjänsten kan använda hello Batch-API: er toocommunicate med hello Batch-tjänsten. Med hello Batch-API: er, kan du skapa och hantera pooler för compute-noder virtuella datorer eller molntjänster. Du kan sedan schemalägga jobb och aktiviteter toorun på noderna. 

Du kan effektivt bearbeta stora arbetsbelastningar för din organisation, eller ange en service klientdel tooyour kunder så att de kan köra jobb och uppgifter – på begäran eller enligt ett schema--på ett, hundratals eller till och med tusentals noder. Du kan också använda Azure Batch som en del av ett större arbetsflöde, med verktyg som [Azure Data Factory](../data-factory/data-factory-data-processing-using-batch.md?toc=%2fazure%2fbatch%2ftoc.json).

> [!TIP]
> När du är klar toodig i toohello Batch-API ha en djupare förståelse för hello funktioner den erbjuder, kolla hello [Batch funktionsöversikt för utvecklare](batch-api-basics.md).
> 
> 

## <a name="azure-accounts-for-batch-development"></a>Azure-konton för Batch-utveckling
När du utvecklar Batch-lösningar använder du hello följande konton i Microsoft Azure.

* **Azure-konto och Azure-prenumeration** – Om du inte redan har en Azure-prenumeration kan du aktivera din [MSDN-prenumerantförmån][msdn_benefits] eller registrera dig för ett [kostnadsfritt Azure-konto][free_account]. När du skapar ett konto skapas en standardprenumeration åt dig.
* **Batch-konto** – Azure Batch-resurser, inklusive pooler, beräkningsnoder, jobb och aktiviteter, är associerade med ett Azure Batch-konto. När programmet gör en begäran mot hello Batch-tjänsten, autentiserar hello-begäran med hello Azure Batch-kontonamn, hello Webbadressen för hello-konto och en snabbtangent. Du kan [skapa Batch-konto](batch-account-create-portal.md) i hello Azure-portalen.
* **Storage-konto** – Batch innehåller inbyggt stöd för att arbeta med filer i [Azure Storage][azure_storage]. Nästan alla Batch-scenario används Azure Blob storage för mellanlagring hello-program som körs i dina aktiviteter och hello data som de bearbeta och hello lagring av utdata som har skapats. toocreate ett lagringskonto finns [om Azure storage-konton](../storage/common/storage-create-storage-account.md).

## <a name="batch-service-apis"></a>API:er för Batch-tjänst

Dina program och tjänster kan utfärda direkt REST API-anrop eller använda en eller flera av följande klienten bibliotek toorun hello och hantera Azure Batch-arbetsbelastningar.

| API | API-referens | Ladda ned | Självstudier | Kodexempel | Mer information |
| --- | --- | --- | --- | --- | --- |
| **REST för Batch** |[MSDN][batch_rest] |Saknas |- |- | [Versioner som stöds](https://docs.microsoft.com/rest/api/batchservice/batch-service-rest-api-versioning) |
| **.NET för Batch** |[docs.microsoft.com][api_net] |[NuGet ][api_net_nuget] |[Självstudie](batch-dotnet-get-started.md) |[GitHub][api_sample_net] | [Viktig information](http://aka.ms/batch-net-dataplane-changelog) |
| **Python för Batch** |[readthedocs.io][api_python] |[PyPI][api_python_pypi] |[Självstudie](batch-python-tutorial.md)|[GitHub][api_sample_python] | [Viktigt](https://github.com/Azure/azure-sdk-for-python/blob/master/doc/batch.rst) |
| **Batch Node.js** |[github.io][api_nodejs] |[npm][api_nodejs_npm] |- |- | [Viktigt](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/batch) |
| **Batch Java** |[github.io][api_java] |[Maven][api_java_jar] |- |[Viktigt][api_sample_java] | [Viktigt](https://github.com/Azure/azure-batch-sdk-for-java)|

## <a name="batch-management-apis"></a>API:er för Batch Management

hello Azure Resource Manager API: erna för Batch ger Programmeringsåtkomst tooBatch konton. Med hjälp av dessa API:er kan du programmatiskt hantera Batch-konton, kvoter och programpaket.  

| API | API-referens | Ladda ned | Självstudier | Kodexempel |
| --- | --- | --- | --- | --- |
| **Batch Resource Manager REST** |[docs.microsoft.com][api_rest_mgmt] |Saknas |- |[GitHub](https://github.com/Azure-Samples/batch-dotnet-manage-batch-accounts) |
| **Batch Resource Manager .NET** |[docs.microsoft.com][api_net_mgmt] |[NuGet ][api_net_mgmt_nuget] | [Självstudie](batch-management-dotnet.md) |[GitHub][api_sample_net] |

## <a name="batch-command-line-tools"></a>Batch kommandoradsverktyg

Dessa verktyg ger hello samma funktioner som hello Batch-tjänsten och Batch Management API: er: 

* [Batch-PowerShell-cmdlets][batch_ps]: hello Azure Batch-cmdletar i hello [Azure PowerShell](/powershell/azure/overview) modulen aktivera toomanage Batch resurser med PowerShell.
* [Azure CLI](/cli/azure/overview): hello Azure-kommandoradsgränssnittet (Azure CLI) är en plattformsoberoende verktyg som ger shell-kommandon för att interagera med många Azure-tjänster, inklusive hello Batch-tjänsten och Batch Management-tjänsten. Se [hantera Batch-resurser med Azure CLI](batch-cli-get-started.md) för mer information om hur du använder hello Azure CLI med Batch.

## <a name="other-tools-for-application-development"></a>Andra verktyg för programutveckling

Här är några fler användbara verktyg för att skapa och felsöka Batch-program och tjänster:

* [Azure-portalen][portal]: du kan skapa, övervaka och ta bort Batch pooler, jobb och aktiviteter i hello Azure portalens Batch blad. Du kan visa hello statusinformation för dessa och andra resurser när du kör dina jobb och även hämta filer från hello compute-noder i din pooler. Du kan till exempel ladda ned en misslyckad uppgifts `stderr.txt` vid felsökning. Du kan också hämta RDP (Remote Desktop) filer som du kan använda toolog toocompute noder.
* [Azure Batch Explorer][batch_explorer]: Batch Explorer ger liknande funktionalitet för Batch-resurs som hello Azure-portalen, men i en fristående klientprogrammet för Windows Presentation Foundation (WPF). En av hello Batch .NET exempelprogram finns på [GitHub][github_samples], kan du skapa den med Visual Studio 2015 eller nyare och använda den toobrowse och hantera hello resurser i Batch-kontot när du utvecklar och felsöka Batch-lösningar. Visa jobb, pool och aktivitetsinformation, hämta filer från datornoder och fjärranslutning toonodes med hjälp av fjärrskrivbord (RDP)-filer som du kan hämta med Batch Explorer.
* [Microsoft Azure Lagringsutforskaren][storage_explorer]: men inte enbart ett Azure Batch-verktyg, hello Lagringsutforskaren är en annan ovärderligt verktyg toohave när du utvecklar och felsökning av Batch-lösningar.

## <a name="additional-resources"></a>Ytterligare resurser

- toolearn om loggning av händelser från Batch-program finns [loggar händelser för diagnostiska utvärdering och övervakning av Batch-lösningar](batch-diagnostics.md). En referens för händelser som skapats av hello Batch-tjänsten finns [Batchanalyser](batch-analytics.md).
- Information om miljövariabler för beräkningsnoder finns i [Miljövariabler för Azure Batch-beräkningsnod](batch-compute-node-environment-variables.md).

## <a name="next-steps"></a>Nästa steg

* Läs hello [Batch funktionsöversikt för utvecklare](batch-api-basics.md), viktig information för alla förbereder toouse Batch. hello artikeln innehåller mer detaljerad information om Batch-tjänsten resurser som pooler, noder, jobb och uppgifter och hello många API-funktioner som du kan använda när du skapar Batch-program.
* [Kom igång med hello Azure Batch-biblioteket för .NET](batch-dotnet-get-started.md) toolearn hur toouse C# och hello Batch .NET-biblioteket tooexecute en enkel arbetsbelastning med hjälp av en gemensam Batch-arbetsflöde. Den här artikeln ska vara någon av dina första stopp då du lär dig hur toouse hello Batch-tjänsten. Det finns också en [Python-versionen](batch-python-tutorial.md) hello kursen.
* Hämta hello [kodexempel på GitHub] [ github_samples] toosee hur både C# och Python kan samverka med Batch tooschedule och processen exempel arbetsbelastningar.
* Kolla in hello [Batch Utbildningsväg] [ learning_path] tooget en uppfattning om hello resurser tillgängliga tooyou som du lär dig toowork med Batch.


[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_rest_mgmt]: https://docs.microsoft.com/\rest/api/batchmanagement/
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: http://azure.github.io/azure-sdk-for-node/azure-batch/latest/
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: /powershell/resourcemanager/azurerm.batch/v2.7.0/azurerm.batch
[batch_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[storage_explorer]: http://storageexplorer.com/
[portal]: https://portal.azure.com
