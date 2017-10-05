---
title: Nyheter i Power BI Embedded
description: "Få den senaste informationen om vad är nytt i Power BI Embedded"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 2794ae98-b9a7-45df-b6e1-962a395b91fa
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
ms.openlocfilehash: 07c53a5d6b1881a4c207a2aefed9fcede0fa069e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="whats-new-in-power-bi-embedded"></a>Nyheter i Power BI Embedded

Uppdateringar för **Power BI Embedded** släpps regelbundet. Dock innehåller inte alla viktig nya funktioner för användarinriktad; vissa versioner fokuserar på funktioner för backend-tjänst. Vi ska markera nya användarinriktad funktioner här. Tänk på att Kom tillbaka ofta.

## <a name="march-2017"></a>Mars 2017

<iframe width="640" height="360" src="https://www.youtube.com/embed/ibuN4DzCl5c?showinfo=0" frameborder="0" allowfullscreen></iframe>

**Självbetjäning funktioner**

* [Skapa ny rapport](power-bi-embedded-create-report-from-dataset.md)
* [Spara rapporten](power-bi-embedded-save-reports.md)
* Bädda in rapporten i Läs/redigera/skapa nya läge 
* [Växla mellan lägen redigera/läsning](power-bi-embedded-toggle-mode.md)

**Dataanslutning med REST API: er**

* [Skapa datauppsättningen](https://msdn.microsoft.com/library/azure/mt778875.aspx)
* Skicka data 

**Management-API: er**

* Klona rapporten och datamängd
* Binda rapporten till en annan datauppsättning

**Exempel**

* Uppdatera [JavaScript rapporten bäddas in exempel](https://microsoft.github.io/PowerBI-JavaScript/demo)

## <a name="december-2016"></a>December 2016

* [Ny JavaScript bädda in exempel](https://microsoft.github.io/PowerBI-JavaScript/demo/)

## <a name="october-2016"></a>Oktober 2016

* [Avancerade analyser med Powerbi Embedded och R](https://powerbi.microsoft.com/blog/r-in-pbie/)

## <a name="august-31st-2016"></a>Den 31 augusti 2016
Ingår i den här versionen:

* Alla nya JavaScript SDK som stöder [avancerade filtrering och sidan navigering](power-bi-embedded-interact-with-reports.md).
* Power BI Embedded stöds nu i Kanada Central datacentret. Kontrollera [datacenter status](https://azure.microsoft.com/status/).

## <a name="july-11th-2016"></a>11 juli 2016
Ingår i den här versionen:

* **Goda nyheter!** Power BI Embedded tjänsten är inte längre i preview - är nu GA (allmänt tillgänglig).  
* Alla REST API: er har flyttats från **/beta** till **/v1.0**.
* .NET- och JavaScript SDK har uppdaterats för **v1.0**.
* Power BI-API-anrop kan nu autentiseras direkt med hjälp av API-nycklar. Apptoken krävs endast för att bädda in. Etablera och dev token har tagits bort v1.0 API: er som en del av detta, men de kommer att fortsätta att arbeta i betaversionen till 2016/12/30. Läs mer i [autentisera och auktorisera med Power BI Embedded](power-bi-embedded-app-token-flow.md).
* Raden säkerhet på radnivå (RLS) stöd för apptoken och inbäddade rapporter. Läs mer i [rad säkerhet på radnivå med Power BI Embedded](power-bi-embedded-rls.md).
* Uppdatera exempelprogram för alla **v1.0** API-anrop.
* Power BI Embedded stöd för Azure SDK, PowerShell och CLI.
* Användare kan exportera visualiseringen data till en **.csv**.
* Power BI Embedded stöds nu i alla de samma/språk som Microsoft Azure. Läs mer i [Azure - språk](http://social.technet.microsoft.com/wiki/contents/articles/4234.windows-azure-extent-of-localization.aspx).

