---
title: "aaaAccess- och användningsdata rapporter för Azure MFA | Microsoft Docs"
description: "Här beskrivs hur toouse hello Azure Multi-Factor Authentication-funktionen - rapporter."
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
ms.openlocfilehash: ae7ccceca4968d7ec7cf0cb1cf9e041d9997c840
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Rapporter i Azure Multi-Factor Authentication
Azure Multi-Factor Authentication innehåller flera rapporter som kan användas av du och din organisation. De här rapporterna kan nås via hello Multi-Factor Authentication-hanteringsportalen. hello nedan följer en lista över tillgängliga hello-rapporter:

| Rapport | Beskrivning |
|:--- |:--- |
| Användning |hello användning rapporter visar information om övergripande användning, Användarsammanfattning och användarinformation. |
| Serverstatus |Den här rapporten visar hello status för multi-Factor Authentication-servrar som är kopplad till ditt konto. |
| Historik över blockerad användare |Rapporterna visar hello historik över förfrågningar tooblock eller avblockera användare. |
| Historik över åsidosatt användare |Visar hello historik över förfrågningar toobypass Multifaktorautentisering för en användares telefonnummer. |
| Bedrägerivarning |Visar en historik över bedrägerivarningar som skickats under hello angivna datumintervallet. |
| I kö |Visar rapporter i kö för bearbetning och deras status. En toodownload eller visa hello rapport tillhandahålls när rapporten hello är klar. |

## <a name="view-reports"></a>Visa rapporter
1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Välj Active Directory hello vänster.
3. Gör något av följande två alternativ, beroende på om du använder Autentiseringsleverantörer:
   * **Alternativ 1**: hello Flerfunktionsautentiseringsleverantörer fliken. Välj din MFA-leverantör och klicka på hello **hantera** knappen längst ned hello.
   * **Alternativ 2**: Välj din katalog och gå toohello **konfigurera** fliken. Markera under hello multifaktorautentisering avsnittet **hantera tjänstinställningar**. Hello längst ned på sidan för hello MFA inställningar klickar du på hello gå toohello portal-länken.
4. Välj hello typ av rapport som du vill använda från hello i hello Azure Multi-Factor Authentication-hanteringsportalen, **visa en rapport** avsnitt i hello vänstra navigeringsrutan.

<center>![Moln](./media/multi-factor-authentication-manage-reports/report.png)</center>


**Ytterligare resurser**

* [För användare](end-user/multi-factor-authentication-end-user.md)
* [Azure Multi-Factor Authentication på MSDN](https://msdn.microsoft.com/library/azure/dn249471.aspx)
