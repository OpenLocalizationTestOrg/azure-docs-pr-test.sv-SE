---
title: "Åtkomst- och rapporter för Azure MFA | Microsoft Docs"
description: "Här beskrivs hur du använder funktionen Azure Multi-Factor Authentication - rapporter."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: curtand
ms.assetid: 3f6b33c4-04c8-47d4-aecb-aa39a61c4189
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: kgremban
ms.openlocfilehash: f76e726c6a67de4b0472c0e97f9e72c31c14c4f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Rapporter i Azure Multi-Factor Authentication
Azure Multi-Factor Authentication innehåller flera rapporter som kan användas av du och din organisation. De här rapporterna kan nås via Multi-Factor Authentication-hanteringsportalen. Följande är en lista över tillgängliga rapporter:

| Rapport | Beskrivning |
|:--- |:--- |
| Användning |Användningen rapporter visar information om övergripande användning, Användarsammanfattning och användarinformation. |
| Serverstatus |Den här rapporten visar statusen för multi-Factor Authentication-servrar som är kopplad till ditt konto. |
| Historik över blockerad användare |Rapporterna Visar historik över förfrågningar om att blockera eller avblockera användare. |
| Historik över åsidosatt användare |Visar historiken över förfrågningar om att koppla förbi Multi-Factor Authentication för en användares telefonnummer. |
| Bedrägerivarning |Visar en historik över bedrägerivarningar som skickats inom det angivna datumintervallet. |
| I kö |Visar rapporter i kö för bearbetning och deras status. En länk för att hämta eller visa rapporten tillhandahålls när rapporten är färdig. |

## <a name="view-reports"></a>Visa rapporter
1. Logga in på den [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Välj Active Directory till vänster.
3. Gör något av följande två alternativ, beroende på om du använder Autentiseringsleverantörer:
   * **Alternativ 1**: Klicka på fliken Flerfunktionsautentiseringsleverantörer. Välj din MFA-leverantör och klicka på den **hantera** längst ned.
   * **Alternativ 2**: Välj din katalog och gå till den **konfigurera** fliken. Välj **Hantera tjänstinställningar** i avsnittet för multifaktorautentisering. Klicka på Gå till portal-länken längst ned på sidan för MFA inställningar.
4. Välj typ av rapport som du vill använda från i Azure Multi-Factor Authentication-hanteringsportalen på **visa en rapport** avsnitt i det vänstra navigeringsfönstret.

<center>![Moln](./media/multi-factor-authentication-manage-reports/report.png)</center>


**Ytterligare resurser**

* [För användare](end-user/multi-factor-authentication-end-user.md)
* [Azure Multi-Factor Authentication på MSDN](https://msdn.microsoft.com/library/azure/dn249471.aspx)
