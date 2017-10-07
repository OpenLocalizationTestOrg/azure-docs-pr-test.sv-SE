---
title: "aaaWhat är Azure Automation | Microsoft Docs"
description: "Lär dig vilket värde som innehåller Azure Automation och få svar på frågor för toocommon så att du kan komma igång skapas med hjälp av runbooks och Azure Automation DSC."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
keywords: "vad är automation, azure automation, azure automation-exempel"
ms.assetid: 0cf1f3e8-dd30-4f33-b52a-e148e97802a9
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/10/2016
ms.author: magoedte;bwren
ms.openlocfilehash: 1e5a90e272d6b2beb7b5007e2fea2c110dbd79b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-overview"></a>Översikt över Azure Automation
Microsoft Azure Automation är ett sätt för användarna tooautomate hello manuell, långvariga, felbenägna och ofta återkommande uppgifter som utförs ofta i en miljö med molnet och företagets. Det sparar tid och ökar tillförlitligheten hello vanliga administrativa uppgifter och även schemalägger dem toobe automatiskt utföras med jämna mellanrum. Du kan automatisera processer med hjälp av runbooks eller automatisera konfigurationshantering med Desired State Configuration. Den här artikeln innehåller en kort översikt över Azure Automation och ger svar på några vanliga frågor. Mer detaljerad information om hello olika ämnen kan du referera tooother artiklar i det här biblioteket.

## <a name="automating-processes-with-runbooks"></a>Automatisera processer med runbooks
En runbook är en uppsättning aktiviteter som utför en automatisk process i Azure Automation. Det kan vara en enkel process som att starta en virtuell dator och skapa en loggpost eller du kan ha en komplex runbook som kombinerar andra mindre runbooks tooperform en komplicerad process över flera resurser eller även flera moln och lokala miljöer.  

Du kan till exempel ha en befintlig manuell process för att trunkera en SQL-databas om den närmar sig maximal storlek som innehåller flera steg, till exempel den anslutande toohello server ansluter toohello databasen, hämta hello aktuella storleken på databasen, kontrollera om Tröskelvärde har överskridits trunkera den och meddela användaren. I stället för att manuellt utföra vart och ett av dessa steg kan du skapa en runbook som utför alla dessa uppgifter som en enda process. Du startar hello runbook, hello krävs information som exempelvis hello SQL-servernamnet, databasnamnet och mottagarens e- och sitta tillbaka medan hello processen har slutförts. 

## <a name="what-can-runbooks-automate"></a>Vad kan runbooks automatisera?
Eftersom runbooks i Azure Automation baseras på Windows PowerShell eller Windows PowerShell Workflow så kan de göra allt som PowerShell kan. Om ett program eller en tjänst har ett API kan en runbook arbeta med dem. Om du har en PowerShell-modul för hello program, kan du läsa in modulen i Azure Automation och inkluderar dessa cmdletar i din runbook. Azure Automation-runbooks körs i hello Azure-molnet och kan komma åt molnresurser eller externa resurser som kan nås från hello molnet. Med hjälp av [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md), runbooks kan köras i dina lokala data center toomanage lokala resurser. 

## <a name="getting-runbooks-from-hello-community"></a>Hämtar runbooks från hello-gemenskapen
Hej [Runbook-galleriet](automation-runbook-gallery.md#runbooks-in-runbook-gallery) innehåller runbooks från Microsoft och hello-community som du kan använda oförändrad i din miljö eller anpassa dem efter egna behov. De är också användbart tooas referenser toolearn hur toocreate egna runbooks. Du kan även bidra med egna runbooks toohello galleriet som du tror att andra användare kan vara användbart. 

## <a name="creating-runbooks-with-azure-automation"></a>Skapa runbooks med Azure Automation
Du kan [skapar egna runbooks](automation-creating-importing-runbook.md) från scratch eller ändra runbooks från hello [Runbook-galleriet](http://msdn.microsoft.com/library/azure/dn781422.aspx) för dina egna behov. Det finns fyra olika [runbook-typer](automation-runbook-types.md) som du kan välja mellan beroende på dina krav och din PowerShell-miljö. Om du föredrar toowork direkt med hello PowerShell-kod, så du kan använda en [PowerShell-runbook](automation-runbook-types.md#powershell-runbooks) eller [PowerShell-arbetsflödesrunbook](automation-runbook-types.md#powershell-workflow-runbooks) du redigera offline eller hello [textrepresentation editor](http://msdn.microsoft.com/library/azure/dn879137.aspx) i hello Azure-portalen. Om du föredrar tooedit en runbook utan att exponeras toohello underliggande kod därefter kan du skapa en [grafisk runbook](automation-runbook-types.md#graphical-runbooks) med hello [grafiska redigerare](automation-graphical-authoring-intro.md) i hello Azure-portalen. 

Vill titta på tooreading? Ta en titt på hello nedan video från Microsoft Ignite session i maj 2015. Obs: Hello koncept och funktioner som beskrivs i den här videon är korrekt, Azure Automation har nått mycket eftersom den här videon registrerades, nu har en mer omfattande användargränssnitt i hello Azure-portalen och har stöd för ytterligare funktioner.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3451/player]
> 
> 

## <a name="automating-configuration-management-with-desired-state-configuration"></a>Automatisera konfigurationshanteringen med Desired State Configuration
[PowerShell DSC](https://technet.microsoft.com/library/dn249912.aspx) är en plattform som gör att du toomanage, distribuera och upprätthålla konfiguration för fysiska värdar och virtuella datorer med en deklarativ PowerShell-syntax. Du kan definiera konfigurationer på en central DSC-hämtningsserver som måldatorer kan hämta och använda automatiskt. DSC tillhandahåller en uppsättning PowerShell-cmdlets som du kan använda toomanage konfigurationer och resurser.  

[Azure Automation DSC](automation-dsc-overview.md) är en molnbaserad lösning för PowerShell DSC som tillhandahåller tjänster som krävs för företagsmiljöer.  Du kan hantera resurser i Azure Automation DSC och använda konfigurationer toovirtual eller fysiska datorer som hämtar dem från en DSC Pull-Server i hello Azure-molnet.  Lösningen innehåller också rapporteringstjänster som informerar dig om viktiga händelser, t.ex. när noder har avvikit från sin tilldelade konfiguration och när en ny konfiguration har tillämpats. 

## <a name="creating-your-own-dsc-configurations-with-azure-automation"></a>Skapa dina egna DSC-konfigurationer med Azure Automation
[DSC-konfigurationer](automation-dsc-overview.md) ange hello önskad tillstånd för en nod.  Flera noder kan tillämpa hello samma konfiguration tooassure alla Underhåll tillståndet identiska.  Du kan skapa en konfiguration med valfri textredigerare på den lokala datorn och sedan importera den till Azure Automation där du kan sammanställa och tillämpa den på noder.

## <a name="getting-modules-and-configurations"></a>Hämta moduler och konfigurationer
Du kan hämta [PowerShell-moduler](automation-runbook-gallery.md#modules-in-powershell-gallery) som innehåller cmdletar som du kan använda i dina runbooks och DSC-konfigurationer från hello [PowerShell-galleriet](http://www.powershellgallery.com/). Du kan öppna den här gallery från hello Azure-portalen och importera moduler direkt till Azure Automation du kan hämta och importera dem manuellt Du kan inte installera hello moduler direkt från hello Azure-portalen, men du kan hämta dem installera dem som en annan modul. 

## <a name="example-practical-applications-of-azure-automation"></a>Exempel på praktiska tillämpningar med Azure Automation
Följande är några exempel på vad är hello typer av automatiseringsscenarier med Azure Automation. 

* Skapa och kopiera virtuella datorer i olika Azure-prenumerationer. 
* Schemalägga att filen kopieras från en lokal dator tooan Azure Blob Storage-behållare. 
* Automatisera säkerhetsfunktioner, t.ex. funktioner som nekar förfrågningar från en klient när en DoS-attack (Denial of Service) upptäckts. 
* Se till att datorerna alltid uppfyller kraven i konfigurerade säkerhetsprinciper.
* Hantera kontinuerlig distribution av programkod i molnet och i den lokala infrastrukturen. 
* Skapa en Active Directory-skog i Azure för din labbmiljö. 
* Trunkera en tabell i en SQL-databas om databasen närmar sig den största tillåtna storleken. 
* Fjärruppdatera miljöinställningar för en Azure-webbplats. 

## <a name="how-does-azure-automation-relate-tooother-automation-tools"></a>Hur relaterar tooother verktyg för automatisering av Azure Automation?
[Service Management Automation (SMA)](http://technet.microsoft.com/library/dn469260.aspx) är avsedda tooautomate hanteringsuppgifter i hello privat moln. Det installeras lokalt i ditt datacenter som en komponent i [Microsoft Azure Pack](https://www.microsoft.com/en-us/server-cloud/). SMA och Azure Automation använder hello samma runbook format baserat på Windows PowerShell och Windows PowerShell-arbetsflöde, men har inte stöd för SMA [grafiska runbook-flöden](automation-graphical-authoring-intro.md).  

[System Center 2012 Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) är avsett för automatisering av lokala resurser. Den använder en annan runbook-format än Azure Automation och Service Management Automation och har ett grafiskt gränssnitt toocreate runbooks utan skript. Dess runbooks består av aktiviteter från integrationspaket som är skrivna specifikt för Orchestrator. 

## <a name="where-can-i-get-more-information"></a>Var hittar jag mer information?
En mängd resurser är tillgängliga för du toolearn mer om Azure Automation och skapa egna runbooks. 

* **Azure Automation-biblioteket** är där du är just nu. hello artiklar i det här biblioteket innehåller komplett dokumentation på hello konfiguration och administration av Azure Automation och egna runbooks för redigering. 
* [Azure PowerShell-cmdlets](http://msdn.microsoft.com/library/jj156055.aspx) innehåller information om hur du automatiserar Azure-åtgärder med hjälp av Windows PowerShell. Runbooks använder dessa cmdlets toowork med Azure-resurser. 
* [Hanteringsbloggen](https://azure.microsoft.com/blog/tag/azure-automation/) innehåller hello senaste information om Azure Automation och andra tekniker för hantering från Microsoft. Du ska prenumerera på toothis blogg toostay in toodate med hello senaste från hello Azure Automation-teamet. 
* [Automation-Forum](http://go.microsoft.com/fwlink/p/?LinkId=390561) kan du toopost frågor om Azure Automation-toobe som hanteras av Microsoft och hello Automation community. 
* [Azure Automation-cmdlets](https://msdn.microsoft.com/library/mt244122.aspx) innehåller information om hur du automatiserar administrationsåtgärder. Den innehåller cmdlets toomanage Automation-konton, tillgångar, runbooks, DSC.

## <a name="can-i-provide-feedback"></a>Kan jag lämna feedback?
**Vi tar gärna emot din feedback!** Om du letar efter en lösning för Azure Automation-runbooks eller en integreringsmodul kan du publicera en skriptbegäran på Script Center. Om du har feedback eller funktionsönskemål om Azure Automation kan du publicera dem i [User Voice](http://feedback.windowsazure.com/forums/34192--general-feedback). Tack! 

