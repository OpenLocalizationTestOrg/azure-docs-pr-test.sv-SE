---
title: "aaaUsing Ruby i Azure App Service Web App på Linux | Microsoft Docs"
description: "Med hjälp av Ruby i Azure App Service Webbapp på Linux."
keywords: "Azure apptjänst, webbprogram, vanliga frågor och svar, linux, oss, ruby"
services: app-service
documentationCenter: 
author: ahmedelnably
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: aelnably;wesmc
ms.openlocfilehash: 45692cb3bf1da9ff65b9466055029bfaef8b7d8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-ruby-in-web-app-on-linux"></a>Använda Ruby i webbprogrammet på Linux #

Vi har fört stöd för Ruby v.2.3 med hello senaste uppdatering tooour serverdel. Genom att ange hello konfiguration av ditt Linux-webbprogram, kan du ändra hello programstack.

## <a name="using-hello-azure-portal"></a>Med hjälp av hello Azure-portalen ##

Nya hello-menyn i hello [Azure-portalen](https://portal.azure.com), du kan välja toocreate ett webbprogram på Linux hello webb + mobilt alternativ som visas i följande bild hello:

![Skapa en webbapp på hello Azure-portalen][1]

Därefter hello **skapa-bladet** öppnas som visas i följande bild hello:

![hello skapa-bladet][2]

1. Namnge ditt webbprogram.
2. Välj en befintlig resursgrupp eller skapa en ny. (Se tillgängliga regioner i hello [avsnittet](app-service-linux-intro.md).)
3. Välj en befintlig Azure App Service-plan eller skapa en ny. (Se App Service-plan kommentarer i hello [avsnittet](app-service-linux-intro.md).)
4. Välj hello Ruby hello inbyggda Runtime stackar.

När ditt Ruby webbprogram skapas, kan du distribuera tooit med Git- eller FTP.

toolearn mer om att skapa en Ruby app Kontrollera hello [get igång-guide](app-service-linux-ruby-get-started.md)

## <a name="next-steps"></a>Nästa steg
* [Vad är Web App på Linux?](app-service-linux-intro.md)
* [Lokal Git-distribution tooAzure Apptjänst](app-service-deploy-local-git.md)
* [Azure App Service Webbapp på Linux vanliga frågor och svar](app-service-linux-faq.md)
* [Skapa en Ruby App med Azure-Webbapp på Linux](app-service-linux-ruby-get-started.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-ruby/New-Linux.png
[2]: ./media/app-service-linux-using-ruby/Ruby-UX.png