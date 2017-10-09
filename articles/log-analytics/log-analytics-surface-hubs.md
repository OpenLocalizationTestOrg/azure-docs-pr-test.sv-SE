---
title: aaaMonitor Surface Hub-enheter med Azure Log Analytics | Microsoft Docs
description: "Använd hello Surface Hub-lösningen tootrack hello hälsotillståndet för Surface Hub-enheter och förstå hur de används."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 8b4e56bc-2d4f-4648-a236-16e9e732ebef
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 623d30e749cafdd4a34ba0c5b3408164f1b4a95b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-surface-hubs-with-log-analytics-tootrack-their-health"></a>Övervaka Surface Hub-enheter med logganalys tootrack deras hälsa

![Visa symbol för hubben](./media/log-analytics-surface-hubs/surface-hub-symbol.png)

Den här artikeln beskriver hur du kan använda hello Surface Hub-lösning i logganalys toomonitor Microsoft Surface Hub-enheter med hello Microsoft Operations Management Suite (OMS). Logga Analytics kan du spåra hello hälsotillståndet för Surface Hub-enheter samt förstå hur de används.

Varje Surface Hub har hello Microsoft Monitoring Agent installeras. Dess via hello-agent som du kan skicka data från Surface Hub-tooOMS. Loggfiler läses från din Surface Hub-enheter och kan sedan skickas toohello OMS-tjänsten. Problem visas som servrar som är offline, hello kalender inte synkroniserar eller om hello enheten konto är toolog i Skype i OMS hello Surface Hub-instrumentpanelen. Du kan identifiera enheter som inte körs eller som är andra problem som potentiellt gäller korrigeringar för hello upptäckt problem med hjälp av hello data i hello instrumentpanelen.

## <a name="installing-and-configuring-hello-solution"></a>Installera och konfigurera hello lösning
Använd följande information tooinstall hello och konfigurera hello lösning. I ordning toomanage din Surface Hub-enheter från hello Microsoft Operations Management Suite (OMS), behöver du hello följande:

* En giltig prenumeration för[OMS](http://www.microsoft.com/oms).
* En [OMS prenumeration](https://azure.microsoft.com/pricing/details/log-analytics/) nivå som stöder hello antalet enheter som du vill toomonitor. Priser för OMS varierar beroende på hur många enheter registreras och hur mycket data den processer. Vill du tootake detta i åtanke när du planerar distributionen Surface Hub.

Därefter måste du antingen lägga till en OMS-prenumeration tooyour befintliga Microsoft Azure-prenumeration eller skapa en ny arbetsyta direkt via hello OMS-portalen. Detaljerade instruktioner för att använda någon av metoderna är på [Kom igång med logganalys](log-analytics-get-started.md). När hello OMS-prenumeration har ställts in, finns det två sätt tooenroll Surface Hub-enheter:

* Automatiskt via Intune
* Manuellt via **inställningar** på Surface Hub-enhet.

## <a name="set-up-monitoring"></a>Konfigurera övervakning
Du kan övervaka hello hälsotillståndet och aktiviteten hos din Surface Hub med logganalys i OMS. Du kan registrera hello Surface Hub i OMS med hjälp av Intune eller lokalt med hjälp av **inställningar** på hello Surface Hub.

## <a name="connect-surface-hubs-toooms-through-intune"></a>Ansluta tooOMS Surface Hub-enheter via Intune
Du måste ha hello arbetsyte-ID och arbetsytenyckel för hello OMS-arbetsyta som ska hantera Surface Hub-enheter. Du kan hämta dem från hello OMS-portalen.

Intune är en Microsoft-produkt som du kan använda toocentrally hantera hello OMS-konfigurationsinställningar som är kopplade tooone eller flera av dina enheter. Följ dessa steg tooconfigure dina enheter via Intune:

1. Logga in tooIntune.
2. Navigera för**inställningar** > **anslutna källor**.
3. Skapa eller redigera en princip baserat på hello Surface Hub-mallen.
4. Navigera toohello OMS (Azure-åtgärdsinformationen) avsnitt av hello princip och Lägg till hello *arbetsyte-ID* och *Arbetsytenyckel* toohello princip.
5. Spara hello princip.
6. Associera hello-princip med hello lämplig grupp av enheter.

   ![Intune-princip](./media/log-analytics-surface-hubs/intune.png)

Intune synkroniserar sedan hello OMS-inställningar med hello enheter i hello målgruppen registrera dem i OMS-arbetsyta.

## <a name="connect-surface-hubs-toooms-using-hello-settings-app"></a>Ansluta tooOMS för Surface Hub-enheter med hjälp av appen för hello-inställningar
Du måste ha hello arbetsyte-ID och arbetsytenyckel för hello OMS-arbetsyta som ska hantera Surface Hub-enheter. Du kan hämta dem från hello OMS-portalen.

Om du inte använder Intune toomanage din miljö, kan du registrera enheter manuellt via **inställningar** på varje Surface Hub:

1. Öppna från din Surface Hub **inställningar**.
2. Ange administratörsautentiseringsuppgifter i hello enheten när du uppmanas.
3. Klicka på **enheten**, och hello under **övervakning**, klickar du på **konfigurera inställningarna för OMS**.
4. Välj **aktivera övervakning**.
5. Ange hello hello OMS inställningar dialogen **arbetsyte-ID** och typen hello **Arbetsytenyckel**.  
   ![inställningar](./media/log-analytics-surface-hubs/settings.png)
6. Klicka på **OK** toocomplete hello konfiguration.

Om huruvida hello OMS-konfiguration har tillämpats toohello enheten visas en bekräftelse. Om den har visas ett meddelande om att hello-agent har anslutits toohello OMS-tjänsten. hello enheten börjar skicka data tooOMS där du kan visa och arbeta med den.

## <a name="monitor-surface-hubs"></a>Övervakare för Surface hub
Övervaka Surface Hub-enheter påminner med hjälp av OMS övervakning av andra registrerade enheter.

1. Logga in toohello OMS-portalen.
2. Navigera toohello Surface Hub-lösningen pack instrumentpanelen.
3. Enhetens hälsotillstånd visas.

   ![Ytan Hub-instrumentpanelen](./media/log-analytics-surface-hubs/surface-hub-dashboard.png)

Du kan skapa [aviseringar](log-analytics-alerts.md) baserat på befintliga eller anpassade loggen sökningar. Använda hello data hello OMS samlar in från Surface Hub-enheter kan söka du efter problem och avisering för hello villkor som du definierar för dina enheter.

## <a name="next-steps"></a>Nästa steg
* Använd [logga sökningar i logganalys](log-analytics-log-searches.md) tooview detaljerade Surface Hub-data.
* Skapa [aviseringar](log-analytics-alerts.md) toonotify när problem uppstår med Surface Hub-enheter.
