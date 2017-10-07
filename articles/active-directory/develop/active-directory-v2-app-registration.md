---
title: "aaaRegister ett program med hello Azure AD v2.0-slutpunkten med hjälp av hello portal | Microsoft Docs"
description: "Hur tooregister en app med Microsoft för att aktivera inloggning och åtkomst till Microsoft tjänster som använder hello v2.0-slutpunkten"
services: active-directory
documentationcenter: 
author: lnalepa
manager: mbaldwin
editor: 
ms.assetid: bb2f701f-3bc3-4759-94a5-8b9d53a8a0b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: lenalepa
ms.custom: aaddev
ms.openlocfilehash: c56c98906656062435516e820cb318a04c03149c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooregister-an-app-with-hello-v20-endpoint"></a>Hur tooregister en app med hello v2.0-slutpunkten
toobuild en app som accepterar både MSA och Azure AD-inloggning, behöver du tooregister en app med Microsoft.  För tillfället inte att kunna toouse alla befintliga appar som du kan ha med Azure AD eller MSA - du behöver toocreate varumärken ny.

> [!NOTE]
> Inte alla Azure Active Directory-scenarier och funktioner som stöds av hello v2.0-slutpunkten.  toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).
> 
> 

## <a name="visit-hello-microsoft-app-registration-portal"></a>Besök portalen för registrering av hello Microsoft app
Första saker först - navigera för[https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Det här är en ny app registrering portal där du kan hantera dina Microsoft-appar.

Logga in med antingen en personlig eller arbets- eller skolkonto för Microsoft.  Om du inte har antingen registrera dig för ett nytt personligt konto. Gå vidare, det tar inte lång - vi väntar här.

Vill du göra? Du bör nu tittar på listan över Microsoft-appar som är förmodligen tom.  Ska vi ändra.

Klicka på **Lägg till en app**, och ge det ett namn.  hello portal tilldelar appen ett globalt unikt Id för programmet som du vill använda senare i koden.  Om appen innehåller en komponent på serversidan som behöver åtkomst-token för anropa API: er (tror: Office, Azure eller egna webb-API), ska du toocreate en **Programhemlighet** även här.

Lägg till hello plattformar som ska använda för din app.

* Webbaserade appar, ange en **omdirigerings-URI** där inloggning meddelanden kan skickas.
* För mobila appar kopiera hello standard omdirigerings-uri som skapats automatiskt åt dig.

Du kan också kan du anpassa hello utseendet på inloggningssidan i hello-profilen.  Se till att tooclick **spara** innan du fortsätter.

> [!NOTE]
> När du skapar ett program som använder [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), hello program registreras i hello hem innehavare för hello kontot du använder toosign i hello-portalen.  Det innebär att du inte registrera ett program i Azure AD-klienten med ett personligt microsoftkonto.  Om du uttryckligen vill tooregister ett program i en viss klient måste du logga in med ett konto som ursprungligen skapades i den klienten.
> 
> 

## <a name="build-a-quick-start-app"></a>Skapa en app för Snabbstart
Nu när du har en Microsoft-app kan du slutföra en av våra självstudier för v2.0-Snabbstart.  Här är några rekommendationer:

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

