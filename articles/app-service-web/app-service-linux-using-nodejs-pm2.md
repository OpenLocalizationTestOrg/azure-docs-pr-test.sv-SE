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
# <a name="use-pm2-configuration-for-nodejs-in-azure-web-app-on-linux"></a><span data-ttu-id="a42d1-104">Använd PM2 konfiguration för Node.js i Azure Web App på Linux</span><span class="sxs-lookup"><span data-stu-id="a42d1-104">Use PM2 configuration for Node.js in Azure Web App on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


<span data-ttu-id="a42d1-105">Om du anger hello programmet stack tooNode.js för Azure Web App på Linux, ger hello alternativet tooset en Node.js-startfil som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="a42d1-105">If you set hello application stack tooNode.js for Azure Web App on Linux, you get hello option tooset a Node.js startup file as shown in hello following image:</span></span>

![Ange en Node.js-startfil][1]

<span data-ttu-id="a42d1-107">Du kan använda det här alternativet toodo något av hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="a42d1-107">You can use this option toodo one of hello following tasks:</span></span>

* <span data-ttu-id="a42d1-108">Ange hello startskript för din Node.js-app (till exempel: /bin/server.js).</span><span class="sxs-lookup"><span data-stu-id="a42d1-108">Specify hello startup script for your Node.js app (for example: /bin/server.js).</span></span>
* <span data-ttu-id="a42d1-109">Ange hello PM2 configuration file toouse för din Node.js-app (till exempel: /foo/process.json).</span><span class="sxs-lookup"><span data-stu-id="a42d1-109">Specify hello PM2 configuration file toouse for your Node.js app (for example: /foo/process.json).</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="a42d1-110">Om du vill att din Node.js processer toorestart automatiskt när vissa filer ändras, använder du hello PM2 konfiguration.</span><span class="sxs-lookup"><span data-stu-id="a42d1-110">If you want your Node.js processes toorestart automatically when certain files are modified, use hello PM2 configuration.</span></span> <span data-ttu-id="a42d1-111">Annars tillämpningsprogrammet inte startar du om när den tar emot meddelanden (till exempel när din programkod ändras).</span><span class="sxs-lookup"><span data-stu-id="a42d1-111">Otherwise, your application won't restart when it receives change notifications (for example, when your application code changes).</span></span>
  > 
  > 

<span data-ttu-id="a42d1-112">Du kan kontrollera hello Node.js [Processdokumentation filen](http://pm2.keymetrics.io/docs/usage/application-declaration/) alla hello alternativ, men följande är ett exempel på vad du kan använda som process.json-fil:</span><span class="sxs-lookup"><span data-stu-id="a42d1-112">You can check hello Node.js [process file documentation](http://pm2.keymetrics.io/docs/usage/application-declaration/) for all hello options, but following is a sample of what you can use as your process.json file:</span></span>

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

<span data-ttu-id="a42d1-113">Viktiga saker toonote i den här konfigurationen är:</span><span class="sxs-lookup"><span data-stu-id="a42d1-113">Important things toonote in this configuration are:</span></span>

* <span data-ttu-id="a42d1-114">Hej ”skript” egenskap anger start programskript.</span><span class="sxs-lookup"><span data-stu-id="a42d1-114">hello "script" property specifies your application's start script.</span></span>
* <span data-ttu-id="a42d1-115">Hej ”förekomster” egenskap anger hur många instanser av hello nod processen toolaunch.</span><span class="sxs-lookup"><span data-stu-id="a42d1-115">hello "instances" property specifies how many instances of hello node process toolaunch.</span></span> <span data-ttu-id="a42d1-116">Om du kör programmet på större virtuella datorer som har flera kärnor, är det en bra idé toomaximize dina resurser genom att ange ett högre värde här.</span><span class="sxs-lookup"><span data-stu-id="a42d1-116">If you are running your application on larger VMs that have multiple cores, it's a good idea toomaximize your resources by setting a higher value here.</span></span>
* <span data-ttu-id="a42d1-117">Hej ”titta på” matrisen anger alla filer som du vill toorestart hello nod process för när de ändras.</span><span class="sxs-lookup"><span data-stu-id="a42d1-117">hello "watch" array specifies all files that you want toorestart hello node process for when they change.</span></span>
* <span data-ttu-id="a42d1-118">För hello ”watch_options” måste för närvarande toospecify ”usePolling” som SANT på grund av hello sätt ditt programinnehåll är monterad.</span><span class="sxs-lookup"><span data-stu-id="a42d1-118">For hello "watch_options", you currently need toospecify "usePolling" as true because of hello way your application content is mounted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a42d1-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a42d1-119">Next steps</span></span>
* [<span data-ttu-id="a42d1-120">Vad är Azure Web App på Linux?</span><span class="sxs-lookup"><span data-stu-id="a42d1-120">What is Azure Web App on Linux?</span></span>](app-service-linux-intro.md)
* [<span data-ttu-id="a42d1-121">Azure App Service Webbapp på Linux vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="a42d1-121">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-nodejs-pm2/nodejs-startup-file.png
