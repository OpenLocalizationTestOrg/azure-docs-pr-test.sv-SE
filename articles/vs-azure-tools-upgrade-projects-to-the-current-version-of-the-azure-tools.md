---
title: aaaHow tooupgrade projekt toohello nuvarande version av hello Azure-verktyg | Microsoft Docs
description: "Lär dig hur tooupgrade en Azure-projekt i Visual Studio toohello nuvarande version av hello Azure-verktyg"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 1d64070a-078d-468a-87f4-e6715de6475f
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: c89ba43af0f2fd9db46ce0c90f0da3d70dc1510b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooupgrade-projects-toohello-current-version-of-hello-azure-tools-for-visual-studio"></a>Hur tooupgrade projekt toohello nuvarande version av hello Azure-verktyg för Visual Studio
## <a name="overview"></a>Översikt
När du har installerat hello aktuella versionen av hello Azure verktyg (eller en tidigare version som är nyare än 1.6) alla projekt som har skapats med hjälp av en Azure-verktyg släpper innan 1.6 (November 2011) uppgraderas automatiskt så fort du öppnar dem.. Om du har skapat projekt med hjälp av hello 1.6 (November 2011) versionen av dessa verktyg och du har installerat uppdateringen kan du öppna projekten i hello äldre versionen och senare bestämmer om tooupgrade dem.

## <a name="how-your-project-changes-when-you-upgrade-it"></a>Hur projektet ändras när du uppgraderar den
Om ett projekt uppgraderas automatiskt eller om du anger att vill du tooupgrade det projektet är ändrade toowork med nuvarande versioner av vissa sammansättningar, och vissa egenskaper är också ändras eftersom det här avsnittet beskrivs. Om ditt projekt kräver andra ändringar toobe som är kompatibel med hello senare version av hello verktyg, måste du utföra ändringarna manuellt.

* hello web.config-filen för webbroller och hello app.config-fil för arbetsroller är uppdaterade tooreference hello senare version av Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitoirTraceListener.dll.
* Hej Microsoft.WindowsAzure.StorageClient.dll och Microsoft.WindowsAzure.Diagnostics.dll Microsoft.WindowsAzure.ServiceRuntime.dll sammansättningarna är uppgraderade toohello nya versioner.
* Publiceringsprofiler som har lagrats i hello Azure projektfilen (.ccproj) är flyttade tooa separat fil med hello tillägget .azurePubXml i hello **publicera** underkatalog.
* Vissa egenskaper i hello publiceringsprofil är uppdaterade toosupport nya och ändrade funktioner. **AllowUpgrade** ersätts av **DeploymentReplacementMethod** eftersom du kan uppdatera en distribuerad molntjänst samtidigt eller inkrementellt.
* Hej egenskapen **UseIISExpressByDefault** läggs till och ange toofalse så att hello webbserver som används för felsökning automatiskt inte ändras från Internet Information Services (IIS) tooIIS Express. IIS Express är hello standard webbserver för projekt som skapas med hello senare versioner av hello verktyg.
* Om Azure Caching finns i en eller flera roller i ditt projekt, ändras vissa egenskaper i hello tjänstkonfiguration (.cscfg-filen) och tjänstdefinitionen (.csdef-fil) när ett projekt har uppgraderats. Om hello projektet använder hello Azure Caching NuGet-paketet, är hello projektet uppgraderade toohello senaste versionen av hello-paketet. Du bör öppna hello web.config-filen och kontrollera att hello klientkonfigurationen har varit korrekt hello uppgraderingsprocessen. Om du har lagt till hello referenser tooAzure cachelagring klientsammansättningar utan att använda hello NuGet-paket uppdateras inte dessa sammansättningar; Du måste manuellt uppdatera dessa referenser toohello nya versioner.

> [!IMPORTANT]
> För F #-projekt, måste du manuellt uppdatera referenser tooAzure sammansättningar så att de refererar till hello senare versioner av dessa sammansättningar.
> 
> 

### <a name="how-tooupgrade-an-azure-project-toohello-current-release"></a>Hur tooupgrade en Azure projektet toohello aktuella versionen
1. Installera hello nuvarande version av hello Azure verktyg i hello installationen av Visual Studio att du vill toouse för hello uppgraderas projektet, och sedan öppna hello som du vill tooupgrade. Om hello projektet har skapats med en Azure-verktyg släpper innan 1.6 (November 2011) hello projektet är automatiskt uppgraderade toohello aktuell version. Om hello projektet har skapats med hello November 2011-versionen och att versionen fortfarande är installerad, öppnas hello-projekt i den versionen.
2. I Solution Explorer, öppna hello snabbmenyn för hello projektnoden väljer **egenskaper**, och välj sedan hello **programmet** fliken hello dialogrutan som visas.
   
    Hej **programmet** visar hello verktyg version som är associerad med hello-projekt. Om hello aktuella versionen av Azure-verktyg visas har hello projekt redan uppgraderats. Om du har installerat en nyare version av hello verktyg än vad hello-fliken visas en **uppgradera** visas knappen.
3. Välj hello **uppgradera** knappen tooupgrade en projektet toohello aktuell version av hello verktyg.
4. Skapa hello projektet och åtgärda eventuella fel som uppstår vid API ändringar. Information om hur toomodify koden för hello ny version, dokumentationen hello för hello specifika API: et.

