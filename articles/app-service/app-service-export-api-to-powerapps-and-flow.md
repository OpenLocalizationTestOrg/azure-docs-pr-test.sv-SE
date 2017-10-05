---
title: "Exportera en Azure-baserad API till PowerApps och Microsoft flödar | Microsoft Docs"
description: "Översikt över hur du exponera en värdbaserad API i Apptjänst PowerApps och Microsoft Flow"
services: app-service
documentationcenter: 
author: mattchenderson
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 06/20/2017
ms.author: mahender
ms.openlocfilehash: 0d166a2e5b60c3b9f911f9fc3ad49ad7f252abb4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="exporting-an-azure-hosted-api-to-powerapps-and-microsoft-flow"></a>Exportera en Azure-baserad API till PowerApps och Microsoft-flöde

## <a name="creating-custom-connectors-for-powerapps-and-microsoft-flow"></a>Skapa egna kopplingar för PowerApps och Microsoft Flow

[PowerApps](https://powerapps.com) är en tjänst för att skapa och använda anpassade appar som ansluter till dina data och fungera mellan olika plattformar. [Microsoft Flow](https://flow.microsoft.com) gör det enkelt att automatisera arbetsflöden och affärsprocesser mellan din favorit appar och tjänster. Både PowerApps och Microsoft Flow har ett antal inbyggda anslutningar till datakällor, till exempel Office 365, Dynamics 365, Salesforce och mycket mer. Användare behöver dock också för att kunna utnyttja datakällor och API: er som skapas av organisationen.

På liknande sätt kan utvecklare som vill använda sina API: er mer brett inom organisationen kanske vill göra deras API: er som är tillgängliga för PowerApps och Microsoft Flow användarna. Det här avsnittet visas hur du exponera en API som byggts med Azure App Service eller Azure Functions PowerApps och Microsoft Flow. [Azure Apptjänst](https://azure.microsoft.com/services/app-service/) är ett erbjudande för plattform som en tjänst som gör att utvecklare att snabbt och enkelt skapa företagsklass webb-, mobil- och API-program. [Azure Functions](https://azure.microsoft.com/services/functions/) är en händelsebaserat serverlösa beräknings-lösning som gör att du kan snabbt författare kod som kan ta hänsyn till andra delar av systemet och skala baserat på begäran.

Mer information om dessa tjänster finns:
- [PowerApps interaktiv utbildning](https://powerapps.microsoft.com/guided-learning/learning-introducing-powerapps/) 
- [Microsoft flödet interaktiv utbildning](https://flow.microsoft.com/guided-learning/learning-introducing-flow/)
- [Vad är App Service?](https://docs.microsoft.com/azure/app-service/app-service-value-prop-what-is)
- [Vad är Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview)

## <a name="sharing-an-api-definition"></a>Dela en API-definition

API: er beskrivs ofta med hjälp av en [OpenAPI dokument](https://www.openapis.org/) (kallas ibland ”Swagger”-dokument). Innehåller all information om vilka åtgärder är tillgängliga och hur data ska vara strukturerad. PowerApps och Microsoft Flow kan skapa egna kopplingar för OpenAPI 2.0 dokument. När en anpassad koppling har skapats kan användas på exakt samma sätt som en av de inbyggda kopplingarna och snabbt kan integreras i ett program.

Azure Apptjänst och Azure Functions har [inbyggt stöd](https://docs.microsoft.com/azure/app-service-api/app-service-api-metadata) för att skapa, värd och hantera ett OpenAPI dokument. För att skapa en anpassad koppling för en webb-, mobil, API eller funktionsapp, PowerApps och flödet måste ges en kopia av definitionen.

> [!NOTE]
> Eftersom en kopia av API-definitionen används vet PowerApps och Microsoft Flow inte omedelbart om uppdateringar eller viktiga förändringar till programmet. Om en ny version av API: et är tillgängligt, ska dessa steg upprepas för den nya versionen. 

Följ dessa steg för att ge PowerApps och Microsoft Flow värdbaserade API-definitionen för din app måste:

1. Öppna den [Azure Portal](https://portal.azure.com) och navigera till ditt program med App Service eller Azure Functions.

    Om du använder Azure App Service, Välj **API-definition** från inställningslistan över. 
    
    Om du använder Azure Functions, Välj appen funktionen och välj sedan **plattformsfunktioner**, och sedan **API-definition**. Du kan också välja att öppna den **API-definition (förhandsgranskning)** fliken i stället.

2. Om en API-definition har angetts, visas en **exportera till PowerApps + Microsoft Flow** knappen. Klicka här om du vill påbörja exporten.

3. Välj den **exportläge**. Detta avgör steg som du måste följa för att skapa en koppling. Apptjänst erbjuder två alternativ för att tillhandahålla PowerApps och Microsoft Flow med din API-definition:

    **Express** kan du skapa den anpassade-kopplingen från Azure-portalen. Det kräver att den nuvarande inloggade användaren har behörighet att skapa kopplingar i målmiljön. Detta är den rekommenderade metoden om detta krav kan uppfyllas. Om du använder det här läget, följer du de [Express export](#express) anvisningarna nedan.

    **Manuell** kan du exportera en kopia av API-ingår som kan importeras med PowerApps eller Microsoft Flow portaler. Detta är den rekommenderade metoden om Azure användare och användare med behörighet att skapa kopplingar är olika personer eller om anslutningen måste skapas i en annan klient. Om du använder det här läget, följer du de [manuell export och import](#manual) anvisningarna nedan.

<a name="express"></a>
## <a name="express-export"></a>Express export

I det här avsnittet skapar du en ny anpassad koppling från i Azure-portalen. Du måste vara inloggad på klienten som du vill exportera och du måste ha behörighet att skapa en anpassad koppling i målmiljön.

1. Välj miljön där du vill skapa kopplingen. Ange sedan ett namn för din anpassade connector.

2. Om din API-definition innehåller alla definitioner för säkerhet, kommer dessa som beskrivs i steg #2. Om det behövs, tillhandahålla säkerhet konfigurationsinformation som behövs för att bevilja användare åtkomst till ditt API. Mer information finns i [autentisering](#auth) nedan. 

3. Klicka på **OK** att skapa en anpassad koppling.


<a name="manual"></a>
## <a name="manual-export-and-import"></a>Manuell export och import

Två steg krävs för att skapa en anpassad koppling för en webb-, mobil, API eller funktionsapp:

1. [Hämta API-definitionen från Apptjänst eller Azure Functions](#export)
2. [Importerar en API-definition i PowerApps och Microsoft Flow](#import)

Det är möjligt att dessa två steg måste utföras av separata enskilda användare i en organisation som en viss användare inte kanske har behörighet att utföra både åtgärder. I detta fall kan måste en utvecklare som har deltagare åtkomst till programmet Apptjänst eller Azure Functions du hämta API-definition (en enda JSON-fil) eller en länk till den. De måste sedan att tillhandahålla den definitionen för en PowerApps eller Microsoft Flow ägare. Som ägare kan använda metadata för att skapa anpassade kopplingen.

<a name="export"></a>
### <a name="retrieving-the-api-definition-from-app-service-or-azure-functions"></a>Hämta API-definitionen från Apptjänst eller Azure Functions

I det här avsnittet ska du exportera API-definitionen för din App Service-API, som ska användas senare i PowerApps eller Microsoft Flow-portalen.

1. Du kan välja att antingen **hämta API-definition** eller **hämta en länk**. Beroende på vilket som du använder, resultatet anges i nästa avsnitt. Välj något av följande alternativ och följ instruktionerna.
 
2. Om din API-definition innehåller alla definitioner för säkerhet, kommer dessa som beskrivs i steg #2. Under importen, PowerApps och Microsoft Flow identifierar dessa och uppmanar säkerhetsinformation. Samla in de autentiseringsuppgifter som rör varje definition för användning i nästa avsnitt. Mer information finns i [autentisering](#auth) nedan. 

<a name="import"></a>
### <a name="importing-the-api-definition-into-powerapps-and-microsoft-flow"></a>Importerar en API-definition i PowerApps och Microsoft Flow

I det här avsnittet skapar du en anpassad koppling i PowerApps och Microsoft Flow använder tidigare hämtade API-definitionen. Anpassade kopplingar delas mellan de två tjänsterna, så du behöver bara importera definition en gång. Mer information om anpassade kopplingar finns [registrera och använda anpassade kopplingar i PowerApps] och [registrera och använda anpassade kopplingar i Microsoft Flow].

1. Öppna den [Powerapps webbportalen](https://web.powerapps.com) eller [Microsoft Flow webbportalen](https://flow.microsoft.com/), och logga in. 

2. Klicka på den **inställningar** knappen (kugghjulsikonen) längst upp till höger på sidan och välj **anpassade kopplingar**. 

3. Klicka på **Skapa anpassad koppling**.

4. På den **allmänna** fliken, ange ett namn för din API och sedan ladda upp OpenAPI definition eller klistra in i metadata-URL. Klicka på **fortsätta**.

4. På den **säkerhet** om du uppmanas att ange information om autentisering, anger du värden som hämtades i föregående avsnitt. Om inte, gå vidare till nästa steg.

5. På den **definitioner** fliken alla åtgärder som definierats i filen OpenAPI är fylls i automatiskt. Om alla nödvändiga åtgärder har definierats kan gå du till nästa steg. Om inte, kan du lägga till och ändra operations här.

6. Klicka på **skapa koppling**. Om du vill testa API-anrop, går du till nästa steg.

7. På den **testa** fliken, skapa en anslutning, Välj en åtgärd för att testa och ange några data som krävs för åtgärden.

8. Klicka på **testa åtgärden**.


<a name="auth"></a>
## <a name="authentication"></a>Autentisering

PowerApps och Microsoft Flow stöd inbyggt för en samling identitetsleverantörer som kan användas för att logga in användare av en anpassad koppling. Om din API kräver autentisering, kontrollera att den avbildas som en _säkerhet definition_ i dokumentet OpenAPI. Under exporten behöver du ange konfigurationsvärden som tillåter PowerApps ett Microsoft-Flow att utföra åtgärder för inloggning.

Det här avsnittet beskriver de autentiseringstyper som stöds av express flödet: API-nyckel, Azure Active Directory och allmänna OAuth 2.0. En fullständig lista över leverantörer och autentiseringsuppgifterna varje kräver finns [registrera och använda anpassade kopplingar i PowerApps] och [registrera och använda anpassade kopplingar i Microsoft Flow].

### <a name="api-key"></a>API-nyckel
När detta säkerhetsprogram används uppmanas användare anslutningstjänsten att ange nyckeln när de skapar en anslutning. Du kan ange en API-nyckelnamn så att de vet vilken nyckel krävs. För Azure Functions, kommer det vanligtvis vara något av värdnycklar, som omfattar flera funktioner i appen funktionen.

### <a name="azure-active-directory"></a>Azure Active Directory
När du konfigurerar en anpassad koppling som kräver AAD inloggning krävs två registreringar för AAD-program: en att modellera serverdelen API och en modellera kopplingen i PowerApps och flöde.

Din API ska konfigureras för att fungera med den första registreringen och detta kommer redan typen om du har använt den [autentisering/auktorisering i Apptjänst](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication) funktion.

Du måste manuellt skapa andra registreringen för anslutningstjänsten, med hjälp av stegen som beskrivs i [lägga till ett AAD-program](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-an-application). Registreringen måste ha delegerad åtkomst till ditt API och svars-URL: en `https://msmanaged-na.consent.azure-apim.net/redirect`. Se [det här exemplet](
https://powerapps.microsoft.com/tutorials/customapi-azure-resource-manager-tutorial/) ersätta ditt API för Azure Resource Manager för mer information.

Följande konfigurationsvärden krävs:
- **Klient-ID** -klient-ID anslutningstjänsten AAD-registrering
- **Klienthemlighet** -klienthemlighet anslutningstjänsten AAD-registrering
- **Inloggnings-URL** -bas-URL för AAD. I offentliga Azure detta vanligtvis blir `https://login.windows.net`.
- **Klient-ID** -ID för innehavaren som ska användas för inloggningen. Detta bör vara ”vanliga” eller ID för innehavaren som anslutningen skapas.
- **Resurs-URL** -resurs-URL för API-serverdel AAD registreringen

> [!IMPORTANT]
> Om en annan person importerar API-definition i PowerApps och Microsoft Flow som en del av manuell flödet, behöver du förser dem med klient-ID och klienthemlighet **av registreringen av anslutningsverktyget**, samt resurs-URL för ditt API. Se till att dessa hemligheter hanteras på ett säkert sätt. **Dela inte säkerhetsreferenser för API: et sig själv.**

### <a name="generic-oauth-20"></a>Allmän OAuth 2.0
Allmänna OAuth 2.0-stöd kan du integrera med valfri OAuth 2.0-provider. På så sätt kan du se till att i en anpassad provider som inte stöds internt.

Följande konfigurationsvärden krävs:
- **Klient-ID** -OAuth 2.0-klient-ID
- **Klienthemlighet** -klienthemlighet OAuth 2.0
- **Webbadress för auktorisering** -Webbadress för OAuth 2.0-auktorisering
- **Token URL** -URL för OAuth 2.0-token
- **Uppdatera URL** -URL för OAuth 2.0-uppdatering



[registrera och använda anpassade kopplingar i PowerApps]: https://powerapps.microsoft.com/tutorials/register-custom-api/
[registrera och använda anpassade kopplingar i Microsoft Flow]: https://flow.microsoft.com/documentation/register-custom-api/
