---
title: aaaAccess Azure AD App Proxy appar i team | Microsoft Docs
description: "Använda Azure AD Application Proxy tooaccess lokalt programmet via Microsoft-Teams."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 13c36e43ae6349df09272e308ad4f40451cbbeb9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="access-your-on-premises-applications-through-microsoft-teams"></a>Åtkomst till lokala program via Microsoft-Teams

Azure Active Directory Application Proxy ger enkel inloggning tooon lokala program oavsett var du befinner dig och Microsoft-Teams förenklar samverkande arbetet på ett ställe. Integrera hello innebär två tillsammans att användarna kan vara produktiva med sina medarbetare i en situation. 

Användarna kan lägga till molnet appar tootheir team kanaler [med hjälp av flikarna](https://support.office.com/article/Video-Using-Tabs-7350a03e-017a-4a00-a6ae-1c9fe8c497b3?ui=en-US&rs=en-US&ad=US), men vad händer om SharePoint-webbplatsen eller alla använder Verktyg för planering finns lokalt? Application Proxy är hello lösning. De kan lägga till appar som publicerats via Application Proxy tootheir hello kanaler som använder samma externa URL: er använder de alltid tooaccess sina appar via fjärranslutning. Och eftersom Application Proxy som autentiserar via Azure Active Directory, hello samma enkel inloggning utför via.


## <a name="install-hello-application-proxy-connector-and-publish-your-app"></a>Installera hello Application Proxy connector och publicera en app

Om du inte redan har gjort [konfigurera programproxyn för din klient och installera hello connector](active-directory-application-proxy-enable.md). Sedan [publicera programmet lokalt](application-proxy-publish-azure-portal.md) för fjärråtkomst. När du publicerar hello app, anteckna hello extern URL eftersom dina slutanvändare behöver informationen när de lägger till hello app tooTeams.

Om du redan har dina appar som publiceras men inte kommer ihåg sina externa URL: er, söker du efter dem i hello [Azure-portalen](https://portal.azure.com). Logga in och sedan navigera för**Azure Active Directory** > **företagsprogram** > **alla program** > Välj appen > **Programproxy**.

## <a name="add-your-app-tooteams"></a>Lägg till app-tooTeams

När du publicerar hello appen via Application Proxy kan du låta dina användare veta att de kan lägga till den som en flik direkt i deras team-kanaler. Ha dem följande tre steg:

1. Navigera toohello team kanaler där du vill att tooadd den här appen och markera  **+**  tooadd en flik.

   ![Välj Lägg till en flik](./media/application-proxy-teams/add-tab.png)

2. Välj **webbplats** från hello alternativ på fliken.

   ![Lägga till en webbplats](./media/application-proxy-teams/website.png)

3. Namnge hello-fliken och ange hello URL toohello Application Proxy externa URL: en. 

   ![Konfigurera fliknamn och en URL](./media/application-proxy-teams/tab-name-url.png)

När en medlem i ett team lägger hello-fliken, visas den för alla hello-kanal. Alla användare som har åtkomst toohello app hämta enkel inloggning med hello autentiseringsuppgifter de använder för Microsoft Teams. Alla användare som inte har åtkomst toohello app hello-fliken i team visas, men är spärrat tills du ge dem behörighet toohello lokalt app och hello Azure portal publicerade versionen av hello app. 

## <a name="next-steps"></a>Nästa steg

- Lär dig hur för[publicera lokala SharePoint-webbplatser](application-proxy-enable-remote-access-sharepoint.md) med Application Proxy.
- Konfigurera din apps toouse [anpassade domäner](active-directory-application-proxy-custom-domains.md) för sina externa URL: en. 
