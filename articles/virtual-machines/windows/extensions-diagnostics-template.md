---
title: "aaaAdd övervakning och diagnostik tooan virtuell Azure-dator | Microsoft Docs"
description: "Använda en Azure resource manager-mall toocreate en ny virtuell dator för Windows Azure-diagnostik-tillägget."
services: virtual-machines-windows
documentationcenter: 
author: sbtron
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8cde8fe7-977b-43d2-be74-ad46dc946058
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: saurabh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d8a831421a0f9d38c09d51cf8c2e6dff913c77ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-monitoring-and-diagnostics-with-a-windows-vm-and-azure-resource-manager-templates"></a>Använd övervakning och diagnostik med en virtuell Windows-dator och Azure Resource Manager-mallar
hello Azure Diagnostics tillägget innehåller hello övervakning och diagnostik-funktioner på en Windows-baserade virtuella Azure-datorn. Du kan aktivera dessa funktioner på hello virtuella datorn genom att inkludera hello-tillägget som en del av hello azure resource manager-mall. Se [redigera Azure Resource Manager-mallar med VM-tillägg](template-description.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#extensions) mer information om inklusive alla tillägg som en del av en mall för virtuella datorer. Den här artikeln beskriver hur du kan lägga till hello Azure Diagnostics tillägget tooa windows virtuell Datormall.  

## <a name="add-hello-azure-diagnostics-extension-toohello-vm-resource-definition"></a>Lägg till hello Azure Diagnostics tillägget toohello VM resursdefinitionen
tooenable hello diagnostik tillägg på en Windows-dator måste tooadd hello-tillägget som en VM-resurs i hello Resource manager-mall.

För en enkel Resource Manager baserat virtuella lägga till hello-tillägget configuration toohello *resurser* matris för hello virtuell dator: 

    "resources": [
                {
                    "name": "Microsoft.Insights.VMDiagnosticsSettings",
                    "type": "extensions",
                    "location": "[resourceGroup().location]",
                    "apiVersion": "2015-06-15",
                    "dependsOn": [
                        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
                    ],
                    "tags": {
                        "displayName": "AzureDiagnostics"
                    },
                    "properties": {
                        "publisher": "Microsoft.Azure.Diagnostics",
                        "type": "IaaSDiagnostics",
                        "typeHandlerVersion": "1.5",
                        "autoUpgradeMinorVersion": true,
                        "settings": {
                            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), variables('vmName'), variables('wadcfgxend')))]",
                            "storageAccount": "[parameters('existingdiagnosticsStorageAccountName')]"
                        },
                        "protectedSettings": {
                            "storageAccountName": "[parameters('existingdiagnosticsStorageAccountName')]",
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-05-01-preview').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                        }
                    }
                }
            ]


En annan vanliga konventionen är lägga till hello tilläggets konfiguration hello resurser rotnoden i hello mall istället för att definiera under hello den virtuella datorns resurser noden. Med den här metoden har tooexplicitly ange en hierarkisk relation mellan hello-tillägget och hello virtuell dator med hello *namn* och *typen* värden. Exempel: 

    "name": "[concat(variables('vmName'),'Microsoft.Insights.VMDiagnosticsSettings')]",
    "type": "Microsoft.Compute/virtualMachines/extensions",

hello tillägget alltid är associerad med hello virtuell dator kan du antingen direkt definiera under hello virtuella datorns resursnoden direkt eller definiera på hello basnivån och använda hello hierarkiska naming convention tooassociate med hello virtuella datorn.

För virtuella datorer hello-tillägg-konfiguration har angetts i hello *extensionProfile* -egenskapen för hello *VirtualMachineProfile*.

hello *publisher* egenskapen med värdet hello **Microsoft.Azure.Diagnostics** och hello *typen* egenskapen med värdet hello **IaaSDiagnostics** identifiera hello Azure Diagnostics tillägg.

Hej värdet för hello *namn* egenskapen kan ha använt toorefer toohello tillägget hello resursgruppen. Om du anger specifikt för**Microsoft.Insights.VMDiagnosticsSettings** kommer det att toobe som enkelt kan identifieras av Azure portal säkerställt hello som hello övervakning diagram visas korrekt i hello Azure-portalen.

Hej *typeHandlerVersion* anger hello version hello-tillägget som toouse. Ange *autoUpgradeMinorVersion* lägre version för**SANT** garanterar att du får hello senaste lägre version av hello-tillägg som är tillgängliga. Vi rekommenderar att du alltid anger *autoUpgradeMinorVersion* tooalways vara **SANT** så att du alltid får toouse hello senaste tillgängliga diagnostik tillägg med alla hello nya funktioner och programfel korrigeringar. 

Hej *inställningar* element innehåller konfigurationer egenskaper för hello-tillägg som kan ange och läsa tillbaka från hello-tillägg (ibland hänvisade tooas offentliga konfiguration). Hej *xmlcfg* egenskap innehåller XML-baserad konfiguration för hello diagnostik loggar prestandaräknare etc hello diagnostik agent kommer att samlas in. Se [diagnostik Konfigurationsschemat](https://msdn.microsoft.com/library/azure/dn782207.aspx) för mer information om hello XML-schema sig själv. Ett vanligt är toostore hello faktiska xml-konfigurationen som en variabel i hello Azure Resource Manager-mall och sedan Sammanfoga och base64 koda dem tooset hello värde för *xmlcfg*. Avsnittet hello på [diagnostik configuration variabler](#diagnostics-configuration-variables) toounderstand mer om hur toostore hello xml i variabler. Hej *storageAccount* egenskapen anger hello namnet på hello storage-konto toowhich diagnostikdata överförs. 

Hej egenskaper i *protectedSettings* (ibland hänvisade tooas privata konfiguration) kan ställas in men går inte att läsa efter ett anges. Hej lässkyddad uppbyggnad *protectedSettings* kan användas för att lagra hemligheter som hello lagringskontonyckel där hello diagnostikdata ska skrivas.    

## <a name="specifying-diagnostics-storage-account-as-parameters"></a>Ange diagnostiklagringskonto som parametrar
hello diagnostik tillägget json fragment ovan förutsätter att två parametrar *existingdiagnosticsStorageAccountName* och *existingdiagnosticsStorageResourceGroup* toospecify hello diagnostik Storage-konto där diagnostikdata ska lagras. Att ange hello diagnostiklagringskonto som en parameter gör det enkelt toochange hello diagnostiklagringskonto mellan olika miljöer t.ex. du kanske toouse en annan diagnostiklagringskonto för testning och en för din Produktionsdistribution.  

        "existingdiagnosticsStorageAccountName": {
            "type": "string",
            "metadata": {
        "description": "hello name of an existing storage account toowhich diagnostics data will be transfered."
            }        
        },
        "existingdiagnosticsStorageResourceGroup": {
            "type": "string",
            "metadata": {
        "description": "hello resource group for hello storage account specified in existingdiagnosticsStorageAccountName"
              }
        }

Det är bästa praxis toospecify en diagnostiklagringskonto i en annan resursgrupp än hello resursgruppen för hello virtuella datorn. Toobe en distributionsenhet med sin egen livstid kan betraktas som en resursgrupp, en virtuell dator kan distribueras och omdistribueras som nya konfigurationer uppdateringar görs den tooit men du kanske vill lagra hello diagnostikdata i hello toocontinue samma lagringskonto i dessa distributioner på virtuella datorer. Med hello storage-konto i en annan resurs aktiverar hello storage-konto tooaccept data från olika distribution av virtuella datorer som gör det enkelt tootroubleshoot hello problem mellan olika versioner.

> [!NOTE]
> Om du skapar en mall för virtuell dator av windows från Visual Studio hello standardkontot för lagring kan anges hello toouse samma lagringskonto där hello virtuella datorns virtuella Hårddisk har överförts. Detta är toosimplify installationen av hello VM. Du bör ändra faktorerna hello mallen toouse ett annat lagringskonto som kan skickas som en parameter. 
> 
> 

## <a name="diagnostics-configuration-variables"></a>Variabler för diagnostik-konfiguration
hello diagnostik tillägget json fragment ovan definierar en *accountid* variabeln toosimplify komma hello lagringskontonyckel för hello diagnostik lagring:   

    "accountid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/',parameters('existingdiagnosticsStorageResourceGroup'), '/providers/','Microsoft.Storage/storageAccounts/', parameters('existingdiagnosticsStorageAccountName'))]"


Hej *xmlcfg* egenskapen för hello diagnostik tillägg har definierats med flera variabler som sammanfogas tillsammans. hello värdena för dessa variabler finns i xml så att de behöver toobe (escaped) korrekt när hello json variabler.

hello nedan beskrivs hello diagnostik xml-konfigurationsfilen som samlar in standard system nivån prestandaräknare tillsammans med vissa windows-händelseloggar och diagnostik infrastruktur loggar. Det har undantaget och formaterats korrekt så att hello konfiguration kan direkt klistras in i hello variabler avsnitt i mallen. Se hello [diagnostik Konfigurationsschemat](https://msdn.microsoft.com/library/azure/dn782207.aspx) en mer mänsklig läsbar exempel på hello konfigurations-xml.

        "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounters1": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Privileged Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU privileged time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% User Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU user time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor Information(_Total)\\Processor Frequency\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"CPU frequency\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\System\\Processes\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Processes\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Threads\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Handle Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Handles\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\% Committed Bytes In Use\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Memory usage\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Available Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory available\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Committed Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory committed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Commit Limit\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory commit limit\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active time\" locale=\"en-us\"/></PerformanceCounterConfiguration>",
        "wadperfcounters2": "<PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Read Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active read time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Write Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active write time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Transfers/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Reads/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk read operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Writes/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk write operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Read Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk read speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Write Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk write speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\LogicalDisk(_Total)\\% Free Space\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk free space (percentage)\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters1'), variables('wadperfcounters2'), '<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name , '/providers/', 'Microsoft.Compute/virtualMachines/')]",
        "wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>"

hello mått definition xml-nod i hello ovan konfiguration är ett viktigt konfigurationselement som definierar hur hello prestandaräknare definierats tidigare i hello xml i *PerformanceCounter* noden kommer att slås samman och lagras. 

> [!IMPORTANT]
> De här måtten enheten hello övervakning diagram och aviseringar i hello Azure-portalen.  Hej **mått** nod med hello *resourceID* och **MetricAggregation** måste inkluderas i hello diagnostik konfigurationen för den virtuella datorn om du vill toosee hello VM övervakningsdata i hello Azure-portalen. 
> 
> 

hello följande är ett exempel på hello xml för definitioner av mått: 

        <Metrics resourceId="/subscriptions/subscription().subscriptionId/resourceGroups/resourceGroup().name/providers/Microsoft.Compute/virtualMachines/vmName">
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>

Hej *resourceID* attributet identifierar hello virtuell dator i din prenumeration. Se till att toouse hello subscription() och resourceGroup() fungerar så att hello mallen uppdateras automatiskt dessa värden baserat på hello-prenumeration och resurs-grupp som du distribuerar till.

Om du skapar flera virtuella datorer i en slinga så har du toopopulate hello *resourceID* värde med en copyIndex() funktionen toocorrectly skilja mellan varje enskild virtuell dator. Hej *xmlCfg* värdet kan vara uppdaterade toosupport detta på följande sätt:  

    "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), concat(parameters('vmNamePrefix'), copyindex()), variables('wadcfgxend')))]", 

Hej MetricAggregation värdet för *PT1H* och *PT1M* anger en sammanställning över en minut och en sammanställning över en timme.

## <a name="wadmetrics-tables-in-storage"></a>WADMetrics tabeller i lagring
hello mått configuration ovan skapar tabeller i din diagnostiklagringskonto med hello följande namngivningsregler:

* **WADMetrics** : Standard prefix för alla WADMetrics tabeller
* **PT1H** eller **PT1M** : innebär det att hello tabellen innehåller sammanställda data över 1 timme eller 1 minut
* **P10D** : innebär det att hello tabellen innehåller data för 10 dagar från när hello tabellen startat datainsamling
* **V2S** : strängkonstant
* **ÅÅÅÅMMDD** : hello startdatum på vilka hello tabell datainsamling

Exempel: *WADMetricsPT1HP10DV2S20151108* innehåller mått data visar det sammanlagda resultatet för 10 dagar från 2015-11-Nov i timmen    

Varje tabell WADMetrics innehåller hello följande kolumner:

* **PartitionKey**: hello partitionkey konstrueras utifrån hello *resourceID* värdet toouniquely identifiera hello Virtuella datorresursen. för t.ex.: 002Fsubscriptions:<subscriptionID>: 002FresourceGroups:002F<ResourceGroupName>: 002Fproviders:002FMicrosoft:002ECompute:002FvirtualMachines:002F<vmName>  
* **RowKey** : sätt hello format <Descending time tick>:<Performance Counter Name>. hello fallande skalstreck beräkningen är max tid tick minus hello tiden för hello början av hello aggregering period. T.ex. Om hello provtagningsperiod startat 10 Nov 2015 och 00:00Hrs UTC sedan hello beräkningen är: DateTime.MaxValue.Ticks - (nya DateTime(2015,11,10,0,0,0,DateTimeKind.Utc). Ticks). För hello minne tillgängliga byte prestandaräknaren hello radnyckel ser ut som: 2519551871999999999__:005CMemory:005CAvailable:0020 byte
* **CounterName** : hello namn för hello prestandaräknare. Detta matchar hello *counterSpecifier* definieras i hello xml-konfigurationen.
* **Maximal** : hello maximivärdet för prestandaräknaren för hello under hello aggregering.
* **Minsta** : hello lägsta värdet för prestandaräknaren för hello under hello aggregering.
* **Totalt antal** : hello summan av alla värden för hello prestandaräknare rapporterat under hello aggregering period.
* **Antal** : hello Totalt antal värden som rapporterats för hello prestandaräknaren.
* **Genomsnittlig** : hello medelvärde (totalt/antal) för prestandaräknaren för hello under hello aggregering.

## <a name="next-steps"></a>Nästa steg
* En fullständig exempelmall för en virtuell dator i Windows med diagnostik tillägg finns [201 och vm-övervakning-diagnostik-tillägg](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-monitoring-diagnostics-extension)   
* Distribuera hello resource manager mallen med hjälp av [Azure PowerShell](ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) eller [Azure Command Line](../linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* Lär dig mer om [redigera Azure Resource Manager-mallar](../../resource-group-authoring-templates.md)

