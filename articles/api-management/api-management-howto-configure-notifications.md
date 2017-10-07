---
title: aaaConfigure meddelanden och e-mallar i Azure API Management | Microsoft Docs
description: "Lär dig hur tooconfigure meddelanden och e-mallar i Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: ee25f26d-4752-433b-af9c-3817db38aed5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: dc23289c25a1641992b73cb955099b3f207b6968
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-notifications-and-email-templates-in-azure-api-management"></a>Hur tooconfigure meddelanden och e-mallar i Azure API Management
API Management ger hello möjlighet tooconfigure meddelanden för specifika händelser och tooconfigure hello e-mallar som används toocommunicate med hello administratörer och utvecklare av en API Management-instans. Det här avsnittet visar hur tooconfigure meddelanden för hello tillgängliga händelser, och ger en översikt över hur du konfigurerar hello e-mallar som används för dessa händelser.

## <a name="publisher-notifications"></a>Konfigurera publisher meddelanden
tooconfigure meddelanden, klickar du på **Publisher portal** i hello Azure-portalen för API Management-tjänsten. Då kommer du toohello API Management publisher portal.

![Utgivarportalen][api-management-management-console]

> [!NOTE] 
> Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management] [ Get started with Azure API Management] kursen.

Klicka på **meddelanden** från hello **API Management** menyn på hello lämnas tooview hello tillgängliga meddelanden.

![Publisher meddelanden][api-management-publisher-notifications]

hello kan följande lista över händelser som konfigureras för meddelanden.

* **Prenumerationsbegäranden (som kräver godkännande)** – hello angivna e-postmottagare och användarna ska få e-postaviseringar om prenumerationen förfrågningar för API-produkter som kräver godkännande.
* **Nya prenumerationer** – hello angivna e-postmottagare och användarna ska få e-postmeddelanden om nya prenumerationer för API-produkten.
* **Galleriet programförfrågningar** – hello angivna e-postmottagare och användarna får e-postmeddelanden när nya program som skickade toohello programgalleriet.
* **Hemlig kopia** – hello angivna e-postmottagare och användarna ska få e-hemliga kopior av alla e-postmeddelanden skickas toodevelopers.
* **Nytt ärende eller kommentar** – hello angivna e-postmottagare och användarna ska få e-postaviseringar när ett nytt ärende eller kommentar skickas på hello developer-portalen.
* **Avsluta konto meddelandet** – hello angivna e-postmottagare och får användarna e-postmeddelanden när ett konto har stängts.
* **Närmar sig kvotgränsen för prenumerationen** – hello följande e-postmottagare och får användarna e-postaviseringar när prenumerationsanvändning hämtar Stäng toousage kvoten.

Du kan ange e-postmottagare med hjälp av hello e-postadress textruta för varje händelse eller kan du välja användare från en lista.

toospecify hello e-postadresser toobe meddelande, ange dem i textrutan för hello e-postadress. Om du har flera e-postadresser avgränsade med kommatecken.

![Meddelandemottagare][api-management-email-addresses]

toospecify hello användare toobe meddelande, klicka på **Lägg till mottagare**hello kryssrutan bredvid hello användare toobe meddelande och klicka på **OK**.

> [!NOTE] 
> Endast administratörer visas i hello.


När du har konfigurerat hello meddelandemottagare klickar du på **spara** tooapply hello uppdatera meddelandemottagare.

> [!NOTE] 
> Om du navigerar bort från hello **Publisher meddelanden** fliken hello publisher portal varnar dig om det finns osparade ändringar.


## <a name="email-templates"></a>Konfigurera e-mallar
API Management innehåller postmallar för e-för hello e-postmeddelanden som skickas i hello loppet av administrera och hello-tjänsten. följande e-postmallar hello tillhandahålls.

* Application gallery skicka godkända
* Utvecklare farewell bokstav
* Kvot för utvecklare som närmar sig meddelande
* Bjud in användare
* Nya kommentaren till tooan problemet
* Nya problemet som tagits emot
* Ny prenumeration som aktiverats
* Prenumerationen förnyas bekräftelse
* Prenumerationsbegäran avböjer
* Prenumerationsbegäran mottagen

Dessa mallar kan ändras enligt önskemål.

tooview och konfigurerar hello e-postmallar för API Management-instans, klickar du på **meddelanden** från hello **API Management** menyn på hello vänster och välj hello **mallar för e-post**  fliken.

![E-postmallar][api-management-email-templates]

tooview eller ändra en mall, väljer du den hello **mallar** listrutan.

![Lista över mallar för e-post][api-management-email-templates-list]

Varje e-postmall har ett ämne med oformaterad text och en brödtext definition i HTML-format. Varje objekt kan anpassas enligt önskemål.

![Redigeraren för mallen för e-post][api-management-email-template]

Hej **parametrar** listan innehåller en lista av parametrar, som under infogas i hello ämne eller brödtext blir ersatta hello avsett värde när hello e-postmeddelande skickas. tooinsert parameter, placerar hello markören där du vill hello parametern toogo, och klicka på hello pilen toohello vänster i hello parameternamn.

Klicka på **Preview** eller **skicka ett test** toosee hur hello e-post ska se ut eller skicka ett test-e-post.

> [!NOTE] 
> hello-parametrar är inte ersättas med verkliga värden när Förhandsgranska eller skickar ett test.

toosave hello ändringar toohello e-postmall klickar du på **spara**, eller klicka på toocancel hello ändringar **Avbryt**.
 

[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png


[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png


[Configure publisher notifications]: #publisher-notifications
[Configure email templates]: #email-templates

[How toocreate and use groups]: api-management-howto-create-groups.md
[How tooassociate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
