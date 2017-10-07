---
title: aaaError hantering i Azure Automation grafiska runbook | Microsoft Docs
description: "Den här artikeln beskriver hur tooimplement felhantering logiken i Azure Automation grafisk runbook."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/26/2016
ms.author: magoedte
ms.openlocfilehash: b9ff01361d2ebd9c0174b074a7a290b1cc2fd1c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="error-handling-in-azure-automation-graphical-runbooks"></a>Felhantering i Azure Automation grafiska runbooks

En nyckel runbook design huvudnamn tooconsider identifierar olika problem som kan uppstå i en runbook. De här problemen kan omfatta lyckade åtgärder, förväntade feltillstånd och oväntade feltillstånd.

Runbooks bör inkludera felhantering. toovalidate hello utdata för en aktivitet eller hantera ett fel med grafiska runbook-flöden, du kan använda en aktivitet för Windows PowerShell-koden, definiera villkorlig logik på hello utdata länk hello aktivitet eller använda en annan metod.          

Om det finns ett icke avslutar fel som uppstår vid en runbook-aktivitet, bearbetas alla aktiviteter som följer ofta, oavsett hello-fel. hello fel är sannolikt toogenerate ett undantag, men hello nästa aktivitet har fortfarande beviljad toorun. Detta är hello sättet att PowerShell är utformad toohandle fel.    

hello typer av PowerShell-fel som kan uppstå under körningen avbryts eller inte avslutas. hello skillnaderna mellan avslutande och icke-avslutande fel är följande:

* **Avslutar fel**: ett allvarligt fel vid körning som stoppar hello kommando (eller skriptkörningen) helt. Exempel på detta är obefintliga cmdlet:ar, syntaxfel som förhindrar att en cmdlet körs eller andra allvarliga fel.

* **Icke-avslutande fel**: ett icke-allvarligt fel som tillåter körning av toocontinue trots hello-fel. Exempel på detta är operativa fel som att en fil inte gick att hitta och behörighetsproblem.

Azure Automation-grafiska runbooks har förbättrats med hello kapaciteten tooinclude felhantering. Du kan nu omvandla undantag till icke-avslutande fel och skapa fellänkar mellan aktiviteter. Den här processen kan en runbook-redigerare toocatch fel och hantera realiserad eller oväntat villkor.  

## <a name="when-toouse-error-handling"></a>När toouse felhantering

När det är en kritisk aktivitet som genererar ett fel eller ett undantag, är det viktigt tooprevent hello nästa aktivitet i din runbook från bearbetning och toohandle hello-fel på lämpligt sätt. Detta är särskilt viktigt när runbooks stöder en företags- eller tjänståtgärdsprocess.

För varje aktivitet som kan producera ett fel, hello runbook-redigerare lägga till ett fel länk tooany andra aktiviteter.  hello målaktiviteten kan vara av valfri typ, inklusive kod aktiviteter, anropar en cmdlet anropar en annan runbook och så vidare.

Hello målaktiviteten kan dessutom också ha utgående länkar. Dessa länkar kan vara vanliga länkar eller fellänkar. Det innebär att hello runbook-redigerare kan implementera komplex logik för felhantering utan sortera tooa code aktivitet. hello rekommenderas praxis toocreate en dedikerad felhantering runbook med vanliga funktioner, men det är inte obligatoriskt. Felhantering logiken i en aktivitet för PowerShell-kod som inte hello endast alternativet.  

Till exempel att du en runbook som försöker toostart en virtuell dator och installera ett program på den. Om hello VM inte startas på rätt sätt, genomför två åtgärder:

1. Den skickar ett meddelande om det här problemet.
2. Den startar en annan runbook som automatiskt etablerar en ny virtuell dator i stället.

En lösning är toohave fel pekar tooan aktivitet som hanterar steg ett. Du kan till exempel ansluta hello **Write-Warningg** cmdlet tooan aktivitet för steg två, till exempel hello **Start AzureRmAutomationRunbook** cmdlet.

Du kan också generalisera beteendet för användning i många runbooks genom att dessa två aktiviteter i en separat hantering runbook och hello vägledningen förslag tidigare. Innan du anropar den här felhantering runbook du skapar ett anpassat meddelande från hello data i hello ursprungliga runbook och skickar den sedan som en parameter toohello felhantering runbook.

## <a name="how-toouse-error-handling"></a>Hur toouse felhantering

Varje aktivitet har en konfigurationsinställning som omvandlar undantag till icke-avslutande fel. Som standard är denna inställning inaktiverad. Vi rekommenderar att du aktiverar den här inställningen på alla aktiviteter där du vill att toohandle fel.  

Genom att aktivera den här konfigurationen, du säkerställa att både avslutande och icke-avslutande fel i hello aktivitet hanteras som ej avslutande fel och kan hanteras med en fel-länk.  

När du har konfigurerat den här inställningen kan du skapa en aktivitet som hanterar hello-fel. Om en aktivitet producerar något fel, hello utgående fel länkar följs och hello reguljära länkar som inte även om hello aktivitet producerar samt regelbundna utdata.<br><br> ![Fellänksexempel för Automation Runbook](media/automation-runbook-graphical-error-handling/error-link-example.png)

I följande exempel hello, hämtar en variabel som innehåller hello datornamnet för en virtuell dator i en runbook. Sedan försöker den toostart hello virtuell dator med hello nästa aktivitet.<br><br> ![Felhanteringsexempel för Automation Runbook](media/automation-runbook-graphical-error-handling/runbook-example-error-handling.png)<br><br>      

Hej **Get-automationvariable,** aktivitet och **Start AzureRmVm** är konfigurerade tooconvert undantag tooerrors.  Om det finns problem med hello variabel eller första hello VM sedan fel genereras.<br><br> ![Felhantering i Automation Runbook, aktivitetsinställningar](media/automation-runbook-graphical-error-handling/activity-blade-convertexception-option.png)

Fel länkar flödar från dessa aktiviteter tooa enda **fel management** aktivitet (en kod aktivitet). Den här aktiviteten är konfigurerad med ett enkelt PowerShell-uttryck som använder hello *utlösa* nyckelordet toostop bearbetning, tillsammans med *$Error.Exception.Message* tooget hello-meddelande som beskriver hello aktuella undantaget.<br><br> ![Automation Runbook-felhantering, kodexempel](media/automation-runbook-graphical-error-handling/runbook-example-error-handling-code.png)


## <a name="next-steps"></a>Nästa steg

* toolearn mer om länkar och länktyper i grafiska runbooks finns [grafiska redigering i Azure Automation](automation-graphical-authoring-intro.md#links-and-workflow).

* Mer om runbook-körningen hur toomonitor runbook-jobb och annan teknisk information finns i toolearn [spåra en runbook-jobbet](automation-runbook-execution.md).
