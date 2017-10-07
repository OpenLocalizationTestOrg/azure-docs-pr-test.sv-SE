---
title: "aaaHow hanterar användarkonton i Azure API Management | Microsoft Docs"
description: "Lär dig hur toocreate eller bjuda in användare i Azure API Management"
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 078abfa5-1e4f-4c9d-b9c7-a172bd19c1a2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 3966f4454e29621d7c615beefee352ec91b48b2e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-user-accounts-in-azure-api-management"></a>Hur toomanage användarkonton i Azure API Management
Utvecklare finns i API Management hello användare av hello API: er som du exponera med hjälp av API-hantering. Den här guiden visar toohow toocreate och inbjudan utvecklare toouse hello API: er och produkter som du gör tillgängliga toothem med din API Management-instans. Information om hur du hanterar användarkonton programmässigt finns hello [användarentiteten](https://msdn.microsoft.com/library/azure/dn776330.aspx) dokumentation i hello [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) referens.

## <a name="create-developer"></a>Skapa en ny utvecklare
toocreate nya utvecklare, klickar du på **Publisher portal** i hello Azure-portalen för API Management-tjänsten. Då kommer du toohello API Management publisher portal. Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management] [ Get started with Azure API Management] kursen.

![Utgivarportalen][api-management-management-console]

Klicka på **användare** från hello **API Management** menyn på hello vänster och klicka sedan på **lägga till användare**.

![Skapa utvecklare][api-management-create-developer]

Ange hello **e-post**, **lösenord**, och **namn** för nya hello-utvecklare och klickar på **spara**.

![Skapa utvecklare][api-management-add-new-user]

Nyligen skapade developer konton är som standard **Active**, och som är associerade med hello **utvecklare** grupp.

![Nya utvecklare][api-management-new-developer]

Utvecklare konton som är i ett **active** tillstånd kan vara används tooaccess alla hello-API: er som de har prenumerationer. tooassociate hello nyskapad utvecklare med ytterligare grupper, se [hur tooassociate grupper med utvecklare][How tooassociate groups with developers].

## <a name="invite-developer"></a>Bjuda in en utvecklare
tooinvite utvecklare, klickar du på **användare** från hello **API Management** menyn på hello vänster och klicka sedan på **bjuda in användare**.

![Bjud in utvecklare][api-management-invite-developer]

Ange hello namn och e-postadress hello utvecklare och klicka på **bjuda in**.

![Bjud in utvecklare][api-management-invite-developer-window]

Ett meddelande visas, men hello nyligen inbjuden developer visas inte i hello listan förrän efter att de accepterar hello inbjudan. 

![Bjud in bekräftelse][api-management-invite-developer-confirmation]

När du uppmanas att utvecklare skickas ett e-postmeddelande toohello utvecklare. Den här e-post skapas med en mall och anpassas. Mer information finns i [Konfigurera e-postmallar][Configure email templates].

Hello konto blir aktiv när hello inbjudan har accepterats.

## <a name="block-developer"></a> Inaktivera eller återaktivera ett utvecklarkonto
Som standard nyskapade eller inbjudna developer konton är **Active**. toodeactivate ett utvecklarkonto klickar du på **Block**. tooreactivate blockerade utvecklarkonto klickar du på **aktivera**. Blockerade utvecklarkonto kan inte komma åt hello developer-portalen eller anropa alla API: er. toodelete ett användarkonto, klickar du på **ta bort**.

![Block-utvecklare][api-management-new-developer]

## <a name="reset-a-user-password"></a>Återställa en användarlösenord
tooreset hello lösenordet för ett användarkonto, klickar du på hello hello-kontot.

![Återställa lösenord][api-management-view-developer]

Klicka på **Återställ lösenord** toosend en länk toohello användaren tooreset sitt lösenord.

![Återställa lösenord][api-management-reset-password]

tooprogrammatically fungerar med användarkonton, se hello [användarentiteten](https://msdn.microsoft.com/library/azure/dn776330.aspx) dokumentation i hello [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) referens. tooreset ett användarens konto lösenord tooa specifikt värde, kan du använda hello [uppdatera en användare](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) igen och ange önskade hello-lösenord.

## <a name="pending-verification"></a>Väntande verifiering
![Väntande verifiering][api-management-pending-verification]

## <a name="next-steps"> </a>Nästa steg
När ett utvecklarkonto har skapats kan du koppla den till roller och prenumerera på den tooproducts och API: er. Mer information finns i [hur toocreate och Använd][How toocreate and use groups].

[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png


[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[How toocreate and use groups]: api-management-howto-create-groups.md
[How tooassociate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Configure email templates]: api-management-howto-configure-notifications.md#email-templates
