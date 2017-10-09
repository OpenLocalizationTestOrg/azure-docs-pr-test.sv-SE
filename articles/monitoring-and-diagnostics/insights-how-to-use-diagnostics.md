---
title: "aaaEnable övervakning och diagnostik i Microsoft Azure | Microsoft Docs"
description: "Lär dig hur tooset in diagnostik för dina resurser i Azure."
author: rboucher
manager: carolz
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: af1947a9-c211-4aa1-8924-880a86240be4
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: robb
ms.openlocfilehash: 865d010c5846fff6d871e20eca2bc4ac35028354
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-monitoring-and-diagnostics"></a>Aktivera övervakning och diagnostik
I hello [Azure Portal](https://portal.azure.com), kan du konfigurera omfattande, frekventa, övervakning och diagnostik data om dina resurser. Du kan också använda hello [REST API](https://msdn.microsoft.com/library/azure/dn931932.aspx) eller [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) tooconfigure diagnostik programmässigt.

Diagnostik, övervaka och mått sparas i Azure i ett lagringskonto som du väljer. Detta gör att du toouse oavsett tooling du vill tooread hello data från en Lagringsutforskaren tooPower BI toothird parts verktygsuppsättning.

## <a name="when-you-create-a-resource"></a>När du skapar en resurs
De flesta tjänster kan du tooenable diagnostik när du skapar dem i hello [Azure Portal](https://portal.azure.com).

1. Gå för**ny** och välja hello-resurs som du är intresserad av.
2. Välj **valfri konfiguration**.
    ![Diagnostik-bladet](./media/insights-how-to-use-diagnostics/Insights_CreateTime.png)
3. Välj **diagnostik**, och klicka på **på**. Du behöver toochoose hello Storage-konto som du vill diagnostik toobe sparas. Du debiteras normal datataxa för lagring och transaktioner när du skickar diagnostik tooa storage-konto.
4. Klicka på **OK** och skapa hello resurs.

## <a name="change-settings-for-an-existing-resource"></a>Ändra inställningarna för en befintlig resurs
Om du redan har skapat en resurs och du vill toochange hello diagnostikinställningarna (toochange hello-nivå för insamling av data, till exempel), gör du det direkt i hello Azure-portalen.

1. Gå toohello resursen och klicka på hello **inställningar** kommando.
2. Välj **diagnostik**.
3. Hej **diagnostik** bladet innehåller alla möjliga hello-diagnostik och samling övervakningsdata för resursen. För vissa resurser kan du också välja en **kvarhållning** princip för hello data, tooclean det upp från ditt lagringskonto.
    ![Lagring](./media/insights-how-to-use-diagnostics/Insights_StorageDiagnostics.png)
4. När du har valt dina inställningar klickar du på hello **spara** kommando. Det kan ta lite medan för att övervaka tooshow data in om du aktiverar det för hello första gången.

### <a name="categories-of-data-collection-for-virtual-machines"></a>Kategorier av datainsamling för virtuella datorer
För virtuella datorer registreras alla mått och loggar en minut regelbundet, så att du alltid hello de flesta uppdaterad information om din dator.

* **Grundläggande mått** : hälsotillstånd mått om den virtuella datorn, till exempel processor och minne
* **Nätverks- och webbmått** : mått om nätverksanslutningar och webbtjänster
* **.NET mått** : mått om hello-.NET- och ASP.NET-program som körs på den virtuella datorn
* **SQL-mått** : Om du använder Microsoft SQL-tjänsten, dess prestandamått
* **Windows-händelseprogramloggar** : Windows-händelser som skickas toohello application kanal
* **Windows-händelsesystemloggar** : Windows-händelser som skickas toohello systemkanal. Detta omfattar även alla händelser från [Microsoft Antimalware](http://go.microsoft.com/fwlink/?LinkID=404171&clcid=0x409).
* **Windows-händelsesäkerhetsloggar** : Windows-händelser som skickas toohello säkerhetskanal
* **Diagnostik infrastruktur loggar** : loggning om hello diagnostik samling infrastruktur
* **IIS-loggar** : loggar om IIS-servern

Observera att just nu stöds inte vissa distributioner av Linux och, hello Gästagenten måste vara installerad på hello virtuell dator.

## <a name="next-steps"></a>Nästa steg
* [Få aviseringar](insights-receive-alert-notifications.md) när drifthändelser inträffar eller när mätvärden överskrider ett tröskelvärde.
* [Övervaka tjänsten](insights-how-to-customize-monitoring.md) toomake att tjänsten är tillgänglig och svarstid.
* [Skala instansantalet automatiskt](insights-how-to-scale.md) toomake att skala din tjänst baserat på begäran.
* [Övervaka programprestanda](../application-insights/app-insights-azure-web-apps.md) om du vill toounderstand exakt hur din kod genomför i hello molnet.
* [Visa händelser och aktivitetsloggen](insights-debugging-with-events.md) toolearn allt som har skett i din tjänst.
* [Spåra tjänstens hälsa](insights-service-health.md) toofind ut när Azure har uppstått prestanda försämras eller tjänst avbrott.

