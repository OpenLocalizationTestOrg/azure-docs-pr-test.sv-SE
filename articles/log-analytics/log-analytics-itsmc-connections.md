---
title: "aaaITSM anslutningar i OMS IT Service Management-anslutningstjänsten | Microsoft Docs"
description: "Ansluta din ITSM produkter och tjänster med IT Service Management-anslutningstjänsten i OMS toocentrally övervaka och hantera hello ITSM arbetsobjekt."
documentationcenter: 
author: JYOTHIRMAISURI
manager: riyazp
editor: 
ms.assetid: 8231b7ce-d67f-4237-afbf-465e2e397105
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2017
ms.author: v-jysur
ms.openlocfilehash: 53ef51bf75fb8ed15ea3ce5072d9365c221f9f4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-itsm-productsservices-with-it-service-management-connector-preview"></a>Anslut ITSM produkter och tjänster med IT Service Management-anslutningstjänsten (förhandsgranskning)
Den här artikeln innehåller information om hur tooconnect din ITSM produkter eller tjänster tooIT Service Management-anslutningstjänsten i OMS och centralt hantera din arbetsobjekt. Mer information om IT Service Management-anslutningstjänsten finns [översikt](log-analytics-itsmc-overview.md).

följande produkter och tjänster hello stöds:

- [System Center Service Manager](#connect-system-center-service-manager-to-it-service-management-connector-in-oms)
- [ServiceNow](#connect-servicenow-to-it-service-management-connector-in-oms)
- [Provance](#connect-provance-to-it-service-management-connector-in-oms)
- [Cherwell](#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="connect-system-center-service-manager-tooit-service-management-connector-in-oms"></a>Anslut System Center Service Manager tooIT Service Management-anslutningstjänsten i OMS

hello följande avsnitt innehåller information om hur tooconnect toohello ditt System Center Service Manager-produkten IT Service Management-anslutningstjänsten i OMS.

### <a name="prerequisites"></a>Krav

Kontrollera att du har hello följande förutsättningar uppfylls:

- IT Service Management-anslutningstjänsten installerad.
Mer information: [Configuration](log-analytics-itsmc-overview.md#configuration).
- hello Service Manager-webbprogram (webbprogram) har distribuerats och konfigurerats. Information om webbapp är [här](#create-and-deploy-service-manager-web-app-service).
- Hybridanslutningen skapas och konfigureras. Mer information: [konfigurera hello hybrid anslutning](#configure-the-hybrid-connection).
- I Service Manager-versioner som stöds: 2012 R2 eller 2016.
- Användarrollen: [avancerad operatör](https://technet.microsoft.com/library/ff461054.aspx).

### <a name="connection-procedure"></a>Proceduren för anslutning

Använd hello följa proceduren tooconnect ditt System Center Service Manager instans toohello IT Service Management-anslutningstjänsten:

1. Gå för**OMS** >**inställningar** > **anslutna källor**.
2. Välj **ITSM Connector** klickar du på **Lägg till ny anslutning**.

    ![Service manager ](./media/log-analytics-itsmc/itsmc-service-manager-connection.png)
3. Ange hello information enligt beskrivningen i följande tabell hello och klicka på **spara** toocreate hello anslutning:

> [!NOTE]
> Dessa parametrar är obligatoriska.

| **Fält** | **Beskrivning** |
| --- | --- |
| **Namn**   | Ange ett namn för hello System Center Service Manager-instans som du vill tooconnect med hello IT Service Management-anslutningstjänsten.  Du använder det här namnet senare när du konfigurerar arbetsobjekt i den här instansen eller visa detaljerad logganalys. |
| **Välj typ av anslutning**   | Välj **System Center Service Manager**. |
| **Server-URL**   | Ange hello URL för hello Service Manager-webbprogrammet. Mer information om Service Manager-webbprogrammet [här](#create-and-deploy-service-manager-web-app-service).
| **Klient-ID**   | Ange hello klient-ID som du genererade (med hello automatiskt skript) för att autentisera hello webbapp. Mer information om hello automatiserade skript [här.](log-analytics-itsmc-service-manager-script.md)|
| **Klienthemlighet**   | Typen hello klienthemligheten genereras för detta ID.   |
| **Omfång för synkronisering av data**   | Välj hello Service Manager arbetsobjekt som du vill toosync via hello IT Service Management-anslutningstjänsten.  Dessa objekt har importerats till logganalys fungerar. **Alternativ:** incidenter, ändringsbegäranden.|
| **Synkronisera Data** | Ange hello antal föregående dagar som du vill hello data från. **Maxgränsen**: 120 dagar. |
| **Skapa nytt konfigurationsobjekt i ITSM lösning** | Välj det här alternativet om du vill toocreate hello konfigurationsobjekt i hello ITSM produkten. När du väljer skapar OMS hello som påverkade konfigurationsobjekt som konfigurationsobjekt (vid CIs) i hello stöds ITSM system. **Som standard**: inaktiverad. |

När du har anslutits och synkroniserats:

- Valda objekt från Service Manager har importerats till OMS **logganalys.** Du kan visa hello sammanfattning av dessa arbetsobjekt på hello IT Service Management-anslutningstjänsten sida vid sida.

- Du kan skapa incidenter från OMS aviseringar eller loggen Sök från OMS, i den här instansen för Service Manager.

Mer information: [skapa ITSM arbetsobjekt för aviseringar OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts) och [skapa ITSM arbetsobjekt från OMS loggar](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs).

### <a name="create-and-deploy-service-manager-web-app-service"></a>Skapa och distribuera tjänsten för Service Manager web app

tooconnect Hej lokalt Service Manager med hello IT Service Management-anslutningstjänsten på OMS, Microsoft har skapat ett webbprogram i Service Manager på hello GitHub.

tooset konfigurera hello ITSM webbprogram för ditt Service Manager hello följande:

- **Distribuera hello webbapp** – distribuera hello webbapp, ange hello egenskaper och autentisera med Azure AD. Du kan distribuera hello webbprogram med hjälp av hello [automatiserade skript](log-analytics-itsmc-service-manager-script.md) att Microsoft tillhandahåller.
- **Konfigurera hello hybridanslutning** - [konfigurera den här anslutningen](#configure-the-hybrid-connection)manuellt.

#### <a name="deploy-hello-web-app"></a>Distribuera hello-webbapp
Använd hello automatisk [skriptet](log-analytics-itsmc-service-manager-script.md) toodeploy hello webbprogram, ange hello egenskaper och autentisera med Azure AD.

Kör hello skript genom att tillhandahålla hello följande obligatoriska information:

- Information om Azure-prenumeration
- Resursgruppens namn
- Plats
- Information om Service Manager (servernamnet, domän, användarnamn och lösenord)
- Platsen namnprefixet för ditt webbprogram
- ServiceBus-Namespace.

hello skriptet skapar hello webbapp med hello namnet du angav (tillsammans med några ytterligare strängar toomake den unika). Den genererar hello **Webbappens URL**, **klient-ID** och **klienthemlighet**.

Spara hello värden använda du dem när du skapar en anslutning med IT Service Management-anslutningstjänsten.

**Kontrollera hello Web app-installation**

1. Gå för**Azure-portalen** > **resurser**.
2. Välj hello webbapp, klicka på **inställningar** > **programinställningar**.
3. Bekräfta hello information om hello Service Manager-instans som du angav när hello distribuerar hello appen via hello skript.

### <a name="configure-hello-hybrid-connection"></a>Konfigurera hello hybridanslutning

Använd följande procedur tooconfigure hello hybridanslutning som ansluter hello Service Manager-instans med hello IT Service Management-anslutningstjänsten i OMS hello.

1. Hitta hello Service Manager Web app under **Azure-resurser**.
2. Klicka på **inställningar** > **nätverk**.
3. Under **Hybridanslutningar**, klickar du på **konfigurera slutpunkter för din hybridanslutning**.

    ![Hybridnätverk för anslutning](./media/log-analytics-itsmc/itsmc-hybrid-connection-networking-and-end-points.png)
4. I hello **Hybridanslutningar** bladet, klickar du på **lägga till hybridanslutning**.

    ![Lägg till hybridanslutning](./media/log-analytics-itsmc/itsmc-new-hybrid-connection-add.png)

5. I hello **lägga till Hybridanslutningar** bladet, klickar du på **skapa nya hybrid anslutning**.

    ![Ny hybridanslutning](./media/log-analytics-itsmc/itsmc-create-new-hybrid-connection.png)

6. Skriv hello följande värden:

    - **Namnet på slutpunkten**: Ange ett namn för hello ny hybridanslutning.
    -  **Slutpunkten värden**: FQDN för hello Service Manager-hanteringsservern.
    - **Port för slutpunkt**: Skriv 5724
    - **Servicebus-namnområdet**: Använd ett befintligt servicebus-namnområde eller skapa en ny.
    - **Plats**: Välj hello plats.
    -  **Namnet**: Ange ett namn toohello servicebus om du skapar den.

    ![Värden för hybrid-anslutning](./media/log-analytics-itsmc/itsmc-new-hybrid-connection-values.png)
6. Klicka på **OK** tooclose hello **skapa hybridanslutning** bladet och börjar skapa hello hybridanslutning.

    När hello hybridanslutningen har skapats visas den under hello-bladet.

7. När du har skapat hello hybridanslutning Välj hello anslutningen och klicka på **Lägg till valda hybridanslutning**.

    ![Ny hybridanslutning](./media/log-analytics-itsmc/itsmc-new-hybrid-connection-added.png)

#### <a name="configure-hello-listener-setup"></a>Konfigurera hello lyssnare installation

Använd hello följa proceduren tooconfigure hello lyssnare installationsprogrammet för hello hybridanslutning.

1. I hello **Hybridanslutningar** bladet, klickar du på **Download hello Anslutningshanteraren** och installera det på hello dator där System Center Service Manager-instansen körs.

    När hello installationen är klar, **Hybridanslutningshanteraren UI** alternativet är tillgängligt under **starta** menyn.

2. Klicka på **Hybridanslutningshanteraren UI** , du uppmanas att dina autentiseringsuppgifter för Azure.

3. Logga in med dina autentiseringsuppgifter för Azure och välja din prenumeration där hello hybridanslutningen har skapats.

4. Klicka på **Spara**.

Din hybridanslutning är korrekt ansluten.

![lyckad hybridanslutning](./media/log-analytics-itsmc/itsmc-hybrid-connection-listener-set-up-successful.png)
> [!NOTE]

> När hello hybridanslutningen har skapats kan kontrollera och testa hello-anslutning genom att besöka hello distribueras Service Manager-webbprogrammet. Kontrollera hello anslutningen är klar innan du försöker tooconnect toohello IT Service Management-anslutningstjänsten i OMS.

hello visar följande bild hello information om anslutningen lyckas:

![Hybrid anslutningstestet](./media/log-analytics-itsmc/itsmc-hybrid-connection-test.png)

## <a name="connect-servicenow-tooit-service-management-connector-in-oms"></a>Ansluta ServiceNow tooIT Service Management-anslutningstjänsten i OMS

hello följande avsnitt innehåller information om hur tooconnect ditt ServiceNow produkten toohello IT Service Management-anslutningstjänsten i OMS.

### <a name="prerequisites"></a>Krav

Kontrollera att du har hello följande förutsättningar uppfylls:

- IT Service Management-anslutningstjänsten installerad. Mer information: [konfiguration.](log-analytics-itsmc-overview.md#configuration)
- ServiceNow stöds versioner – Fuji, Geneva, Helsingfors.

ServiceNow Admins måste göra hello efter deras ServiceNow-instans:
- Generera klient-ID och klienthemlighet för hello ServiceNow-produkten. Mer information om hur toogenerate klient-ID och hemlighet, se [OAuth-installationen](http://wiki.servicenow.com/index.php?title=OAuth_Setup).
- Installera hello användaren App för Microsoft OMS-integrering (ServiceNow-app). [Läs mer](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.0 ).
- Skapa integration användarroll för hello användaren appen installerad. Information om hur toocreate hello integration användarrollen är [här](#create-integration-user-role-in-servicenow-app).


### <a name="connection-procedure"></a>**Proceduren för anslutning**

Använd hello följa proceduren toocreate en ServiceNow-anslutning:

1. Gå för**OMS** > **inställningar** > **anslutna källor**.
2. Välj **ITSM Connector** klickar du på **Lägg till ny anslutning**.

    ![ServiceNow-anslutning](./media/log-analytics-itsmc/itsmc-servicenow-connection.png)

3. Ange hello information enligt beskrivningen i följande tabell hello och klicka på **spara** toocreate hello anslutning:

> [!NOTE]
> Dessa parametrar är obligatoriska.

| **Fält** | **Beskrivning** |
| --- | --- |
| **Namn**   | Ange ett namn för hello ServiceNow-instans som du vill tooconnect med hello IT Service Management-anslutningstjänsten.  Du kan använda det här namnet senare i OMS när du konfigurerar arbetsobjekt i den här ITSM eller visa detaljerad logganalys. |
| **Välj typ av anslutning**   | Välj **ServiceNow**. |
| **Användarnamn**   | Ange användarnamn för hello integration som du skapade i hello ServiceNow app toosupport hello anslutning toohello IT Service Management-anslutningstjänsten. Mer information: [skapa ServiceNow app användarrollen](#create-integration-user-role-in-servicenow-app).|
| **Lösenord**   | Ange hello lösenord som associeras med det här användarnamnet. **Obs**: användarnamn och lösenord som används för att generera autentiseringstoken endast och lagras inte någonstans i hello OMS-tjänsten.  |
| **Server-URL**   | Ange hello URL för hello ServiceNow-instans som du vill tooconnect tooIT Service Management-anslutningstjänsten. |
| **Klient-ID**   | Ange hello klient-ID som du vill toouse för OAuth2-autentisering, som du tidigare genererade.  Mer information om genererar klient-ID och Hemlig: [OAuth-installationen](http://wiki.servicenow.com/index.php?title=OAuth_Setup). |
| **Klienthemlighet**   | Typen hello klienthemligheten genereras för detta ID.   |
| **Omfång för synkronisering av data**   | Välj hello ServiceNow arbetsobjekt som du vill toosync tooOMS via hello IT Service Management-anslutningstjänsten.  hello valda värden importeras till logganalys.   **Alternativ:** incidenter och ändringsbegäranden.|
| **Synkronisera Data** | Ange hello antal föregående dagar som du vill hello data från. **Maxgränsen**: 120 dagar. |
| **Skapa nytt konfigurationsobjekt i ITSM lösning** | Välj det här alternativet om du vill toocreate hello konfigurationsobjekt i hello ITSM produkten. När du väljer skapar OMS hello som påverkade konfigurationsobjekt som konfigurationsobjekt (vid CIs) i hello stöds ITSM system. **Som standard**: inaktiverad. |


När du har anslutits och synkroniserats:

- Valda objekt från ServiceNow-anslutning har importerats till OMS logganalys arbete.  Du kan visa hello sammanfattning av dessa arbetsobjekt på hello IT Service Management-anslutningstjänsten sida vid sida.
- Du kan skapa incidenter, aviseringar och händelser från OMS aviseringar eller loggfil sökning i den här instansen av ServiceNow.  


Mer information: [skapa ITSM arbetsobjekt för aviseringar OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts) och [skapa ITSM arbetsobjekt från OMS loggar](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs).

### <a name="create-integration-user-role-in-servicenow-app"></a>Skapa en användarroll för integrering i ServiceNow-app

Användaren hello nedan:

1.  Besök hello [ServiceNow store](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.0) och installera hello **användaren App för ServiceNow och Microsoft OMS-integrering** till ServiceNow-instans.
2.  Besök hello kvar navigeringsfältet i instansen av ServiceNow hello, Sök och välj Microsoft OMS integrator efter installationen.  
3.  Klicka på **checklista för installationen**.

    hello status visas som **inte slutföra** om hello användarrollen är ännu toobe skapas.

4.  I hello texten kryssrutorna bredvid i för**skapa Integrationsanvändare**, ange hello användarnamn för hello-användare som kan ansluta toohello IT Service Management-anslutningstjänsten i OMS.
5.  Ange hello lösenord för den här användaren och klicka **OK**.  

>[!NOTE]

> Du använder dessa autentiseringsuppgifter toomake hello ServiceNow-anslutning i OMS.

hello nyligen skapade användaren visas med hello standardroller tilldelad.

Standardroller:
- personalize_choices
- import_transformer
-   x_mioms_microsoft.User
-   ITIL
-   template_editor
-   view_changer

När hello användaren har skapats, hello status för **Kontrollera installationschecklista** flyttar tooCompleted, lista hello information hello användarrollen som skapats för hello app.

> [!NOTE]

> tooallow en användare toocreate **aviseringar** och **händelser** i ServiceNow från OMS:

> - Kontrollera att du har hello Event Management-modulen installerad för ServiceNow-instans.

> - Lägg till följande roller toohello integrering användaren hello:
>      - evt_mgmt_integration
>      - evt_mgmt_operator  


## <a name="connect-provance-tooit-service-management-connector-in-oms"></a>Ansluta Provance tooIT Service Management-anslutningstjänsten i OMS

hello följande avsnitt innehåller information om hur tooconnect din Provance produkten toohello IT Service Management-anslutningstjänsten i OMS.

### <a name="prerequisites"></a>Krav

Kontrollera att du har hello följande förutsättningar uppfylls:

- IT Service Management-anslutningstjänsten installerad. Mer information: [Configuration](log-analytics-itsmc-overview.md#configuration).
- Provance App ska registreras med Azure AD - och klient-ID görs tillgängligt. Detaljerad information finns i [hur tooconfigure active directory-autentisering](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).
- Användarrollen: administratör.

### <a name="connection-procedure"></a>Proceduren för anslutning

Använd hello följa proceduren toocreate en Provance anslutning:

1. Gå för**OMS** > **inställningar** > **anslutna källor**.
2. Välj **ITSM Connector** klickar du på **Lägg till ny anslutning**.  

    ![Provance anslutning](./media/log-analytics-itsmc/itsmc-provance-connection.png)
3. Ange hello information enligt beskrivningen i följande tabell hello och klicka på **spara** toocreate hello anslutning.

> [!NOTE]
> Dessa parametrar är obligatoriska.

| **Fält** | **Beskrivning** |
| --- | --- |
| **Namn**   | Ange ett namn för hello Provance-instans som du vill tooconnect med hello IT Service Management-anslutningstjänsten.  Du kan använda det här namnet senare i OMS när du konfigurerar arbetsobjekt i den här ITSM eller visa detaljerad logganalys. |
| **Välj typ av anslutning**   | Välj **Provance**. |
| **Användarnamn**   | Ange hello användarnamn som kan ansluta toohello IT Service Management-anslutningstjänsten.    |
| **Lösenord**   | Ange hello lösenord som associeras med det här användarnamnet. **Obs:** användarnamn och lösenord som används för att generera autentiseringstoken endast och lagras inte någonstans i hello OMS-tjänsten. _|
| **Server-URL**   | Ange hello URL för din Provance-instans som du vill tooconnect tooIT Service Management-anslutningstjänsten. |
| **Klient-ID**   | Ange hello klient-ID för att autentisera den här anslutningen som du genererade i Provance-instans.  Mer information om klient-ID, se [hur tooconfigure active directory-autentisering](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). |
| **Omfång för synkronisering av data**   | Välj hello Provance arbetsobjekt som du vill toosync tooOMS via hello IT Service Management-anslutningstjänsten.  Dessa objekt har importerats till logganalys fungerar.   **Alternativ:** incidenter, ändringsbegäranden.|
| **Synkronisera Data** | Ange hello antal föregående dagar som du vill hello data från. **Maxgränsen**: 120 dagar. |
| **Skapa nytt konfigurationsobjekt i ITSM lösning** | Välj det här alternativet om du vill toocreate hello konfigurationsobjekt i hello ITSM produkten. När du väljer skapar OMS hello som påverkade konfigurationsobjekt som konfigurationsobjekt (vid CIs) i hello stöds ITSM system. **Som standard**: inaktiverad.|

När du har anslutits och synkroniserats:

- Valda objekt från Provance anslutning har importerats till OMS **logganalys.**  Du kan visa hello sammanfattning av dessa arbetsobjekt på hello IT Service Management-anslutningstjänsten sida vid sida.
- Du kan skapa incidenter och händelser från OMS aviseringar eller Sök i loggfilen i den här Provance-instansen.

Mer information: [skapa ITSM arbetsobjekt för aviseringar OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts) och [skapa ITSM arbetsobjekt från OMS loggar](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs).

## <a name="connect-cherwell-tooit-service-management-connector-in-oms"></a>Ansluta Cherwell tooIT Service Management-anslutningstjänsten i OMS

hello följande avsnitt innehåller information om hur tooconnect din Cherwell produkten toohello IT Service Management-anslutningstjänsten i OMS.

### <a name="prerequisites"></a>Krav

Kontrollera att du har hello följande förutsättningar uppfylls:

- IT Service Management-anslutningstjänsten installerad. Mer information: [Configuration](log-analytics-itsmc-overview.md#configuration).
- Klient-ID har genererats. Mer information: [generera klient-ID för Cherwell](#generate-client-id-for-cherwell).
- Användarrollen: administratör.

### <a name="connection-procedure"></a>Proceduren för anslutning

Använd hello följa proceduren toocreate en Cherwell anslutning:

1. Gå för**OMS** >  **inställningar** > **anslutna källor**.
2. Välj **ITSM Connector** klickar du på **Lägg till ny anslutning**.  

    ![Cherwell användar-id](./media/log-analytics-itsmc/itsmc-cherwell-connection.png)

3. Ange hello information enligt beskrivningen i följande tabell hello och klicka på **spara** toocreate hello anslutning.

> [!NOTE]
> Dessa parametrar är obligatoriska.

| **Fält** | **Beskrivning** |
| --- | --- |
| **Namn**   | Ange ett namn för hello Cherwell-instans som du vill tooconnect toohello IT Service Management-anslutningstjänsten.  Du kan använda det här namnet senare i OMS när du konfigurerar arbetsobjekt i den här ITSM eller visa detaljerad logganalys. |
| **Välj typ av anslutning**   | Välj **Cherwell.** |
| **Användarnamn**   | Ange användarnamn för hello Cherwell som kan ansluta toohello IT Service Management-anslutningstjänsten. |
| **Lösenord**   | Ange hello lösenord som associeras med det här användarnamnet. **Obs:** användarnamn och lösenord som används för att generera autentiseringstoken endast och lagras inte någonstans i hello OMS-tjänsten.|
| **Server-URL**   | Ange hello URL för din Cherwell-instans som du vill tooconnect tooIT Service Management-anslutningstjänsten. |
| **Klient-ID**   | Ange hello klient-ID för att autentisera den här anslutningen som du genererade i Cherwell-instans.   |
| **Omfång för synkronisering av data**   | Välj hello Cherwell arbetsobjekt som du vill toosync via hello IT Service Management-anslutningstjänsten.  Dessa objekt har importerats till logganalys fungerar.   **Alternativ:** incidenter, ändringsbegäranden. |
| **Synkronisera Data** | Ange hello antal föregående dagar som du vill hello data från. **Maxgränsen**: 120 dagar. |
| **Skapa nytt konfigurationsobjekt i ITSM lösning** | Välj det här alternativet om du vill toocreate hello konfigurationsobjekt i hello ITSM produkten. När du väljer skapar OMS hello som påverkade konfigurationsobjekt som konfigurationsobjekt (vid CIs) i hello stöds ITSM system. **Som standard**: inaktiverad. |

När du har anslutits och synkroniserats:

- Valda objekt från den här anslutningen Cherwell importeras till OMS logganalys arbete. Du kan visa hello sammanfattning av dessa arbetsobjekt på hello IT Service Management-anslutningstjänsten sida vid sida.
- Du kan skapa incidenter och händelser i den här instansen av Cherwell från OMS. Mer information: skapa ITSM arbetsobjekt för OMS aviseringar och skapa ITSM arbetsobjekt från OMS-loggar.

Mer information: [skapa ITSM arbetsobjekt för aviseringar OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts) och [skapa ITSM arbetsobjekt från OMS loggar](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs).

### <a name="generate-client-id-for-cherwell"></a>Generera klient-ID för Cherwell

toogenerate hello klient-ID-nyckel för Cherwell använder hello nedan:

1. Logga in tooyour Cherwell-instans som administratör.
2. Klicka på **säkerhet** > **redigera REST API-klientinställningar**.
3. Välj **Skapa ny klient** > **klienthemlighet**.

    ![Cherwell användar-id](./media/log-analytics-itsmc/itsmc-cherwell-client-id.png)


## <a name="next-steps"></a>Nästa steg
 - [Skapa ITSM arbetsobjekt för OMS-aviseringar](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts)

 - [Skapa ITSM arbetsobjekt från OMS-loggar](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs)

- [Visa logganalys för din anslutning](log-analytics-itsmc-overview.md#using-the-solution)
