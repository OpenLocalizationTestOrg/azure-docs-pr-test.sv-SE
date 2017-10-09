---
title: "aaaUsing PM2 konfiguration för Node.js i Azure Web App på Linux | Microsoft Docs"
description: "Med PM2 konfiguration för Node.js i Azure Web App på Linux"
keywords: "Azure apptjänst, webbprogram, nodejs, pm2, linux, oss"
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: 
ms.assetid: fb420f32-6d74-49c7-992f-0ed5616e66e7
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: naziml;wesmc
ms.openlocfilehash: 923783ffe656e01c43318899d1a656b553ebb5f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-pm2-configuration-for-nodejs-in-azure-web-app-on-linux"></a>Använd PM2 konfiguration för Node.js i Azure Web App på Linux

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


Om du anger hello programmet stack tooNode.js för Azure Web App på Linux, ger hello alternativet tooset en Node.js-startfil som visas i följande bild hello:

![Ange en Node.js-startfil][1]

Du kan använda det här alternativet toodo något av hello följande uppgifter:

* Ange hello startskript för din Node.js-app (till exempel: /bin/server.js).
* Ange hello PM2 configuration file toouse för din Node.js-app (till exempel: /foo/process.json).
  
  > [!NOTE]
  > Om du vill att din Node.js processer toorestart automatiskt när vissa filer ändras, använder du hello PM2 konfiguration. Annars tillämpningsprogrammet inte startar du om när den tar emot meddelanden (till exempel när din programkod ändras).
  > 
  > 

Du kan kontrollera hello Node.js [Processdokumentation filen](http://pm2.keymetrics.io/docs/usage/application-declaration/) alla hello alternativ, men följande är ett exempel på vad du kan använda som process.json-fil:

        {
          "name"        : "worker",
          "script"      : "./bin/server.js",
          "instances"   : 1,
          "merge_logs"  : true,
          "log_date_format" : "YYYY-MM-DD HH:mm Z",
          "watch": ["./bin/server.js", "foo.txt"],
          "watch_options": {
            "followSymlinks": true,
            "usePolling"   : true,
            "interval"    : 5
          }
        }

Viktiga saker toonote i den här konfigurationen är:

* Hej ”skript” egenskap anger start programskript.
* Hej ”förekomster” egenskap anger hur många instanser av hello nod processen toolaunch. Om du kör programmet på större virtuella datorer som har flera kärnor, är det en bra idé toomaximize dina resurser genom att ange ett högre värde här.
* Hej ”titta på” matrisen anger alla filer som du vill toorestart hello nod process för när de ändras.
* För hello ”watch_options” måste för närvarande toospecify ”usePolling” som SANT på grund av hello sätt ditt programinnehåll är monterad.

## <a name="next-steps"></a>Nästa steg
* [Vad är Azure Web App på Linux?](app-service-linux-intro.md)
* [Azure App Service Webbapp på Linux vanliga frågor och svar](app-service-linux-faq.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-nodejs-pm2/nodejs-startup-file.png
