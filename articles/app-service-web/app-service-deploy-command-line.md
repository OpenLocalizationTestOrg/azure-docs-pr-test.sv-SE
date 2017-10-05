---
title: Automatisera distributionen av din Azure-app med kommandoradsverktyg | Microsoft Docs
description: "Identifiera information om hur du distribuerar din Azure-app från kommandoraden"
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
ms.openlocfilehash: e0e2e65557911bcac06d4dc355f47e9331934f8a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="automate-deployment-of-your-azure-app-with-command-line-tools"></a>Automatisera distributionen av din Azure-app med kommandoradsverktyg
Du kan automatisera distributionen av dina Azure appar med kommandoradsverktyg. Den här artikeln visar tillgängliga verktyg och länkar som visar hur du använder dem i ditt arbetsflöde för distribution. 

## <a name="msbuild"></a>Automatisera distributionen med MSBuild
Om du använder den [Visual Studio IDE](#vs) för utveckling, kan du använda [MSBuild](http://msbuildbook.com/) att automatisera något du kan göra i din IDE. Du kan konfigurera MSBuild för att använda antingen [webbdistribution](#webdeploy) eller [FTP/FTPS](#ftp) att kopiera filer. Webbdistribution kan också automatisera många andra distribution-relaterade uppgifter, till exempel att distribuera databaser.

Mer information om kommandoradsverktyget distribution med MSBuild finns i följande resurser:

* [ASP.NET Web-distribution med Visual Studio: kommandoraden distribution](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). Tionde i en serie kurser om distribution till Azure med hjälp av Visual Studio. Visar hur du använder kommandoraden för att distribuera efter att ställa in publicera profiler i Visual Studio.
* [I Microsoft Build-motorn: med hjälp av MSBuild och Team Foundation Build](http://msbuildbook.com/). Papperskopior adressbok som innehåller kapitel om hur du använder MSBuild för distribution.

## <a name="powershell"></a>Automatisera distributionen av Windows PowerShell
Du kan utföra MSBuild- eller FTP-distribution från [Windows PowerShell](http://msdn.microsoft.com/library/dd835506.aspx). Om du gör det kan använda du också en uppsättning Windows PowerShell-cmdlets som gör det enkelt att anropa Azure REST API för hantering.

Mer information finns i följande resurser:

* [Distribuera en webbapp som är kopplad till en GitHub-databas](app-service-web-arm-from-github-provision.md)
* [Etablera ett webbprogram med en SQL-databas](app-service-web-arm-with-sql-database-provision.md)
* [Etablera och distribuera mikrotjänster förutsägbart i Azure](app-service-deploy-complex-application-predictably.md)
* [Skapa verkliga Molnappar med Azure - automatisera allt](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything). E-bok kapitel som förklarar hur exempelprogrammet visas i e-bok använder Windows PowerShell-skript för att skapa en Azure testmiljö och distribuera den. Finns det [resurser](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything#resources) avsnittet länkar till ytterligare dokumentation för Azure PowerShell.
* [Med Windows PowerShell-skript för att publicera till utvecklings- och testmiljöer](../vs-azure-tools-publishing-using-powershell-scripts.md). Hur du använder Windows PowerShell skript distribution som genererar Visual Studio.

## <a name="api"></a>Automatisera distributionen med .NET hanterings-API
Du kan skriva C#-kod för att utföra MSBuild- eller FTP-funktioner för distribution. Om du gör det kan du komma åt Azure management REST API för att utföra hanteringsfunktioner för platsen.

Mer information finns i följande resurser:

* [Automatisera allt med bibliotek för Azure och .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Introduktion till .NET API för hantering och länkar till mer dokumentation.

## <a name="cli"></a>Distribuera från Azure kommandoradsgränssnittet (Azure CLI)
Du kan använda kommandoraden i Windows, Mac eller Linux-datorer för att distribuera med hjälp av FTP. Om du gör det, kan du också öppna hanteringskonsolen för Azure REST API: et med Azure CLI.

Mer information finns i följande resurser:

* [Azure kommandoradsverktyg](https://azure.microsoft.com/downloads/). Portalsida i Azure.com kommandoraden verktyget information.

## <a name="webdeploy"></a>Distribuera från Web Deploy kommandorad
[Web Deploy](http://www.iis.net/downloads/microsoft/web-deploy) är Microsoft-programvara för distribution till IIS som ger inte bara intelligent filsynkronisering funktioner men även kan utföra eller samordna många andra distribution-relaterade uppgifter som inte kan ske automatiskt när du använder FTP. Till exempel kan Web Deploy distribuera en ny databas eller databasuppdateringar tillsammans med ditt webbprogram. Webbdistribution kan också minimera den tid som krävs för att uppdatera en befintlig plats eftersom den använder det Intelligent kan kopiera bara ändrade filer. Microsoft Visual Studio och Team Foundation Server har stöd för Web Deploy inbyggda, men du kan också använda Web Deploy direkt från kommandoraden för att automatisera distribution. Web Deploy kommandon är mycket kraftfulla men inlärningskurvan kan vara brant.

## <a name="more-resources"></a>Fler resurser
En annan distributionsalternativ som kommandorader för automatisering är att använda en molnbaserad tjänst som [bläckfisk distribuera](http://en.wikipedia.org/wiki/Octopus_Deploy). Mer information finns i [distribuera ASP.NET-program till Azure webbplatser](https://octopusdeploy.com/blog/deploy-aspnet-applications-to-azure-websites).

Mer information om kommandoradsverktyg finns i följande resurs:

* [Enkel Web Apps: Distribution](https://azure.microsoft.com/blog/2014/07/28/simple-azure-websites-deployment/). Bloggen av David Ebbo om ett verktyg som han skrev att göra det enklare att använda webbdistribution.
* [Web distributionsverktyget](http://technet.microsoft.com/library/dd568996). Officiella dokumentation på webbplatsen Microsoft TechNet. Av den men fortfarande en bra start.
* [Med hjälp av Web distribuera](http://www.iis.net/learn/publish/using-web-deploy). Officiella dokumentation på webbplatsen Microsoft IIS.NET. Av den också men en bra start.
* [ASP.NET Web-distribution med Visual Studio: kommandoraden distribution](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). MSBuild är build-motorn används av Visual Studio och det kan också användas från kommandoraden för att distribuera webbprogram för Web Apps. Den här kursen är en del av en serie som handlar främst om Visual Studio-distribution.

