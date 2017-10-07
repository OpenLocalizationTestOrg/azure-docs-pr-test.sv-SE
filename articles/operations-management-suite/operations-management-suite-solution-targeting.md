---
title: "aaaSolution mål i OMS | Microsoft Docs"
description: "Lösning målinriktning är en funktion i Operations Management Suite (OMS) och som gör att du toolimit management lösningar tooa specifik uppsättning agenter.  Den här artikeln beskriver hur toocreate en scope-konfiguration och tillämpa det tooa lösning."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1f054a4e-6243-4a66-a62a-0031adb750d8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2017
ms.author: bwren
ms.openlocfilehash: 6f8c8109e0d9e282e18724bf8b673b10de8e498a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-solution-targeting-in-operations-management-suite-oms-tooscope-management-solutions-toospecific-agents-preview"></a>Använda lösning mål i Operations Management Suite (OMS) tooscope lösningar toospecific hanteringsagenter (förhandsgranskning)
När du lägger till en lösning tooOMS distribueras den automatiskt med standard tooall Windows och Linux-agenter anslutna tooyour logganalys-arbetsytan.  Du kanske vill toomanage dina kostnader och begränsa hello mängden data som samlas in för en lösning genom att begränsa den tooa viss uppsättning agenter.  Den här artikeln beskriver hur toouse **lösning riktad** som är en OMS-funktion som gör att du tooapply ett scope tooyour lösningar.

## <a name="how-tootarget-a-solution"></a>Hur tootarget en lösning
Det finns tre steg tootargeting en lösning som beskrivs i följande avsnitt hello.  Observera att du måste både hello OMS-portalen och hello Azure-portalen för olika steg.


### <a name="1-create-a-computer-group"></a>1. Skapa en datorgrupp
Du anger hello datorer som du vill tooinclude i en omfattning genom att skapa en [datorgrupp](../log-analytics/log-analytics-computer-groups.md) i logganalys.  hello datorgrupp kan baserat på en logg sökning eller importeras från andra källor, till exempel Active Directory eller WSUS-grupper. Som [beskrivs nedan](#solutions-and-agents-that-cant-be-targeted), men endast datorer som är direkt anslutna tooLog Analytics ska inkluderas i hello omfång.

När du har hello datorgrupp som skapats i arbetsytan måste du inkludera den i en scope-konfiguration som kan vara tillämpade tooone eller fler lösningar.
 
 
 ### <a name="2-create-a-scope-configuration"></a>2. Skapa en scope-konfiguration
 En **konfigurationen** innehåller en eller flera datorgrupper och kan vara tillämpade tooone eller fler lösningar. 
 
 Skapa ett scope-konfigurationen med hjälp av hello följande process.  

 1. I hello Azure portal, navigerar för**logganalys** och välj din arbetsyta.
 2. I hello egenskaper för hello arbetsytan under **arbetsytan datakällor** Välj **omfång konfigurationer**.
 3. Klicka på **Lägg till** toocreate en ny konfiguration för scope.
 4. Ange en **namn** för konfiguration av hello scope.
 5. Klicka på **Markera datorgrupper**.
 6. Välj hello datorgrupp som du skapade och eventuellt andra grupper tooadd toohello konfiguration.  Klicka på **Välj**.  
 6. Klicka på **OK** toocreate hello konfigurationen. 


 ### <a name="3-apply-hello-scope-configuration-tooa-solution"></a>3. Tillämpa hello scope configuration tooa lösning.
När du har en scope-konfiguration, och du kan använda den tooone eller fler lösningar.  Observera att när en enda scope-konfiguration kan användas med flera lösningar, varje lösning kan bara använda en konfiguration för scope.

Tillämpa en scope-konfigurationen med hjälp av hello följande process.  

 1. I hello Azure portal, navigerar för**logganalys** och välj din arbetsyta.
 2. Markera i hello egenskaper för hello arbetsytan **lösningar**.
 3. Klicka på hello lösningen ska tooscope.
 4. I hello egenskaper för hello lösningen under **arbetsytan datakällor** Välj **lösning riktad**.  Om hello inte är tillgängligt sedan [den här lösningen kan inte riktas](#solutions-and-agents-that-cant-be-targeted).
 5. Klicka på **Lägg till konfigurationen**.  Om du redan har en konfiguration som tillämpas toothis lösning sedan otillgängligt det här alternativet.  Du måste ta bort hello befintlig konfiguration innan du lägger till en annan.
 6. Klicka på konfiguration av hello scope som du skapade.
 7. Titta på hello **Status** av hello configuration tooensure som den visar **lyckades**.  Om hello status anger ett fel, klicka på hello ellips toohello höger hello-konfiguration och välj **redigera konfigurationen** toomake ändringar.

## <a name="solutions-and-agents-that-cant-be-targeted"></a>Lösningar och agenter som inte vara mål
Följande är hello kriterier för agenter och lösningar som inte kan användas med lösningen som mål.

- Mål för lösning gäller endast toosolutions som distribuerar tooagents.
- Mål för lösning gäller endast toosolutions som tillhandahålls av Microsoft.  Den gäller inte toosolutions [skapas av dig själv eller partners](operations-management-suite-solutions-creating.md).
- Du kan bara filtrera ut agenter som ansluter direkt tooLog Analytics.  Lösningar distribuerar automatiskt tooany agenter som ingår i en ansluten hanteringsgrupp i Operations Manager om de finns med i en konfiguration för scope.

### <a name="exceptions"></a>Undantag
Mål för lösning kan inte användas med hello följande lösningar trots att de passar hello anges villkor.

- Bedömning av hälsotillstånd för Agent

## <a name="next-steps"></a>Nästa steg
- Mer information om lösningar inklusive hello-lösningar som är tillgängliga tooinstall i din miljö på [lägga till Azure Log Analytics management lösningar tooyour arbetsyta](../log-analytics/log-analytics-add-solutions.md).
- Lär dig mer om hur du skapar datorgrupper på [datorgrupper i logganalys logga sökningar](../log-analytics/log-analytics-computer-groups.md).