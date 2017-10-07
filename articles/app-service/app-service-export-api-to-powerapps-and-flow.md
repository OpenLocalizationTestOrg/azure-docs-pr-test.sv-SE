---
title: aaaExporting en Azure-baserad API tooPowerApps och Microsoft Flow | Microsoft Docs
description: "Översikt över hur tooexpose API finns i Apptjänst tooPowerApps och Microsoft Flow"
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
ms.openlocfilehash: 285b6efa3af5b0feac1ee2f617c0dc56dc3fd198
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="exporting-an-azure-hosted-api-toopowerapps-and-microsoft-flow"></a>Exportera en Azure-baserad API tooPowerApps och Microsoft Flow

## <a name="creating-custom-connectors-for-powerapps-and-microsoft-flow"></a>Skapa egna kopplingar för PowerApps och Microsoft Flow

[PowerApps](https://powerapps.com) är en tjänst för att skapa och använda anpassade appar som ansluter tooyour data och fungera mellan olika plattformar. [Microsoft Flow](https://flow.microsoft.com) gör det enkelt tooautomate arbetsflöden och affärsprocesser mellan din favorit appar och tjänster. Både PowerApps och Microsoft Flow levereras med en mängd olika inbyggda kopplingar toodata källor, till exempel Office 365, Dynamics 365, Salesforce och mycket mer. Användare behöver dock också toobe kan tooleverage datakällor och API: er som skapas av organisationen.

På liknande sätt kan utvecklare som vill tooexpose sina mer brett inom hello organisation kanske vill toomake sina tillgängliga tooPowerApps för API: er och Microsoft Flow användare API: er. Det här avsnittet visar hur tooexpose en API som byggts med Azure App Service eller Azure Functions tooPowerApps och Microsoft Flow. [Azure Apptjänst](https://azure.microsoft.com/services/app-service/) är ett erbjudande för plattform som en tjänst som gör att utvecklare tooquickly och enkelt skapa företagsklass webb-, mobil och API-program. [Azure Functions](https://azure.microsoft.com/services/functions/) är en händelsebaserat serverlösa beräknings-lösning som gör att du tooquickly författare kod som kan reagera tooother delar av systemet och skala baserat på begäran.

toolearn mer om dessa tjänster finns i:
- [PowerApps interaktiv utbildning](https://powerapps.microsoft.com/guided-learning/learning-introducing-powerapps/) 
- [Microsoft flödet interaktiv utbildning](https://flow.microsoft.com/guided-learning/learning-introducing-flow/)
- [Vad är App Service?](https://docs.microsoft.com/azure/app-service/app-service-value-prop-what-is)
- [Vad är Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview)

## <a name="sharing-an-api-definition"></a>Dela en API-definition

API: er beskrivs ofta med hjälp av en [OpenAPI dokument](https://www.openapis.org/) (kallas ibland tooas ”Swagger”-dokument). Det innehåller alla hello information om vilka åtgärder är tillgängliga och hur data hello strukturerad. PowerApps och Microsoft Flow kan skapa egna kopplingar för OpenAPI 2.0 dokument. När en anpassad koppling har skapats kan den användas i hello exakt samma sätt som en hello inbyggda kopplingar och snabbt kan integreras i ett program.

Azure Apptjänst och Azure Functions har [inbyggt stöd](https://docs.microsoft.com/azure/app-service-api/app-service-api-metadata) för att skapa, värd och hantera ett OpenAPI dokument. I ordning toocreate en anpassad koppling för en webb-, mobil, API eller funktionsapp måste PowerApps och flödar toobe en kopia av hello definition.

> [!NOTE]
> Eftersom en kopia av hello API-definition används vet PowerApps och Microsoft Flow inte omedelbart om uppdateringar eller bryta ändringar toohello program. Om en ny version av hello API är tillgängligt, ska dessa steg upprepas för hello nya versionen. 

Så här tooprovide PowerApps och Microsoft Flow med hello värdbaserade API-definition för din app:

1. Öppna hello [Azure Portal](https://portal.azure.com) och navigera tooyour Apptjänst eller Azure Functions.

    Om du använder Azure App Service, Välj **API-definition** hello inställningar för listan. 
    
    Om du använder Azure Functions, Välj appen funktionen och välj sedan **plattformsfunktioner**, och sedan **API-definition**. Du kan också välja tooopen hello **API-definition (förhandsgranskning)** fliken i stället.

2. Om en API-definition har angetts, visas en **exportera tooPowerApps + Microsoft Flow** knappen. Klicka på knappen toobegin hello detta.

3. Välj hello **exportläge**. Detta avgör hello steg som du måste toofollow toocreate en koppling. Apptjänst erbjuder två alternativ för att tillhandahålla PowerApps och Microsoft Flow med din API-definition:

    **Express** kan du skapa hello anpassad koppling från inom hello Azure-portalen. Det krävs hello nuvarande inloggade användaren har behörighet toocreate kopplingar i hello målmiljön. Detta är hello rekommendationer om detta krav kan uppfyllas. Om du använder det här läget följer hello [Express export](#express) anvisningarna nedan.

    **Manuell** kan du exportera en kopia av hello API ingår som kan importeras med hello PowerApps eller Microsoft Flow portaler. Detta är hello rekommendationer om hello Azure användar- och hello användare med behörighet toocreate kopplingar är olika personer eller om hello connector måste toobe som skapats i en annan klient. Om du använder det här läget följer hello [manuell export och import](#manual) anvisningarna nedan.

<a name="express"></a>
## <a name="express-export"></a>Express export

I det här avsnittet skapar du en ny anpassad koppling från inom hello Azure-portalen. Du måste vara inloggad i hello klient toowhich gärna tooexport och du måste ha behörigheten toocreate en anpassad koppling i hello målmiljön.

1. Välj hello miljö där du vill att toocreate hello-anslutningen. Ange sedan ett namn för din anpassade connector.

2. Om din API-definition innehåller alla definitioner för säkerhet, kommer dessa som beskrivs i steg #2. Om det behövs, skydda hello konfigurationsinformation behövs toogrant användare komma åt tooyour API. Mer information finns i [autentisering](#auth) nedan. 

3. Klicka på **OK** toocreate din anpassad koppling.


<a name="manual"></a>
## <a name="manual-export-and-import"></a>Manuell export och import

I ordning toocreate en anpassad koppling för en webb-, mobil, API eller funktionsapp, två steg som behövs:

1. [Hämta API-definitions-hello från Apptjänst eller Azure Functions](#export)
2. [Importera hello API-definition i PowerApps och Microsoft Flow](#import)

Det är möjligt att dessa två steg måste toobe som utförs av olika personer i en organisation som en viss användare inte kanske har behörighet tooperform båda åtgärderna. I så fall måste en utvecklare som har deltagare åtkomst toohello Apptjänst eller Azure Functions tooobtain hello API-definition (en enda JSON-fil) eller en länk tooit. De måste sedan tooprovide att definition tooa PowerApps eller Microsoft Flow ägare. Som ägare kan använda hello metadata toocreate hello anpassad koppling.

<a name="export"></a>
### <a name="retrieving-hello-api-definition-from-app-service-or-azure-functions"></a>Hämta API-definitions-hello från Apptjänst eller Azure Functions

I det här avsnittet ska du exportera hello API-definition för din App Service API, toobe används senare i hello PowerApps eller Microsoft Flow portalen.

1. Du kan välja tooeither **hämta hello API-definition** eller **hämta en länk**. Beroende på vilket som du använder, hello resultatet ska anges i hello nästa avsnitt. Välj något av följande alternativ och följ anvisningarna för hello.
 
2. Om din API-definition innehåller alla definitioner för säkerhet, kommer dessa som beskrivs i steg #2. Under importen, PowerApps och Microsoft Flow identifierar dessa och uppmanar säkerhetsinformation. Samla in hello autentiseringsuppgifter relaterade tooeach definition för användning i nästa avsnitt om hello. Mer information finns i [autentisering](#auth) nedan. 

<a name="import"></a>
### <a name="importing-hello-api-definition-into-powerapps-and-microsoft-flow"></a>Importera hello API-definition i PowerApps och Microsoft Flow

I det här avsnittet skapar du en anpassad koppling i PowerApps och Microsoft Flow med hello API-definition fick tidigare. Anpassade kopplingar delas mellan hello två tjänster, så behöver du bara en gång tooimport hello-definition. Mer information om anpassade kopplingar finns [registrera och använda anpassade kopplingar i PowerApps] och [registrera och använda anpassade kopplingar i Microsoft Flow].

1. Öppna hello [Powerapps webbportalen](https://web.powerapps.com) eller hello [Microsoft Flow webbportalen](https://flow.microsoft.com/), och logga in. 

2. Klicka på hello **inställningar** (hello växeln ikonen) längst hello övre högra hörnet av hello sida och välj **anpassade kopplingar**. 

3. Klicka på **Skapa anpassad koppling**.

4. På hello **allmänna** fliken, ange ett namn för din API och sedan överföra hello OpenAPI definition eller klistra in i hello metadata-URL. Klicka på **fortsätta**.

4. På hello **säkerhet** fliken om du tillfrågas tooprovide autentisering information ange hello värden som erhölls i hello föregående avsnitt. Annars fortsätter toohello nästa steg.

5. På hello **definitioner** fliken alla hello-åtgärder som definierats i filen OpenAPI är fylls i automatiskt. Om alla nödvändiga åtgärder har definierats kan gå du toohello nästa steg. Om inte, kan du lägga till och ändra operations här.

6. Klicka på **skapa koppling**. Om du vill tootest API-anrop, gå toohello nästa steg.

7. På hello **Test** fliken, skapa en anslutning, Välj en åtgärd tootest och ange några data som krävs av hello-åtgärd.

8. Klicka på **testa åtgärden**.


<a name="auth"></a>
## <a name="authentication"></a>Autentisering

PowerApps och Microsoft Flow stöd inbyggt för en samling identitetsleverantörer som kan vara används toolog användare av din anpassade connector. Om din API kräver autentisering, kontrollera att den avbildas som en _säkerhet definition_ i dokumentet OpenAPI. Under exporten måste tooprovide konfigurationsvärden som tillåter PowerApps en Microsoft Flow tooperform inloggningen åtgärder.

Det här avsnittet beskriver hello autentiseringstyper som stöds av hello express flöde: API-nyckel, Azure Active Directory och allmänna OAuth 2.0. En fullständig lista över leverantörer och hello autentiseringsuppgifter kräver finns [registrera och använda anpassade kopplingar i PowerApps] och [registrera och använda anpassade kopplingar i Microsoft Flow].

### <a name="api-key"></a>API-nyckel
När detta säkerhetsprogram används kommer hello användare anslutningstjänsten att tillfrågas tooprovide hello nyckel när de skapar en anslutning. Du kan ange en API nyckelnamn toohelp de vet vilken nyckel krävs. För Azure Functions, vanligtvis blir en hello-värdnycklar, som omfattar flera funktioner i hello funktionsapp.

### <a name="azure-active-directory"></a>Azure Active Directory
När du konfigurerar en anpassad koppling som kräver AAD inloggning krävs två registreringar för AAD-program: en toomodel hello backend-API och en toomodel hello koppling i PowerApps och flöde.

Din API ska vara konfigurerade toowork med hello första registreringen och detta kommer redan typen om du använde hello [autentisering/auktorisering i Apptjänst](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication) funktion.

Du måste toomanually skapa hello andra registrering för hello connector med hello steg som beskrivs i [lägga till ett AAD-program](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-an-application). hello registrering måste toohave delegerad åtkomst tooyour API och svars-URL: en `https://msmanaged-na.consent.azure-apim.net/redirect`. Se [det här exemplet](
https://powerapps.microsoft.com/tutorials/customapi-azure-resource-manager-tutorial/) ersätta ditt API för hello Azure Resource Manager för mer information.

hello följande konfigurationsvärden krävs:
- **Klient-ID** -hello klient-ID anslutningstjänsten AAD-registrering
- **Klienthemlighet** -hello klienthemlighet anslutningstjänsten AAD-registrering
- **Inloggnings-URL** - hello bas-URL för AAD. I offentliga Azure detta vanligtvis blir `https://login.windows.net`.
- **Klient-ID** -hello-ID för hello klient toobe används för hello inloggningen. Det bör vara ”gemensamma” eller hello-ID för hello klient i vilka hello anslutningen skapas.
- **Resurs-URL** -hello resurs-URL för API-serverdel AAD registreringen

> [!IMPORTANT]
> Om en annan person importerar hello API-definition i PowerApps och Microsoft Flow som en del av hello manuell flödet, behöver du tooprovide dem med hello-ID och klienten klienthemlighet **av registreringen av anslutningsverktyget hello**, samt som hello resurs-URL för ditt API. Se till att dessa hemligheter hanteras på ett säkert sätt. **Dela inte hello säkerhetsreferenser för hello API sig själv.**

### <a name="generic-oauth-20"></a>Allmän OAuth 2.0
hello allmänna OAuth 2.0-stöd kan toointegrate med valfri OAuth 2.0-provider. Detta ger dig toobring eventuella anpassade providern som inte stöds internt.

hello följande konfigurationsvärden krävs:
- **Klient-ID** -hello OAuth 2.0 klient-ID
- **Klienthemlighet** -hello OAuth 2.0-klienthemlighet
- **Webbadress för auktorisering** -hello Webbadress för OAuth 2.0-auktorisering
- **Token URL** -hello OAuth 2.0-token URL
- **Uppdatera URL** -hello URL: en för OAuth 2.0-uppdatering



[registrera och använda anpassade kopplingar i PowerApps]: https://powerapps.microsoft.com/tutorials/register-custom-api/
[registrera och använda anpassade kopplingar i Microsoft Flow]: https://flow.microsoft.com/documentation/register-custom-api/
