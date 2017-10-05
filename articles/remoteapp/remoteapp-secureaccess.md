---
title: "Skydda åtkomst till Azure RemoteApp och utöver | Microsoft Docs"
description: "Lär dig hur säker åtkomst till Azure RemoteApp med hjälp av villkorlig åtkomst i Azure Active Directory"
services: remoteapp
documentationcenter: 
author: piotrci
manager: mbaldwin
ms.assetid: a19b0b09-ab26-4beb-80bb-33a46da39887
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 28ee4698515d11964e5371628d21dbc00687c861
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="securing-access-to-azure-remoteapp-and-beyond"></a>Skydda åtkomst till Azure RemoteApp och utöver
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.
> 
> 

I den här artikeln ska vi ger en översikt över hur en administratör kan ställa in en säker åtkomst kanal börjar från slutanvändaren via Azure RemoteApp och slutar med en säker resurs, till exempel en SQL-databas eller ett annat program serverdel. Målet är att se till att endast behöriga användare som uppfyller villkor som önskade kan komma åt fjärrprogram och att säkra serverdel kan bara kommas åt från den kontrollerade Azure RemoteApp-miljön och inte från andra platser.

Det finns 3 huvudområden som administratören behöver titta på:

![Överväganden för villkorlig åtkomst för Azure RemoteApp](./media/remoteapp-secureaccess/ra-conditionalenvironment.png)

Läs om information och svaren på dessa frågor.

## <a name="who-can-access-the-collection"></a>Vem som kan komma åt samlingen?
Administratören väljer de användare som har åtkomst till fjärrprogram i samlingen. Du kan använda Azure Active Directory (AD Azure) arbets- eller skolkonton (tidigare kallade, ”organisationskonton”) eller Microsoft-konton (t.ex. @outlook.com). De flesta företagsscenarier använda Azure AD-konton de kan du använda villkorlig åtkomst funktioner beskrivs senare och också är det enda alternativet för domänanslutna samlingar. Resten av artikeln förutsätter att du använder Azure AD-konton med Azure RemoteApp.

**Vad vi har slutfört:**

Använder Azure AD-konton för att styra åtkomst till Azure RemoteApp får vi två saker:

1. Vi vet alltid som kan komma åt de program som vi har publicerat och komma åt-servrar programmen ansluta till.
2. Vi styra underliggande Azure AD så att vi kan skapa och ta bort användarkonton, ange lösenordsprinciper använda multifaktorautentisering, osv. 

## <a name="how-is-the-collection-accessed-from-where"></a>Hur är mängden tillgängligt? Varifrån?
Administratörer vill ofta att definiera principer för åtkomst till en offentlig mot Internet-miljö, exempelvis Azure RemoteApp. Till exempel vill de se till att användare med åtkomst till miljön från utanför företagets nätverk måste använda multifaktorautentisering (MFA) för att komma åt; eller kanske de ska blockeras helt och hållet.

Azure RemoteApp-administratörer kan använda funktionerna som är tillgängliga via Azure AD Premium för att ange principer för villkorlig åtkomst för sina Azure RemoteApp-miljö. De kan också använda omfattande rapporter och aviseringar funktioner för att övervaka hur miljön används.

### <a name="how-to-set-up-conditional-access-for-azure-remoteapp"></a>Hur du konfigurerar villkorlig åtkomst för Azure RemoteApp
Vi kommer att gå igenom ett exempelscenario – Azure RemoteApp-administratören vill blockera åtkomst till miljön när användare utanför företagsnätverket.

> [!NOTE]
> Vi förutsätter att du har uppgraderat Azure AD Premium-nivån och att du har skapat minst en Azure RemoteApp-samling.
> 
> 

1. I Azure portal klickar du på den **Active Directory** fliken. Klicka sedan på den katalog som du vill konfigurera.
   
   Kom ihåg: Villkorlig åtkomst är en egenskap för din katalog och inte av Azure RemoteApp, så alla konfigurationen är klar på katalognivå. Detta betyder också att du måste vara directory-administratören att göra dessa ändringar.
2. Klicka på **program**, och klicka sedan på **Microsoft Azure RemoteApp** att ställa in villkorlig åtkomst. Observera att du kan ställa in villkorlig åtkomst för varje ”programvara som en tjänst”-program i din katalog separat.
   ![Konfigurera villkorlig åtkomst för Azure RemoteApp](./media/remoteapp-secureaccess/ra-conditionalaccessscreen.png)
3. På den **konfigurera** ställer du in **aktivera åtkomstregler** on.
   ![Aktivera åtkomstregler för Azure RemoteApp](./media/remoteapp-secureaccess/ra-enableaccessrules.png)
4. Nu kan du konfigurera olika regler och Välj vem som ska använda dem till:
   
   1. Välj **blockera åtkomst när de inte är på arbetet** att helt hindra användare från att komma åt Azure RemoteApp utanför nätverksmiljön som du anger.
   2. Klicka på alternativ nedan för att definiera de IP-adressintervall som utgör ditt ”betrodda nätverk”. Allt utanför de återställs.
5. Testa konfigurationen genom att starta Azure RemoteApp-klienten från en IP-adress utanför intervallet som du angav. När du loggar in med dina autentiseringsuppgifter för Azure AD bör du se ett meddelande så här:

![Neka åtkomst till Azure RemoteApp](./media/remoteapp-secureaccess/ra-accessdenied.png)

### <a name="future-conditional-access-features"></a>Funktioner för framtida villkorlig åtkomst
Azure Active Directory-teamet arbetar med nya funktioner för villkorlig åtkomst. Administratörer kommer att kunna skapa nya typer av regler utanför nätverket plats baserat regler. En förhandsversion av den nya funktionaliteten ska snart vara tillgänglig.

### <a name="how-to-monitor-access-to-azure-remoteapp"></a>Så här övervakar du åtkomst till Azure RemoteApp
En bra funktion att använda tillsammans med villkorlig åtkomst är Azure Active Directory Premium rapportfunktioner. Du kan använda rapporter för att övervaka vem som har åtkomst till din miljö och identifiera eventuella misstänkt aktivitet.

Du kan till exempel se namnen på de användare som kan komma åt Azure RemoteApp, hur många gånger som de gjorde det och när.

1. I Azure-portalen klickar du på **Active Directory**, och klicka sedan på din katalog.
2. Gå till den **rapporter** fliken.
3. Välj i listan över rapporter **programanvändning** under **integrerade program**.
   
   Du ser vissa statistik för Azure RemoteApp. 
   ![Sammanställda statistik för Azure RemoteApp-åtkomst](./media/remoteapp-secureaccess/ra-accessstats.png)
4. Klicka på programmet som ska visa information om användare som ansluter till Azure RemoteApp.
   ![Användaren åtkomst statistik för Azure RemoteApp](./media/remoteapp-secureaccess/ra-userstats.png)

### <a name="summary"></a>Sammanfattning
Med Azure Active Directory Premium kan du konfigurera regler för åtkomst till Azure RemoteApp (och annan programvara som en tjänstprogram som är tillgängliga via Azure AD). Regler för närvarande är begränsade till den plats baserat nätverksprinciper men i framtiden kommer utökas till andra aspekter av enterprise management.

Azure AD Premium ger också rapportering och övervakningsfunktionerna som utökar kontrollen administratören har över sin Azure RemoteApp-miljö.

## <a name="how-do-i-make-sure-my-secure-resource-is-accessible-only-from-my-azure-remoteapp-environment"></a>Hur gör jag att mitt säker resurs är enbart tillgänglig från Azure RemoteApp-miljön?
I föregående avsnitt i den här artikeln fokuserar på att skydda åtkomsten till Azure RemoteApp-miljön. Vi har du erhålla genom att välja vilka användare som har åtkomst och konfigurera åtkomstregler för att ytterligare kontrollera hur de kan använda tjänsten.

Ett vanligt scenario för distribution av Azure RemoteApp är att fjärrprogram behöver kommunicera med en backend-resurs, till exempel en SQL-databas. Den här resursen finns antingen lokalt (t.ex. i ett företagsnätverk) eller i molnet (exempelvis i Azure IaaS). Administratörer vill ofta kontrollera att backend-resursen kan endast användas av program som distribueras via Azure RemoteApp och inte till exempel av ett program som körs direkt på en användares dator och hämtar offentliga Internet. Azure RemoteApp visas ofta som centralt hanterad och säker miljö och därmed endast sökvägen som användarna ska interagera med backend-resursen.

Lösningen är att placera både Azure RemoteApp-miljön och säker resurs i samma Azure virtuella nätverk (VNET). Om resursen finns i en annan plats, kan du upprätta en plats-till-plats VPN-anslutning, till exempel skapa ett VNet utsträckning Azure-datacentret och kundens lokala miljö.

Azure RemoteApp stöder två typer av samlingen distributioner där du kan ange egna virtuella nätverk:

* Icke-domänanslutna: program som har ”fri” andra resurser i VNET. T.ex, detta kan användas för att ansluta program till en SQL-databas som använder SQL-autentisering (program autentiserar användaren direkt mot databasen)
* Domänanslutna: de virtuella datorerna som används av Azure RemoteApp är anslutna till en domänkontrollant i VNET. Detta är användbart när programmen behöver autentiseras mot en Windows-domänkontrollant för att få åtkomst till en backend-resurs.
  ![En domänansluten samling i Azure RemoteApp](./media/remoteapp-secureaccess/ra-domainjoined.png)

### <a name="how-to-create-a-secure-connection-between-azure-and-my-on-premises-environment"></a>Så här skapar du en säker anslutning mellan Azure och min lokala miljö
Det finns flera konfigurationsalternativ för att ansluta dina Azure och lokala miljöer. En bra översikt över alternativen finns här.

Med Azure RemoteApp måste du först konfigurera ditt VNet och använder den under skapandeprocessen för din samling. 

## <a name="the-complete-solution"></a>Den kompletta lösningen
Diagrammet nedan visar den kompletta lösningen där vi har skapat en kanal för säker åtkomst från slutanvändaren via Azure RemoteApp (ARA), till backend-resurs.
![Skydda Azure RemoteApp](./media/remoteapp-secureaccess/ra-secureoverview.png) i steg 1 vi valt användarna och skapa åtkomstregler som styr hur ARA kan nås. I exemplet nedan tillåta vi bara åtkomst för användare som arbetar från företagsnätverket. Icke-kompatibla användare kommer inte att kunna komma åt ARA-miljön på alla.
I steg 2 har vi exponerad backend-resurs endast via VNet/VPN-konfiguration som vi kontrollerar. Azure RemoteApp har placerats i samma virtuella nätverk. Slutresultatet är resursen kan endast nås via ARA-miljö.

