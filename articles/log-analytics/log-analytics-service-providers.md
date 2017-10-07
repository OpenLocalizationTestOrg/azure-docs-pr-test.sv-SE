---
title: "aaaLog analysfunktioner för tjänstleverantörer | Microsoft Docs"
description: "Logganalys hjälper hanteras tjänsteleverantörer (MSPs), stora företag oberoende programvara leverantörer (ISV) och värdleverantörer hantera och övervaka servrar i kundens lokala eller molninfrastruktur."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: jochan
editor: 
ms.assetid: c07f0b9f-ec37-480d-91ec-d9bcf6786464
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2016
ms.author: richrund
ms.openlocfilehash: 3c0a93232293f90385c6c724be436cee1cf855f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-features-for-service-providers"></a>Log Analytics funktioner för leverantörer
Logganalys hjälper hanterade tjänsteleverantörer (MSPs), stora företag, oberoende programvaruleverantörer (ISV) och värdleverantörer hantera och övervaka servrar i kundens lokala eller molninfrastruktur. 

Stora företag dela många likheter med leverantörer, särskilt när det finns en central IT-teamet som ansvarar för att hantera IT-avdelningen för många olika affärsenheter. För enkelhetens skull använder det här dokumentet hello termen *tjänstleverantör* men hello samma funktion är också tillgängligt för företag och andra kunder.

## <a name="cloud-solution-provider"></a>Cloud Solution Provider
Partners och leverantörer som ingår i hello [Cloud Solution Providers (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) programmet Log Analytics är en av hello Azure-tjänster på en CSP-prenumeration. 

För Log Analytics hello följande funktioner är aktiverade i *leverantör* prenumerationer.

Som en *leverantör* kan du:

* Skapa logganalys arbetsytor i prenumeration för en klient (kundens).
* Åtkomst arbetsytor som skapats av klienter. 
* Lägg till och ta bort användaren åtkomst toohello arbetsytan med hjälp av Azure användarhantering. När en klient arbetsytan i hello OMS portal hello användaren hanteringssidan under inställningar inte är tillgänglig
  * Log Analytics har inte stöd för rollbaserad åtkomst ännu -ge en användare `reader` i hello Azure-portalen tillåter dem toomake konfigurationsändringar i hello OMS-portalen

toolog i tooa innehavarens prenumeration behöver du toospecify hello klient identifierare. hello klient-ID är ofta den sista delen av hello e-postadress används toosign i.

* Hello OMS-portalen, lägga till `?tenant=contoso.com` i hello URL: en för hello-portalen. Till exempel, `mms.microsoft.com/?tenant=contoso.com`
* Använd hello i PowerShell `-Tenant contoso.com` parameter när du använder `Add-AzureRmAccount` cmdlet
* hello klient-ID läggs till automatiskt när du använder hello `OMS portal` länk från hello Azure portal tooopen och logga in toohello OMS-portalen för hello valda arbetsytan

Som en *kunden* av en leverantör kan du:

* Skapa log analytics arbetsytor i en CSP-prenumeration
* Åtkomst arbetsytor som skapats av hello CSP
  * Använd hello `OMS portal` länk från hello Azure portal tooopen och logga in toohello OMS-portalen för hello valda arbetsytan
* Visa och använda hello management användarsidan under inställningar i hello OMS-portalen

> [!NOTE]
> hello ingår säkerhetskopiering och Site Recovery-lösningar för Log Analytics är inte kan tooconnect tooa Recovery Services-valvet och inte kan konfigureras i en CSP-prenumeration. 
> 
> 

## <a name="managing-multiple-customers-using-log-analytics"></a>Hantera flera kunder som använder logganalys
Vi rekommenderar att du skapar en logganalys-arbetsytan för varje kund som du hanterar. Logganalys-arbetsytan innehåller:

* En geografisk plats för data toobe lagras. 
* Precision för fakturering 
* Dataisolering 
* Unik konfiguration

Genom att skapa en arbetsyta per kund kan du kan tookeep varje kund data separat och även spåra hello användning av varje kund.

Mer information om när och varför toocreate flera arbetsytor beskrivs i [hantera åtkomst toolog analytics](log-analytics-manage-access.md#determine-the-number-of-workspaces-you-need).

Skapa och konfiguration av customer arbetsytor kan automatiseras med hjälp av [PowerShell](log-analytics-powershell-workspace-configuration.md), [Resource Manager-mallar](log-analytics-template-workspace-configuration.md), eller med hjälp av hello [REST API](https://www.nuget.org/packages/Microsoft.Azure.Management.OperationalInsights/).

hello kan med Resource Manager-mallar för arbetsytan konfiguration du toohave en master konfiguration som kan använda toocreate och konfigurera arbetsytor. Du kan vara säker på att som arbetsytor skapas för kunder som de är automatiskt konfigurerade tooyour krav. När du uppdaterar dina krav hello mallen uppdateras och sedan in igen hello befintliga arbetsytor. Den här processen säkerställer att även befintliga arbetsytor uppfyller dina nya.    

När du hanterar flera logganalys arbetsytor, rekommenderar vi integrerar varje arbetsyta med din befintliga biljettsystem / operations-konsolen använder hello [aviseringar](log-analytics-alerts.md) funktioner. Genom att integrera med befintliga system kan supportpersonal toofollow fortsätta sina bekant processer. Logganalys regelbundet kontrollerar varje arbetsyta mot hello aviseringsvillkoren som du anger och genererar en avisering när åtgärd krävs.

För anpassade vyer av data använder du hello [instrumentpanelen](../azure-portal/azure-portal-dashboards.md) funktionen hello Azure-portalen.  

För verkställande nivå rapporter som summerar data över arbetsytor som du kan använda hello integrering mellan logganalys och [PowerBI](log-analytics-powerbi.md). Om du behöver toointegrate med ett annat reporting system, kan du använda hello Sök-API (via PowerShell eller [REST](log-analytics-log-search-api.md)) toorun frågor och exportera sökresultat.

## <a name="next-steps"></a>Nästa steg
* Automatisera skapandet och konfiguration av arbetsytor med [Resource Manager-mallar](log-analytics-template-workspace-configuration.md)
* Automatisera genereringen av arbetsytor med [PowerShell](log-analytics-powershell-workspace-configuration.md) 
* Använd [aviseringar](log-analytics-alerts.md) toointegrate med befintliga system
* Generera sammanfattningsrapporter med [PowerBI](log-analytics-powerbi.md)

