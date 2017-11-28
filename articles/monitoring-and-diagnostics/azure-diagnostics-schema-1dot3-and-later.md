---
title: "aaaAzure diagnostik tilläggets 1.3 och senare konfigurationsschema | Microsoft Docs"
description: Schemaversionen 1.3 och senare Azure diagnostics levereras som en del av hello Microsoft Azure SDK 2,4 och senare.
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
ms.date: 05/15/2017
ms.author: robb
ms.openlocfilehash: bd15d3a79ea818fcb3235854717e58d5da36518e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-diagnostics-13-and-later-configuration-schema"></a><span data-ttu-id="ec857-103">Azure Diagnostics 1.3 och senare Konfigurationsschemat</span><span class="sxs-lookup"><span data-stu-id="ec857-103">Azure Diagnostics 1.3 and later configuration schema</span></span>
> [!NOTE]
> <span data-ttu-id="ec857-104">hello Azure Diagnostics tillägget är hello komponent används toocollect prestandaräknare och annan statistik från:</span><span class="sxs-lookup"><span data-stu-id="ec857-104">hello Azure Diagnostics extension is hello component used toocollect performance counters and other statistics from:</span></span>
> - <span data-ttu-id="ec857-105">Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="ec857-105">Azure Virtual Machines</span></span> 
> - <span data-ttu-id="ec857-106">Skalningsuppsättningar för Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="ec857-106">Virtual Machine Scale Sets</span></span>
> - <span data-ttu-id="ec857-107">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="ec857-107">Service Fabric</span></span> 
> - <span data-ttu-id="ec857-108">Molntjänster</span><span class="sxs-lookup"><span data-stu-id="ec857-108">Cloud Services</span></span> 
> - <span data-ttu-id="ec857-109">Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="ec857-109">Network Security Groups</span></span>
> 
> <span data-ttu-id="ec857-110">Den här sidan gäller endast om du använder någon av dessa tjänster.</span><span class="sxs-lookup"><span data-stu-id="ec857-110">This page is only relevant if you are using one of these services.</span></span>

<span data-ttu-id="ec857-111">Den här sidan gäller versioner 1.3 och senare (Azure SDK 2,4 och senare).</span><span class="sxs-lookup"><span data-stu-id="ec857-111">This page is valid for versions 1.3 and newer (Azure SDK 2.4 and newer).</span></span> <span data-ttu-id="ec857-112">Nyare konfigurationsavsnitt är kommenterade tooshow i vilken version de har lagts till.</span><span class="sxs-lookup"><span data-stu-id="ec857-112">Newer configuration sections are commented tooshow in what version they were added.</span></span>  

<span data-ttu-id="ec857-113">hello konfigurationsfilen som beskrivs här är används tooset diagnostiska konfigurationsinställningar när hello diagnostik övervakar startar.</span><span class="sxs-lookup"><span data-stu-id="ec857-113">hello configuration file described here is used tooset diagnostic configuration settings when hello diagnostics monitor starts.</span></span>  

<span data-ttu-id="ec857-114">hello-tillägget används tillsammans med andra produkter från Microsoft diagnostics som Azure-Monitor, Application Insights och logganalys.</span><span class="sxs-lookup"><span data-stu-id="ec857-114">hello extension is used in conjunction with other Microsoft diagnostics products like Azure Monitor, Application Insights, and Log Analytics.</span></span>



<span data-ttu-id="ec857-115">Hämta hello offentliga konfiguration filen schemadefinition genom att köra följande PowerShell-kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ec857-115">Download hello public configuration file schema definition by executing hello following PowerShell command:</span></span>  

```powershell  
(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File –Encoding utf8 -FilePath 'C:\temp\WadConfig.xsd'  
```  

<span data-ttu-id="ec857-116">Mer information om hur du använder Azure-diagnostik finns [Azure Diagnostics tillägget](azure-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="ec857-116">For more information about using Azure Diagnostics, see [Azure Diagnostics Extension](azure-diagnostics.md).</span></span>  

## <a name="example-of-hello-diagnostics-configuration-file"></a><span data-ttu-id="ec857-117">Exempel på hello diagnostik-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="ec857-117">Example of hello diagnostics configuration file</span></span>  
 <span data-ttu-id="ec857-118">hello som följande exempel visar en typisk diagnostik konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="ec857-118">hello following example shows a typical diagnostics configuration file:</span></span>  

```xml  
<?xml version="1.0" encoding="utf-8"?>  
<DiagnosticsConfiguration  xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">   
  <PublicConfig>  
    <WadCfg>  
      <DiagnosticMonitorConfiguration overallQuotaInMB="10000">  

        <PerformanceCounters scheduledTransferPeriod="PT1M">  
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />  
        </PerformanceCounters>  

        <Directories scheduledTransferPeriod="PT5M">  
          <IISLogs containerName="iislogs" />  
          <FailedRequestLogs containerName="iisfailed" />  

          <DataSources>  
            <DirectoryConfiguration containerName="mynewprocess">  
              <Absolute path="C:\MyNewProcess" expandEnvironment="false" />  
            </DirectoryConfiguration>  
            <DirectoryConfiguration containerName="badapp">  
              <Absolute path="%SYSTEMDRIVE%\BadApp" expandEnvironment="true" />  
            </DirectoryConfiguration>  
            <DirectoryConfiguration containerName="goodapp">  
              <LocalResource name="Skippy" relativePath="..\PeanutButter"/>  
            </DirectoryConfiguration>  
          </DataSources>  

        </Directories>  

        <EtwProviders>  
          <EtwEventSourceProviderConfiguration   
                       provider="MyProviderClass"   
                       scheduledTransferPeriod="PT5M">  
            <Event id="0"/>  
            <Event id="1" eventDestination="errorTable"/>  
            <DefaultEvents />  
          </EtwEventSourceProviderConfiguration>  
          <EtwManifestProviderConfiguration provider="5974b00b-84c2-44bc-9e58-3a2451b4e3ad" scheduledTransferLogLevelFilter="Information" scheduledTransferPeriod="PT2M">  
            <Event id="0"/>  
            <DefaultEvents eventDestination="defaultTable"/>  
          </EtwManifestProviderConfiguration>  
        </EtwProviders>  

        <WindowsEventLog scheduledTransferPeriod="PT5M">  
          <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>  
          <DataSource name="System!*[System[Provider[@Name='NTFS'] and (EventID=55)]]" />  
          <DataSource name="System!*[System[Provider[@Name='disk'] and (EventID=7 or EventID=52 or EventID=55)]]" />  
        </WindowsEventLog>  

        <Logs  bufferQuotaInMB="1024"   
             scheduledTransferPeriod="PT1M"   
             scheduledTransferLogLevelFilter="Verbose"   
             sinks="ApplicationInsights.AppLogs"/>  <!-- sinks attribute added in 1.5 -->  

        <CrashDumps containerName="wad-crashdumps" directoryQuotaPercentage="30" dumpType="Mini">  
          <CrashDumpConfiguration processName="mynewprocess.exe" />  
          <CrashDumpConfiguration processName="badapp.exe"/>  
        </CrashDumps>  

        <DockerSources> <!-- Added in 1.9 --> 
          <Stats enabled="true" sampleRate="PT1M" scheduledTransferPeriod="PT1M" />
        </DockerSources>

      </DiagnosticMonitorConfiguration>  

      <SinksConfig>   <!-- Added in 1.5 -->  
        <Sink name="ApplicationInsights">   
          <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>   
          <Channels>   
            <Channel logLevel="Error" name="Errors"  />   
            <Channel logLevel="Verbose" name="AppLogs"  />   
          </Channels>   
        </Sink>   
        <Sink name="EventHub"> <!-- Added in 1.7 -->
          <EventHub Url="https://myeventhub-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" usePublisherId="false" />
        </Sink>
        <Sink name="secondaryEventHub"> <!-- Added in 1.7 -->
          <EventHub Url="https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub" SharedAccessKeyName="SendRule" usePublisherId="false" />
        </Sink>
        <Sink name="secondaryStorageAccount"> <!-- Added in 1.7 -->
          <StorageAccount name="secondarydiagstorageaccount" endpoint="https://core.windows.net" />
        </Sink>
   </SinksConfig>

  </WadCfg>  

  <StorageAccount>diagstorageaccount</StorageAccount>
  <StorageType>TableAndBlob</StorageType> <!-- Added in 1.8 -->  
  </PublicConfig>  

  <PrivateConfig>  <!-- Added in 1.3 -->  
    <StorageAccount name="" key="" endpoint="" sasToken="{sas token}"  />  <!-- sasToken in Private config added in 1.8.1 -->  
    <EventHub Url="https://myeventhub-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="{base64 encoded key}" />
   
    <SecondaryStorageAccounts>
       <StorageAccount name="secondarydiagstorageaccount" key="{base64 encoded key}" endpoint="https://core.windows.net" sasToken="{sas token}" />
    </SecondaryStorageAccounts>
   
    <SecondaryEventHubs>
       <EventHub Url="https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="{base64 encoded key}" />
    </SecondaryEventHubs>

  </PrivateConfig>  
  <IsEnabled>true</IsEnabled>  
</DiagnosticsConfiguration>  

```  

<span data-ttu-id="ec857-119">JSON motsvarande hello tidigare XML-konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="ec857-119">JSON equivalent of hello previous XML configuration file.</span></span> 

<span data-ttu-id="ec857-120">Hej PublicConfig och PrivateConfig avgränsas eftersom json användning oftast skickas de som olika variabler.</span><span class="sxs-lookup"><span data-stu-id="ec857-120">hello PublicConfig and PrivateConfig are separated because in most json usage cases, they are passed as different variables.</span></span> <span data-ttu-id="ec857-121">Dessa fall innehåller Resource Manager-mallar, Virtual Machine Scale set PowerShell och Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ec857-121">These cases include Resource Manager templates, Virtual Machine Scale set PowerShell, and Visual Studio.</span></span> 

```json
"PublicConfig" {
    "WadCfg": {
        "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": 10000,
            "DiagnosticInfrastructureLogs": {
                "scheduledTransferLogLevelFilter": "Error"
            },
            "PerformanceCounters": {
                "scheduledTransferPeriod": "PT1M",
                "PerformanceCounterConfiguration": [
                    {
                        "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                        "sampleRate": "PT1M",
                        "unit": "percent"
                    }
                ]
            },
            "Directories": {
                "scheduledTransferPeriod": "PT5M",
                "IISLogs": {
                    "containerName": "iislogs"
                },
                "FailedRequestLogs": {
                    "containerName": "iisfailed"
                },
                "DataSources": [
                    {
                        "containerName": "mynewprocess",
                        "Absolute": {
                            "path": "C:\\MyNewProcess",
                            "expandEnvironment": false
                        }
                    },
                    {
                        "containerName": "badapp",
                        "Absolute": {
                            "path": "%SYSTEMDRIVE%\\BadApp",
                            "expandEnvironment": true
                        }
                    },
                    {
                        "containerName": "goodapp",
                        "LocalResource": {
                            "relativePath": "..\\PeanutButter",
                            "name": "Skippy"
                        }
                    }
                ]
            },
            "EtwProviders": {
                "sinks": "",
                "EtwEventSourceProviderConfiguration": [
                    {
                        "scheduledTransferPeriod": "PT5M",
                        "provider": "MyProviderClass",
                        "Event": [
                            {
                                "id": 0
                            },
                            {
                                "id": 1,
                                "eventDestination": "errorTable"
                            }
                        ],
                        "DefaultEvents": {
                        }
                    }
                ],
                "EtwManifestProviderConfiguration": [
                    {
                        "scheduledTransferPeriod": "PT2M",
                        "scheduledTransferLogLevelFilter": "Information",
                        "provider": "5974b00b-84c2-44bc-9e58-3a2451b4e3ad",
                        "Event": [
                            {
                                "id": 0
                            }
                        ],
                        "DefaultEvents": {
                        }
                    }
                ]
            },
            "WindowsEventLog": {
                "scheduledTransferPeriod": "PT5M",
                "DataSource": [
                    {
                        "name": "System!*[System[Provider[@Name='Microsoft Antimalware']]]"
                    },
                    {
                        "name": "System!*[System[Provider[@Name='NTFS'] and (EventID=55)]]"
                    },
                    {
                        "name": "System!*[System[Provider[@Name='disk'] and (EventID=7 or EventID=52 or EventID=55)]]"
                    }
                ]
            },
            "Logs": {
                "scheduledTransferPeriod": "PT1M",
                "scheduledTransferLogLevelFilter": "Verbose",
                "sinks": "ApplicationInsights.AppLogs"
            },
            "CrashDumps": {
                "directoryQuotaPercentage": 30,
                "dumpType": "Mini",
                "containerName": "wad-crashdumps",
                "CrashDumpConfiguration": [
                    {
                        "processName": "mynewprocess.exe"
                    },
                    {
                        "processName": "badapp.exe"
                    }
                ]
            }
        },
        "SinksConfig": {
            "Sink": [
                {
                    "name": "ApplicationInsights",
                    "ApplicationInsights": "{Insert InstrumentationKey}",
                    "Channels": {
                        "Channel": [
                            {
                                "logLevel": "Error",
                                "name": "Errors"
                            },
                            {
                                "logLevel": "Verbose",
                                "name": "AppLogs"
                            }
                        ]
                    }
                },
                {
                    "name": "EventHub",
                    "EventHub": {
                        "Url": "https://myeventhub-ns.servicebus.windows.net/diageventhub",
                        "SharedAccessKeyName": "SendRule",
                        "usePublisherId": false
                    }
                },
                {
                    "name": "secondaryEventHub",
                    "EventHub": {
                        "Url": "https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub",
                        "SharedAccessKeyName": "SendRule",
                        "usePublisherId": false
                    }
                },
                {
                    "name": "secondaryStorageAccount",
                    "StorageAccount": {
                        "name": "secondarydiagstorageaccount",
                        "endpoint": "https://core.windows.net"
                    }
                }
            ]
        }
    },
    "StorageAccount": "diagstorageaccount",
    "StorageType": "TableAndBlob"
}
```

```json
"PrivateConfig" {
    "storageAccountName": "diagstorageaccount",
    "storageAccountKey": "{base64 encoded key}",
    "storageAccountEndPoint": "https://core.windows.net",
    "storageAccountSasToken": "{sas token}",
    "EventHub": {
        "Url": "https://myeventhub-ns.servicebus.windows.net/diageventhub",
        "SharedAccessKeyName": "SendRule",
        "SharedAccessKey": "{base64 encoded key}"
    },
    "SecondaryStorageAccounts": {
        "StorageAccount": [
            {
                "name": "secondarydiagstorageaccount",
                "key": "{base64 encoded key}",
                "endpoint": "https://core.windows.net",
                "sasToken": "{sas token}"
            }
        ]
    },
    "SecondaryEventHubs": {
        "EventHub": [
            {
                "Url": "https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub",
                "SharedAccessKeyName": "SendRule",
                "SharedAccessKey": "{base64 encoded key}"
            }
        ]
    }
}

```

## <a name="reading-this-page"></a><span data-ttu-id="ec857-122">Läsa den här sidan</span><span class="sxs-lookup"><span data-stu-id="ec857-122">Reading this page</span></span>  
 <span data-ttu-id="ec857-123">hello är taggar efter ungefär i ordning som visas i föregående exempel hello.</span><span class="sxs-lookup"><span data-stu-id="ec857-123">hello tags following are roughly in order shown in hello preceding example.</span></span>  <span data-ttu-id="ec857-124">Om du inte ser en fullständig beskrivning där du förväntar dig, kan du söka hello sidan för hello element eller attribut.</span><span class="sxs-lookup"><span data-stu-id="ec857-124">If you don't see a full description where you expect it, search hello page for hello element or attribute.</span></span>  

## <a name="common-attribute-types"></a><span data-ttu-id="ec857-125">Vanliga attribut</span><span class="sxs-lookup"><span data-stu-id="ec857-125">Common Attribute Types</span></span>  
 <span data-ttu-id="ec857-126">**scheduledTransferPeriod** attributet visas i flera element.</span><span class="sxs-lookup"><span data-stu-id="ec857-126">**scheduledTransferPeriod** attribute appears in several elements.</span></span> <span data-ttu-id="ec857-127">Det är hello intervallet mellan schemalagda överföringar toostorage avrundat toohello närmast minuter.</span><span class="sxs-lookup"><span data-stu-id="ec857-127">It is hello interval between scheduled transfers toostorage rounded up toohello nearest minute.</span></span> <span data-ttu-id="ec857-128">hello-värdet är en [XML ”varaktighet datatyp”.](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span><span class="sxs-lookup"><span data-stu-id="ec857-128">hello value is an [XML “Duration Data Type.”](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span></span>


## <a name="diagnosticsconfiguration-element"></a><span data-ttu-id="ec857-129">DiagnosticsConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="ec857-129">DiagnosticsConfiguration Element</span></span>  
 <span data-ttu-id="ec857-130">*: Trädrot - DiagnosticsConfiguration*</span><span class="sxs-lookup"><span data-stu-id="ec857-130">*Tree: Root - DiagnosticsConfiguration*</span></span>

<span data-ttu-id="ec857-131">Lägga till i version 1.3.</span><span class="sxs-lookup"><span data-stu-id="ec857-131">Added in version 1.3.</span></span>  

<span data-ttu-id="ec857-132">hello översta elementet i hello diagnostik konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="ec857-132">hello top-level element of hello diagnostics configuration file.</span></span>  

<span data-ttu-id="ec857-133">**Attributet** xmlns - hello XML-namnområdet för hello diagnostik konfigurationsfilen är:</span><span class="sxs-lookup"><span data-stu-id="ec857-133">**Attribute**  xmlns - hello XML namespace for hello diagnostics configuration file is:</span></span>  
<span data-ttu-id="ec857-134">http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration</span><span class="sxs-lookup"><span data-stu-id="ec857-134">http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration</span></span>  


|<span data-ttu-id="ec857-135">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="ec857-135">Child Elements</span></span>|<span data-ttu-id="ec857-136">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec857-136">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="ec857-137">**PublicConfig**</span><span class="sxs-lookup"><span data-stu-id="ec857-137">**PublicConfig**</span></span>|<span data-ttu-id="ec857-138">Krävs.</span><span class="sxs-lookup"><span data-stu-id="ec857-138">Required.</span></span> <span data-ttu-id="ec857-139">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="ec857-139">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="ec857-140">**PrivateConfig**</span><span class="sxs-lookup"><span data-stu-id="ec857-140">**PrivateConfig**</span></span>|<span data-ttu-id="ec857-141">Valfri.</span><span class="sxs-lookup"><span data-stu-id="ec857-141">Optional.</span></span> <span data-ttu-id="ec857-142">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="ec857-142">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="ec857-143">**IsEnabled**</span><span class="sxs-lookup"><span data-stu-id="ec857-143">**IsEnabled**</span></span>|<span data-ttu-id="ec857-144">Booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="ec857-144">Boolean.</span></span> <span data-ttu-id="ec857-145">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="ec857-145">See description elsewhere on this page.</span></span>|  

## <a name="publicconfig-element"></a><span data-ttu-id="ec857-146">PublicConfig Element</span><span class="sxs-lookup"><span data-stu-id="ec857-146">PublicConfig Element</span></span>  
 <span data-ttu-id="ec857-147">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig*</span><span class="sxs-lookup"><span data-stu-id="ec857-147">*Tree: Root - DiagnosticsConfiguration - PublicConfig*</span></span>

 <span data-ttu-id="ec857-148">Beskriver hello offentliga diagnostik konfiguration.</span><span class="sxs-lookup"><span data-stu-id="ec857-148">Describes hello public diagnostics configuration.</span></span>  

|<span data-ttu-id="ec857-149">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="ec857-149">Child Elements</span></span>|<span data-ttu-id="ec857-150">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec857-150">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="ec857-151">**WadCfg**</span><span class="sxs-lookup"><span data-stu-id="ec857-151">**WadCfg**</span></span>|<span data-ttu-id="ec857-152">Krävs.</span><span class="sxs-lookup"><span data-stu-id="ec857-152">Required.</span></span> <span data-ttu-id="ec857-153">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="ec857-153">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="ec857-154">**StorageAccount**</span><span class="sxs-lookup"><span data-stu-id="ec857-154">**StorageAccount**</span></span>|<span data-ttu-id="ec857-155">hello namnet på hello Azure Storage-konto toostore hello data i.</span><span class="sxs-lookup"><span data-stu-id="ec857-155">hello name of hello Azure Storage account toostore hello data in.</span></span> <span data-ttu-id="ec857-156">Kan också anges som en parameter när du kör cmdlet hello Set-AzureServiceDiagnosticsExtension.</span><span class="sxs-lookup"><span data-stu-id="ec857-156">May also be specified as a parameter when executing hello Set-AzureServiceDiagnosticsExtension cmdlet.</span></span>|  
|<span data-ttu-id="ec857-157">**StorageType**</span><span class="sxs-lookup"><span data-stu-id="ec857-157">**StorageType**</span></span>|<span data-ttu-id="ec857-158">Kan vara *tabell*, *Blob*, eller *TableAndBlob*.</span><span class="sxs-lookup"><span data-stu-id="ec857-158">Can be *Table*, *Blob*, or *TableAndBlob*.</span></span> <span data-ttu-id="ec857-159">Tabellen är standard.</span><span class="sxs-lookup"><span data-stu-id="ec857-159">Table is default.</span></span> <span data-ttu-id="ec857-160">När du har valt TableAndBlob skrivs diagnostikdata två gånger – en gång tooeach typen.</span><span class="sxs-lookup"><span data-stu-id="ec857-160">When TableAndBlob is chosen, diagnostic data is written twice -- once tooeach type.</span></span>|  
|<span data-ttu-id="ec857-161">**LocalResourceDirectory**</span><span class="sxs-lookup"><span data-stu-id="ec857-161">**LocalResourceDirectory**</span></span>|<span data-ttu-id="ec857-162">hello katalog på hello virtuell dator där hello Monitoring Agent lagrar händelsedata.</span><span class="sxs-lookup"><span data-stu-id="ec857-162">hello directory on hello virtual machine where hello Monitoring Agent stores event data.</span></span> <span data-ttu-id="ec857-163">Om du inte har angetts hello standardkatalogen:</span><span class="sxs-lookup"><span data-stu-id="ec857-163">If not, set, hello default directory is used:</span></span><br /><br /> <span data-ttu-id="ec857-164">För en arbetare/webbroll:`C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`</span><span class="sxs-lookup"><span data-stu-id="ec857-164">For a Worker/web role: `C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`</span></span><br /><br /> <span data-ttu-id="ec857-165">För en virtuell dator:`C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`</span><span class="sxs-lookup"><span data-stu-id="ec857-165">For a Virtual Machine: `C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`</span></span><br /><br /> <span data-ttu-id="ec857-166">Obligatoriska attribut är:</span><span class="sxs-lookup"><span data-stu-id="ec857-166">Required attributes are:</span></span><br /><br /> <span data-ttu-id="ec857-167">- **sökvägen** - hello på hello system toobe används av Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="ec857-167">- **path** - hello directory on hello system toobe used by Azure Diagnostics.</span></span><br /><br /> <span data-ttu-id="ec857-168">- **expandEnvironment** -styr om miljövariabler expanderas i hello sökvägsnamn.</span><span class="sxs-lookup"><span data-stu-id="ec857-168">- **expandEnvironment** - Controls whether environment variables are expanded in hello path name.</span></span>|  

## <a name="wadcfg-element"></a><span data-ttu-id="ec857-169">WadCFG Element</span><span class="sxs-lookup"><span data-stu-id="ec857-169">WadCFG Element</span></span>  
 <span data-ttu-id="ec857-170">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG*</span><span class="sxs-lookup"><span data-stu-id="ec857-170">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG*</span></span>
 
 <span data-ttu-id="ec857-171">Identifierar och konfigurerar hello telemetri data toobe samlas in.</span><span class="sxs-lookup"><span data-stu-id="ec857-171">Identifies and configures hello telemetry data toobe collected.</span></span>  


## <a name="diagnosticmonitorconfiguration-element"></a><span data-ttu-id="ec857-172">DiagnosticMonitorConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="ec857-172">DiagnosticMonitorConfiguration Element</span></span> 
 <span data-ttu-id="ec857-173">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration*</span><span class="sxs-lookup"><span data-stu-id="ec857-173">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration*</span></span>

 <span data-ttu-id="ec857-174">Krävs</span><span class="sxs-lookup"><span data-stu-id="ec857-174">Required</span></span> 

|<span data-ttu-id="ec857-175">Attribut</span><span class="sxs-lookup"><span data-stu-id="ec857-175">Attributes</span></span>|<span data-ttu-id="ec857-176">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec857-176">Description</span></span>|  
|----------------|-----------------|  
| <span data-ttu-id="ec857-177">**overallQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="ec857-177">**overallQuotaInMB**</span></span> | <span data-ttu-id="ec857-178">hello maximal mängd lokalt diskutrymme som kan användas av hello olika typer av diagnostiska data som samlas in av Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="ec857-178">hello maximum amount of local disk space that may be consumed by hello various types of diagnostic data collected by Azure Diagnostics.</span></span> <span data-ttu-id="ec857-179">hello standardinställningen är 5 120 MB.</span><span class="sxs-lookup"><span data-stu-id="ec857-179">hello default setting is 5120 MB.</span></span><br />
|<span data-ttu-id="ec857-180">**useProxyServer**</span><span class="sxs-lookup"><span data-stu-id="ec857-180">**useProxyServer**</span></span> | <span data-ttu-id="ec857-181">Konfigurera Azure-diagnostik toouse hello proxyserverinställningar som angetts i Internet Explorer-inställningar.</span><span class="sxs-lookup"><span data-stu-id="ec857-181">Configure Azure Diagnostics toouse hello proxy server settings as set in IE settings.</span></span>|  

<br /> <br />

|<span data-ttu-id="ec857-182">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="ec857-182">Child Elements</span></span>|<span data-ttu-id="ec857-183">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec857-183">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="ec857-184">**CrashDumps**</span><span class="sxs-lookup"><span data-stu-id="ec857-184">**CrashDumps**</span></span>|<span data-ttu-id="ec857-185">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="ec857-185">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="ec857-186">**DiagnosticInfrastructureLogs**</span><span class="sxs-lookup"><span data-stu-id="ec857-186">**DiagnosticInfrastructureLogs**</span></span>|<span data-ttu-id="ec857-187">Aktivera insamling av loggar som genereras av Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="ec857-187">Enable collection of logs generated by Azure Diagnostics.</span></span> <span data-ttu-id="ec857-188">hello diagnostiska infrastruktur loggarna är användbar vid felsökning av hello diagnostik själva systemet.</span><span class="sxs-lookup"><span data-stu-id="ec857-188">hello diagnostic infrastructure logs are useful for troubleshooting hello diagnostics system itself.</span></span> <span data-ttu-id="ec857-189">Valfria attribut är:</span><span class="sxs-lookup"><span data-stu-id="ec857-189">Optional attributes are:</span></span><br /><br /> <span data-ttu-id="ec857-190">- **scheduledTransferLogLevelFilter** -konfigurerar hello lägsta allvarlighetsgrad hello loggar samlas in.</span><span class="sxs-lookup"><span data-stu-id="ec857-190">- **scheduledTransferLogLevelFilter** - Configures hello minimum severity level of hello logs collected.</span></span><br /><br /> <span data-ttu-id="ec857-191">- **scheduledTransferPeriod** -hello intervallet mellan schemalagda överföringar toostorage avrundat toohello närmast minuter.</span><span class="sxs-lookup"><span data-stu-id="ec857-191">- **scheduledTransferPeriod** - hello interval between scheduled transfers toostorage rounded up toohello nearest minute.</span></span> <span data-ttu-id="ec857-192">hello-värdet är en [XML ”varaktighet datatyp”.](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span><span class="sxs-lookup"><span data-stu-id="ec857-192">hello value is an [XML “Duration Data Type.”](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span></span> |  
|<span data-ttu-id="ec857-193">**Kataloger**</span><span class="sxs-lookup"><span data-stu-id="ec857-193">**Directories**</span></span>|<span data-ttu-id="ec857-194">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="ec857-194">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="ec857-195">**EtwProviders**</span><span class="sxs-lookup"><span data-stu-id="ec857-195">**EtwProviders**</span></span>|<span data-ttu-id="ec857-196">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="ec857-196">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="ec857-197">**Mått**</span><span class="sxs-lookup"><span data-stu-id="ec857-197">**Metrics**</span></span>|<span data-ttu-id="ec857-198">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="ec857-198">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="ec857-199">**PerformanceCounters**</span><span class="sxs-lookup"><span data-stu-id="ec857-199">**PerformanceCounters**</span></span>|<span data-ttu-id="ec857-200">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="ec857-200">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="ec857-201">**WindowsEventLog**</span><span class="sxs-lookup"><span data-stu-id="ec857-201">**WindowsEventLog**</span></span>|<span data-ttu-id="ec857-202">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="ec857-202">See description elsewhere on this page.</span></span>| 
|<span data-ttu-id="ec857-203">**DockerSources**</span><span class="sxs-lookup"><span data-stu-id="ec857-203">**DockerSources**</span></span>|<span data-ttu-id="ec857-204">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="ec857-204">See description elsewhere on this page.</span></span> | 



## <a name="crashdumps-element"></a><span data-ttu-id="ec857-205">CrashDumps Element</span><span class="sxs-lookup"><span data-stu-id="ec857-205">CrashDumps Element</span></span>  
 <span data-ttu-id="ec857-206">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - CrashDumps*</span><span class="sxs-lookup"><span data-stu-id="ec857-206">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - CrashDumps*</span></span>
 
 <span data-ttu-id="ec857-207">Aktivera hello insamling av krascher Dumpar.</span><span class="sxs-lookup"><span data-stu-id="ec857-207">Enable hello collection of crash dumps.</span></span>  

|<span data-ttu-id="ec857-208">Attribut</span><span class="sxs-lookup"><span data-stu-id="ec857-208">Attributes</span></span>|<span data-ttu-id="ec857-209">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec857-209">Description</span></span>|  
|----------------|-----------------|  
|<span data-ttu-id="ec857-210">**containerName**</span><span class="sxs-lookup"><span data-stu-id="ec857-210">**containerName**</span></span>|<span data-ttu-id="ec857-211">Valfri.</span><span class="sxs-lookup"><span data-stu-id="ec857-211">Optional.</span></span> <span data-ttu-id="ec857-212">hello namnet på hello blob-behållaren i ditt Azure Storage-konto toobe används toostore krashdumpar.</span><span class="sxs-lookup"><span data-stu-id="ec857-212">hello name of hello blob container in your Azure Storage account toobe used toostore crash dumps.</span></span>|  
|<span data-ttu-id="ec857-213">**crashDumpType**</span><span class="sxs-lookup"><span data-stu-id="ec857-213">**crashDumpType**</span></span>|<span data-ttu-id="ec857-214">Valfri.</span><span class="sxs-lookup"><span data-stu-id="ec857-214">Optional.</span></span>  <span data-ttu-id="ec857-215">Konfigurerar Azure Diagnostics toocollect mini eller fullständig kraschar Dumpar.</span><span class="sxs-lookup"><span data-stu-id="ec857-215">Configures Azure Diagnostics toocollect mini or full crash dumps.</span></span>|  
|<span data-ttu-id="ec857-216">**directoryQuotaPercentage**</span><span class="sxs-lookup"><span data-stu-id="ec857-216">**directoryQuotaPercentage**</span></span>|<span data-ttu-id="ec857-217">Valfri.</span><span class="sxs-lookup"><span data-stu-id="ec857-217">Optional.</span></span>  <span data-ttu-id="ec857-218">Konfigurerar hello procentandelen av **overallQuotaInMB** toobe reserverad för kraschdumpar på hello VM.</span><span class="sxs-lookup"><span data-stu-id="ec857-218">Configures hello percentage of **overallQuotaInMB** toobe reserved for crash dumps on hello VM.</span></span>|  

|<span data-ttu-id="ec857-219">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="ec857-219">Child Elements</span></span>|<span data-ttu-id="ec857-220">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec857-220">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="ec857-221">**CrashDumpConfiguration**</span><span class="sxs-lookup"><span data-stu-id="ec857-221">**CrashDumpConfiguration**</span></span>|<span data-ttu-id="ec857-222">Krävs.</span><span class="sxs-lookup"><span data-stu-id="ec857-222">Required.</span></span> <span data-ttu-id="ec857-223">Definierar konfigurationsvärden för varje process.</span><span class="sxs-lookup"><span data-stu-id="ec857-223">Defines configuration values for each process.</span></span><br /><br /> <span data-ttu-id="ec857-224">Det krävs också hello följande attribut:</span><span class="sxs-lookup"><span data-stu-id="ec857-224">hello following attribute is also required:</span></span><br /><br /> <span data-ttu-id="ec857-225">**Processnamn** - hello namn på hello process som du vill använda Azure-diagnostik toocollect krasch-dump för.</span><span class="sxs-lookup"><span data-stu-id="ec857-225">**processName** - hello name of hello process you want Azure Diagnostics toocollect a crash dump for.</span></span>|  

## <a name="directories-element"></a><span data-ttu-id="ec857-226">Kataloger Element</span><span class="sxs-lookup"><span data-stu-id="ec857-226">Directories Element</span></span> 
 <span data-ttu-id="ec857-227">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - kataloger*</span><span class="sxs-lookup"><span data-stu-id="ec857-227">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration -  Directories*</span></span>

 <span data-ttu-id="ec857-228">Aktiverar hello samling hello innehållet i en katalog, IIS kunde inte begäran åtkomstloggar och/eller IIS-loggar.</span><span class="sxs-lookup"><span data-stu-id="ec857-228">Enables hello collection of hello contents of a directory, IIS failed access request logs and/or IIS logs.</span></span>  

 <span data-ttu-id="ec857-229">Valfria **scheduledTransferPeriod** attribut.</span><span class="sxs-lookup"><span data-stu-id="ec857-229">Optional **scheduledTransferPeriod** attribute.</span></span> <span data-ttu-id="ec857-230">Se tidigare förklaring.</span><span class="sxs-lookup"><span data-stu-id="ec857-230">See explanation earlier.</span></span>  

|<span data-ttu-id="ec857-231">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="ec857-231">Child Elements</span></span>|<span data-ttu-id="ec857-232">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec857-232">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="ec857-233">**IISLogs**</span><span class="sxs-lookup"><span data-stu-id="ec857-233">**IISLogs**</span></span>|<span data-ttu-id="ec857-234">Inkludera det här elementet i hello konfiguration kan hello samling av IIS-loggar:</span><span class="sxs-lookup"><span data-stu-id="ec857-234">Including this element in hello configuration enables hello collection of IIS logs:</span></span><br /><br /> <span data-ttu-id="ec857-235">**containerName** -hello namnet hello blob-behållaren i ditt Azure Storage-konto toobe används toostore hello IIS-loggar.</span><span class="sxs-lookup"><span data-stu-id="ec857-235">**containerName** - hello name of hello blob container in your Azure Storage account toobe used toostore hello IIS logs.</span></span>|   
|<span data-ttu-id="ec857-236">**FailedRequestLogs**</span><span class="sxs-lookup"><span data-stu-id="ec857-236">**FailedRequestLogs**</span></span>|<span data-ttu-id="ec857-237">Inklusive det här elementet i hello konfiguration gör det möjligt för insamling av loggar för misslyckade begäranden tooan IIS-webbplatsen eller programmet.</span><span class="sxs-lookup"><span data-stu-id="ec857-237">Including this element in hello configuration enables collection of logs about failed requests tooan IIS site or application.</span></span> <span data-ttu-id="ec857-238">Du måste även aktivera spårningsalternativ under **system. Webbserver** i **Web.config**.</span><span class="sxs-lookup"><span data-stu-id="ec857-238">You must also enable tracing options under **system.WebServer** in **Web.config**.</span></span>|  
|<span data-ttu-id="ec857-239">**Datakällor**</span><span class="sxs-lookup"><span data-stu-id="ec857-239">**DataSources**</span></span>|<span data-ttu-id="ec857-240">En lista över kataloger toomonitor.</span><span class="sxs-lookup"><span data-stu-id="ec857-240">A list of directories toomonitor.</span></span>| 




## <a name="datasources-element"></a><span data-ttu-id="ec857-241">Datakällor Element</span><span class="sxs-lookup"><span data-stu-id="ec857-241">DataSources Element</span></span>  
 <span data-ttu-id="ec857-242">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - kataloger - datakällor*</span><span class="sxs-lookup"><span data-stu-id="ec857-242">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Directories - DataSources*</span></span>

 <span data-ttu-id="ec857-243">En lista över kataloger toomonitor.</span><span class="sxs-lookup"><span data-stu-id="ec857-243">A list of directories toomonitor.</span></span>  

|<span data-ttu-id="ec857-244">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="ec857-244">Child Elements</span></span>|<span data-ttu-id="ec857-245">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec857-245">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="ec857-246">**DirectoryConfiguration**</span><span class="sxs-lookup"><span data-stu-id="ec857-246">**DirectoryConfiguration**</span></span>|<span data-ttu-id="ec857-247">Krävs.</span><span class="sxs-lookup"><span data-stu-id="ec857-247">Required.</span></span> <span data-ttu-id="ec857-248">Obligatoriskt attribut:</span><span class="sxs-lookup"><span data-stu-id="ec857-248">Required attribute:</span></span><br /><br /> <span data-ttu-id="ec857-249">**containerName** - hello namn på hello blob-behållaren i ditt Azure Storage-konto som toobe används toostore hello loggfiler.</span><span class="sxs-lookup"><span data-stu-id="ec857-249">**containerName** - hello name of hello blob container in your Azure Storage account that toobe used toostore hello log files.</span></span>|  





## <a name="directoryconfiguration-element"></a><span data-ttu-id="ec857-250">DirectoryConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="ec857-250">DirectoryConfiguration Element</span></span>  
 <span data-ttu-id="ec857-251">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - kataloger - datakällor - DirectoryConfiguration*</span><span class="sxs-lookup"><span data-stu-id="ec857-251">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Directories - DataSources - DirectoryConfiguration*</span></span>

 <span data-ttu-id="ec857-252">Kan innehålla antingen hello **absolut** eller **LocalResource** element, men inte båda.</span><span class="sxs-lookup"><span data-stu-id="ec857-252">May include either hello **Absolute** or **LocalResource** element but not both.</span></span>  

|<span data-ttu-id="ec857-253">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="ec857-253">Child Elements</span></span>|<span data-ttu-id="ec857-254">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec857-254">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="ec857-255">**Absolut**</span><span class="sxs-lookup"><span data-stu-id="ec857-255">**Absolute**</span></span>|<span data-ttu-id="ec857-256">hello absolut sökväg toohello directory toomonitor.</span><span class="sxs-lookup"><span data-stu-id="ec857-256">hello absolute path toohello directory toomonitor.</span></span> <span data-ttu-id="ec857-257">Det krävs hello följande attribut:</span><span class="sxs-lookup"><span data-stu-id="ec857-257">hello following attributes are required:</span></span><br /><br /> <span data-ttu-id="ec857-258">- **Sökvägen** -hello absolut sökväg toohello directory toomonitor.</span><span class="sxs-lookup"><span data-stu-id="ec857-258">- **Path** - hello absolute path toohello directory toomonitor.</span></span><br /><br /> <span data-ttu-id="ec857-259">- **expandEnvironment** -anger om miljövariabler i sökvägen expanderas.</span><span class="sxs-lookup"><span data-stu-id="ec857-259">- **expandEnvironment** - Configures whether environment variables in Path are expanded.</span></span>|  
|<span data-ttu-id="ec857-260">**LocalResource**</span><span class="sxs-lookup"><span data-stu-id="ec857-260">**LocalResource**</span></span>|<span data-ttu-id="ec857-261">hello sökväg relativa tooa lokala resursen toomonitor.</span><span class="sxs-lookup"><span data-stu-id="ec857-261">hello path relative tooa local resource toomonitor.</span></span> <span data-ttu-id="ec857-262">Obligatoriska attribut är:</span><span class="sxs-lookup"><span data-stu-id="ec857-262">Required attributes are:</span></span><br /><br /> <span data-ttu-id="ec857-263">- **Namnet** -hello lokal resurs som innehåller hello directory toomonitor</span><span class="sxs-lookup"><span data-stu-id="ec857-263">- **Name** - hello local resource that contains hello directory toomonitor</span></span><br /><br /> <span data-ttu-id="ec857-264">- **relativePath** -hello relativa sökvägen tooName som innehåller hello directory toomonitor</span><span class="sxs-lookup"><span data-stu-id="ec857-264">- **relativePath** - hello path relative tooName that contains hello directory toomonitor</span></span>|  



## <a name="etwproviders-element"></a><span data-ttu-id="ec857-265">EtwProviders Element</span><span class="sxs-lookup"><span data-stu-id="ec857-265">EtwProviders Element</span></span>  
 <span data-ttu-id="ec857-266">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders*</span><span class="sxs-lookup"><span data-stu-id="ec857-266">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders*</span></span>

 <span data-ttu-id="ec857-267">Konfigurerar samling ETW-händelser från EventSource och/eller ETW Manifest baserade providers.</span><span class="sxs-lookup"><span data-stu-id="ec857-267">Configures collection of ETW events from EventSource and/or ETW Manifest based providers.</span></span>  

|<span data-ttu-id="ec857-268">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="ec857-268">Child Elements</span></span>|<span data-ttu-id="ec857-269">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec857-269">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="ec857-270">**EtwEventSourceProviderConfiguration**</span><span class="sxs-lookup"><span data-stu-id="ec857-270">**EtwEventSourceProviderConfiguration**</span></span>|<span data-ttu-id="ec857-271">Konfigurerar insamling av händelser som genereras från [EventSource klassen](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="ec857-271">Configures collection of events generated from [EventSource Class](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).</span></span> <span data-ttu-id="ec857-272">Obligatoriskt attribut:</span><span class="sxs-lookup"><span data-stu-id="ec857-272">Required attribute:</span></span><br /><br /> <span data-ttu-id="ec857-273">**Providern** -hello klassnamnet för hello EventSource händelse.</span><span class="sxs-lookup"><span data-stu-id="ec857-273">**provider** - hello class name of hello EventSource event.</span></span><br /><br /> <span data-ttu-id="ec857-274">Valfria attribut är:</span><span class="sxs-lookup"><span data-stu-id="ec857-274">Optional attributes are:</span></span><br /><br /> <span data-ttu-id="ec857-275">- **scheduledTransferLogLevelFilter** -hello lägsta allvarlighetsgrad nivå tootransfer tooyour storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ec857-275">- **scheduledTransferLogLevelFilter** - hello minimum severity level tootransfer tooyour storage account.</span></span><br /><br /> <span data-ttu-id="ec857-276">- **scheduledTransferPeriod** -hello intervallet mellan schemalagda överföringar toostorage avrundat toohello närmast minuter.</span><span class="sxs-lookup"><span data-stu-id="ec857-276">- **scheduledTransferPeriod** - hello interval between scheduled transfers toostorage rounded up toohello nearest minute.</span></span> <span data-ttu-id="ec857-277">hello-värdet är en [XML ”varaktighet datatyp”.](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span><span class="sxs-lookup"><span data-stu-id="ec857-277">hello value is an [XML “Duration Data Type.”](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span></span> |  
|<span data-ttu-id="ec857-278">**EtwManifestProviderConfiguration**</span><span class="sxs-lookup"><span data-stu-id="ec857-278">**EtwManifestProviderConfiguration**</span></span>|<span data-ttu-id="ec857-279">Obligatoriskt attribut:</span><span class="sxs-lookup"><span data-stu-id="ec857-279">Required attribute:</span></span><br /><br /> <span data-ttu-id="ec857-280">**Providern** -hello GUID för hello-Händelseprovidern</span><span class="sxs-lookup"><span data-stu-id="ec857-280">**provider** - hello GUID of hello event provider</span></span><br /><br /> <span data-ttu-id="ec857-281">Valfria attribut är:</span><span class="sxs-lookup"><span data-stu-id="ec857-281">Optional attributes are:</span></span><br /><br /> <span data-ttu-id="ec857-282">- **scheduledTransferLogLevelFilter** -hello lägsta allvarlighetsgrad nivå tootransfer tooyour storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ec857-282">- **scheduledTransferLogLevelFilter** - hello minimum severity level tootransfer tooyour storage account.</span></span><br /><br /> <span data-ttu-id="ec857-283">- **scheduledTransferPeriod** -hello intervallet mellan schemalagda överföringar toostorage avrundat toohello närmast minuter.</span><span class="sxs-lookup"><span data-stu-id="ec857-283">- **scheduledTransferPeriod** - hello interval between scheduled transfers toostorage rounded up toohello nearest minute.</span></span> <span data-ttu-id="ec857-284">hello-värdet är en [XML ”varaktighet datatyp”.](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span><span class="sxs-lookup"><span data-stu-id="ec857-284">hello value is an [XML “Duration Data Type.”](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span></span> |  



## <a name="etweventsourceproviderconfiguration-element"></a><span data-ttu-id="ec857-285">EtwEventSourceProviderConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="ec857-285">EtwEventSourceProviderConfiguration Element</span></span>  
 <span data-ttu-id="ec857-286">*: Trädrot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwEventSourceProviderConfiguration*</span><span class="sxs-lookup"><span data-stu-id="ec857-286">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders- EtwEventSourceProviderConfiguration*</span></span>

 <span data-ttu-id="ec857-287">Konfigurerar insamling av händelser som genereras från [EventSource klassen](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="ec857-287">Configures collection of events generated from [EventSource Class](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).</span></span>  

|<span data-ttu-id="ec857-288">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="ec857-288">Child Elements</span></span>|<span data-ttu-id="ec857-289">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec857-289">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="ec857-290">**DefaultEvents**</span><span class="sxs-lookup"><span data-stu-id="ec857-290">**DefaultEvents**</span></span>|<span data-ttu-id="ec857-291">Valfria attribut:</span><span class="sxs-lookup"><span data-stu-id="ec857-291">Optional attribute:</span></span><br/><br/> <span data-ttu-id="ec857-292">**eventDestination** - hello namn på tabellen hello toostore hello händelser i</span><span class="sxs-lookup"><span data-stu-id="ec857-292">**eventDestination** - hello name of hello table toostore hello events in</span></span>|  
|<span data-ttu-id="ec857-293">**Händelse**</span><span class="sxs-lookup"><span data-stu-id="ec857-293">**Event**</span></span>|<span data-ttu-id="ec857-294">Obligatoriskt attribut:</span><span class="sxs-lookup"><span data-stu-id="ec857-294">Required attribute:</span></span><br /><br /> <span data-ttu-id="ec857-295">**ID** -hello-id för hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="ec857-295">**id** - hello id of hello event.</span></span><br /><br /> <span data-ttu-id="ec857-296">Valfria attribut:</span><span class="sxs-lookup"><span data-stu-id="ec857-296">Optional attribute:</span></span><br /><br /> <span data-ttu-id="ec857-297">**eventDestination** - hello namn på tabellen hello toostore hello händelser i</span><span class="sxs-lookup"><span data-stu-id="ec857-297">**eventDestination** - hello name of hello table toostore hello events in</span></span>|  



## <a name="etwmanifestproviderconfiguration-element"></a><span data-ttu-id="ec857-298">EtwManifestProviderConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="ec857-298">EtwManifestProviderConfiguration Element</span></span>  
 <span data-ttu-id="ec857-299">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwManifestProviderConfiguration*</span><span class="sxs-lookup"><span data-stu-id="ec857-299">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwManifestProviderConfiguration*</span></span>

|<span data-ttu-id="ec857-300">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="ec857-300">Child Elements</span></span>|<span data-ttu-id="ec857-301">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec857-301">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="ec857-302">**DefaultEvents**</span><span class="sxs-lookup"><span data-stu-id="ec857-302">**DefaultEvents**</span></span>|<span data-ttu-id="ec857-303">Valfria attribut:</span><span class="sxs-lookup"><span data-stu-id="ec857-303">Optional attribute:</span></span><br /><br /> <span data-ttu-id="ec857-304">**eventDestination** - hello namn på tabellen hello toostore hello händelser i</span><span class="sxs-lookup"><span data-stu-id="ec857-304">**eventDestination** - hello name of hello table toostore hello events in</span></span>|  
|<span data-ttu-id="ec857-305">**Händelse**</span><span class="sxs-lookup"><span data-stu-id="ec857-305">**Event**</span></span>|<span data-ttu-id="ec857-306">Obligatoriskt attribut:</span><span class="sxs-lookup"><span data-stu-id="ec857-306">Required attribute:</span></span><br /><br /> <span data-ttu-id="ec857-307">**ID** -hello-id för hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="ec857-307">**id** - hello id of hello event.</span></span><br /><br /> <span data-ttu-id="ec857-308">Valfria attribut:</span><span class="sxs-lookup"><span data-stu-id="ec857-308">Optional attribute:</span></span><br /><br /> <span data-ttu-id="ec857-309">**eventDestination** - hello namn på tabellen hello toostore hello händelser i</span><span class="sxs-lookup"><span data-stu-id="ec857-309">**eventDestination** - hello name of hello table toostore hello events in</span></span>|  



## <a name="metrics-element"></a><span data-ttu-id="ec857-310">Mått Element</span><span class="sxs-lookup"><span data-stu-id="ec857-310">Metrics Element</span></span>  
 <span data-ttu-id="ec857-311">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - mått*</span><span class="sxs-lookup"><span data-stu-id="ec857-311">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Metrics*</span></span>

 <span data-ttu-id="ec857-312">Låter dig toogenerate en prestandaräknaren tabell som är optimerad för snabb frågor.</span><span class="sxs-lookup"><span data-stu-id="ec857-312">Enables you toogenerate a performance counter table that is optimized for fast queries.</span></span> <span data-ttu-id="ec857-313">Varje prestandaräknare som definieras i hello **PerformanceCounters** element lagras i hello mått tabellen i tillägg toohello prestandaräknaren tabell.</span><span class="sxs-lookup"><span data-stu-id="ec857-313">Each performance counter that is defined in hello **PerformanceCounters** element is stored in hello Metrics table in addition toohello Performance Counter table.</span></span>  

 <span data-ttu-id="ec857-314">Hej **resourceId** -attribut krävs.</span><span class="sxs-lookup"><span data-stu-id="ec857-314">hello **resourceId** attribute is required.</span></span>  <span data-ttu-id="ec857-315">hello resurs-ID för hello virtuell dator som du distribuerar till Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="ec857-315">hello resource ID of hello Virtual Machine you are deploying Azure Diagnostics to.</span></span> <span data-ttu-id="ec857-316">Hämta hello **resourceID** från hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ec857-316">Get hello **resourceID** from hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="ec857-317">Välj **Bläddra** -> **resursgrupper** -> **< namn\>**.</span><span class="sxs-lookup"><span data-stu-id="ec857-317">Select **Browse** -> **Resource Groups** -> **<Name\>**.</span></span> <span data-ttu-id="ec857-318">Klicka på hello **egenskaper** panelen och kopiera hello värdet från hello **ID** fältet.</span><span class="sxs-lookup"><span data-stu-id="ec857-318">Click hello **Properties** tile and copy hello value from hello **ID** field.</span></span>  

|<span data-ttu-id="ec857-319">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="ec857-319">Child Elements</span></span>|<span data-ttu-id="ec857-320">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec857-320">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="ec857-321">**MetricAggregation**</span><span class="sxs-lookup"><span data-stu-id="ec857-321">**MetricAggregation**</span></span>|<span data-ttu-id="ec857-322">Obligatoriskt attribut:</span><span class="sxs-lookup"><span data-stu-id="ec857-322">Required attribute:</span></span><br /><br /> <span data-ttu-id="ec857-323">**scheduledTransferPeriod** -hello intervallet mellan schemalagda överföringar toostorage avrundat toohello närmast minuter.</span><span class="sxs-lookup"><span data-stu-id="ec857-323">**scheduledTransferPeriod** - hello interval between scheduled transfers toostorage rounded up toohello nearest minute.</span></span> <span data-ttu-id="ec857-324">hello-värdet är en [XML ”varaktighet datatyp”.](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span><span class="sxs-lookup"><span data-stu-id="ec857-324">hello value is an [XML “Duration Data Type.”](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span></span> |  



## <a name="performancecounters-element"></a><span data-ttu-id="ec857-325">PerformanceCounters elementet</span><span class="sxs-lookup"><span data-stu-id="ec857-325">PerformanceCounters Element</span></span>  
 <span data-ttu-id="ec857-326">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - prestandaräknarna*</span><span class="sxs-lookup"><span data-stu-id="ec857-326">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - PerformanceCounters*</span></span>

 <span data-ttu-id="ec857-327">Aktiverar hello insamling av prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="ec857-327">Enables hello collection of performance counters.</span></span>  

 <span data-ttu-id="ec857-328">Valfria attribut:</span><span class="sxs-lookup"><span data-stu-id="ec857-328">Optional attribute:</span></span>  

 <span data-ttu-id="ec857-329">Valfria **scheduledTransferPeriod** attribut.</span><span class="sxs-lookup"><span data-stu-id="ec857-329">Optional **scheduledTransferPeriod** attribute.</span></span> <span data-ttu-id="ec857-330">Se tidigare förklaring.</span><span class="sxs-lookup"><span data-stu-id="ec857-330">See explanation earlier.</span></span>

|<span data-ttu-id="ec857-331">Underordnat Element</span><span class="sxs-lookup"><span data-stu-id="ec857-331">Child Element</span></span>|<span data-ttu-id="ec857-332">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec857-332">Description</span></span>|  
|-------------------|-----------------|  
|<span data-ttu-id="ec857-333">**PerformanceCounterConfiguration**</span><span class="sxs-lookup"><span data-stu-id="ec857-333">**PerformanceCounterConfiguration**</span></span>|<span data-ttu-id="ec857-334">Det krävs hello följande attribut:</span><span class="sxs-lookup"><span data-stu-id="ec857-334">hello following attributes are required:</span></span><br /><br /> <span data-ttu-id="ec857-335">- **counterSpecifier** - hello namn för hello prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="ec857-335">- **counterSpecifier** - hello name of hello performance counter.</span></span> <span data-ttu-id="ec857-336">Till exempel `\Processor(_Total)\% Processor Time`.</span><span class="sxs-lookup"><span data-stu-id="ec857-336">For example, `\Processor(_Total)\% Processor Time`.</span></span> <span data-ttu-id="ec857-337">tooget en lista över prestanda prestandaräknare på din värd kör hello kommando `typeperf`.</span><span class="sxs-lookup"><span data-stu-id="ec857-337">tooget a list of performance counters on your host, run hello command `typeperf`.</span></span><br /><br /> <span data-ttu-id="ec857-338">- **sampleRate** -hur ofta hello räknaren ska samlas in.</span><span class="sxs-lookup"><span data-stu-id="ec857-338">- **sampleRate** - How often hello counter should be sampled.</span></span><br /><br /> <span data-ttu-id="ec857-339">Valfria attribut:</span><span class="sxs-lookup"><span data-stu-id="ec857-339">Optional attribute:</span></span><br /><br /> <span data-ttu-id="ec857-340">**enhet** -hello måttenhet hello räknaren.</span><span class="sxs-lookup"><span data-stu-id="ec857-340">**unit** - hello unit of measure of hello counter.</span></span>|  




## <a name="windowseventlog-element"></a><span data-ttu-id="ec857-341">WindowsEventLog Element</span><span class="sxs-lookup"><span data-stu-id="ec857-341">WindowsEventLog Element</span></span>
 <span data-ttu-id="ec857-342">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - WindowsEventLog*</span><span class="sxs-lookup"><span data-stu-id="ec857-342">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - WindowsEventLog*</span></span>
 
 <span data-ttu-id="ec857-343">Aktiverar hello insamling av Windows-händelseloggar.</span><span class="sxs-lookup"><span data-stu-id="ec857-343">Enables hello collection of Windows Event Logs.</span></span>  

 <span data-ttu-id="ec857-344">Valfria **scheduledTransferPeriod** attribut.</span><span class="sxs-lookup"><span data-stu-id="ec857-344">Optional **scheduledTransferPeriod** attribute.</span></span> <span data-ttu-id="ec857-345">Se tidigare förklaring.</span><span class="sxs-lookup"><span data-stu-id="ec857-345">See explanation  earlier.</span></span>  

|<span data-ttu-id="ec857-346">Underordnat Element</span><span class="sxs-lookup"><span data-stu-id="ec857-346">Child Element</span></span>|<span data-ttu-id="ec857-347">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec857-347">Description</span></span>|  
|-------------------|-----------------|  
|<span data-ttu-id="ec857-348">**DataSource**</span><span class="sxs-lookup"><span data-stu-id="ec857-348">**DataSource**</span></span>|<span data-ttu-id="ec857-349">Hej toocollect för Windows-händelseloggar.</span><span class="sxs-lookup"><span data-stu-id="ec857-349">hello Windows Event logs toocollect.</span></span> <span data-ttu-id="ec857-350">Obligatoriskt attribut:</span><span class="sxs-lookup"><span data-stu-id="ec857-350">Required attribute:</span></span><br /><br /> <span data-ttu-id="ec857-351">**namnet** -hello XPath-fråga som beskriver hello windows händelser toobe samlas in.</span><span class="sxs-lookup"><span data-stu-id="ec857-351">**name** - hello XPath query describing hello windows events toobe collected.</span></span> <span data-ttu-id="ec857-352">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ec857-352">For example:</span></span><br /><br /> `Application!*[System[(Level <=3)]], System!*[System[(Level <=3)]], System!*[System[Provider[@Name='Microsoft Antimalware']]], Security!*[System[(Level <= 3)]`<br /><br /> <span data-ttu-id="ec857-353">Ange om toocollect alla händelser ”*”</span><span class="sxs-lookup"><span data-stu-id="ec857-353">toocollect all events, specify "*"</span></span>|  




## <a name="logs-element"></a><span data-ttu-id="ec857-354">Loggar Element</span><span class="sxs-lookup"><span data-stu-id="ec857-354">Logs Element</span></span>  
 <span data-ttu-id="ec857-355">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - loggar*</span><span class="sxs-lookup"><span data-stu-id="ec857-355">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Logs*</span></span>

 <span data-ttu-id="ec857-356">Ge i version 1.0- och 1.1.</span><span class="sxs-lookup"><span data-stu-id="ec857-356">Present in version 1.0 and 1.1.</span></span> <span data-ttu-id="ec857-357">Saknas i 1.2.</span><span class="sxs-lookup"><span data-stu-id="ec857-357">Missing in 1.2.</span></span> <span data-ttu-id="ec857-358">Lägga till i 1.3.</span><span class="sxs-lookup"><span data-stu-id="ec857-358">Added back in 1.3.</span></span>  

 <span data-ttu-id="ec857-359">Definierar hello buffert konfigurationen för grundläggande Azure loggar.</span><span class="sxs-lookup"><span data-stu-id="ec857-359">Defines hello buffer configuration for basic Azure logs.</span></span>  

|<span data-ttu-id="ec857-360">Attribut</span><span class="sxs-lookup"><span data-stu-id="ec857-360">Attribute</span></span>|<span data-ttu-id="ec857-361">Typ</span><span class="sxs-lookup"><span data-stu-id="ec857-361">Type</span></span>|<span data-ttu-id="ec857-362">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec857-362">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="ec857-363">**bufferQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="ec857-363">**bufferQuotaInMB**</span></span>|<span data-ttu-id="ec857-364">**unsignedInt**</span><span class="sxs-lookup"><span data-stu-id="ec857-364">**unsignedInt**</span></span>|<span data-ttu-id="ec857-365">Valfri.</span><span class="sxs-lookup"><span data-stu-id="ec857-365">Optional.</span></span> <span data-ttu-id="ec857-366">Anger hello maximal mängd fillagring för system som är tillgängligt för hello angivna data.</span><span class="sxs-lookup"><span data-stu-id="ec857-366">Specifies hello maximum amount of file system storage that is available for hello specified data.</span></span><br /><br /> <span data-ttu-id="ec857-367">hello standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="ec857-367">hello default is 0.</span></span>|  
|<span data-ttu-id="ec857-368">**scheduledTransferLogLevelFilterr**</span><span class="sxs-lookup"><span data-stu-id="ec857-368">**scheduledTransferLogLevelFilterr**</span></span>|<span data-ttu-id="ec857-369">**sträng**</span><span class="sxs-lookup"><span data-stu-id="ec857-369">**string**</span></span>|<span data-ttu-id="ec857-370">Valfri.</span><span class="sxs-lookup"><span data-stu-id="ec857-370">Optional.</span></span> <span data-ttu-id="ec857-371">Anger hello lägsta allvarlighetsgrad för loggposter som överförs.</span><span class="sxs-lookup"><span data-stu-id="ec857-371">Specifies hello minimum severity level for log entries that are transferred.</span></span> <span data-ttu-id="ec857-372">hello standardvärdet är **Undefined**, som överför alla loggar.</span><span class="sxs-lookup"><span data-stu-id="ec857-372">hello default value is **Undefined**, which transfers all logs.</span></span> <span data-ttu-id="ec857-373">Andra möjliga värden (i ordningen för de flesta tooleast information) är **utförlig**, **Information**, **varning**, **fel**, och **Kritiska**.</span><span class="sxs-lookup"><span data-stu-id="ec857-373">Other possible values (in order of most tooleast information) are **Verbose**, **Information**, **Warning**, **Error**, and **Critical**.</span></span>|  
|<span data-ttu-id="ec857-374">**scheduledTransferPeriod**</span><span class="sxs-lookup"><span data-stu-id="ec857-374">**scheduledTransferPeriod**</span></span>|<span data-ttu-id="ec857-375">**varaktighet**</span><span class="sxs-lookup"><span data-stu-id="ec857-375">**duration**</span></span>|<span data-ttu-id="ec857-376">Valfri.</span><span class="sxs-lookup"><span data-stu-id="ec857-376">Optional.</span></span> <span data-ttu-id="ec857-377">Anger hello intervallet mellan schemalagda dataöverföringar, avrundat toohello närmast minuter.</span><span class="sxs-lookup"><span data-stu-id="ec857-377">Specifies hello interval between scheduled transfers of data, rounded up toohello nearest minute.</span></span><br /><br /> <span data-ttu-id="ec857-378">hello standardvärdet är PT0S.</span><span class="sxs-lookup"><span data-stu-id="ec857-378">hello default is PT0S.</span></span>|  
|<span data-ttu-id="ec857-379">**egenskaperna** lades till i 1.5</span><span class="sxs-lookup"><span data-stu-id="ec857-379">**sinks** Added in 1.5</span></span>|<span data-ttu-id="ec857-380">**sträng**</span><span class="sxs-lookup"><span data-stu-id="ec857-380">**string**</span></span>|<span data-ttu-id="ec857-381">Valfri.</span><span class="sxs-lookup"><span data-stu-id="ec857-381">Optional.</span></span> <span data-ttu-id="ec857-382">Punkter tooa sink plats tooalso skicka diagnostikdata.</span><span class="sxs-lookup"><span data-stu-id="ec857-382">Points tooa sink location tooalso send diagnostic data.</span></span> <span data-ttu-id="ec857-383">Till exempel Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ec857-383">For example, Application Insights.</span></span>|  

## <a name="dockersources"></a><span data-ttu-id="ec857-384">DockerSources</span><span class="sxs-lookup"><span data-stu-id="ec857-384">DockerSources</span></span>
 <span data-ttu-id="ec857-385">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - DockerSources*</span><span class="sxs-lookup"><span data-stu-id="ec857-385">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - DockerSources*</span></span>

 <span data-ttu-id="ec857-386">Lägga till i 1.9.</span><span class="sxs-lookup"><span data-stu-id="ec857-386">Added in 1.9.</span></span>

|<span data-ttu-id="ec857-387">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="ec857-387">Element Name</span></span>|<span data-ttu-id="ec857-388">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec857-388">Description</span></span>|  
|------------------|-----------------|  
|<span data-ttu-id="ec857-389">**Statistik**</span><span class="sxs-lookup"><span data-stu-id="ec857-389">**Stats**</span></span>|<span data-ttu-id="ec857-390">Visar hello system toocollect statistik för Docker-behållare</span><span class="sxs-lookup"><span data-stu-id="ec857-390">Tells hello system toocollect stats for Docker containers</span></span>|  

## <a name="sinksconfig-element"></a><span data-ttu-id="ec857-391">SinksConfig Element</span><span class="sxs-lookup"><span data-stu-id="ec857-391">SinksConfig Element</span></span>  
 <span data-ttu-id="ec857-392">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig*</span><span class="sxs-lookup"><span data-stu-id="ec857-392">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig*</span></span>

 <span data-ttu-id="ec857-393">En lista över platser toosend diagnostik tooand hello Konfigurationshanteraren för data som är kopplade till dessa platser.</span><span class="sxs-lookup"><span data-stu-id="ec857-393">A list of locations toosend diagnostics data tooand hello configuration associated with those locations.</span></span>  

|<span data-ttu-id="ec857-394">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="ec857-394">Element Name</span></span>|<span data-ttu-id="ec857-395">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec857-395">Description</span></span>|  
|------------------|-----------------|  
|<span data-ttu-id="ec857-396">**Sink**</span><span class="sxs-lookup"><span data-stu-id="ec857-396">**Sink**</span></span>|<span data-ttu-id="ec857-397">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="ec857-397">See description elsewhere on this page.</span></span>|  

## <a name="sink-element"></a><span data-ttu-id="ec857-398">Sink Element</span><span class="sxs-lookup"><span data-stu-id="ec857-398">Sink Element</span></span>
 <span data-ttu-id="ec857-399">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - mottagare*</span><span class="sxs-lookup"><span data-stu-id="ec857-399">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink*</span></span>

 <span data-ttu-id="ec857-400">Lägga till i version 1.5.</span><span class="sxs-lookup"><span data-stu-id="ec857-400">Added in version 1.5.</span></span>  

 <span data-ttu-id="ec857-401">Definierar platser toosend diagnostikdata till.</span><span class="sxs-lookup"><span data-stu-id="ec857-401">Defines locations toosend diagnostic data to.</span></span> <span data-ttu-id="ec857-402">Till exempel hello Application Insights-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ec857-402">For example, hello Application Insights service.</span></span>  

|<span data-ttu-id="ec857-403">Attribut</span><span class="sxs-lookup"><span data-stu-id="ec857-403">Attribute</span></span>|<span data-ttu-id="ec857-404">Typ</span><span class="sxs-lookup"><span data-stu-id="ec857-404">Type</span></span>|<span data-ttu-id="ec857-405">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec857-405">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="ec857-406">**Namn**</span><span class="sxs-lookup"><span data-stu-id="ec857-406">**name**</span></span>|<span data-ttu-id="ec857-407">Sträng</span><span class="sxs-lookup"><span data-stu-id="ec857-407">string</span></span>|<span data-ttu-id="ec857-408">En sträng identifierande hello sinkname.</span><span class="sxs-lookup"><span data-stu-id="ec857-408">A string identifying hello sinkname.</span></span>|  

|<span data-ttu-id="ec857-409">Element</span><span class="sxs-lookup"><span data-stu-id="ec857-409">Element</span></span>|<span data-ttu-id="ec857-410">Typ</span><span class="sxs-lookup"><span data-stu-id="ec857-410">Type</span></span>|<span data-ttu-id="ec857-411">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec857-411">Description</span></span>|  
|-------------|----------|-----------------|  
|<span data-ttu-id="ec857-412">**Application Insights**</span><span class="sxs-lookup"><span data-stu-id="ec857-412">**Application Insights**</span></span>|<span data-ttu-id="ec857-413">Sträng</span><span class="sxs-lookup"><span data-stu-id="ec857-413">string</span></span>|<span data-ttu-id="ec857-414">Används bara när du skickar data tooApplication insikter.</span><span class="sxs-lookup"><span data-stu-id="ec857-414">Used only when sending data tooApplication Insights.</span></span> <span data-ttu-id="ec857-415">Innehåller hello Instrumentation nyckeln för ett aktivt Application Insights-konto som du har åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="ec857-415">Contain hello Instrumentation Key for an active Application Insights account that you have access to.</span></span>|  
|<span data-ttu-id="ec857-416">**Kanaler**</span><span class="sxs-lookup"><span data-stu-id="ec857-416">**Channels**</span></span>|<span data-ttu-id="ec857-417">Sträng</span><span class="sxs-lookup"><span data-stu-id="ec857-417">string</span></span>|<span data-ttu-id="ec857-418">En för varje ytterligare filtrering strömma som du</span><span class="sxs-lookup"><span data-stu-id="ec857-418">One for each additional filtering that stream that you</span></span>|  

## <a name="channels-element"></a><span data-ttu-id="ec857-419">Kanaler Element</span><span class="sxs-lookup"><span data-stu-id="ec857-419">Channels Element</span></span>  
 <span data-ttu-id="ec857-420">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink - kanaler*</span><span class="sxs-lookup"><span data-stu-id="ec857-420">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink - Channels*</span></span>

 <span data-ttu-id="ec857-421">Lägga till i version 1.5.</span><span class="sxs-lookup"><span data-stu-id="ec857-421">Added in version 1.5.</span></span>  

 <span data-ttu-id="ec857-422">Definierar filter för dataströmmar logg går genom en mottagare.</span><span class="sxs-lookup"><span data-stu-id="ec857-422">Defines filters for streams of log data passing through a sink.</span></span>  

|<span data-ttu-id="ec857-423">Element</span><span class="sxs-lookup"><span data-stu-id="ec857-423">Element</span></span>|<span data-ttu-id="ec857-424">Typ</span><span class="sxs-lookup"><span data-stu-id="ec857-424">Type</span></span>|<span data-ttu-id="ec857-425">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec857-425">Description</span></span>|  
|-------------|----------|-----------------|  
|<span data-ttu-id="ec857-426">**Kanal**</span><span class="sxs-lookup"><span data-stu-id="ec857-426">**Channel**</span></span>|<span data-ttu-id="ec857-427">Sträng</span><span class="sxs-lookup"><span data-stu-id="ec857-427">string</span></span>|<span data-ttu-id="ec857-428">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="ec857-428">See description elsewhere on this page.</span></span>|  

## <a name="channel-element"></a><span data-ttu-id="ec857-429">Kanal Element</span><span class="sxs-lookup"><span data-stu-id="ec857-429">Channel Element</span></span>
 <span data-ttu-id="ec857-430">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink - kanaler - kanal*</span><span class="sxs-lookup"><span data-stu-id="ec857-430">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink - Channels - Channel*</span></span>

 <span data-ttu-id="ec857-431">Lägga till i version 1.5.</span><span class="sxs-lookup"><span data-stu-id="ec857-431">Added in version 1.5.</span></span>  

 <span data-ttu-id="ec857-432">Definierar platser toosend diagnostikdata till.</span><span class="sxs-lookup"><span data-stu-id="ec857-432">Defines locations toosend diagnostic data to.</span></span> <span data-ttu-id="ec857-433">Till exempel hello Application Insights-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ec857-433">For example, hello Application Insights service.</span></span>  

|<span data-ttu-id="ec857-434">Attribut</span><span class="sxs-lookup"><span data-stu-id="ec857-434">Attributes</span></span>|<span data-ttu-id="ec857-435">Typ</span><span class="sxs-lookup"><span data-stu-id="ec857-435">Type</span></span>|<span data-ttu-id="ec857-436">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec857-436">Description</span></span>|  
|----------------|----------|-----------------|  
|<span data-ttu-id="ec857-437">**logLevel**</span><span class="sxs-lookup"><span data-stu-id="ec857-437">**logLevel**</span></span>|<span data-ttu-id="ec857-438">**sträng**</span><span class="sxs-lookup"><span data-stu-id="ec857-438">**string**</span></span>|<span data-ttu-id="ec857-439">Anger hello lägsta allvarlighetsgrad för loggposter som överförs.</span><span class="sxs-lookup"><span data-stu-id="ec857-439">Specifies hello minimum severity level for log entries that are transferred.</span></span> <span data-ttu-id="ec857-440">hello standardvärdet är **Undefined**, som överför alla loggar.</span><span class="sxs-lookup"><span data-stu-id="ec857-440">hello default value is **Undefined**, which transfers all logs.</span></span> <span data-ttu-id="ec857-441">Andra möjliga värden (i ordningen för de flesta tooleast information) är **utförlig**, **Information**, **varning**, **fel**, och **Kritiska**.</span><span class="sxs-lookup"><span data-stu-id="ec857-441">Other possible values (in order of most tooleast information) are **Verbose**, **Information**, **Warning**, **Error**, and **Critical**.</span></span>|  
|<span data-ttu-id="ec857-442">**Namn**</span><span class="sxs-lookup"><span data-stu-id="ec857-442">**name**</span></span>|<span data-ttu-id="ec857-443">**sträng**</span><span class="sxs-lookup"><span data-stu-id="ec857-443">**string**</span></span>|<span data-ttu-id="ec857-444">Ett unikt namn för hello kanal toorefer till</span><span class="sxs-lookup"><span data-stu-id="ec857-444">A unique name of hello channel toorefer to</span></span>|  


## <a name="privateconfig-element"></a><span data-ttu-id="ec857-445">PrivateConfig Element</span><span class="sxs-lookup"><span data-stu-id="ec857-445">PrivateConfig Element</span></span> 
 <span data-ttu-id="ec857-446">*Trädet: Rot - DiagnosticsConfiguration - PrivateConfig*</span><span class="sxs-lookup"><span data-stu-id="ec857-446">*Tree: Root - DiagnosticsConfiguration - PrivateConfig*</span></span>

 <span data-ttu-id="ec857-447">Lägga till i version 1.3.</span><span class="sxs-lookup"><span data-stu-id="ec857-447">Added in version 1.3.</span></span>  

 <span data-ttu-id="ec857-448">Valfri</span><span class="sxs-lookup"><span data-stu-id="ec857-448">Optional</span></span>  

 <span data-ttu-id="ec857-449">Lagrar hello privat information om hello storage-konto (namn, nyckel och slutpunkten).</span><span class="sxs-lookup"><span data-stu-id="ec857-449">Stores hello private details of hello storage account (name, key, and endpoint).</span></span> <span data-ttu-id="ec857-450">Den här informationen skickas toohello virtuella datorn, men kan inte hämtas från den.</span><span class="sxs-lookup"><span data-stu-id="ec857-450">This information is sent toohello virtual machine, but cannot be retrieved from it.</span></span>  

|<span data-ttu-id="ec857-451">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="ec857-451">Child Elements</span></span>|<span data-ttu-id="ec857-452">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec857-452">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="ec857-453">**StorageAccount**</span><span class="sxs-lookup"><span data-stu-id="ec857-453">**StorageAccount**</span></span>|<span data-ttu-id="ec857-454">Hej toouse för storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ec857-454">hello storage account toouse.</span></span> <span data-ttu-id="ec857-455">hello följande attribut krävs</span><span class="sxs-lookup"><span data-stu-id="ec857-455">hello following attributes are required</span></span><br /><br /> <span data-ttu-id="ec857-456">- **namnet** - hello namn på hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ec857-456">- **name** - hello name of hello storage account.</span></span><br /><br /> <span data-ttu-id="ec857-457">- **nyckeln** - hello nyckeln toohello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ec857-457">- **key** - hello key toohello storage account.</span></span><br /><br /> <span data-ttu-id="ec857-458">- **slutpunkten** -hello endpoint tooaccess hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ec857-458">- **endpoint** - hello endpoint tooaccess hello storage account.</span></span> <br /><br /> <span data-ttu-id="ec857-459">-**sasToken** (läggs till 1.8.1)-som du kan ange en SAS-token i stället för en lagringskontonyckel i hello privata config. Om hello lagringskontonyckel ignoreras.</span><span class="sxs-lookup"><span data-stu-id="ec857-459">-**sasToken** (added 1.8.1)- You can specify an SAS token instead of a storage account key in hello private config. If provided, hello storage account key is ignored.</span></span> <br /><span data-ttu-id="ec857-460">Krav för hello SAS-Token:</span><span class="sxs-lookup"><span data-stu-id="ec857-460">Requirements for hello SAS Token:</span></span> <br /><span data-ttu-id="ec857-461">– Stöder endast konto SAS-token</span><span class="sxs-lookup"><span data-stu-id="ec857-461">- Supports account SAS token only</span></span> <br /><span data-ttu-id="ec857-462">- *b*, *t* tjänsttyper krävs.</span><span class="sxs-lookup"><span data-stu-id="ec857-462">- *b*, *t* service types are required.</span></span> <br /> <span data-ttu-id="ec857-463">- *en*, *c*, *u*, *w* behörigheter som krävs.</span><span class="sxs-lookup"><span data-stu-id="ec857-463">- *a*, *c*, *u*, *w* permissions are required.</span></span> <br /> <span data-ttu-id="ec857-464">- *c*, *o* resurstyper krävs.</span><span class="sxs-lookup"><span data-stu-id="ec857-464">- *c*, *o* resource types are required.</span></span> <br /> <span data-ttu-id="ec857-465">– Stöder endast hello HTTPS-protokoll</span><span class="sxs-lookup"><span data-stu-id="ec857-465">- Supports hello HTTPS protocol only</span></span> <br /> <span data-ttu-id="ec857-466">-Start och förfallotiden måste vara giltigt.</span><span class="sxs-lookup"><span data-stu-id="ec857-466">- Start and expiry time must be valid.</span></span>|  


## <a name="isenabled-element"></a><span data-ttu-id="ec857-467">IsEnabled Element</span><span class="sxs-lookup"><span data-stu-id="ec857-467">IsEnabled Element</span></span>  
 <span data-ttu-id="ec857-468">*Trädet: Rot - DiagnosticsConfiguration - IsEnabled*</span><span class="sxs-lookup"><span data-stu-id="ec857-468">*Tree: Root - DiagnosticsConfiguration - IsEnabled*</span></span>

 <span data-ttu-id="ec857-469">Booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="ec857-469">Boolean.</span></span> <span data-ttu-id="ec857-470">Använd `true` tooenable hello diagnostik eller `false` toodisable hello diagnostik.</span><span class="sxs-lookup"><span data-stu-id="ec857-470">Use `true` tooenable hello diagnostics or `false` toodisable hello diagnostics.</span></span>
