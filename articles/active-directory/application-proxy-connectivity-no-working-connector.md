---
title: "aaaNo koppling arbetsgruppen hittades för ett program med Application Proxy | Microsoft Docs"
description: "Åtgärda problem som kan uppstå när det finns ingen aktiv anslutning i en grupp för anslutningen för ditt program med hello Azure AD Application Proxy"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 4c4baf296b316db131929c9a7c618fb9960713e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="no-working-connector-group-found-for-an-application-proxy-application"></a>Ingen koppling arbetsgruppen hittades för ett program med Application Proxy

Den här artikeln hjälpen du tooresolve hello vanliga problem inför när det inte är en koppling upptäcktes för ett program med Application Proxy integrerat med Azure Active Directory.

## <a name="overview-of-steps"></a>Översikt över steg
Om det finns ingen aktiv anslutning i en grupp för kopplingen för programmet, finns det ett par sätt tooresolve hello problemet:

-   Om du har inga kopplingar i hello grupp, kan du:

    -   Hämta en ny koppling på hello rätt lokal server och tilldela den toothis grupp

    -   Flytta en aktiv koppling till hello grupp

-   Om du har inga aktiva kopplingar i hello grupp, kan du:

    -   Identifiera hello orsak din anslutning är inaktiv och lösa

    -   Flytta en aktiv koppling till hello grupp

tooknow som dessa är hello problemet öppna hello ”Application Proxy”-menyn i ditt program och titta på hello Connector grupp varningsmeddelandet. Det anger du antingen hello gruppen måste minst en koppling (du har ingen i hello grupp) eller att det finns inga aktiva anslutningar (även om du har förmodligen inaktiva kopplingar).

   ![Val av anslutningen i Azure Portal](./media/application-proxy-connectivity-no-working-connector/no-active-connector.png)

Mer information om de här alternativen finns i avsnittet hello motsvarande nedan. Var och en av dessa förutsätter att du startar från hanteringssidan om hello-anslutningen. Om du tittar på hello felmeddelandet ovan, fortsätter du toothis sidan genom att klicka på hello varningsmeddelandet. Annars det hittar du genom att gå för**Azure Active Directory**, klicka på **företagsprogram**, sedan **Application Proxy.**

   ![Kopplingen grupphantering i Azure Portal](./media/application-proxy-connectivity-no-working-connector/app-proxy.png)

## <a name="download-a-new-connector"></a>Hämta en ny koppling

toodownload en ny koppling, Använd hello ”hämta anslutning” knappen hello överst på hello sidan.

Obs hello Connector behov toobe installeras på en dator med fri toohello backend-programmet och vanligtvis är placerad hello samma server som hello program. När du har hämtat, hello anslutningen ska visas i den här menyn. klickar hello koppling och använder hello ”Connector grupp” nedrullningsbara toomake att toohello rätt gruppen tillhör. Spara hello ändringen.

   ![Hämta hello koppling från hello Azure-portalen](./media/application-proxy-connectivity-no-working-connector/download-connector.png)
   
## <a name="move-an-active-connector"></a>Flytta en aktiv koppling

Om du har en aktiv koppling som ska tillhöra toohello grupp och har fri toohello målprogrammet backend kan du flytta hello Connector till hello som tilldelats gruppen. toodo Klicka på hello Connector. Använda hello nedrullningsbara tooselect hello rätt grupp i hello ”Connector” fältet och klickar du på Spara.

## <a name="resolve-an-inactive-connector"></a>Lösa en inaktiv anslutning

Om hello endast kopplingar i hello grupp är inaktiva de troligen på en dator som inte har alla nödvändiga portar för hello avblockerad.

Se hello portar felsöka dokument för information om undersöker problemet.

## <a name="next-steps"></a>Nästa steg
[Förstå Azure AD Application Proxy-kopplingar](application-proxy-understand-connectors.md)


