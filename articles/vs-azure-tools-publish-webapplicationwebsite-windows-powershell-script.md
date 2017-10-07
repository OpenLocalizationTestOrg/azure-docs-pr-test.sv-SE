---
title: aaaPublish WebApplicationWebSite (Windows PowerShell-skript) | Microsoft Docs
description: "Lär dig hur toopublish en web project tooan Azure-webbplatsen. Det här skriptet skapar hello krävs resurser i din Azure-prenumeration om de inte redan finns."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 63cfaa2d-f04d-40dc-8677-345385c278d5
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: d46904e30e3c2e040e57888fa31543e8e366527f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-webapplicationwebsite-windows-powershell-script"></a>Publicera-WebApplicationWebSite (Windows PowerShell-skript)
## <a name="syntax"></a>Syntax
Publicerar en web project tooan Azure-webbplatsen. hello skriptet skapar hello krävs resurser i din Azure-prenumeration om de inte redan finns.

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a>Konfiguration
hello sökvägen toohello JSON konfigurationsfil som beskriver hello information om hello distributionen.

| Parameter | Standardvärde |
| --- | --- |
| Alias |Ingen |
| Krävs? |SANT |
| Position |Med namnet |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |FALSKT |
| Acceptera jokertecken? |FALSKT |

## <a name="subscriptionname"></a>SubscriptionName
hello namnet på hello Azure-prenumeration som du vill ha toocreate hello webbplats.

| Parameter | Standardvärde |
| --- | --- |
| Alias |Ingen |
| Krävs? |FALSKT |
| Position |Med namnet |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |FALSKT |
| Acceptera jokertecken? |FALSKT |

## <a name="webdeploypackage"></a>WebDeployPackage
hello sökvägen toohello web distribution paketet toopublish toohello webbplats. Du kan skapa det här paketet med hjälp av guiden för hello Publicera webbplats i Visual Studio. Mer information finns i [Kom igång med Azure Cloud Services och ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).

| Parameter | Standardvärde |
| --- | --- |
| Alias |Ingen |
| Krävs? |FALSKT |
| Position |Med namnet |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |FALSKT |
| Acceptera jokertecken? |FALSKT |

## <a name="databaseserverpassword"></a>DatabaseServerPassword
hello användarnamn och lösenord för hello SQL-databas i Azure.

| Parameter | Standardvärde |
| --- | --- |
| Alias |Ingen |
| Krävs? |FALSKT |
| Position |Med namnet |
| Standardvärde |Ingen |
| Acceptera indata från pipeline? |FALSKT |
| Acceptera jokertecken? |FALSKT |

## <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput
Om värdet är true utdata ut meddelanden från hello skriptet toohello dataströmmen.

| Parameter | Standardvärde |
| --- | --- |
| Alias |Ingen |
| Krävs? |FALSKT |
| Position |Med namnet |
| Standardvärde |FALSKT |
| Acceptera indata från pipeline? |FALSKT |
| Acceptera jokertecken? |FALSKT |

## <a name="remarks"></a>Kommentarer
En fullständig förklaring av hur toouse hello skriptet toocreate utvecklings- och testmiljöer, se [med hjälp av Windows PowerShell-skript tooPublish tooDev och testmiljöer](vs-azure-tools-publishing-using-powershell-scripts.md).

hello JSON-konfigurationsfil anger hello information om vad är toobe distribueras. Den omfattar hello information som du angav när du skapade hello projekt, till exempel hello namn och användarnamn för hello webbplats. Den omfattar också hello databasen tooprovision eventuella. hello följande kod visar ett exempel JSON-konfigurationsfil:

    {
        "environmentSettings": {
            "webSite": {
                "name": "WebApplication10554",
                "location": "West US"
            },
            "databases": [
                {
                    "connectionStringName": "DefaultConnection",
                    "databaseName": "WebApplication10554_db",
                    "serverName": "iss00brc88",
                    "user": "sqluser2",
                    "password": "",
                    "edition": "",
                    "size": "",
                    "collation": "",
                    "location": "West US"
                }
            ]
        }
    }

Du kan redigera hello JSON configuration file toochange vad distribueras. Ett avsnitt på webbplatsen krävs men hello databasen avsnittet är valfritt.

## <a name="next-steps"></a>Nästa steg
Mer information finns i [publicera WebApplicationVM (Windows PowerShell-skript)](vs-azure-tools-publish-webapplicationvm.md)

