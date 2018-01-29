---
title: "Azure Batch API:er och verktyg för utvecklare | Microsoft Docs"
description: "Läs mer om API:er och verktyg som är tillgängliga för utveckling av lösningar med Azure Batch-tjänsten."
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: get-started-article
ms.date: 10/12/2017
ms.author: tamram
ms.openlocfilehash: a17856013c8db2e671b8f5201fbcc9223953ab6f
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/20/2017
---
# <a name="overview-of-batch-apis-and-tools"></a>Översikt över Batch-API:er och verktyg

Bearbetning av parallella arbetsbelastningar med Azure Batch görs normalt programmässigt med någon av [Batch-API:erna](#batch-development-apis). Ditt klientprogram eller -tjänst kan använda Batch-API:erna för att kommunicera med Batch-tjänsten. Med Batch-API:erna kan du skapa och hantera pooler för beräkningsnoder, antingen virtuella datorer eller molntjänster. Sedan kan du schemalägga jobb och uppgifter som ska köras på dessa noder. 

Du kan effektivt bearbeta storskaliga arbetsbelastningar för din organisation eller tillhandahålla en tjänstportal för dina kunder så att de kan köra jobb och aktiviteter (på begäran eller enligt ett schema) på en eller flera hundra tusen noder. Du kan också använda Azure Batch som en del av ett större arbetsflöde, med verktyg som [Azure Data Factory](../data-factory/v1/data-factory-data-processing-using-batch.md?toc=%2fazure%2fbatch%2ftoc.json).

> [!TIP]
> När du är redo att fördjupa dig i Batch-API:et rekommenderar vi att du börjar med [Batch-funktionsöversikten för utvecklare](batch-api-basics.md).
> 
> 

## <a name="azure-accounts-for-batch-development"></a>Azure-konton för Batch-utveckling
När du utvecklar Batch-lösningar, använder du följande konton i Microsoft Azure.

* **Azure-konto och -prenumeration** – om du inte redan har en Azure-prenumeration, kan du aktivera din [Visual Studio-prenumerantförmån][msdn_benefits], eller registrera dig för ett [kostnadsfritt Azure-konto][free_account]. När du skapar ett konto skapas en standardprenumeration åt dig.
* **Batch-konto** – Azure Batch-resurser, inklusive pooler, beräkningsnoder, jobb och aktiviteter, är associerade med ett Azure Batch-konto. När ditt program gör en begäran mot batchtjänsten, autentiserar det begäran med hjälp av Azure Batch-kontonamnet, URL:en för kontot och antingen en åtkomstnyckel eller en Azure Active Directory-token. Du kan [skapa ett Batch-konto](batch-account-create-portal.md) på Azure-portalen, eller programmässigt.
* **Storage-konto** – Batch innehåller inbyggt stöd för att arbeta med filer i [Azure Storage][azure_storage]. Nästan alla Batch-scenarier använder Azure Blob Storage, dels för att mellanlagra programmen som dina aktiviteter kör och de data som de bearbetar, dels för att lagra de utdata som de genererar. Om du vill skapa ett Storage-konto läser du [Om Azure Storage-konton](../storage/common/storage-create-storage-account.md).

## <a name="batch-service-apis"></a>API:er för Batch-tjänst

Dina program och tjänster kan skicka direkta REST-API-anrop eller använda ett eller flera av följande klientbibliotek för att köra och hantera dina Azure Batch-arbetsbelastningar.

| API | API-referens | Ladda ned | Självstudier | Kodexempel | Mer information |
| --- | --- | --- | --- | --- | --- |
| **REST för Batch** |[docs.microsoft.com][batch_rest] |Saknas |- |- | [Versioner som stöds](/rest/api/batchservice/batch-service-rest-api-versioning) |
| **.NET för Batch** |[docs.microsoft.com][api_net] |[NuGet ][api_net_nuget] |[Självstudie](batch-dotnet-get-started.md) |[GitHub][api_sample_net] | [Viktig information](http://aka.ms/batch-net-dataplane-changelog) |
| **Python för Batch** |[readthedocs.io][api_python] |[PyPI][api_python_pypi] |[Självstudie](batch-python-tutorial.md)|[GitHub][api_sample_python] | [Viktigt](https://github.com/Azure/azure-sdk-for-python/blob/master/doc/batch.rst) |
| **Batch Node.js** |[docs.microsoft.com][api_nodejs] |[npm][api_nodejs_npm] |[Självstudie](batch-nodejs-get-started.md) |- | [Viktigt](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/batch) |
| **Batch Java** |[github.io][api_java] |[Maven][api_java_jar] |- |[Viktigt][api_sample_java] | [Viktigt](https://github.com/Azure/azure-batch-sdk-for-java)|

## <a name="batch-management-apis"></a>API:er för Batch Management

Azure Resource Manager-API:erna för Batch ger programmatisk åtkomst till Batch-konton. Med hjälp av dessa API:er kan du programmatiskt hantera Batch-konton, kvoter och programpaket.  

| API | API-referens | Ladda ned | Självstudier | Kodexempel |
| --- | --- | --- | --- | --- |
| **Batch Resource Manager REST** |[docs.microsoft.com][api_rest_mgmt] |Saknas |- |[GitHub](https://github.com/Azure-Samples/batch-dotnet-manage-batch-accounts) |
| **Batch Resource Manager .NET** |[docs.microsoft.com][api_net_mgmt] |[NuGet ][api_net_mgmt_nuget] | [Självstudie](batch-management-dotnet.md) |[GitHub][api_sample_net] |

## <a name="batch-command-line-tools"></a>Batch kommandoradsverktyg

Dessa kommandoradsverktyg innehåller samma funktioner som Batch-tjänsten och API:erna för Batch Management: 

* [PowerShell-cmdlets för Batch ][batch_ps]: Du kan använda Azure Batch-cmdlets i [Azure PowerShell](/powershell/azure/overview)-modulen om du vill hantera Batch-resurser med PowerShell.
* [Azure CLI 2.0](/cli/azure/overview): Azure-kommandoradsgränssnittet (Azure CLI) är en plattformsoberoende verktygsuppsättning som tillhandahåller shell-kommandon för att interagera med flera Azure-tjänster, inklusive Batch-tjänsten och Batch Management-tjänsten. Mer information om hur du använder Azure CLI med Batch finns i [Hantera Batch-resurser med Azure CLI](batch-cli-get-started.md).

## <a name="other-tools-for-application-development"></a>Andra verktyg för programutveckling

Här är några fler användbara verktyg för att skapa och felsöka Batch-program och tjänster:

* [Azure Portal][portal]: Du kan skapa, övervaka och ta bort Batch-pooler, jobb och uppgifter i Azure Portal. Du kan visa statusinformation för dessa och andra resurser när du kör dina jobb, och även ladda ned filer från beräkningsnoderna i dina pooler. Du kan till exempel ladda ned en misslyckad uppgifts `stderr.txt` vid felsökning. Du kan också hämta fjärrskrivbordsfiler (RDP) som du kan använda för att logga in till beräkningsnoder.
* [Azure BatchLabs][batch_labs]: BatchLabs är ett kostnadsfritt, fristående klientverktyg med omfattande funktioner för att skapa, felsöka och övervaka Azure Batch-program. Hämta ett [installationspaketet](https://azure.github.io/BatchLabs/) för Mac, Linux eller Windows.
* [Microsoft Azure Storage Explorer][storage_explorer]: Även om det strikt sett inte är ett Azure Batch-verktyg är Storage Explorer ett annat värdefullt verktyg som du kan använda när du utvecklar och felsöker dina Batch-lösningar.

## <a name="additional-resources"></a>Ytterligare resurser

- Mer information om loggning av händelser från Batch-programmet finns i [Log events for diagnostic evaluation and monitoring of Batch solutions](batch-diagnostics.md) (Logga händelser för diagnostisk utvärdering och övervakning av Batch-lösningar). Information om händelser som utlöses av Batch-tjänsten finns i [Batchanalys](batch-analytics.md).
- Information om miljövariabler för beräkningsnoder finns i [Miljövariabler för Azure Batch-beräkningsnod](batch-compute-node-environment-variables.md).

## <a name="next-steps"></a>Nästa steg

* Läs [Översikt över Batch-funktioner för utvecklare](batch-api-basics.md). Här finns viktig information för alla som tänker använda Batch. Artikeln innehåller mer detaljerad information om Batch-tjänstresurser som pooler, noder, jobb och uppgifter, och de många API-funktioner som du kan använda när du skapar ett Batch-program.
* [Komma igång med Azure Batch-biblioteket för .NET](batch-dotnet-get-started.md) innehåller information om hur du använder C# och Batch .NET-biblioteket för att köra en enkel arbetsbelastning med ett vanligt Batch-arbetsflöde. Den här artikeln bör vara en av dina självklara utgångspunkter när du lär dig hur man använder Batch-tjänsten. En [Python-version](batch-python-tutorial.md) och en [Node.js](batch-nodejs-get-started.md)-version av guiden finns också tillgängliga.
* Hämta [kodexemplet på GitHub][github_samples] om du vill se hur både C# och Python kan användas med Batch för att schemalägga och bearbeta exempelarbetsbelastningar.


[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: /dotnet/api/overview/azure/batch/client
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_rest_mgmt]: /rest/api/batchmanagement/
[api_net_mgmt]: /dotnet/api/overview/azure/batch/management
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: /nodejs/api/overview/azure/batch
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: /powershell/module/azurerm.batch/
[batch_rest]: /rest/api/batchservice/
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_labs]: https://azure.github.io/BatchLabs/
[storage_explorer]: http://storageexplorer.com/
[portal]: https://portal.azure.com
