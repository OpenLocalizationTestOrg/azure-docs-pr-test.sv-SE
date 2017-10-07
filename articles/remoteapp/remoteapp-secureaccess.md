---
title: "aaaSecuring åtkomst tooAzure RemoteApp och därutöver | Microsoft Docs"
description: "Lär dig hur säker åtkomst tooAzure RemoteApp med hjälp av villkorlig åtkomst i Azure Active Directory"
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
ms.openlocfilehash: 98dfe69e2f5fa5014b6eb3157e01f131c287134d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="securing-access-tooazure-remoteapp-and-beyond"></a>Skydda åtkomst tooAzure RemoteApp och senare
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

I den här artikeln ska vi ger en översikt över hur en administratör kan ställa in en säker åtkomst kanal börjar från hello slutanvändaren via Azure RemoteApp och slutar med en säker resurs, till exempel en SQL-databas eller ett annat program serverdel. hello målet är toomake till att endast auktoriserade användare möte hello önskad villkor kan komma åt fjärrprogram och som hello säker backend-bara är tillgänglig från Hej kontrolleras Azure RemoteApp-miljön och inte från andra platser.

Det finns 3 huvudområden Hej administratör måste toolook på:

![Överväganden för villkorlig åtkomst för Azure RemoteApp](./media/remoteapp-secureaccess/ra-conditionalenvironment.png)

Innehåller information om information och svar toothese frågor.

## <a name="who-can-access-hello-collection"></a>Vem som kan komma åt hello samlingen?
Hej administratör väljer hello-användare som har åtkomst till fjärrprogram i hello samling. Du kan använda Azure Active Directory (AD Azure) arbets- eller skolkonton (tidigare kallade, ”organisationskonton”) eller Microsoft-konton (t.ex. @outlook.com). De flesta företagsscenarier använda Azure AD-konton de kan du använda villkorlig åtkomst-funktioner som beskrivs senare och är också hello endast alternativ för domänanslutna samlingar. hello resten av hello artikeln förutsätter att du använder Azure AD-konton med Azure RemoteApp.

**Vad vi har slutfört:**

Med hjälp av Azure AD-konton toocontrol åtkomst tooAzure RemoteApp får vi två saker:

1. Alltid vet vem som kan komma åt hello program vi har publicerat och åtkomst till några tillbaka ends programmen ansluta till.
2. Vi styra hello underliggande Azure AD så att vi kan skapa och ta bort användarkonton, ange lösenordsprinciper använda multifaktorautentisering, osv. 

## <a name="how-is-hello-collection-accessed-from-where"></a>Hur används hello samlingen? Varifrån?
Administratörer vill ofta toodefine principer för åtkomst till en offentlig mot Internet-miljö, exempelvis Azure RemoteApp. Till exempel vill de tooensure att användare som ansluter till hello-miljö från utanför företagsnätverket hello har toouse multifaktorautentisering (MFA) toogain access. eller kanske de ska blockeras helt och hållet.

Azure RemoteApp-administratörer kan använda hello-funktioner som är tillgängliga via Azure AD Premium tooset villkorliga åtkomstprinciper för sina Azure RemoteApp-miljön. De kan också använda omfattande rapporter och aviseringar funktioner toomonitor hur hello miljö används.

### <a name="how-tooset-up-conditional-access-for-azure-remoteapp"></a>Hur tooset konfigurerar villkorlig åtkomst för Azure RemoteApp
Vi är pågående toowalk via ett exempelscenario – hello Azure RemoteApp administratör tooblock åtkomst toohello miljön när användare utanför hello företagsnätverket.

> [!NOTE]
> Vi förutsätter att du har uppgraderat toohello Azure AD Premium-nivån och att du har skapat minst en Azure RemoteApp-samling.
> 
> 

1. I Azure-portalen klickar du på hello **Active Directory** fliken. Klicka sedan på hello-katalog som du vill tooconfigure.
   
   Kom ihåg: Villkorlig åtkomst är en egenskap i din katalog och inte av Azure RemoteApp, så att all konfiguration kommer att göras vid hello katalog-nivå. Detta betyder också att du behöver toobe hello directory administratören toomake ändringarna.
2. Klicka på **program**, och klicka sedan på **Microsoft Azure RemoteApp** tooset konfigurerar villkorlig åtkomst. Observera att du kan ställa in villkorlig åtkomst för varje ”programvara som en tjänst”-program i din katalog separat.
   ![Konfigurera villkorlig åtkomst för Azure RemoteApp](./media/remoteapp-secureaccess/ra-conditionalaccessscreen.png)
3. På hello **konfigurera** ställer du in **aktivera åtkomstregler** tooON.
   ![Aktivera åtkomstregler för Azure RemoteApp](./media/remoteapp-secureaccess/ra-enableaccessrules.png)
4. Du kan nu konfigurera olika regler och välja som tooapply dem till:
   
   1. Välj **blockera åtkomst när de inte är på arbetet** toocompletely hindra användare från att komma åt Azure RemoteApp utanför hello nätverksmiljö som du anger.
   2. Klicka på hello alternativet nedan toodefine hello IP-adressintervall som utgör ditt ”betrodda nätverk”. Allt utanför de återställs.
5. Testa konfigurationen genom att starta hello Azure RemoteApp-klienten från en IP-adress utanför hello-intervall som du angav. När du loggar in med dina autentiseringsuppgifter för Azure AD bör du se ett meddelande så här:

![Nekade åtkomst tooAzure RemoteApp](./media/remoteapp-secureaccess/ra-accessdenied.png)

### <a name="future-conditional-access-features"></a>Funktioner för framtida villkorlig åtkomst
hello Azure Active Directory-teamet arbetar med nya funktioner för villkorlig åtkomst. Administratörer kommer att kunna toocreate nya typer av regler utanför nätverket plats baserat regler. En offentlig förhandsgranskning av nya funktioner för hello bör snart vara tillgänglig.

### <a name="how-toomonitor-access-tooazure-remoteapp"></a>Hur toomonitor åt tooAzure RemoteApp
En bra funktionen toouse tillsammans med villkorlig åtkomst är hello Azure Active Directory Premium rapportfunktioner. Du kan använda rapporter toomonitor som har åtkomst till din miljö och identifiera eventuella misstänkt aktivitet.

Du kan till exempel se hello namnen på hello-användare som kan komma åt Azure RemoteApp, hur många gånger som de gjorde det och när.

1. I Azure-portalen klickar du på **Active Directory**, och klicka sedan på din katalog.
2. Gå toohello **rapporter** fliken.
3. Välj hello listan över rapporter **programanvändning** under **integrerade program**.
   
   Du ser vissa statistik för Azure RemoteApp. 
   ![Sammanställda statistik för Azure RemoteApp-åtkomst](./media/remoteapp-secureaccess/ra-accessstats.png)
4. Klicka på hello programmet tooreveal information om användare som ansluter till Azure RemoteApp.
   ![Användaren åtkomst statistik för Azure RemoteApp](./media/remoteapp-secureaccess/ra-userstats.png)

### <a name="summary"></a>Sammanfattning
Med Azure Active Directory Premium kan du ställa in åtkomst regler tooAzure RemoteApp (och annan programvara som en tjänstprogram som är tillgängliga via Azure AD). Regler är för närvarande begränsad toonetwork plats baserat principer, men i hello framtida utökas tooother aspekter av enterprise management.

Azure AD Premium ger också rapportering och övervakningsfunktionerna som utökar hello kontrollen Hej administratör har över sin Azure RemoteApp-miljö.

## <a name="how-do-i-make-sure-my-secure-resource-is-accessible-only-from-my-azure-remoteapp-environment"></a>Hur gör jag att mitt säker resurs är enbart tillgänglig från Azure RemoteApp-miljön?
I föregående avsnitt i den här artikeln fokuserar på att skydda åtkomst toohello Azure RemoteApp-miljön. Vi har du erhålla genom att välja hello användare tillåts åtkomst och ställa in åtkomstkontroll regler toofurther hur de kan använda hello-tjänsten.

Ett vanligt scenario för distribution av Azure RemoteApp är att hello fjärrprogram måste toocommunicate med en backend-resurs, till exempel en SQL-databas. Den här resursen finns antingen lokalt (t.ex. i ett företagsnätverk) eller i molnet hello (t.ex. i Azure IaaS). Administratörer vill ofta toomake till att hello backend-resurs bara är tillgänglig genom program som distribueras via Azure RemoteApp och inte till exempel ett program som körs direkt på en användares dator och hämtar offentliga Internet. Azure RemoteApp visas ofta när hello centralt hanterad och säker miljö och därmed hello endast sökväg som användarna ska interagera med hello backend-resurs.

hello-lösning är tooplace både hello Azure RemoteApp-miljön och hello säker resurs i hello samma Azure virtuella nätverk (VNET). Om hello resursen är i en annan plats, kan du upprätta en plats-till-plats VPN-anslutning, till exempel toocreate ett VNet spanning hello Azure datacenter och hello kundens lokala miljö.

Azure RemoteApp stöder två typer av samlingen distributioner där du kan ange egna virtuella nätverk:

* Icke-domänanslutna: hello program har ”fri” av hello andra resurser i hello virtuella nätverk. Detta kan till exempel vara används tooconnect program tooa SQL-databas som använder SQL-autentisering (program autentisera hello användaren direkt mot hello-databasen)
* Domänanslutna: hello virtuella datorer som används av Azure RemoteApp är domänansluten tooa domänkontrollant i hello virtuella nätverk. Detta är användbart när hello program behöver tooauthenticate mot en Windows-domänkontrollant i ordning tooget åtkomst tooa backend-resursen.
  ![En domänansluten samling i Azure RemoteApp](./media/remoteapp-secureaccess/ra-domainjoined.png)

### <a name="how-toocreate-a-secure-connection-between-azure-and-my-on-premises-environment"></a>Hur toocreate en säker anslutning mellan Azure och min lokala miljö
Det finns flera konfigurationsalternativ för att ansluta dina Azure och lokala miljöer. En bra översikt över hello alternativ finns här.

Du behöver tooconfigure ditt VNet först med Azure RemoteApp och använder den under hello skapandeprocessen av din samling. 

## <a name="hello-complete-solution"></a>hello komplett lösning
hello diagrammet nedan visar hello komplett lösning där vi har skapat en kanal för säker åtkomst från hello användaren via Azure RemoteApp (ARA), i hello backend-resurs.
![Skydda Azure RemoteApp](./media/remoteapp-secureaccess/ra-secureoverview.png) i steg 1 vi valt hello användare och skapa åtkomstregler som styr hur ARA kan nås. I hello exemplet nedan tillåta vi bara åtkomst för användare arbetar hello företagsnätverket. Icke-kompatibla användare inte kan tooaccess hello ARA miljö alls.
I steg 2 har vi exponerad hello backend resurs endast via hello VNet/VPN-konfiguration som vi kontrollerar. Azure RemoteApp har placerats i hello samma virtuella nätverk. hello slutresultatet är hello resurs kan endast nås via hello ARA miljö.

