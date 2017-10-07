---
title: "kompatibilitetsnivån för aaaData modellen i Azure Analysis Services | Microsoft Docs"
description: "Förstå tabelldata modellen kompatibilitetsnivå."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/16/2017
ms.author: owend
ms.openlocfilehash: bfaf0c60666729d1e6e0baf082c046ea9faa4e86
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="compatibility-level-for-analysis-services-tabular-models"></a>Kompatibilitetsnivån för Analysis Services-tabellmodeller

*Kompatibilitetsnivån* refererar toorelease-specifika funktioner i hello Analysis Services-motorn. Ändringar toohello kompatibilitetsnivå sammanfaller normalt med större versioner av SQL Server. De här ändringarna implementeras också i Azure Analysis Services toomaintain paritet mellan båda plattformar. Kompatibilitet ändras även påverka funktionerna i din tabellmodeller. Till exempel har DirectQuery och tabular objektmetadata olika implementeringar beroende på hello kompatibilitetsnivå. 

Azure Analysis Services har stöd för tabellmodeller på hello 1200 och 1400 kompatibilitetsnivåer.

hello senaste kompatibilitetsnivå är 1400. Den här nivån sammanfaller med SQL Server 2017 Analysis Services. Viktiga funktioner i hello 1400 kompatibilitetsnivå omfattar:

*  Ny infrastruktur för dataanslutning och importeras till tabellmodeller med stöd för TOM APIs och TMSL skript. Den här nya funktionen aktiverar stöd för andra datakällor, till exempel Azure Blob storage.
*  Dataomvandling av och data mashup-funktioner med hjälp av hämta Data och M uttryck.
*  Åtgärder som stöd för en detaljrader egenskap med ett DAX-uttryck. Den här egenskapen gör klientverktyg som Microsoft Excel toodrill toodetailed data från en sammansatt rapport. När användare visar total försäljning för en region och månad, kan de visa hello associerade orderinformationen. 
*  Säkerhet på objektnivå för tabell- och namn, dessutom toohello data i dem.
*  Förbättrat stöd för ojämna hierarkier.
*  Prestanda och förbättringar i övervakning.
  
## <a name="set-compatibility-level"></a>Ange kompatibilitetsnivån 
 När du skapar ett nytt tabellmodell-projekt i SSDT, kan du ange hello kompatibilitetsnivån på hello **tabellmodell designer** dialogrutan. 
  
 ![ssas_tabularproject_compat1200](./media/analysis-services-compat-level/aas-tabularproject-compat.png)  
  
 Om du väljer hello **Visa inte detta meddelande igen** , alla efterföljande projekt använder hello kompatibilitetsnivå som du angav som hello standard. Du kan ändra hello standard kompatibilitetsnivån i SSDT i **verktyg** > **alternativ**.  
  
 tooupgrade ett befintligt tabellmodell-projekt i SSDT set hello **kompatibilitetsnivå** egenskap i hello modellen **egenskaper** fönster. Håll i åtanke, uppgradera hello kompatibilitetsnivån går inte att ångra.
  
## <a name="check-compatibility-level-for-a-tabular-model-database-in-sql-server-management-studio"></a>Kontrollera kompatibilitetsnivån för en tabellmodelldatabas i SQL Server Management Studio 
 I SSMS, högerklickar du på hello-databasnamn > **egenskaper** > **kompatibilitetsnivå**.  
  
## <a name="check-supported-compatibility-level-for-a-server-in-ssms"></a>Kontrollera stöds kompatibilitetsnivån för en server i SSMS  
 I SSMS, högerklickar du på hello-servernamn > **egenskaper** > **stöds kompatibilitetsnivå**.  
  
 Den här egenskapen anger hello högsta kompatibilitetsnivån för en databas som ska köras på hello-server (exklusive preview). hello stöds kompatibilitetsnivån kan inte ändras.  

## <a name="next-steps"></a>Nästa steg
  [Skapa en modell i Azure-portalen](analysis-services-create-model-portal.md)   
  [Hantera Analysis Services](analysis-services-manage.md)  
