---
title: aaaAutomate distribution av din Azure-app med kommandoradsverktyg | Microsoft Docs
description: "Identifiera information om hur du distribuerar din Azure-app från hello kommandoraden"
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 8b65980c-eb75-44a2-8e0f-f9eb9e617d16
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2017
ms.author: cephalin
ms.openlocfilehash: 3df66cc4bf4e6819ed0eee7278ac79dca2e5daa2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automate-deployment-of-your-azure-app-with-command-line-tools"></a>Automatisera distributionen av din Azure-app med kommandoradsverktyg
Du kan automatisera distributionen av dina Azure appar med kommandoradsverktyg. Den här artikeln beskrivs tillgängliga verktyg och hello länkar som visar hur toouse dem i ditt arbetsflöde för distribution. 

## <a name="msbuild"></a>Automatisera distributionen med MSBuild
Om du använder hello [Visual Studio IDE](#vs) för utveckling, kan du använda [MSBuild](http://msbuildbook.com/) tooautomate något du kan göra i din IDE. Du kan konfigurera MSBuild toouse antingen [webbdistribution](#webdeploy) eller [FTP/FTPS](#ftp) toocopy filer. Webbdistribution kan också automatisera många andra distribution-relaterade uppgifter, till exempel att distribuera databaser.

Mer information om kommandoradsverktyget distribution med MSBuild finns hello följande resurser:

* [ASP.NET Web-distribution med Visual Studio: kommandoraden distribution](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). Tionde i en serie kurser om distribution tooAzure med Visual Studio. Visar hur toouse hello kommandoraden toodeploy när du har installerat publicera profiler i Visual Studio.
* [I hello Microsoft skapa Engine: med MSBuild och Team Foundation bygga](http://msbuildbook.com/). Papperskopior adressbok som innehåller kapitel om hur toouse MSBuild för distribution.

## <a name="powershell"></a>Automatisera distributionen av Windows PowerShell
Du kan utföra MSBuild- eller FTP-distribution från [Windows PowerShell](http://msdn.microsoft.com/library/dd835506.aspx). Om du gör det kan använda du också en uppsättning Windows PowerShell-cmdlets som gör hello Azure REST management API enkelt toocall.

Mer information finns i hello följande resurser:

* [Distribuera en web app länkade tooa GitHub-databas](app-service-web-arm-from-github-provision.md)
* [Etablera ett webbprogram med en SQL-databas](app-service-web-arm-with-sql-database-provision.md)
* [Etablera och distribuera mikrotjänster förutsägbart i Azure](app-service-deploy-complex-application-predictably.md)
* [Skapa verkliga Molnappar med Azure - automatisera allt](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything). E-bok kapitel som förklarar hur hello exempelprogrammet visas i hello e-bok använder Windows PowerShell-skript toocreate en Azure-testmiljö och distribuera tooit. Se hello [resurser](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything#resources) avsnittet länkar tooadditional Azure PowerShell-dokumentationen.
* [Med hjälp av Windows PowerShell-skript tooPublish tooDev och testmiljöer](../vs-azure-tools-publishing-using-powershell-scripts.md). Hur toouse distribution av Windows PowerShell-skript som Visual Studio genererar.

## <a name="api"></a>Automatisera distributionen med .NET hanterings-API
Du kan skriva C#-kod tooperform MSBuild- eller FTP-funktioner för distribution. Om du gör det kan du komma åt hello Azure management REST API tooperform platshanteringsfunktioner.

Mer information finns i följande resurs hello:

* [Automatisera allt med hello Azure Management bibliotek och .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Introduktion toohello .NET-API och länkar toomore-dokumentation för hantering.

## <a name="cli"></a>Distribuera från Azure kommandoradsgränssnittet (Azure CLI)
Du kan använda hello kommandoraden i toodeploy för Windows, Mac eller Linux-datorer med hjälp av FTP. Om du gör det, kan du också öppna hello Azure REST management API: et med hello Azure CLI.

Mer information finns i följande resurs hello:

* [Azure kommandoradsverktyg](https://azure.microsoft.com/downloads/). Portalsida i Azure.com kommandoraden verktyget information.

## <a name="webdeploy"></a>Distribuera från Web Deploy kommandorad
[Web Deploy](http://www.iis.net/downloads/microsoft/web-deploy) är Microsoft-programvara för distribution tooIIS som ger inte bara intelligent filen synkronisera funktioner men även kan utföra eller samordna många andra distribution-relaterade uppgifter som inte kan ske automatiskt när du använder FTP. Till exempel kan Web Deploy distribuera en ny databas eller databasuppdateringar tillsammans med ditt webbprogram. Webbdistribution kan också minimera tooupdate hello-tid som krävs för en befintlig plats eftersom den använder det Intelligent kan kopiera bara ändrade filer. Microsoft Visual Studio och Team Foundation Server har stöd för Web Deploy inbyggda, men du kan också använda Web Deploy direkt från hello kommandoraden tooautomate distribution. Web Deploy kommandon är mycket kraftfulla men hello inlärningskurvan kan vara brant.

## <a name="more-resources"></a>Fler resurser
En annan distribution alternativet toocommand rad automation är toouse en molnbaserad tjänst som [bläckfisk distribuera](http://en.wikipedia.org/wiki/Octopus_Deploy). Mer information finns i [distribuera ASP.NET-program tooAzure webbplatser](https://octopusdeploy.com/blog/deploy-aspnet-applications-to-azure-websites).

Mer information om kommandoradsverktyg finns hello följande resurser:

* [Enkel Web Apps: Distribution](https://azure.microsoft.com/blog/2014/07/28/simple-azure-websites-deployment/). Bloggen av David Ebbo om ett verktyg som han skrev toomake den enklare toouse webbdistribution.
* [Web distributionsverktyget](http://technet.microsoft.com/library/dd568996). Officiella dokumentation på hello Microsoft TechNet-webbplatsen. Av den men ändå bra toostart.
* [Med hjälp av Web distribuera](http://www.iis.net/learn/publish/using-web-deploy). Officiella dokumentation på hello Microsoft IIS.NET platsen. Av den också men en bra toostart.
* [ASP.NET Web-distribution med Visual Studio: kommandoraden distribution](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). MSBuild är hello skapa motor som används av Visual Studio och kan även användas från hello kommandoraden toodeploy web applications tooWeb appar. Den här kursen är en del av en serie som handlar främst om Visual Studio-distribution.

