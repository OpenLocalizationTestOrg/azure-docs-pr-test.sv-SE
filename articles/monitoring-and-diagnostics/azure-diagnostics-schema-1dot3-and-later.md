---
title: "Azure Diagnostics tillägget 1.3 och senare Konfigurationsschemat | Microsoft Docs"
description: Schemaversionen 1.3 och senare Azure diagnostics levereras som en del av Microsoft Azure SDK 2.4 och senare.
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
ms.openlocfilehash: 0d814825fb08452238a254ccd30bde230380c74c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-diagnostics-13-and-later-configuration-schema"></a><span data-ttu-id="f2200-103">Azure Diagnostics 1.3 och senare Konfigurationsschemat</span><span class="sxs-lookup"><span data-stu-id="f2200-103">Azure Diagnostics 1.3 and later configuration schema</span></span>
> [!NOTE]
> <span data-ttu-id="f2200-104">Tillägget för Azure-diagnostik är den komponent som används för att samla in prestandaräknare och annan statistik från:</span><span class="sxs-lookup"><span data-stu-id="f2200-104">The Azure Diagnostics extension is the component used to collect performance counters and other statistics from:</span></span>
> - <span data-ttu-id="f2200-105">Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="f2200-105">Azure Virtual Machines</span></span> 
> - <span data-ttu-id="f2200-106">Skalningsuppsättningar för Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="f2200-106">Virtual Machine Scale Sets</span></span>
> - <span data-ttu-id="f2200-107">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="f2200-107">Service Fabric</span></span> 
> - <span data-ttu-id="f2200-108">Molntjänster</span><span class="sxs-lookup"><span data-stu-id="f2200-108">Cloud Services</span></span> 
> - <span data-ttu-id="f2200-109">Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="f2200-109">Network Security Groups</span></span>
> 
> <span data-ttu-id="f2200-110">Den här sidan gäller endast om du använder någon av dessa tjänster.</span><span class="sxs-lookup"><span data-stu-id="f2200-110">This page is only relevant if you are using one of these services.</span></span>

<span data-ttu-id="f2200-111">Den här sidan gäller versioner 1.3 och senare (Azure SDK 2,4 och senare).</span><span class="sxs-lookup"><span data-stu-id="f2200-111">This page is valid for versions 1.3 and newer (Azure SDK 2.4 and newer).</span></span> <span data-ttu-id="f2200-112">Nyare konfigurationsavsnitt kommenterats ska visas i vilken version de har lagts till.</span><span class="sxs-lookup"><span data-stu-id="f2200-112">Newer configuration sections are commented to show in what version they were added.</span></span>  

<span data-ttu-id="f2200-113">Konfigurationsfilen som beskrivs här används för att ange inställningar för diagnostik när diagnostik monitor startar.</span><span class="sxs-lookup"><span data-stu-id="f2200-113">The configuration file described here is used to set diagnostic configuration settings when the diagnostics monitor starts.</span></span>  

<span data-ttu-id="f2200-114">Tillägget används tillsammans med andra produkter från Microsoft diagnostics som Azure-Monitor, Application Insights och logganalys.</span><span class="sxs-lookup"><span data-stu-id="f2200-114">The extension is used in conjunction with other Microsoft diagnostics products like Azure Monitor, Application Insights, and Log Analytics.</span></span>



<span data-ttu-id="f2200-115">Hämta schemadefinitionen offentliga konfiguration filen genom att köra följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="f2200-115">Download the public configuration file schema definition by executing the following PowerShell command:</span></span>  

```powershell  
(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File –Encoding utf8 -FilePath 'C:\temp\WadConfig.xsd'  
```  

<span data-ttu-id="f2200-116">Mer information om hur du använder Azure-diagnostik finns [Azure Diagnostics tillägget](azure-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="f2200-116">For more information about using Azure Diagnostics, see [Azure Diagnostics Extension](azure-diagnostics.md).</span></span>  

## <a name="example-of-the-diagnostics-configuration-file"></a><span data-ttu-id="f2200-117">Exempel på konfigurationsfilen diagnostik</span><span class="sxs-lookup"><span data-stu-id="f2200-117">Example of the diagnostics configuration file</span></span>  
 <span data-ttu-id="f2200-118">I följande exempel visas en typisk diagnostik konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="f2200-118">The following example shows a typical diagnostics configuration file:</span></span>  

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

<span data-ttu-id="f2200-119">JSON motsvarande tidigare XML-konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="f2200-119">JSON equivalent of the previous XML configuration file.</span></span> 

<span data-ttu-id="f2200-120">PublicConfig och PrivateConfig avgränsas eftersom json användning oftast skickas de som olika variabler.</span><span class="sxs-lookup"><span data-stu-id="f2200-120">The PublicConfig and PrivateConfig are separated because in most json usage cases, they are passed as different variables.</span></span> <span data-ttu-id="f2200-121">Dessa fall innehåller Resource Manager-mallar, Virtual Machine Scale set PowerShell och Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f2200-121">These cases include Resource Manager templates, Virtual Machine Scale set PowerShell, and Visual Studio.</span></span> 

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

## <a name="reading-this-page"></a><span data-ttu-id="f2200-122">Läsa den här sidan</span><span class="sxs-lookup"><span data-stu-id="f2200-122">Reading this page</span></span>  
 <span data-ttu-id="f2200-123">De taggar som följer är ungefär i ordning som visas i föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="f2200-123">The tags following are roughly in order shown in the preceding example.</span></span>  <span data-ttu-id="f2200-124">Om du inte ser en fullständig beskrivning där du förväntar dig, kan du söka sidan för elementet eller attributet.</span><span class="sxs-lookup"><span data-stu-id="f2200-124">If you don't see a full description where you expect it, search the page for the element or attribute.</span></span>  

## <a name="common-attribute-types"></a><span data-ttu-id="f2200-125">Vanliga attribut</span><span class="sxs-lookup"><span data-stu-id="f2200-125">Common Attribute Types</span></span>  
 <span data-ttu-id="f2200-126">**scheduledTransferPeriod** attributet visas i flera element.</span><span class="sxs-lookup"><span data-stu-id="f2200-126">**scheduledTransferPeriod** attribute appears in several elements.</span></span> <span data-ttu-id="f2200-127">Det är intervallet mellan schemalagda överföringar till lagring avrundat till närmaste minut.</span><span class="sxs-lookup"><span data-stu-id="f2200-127">It is the interval between scheduled transfers to storage rounded up to the nearest minute.</span></span> <span data-ttu-id="f2200-128">Värdet är en [XML ”varaktighet datatyp”.](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span><span class="sxs-lookup"><span data-stu-id="f2200-128">The value is an [XML “Duration Data Type.”](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span></span>


## <a name="diagnosticsconfiguration-element"></a><span data-ttu-id="f2200-129">DiagnosticsConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="f2200-129">DiagnosticsConfiguration Element</span></span>  
 <span data-ttu-id="f2200-130">*: Trädrot - DiagnosticsConfiguration*</span><span class="sxs-lookup"><span data-stu-id="f2200-130">*Tree: Root - DiagnosticsConfiguration*</span></span>

<span data-ttu-id="f2200-131">Lägga till i version 1.3.</span><span class="sxs-lookup"><span data-stu-id="f2200-131">Added in version 1.3.</span></span>  

<span data-ttu-id="f2200-132">Det översta elementet i konfigurationsfilen diagnostik.</span><span class="sxs-lookup"><span data-stu-id="f2200-132">The top-level element of the diagnostics configuration file.</span></span>  

<span data-ttu-id="f2200-133">**Attributet** xmlns - XML-namnområdet för diagnostik-konfigurationsfilen är:</span><span class="sxs-lookup"><span data-stu-id="f2200-133">**Attribute**  xmlns - The XML namespace for the diagnostics configuration file is:</span></span>  
<span data-ttu-id="f2200-134">http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration</span><span class="sxs-lookup"><span data-stu-id="f2200-134">http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration</span></span>  


|<span data-ttu-id="f2200-135">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="f2200-135">Child Elements</span></span>|<span data-ttu-id="f2200-136">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f2200-136">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="f2200-137">**PublicConfig**</span><span class="sxs-lookup"><span data-stu-id="f2200-137">**PublicConfig**</span></span>|<span data-ttu-id="f2200-138">Krävs.</span><span class="sxs-lookup"><span data-stu-id="f2200-138">Required.</span></span> <span data-ttu-id="f2200-139">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="f2200-139">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="f2200-140">**PrivateConfig**</span><span class="sxs-lookup"><span data-stu-id="f2200-140">**PrivateConfig**</span></span>|<span data-ttu-id="f2200-141">Valfri.</span><span class="sxs-lookup"><span data-stu-id="f2200-141">Optional.</span></span> <span data-ttu-id="f2200-142">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="f2200-142">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="f2200-143">**IsEnabled**</span><span class="sxs-lookup"><span data-stu-id="f2200-143">**IsEnabled**</span></span>|<span data-ttu-id="f2200-144">Booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="f2200-144">Boolean.</span></span> <span data-ttu-id="f2200-145">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="f2200-145">See description elsewhere on this page.</span></span>|  

## <a name="publicconfig-element"></a><span data-ttu-id="f2200-146">PublicConfig Element</span><span class="sxs-lookup"><span data-stu-id="f2200-146">PublicConfig Element</span></span>  
 <span data-ttu-id="f2200-147">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig*</span><span class="sxs-lookup"><span data-stu-id="f2200-147">*Tree: Root - DiagnosticsConfiguration - PublicConfig*</span></span>

 <span data-ttu-id="f2200-148">Beskriver offentliga diagnostik-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="f2200-148">Describes the public diagnostics configuration.</span></span>  

|<span data-ttu-id="f2200-149">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="f2200-149">Child Elements</span></span>|<span data-ttu-id="f2200-150">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f2200-150">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="f2200-151">**WadCfg**</span><span class="sxs-lookup"><span data-stu-id="f2200-151">**WadCfg**</span></span>|<span data-ttu-id="f2200-152">Krävs.</span><span class="sxs-lookup"><span data-stu-id="f2200-152">Required.</span></span> <span data-ttu-id="f2200-153">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="f2200-153">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="f2200-154">**StorageAccount**</span><span class="sxs-lookup"><span data-stu-id="f2200-154">**StorageAccount**</span></span>|<span data-ttu-id="f2200-155">Namnet på Azure Storage-konto för att lagra data i.</span><span class="sxs-lookup"><span data-stu-id="f2200-155">The name of the Azure Storage account to store the data in.</span></span> <span data-ttu-id="f2200-156">Kan också anges som en parameter när du kör cmdlet Set-AzureServiceDiagnosticsExtension.</span><span class="sxs-lookup"><span data-stu-id="f2200-156">May also be specified as a parameter when executing the Set-AzureServiceDiagnosticsExtension cmdlet.</span></span>|  
|<span data-ttu-id="f2200-157">**StorageType**</span><span class="sxs-lookup"><span data-stu-id="f2200-157">**StorageType**</span></span>|<span data-ttu-id="f2200-158">Kan vara *tabell*, *Blob*, eller *TableAndBlob*.</span><span class="sxs-lookup"><span data-stu-id="f2200-158">Can be *Table*, *Blob*, or *TableAndBlob*.</span></span> <span data-ttu-id="f2200-159">Tabellen är standard.</span><span class="sxs-lookup"><span data-stu-id="f2200-159">Table is default.</span></span> <span data-ttu-id="f2200-160">När du har valt TableAndBlob skrivs diagnostikdata två gånger – en gång till varje typ av.</span><span class="sxs-lookup"><span data-stu-id="f2200-160">When TableAndBlob is chosen, diagnostic data is written twice -- once to each type.</span></span>|  
|<span data-ttu-id="f2200-161">**LocalResourceDirectory**</span><span class="sxs-lookup"><span data-stu-id="f2200-161">**LocalResourceDirectory**</span></span>|<span data-ttu-id="f2200-162">Katalogen på den virtuella datorn där Monitoring Agent lagrar händelsedata.</span><span class="sxs-lookup"><span data-stu-id="f2200-162">The directory on the virtual machine where the Monitoring Agent stores event data.</span></span> <span data-ttu-id="f2200-163">Om du inte har angetts standardkatalogen:</span><span class="sxs-lookup"><span data-stu-id="f2200-163">If not, set, the default directory is used:</span></span><br /><br /> <span data-ttu-id="f2200-164">För en arbetare/webbroll:`C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`</span><span class="sxs-lookup"><span data-stu-id="f2200-164">For a Worker/web role: `C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`</span></span><br /><br /> <span data-ttu-id="f2200-165">För en virtuell dator:`C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`</span><span class="sxs-lookup"><span data-stu-id="f2200-165">For a Virtual Machine: `C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`</span></span><br /><br /> <span data-ttu-id="f2200-166">Obligatoriska attribut är:</span><span class="sxs-lookup"><span data-stu-id="f2200-166">Required attributes are:</span></span><br /><br /> <span data-ttu-id="f2200-167">- **sökvägen** -katalogen på system som ska användas av Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="f2200-167">- **path** - The directory on the system to be used by Azure Diagnostics.</span></span><br /><br /> <span data-ttu-id="f2200-168">- **expandEnvironment** -styr om expanderas miljövariabler i sökvägen.</span><span class="sxs-lookup"><span data-stu-id="f2200-168">- **expandEnvironment** - Controls whether environment variables are expanded in the path name.</span></span>|  

## <a name="wadcfg-element"></a><span data-ttu-id="f2200-169">WadCFG Element</span><span class="sxs-lookup"><span data-stu-id="f2200-169">WadCFG Element</span></span>  
 <span data-ttu-id="f2200-170">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG*</span><span class="sxs-lookup"><span data-stu-id="f2200-170">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG*</span></span>
 
 <span data-ttu-id="f2200-171">Identifierar och konfigurerar att samla in telemetridata.</span><span class="sxs-lookup"><span data-stu-id="f2200-171">Identifies and configures the telemetry data to be collected.</span></span>  


## <a name="diagnosticmonitorconfiguration-element"></a><span data-ttu-id="f2200-172">DiagnosticMonitorConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="f2200-172">DiagnosticMonitorConfiguration Element</span></span> 
 <span data-ttu-id="f2200-173">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration*</span><span class="sxs-lookup"><span data-stu-id="f2200-173">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration*</span></span>

 <span data-ttu-id="f2200-174">Krävs</span><span class="sxs-lookup"><span data-stu-id="f2200-174">Required</span></span> 

|<span data-ttu-id="f2200-175">Attribut</span><span class="sxs-lookup"><span data-stu-id="f2200-175">Attributes</span></span>|<span data-ttu-id="f2200-176">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f2200-176">Description</span></span>|  
|----------------|-----------------|  
| <span data-ttu-id="f2200-177">**overallQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="f2200-177">**overallQuotaInMB**</span></span> | <span data-ttu-id="f2200-178">Den maximala mängden lokalt diskutrymme som kan användas av de olika typerna av diagnostiska data som samlas in av Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="f2200-178">The maximum amount of local disk space that may be consumed by the various types of diagnostic data collected by Azure Diagnostics.</span></span> <span data-ttu-id="f2200-179">Standardinställningen är 5 120 MB.</span><span class="sxs-lookup"><span data-stu-id="f2200-179">The default setting is 5120 MB.</span></span><br />
|<span data-ttu-id="f2200-180">**useProxyServer**</span><span class="sxs-lookup"><span data-stu-id="f2200-180">**useProxyServer**</span></span> | <span data-ttu-id="f2200-181">Konfigurera Azure-diagnostik för att använda inställningarna för proxyservern som angetts i Internet Explorer-inställningar.</span><span class="sxs-lookup"><span data-stu-id="f2200-181">Configure Azure Diagnostics to use the proxy server settings as set in IE settings.</span></span>|  

<br /> <br />

|<span data-ttu-id="f2200-182">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="f2200-182">Child Elements</span></span>|<span data-ttu-id="f2200-183">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f2200-183">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="f2200-184">**CrashDumps**</span><span class="sxs-lookup"><span data-stu-id="f2200-184">**CrashDumps**</span></span>|<span data-ttu-id="f2200-185">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="f2200-185">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="f2200-186">**DiagnosticInfrastructureLogs**</span><span class="sxs-lookup"><span data-stu-id="f2200-186">**DiagnosticInfrastructureLogs**</span></span>|<span data-ttu-id="f2200-187">Aktivera insamling av loggar som genereras av Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="f2200-187">Enable collection of logs generated by Azure Diagnostics.</span></span> <span data-ttu-id="f2200-188">Infrastruktur för diagnostiska loggar är användbara för att felsöka systemets diagnostik.</span><span class="sxs-lookup"><span data-stu-id="f2200-188">The diagnostic infrastructure logs are useful for troubleshooting the diagnostics system itself.</span></span> <span data-ttu-id="f2200-189">Valfria attribut är:</span><span class="sxs-lookup"><span data-stu-id="f2200-189">Optional attributes are:</span></span><br /><br /> <span data-ttu-id="f2200-190">- **scheduledTransferLogLevelFilter** -konfigurerar lägsta allvarlighetsgrad för loggar samlas in.</span><span class="sxs-lookup"><span data-stu-id="f2200-190">- **scheduledTransferLogLevelFilter** - Configures the minimum severity level of the logs collected.</span></span><br /><br /> <span data-ttu-id="f2200-191">- **scheduledTransferPeriod** -intervallet mellan schemalagda överföringar till lagring avrundat uppåt till närmaste minut.</span><span class="sxs-lookup"><span data-stu-id="f2200-191">- **scheduledTransferPeriod** - The interval between scheduled transfers to storage rounded up to the nearest minute.</span></span> <span data-ttu-id="f2200-192">Värdet är en [XML ”varaktighet datatyp”.](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span><span class="sxs-lookup"><span data-stu-id="f2200-192">The value is an [XML “Duration Data Type.”](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span></span> |  
|<span data-ttu-id="f2200-193">**Kataloger**</span><span class="sxs-lookup"><span data-stu-id="f2200-193">**Directories**</span></span>|<span data-ttu-id="f2200-194">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="f2200-194">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="f2200-195">**EtwProviders**</span><span class="sxs-lookup"><span data-stu-id="f2200-195">**EtwProviders**</span></span>|<span data-ttu-id="f2200-196">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="f2200-196">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="f2200-197">**Mått**</span><span class="sxs-lookup"><span data-stu-id="f2200-197">**Metrics**</span></span>|<span data-ttu-id="f2200-198">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="f2200-198">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="f2200-199">**PerformanceCounters**</span><span class="sxs-lookup"><span data-stu-id="f2200-199">**PerformanceCounters**</span></span>|<span data-ttu-id="f2200-200">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="f2200-200">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="f2200-201">**WindowsEventLog**</span><span class="sxs-lookup"><span data-stu-id="f2200-201">**WindowsEventLog**</span></span>|<span data-ttu-id="f2200-202">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="f2200-202">See description elsewhere on this page.</span></span>| 
|<span data-ttu-id="f2200-203">**DockerSources**</span><span class="sxs-lookup"><span data-stu-id="f2200-203">**DockerSources**</span></span>|<span data-ttu-id="f2200-204">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="f2200-204">See description elsewhere on this page.</span></span> | 



## <a name="crashdumps-element"></a><span data-ttu-id="f2200-205">CrashDumps Element</span><span class="sxs-lookup"><span data-stu-id="f2200-205">CrashDumps Element</span></span>  
 <span data-ttu-id="f2200-206">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - CrashDumps*</span><span class="sxs-lookup"><span data-stu-id="f2200-206">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - CrashDumps*</span></span>
 
 <span data-ttu-id="f2200-207">Aktivera insamling av krascher Dumpar.</span><span class="sxs-lookup"><span data-stu-id="f2200-207">Enable the collection of crash dumps.</span></span>  

|<span data-ttu-id="f2200-208">Attribut</span><span class="sxs-lookup"><span data-stu-id="f2200-208">Attributes</span></span>|<span data-ttu-id="f2200-209">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f2200-209">Description</span></span>|  
|----------------|-----------------|  
|<span data-ttu-id="f2200-210">**containerName**</span><span class="sxs-lookup"><span data-stu-id="f2200-210">**containerName**</span></span>|<span data-ttu-id="f2200-211">Valfri.</span><span class="sxs-lookup"><span data-stu-id="f2200-211">Optional.</span></span> <span data-ttu-id="f2200-212">Namnet på blob-behållaren i Azure Storage-konto som används för att lagra krascher Dumpar.</span><span class="sxs-lookup"><span data-stu-id="f2200-212">The name of the blob container in your Azure Storage account to be used to store crash dumps.</span></span>|  
|<span data-ttu-id="f2200-213">**crashDumpType**</span><span class="sxs-lookup"><span data-stu-id="f2200-213">**crashDumpType**</span></span>|<span data-ttu-id="f2200-214">Valfri.</span><span class="sxs-lookup"><span data-stu-id="f2200-214">Optional.</span></span>  <span data-ttu-id="f2200-215">Konfigurerar Azure-diagnostik för att samla in Dumpar mini eller fullständig kraschar.</span><span class="sxs-lookup"><span data-stu-id="f2200-215">Configures Azure Diagnostics to collect mini or full crash dumps.</span></span>|  
|<span data-ttu-id="f2200-216">**directoryQuotaPercentage**</span><span class="sxs-lookup"><span data-stu-id="f2200-216">**directoryQuotaPercentage**</span></span>|<span data-ttu-id="f2200-217">Valfri.</span><span class="sxs-lookup"><span data-stu-id="f2200-217">Optional.</span></span>  <span data-ttu-id="f2200-218">Konfigurerar procentandelen av **overallQuotaInMB** som ska reserveras för kraschdumpar på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f2200-218">Configures the percentage of **overallQuotaInMB** to be reserved for crash dumps on the VM.</span></span>|  

|<span data-ttu-id="f2200-219">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="f2200-219">Child Elements</span></span>|<span data-ttu-id="f2200-220">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f2200-220">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="f2200-221">**CrashDumpConfiguration**</span><span class="sxs-lookup"><span data-stu-id="f2200-221">**CrashDumpConfiguration**</span></span>|<span data-ttu-id="f2200-222">Krävs.</span><span class="sxs-lookup"><span data-stu-id="f2200-222">Required.</span></span> <span data-ttu-id="f2200-223">Definierar konfigurationsvärden för varje process.</span><span class="sxs-lookup"><span data-stu-id="f2200-223">Defines configuration values for each process.</span></span><br /><br /> <span data-ttu-id="f2200-224">Det krävs också följande attribut:</span><span class="sxs-lookup"><span data-stu-id="f2200-224">The following attribute is also required:</span></span><br /><br /> <span data-ttu-id="f2200-225">**Processnamn** -namnet på processen som du vill att Azure-diagnostik för att samla in en krasch-dump för.</span><span class="sxs-lookup"><span data-stu-id="f2200-225">**processName** - The name of the process you want Azure Diagnostics to collect a crash dump for.</span></span>|  

## <a name="directories-element"></a><span data-ttu-id="f2200-226">Kataloger Element</span><span class="sxs-lookup"><span data-stu-id="f2200-226">Directories Element</span></span> 
 <span data-ttu-id="f2200-227">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - kataloger*</span><span class="sxs-lookup"><span data-stu-id="f2200-227">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration -  Directories*</span></span>

 <span data-ttu-id="f2200-228">Aktiverar insamlingen av innehållet i en katalog, IIS kunde inte åtkomstloggar för begäran och/eller IIS-loggar.</span><span class="sxs-lookup"><span data-stu-id="f2200-228">Enables the collection of the contents of a directory, IIS failed access request logs and/or IIS logs.</span></span>  

 <span data-ttu-id="f2200-229">Valfria **scheduledTransferPeriod** attribut.</span><span class="sxs-lookup"><span data-stu-id="f2200-229">Optional **scheduledTransferPeriod** attribute.</span></span> <span data-ttu-id="f2200-230">Se tidigare förklaring.</span><span class="sxs-lookup"><span data-stu-id="f2200-230">See explanation earlier.</span></span>  

|<span data-ttu-id="f2200-231">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="f2200-231">Child Elements</span></span>|<span data-ttu-id="f2200-232">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f2200-232">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="f2200-233">**IISLogs**</span><span class="sxs-lookup"><span data-stu-id="f2200-233">**IISLogs**</span></span>|<span data-ttu-id="f2200-234">Inkludera det här elementet i konfigurationen aktiverar insamlingen av IIS-loggar:</span><span class="sxs-lookup"><span data-stu-id="f2200-234">Including this element in the configuration enables the collection of IIS logs:</span></span><br /><br /> <span data-ttu-id="f2200-235">**containerName** -namnet på blob-behållaren i Azure Storage-konto som används för att lagra IIS-loggarna.</span><span class="sxs-lookup"><span data-stu-id="f2200-235">**containerName** - The name of the blob container in your Azure Storage account to be used to store the IIS logs.</span></span>|   
|<span data-ttu-id="f2200-236">**FailedRequestLogs**</span><span class="sxs-lookup"><span data-stu-id="f2200-236">**FailedRequestLogs**</span></span>|<span data-ttu-id="f2200-237">Inklusive det här elementet i konfigurationen möjliggör insamling av loggar för misslyckade begäranden till en IIS-webbplats eller program.</span><span class="sxs-lookup"><span data-stu-id="f2200-237">Including this element in the configuration enables collection of logs about failed requests to an IIS site or application.</span></span> <span data-ttu-id="f2200-238">Du måste även aktivera spårningsalternativ under **system. Webbserver** i **Web.config**.</span><span class="sxs-lookup"><span data-stu-id="f2200-238">You must also enable tracing options under **system.WebServer** in **Web.config**.</span></span>|  
|<span data-ttu-id="f2200-239">**Datakällor**</span><span class="sxs-lookup"><span data-stu-id="f2200-239">**DataSources**</span></span>|<span data-ttu-id="f2200-240">En lista över kataloger om du vill övervaka.</span><span class="sxs-lookup"><span data-stu-id="f2200-240">A list of directories to monitor.</span></span>| 




## <a name="datasources-element"></a><span data-ttu-id="f2200-241">Datakällor Element</span><span class="sxs-lookup"><span data-stu-id="f2200-241">DataSources Element</span></span>  
 <span data-ttu-id="f2200-242">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - kataloger - datakällor*</span><span class="sxs-lookup"><span data-stu-id="f2200-242">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Directories - DataSources*</span></span>

 <span data-ttu-id="f2200-243">En lista över kataloger om du vill övervaka.</span><span class="sxs-lookup"><span data-stu-id="f2200-243">A list of directories to monitor.</span></span>  

|<span data-ttu-id="f2200-244">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="f2200-244">Child Elements</span></span>|<span data-ttu-id="f2200-245">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f2200-245">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="f2200-246">**DirectoryConfiguration**</span><span class="sxs-lookup"><span data-stu-id="f2200-246">**DirectoryConfiguration**</span></span>|<span data-ttu-id="f2200-247">Krävs.</span><span class="sxs-lookup"><span data-stu-id="f2200-247">Required.</span></span> <span data-ttu-id="f2200-248">Obligatoriskt attribut:</span><span class="sxs-lookup"><span data-stu-id="f2200-248">Required attribute:</span></span><br /><br /> <span data-ttu-id="f2200-249">**containerName** -namnet på blob-behållaren i Azure Storage-konto som används för att lagra loggfilerna.</span><span class="sxs-lookup"><span data-stu-id="f2200-249">**containerName** - The name of the blob container in your Azure Storage account that to be used to store the log files.</span></span>|  





## <a name="directoryconfiguration-element"></a><span data-ttu-id="f2200-250">DirectoryConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="f2200-250">DirectoryConfiguration Element</span></span>  
 <span data-ttu-id="f2200-251">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - kataloger - datakällor - DirectoryConfiguration*</span><span class="sxs-lookup"><span data-stu-id="f2200-251">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Directories - DataSources - DirectoryConfiguration*</span></span>

 <span data-ttu-id="f2200-252">Kan innehålla antingen den **absolut** eller **LocalResource** element, men inte båda.</span><span class="sxs-lookup"><span data-stu-id="f2200-252">May include either the **Absolute** or **LocalResource** element but not both.</span></span>  

|<span data-ttu-id="f2200-253">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="f2200-253">Child Elements</span></span>|<span data-ttu-id="f2200-254">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f2200-254">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="f2200-255">**Absolut**</span><span class="sxs-lookup"><span data-stu-id="f2200-255">**Absolute**</span></span>|<span data-ttu-id="f2200-256">Den absoluta sökvägen till katalogen som ska övervakas.</span><span class="sxs-lookup"><span data-stu-id="f2200-256">The absolute path to the directory to monitor.</span></span> <span data-ttu-id="f2200-257">Det krävs följande attribut:</span><span class="sxs-lookup"><span data-stu-id="f2200-257">The following attributes are required:</span></span><br /><br /> <span data-ttu-id="f2200-258">- **Sökvägen** -den absoluta sökvägen till katalogen som ska övervakas.</span><span class="sxs-lookup"><span data-stu-id="f2200-258">- **Path** - The absolute path to the directory to monitor.</span></span><br /><br /> <span data-ttu-id="f2200-259">- **expandEnvironment** -anger om miljövariabler i sökvägen expanderas.</span><span class="sxs-lookup"><span data-stu-id="f2200-259">- **expandEnvironment** - Configures whether environment variables in Path are expanded.</span></span>|  
|<span data-ttu-id="f2200-260">**LocalResource**</span><span class="sxs-lookup"><span data-stu-id="f2200-260">**LocalResource**</span></span>|<span data-ttu-id="f2200-261">Sökväg i förhållande till en lokal resurs för att övervaka.</span><span class="sxs-lookup"><span data-stu-id="f2200-261">The path relative to a local resource to monitor.</span></span> <span data-ttu-id="f2200-262">Obligatoriska attribut är:</span><span class="sxs-lookup"><span data-stu-id="f2200-262">Required attributes are:</span></span><br /><br /> <span data-ttu-id="f2200-263">- **Namnet** – den lokala resursen som innehåller katalogen som ska övervakas</span><span class="sxs-lookup"><span data-stu-id="f2200-263">- **Name** - The local resource that contains the directory to monitor</span></span><br /><br /> <span data-ttu-id="f2200-264">- **relativePath** -sökväg i förhållande till namn som innehåller katalogen som ska övervakas</span><span class="sxs-lookup"><span data-stu-id="f2200-264">- **relativePath** - The path relative to Name that contains the directory to monitor</span></span>|  



## <a name="etwproviders-element"></a><span data-ttu-id="f2200-265">EtwProviders Element</span><span class="sxs-lookup"><span data-stu-id="f2200-265">EtwProviders Element</span></span>  
 <span data-ttu-id="f2200-266">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders*</span><span class="sxs-lookup"><span data-stu-id="f2200-266">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders*</span></span>

 <span data-ttu-id="f2200-267">Konfigurerar samling ETW-händelser från EventSource och/eller ETW Manifest baserade providers.</span><span class="sxs-lookup"><span data-stu-id="f2200-267">Configures collection of ETW events from EventSource and/or ETW Manifest based providers.</span></span>  

|<span data-ttu-id="f2200-268">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="f2200-268">Child Elements</span></span>|<span data-ttu-id="f2200-269">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f2200-269">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="f2200-270">**EtwEventSourceProviderConfiguration**</span><span class="sxs-lookup"><span data-stu-id="f2200-270">**EtwEventSourceProviderConfiguration**</span></span>|<span data-ttu-id="f2200-271">Konfigurerar insamling av händelser som genereras från [EventSource klassen](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="f2200-271">Configures collection of events generated from [EventSource Class](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).</span></span> <span data-ttu-id="f2200-272">Obligatoriskt attribut:</span><span class="sxs-lookup"><span data-stu-id="f2200-272">Required attribute:</span></span><br /><br /> <span data-ttu-id="f2200-273">**Providern** -klassnamnet för händelsen EventSource.</span><span class="sxs-lookup"><span data-stu-id="f2200-273">**provider** - The class name of the EventSource event.</span></span><br /><br /> <span data-ttu-id="f2200-274">Valfria attribut är:</span><span class="sxs-lookup"><span data-stu-id="f2200-274">Optional attributes are:</span></span><br /><br /> <span data-ttu-id="f2200-275">- **scheduledTransferLogLevelFilter** -lägsta allvarlighetsgrad att överföra till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="f2200-275">- **scheduledTransferLogLevelFilter** - The minimum severity level to transfer to your storage account.</span></span><br /><br /> <span data-ttu-id="f2200-276">- **scheduledTransferPeriod** -intervallet mellan schemalagda överföringar till lagring avrundat uppåt till närmaste minut.</span><span class="sxs-lookup"><span data-stu-id="f2200-276">- **scheduledTransferPeriod** - The interval between scheduled transfers to storage rounded up to the nearest minute.</span></span> <span data-ttu-id="f2200-277">Värdet är en [XML ”varaktighet datatyp”.](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span><span class="sxs-lookup"><span data-stu-id="f2200-277">The value is an [XML “Duration Data Type.”](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span></span> |  
|<span data-ttu-id="f2200-278">**EtwManifestProviderConfiguration**</span><span class="sxs-lookup"><span data-stu-id="f2200-278">**EtwManifestProviderConfiguration**</span></span>|<span data-ttu-id="f2200-279">Obligatoriskt attribut:</span><span class="sxs-lookup"><span data-stu-id="f2200-279">Required attribute:</span></span><br /><br /> <span data-ttu-id="f2200-280">**Providern** -GUID av Händelseprovidern</span><span class="sxs-lookup"><span data-stu-id="f2200-280">**provider** - The GUID of the event provider</span></span><br /><br /> <span data-ttu-id="f2200-281">Valfria attribut är:</span><span class="sxs-lookup"><span data-stu-id="f2200-281">Optional attributes are:</span></span><br /><br /> <span data-ttu-id="f2200-282">- **scheduledTransferLogLevelFilter** -lägsta allvarlighetsgrad att överföra till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="f2200-282">- **scheduledTransferLogLevelFilter** - The minimum severity level to transfer to your storage account.</span></span><br /><br /> <span data-ttu-id="f2200-283">- **scheduledTransferPeriod** -intervallet mellan schemalagda överföringar till lagring avrundat uppåt till närmaste minut.</span><span class="sxs-lookup"><span data-stu-id="f2200-283">- **scheduledTransferPeriod** - The interval between scheduled transfers to storage rounded up to the nearest minute.</span></span> <span data-ttu-id="f2200-284">Värdet är en [XML ”varaktighet datatyp”.](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span><span class="sxs-lookup"><span data-stu-id="f2200-284">The value is an [XML “Duration Data Type.”](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span></span> |  



## <a name="etweventsourceproviderconfiguration-element"></a><span data-ttu-id="f2200-285">EtwEventSourceProviderConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="f2200-285">EtwEventSourceProviderConfiguration Element</span></span>  
 <span data-ttu-id="f2200-286">*: Trädrot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwEventSourceProviderConfiguration*</span><span class="sxs-lookup"><span data-stu-id="f2200-286">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders- EtwEventSourceProviderConfiguration*</span></span>

 <span data-ttu-id="f2200-287">Konfigurerar insamling av händelser som genereras från [EventSource klassen](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="f2200-287">Configures collection of events generated from [EventSource Class](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).</span></span>  

|<span data-ttu-id="f2200-288">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="f2200-288">Child Elements</span></span>|<span data-ttu-id="f2200-289">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f2200-289">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="f2200-290">**DefaultEvents**</span><span class="sxs-lookup"><span data-stu-id="f2200-290">**DefaultEvents**</span></span>|<span data-ttu-id="f2200-291">Valfria attribut:</span><span class="sxs-lookup"><span data-stu-id="f2200-291">Optional attribute:</span></span><br/><br/> <span data-ttu-id="f2200-292">**eventDestination** -namnet på tabellen för att lagra händelser i</span><span class="sxs-lookup"><span data-stu-id="f2200-292">**eventDestination** - The name of the table to store the events in</span></span>|  
|<span data-ttu-id="f2200-293">**Händelse**</span><span class="sxs-lookup"><span data-stu-id="f2200-293">**Event**</span></span>|<span data-ttu-id="f2200-294">Obligatoriskt attribut:</span><span class="sxs-lookup"><span data-stu-id="f2200-294">Required attribute:</span></span><br /><br /> <span data-ttu-id="f2200-295">**ID** -id för händelsen.</span><span class="sxs-lookup"><span data-stu-id="f2200-295">**id** - The id of the event.</span></span><br /><br /> <span data-ttu-id="f2200-296">Valfria attribut:</span><span class="sxs-lookup"><span data-stu-id="f2200-296">Optional attribute:</span></span><br /><br /> <span data-ttu-id="f2200-297">**eventDestination** -namnet på tabellen för att lagra händelser i</span><span class="sxs-lookup"><span data-stu-id="f2200-297">**eventDestination** - The name of the table to store the events in</span></span>|  



## <a name="etwmanifestproviderconfiguration-element"></a><span data-ttu-id="f2200-298">EtwManifestProviderConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="f2200-298">EtwManifestProviderConfiguration Element</span></span>  
 <span data-ttu-id="f2200-299">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwManifestProviderConfiguration*</span><span class="sxs-lookup"><span data-stu-id="f2200-299">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwManifestProviderConfiguration*</span></span>

|<span data-ttu-id="f2200-300">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="f2200-300">Child Elements</span></span>|<span data-ttu-id="f2200-301">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f2200-301">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="f2200-302">**DefaultEvents**</span><span class="sxs-lookup"><span data-stu-id="f2200-302">**DefaultEvents**</span></span>|<span data-ttu-id="f2200-303">Valfria attribut:</span><span class="sxs-lookup"><span data-stu-id="f2200-303">Optional attribute:</span></span><br /><br /> <span data-ttu-id="f2200-304">**eventDestination** -namnet på tabellen för att lagra händelser i</span><span class="sxs-lookup"><span data-stu-id="f2200-304">**eventDestination** - The name of the table to store the events in</span></span>|  
|<span data-ttu-id="f2200-305">**Händelse**</span><span class="sxs-lookup"><span data-stu-id="f2200-305">**Event**</span></span>|<span data-ttu-id="f2200-306">Obligatoriskt attribut:</span><span class="sxs-lookup"><span data-stu-id="f2200-306">Required attribute:</span></span><br /><br /> <span data-ttu-id="f2200-307">**ID** -id för händelsen.</span><span class="sxs-lookup"><span data-stu-id="f2200-307">**id** - The id of the event.</span></span><br /><br /> <span data-ttu-id="f2200-308">Valfria attribut:</span><span class="sxs-lookup"><span data-stu-id="f2200-308">Optional attribute:</span></span><br /><br /> <span data-ttu-id="f2200-309">**eventDestination** -namnet på tabellen för att lagra händelser i</span><span class="sxs-lookup"><span data-stu-id="f2200-309">**eventDestination** - The name of the table to store the events in</span></span>|  



## <a name="metrics-element"></a><span data-ttu-id="f2200-310">Mått Element</span><span class="sxs-lookup"><span data-stu-id="f2200-310">Metrics Element</span></span>  
 <span data-ttu-id="f2200-311">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - mått*</span><span class="sxs-lookup"><span data-stu-id="f2200-311">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Metrics*</span></span>

 <span data-ttu-id="f2200-312">Kan du generera en prestandaräknaren tabell som är optimerad för snabb frågor.</span><span class="sxs-lookup"><span data-stu-id="f2200-312">Enables you to generate a performance counter table that is optimized for fast queries.</span></span> <span data-ttu-id="f2200-313">Varje prestandaräknare som definieras i den **PerformanceCounters** element lagras i tabellen mått förutom tabellen prestandaräknaren.</span><span class="sxs-lookup"><span data-stu-id="f2200-313">Each performance counter that is defined in the **PerformanceCounters** element is stored in the Metrics table in addition to the Performance Counter table.</span></span>  

 <span data-ttu-id="f2200-314">Den **resourceId** -attribut krävs.</span><span class="sxs-lookup"><span data-stu-id="f2200-314">The **resourceId** attribute is required.</span></span>  <span data-ttu-id="f2200-315">Resurs-ID för den virtuella datorn som du distribuerar till Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="f2200-315">The resource ID of the Virtual Machine you are deploying Azure Diagnostics to.</span></span> <span data-ttu-id="f2200-316">Hämta den **resourceID** från den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f2200-316">Get the **resourceID** from the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="f2200-317">Välj **Bläddra** -> **resursgrupper** -> **< namn\>**.</span><span class="sxs-lookup"><span data-stu-id="f2200-317">Select **Browse** -> **Resource Groups** -> **<Name\>**.</span></span> <span data-ttu-id="f2200-318">Klicka på den **egenskaper** panelen och kopiera värdet från den **ID** fältet.</span><span class="sxs-lookup"><span data-stu-id="f2200-318">Click the **Properties** tile and copy the value from the **ID** field.</span></span>  

|<span data-ttu-id="f2200-319">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="f2200-319">Child Elements</span></span>|<span data-ttu-id="f2200-320">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f2200-320">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="f2200-321">**MetricAggregation**</span><span class="sxs-lookup"><span data-stu-id="f2200-321">**MetricAggregation**</span></span>|<span data-ttu-id="f2200-322">Obligatoriskt attribut:</span><span class="sxs-lookup"><span data-stu-id="f2200-322">Required attribute:</span></span><br /><br /> <span data-ttu-id="f2200-323">**scheduledTransferPeriod** -intervallet mellan schemalagda överföringar till lagring avrundat uppåt till närmaste minut.</span><span class="sxs-lookup"><span data-stu-id="f2200-323">**scheduledTransferPeriod** - The interval between scheduled transfers to storage rounded up to the nearest minute.</span></span> <span data-ttu-id="f2200-324">Värdet är en [XML ”varaktighet datatyp”.](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span><span class="sxs-lookup"><span data-stu-id="f2200-324">The value is an [XML “Duration Data Type.”](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span></span> |  



## <a name="performancecounters-element"></a><span data-ttu-id="f2200-325">PerformanceCounters elementet</span><span class="sxs-lookup"><span data-stu-id="f2200-325">PerformanceCounters Element</span></span>  
 <span data-ttu-id="f2200-326">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - prestandaräknarna*</span><span class="sxs-lookup"><span data-stu-id="f2200-326">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - PerformanceCounters*</span></span>

 <span data-ttu-id="f2200-327">Aktiverar insamlingen av prestandaräknare.</span><span class="sxs-lookup"><span data-stu-id="f2200-327">Enables the collection of performance counters.</span></span>  

 <span data-ttu-id="f2200-328">Valfria attribut:</span><span class="sxs-lookup"><span data-stu-id="f2200-328">Optional attribute:</span></span>  

 <span data-ttu-id="f2200-329">Valfria **scheduledTransferPeriod** attribut.</span><span class="sxs-lookup"><span data-stu-id="f2200-329">Optional **scheduledTransferPeriod** attribute.</span></span> <span data-ttu-id="f2200-330">Se tidigare förklaring.</span><span class="sxs-lookup"><span data-stu-id="f2200-330">See explanation earlier.</span></span>

|<span data-ttu-id="f2200-331">Underordnat Element</span><span class="sxs-lookup"><span data-stu-id="f2200-331">Child Element</span></span>|<span data-ttu-id="f2200-332">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f2200-332">Description</span></span>|  
|-------------------|-----------------|  
|<span data-ttu-id="f2200-333">**PerformanceCounterConfiguration**</span><span class="sxs-lookup"><span data-stu-id="f2200-333">**PerformanceCounterConfiguration**</span></span>|<span data-ttu-id="f2200-334">Det krävs följande attribut:</span><span class="sxs-lookup"><span data-stu-id="f2200-334">The following attributes are required:</span></span><br /><br /> <span data-ttu-id="f2200-335">- **counterSpecifier** -namnet på prestandaräknaren.</span><span class="sxs-lookup"><span data-stu-id="f2200-335">- **counterSpecifier** - The name of the performance counter.</span></span> <span data-ttu-id="f2200-336">Till exempel `\Processor(_Total)\% Processor Time`.</span><span class="sxs-lookup"><span data-stu-id="f2200-336">For example, `\Processor(_Total)\% Processor Time`.</span></span> <span data-ttu-id="f2200-337">Om du vill hämta en lista över prestandaräknare på värddatorn, kör du kommandot `typeperf`.</span><span class="sxs-lookup"><span data-stu-id="f2200-337">To get a list of performance counters on your host, run the command `typeperf`.</span></span><br /><br /> <span data-ttu-id="f2200-338">- **sampleRate** -hur ofta räknaren ska samlas in.</span><span class="sxs-lookup"><span data-stu-id="f2200-338">- **sampleRate** - How often the counter should be sampled.</span></span><br /><br /> <span data-ttu-id="f2200-339">Valfria attribut:</span><span class="sxs-lookup"><span data-stu-id="f2200-339">Optional attribute:</span></span><br /><br /> <span data-ttu-id="f2200-340">**enhet** -enheten för räknaren.</span><span class="sxs-lookup"><span data-stu-id="f2200-340">**unit** - The unit of measure of the counter.</span></span>|  




## <a name="windowseventlog-element"></a><span data-ttu-id="f2200-341">WindowsEventLog Element</span><span class="sxs-lookup"><span data-stu-id="f2200-341">WindowsEventLog Element</span></span>
 <span data-ttu-id="f2200-342">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - WindowsEventLog*</span><span class="sxs-lookup"><span data-stu-id="f2200-342">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - WindowsEventLog*</span></span>
 
 <span data-ttu-id="f2200-343">Aktiverar insamlingen av Windows-händelseloggar.</span><span class="sxs-lookup"><span data-stu-id="f2200-343">Enables the collection of Windows Event Logs.</span></span>  

 <span data-ttu-id="f2200-344">Valfria **scheduledTransferPeriod** attribut.</span><span class="sxs-lookup"><span data-stu-id="f2200-344">Optional **scheduledTransferPeriod** attribute.</span></span> <span data-ttu-id="f2200-345">Se tidigare förklaring.</span><span class="sxs-lookup"><span data-stu-id="f2200-345">See explanation  earlier.</span></span>  

|<span data-ttu-id="f2200-346">Underordnat Element</span><span class="sxs-lookup"><span data-stu-id="f2200-346">Child Element</span></span>|<span data-ttu-id="f2200-347">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f2200-347">Description</span></span>|  
|-------------------|-----------------|  
|<span data-ttu-id="f2200-348">**DataSource**</span><span class="sxs-lookup"><span data-stu-id="f2200-348">**DataSource**</span></span>|<span data-ttu-id="f2200-349">Windows-händelseloggar att samla in.</span><span class="sxs-lookup"><span data-stu-id="f2200-349">The Windows Event logs to collect.</span></span> <span data-ttu-id="f2200-350">Obligatoriskt attribut:</span><span class="sxs-lookup"><span data-stu-id="f2200-350">Required attribute:</span></span><br /><br /> <span data-ttu-id="f2200-351">**namnet** - XPath-frågan som beskriver de windows-händelserna som ska hämtas.</span><span class="sxs-lookup"><span data-stu-id="f2200-351">**name** - The XPath query describing the windows events to be collected.</span></span> <span data-ttu-id="f2200-352">Exempel:</span><span class="sxs-lookup"><span data-stu-id="f2200-352">For example:</span></span><br /><br /> `Application!*[System[(Level <=3)]], System!*[System[(Level <=3)]], System!*[System[Provider[@Name='Microsoft Antimalware']]], Security!*[System[(Level <= 3)]`<br /><br /> <span data-ttu-id="f2200-353">Om du vill samla in alla händelser, ange ”*”</span><span class="sxs-lookup"><span data-stu-id="f2200-353">To collect all events, specify "*"</span></span>|  




## <a name="logs-element"></a><span data-ttu-id="f2200-354">Loggar Element</span><span class="sxs-lookup"><span data-stu-id="f2200-354">Logs Element</span></span>  
 <span data-ttu-id="f2200-355">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - loggar*</span><span class="sxs-lookup"><span data-stu-id="f2200-355">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Logs*</span></span>

 <span data-ttu-id="f2200-356">Ge i version 1.0- och 1.1.</span><span class="sxs-lookup"><span data-stu-id="f2200-356">Present in version 1.0 and 1.1.</span></span> <span data-ttu-id="f2200-357">Saknas i 1.2.</span><span class="sxs-lookup"><span data-stu-id="f2200-357">Missing in 1.2.</span></span> <span data-ttu-id="f2200-358">Lägga till i 1.3.</span><span class="sxs-lookup"><span data-stu-id="f2200-358">Added back in 1.3.</span></span>  

 <span data-ttu-id="f2200-359">Definierar konfigurationen buffert för grundläggande Azure loggar.</span><span class="sxs-lookup"><span data-stu-id="f2200-359">Defines the buffer configuration for basic Azure logs.</span></span>  

|<span data-ttu-id="f2200-360">Attribut</span><span class="sxs-lookup"><span data-stu-id="f2200-360">Attribute</span></span>|<span data-ttu-id="f2200-361">Typ</span><span class="sxs-lookup"><span data-stu-id="f2200-361">Type</span></span>|<span data-ttu-id="f2200-362">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f2200-362">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="f2200-363">**bufferQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="f2200-363">**bufferQuotaInMB**</span></span>|<span data-ttu-id="f2200-364">**unsignedInt**</span><span class="sxs-lookup"><span data-stu-id="f2200-364">**unsignedInt**</span></span>|<span data-ttu-id="f2200-365">Valfri.</span><span class="sxs-lookup"><span data-stu-id="f2200-365">Optional.</span></span> <span data-ttu-id="f2200-366">Anger maximal mängd fillagring för system som är tillgängliga för angivna data.</span><span class="sxs-lookup"><span data-stu-id="f2200-366">Specifies the maximum amount of file system storage that is available for the specified data.</span></span><br /><br /> <span data-ttu-id="f2200-367">Standardvärdet är 0.</span><span class="sxs-lookup"><span data-stu-id="f2200-367">The default is 0.</span></span>|  
|<span data-ttu-id="f2200-368">**scheduledTransferLogLevelFilterr**</span><span class="sxs-lookup"><span data-stu-id="f2200-368">**scheduledTransferLogLevelFilterr**</span></span>|<span data-ttu-id="f2200-369">**sträng**</span><span class="sxs-lookup"><span data-stu-id="f2200-369">**string**</span></span>|<span data-ttu-id="f2200-370">Valfri.</span><span class="sxs-lookup"><span data-stu-id="f2200-370">Optional.</span></span> <span data-ttu-id="f2200-371">Anger den lägsta allvarlighetsgraden för loggposter som överförs.</span><span class="sxs-lookup"><span data-stu-id="f2200-371">Specifies the minimum severity level for log entries that are transferred.</span></span> <span data-ttu-id="f2200-372">Standardvärdet är **Undefined**, som överför alla loggar.</span><span class="sxs-lookup"><span data-stu-id="f2200-372">The default value is **Undefined**, which transfers all logs.</span></span> <span data-ttu-id="f2200-373">Andra möjliga värden (i ordningen för de flesta till minst information) är **utförlig**, **Information**, **varning**, **fel**, och **Kritiska**.</span><span class="sxs-lookup"><span data-stu-id="f2200-373">Other possible values (in order of most to least information) are **Verbose**, **Information**, **Warning**, **Error**, and **Critical**.</span></span>|  
|<span data-ttu-id="f2200-374">**scheduledTransferPeriod**</span><span class="sxs-lookup"><span data-stu-id="f2200-374">**scheduledTransferPeriod**</span></span>|<span data-ttu-id="f2200-375">**varaktighet**</span><span class="sxs-lookup"><span data-stu-id="f2200-375">**duration**</span></span>|<span data-ttu-id="f2200-376">Valfri.</span><span class="sxs-lookup"><span data-stu-id="f2200-376">Optional.</span></span> <span data-ttu-id="f2200-377">Anger intervallet mellan schemalagda dataöverföringar, avrundat till närmaste minut.</span><span class="sxs-lookup"><span data-stu-id="f2200-377">Specifies the interval between scheduled transfers of data, rounded up to the nearest minute.</span></span><br /><br /> <span data-ttu-id="f2200-378">Standardvärdet är PT0S.</span><span class="sxs-lookup"><span data-stu-id="f2200-378">The default is PT0S.</span></span>|  
|<span data-ttu-id="f2200-379">**egenskaperna** lades till i 1.5</span><span class="sxs-lookup"><span data-stu-id="f2200-379">**sinks** Added in 1.5</span></span>|<span data-ttu-id="f2200-380">**sträng**</span><span class="sxs-lookup"><span data-stu-id="f2200-380">**string**</span></span>|<span data-ttu-id="f2200-381">Valfri.</span><span class="sxs-lookup"><span data-stu-id="f2200-381">Optional.</span></span> <span data-ttu-id="f2200-382">Pekar på en plats för mottagaren att också skicka diagnostikdata.</span><span class="sxs-lookup"><span data-stu-id="f2200-382">Points to a sink location to also send diagnostic data.</span></span> <span data-ttu-id="f2200-383">Till exempel Application Insights.</span><span class="sxs-lookup"><span data-stu-id="f2200-383">For example, Application Insights.</span></span>|  

## <a name="dockersources"></a><span data-ttu-id="f2200-384">DockerSources</span><span class="sxs-lookup"><span data-stu-id="f2200-384">DockerSources</span></span>
 <span data-ttu-id="f2200-385">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - DockerSources*</span><span class="sxs-lookup"><span data-stu-id="f2200-385">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - DockerSources*</span></span>

 <span data-ttu-id="f2200-386">Lägga till i 1.9.</span><span class="sxs-lookup"><span data-stu-id="f2200-386">Added in 1.9.</span></span>

|<span data-ttu-id="f2200-387">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="f2200-387">Element Name</span></span>|<span data-ttu-id="f2200-388">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f2200-388">Description</span></span>|  
|------------------|-----------------|  
|<span data-ttu-id="f2200-389">**Statistik**</span><span class="sxs-lookup"><span data-stu-id="f2200-389">**Stats**</span></span>|<span data-ttu-id="f2200-390">Anger att samla in statistik för Docker-behållare</span><span class="sxs-lookup"><span data-stu-id="f2200-390">Tells the system to collect stats for Docker containers</span></span>|  

## <a name="sinksconfig-element"></a><span data-ttu-id="f2200-391">SinksConfig Element</span><span class="sxs-lookup"><span data-stu-id="f2200-391">SinksConfig Element</span></span>  
 <span data-ttu-id="f2200-392">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig*</span><span class="sxs-lookup"><span data-stu-id="f2200-392">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig*</span></span>

 <span data-ttu-id="f2200-393">En lista över platser för att skicka diagnostikdata till och den konfiguration som är kopplade till dessa platser.</span><span class="sxs-lookup"><span data-stu-id="f2200-393">A list of locations to send diagnostics data to and the configuration associated with those locations.</span></span>  

|<span data-ttu-id="f2200-394">Elementnamn</span><span class="sxs-lookup"><span data-stu-id="f2200-394">Element Name</span></span>|<span data-ttu-id="f2200-395">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f2200-395">Description</span></span>|  
|------------------|-----------------|  
|<span data-ttu-id="f2200-396">**Sink**</span><span class="sxs-lookup"><span data-stu-id="f2200-396">**Sink**</span></span>|<span data-ttu-id="f2200-397">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="f2200-397">See description elsewhere on this page.</span></span>|  

## <a name="sink-element"></a><span data-ttu-id="f2200-398">Sink Element</span><span class="sxs-lookup"><span data-stu-id="f2200-398">Sink Element</span></span>
 <span data-ttu-id="f2200-399">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - mottagare*</span><span class="sxs-lookup"><span data-stu-id="f2200-399">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink*</span></span>

 <span data-ttu-id="f2200-400">Lägga till i version 1.5.</span><span class="sxs-lookup"><span data-stu-id="f2200-400">Added in version 1.5.</span></span>  

 <span data-ttu-id="f2200-401">Definierar platser för att skicka diagnostikdata till.</span><span class="sxs-lookup"><span data-stu-id="f2200-401">Defines locations to send diagnostic data to.</span></span> <span data-ttu-id="f2200-402">Till exempel Application Insights-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f2200-402">For example, the Application Insights service.</span></span>  

|<span data-ttu-id="f2200-403">Attribut</span><span class="sxs-lookup"><span data-stu-id="f2200-403">Attribute</span></span>|<span data-ttu-id="f2200-404">Typ</span><span class="sxs-lookup"><span data-stu-id="f2200-404">Type</span></span>|<span data-ttu-id="f2200-405">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f2200-405">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="f2200-406">**Namn**</span><span class="sxs-lookup"><span data-stu-id="f2200-406">**name**</span></span>|<span data-ttu-id="f2200-407">Sträng</span><span class="sxs-lookup"><span data-stu-id="f2200-407">string</span></span>|<span data-ttu-id="f2200-408">En sträng som identifierar sinkname.</span><span class="sxs-lookup"><span data-stu-id="f2200-408">A string identifying the sinkname.</span></span>|  

|<span data-ttu-id="f2200-409">Element</span><span class="sxs-lookup"><span data-stu-id="f2200-409">Element</span></span>|<span data-ttu-id="f2200-410">Typ</span><span class="sxs-lookup"><span data-stu-id="f2200-410">Type</span></span>|<span data-ttu-id="f2200-411">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f2200-411">Description</span></span>|  
|-------------|----------|-----------------|  
|<span data-ttu-id="f2200-412">**Application Insights**</span><span class="sxs-lookup"><span data-stu-id="f2200-412">**Application Insights**</span></span>|<span data-ttu-id="f2200-413">Sträng</span><span class="sxs-lookup"><span data-stu-id="f2200-413">string</span></span>|<span data-ttu-id="f2200-414">Används endast när data skickades till Application Insights.</span><span class="sxs-lookup"><span data-stu-id="f2200-414">Used only when sending data to Application Insights.</span></span> <span data-ttu-id="f2200-415">Innehåller den Instrumentation nyckeln för ett aktivt Application Insights-konto som du har åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="f2200-415">Contain the Instrumentation Key for an active Application Insights account that you have access to.</span></span>|  
|<span data-ttu-id="f2200-416">**Kanaler**</span><span class="sxs-lookup"><span data-stu-id="f2200-416">**Channels**</span></span>|<span data-ttu-id="f2200-417">Sträng</span><span class="sxs-lookup"><span data-stu-id="f2200-417">string</span></span>|<span data-ttu-id="f2200-418">En för varje ytterligare filtrering strömma som du</span><span class="sxs-lookup"><span data-stu-id="f2200-418">One for each additional filtering that stream that you</span></span>|  

## <a name="channels-element"></a><span data-ttu-id="f2200-419">Kanaler Element</span><span class="sxs-lookup"><span data-stu-id="f2200-419">Channels Element</span></span>  
 <span data-ttu-id="f2200-420">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink - kanaler*</span><span class="sxs-lookup"><span data-stu-id="f2200-420">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink - Channels*</span></span>

 <span data-ttu-id="f2200-421">Lägga till i version 1.5.</span><span class="sxs-lookup"><span data-stu-id="f2200-421">Added in version 1.5.</span></span>  

 <span data-ttu-id="f2200-422">Definierar filter för dataströmmar logg går genom en mottagare.</span><span class="sxs-lookup"><span data-stu-id="f2200-422">Defines filters for streams of log data passing through a sink.</span></span>  

|<span data-ttu-id="f2200-423">Element</span><span class="sxs-lookup"><span data-stu-id="f2200-423">Element</span></span>|<span data-ttu-id="f2200-424">Typ</span><span class="sxs-lookup"><span data-stu-id="f2200-424">Type</span></span>|<span data-ttu-id="f2200-425">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f2200-425">Description</span></span>|  
|-------------|----------|-----------------|  
|<span data-ttu-id="f2200-426">**Kanal**</span><span class="sxs-lookup"><span data-stu-id="f2200-426">**Channel**</span></span>|<span data-ttu-id="f2200-427">Sträng</span><span class="sxs-lookup"><span data-stu-id="f2200-427">string</span></span>|<span data-ttu-id="f2200-428">Se beskrivning på annan plats på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="f2200-428">See description elsewhere on this page.</span></span>|  

## <a name="channel-element"></a><span data-ttu-id="f2200-429">Kanal Element</span><span class="sxs-lookup"><span data-stu-id="f2200-429">Channel Element</span></span>
 <span data-ttu-id="f2200-430">*Trädet: Rot - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink - kanaler - kanal*</span><span class="sxs-lookup"><span data-stu-id="f2200-430">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink - Channels - Channel*</span></span>

 <span data-ttu-id="f2200-431">Lägga till i version 1.5.</span><span class="sxs-lookup"><span data-stu-id="f2200-431">Added in version 1.5.</span></span>  

 <span data-ttu-id="f2200-432">Definierar platser för att skicka diagnostikdata till.</span><span class="sxs-lookup"><span data-stu-id="f2200-432">Defines locations to send diagnostic data to.</span></span> <span data-ttu-id="f2200-433">Till exempel Application Insights-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f2200-433">For example, the Application Insights service.</span></span>  

|<span data-ttu-id="f2200-434">Attribut</span><span class="sxs-lookup"><span data-stu-id="f2200-434">Attributes</span></span>|<span data-ttu-id="f2200-435">Typ</span><span class="sxs-lookup"><span data-stu-id="f2200-435">Type</span></span>|<span data-ttu-id="f2200-436">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f2200-436">Description</span></span>|  
|----------------|----------|-----------------|  
|<span data-ttu-id="f2200-437">**logLevel**</span><span class="sxs-lookup"><span data-stu-id="f2200-437">**logLevel**</span></span>|<span data-ttu-id="f2200-438">**sträng**</span><span class="sxs-lookup"><span data-stu-id="f2200-438">**string**</span></span>|<span data-ttu-id="f2200-439">Anger den lägsta allvarlighetsgraden för loggposter som överförs.</span><span class="sxs-lookup"><span data-stu-id="f2200-439">Specifies the minimum severity level for log entries that are transferred.</span></span> <span data-ttu-id="f2200-440">Standardvärdet är **Undefined**, som överför alla loggar.</span><span class="sxs-lookup"><span data-stu-id="f2200-440">The default value is **Undefined**, which transfers all logs.</span></span> <span data-ttu-id="f2200-441">Andra möjliga värden (i ordningen för de flesta till minst information) är **utförlig**, **Information**, **varning**, **fel**, och **Kritiska**.</span><span class="sxs-lookup"><span data-stu-id="f2200-441">Other possible values (in order of most to least information) are **Verbose**, **Information**, **Warning**, **Error**, and **Critical**.</span></span>|  
|<span data-ttu-id="f2200-442">**Namn**</span><span class="sxs-lookup"><span data-stu-id="f2200-442">**name**</span></span>|<span data-ttu-id="f2200-443">**sträng**</span><span class="sxs-lookup"><span data-stu-id="f2200-443">**string**</span></span>|<span data-ttu-id="f2200-444">Ett unikt namn för att referera till kanalen</span><span class="sxs-lookup"><span data-stu-id="f2200-444">A unique name of the channel to refer to</span></span>|  


## <a name="privateconfig-element"></a><span data-ttu-id="f2200-445">PrivateConfig Element</span><span class="sxs-lookup"><span data-stu-id="f2200-445">PrivateConfig Element</span></span> 
 <span data-ttu-id="f2200-446">*Trädet: Rot - DiagnosticsConfiguration - PrivateConfig*</span><span class="sxs-lookup"><span data-stu-id="f2200-446">*Tree: Root - DiagnosticsConfiguration - PrivateConfig*</span></span>

 <span data-ttu-id="f2200-447">Lägga till i version 1.3.</span><span class="sxs-lookup"><span data-stu-id="f2200-447">Added in version 1.3.</span></span>  

 <span data-ttu-id="f2200-448">Valfri</span><span class="sxs-lookup"><span data-stu-id="f2200-448">Optional</span></span>  

 <span data-ttu-id="f2200-449">Lagrar privat information om lagringskontot (namn, nyckel och slutpunkten).</span><span class="sxs-lookup"><span data-stu-id="f2200-449">Stores the private details of the storage account (name, key, and endpoint).</span></span> <span data-ttu-id="f2200-450">Den här informationen skickas till den virtuella datorn, men kan inte hämtas från den.</span><span class="sxs-lookup"><span data-stu-id="f2200-450">This information is sent to the virtual machine, but cannot be retrieved from it.</span></span>  

|<span data-ttu-id="f2200-451">Underordnade element</span><span class="sxs-lookup"><span data-stu-id="f2200-451">Child Elements</span></span>|<span data-ttu-id="f2200-452">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f2200-452">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="f2200-453">**StorageAccount**</span><span class="sxs-lookup"><span data-stu-id="f2200-453">**StorageAccount**</span></span>|<span data-ttu-id="f2200-454">Lagringskontot för att använda.</span><span class="sxs-lookup"><span data-stu-id="f2200-454">The storage account to use.</span></span> <span data-ttu-id="f2200-455">Följande attribut krävs</span><span class="sxs-lookup"><span data-stu-id="f2200-455">The following attributes are required</span></span><br /><br /> <span data-ttu-id="f2200-456">- **namnet** -namnet på lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="f2200-456">- **name** - The name of the storage account.</span></span><br /><br /> <span data-ttu-id="f2200-457">- **nyckeln** -nyckeln till lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="f2200-457">- **key** - The key to the storage account.</span></span><br /><br /> <span data-ttu-id="f2200-458">- **slutpunkten** -slutpunkter för åtkomst till lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="f2200-458">- **endpoint** - The endpoint to access the storage account.</span></span> <br /><br /> <span data-ttu-id="f2200-459">-**sasToken** (läggs till 1.8.1)-som du kan ange en SAS-token i stället för en lagringskontonyckel i privata konfig.</span><span class="sxs-lookup"><span data-stu-id="f2200-459">-**sasToken** (added 1.8.1)- You can specify an SAS token instead of a storage account key in the private config.</span></span> <span data-ttu-id="f2200-460">Om anges ignoreras lagringskontots åtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="f2200-460">If provided, the storage account key is ignored.</span></span> <br /><span data-ttu-id="f2200-461">Krav för SAS-Token:</span><span class="sxs-lookup"><span data-stu-id="f2200-461">Requirements for the SAS Token:</span></span> <br /><span data-ttu-id="f2200-462">– Stöder endast konto SAS-token</span><span class="sxs-lookup"><span data-stu-id="f2200-462">- Supports account SAS token only</span></span> <br /><span data-ttu-id="f2200-463">- *b*, *t* tjänsttyper krävs.</span><span class="sxs-lookup"><span data-stu-id="f2200-463">- *b*, *t* service types are required.</span></span> <br /> <span data-ttu-id="f2200-464">- *en*, *c*, *u*, *w* behörigheter som krävs.</span><span class="sxs-lookup"><span data-stu-id="f2200-464">- *a*, *c*, *u*, *w* permissions are required.</span></span> <br /> <span data-ttu-id="f2200-465">- *c*, *o* resurstyper krävs.</span><span class="sxs-lookup"><span data-stu-id="f2200-465">- *c*, *o* resource types are required.</span></span> <br /> <span data-ttu-id="f2200-466">– Stöder HTTPS-protokollet</span><span class="sxs-lookup"><span data-stu-id="f2200-466">- Supports the HTTPS protocol only</span></span> <br /> <span data-ttu-id="f2200-467">-Start och förfallotiden måste vara giltigt.</span><span class="sxs-lookup"><span data-stu-id="f2200-467">- Start and expiry time must be valid.</span></span>|  


## <a name="isenabled-element"></a><span data-ttu-id="f2200-468">IsEnabled Element</span><span class="sxs-lookup"><span data-stu-id="f2200-468">IsEnabled Element</span></span>  
 <span data-ttu-id="f2200-469">*Trädet: Rot - DiagnosticsConfiguration - IsEnabled*</span><span class="sxs-lookup"><span data-stu-id="f2200-469">*Tree: Root - DiagnosticsConfiguration - IsEnabled*</span></span>

 <span data-ttu-id="f2200-470">Booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="f2200-470">Boolean.</span></span> <span data-ttu-id="f2200-471">Använd `true` att aktivera diagnostiken eller `false` att inaktivera diagnostiken.</span><span class="sxs-lookup"><span data-stu-id="f2200-471">Use `true` to enable the diagnostics or `false` to disable the diagnostics.</span></span>
