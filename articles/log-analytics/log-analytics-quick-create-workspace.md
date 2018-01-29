---
title: Skapa en arbetsyta i Azure Log Analytics | Microsoft Docs
description: "Lär dig hur du skapar en logganalys-arbetsytan om du vill aktivera hantering av lösningar och datainsamling från dina molntjänster och lokala miljöer."
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2017
ms.author: magoedte
ms.openlocfilehash: 8259a97d28effa7bfa9cfb9d7cd9cd2a14c9d906
ms.sourcegitcommit: c50171c9f28881ed3ac33100c2ea82a17bfedbff
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/26/2017
---
# <a name="create-a-log-analytics-workspace-in-the-azure-portal"></a>Skapa en logganalys-arbetsyta i Azure-portalen
I Azure-portalen kan du ställa in en logganalys-arbetsyta som är en unik logganalys-miljö med en egen lagringsplats för data, datakällor och lösningar.  Stegen som beskrivs i den här artikeln krävs om du avser att samla in data från följande källor:

* Azure-resurser i din prenumeration
* Lokala datorer som övervakas av System Center Operations Manager
* Enhetssamlingar från System Center Configuration Manager 
* Diagnostik- eller loggdata från Azure Storage

Andra källor, till exempel virtuella Azure-datorer och Windows- eller Linux-datorer i din miljö finns i följande avsnitt:

*  [Samla in data om virtuella datorer i Azure](log-analytics-quick-collect-azurevm.md) 
*  [Samla in data om Linux-datorer](log-analytics-quick-collect-linux-computer.md)
*  [Samla in data om Windows-datorer](log-analytics-quick-collect-windows-computer.md)

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

## <a name="log-in-to-azure-portal"></a>Logga in på Azure-portalen
Logga in på Azure-portalen på [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-workspace"></a>Skapa en arbetsyta
1. I Azure-portalen klickar du på **fler tjänster** hittades i det nedre vänstra hörnet. I listan över resurser skriver du **Log Analytics**. När du börjar skriva filtreras listan baserat på det du skriver. Välj **logga Analytics**.<br><br> ![Azure Portal](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
2. Klicka på **skapa**, och välj sedan alternativ för följande objekt:

  * Ange ett namn för den nya **OMS-arbetsytan**, som *DefaultLAWorkspace*. 
  * Välj en **prenumeration** att länka till genom att välja från den listrutan om standardvalet inte är lämpligt.
  * För **resursgruppen**, väljer att använda en befintlig resurs grupp redan installationen eller skapa en ny.  
  * Välj ett tillgängligt **plats**.  Mer information finns i som [regioner Log Analytics är tillgängligt i](https://azure.microsoft.com/regions/services/).
  * Du kan välja mellan tre olika **prisnivåer** i logganalys, men för denna Snabbstart som du ska välja den **ledigt** nivå.  Mer information om de specifika nivåerna finns [Log Analytics-prisinformation](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Create Log Analytics resource blade](media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-01.png)<br>  
3. När du har angett informationen som krävs på den **OMS-arbetsytan** rutan klickar du på **OK**.  

När informationen har verifierats och arbetsytan skapas, kan du spåra förloppet under **Meddelanden** på menyn. 

## <a name="next-steps"></a>Nästa steg
Nu när du har en arbetsyta tillgängliga du konfigurera mängd övervakning telemetri, kör loggen sökningar för att analysera data och Lägg till en lösning för att tillhandahålla ytterligare data och analytiska insikter. 

* Om du vill aktivera insamling av data från Azure-resurser med Azure-diagnostik eller Azure storage finns [samla in Azure tjänstloggar och mått för användning i logganalys](log-analytics-azure-storage.md).  
* [Lägg till System Center Operations Manager som en datakälla](log-analytics-om-agents.md) samla in data från agenter som rapporterar din Operations Manager-hanteringsgrupp och lagra den på din databas för logganalys-arbetsytan. 
* Ansluta [Configuration Manager](log-analytics-sccm.md) att importera datorer som är medlemmar av samlingar i hierarkin.  
* Granska de [hanteringslösningar](/log-analytics-add-solutions.md) tillgängliga och lägga till eller ta bort en lösning från arbetsytan.

