---
title: aaaPublish WebApplicationVM | Microsoft Docs
description: "Lär dig hur toodeploy en web application tooa virtuell dator. Det här skriptet skapar hello krävs resurser i din Azure-prenumeration om de inte redan finns."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: de4cec95-f73f-44d9-babd-9f47f2633cdb
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: e4b52b620bebf44b87ddfc3b19c155bb65111814
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-webapplicationvm-windows-powershell-script"></a>Publicera-WebApplicationVM (Windows PowerShell-skript)
Distribuerar en web application tooa virtuell dator. hello skriptet skapar hello krävs resurser i din Azure-prenumeration om de inte redan finns.

```
Publish-WebApplicationVM
–Configuration <configuration>
-SubscriptionName <subscriptionName>
-WebDeployPackage <packageName>
-VMPassword @{Name = "name"; Password = "password")
-DatabaseServerPassword @{Name = "name"; Password = "password"}
-SendHostMessagesToOutput
-Verbose
```

### <a name="configuration"></a>Konfiguration
hello sökvägen toohello JSON konfigurationsfil som beskriver hello information om hello distributionen.

| Alias | Ingen |
| --- | --- |
| Krävs? |SANT |
| Position |Med namnet |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |FALSKT |
| Acceptera jokertecken? |FALSKT |

### <a name="subscriptionname"></a>SubscriptionName
hello namnet på hello Azure-prenumeration som du vill toocreate hello virtuell dator.

| Alias | Ingen |
| --- | --- |
| Krävs? |FALSKT |
| Position |Med namnet |
| Standardvärde |Använder hello första prenumeration i hello prenumerationsfilen |
| Acceptera indata från pipeline? |FALSKT |
| Acceptera jokertecken? |FALSKT |

### <a name="webdeploypackage"></a>WebDeployPackage
hello sökvägen toohello distribution paketet toopublish toohello virtuella datorn på webbservern. Du kan skapa det här paketet med hjälp av guiden för hello Publicera webbplats i Visual Studio. Se [så här: skapa ett Webbdistributionspaket i Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).

| Alias | Ingen |
| --- | --- |
| Krävs? |FALSKT |
| Position |Med namnet |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |FALSKT |
| Acceptera jokertecken? |FALSKT |

### <a name="allowuntrusted"></a>AllowUntrusted
Om värdet är true Tillåt hello användning av certifikat som inte är signerat av en betrodd rotcertifikatutfärdare.

| Alias | Ingen |
| --- | --- |
| Krävs? |FALSKT |
| Position |Med namnet |
| Standardvärde |FALSKT |
| Acceptera indata från pipeline? |FALSKT |
| Acceptera jokertecken? |FALSKT |

### <a name="vmpassword"></a>VMPassword
hello autentiseringsuppgifterna för kontot för hello virtuell dator. Exempel: - VMPassword @{Name = ”admin”; Lösenord = ”password”}

| Alias | Ingen |
| --- | --- |
| Krävs? |FALSKT |
| Position |Med namnet |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |FALSKT |
| Acceptera jokertecken? |FALSKT |

### <a name="databaseserverpassword"></a>DatabaseServerPassword
hello autentiseringsuppgifter för hello SQL-databas i Azure. Exempel: - DatabaseServerPassword @{Name = ”admin”; Lösenord = ”password”}

| Alias | Ingen |
| --- | --- |
| Krävs? |FALSKT |
| Position |Med namnet |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |FALSKT |
| Acceptera jokertecken? |FALSKT |

### <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput
Om värdet är true utdata ut meddelanden från hello skriptet toohello dataströmmen.

| Alias | Ingen |
| --- | --- |
| Krävs? |FALSKT |
| Position |Med namnet |
| Standardvärde |FALSKT |
| Acceptera indata från pipeline? |FALSKT |
| Acceptera jokertecken? |FALSKT |

## <a name="remarks"></a>Kommentarer
En fullständig förklaring av hur toouse hello skriptet toocreate utvecklings- och testmiljöer, se [med hjälp av Windows PowerShell-skript tooPublish tooDev och testmiljöer](vs-azure-tools-publishing-using-powershell-scripts.md).

hello JSON-konfigurationsfil anger hello information om vad är toobe distribueras. Den omfattar hello information som du angav när du skapade hello projekt, till exempel hello namn, tillhörighetsgrupp, VHD-avbildningen och storleken på hello virtuella datorn. Dessutom innehåller hello slutpunkter på hello databaser tooprovision hello virtuell dator, och web distributionsparametrarna. hello följande kod visar ett exempel JSON-konfigurationsfil:

```
{
    "environmentSettings": {
        "cloudService": {
            "name": "myvmname",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myvmname",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201404.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [
                    {
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [
            {
                "connectionStringName": "",
                "databaseName": "",
                "serverName": "",
                "user": "",
                "password": ""
            }
        ],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

Du kan redigera hello JSON configuration file toochange vad etableras. En virtuell dator och en molnbaserad tjänst som krävs, men hello databasen avsnittet är valfritt.

