---
title: "aaa ”kan inte komma åt felet företagets program när du använder ett program med Application Proxy | Microsoft Docs ”"
description: "Hur tooresolve vanliga åtkomst problem med Azure AD Application Proxy-program."
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
ms.openlocfilehash: 490b106b7d774ee43fc076cc5d082997a1df85e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="cant-access-this-corporate-application-error-when-using-an-application-proxy-application"></a>”Det går inte att komma åt företagets programmet” fel när du använder ett program med Application Proxy

Den här artikeln hjälper dig tootroubleshoot vanliga problem inför när du ser felet ”företagets appen kan inte nås” på ett program för Azure AD Application Proxy.

## <a name="overview"></a>Översikt
När du ser felet hello sidan också dela en statuskod. Koden är förmodligen en av hello följande:

-   **Gateway-Timeout**: hello Application Proxy-tjänsten är tooreach hello koppling. Detta tyder oftast på ett problem med hello connector tilldelning, kopplingen själv eller hello nätverk regler runt hello-anslutningen.

-   **Ogiltig Gateway**: hello connector är tooreach hello backend-programmet. Detta kan tyda på en felaktig konfiguration av programmet hello.

-   **Tillåts inte**: hello användaren är inte auktoriserad tooaccess hello-program. Detta kan inträffa om hello användaren inte har tilldelats toohello program i Azure Active Directory eller om på hello serverdel hello användaren inte har behörighet tooaccess hello program.

toofind hello kod, titta på hello text hello längst ned till vänster i hello felmeddelandet hello ”Status” fältet. Också söka efter eventuella kommentarer längst ned hello mycket hello sida med ytterligare tips.

   ![Gateway-timeout-fel](./media/application-proxy/connection-problem.png)

Mer information om hur tootroubleshoot hello grundorsaken till dessa fel och mer information om föreslagna korrigeringar finns i avsnittet hello motsvarande nedan.

## <a name="gateway-timeout-errors"></a>Gateway-Timeout-fel

En gateway-timeout inträffar när hello-tjänsten försöker tooreach hello koppling och är toowithin hello tidsgränsen. Detta orsakas typiskt av ett program som tilldelats tooa Connector grupp med ingen fungerande kopplingar eller vissa portar som krävs av hello anslutningen är inte öppna.


## <a name="bad-gateway-errors"></a>Felaktig Gateway-fel

En ogiltig gateway felet indikerar hello kopplingen är tooreach hello backend-programmet. Kontrollera att du har publicerat hello rätt program. Vanliga fel som orsakar felet:

-   Skriv- eller fel i intern hello-URL

-   Publicera inte hello programmet hello rot. Till exempel publicera <http://expenses/reimbursement> men försök tooaccess <http://expenses>

-   Problem med konfigurationen av hello Kerberos-begränsad delegering (KCD)

-   Problem med hello backend-programmet

## <a name="forbidden-errors"></a>Förbjudet fel

Om du ser felet förbjuden hello användaren har inte tilldelats toohello program. Detta kan vara i Azure Active Directory eller på hello backend-programmet.

toolearn hur tooassign användare toohello program i Azure, se hello [configuration dokumentationen](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal#add-a-test-user).

Om du bekräfta hello användaren har tilldelats toohello program i Azure, kontrollera hello Användarkonfiguration i hello backend-programmet. Du kan se sidan med våra KCD felsöka vissa riktlinjer om du använder Kerberos-begränsad delegering/integrerad Windows-autentisering.

## <a name="check-hello-applications-internal-url"></a>Kontrollera hello programmet Intern URL

Som ett första snabba steg dubbelkolla kontrollerar och korrigerar hello Intern URL genom att öppna programmet hello via **företagsprogram**, och sedan välja hello **Application Proxy** menyn. Verifiera det här är hello rätt Intern URL, hello används från ditt lokala nätverk tooaccess hello program.

## <a name="check-hello-application-is-assigned-tooa-working-connector-group"></a>Kontrollera hello programmet tilldelas tooa arbetsgrupp Connector

tooverify hello programmet tilldelas tooa arbetsgrupp för anslutningen:

1.  Öppna programmet hello i hello-portalen genom att gå för**Azure Active Directory**, klicka på **företagsprogram**, sedan **alla program.** Öppna hello programmet och välj sedan **Application Proxy** hello vänstra menyn.

2.  Titta på hello Connector gruppfältet. Om det finns inga aktiva kopplingar i hello grupp, visas en varning. Om du inte ser alla varningar vidare för ”verifiera alla nödvändiga portarna är godkända”.

3.  Om detta är hello fel Connector grupp använder hello nedrullningsbara tooselect hello grupp och bekräfta att du inte längre se alla varningar. Om det här är hello avsedd Connector-gruppen, klicka på hello varning meddelandet tooopen hello med hantering av anslutningen.

4.  Här finns några sätt toodrill i ytterligare:

  * Flytta en aktiv koppling till hello grupp: Om du har en aktiv koppling som ska tillhöra toothis grupp och har fri toohello målprogrammet backend, du kan flytta hello koppling till hello som tilldelats gruppen. toodo Klicka på hello Connector. Använda hello nedrullningsbara tooselect hello rätt grupp i hello ”Connector” fältet och klickar du på Spara.

  * Hämta en ny koppling för gruppen: den här sidan kan du hämta hello länk för[ladda ned en ny koppling](https://download.msappproxy.net/Subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/Connector/Download). hello Connector behov toobe installeras på en dator med fri toohello backend-programmet och vanligtvis är placerad hello samma server som hello program. Använd hello hämta Connector länken toodownload en koppling till hello måldatorn. Klicka på hello Connector och använda hello ”Connector grupp” nedrullningsbara toomake att toohello rätt gruppen tillhör.

  * Undersöka en inaktiv anslutning: om en koppling visas som inaktiva, är det tooreach hello-tjänsten. Detta är vanligtvis på grund av toosome krävs portar som är blockerade. toosolve problemet genom att flytta för ”Kontrollera på alla portar som krävs är godkända”.

När du använder de här stegen är tooensure hello programmet tilldelade tooa grupp med fungerar kopplingar, test hello programmet igen. Om det fortfarande inte fungerar kan du fortsätta toohello nästa avsnitt.

## <a name="check-all-required-ports-are-whitelisted"></a>Kontrollera att alla nödvändiga portarna är godkända

tooverify att alla nödvändiga portar är öppna, finns i vår dokumentation om hur du öppnar portar. Om alla hello portar som krävs är öppna, flytta toohello nästa avsnitt.

## <a name="check-for-other-connector-errors"></a>Sök efter andra Connector-fel

Om inget av ovanstående hello löser problemet hello är hello nästa steg toolook för problem eller fel med hello kopplingen själv. Du kan se några vanliga fel i hello [Felsök dokumentet](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot#connector-errors). 

Du kan också söka direkt på hello Connector-loggar tooidentify eventuella fel. Många av våra felmeddelanden för att kunna tooshare mer specifika rekommendationer för korrigeringar. hur tooview hello loggar finns toolearn [vår dokumentation kopplingar](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors#under-the-hood).

## <a name="additional-resolutions"></a>Ytterligare lösningar

Om hello ovan inte åtgärda hello finns några olika orsaker. tooidentify hello problem:

Om ditt program är konfigurerade toouse integrerad autentisering IWA (Windows), hello testprogrammet utan enkel inloggning. Om inte, flytta toohello nästa punkt. toocheck hello program utan enkel inloggning, öppna ditt program via **företagsprogram,** och gå toohello **enkel inloggning** menyn. Ändra hello nedrullningsbara från ”integrerad Windows-autentisering” för ”Azure AD enkel inloggning inaktiverad”. 

Öppna en webbläsare och försök tooaccess hello programmet igen. Du ska bli ombedd att autentisera och hämta till hello program. Om det här fungerar är hello problem med hello Kerberos-begränsad delegering (KCD) konfiguration som aktiverar hello enkel inloggning. Se hello KCD felsöka sidan.

Om du fortsätter toosee hello fel går toohello datorn där hello Connector installeras, öppna en webbläsare och försök tooreach hello Intern URL som används för programmet hello. hello kopplingen fungerar som en annan klient från hello samma dator. Om du inte kan nå hello program, undersöka varför som datorn är tooreach hello program eller via en anslutning på en server som är kan tooaccess hello program.

Om du kan nå hello programmet från den datorn toolook problem eller fel med hello kopplingen själv. Du kan se några vanliga fel i hello [Felsök dokumentet](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot#connector-errors). Du kan också söka direkt på hello Connector-loggar tooidentify eventuella fel. Många av våra felmeddelanden för att kunna tooshare mer specifika rekommendationer för korrigeringar. hur tooview hello loggar finns toolearn [vår dokumentation kopplingar](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors#under-the-hood).

## <a name="next-steps"></a>Nästa steg
[Förstå Azure AD Application Proxy-kopplingar](application-proxy-understand-connectors.md)
