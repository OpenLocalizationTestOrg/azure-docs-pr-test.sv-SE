---
title: "slutföra jobb och uppgifter tooa data aaaPersist resultat eller loggar från store - Azure Batch | Microsoft Docs"
description: "Lär dig mer om olika alternativ för bestående av utdata från batchuppgifter och jobb. Du kan spara data tooAzure lagring eller tooanother data lagras."
services: batch
author: tamram
manager: timlt
editor: 
ms.assetid: 16e12d0e-958c-46c2-a6b8-7843835d830e
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/16/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f0b11e387f1694e1ce3e9573db7f6013f0154cad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="persist-job-and-task-output"></a>Bevara jobb- och uppgiftsutdata

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Vissa vanliga exempel på utdata för aktiviteten:

- Filer som skapas när hello uppgiften processer indata.
- Loggfiler som är associerade med körning av aktiviteten. 

Den här artikeln beskrivs olika alternativ för bestående uppgiften utdata och hello som varje alternativ passar bäst.   

## <a name="about-hello-batch-file-conventions-standard"></a>Om hello Batch filen konventioner standard

Batch definierar en valfri uppsättning namnkonventionerna för aktiviteten utdatafilerna i Azure Storage. Hej [Batch filen konventioner standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) beskriver dessa regler. hello filen konventioner standard avgör hello namnen på hello-behållaren och blob målsökväg i Azure Storage för en given utdatafilen baserat på hello namnen på hello projekt- och.

Är det upp tooyou om du bestämmer dig för toouse hello filen konventioner standard för namngivning av datafilerna utdata. Du kan också namnge hello målbehållare och blob, men du vill. Om du använder hello filen konventioner standard för namngivning av filer och utgående filer är tillgängliga för visning i hello [Azure-portalen][portal].

Det finns några olika sätt som du kan använda hello filen konventioner standard:

- Om du använder hello service API toopersist utdata kommandofiler du tooname mål behållare och blobbar enligt toohello filen konventioner som standard. hello API för Batch-tjänsten kan du toopersist utdatafilerna från klientkoden, utan att ändra tillämpningsprogrammet aktivitet.
- Om du utvecklar med .NET, kan du använda hello [filen konventioner för Azure Batch-biblioteket för .NET][nuget_package]. En fördel med det här biblioteket är att den stöder fråga din utdatafilerna enligt tootheir ID eller syfte. hello inbyggd frågar funktion gör det enkelt tooaccess utdatafilerna från ett klientprogram eller andra uppgifter. Tillämpningsprogrammet uppgiften måste dock vara ändrade toocall filen konventioner bibliotek. Mer information finns i hello referens för hello [filen konventioner biblioteket för .NET](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.aspx).
- Om du utvecklar med ett annat språk än .NET, kan du implementera hello filen konventioner som standard i ditt program.

## <a name="design-considerations-for-persisting-output"></a>Designöverväganden för att spara utdata 

När du skapar Batch-lösning bör överväga hello följande faktorer relaterade toojob och resultat av åtgärder.

* **Compute-nod livstid**: Compute-noder är ofta övergående, särskilt i Autoskala-aktiverade pooler. Utdata från en aktivitet som körs på en nod är bara tillgänglig när hello noden finns och endast inom kvarhållningsperioden för hello-filen som har angetts för hello-aktivitet. Om en aktivitet producerar utdata som kan behövas när hello åtgärden är klar, överför hello uppgiften utgående filer tooa beständiga arkivet, t.ex Azure Storage.

* **Utdata lagring**: Azure Storage rekommenderas som ett datalager för uppgiftens utdata, men du kan använda alla beständig lagring. Skriva uppgiften utdata tooAzure är lagring integrerad i hello API för Batch-tjänsten. Om du använder en annan form av beständiga lagring måste toowrite hello programmet logik toopersist uppgiftens utdata själv.   

* **Utdata hämtning**: du kan hämta uppgiftens utdata direkt från hello beräkningsnoder i din pool eller från Azure Storage eller ett annat datalager om du har sparat uppgiftens utdata. tooretrieve en uppgift utdata direkt från en beräkningsnod behöver du hello filnamnet och dess platsen på hello-nod. Om du bevara uppgiften utdata tooAzure lagring måste hello fullständig sökväg toohello filen i Azure Storage toodownload hello utdatafilerna med hello Azure Storage SDK: N.

* **Visa utdata**: när du navigerar tooa batchaktiviteten i hello Azure portal och välj **filer på noden**, visas alla filer som hör till hello aktivitet, inte bara hello utdatafiler som du är intresserad av. Filer på datornoderna finns igen, medan hello noden finns och endast inom kvarhållningstiden för hello-fil som har angetts för hello-aktivitet. tooview aktivitet utdata som du har sparat tooAzure lagring, kan du använda hello Azure-portalen eller ett Azure Storage client-program, till exempel hello [Azure Lagringsutforskaren][storage_explorer]. tooview utdata i Azure Storage med hello portal eller något annat verktyg, måste du veta hello filens plats och navigera tooit direkt.

## <a name="options-for-persisting-output"></a>Alternativ för att spara utdata

Det finns flera olika metoder som du kan vidta toopersist uppgiftsutdata beroende på ditt scenario:

- Använd API för hello Batch-tjänsten.  
- Använd hello Batch filen konventioner biblioteket för .NET.  
- Implementera hello Batch filen konventioner standard i ditt program.
- Implementera en lösning för flytt av anpassade filen.

hello följande avsnitt beskrivs varje metod i detalj.

### <a name="use-hello-batch-service-api"></a>Använd API för hello Batch-tjänsten

Med version 2017-05-01 hello Batch-tjänsten lägger till stöd för att ange utdatafilerna i Azure Storage för aktivitetsdata när du [lägga till ett jobb för aktiviteten tooa](https://docs.microsoft.com/rest/api/batchservice/add-a-task-to-a-job) eller [lägga till en samling aktiviteter tooa jobbet](https://docs.microsoft.com/rest/api/batchservice/add-a-collection-of-tasks-to-a-job).

hello API för Batch-tjänsten stöder bestående aktivitet data tooan Azure Storage-konto från poolen som skapats med hello konfiguration av virtuell dator. Med hello API för Batch-tjänsten bevara du aktivitetsdata utan att ändra hello-program som uppgiften körs. Alternativt kan du följa toohello [Batch filen konventioner standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) för namngivning av hello filer som du bevara tooAzure lagring. 

Använd hello API för Batch-tjänsten toopersist aktiviteten utdata när:

- Vill du toopersist data från Batch-aktiviteter och jobb manager aktiviteter i pooler som har skapats med hello konfiguration av virtuell dator.
- Vill du toopersist data tooan Azure Storage-behållare med ett godtyckligt namn.
- Du vill toopersist data tooan Azure Storage-behållare med namnet bl.a toohello [Batch filen konventioner standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). 

> [!NOTE]
> hello API för Batch-tjänsten stöder inte bestående data från aktiviteter som körs i pooler som har skapats med hello cloud service configuration. Information om bestående uppgiften utdata från pooler kör hello cloud services-konfigurering finns [spara projekt- och data tooAzure lagring med hello Batch filen konventioner biblioteket för .NET toopersist](batch-task-output-file-conventions.md)
> 
> 

Mer information om bestående uppgiftens utdata med hello API för Batch-tjänsten finns [spara aktiviteten data tooAzure lagring med hello API för Batch-tjänsten](batch-task-output-files.md). Se även hello [PersistOutputs] [github_persistoutputs] exempel projekt på GitHub, som visar hur toouse hello Batch-klientbibliotek för .NET toopersist aktivitet utdata toodurable lagring.

### <a name="use-hello-batch-file-conventions-library-for-net"></a>Använd hello Batch filen konventioner biblioteket för .NET

Skapa Batch lösningar med C# och .NET-utvecklare kan använda hello [filen konventioner biblioteket för .NET] [ nuget_package] toopersist aktivitet data tooan Azure Storage-konto, enligt toohello [kommandofil Konventioner standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). hello filen konventioner biblioteket hanterar glidande utgående filer tooAzure lagring och namngivning mål behållare och blobbar på kända sätt.

hello filen konventioner biblioteket har stöd för frågor utdatafilerna efter ID eller syfte, slutföra vilket gör det enkelt toolocate dem utan att behöva hello filen URI: er. 

Använd hello Batch filen konventioner biblioteket för .NET toopersist aktivitet utdata när:

- Du vill toostream data tooAzure lagring medan hello aktivitet körs.
- Vill du toopersist data från poolen som skapats med hello molnet tjänstkonfiguration eller hello konfiguration av virtuell dator.
- Klientprogrammet eller andra uppgifter i hello jobb behov toolocate och hämta utdata aktivitetsfiler genom ID eller syfte. 
- Vill du tooperform Kontrollera pekar eller tidig överföring av första resultaten.
- Vill du tooview uppgiftens utdata i hello Azure-portalen.

Mer information om bestående uppgiftens utdata med hello filen konventioner bibliotek för .NET finns [spara projekt- och data tooAzure lagring med hello Batch filen konventioner biblioteket för .NET-toopersist ](batch-task-output-file-conventions.md). Se även hello [PersistOutputs] [github_persistoutputs] exempel projekt på GitHub, som visar hur toouse hello filen konventioner biblioteket för .NET toopersist aktivitet utdata toodurable lagring.

Hej visar [PersistOutputs] [github_persistoutputs] exempelprojektet på GitHub hur toouse hello Batch-klientbibliotek för .NET toopersist aktivitet utdata toodurable lagring.

### <a name="implement-hello-batch-file-conventions-standard"></a>Implementera hello Batch filen konventioner standard

Om du använder ett annat språk än .NET, kan du implementera hello [Batch filen konventioner standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) i ditt eget program. 

Du kanske vill tooimplement hello filen konventioner namngivningsstandarden själv om du vill ha en beprövad namngivningsschemat, eller om du vill tooview uppgiftens utdata i hello Azure-portalen.

### <a name="implement-a-custom-file-movement-solution"></a>Implementera en lösning för flytt av anpassad fil

Du kan också implementera din egen lösning för flytt av hela filen. Använd detta närmar sig när:

- Vill du toopersist aktivitet tooa data dataarkiv än Azure Storage. tooupload tooa data som lagras som Azure SQL- eller Azure DataLake, kan du skapa ett anpassat skript eller körbara tooupload toothat plats. Du kan sedan anropa den på hello kommandorad när du har kört din primära körbar fil. Till exempel på en Windows-nod, kan du anropa dessa två kommandon:`doMyWork.exe && uploadMyFilesToSql.exe`
- Vill du tooperform Kontrollera pekar eller tidig överföring av första resultaten.
- Du vill toomaintain detaljerad kontroll över felhantering. Du kanske till exempel tooimplement din egen lösning om du vill toouse uppgiften beroende åtgärder tootake vissa överför åtgärder baserat på en viss uppgift slutkoder. Mer information om aktiviteten beroende åtgärder finns i [och skapa aktiviteten beroenden toorun uppgifter som är beroende av andra aktiviteter](batch-task-dependencies.md). 

## <a name="next-steps"></a>Nästa steg

- Utforska använda hello nya funktioner i hello API för Batch-tjänsten toopersist aktivitetsdata i [spara aktiviteten data tooAzure lagring med hello API för Batch-tjänsten](batch-task-output-files.md).
- Lär dig mer om hur du använder hello Batch filen konventioner biblioteket för .NET i [spara projekt- och data tooAzure lagring med hello Batch filen konventioner biblioteket för .NET-toopersist ](batch-task-output-file-conventions.md).
- Se hello [PersistOutputs] [github_persistoutputs] exempelprojektet på GitHub, som visar hur toouse både hello Batch-klientbibliotek för .NET och hello filen konventioner bibliotek för .NET toopersist aktivitet utdata toodurable lagring.

[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/
