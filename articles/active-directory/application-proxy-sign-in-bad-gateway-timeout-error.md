---
title: "Kan inte komma åt felet företagets program när du använder ett program med Application Proxy | Microsoft Docs"
description: "Hur du löser vanliga problem med Azure AD Application Proxy-program."
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
ms.openlocfilehash: 78ff8763a461162cbcfa04c6a86123973271928a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="cant-access-this-corporate-application-error-when-using-an-application-proxy-application"></a>”Det går inte att komma åt företagets programmet” fel när du använder ett program med Application Proxy

Den här artikeln hjälper dig att felsöka vanliga problem med står inför när du ser felet ”företagets appen kan inte nås” på ett program för Azure AD Application Proxy.

## <a name="overview"></a>Översikt
När det här felet visas sidan också dela en statuskod. Koden är förmodligen en av följande:

-   **Gateway-Timeout**: The Application Proxy-tjänsten kan inte nå kopplingen. Detta tyder oftast på ett problem med anslutningen tilldelningen kopplingen själv, eller till nätverk regler runt kopplingen.

-   **Ogiltig Gateway**: kopplingen kan inte nå backend-programmet. Detta kan tyda på en felaktig konfiguration av programmet.

-   **Tillåts inte**: användaren har inte behörighet att komma åt programmet. Detta kan inträffa om användaren inte har tilldelats till programmet i Azure Active Directory eller om på serverdelen användaren inte har behörighet att komma åt programmet.

Granska texten längst ned till vänster i felmeddelandet för fältet ”statuskod” för att hitta koden. Också söka efter alla anteckningar längst ned på sidan med ytterligare tips.

   ![Gateway-timeout-fel](./media/application-proxy/connection-problem.png)

Mer information om felsökning av orsaken till felen och mer information om föreslagna korrigeringar avsnittet motsvarande nedan.

## <a name="gateway-timeout-errors"></a>Gateway-Timeout-fel

En gateway-timeout inträffar när tjänsten försöker nå kopplingen och är inte inom tidsgränsen. Detta orsakas typiskt av ett program som har tilldelats en koppling grupp med ingen fungerande kopplingar eller vissa portar som krävs av anslutningen är inte öppna.


## <a name="bad-gateway-errors"></a>Felaktig Gateway-fel

En ogiltig gateway felet indikerar att anslutningen är inte nå backend-programmet. Kontrollera att du har publicerat rätt program. Vanliga fel som orsakar felet:

-   Skriv- eller fel i intern URL

-   Publicera inte roten för programmet. Till exempel publicera <http://expenses/reimbursement> men försök att få åtkomst till <http://expenses>

-   Problem med konfigurationen av Kerberos-begränsad delegering (KCD)

-   Problem med backend-programmet

## <a name="forbidden-errors"></a>Förbjudet fel

Om du ser felet förbjuden har användaren inte tilldelats programmet. Detta kan vara i Azure Active Directory eller på backend-programmet.

Om du vill lära dig mer om att tilldela användare till programmet i Azure, finns det [configuration dokumentationen](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal#add-a-test-user).

Om du bekräfta att användaren har tilldelats till programmet i Azure, kontrollera Användarkonfiguration i backend-programmet. Du kan se sidan med våra KCD felsöka vissa riktlinjer om du använder Kerberos-begränsad delegering/integrerad Windows-autentisering.

## <a name="check-the-applications-internal-url"></a>Kontrollera programmets Intern URL

Som ett första snabba steg dubbelkolla kontrollerar och korrigerar Intern URL genom att öppna programmet via **företagsprogram**, sedan välja den **Application Proxy** menyn. Verifiera det här är rätt Intern URL-Adressen som används från ditt lokala nätverk för att få åtkomst till programmet.

## <a name="check-the-application-is-assigned-to-a-working-connector-group"></a>Kontrollera att programmet har tilldelats en fungerande koppling grupp

Att kontrollera att tilldelas till en fungerande koppling grupp:

1.  Öppna programmet i portalen genom att gå till **Azure Active Directory**, klicka på **företagsprogram**, sedan **alla program.** Öppna programmet och välj sedan **Application Proxy** i den vänstra menyn.

2.  Titta på fältet Connector. Om det finns inga aktiva kopplingar i gruppen kan se en varning. Om du inte ser alla varningar, gå vidare till ”verifiera alla nödvändiga portarna är godkända”.

3.  Om det är fel Connector grupp i listrutan nedåt och välj rätt grupp och bekräfta att du inte längre se alla varningar. Om den avsedda Connector gruppen klickar du på varningsmeddelandet för att öppna sidan med hantering av anslutningen.

4.  Det finns några olika sätt att visa från den här:

  * Flytta en aktiv koppling till gruppen: Om du har en aktiv koppling som ska tillhöra den här gruppen och har fri backend målprogrammet kan du flytta kopplingen i grupp. Gör du genom att klicka på kopplingen. I fältet ”Connector grupp” använder du i listrutan och välj rätt grupp klickar du på Spara.

  * Hämta en ny koppling för gruppen: den här sidan kan du hämta en länk till [ladda ned en ny koppling](https://download.msappproxy.net/Subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/Connector/Download). Anslutningen måste vara installerad på en dator med fri till backend-programmet och placeras vanligtvis på samma server som programmet. Använda nedladdningen Connector länk för att hämta en koppling till måldatorn. Klicka på anslutningen och använda listrutan ”Connector grupp” för att se till att det tillhör gruppen rätt.

  * Undersöka en inaktiv anslutning: om en koppling visas som inaktiva, är det att nå tjänsten. Detta beror vanligtvis på vissa portar som krävs är blockerade. Gå vidare till ”verifiera alla nödvändiga portarna är godkända” för att lösa problemet.

När du använder testa dessa steg för att se till att programmet har tilldelats en grupp med fungerar kopplingar, programmet igen. Om det fortfarande inte fungerar kan du fortsätta till nästa avsnitt.

## <a name="check-all-required-ports-are-whitelisted"></a>Kontrollera att alla nödvändiga portarna är godkända

Om du vill kontrollera att alla portar är öppna, finns i vår dokumentation om hur du öppnar portar. Om portarna som krävs är öppna, flytta till nästa avsnitt.

## <a name="check-for-other-connector-errors"></a>Sök efter andra Connector-fel

Om inget av ovanstående löser problemet, är nästa steg att söka efter problem eller fel med kopplingen själv. Du kan se några vanliga fel i den [Felsök dokumentet](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot#connector-errors). 

Du kan också söka direkt på Connector-loggarna för att identifiera eventuella fel. Många av våra felmeddelanden ska kunna dela mer specifika rekommendationer för korrigeringar. Information om hur du visar loggarna finns [vår dokumentation kopplingar](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors#under-the-hood).

## <a name="additional-resolutions"></a>Ytterligare lösningar

Om detta inte löser problemet, finns det några olika orsaker. Att identifiera problemet:

Om programmet är konfigurerat för att använda integrerad autentisering IWA (Windows), testa programmet utan enkel inloggning. Om inte, gå vidare till nästa stycke. Om du vill kontrollera programmet utan enkel inloggning, öppna ditt program via **företagsprogram,** och gå till den **enkel inloggning** menyn. Ändra nedrullningsbara från ”integrerad Windows-autentisering” till ”Azure AD enkel inloggning inaktiverad”. 

Öppna en webbläsare och försök att komma åt programmet igen. Du ska bli ombedd att autentisera och komma åt programmet. Om det här fungerar är problemet med Kerberos-begränsad delegering (KCD) konfiguration som gör den enkel inloggning. Se sidan KCD felsöka.

Om du ser felet, gå till datorn där kopplingen har installerats, öppna en webbläsare och försöker komma åt den interna URL som används för programmet. Kopplingen fungerar som en annan klient från samma dator. Om du inte kan nå programmet, kan du undersöka varför som datorn är inte når programmet eller använda en koppling på en server som har tillgång till programmet.

Om du kan komma åt programmet från den datorn för att leta efter problem eller fel med kopplingen själv. Du kan se några vanliga fel i den [Felsök dokumentet](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot#connector-errors). Du kan också söka direkt på Connector-loggarna för att identifiera eventuella fel. Många av våra felmeddelanden ska kunna dela mer specifika rekommendationer för korrigeringar. Information om hur du visar loggarna finns [vår dokumentation kopplingar](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors#under-the-hood).

## <a name="next-steps"></a>Nästa steg
[Förstå Azure AD Application Proxy-kopplingar](application-proxy-understand-connectors.md)
