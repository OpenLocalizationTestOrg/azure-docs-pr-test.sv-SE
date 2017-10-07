---
title: "aaaNo prenumerationer påträffade fel när försöker toosign i tooAzure portal eller Azure kontocenter | Microsoft Docs"
description: "Är hello lösning på problemet som inga prenumerationer hittades fel uppstår när loggar in tooAzure portalen eller Azure kontocenter."
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.assetid: d1545298-99db-4941-8e97-f24a06bb7cb6
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: genli
ms.openlocfilehash: def4d4a1f883dd948fe8132f2d85abc4c23ae624
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="no-subscriptions-found-error-in-azure-portal-or-azure-account-center"></a>Inga prenumerationer hittades fel i Azure-portalen eller Azure-kontocenter
Du kan få ett meddelande om ”inga prenumerationer hittades” när du försöker toosign i toohello [Azure-portalen](https://portal.azure.com/) eller hello [Azure kontocenter](https://account.windowsazure.com/Subscriptions). Den här artikeln innehåller en lösning på problemet.

## <a name="symptom"></a>Symtom

När du försöker toosign i toohello [Azure-portalen](https://portal.azure.com/) eller hello [Azure kontocenter](https://account.windowsazure.com/Subscriptions), du får följande felmeddelande hello: ”inga prenumerationer hittades”.

## <a name="cause"></a>Orsak

Det här problemet uppstår om ditt konto inte har tillräcklig behörighet. 

## <a name="solution"></a>Lösning

Kontrollera att du logga in som administratör för hello korrekt. En kontoadministratör kan komma åt endast hello Account Center. Tjänsten administratörer (SA) och Medadministratörer (CA) har åtkomst behörighet endast toohello Azure-portalen eller hello klassiska Azure-portalen.

### <a name="scenario-1-error-message-is-received-in-hello-azure-portalhttpsportalazurecom"></a>Scenario 1: Meddelande tas emot i hello [Azure-portalen](https://portal.azure.com)

toofix problemet:

* Kontrollera att rätt Azure directory väljs genom att klicka på ditt konto på hello uppe till höger hello.

  ![Välj hello katalog vid hello uppifrån höger om hello Azure-portalen](./media/billing-no-subscriptions-found/directory-switch.png)

* Om hello höger Azure-katalogen har valts men fortfarande felmeddelande hello, [ditt konto som ägare](billing-add-change-azure-subscription-administrator.md).

### <a name="scenario-2-error-message-is-received-in-hello-azure-account-centerhttpsaccountwindowsazurecomsubscriptions"></a>Scenario 2: Felmeddelande tas emot i hello [Azure kontocenter](https://account.windowsazure.com/Subscriptions)

Kontrollera om hello-konto som du använde hello kontoadministratör. tooverify som hello kontoadministratör, gör du följande:

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. På navmenyn hello väljer **prenumeration**.
3. Välj hello prenumeration som du vill toocheck och välj sedan **inställningar**.
4. Välj **egenskaper**. hello kontoadministratör för hello prenumerationen visas i hello **kontoadministratören** rutan.

## <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten.
Om du fortfarande behöver hjälp [supporten](http://go.microsoft.com/fwlink/?linkid=544831&clcid=0x409) tooget problemet lösas snabbt. 
