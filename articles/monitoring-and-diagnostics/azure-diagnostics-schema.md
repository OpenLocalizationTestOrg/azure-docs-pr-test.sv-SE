---
title: "aaaAzure diagnostik tillägget konfigurationsversionerna schema och historik | Microsoft Docs"
description: "Relevanta toocollecting prestandaräknarna i Azure Virtual Machines, Skalningsuppsättningar, Service Fabric och Cloud Services."
services: monitoring-and-diagnostics
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/16/2017
ms.author: robb
ms.openlocfilehash: 854ad118f660810aa38703670284794d658142c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-diagnostics-extention-configuration-schema-versions-and-history"></a>Azure Diagnostics-tillägget configuration schemat versioner och historik
Den här sidan index Azure Diagnostics tillägget schemat versioner levereras som en del av hello Microsoft Azure SDK.  

> [!NOTE]
> hello Azure Diagnostics tillägget är hello komponent används toocollect prestandaräknare och annan statistik från:
> - Azure Virtual Machines 
> - Skalningsuppsättningar för Virtual Machines
> - Service Fabric 
> - Molntjänster 
> - Nätverkssäkerhetsgrupper
> 
> Den här sidan gäller endast om du använder någon av dessa tjänster.

hello Azure Diagnostics tillägget används med andra Microsoft-produkter för diagnostik som Azure-Monitor, Application Insights och logganalys. Mer information finns i [övervakning verktyg översikt över Microsoft](monitoring-overview.md).

## <a name="azure-sdk-and-diagnostics-versions-shipping-chart"></a>Azure SDK och diagnostik versioner leverans diagram  

|Azure SDK-version | Version för diagnostik-tillägg | Modellen|  
|------------------|-------------------------------|------|  
|1.x               |1.0                            |plugin-program|  
|2.0 - 2.4         |1.0                            |plugin-program|  
|2.5               |1.2                            |Tillägg|  
|2.6               |1.3                            |"|  
|2.7               |1.4                            |"|  
|2.8               |1.5                            |"|  
|2.9               |1.6                            |"|
|2.96              |1.7                            |"|
|2.96              |1.8                            |"|
|2.96              |1.8.1                          |"|
|2.96              |1.9                            |"|



 Azure Diagnostics version 1.0 levererades först i en plugin-modell--vilket innebär att du fick hello versionen av Azure diagnostics som medföljer den när du installerade hello Azure SDK.  

 Från och med SDK 2.5 (diagnostics version 1.2), Azure-diagnostik har gått tooan tillägget modellen. nya funktioner för hello verktyg tooutilize endast var tillgängliga i den nyare Azure SDK, men alla tjänster med hjälp av Azure-diagnostik skulle hämta hello senaste leverans versionen direkt från Azure. Till exempel skulle alla som fortfarande använder SDK 2.5 laddas hello senaste versionen visas i föregående tabell hello oavsett om de använder hello nyare funktioner.  

## <a name="schemas-index"></a>Scheman index  
Konfiguration av olika scheman används för olika versioner av Azure-diagnostik. 

[Diagnostik 1.0 Konfigurationsschemat](azure-diagnostics-schema-1dot0.md)  

[Diagnostik 1.2 Konfigurationsschemat](azure-diagnostics-schema-1dot2.md)  

[Diagnostik 1.3 och senare Konfigurationsschemat](azure-diagnostics-schema-1dot3-and-later.md)  

## <a name="version-history"></a>Versionshistorik


### <a name="diagnostics-extension-19"></a>Diagnostik tillägget 1.9. 
Stöd har lagts till Docker.


### <a name="diagnostics-extension-181"></a>Diagnostik tillägget 1.8.1 
Ange en SAS-token i stället för en lagringskontonyckel i hello privata config. Om en SAS-token har angetts, ignoreras hello lagringskontonyckel.


```json
{
    "storageAccountName": "diagstorageaccount",
    "storageAccountEndPoint": "https://core.windows.net",
    "storageAccountSasToken": "{sas token}",
    "SecondaryStorageAccounts": {
        "StorageAccount": [
            {
                "name": "secondarydiagstorageaccount",
                "endpoint": "https://core.windows.net",
                "sasToken": "{sas token}"
            }
        ]
    }
}
```

```xml
<PrivateConfig>
    <StorageAccount name="diagstorageaccount" endpoint="https://core.windows.net" sasToken="{sas token}" />
    <SecondaryStorageAccounts>
        <StorageAccount name="secondarydiagstorageaccount" endpoint="https://core.windows.net" sasToken="{sas token}" />
    </SecondaryStorageAccounts>
</PrivateConfig>
```


### <a name="diagnostics-extension-18"></a>Diagnostik tillägget 1.8 
Tillagda lagringstyp tooPublicConfig. StorageType kan vara *tabell*, *Blob*, *TableAndBlob*. *Tabellen* hello standard.


```json
{
    "WadCfg": {
    },
    "StorageAccount": "diagstorageaccount",
    "StorageType": "TableAndBlob"
}
```

```xml
<PublicConfig>
    <WadCfg />
    <StorageAccount>diagstorageaccount</StorageAccount>
    <StorageType>TableAndBlob</StorageType>
</PublicConfig>
```


### <a name="diagnostics-extension-17"></a>Diagnostik tillägget 1.7 
Tillagda hello möjlighet tooroute tooEventHub.

### <a name="diagnostics-extension-15"></a>Diagnostik tillägget 1.5
Lägga till hello egenskaperna element och hello möjlighet toosend diagnostikdata för[Programinsikter](../application-insights/app-insights-cloudservices.md) vilket gör det enklare toodiagnose problem i din App samt hello system och infrastruktur-nivå.

### <a name="azure-sdk-26-and-diagnostics-extension-13"></a>Azure SDK 2.6 och diagnostik tillägget 1.3 
Hello följande ändringar har gjorts för Cloud Service-projekt i Visual Studio. (Ändringarna gäller även toolater versioner av Azure SDK.)

* hello lokala emulator stöder nu diagnostik. Det innebär att du kan samla in diagnostikdata och se till att programmet skapar hello rätt spår när du utvecklar och testar i Visual Studio. Hej anslutningssträngen `UseDevelopmentStorage=true` aktiverar diagnostik datainsamling när du kör cloud service-projekt i Visual Studio med hjälp av hello Azure storage-emulatorn. Alla diagnostikdata samlas in i hello (utveckling lagring) storage-konto.
* hello diagnostik konto lagringsanslutningssträng (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) lagras igen i hello tjänstekonfigurationsfilen (.cscfg). I Azure SDK 2.5 angavs hello diagnostiklagringskonto i hello diagnostics.wadcfgx-filen.

Det finns några viktiga skillnader mellan hur hello anslutningssträngen fungerade i Azure SDK 2.4 och tidigare och hur det fungerar i Azure SDK 2.6 eller senare.

* I Azure SDK 2.4 och tidigare användes hello anslutningssträngen som en körning av hello diagnostik plugin-programmet tooget hello information om lagringskonto för överföring av diagnostik loggar.
* Azure SDK 2.6 och senare, används hello diagnostik anslutningssträngen av Visual Studio tooconfigure hello diagnostik tillägg med information om hello lämpliga lagringskonto vid publicering. hello anslutningssträngen kan du definiera olika lagringskonton för olika konfigurationer som Visual Studio ska använda när du publicerar. Eftersom hello diagnostik plugin-programmet inte längre är tillgängliga (efter Azure SDK 2.5) kan inte hello .cscfg-filen ensamt aktivera hello diagnostik tillägg. Du har tooenable hello tillägget separat via verktyg som Visual Studio eller PowerShell.
* toosimplify hello konfigureringen av hello diagnostik tillägget med PowerShell, hello paketet utdata från Visual Studio innehåller också hello offentliga konfigurations-XML för hello diagnostik filnamnstillägget för varje roll. Visual Studio använder hello diagnostik toopopulate hello storage-kontoinformation om anslutningssträngar finns i hello offentliga konfiguration. hello offentliga config-filer skapas i hello tillägg mapp och följer hello mönster PaaSDiagnostics. <RoleName>. PubConfig.xml. PowerShell-baserade distributioner kan använda det här mönstret toomap varje configuration tooa roll.
* hello anslutningssträngen i hello .cscfg-filen används också av hello Azure portal tooaccess hello diagnostikdata så visas det i hello **övervakning** fliken hello anslutningssträngen är nödvändiga tooconfigure hello service tooshow utförlig övervakningsdata i hello-portalen.

#### <a name="migrating-projects-tooazure-sdk-26-and-later"></a>Migrera projekt tooAzure SDK 2.6 och senare
När du migrerar från Azure SDK 2,5 tooAzure SDK 2.6 eller senare, om du har en diagnostiklagringskonto som anges i hello .wadcfgx filen sedan förblir den det. tootake nytta av hello flexibilitet för att använda olika lagringsplatser konton för olika lagringskonfigurationer, har du toomanually lägga till hello anslutning sträng tooyour projekt. Om du migrerar ett projekt från Azure SDK 2.4 eller tidigare tooAzure SDK 2.6 hello du diagnostik anslutningssträngar bevaras. Observera dock hello ändringar i hur anslutningssträngar behandlas i Azure SDK 2.6 som anges i hello föregående avsnitt.

#### <a name="how-visual-studio-determines-hello-diagnostics-storage-account"></a>Hur hello diagnostiklagringskonto avgör i Visual Studio
* Om en anslutningssträng för diagnostik anges i .cscfg-filen för hello använder Visual Studio den tooconfigure hello diagnostik tillägget när du publicerar och vid generering av xml-filer för hello offentliga konfiguration under paketering.
* Om inga diagnostik anslutningssträngen har angetts i hello .cscfg-filen, sedan faller Visual Studio tillbaka toousing hello storage-konto som anges i hello .wadcfgx tooconfigure hello diagnostik filnamnstillägg när filen publicering och generera hello offentliga XML-konfigurationsfiler när paketera.
* hello diagnostik anslutningssträngen i hello .cscfg-filen har företräde framför hello storage-konto i hello .wadcfgx-filen. Om en anslutningssträng för diagnostik är anges i hello .cscfg-filen har Visual Studio använder som och ignorerar hello storage-konto i .wadcfgx.

#### <a name="what-does-hello-update-development-storage-connection-strings-checkbox-do"></a>Vad hello ”uppdatera development storage-anslutningssträngar...” kryssrutan för?
Hej kryssrutan för **uppdatera development storage-anslutningssträngar för diagnostik- och cachelagring med Microsoft Azure storage-kontoautentiseringsuppgifter när du publicerar tooMicrosoft Azure** ger dig ett enkelt sätt tooupdate alla utveckling lagringskontots anslutningssträngar med hello Azure storage-konto anges vid publicering.

Anta att du väljer den här kryssrutan och hello diagnostik anslutningssträngen anger `UseDevelopmentStorage=true`. När du publicerar hello projektet tooAzure uppdateras automatiskt hello diagnostik anslutningssträngen med hello storage-konto som du angav i guiden för hello publicera i Visual Studio. Men om en verklig lagringskonto har angetts som hello diagnostik anslutningssträng, används detta konto i stället.

### <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Diagnostik funktioner skillnaderna mellan Azure SDK 2.4 och tidigare och Azure SDK 2,5 och senare
Om du uppgraderar ditt projekt från Azure SDK 2.4 tooAzure SDK 2.5 eller senare, du bör ha i åtanke hello följande diagnostik funktioner skillnader.

* **Konfiguration av API: er är inaktuella** – programkonfiguration av diagnostik är tillgänglig i Azure SDK 2.4 eller tidigare versioner, men är föråldrad i Azure SDK 2.5 och senare. Om konfigurationen av diagnostik har definierats i koden, behöver du tooreconfigure dessa inställningar från grunden i hello migrerade projekt för diagnostik tookeep fungerar. konfigurationsfilen för hello diagnostik för Azure SDK 2.4 är diagnostics.wadcfg och diagnostics.wadcfgx för Azure SDK 2.5 och senare.
* **Diagnostik för molnprogram för tjänsten kan bara konfigureras på hello rollnivå, inte på instansnivå hello.**
* **Varje gång du distribuerar appen hello diagnostik konfiguration uppdateras** – om du ändrar konfigurationen diagnostik från Server Explorer och distribuera din app kan det medföra problem för paritet.
* **Azure SDK 2.5 och senare, krascher Dumpar konfigureras i hello konfigurationsfil diagnostik, inte i kod** – om du har krascher Dumpar som konfigurerats i koden har du toomanually överföring hello konfiguration från kod toohello konfigurationsfilen eftersom hello kraschdumpar överförs inte under hello migrering tooAzure SDK 2.6.

