---
title: "aaaGetting igång Azure Multi-Factor Authentication-Server | Microsoft Docs"
description: "Det här är hello Azure Multi-Factor authentication sida som beskriver hur tooget igång med Azure MFA-Server."
services: multi-factor-authentication
keywords: "autentiseringsserver, azure multifaktor autentisering appaktiveringssida, hämtning autentiseringsserver"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: e94120e4-ed77-44b8-84e4-1c5f7e186a6b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2017
ms.author: joflore
ms.reviewer: alexwe
ms.custom: it-pro
ms.openlocfilehash: 92a6a586eb96375e92a9455ad64e67221001db81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-hello-azure-multi-factor-authentication-server"></a>Komma igång med hello Azure Multi-Factor Authentication-servern

<center>![MFA lokalt](./media/multi-factor-authentication-get-started-server/server2.png)</center>

Nu när vi har fastställt toouse lokala Multi-Factor Authentication-servern gör att komma igång. Den här sidan innehåller en ny installation av hello-server och konfigurera den med lokal Active Directory. Om du redan har installerat hello MFA-servern och söker tooupgrade Se [uppgradera toohello senaste Azure Multi-Factor Authentication-servern](multi-factor-authentication-server-upgrade.md). Om du letar efter information om hur du installerar webbtjänsten hello bara se [distribuera hello Azure Multi-Factor Authentication Server Mobilappwebbtjänsten](multi-factor-authentication-get-started-server-webservice.md).

## <a name="plan-your-deployment"></a>Planera distributionen

Innan du hämtar hello Azure Multi-Factor Authentication-servern måste du tänka på belastnings- och hög tillgänglighet är. Använd den här informationen toodecide hur och var toodeploy.

En tumregel för hello mängden minne som du behöver är hello antalet användare som du förväntar dig tooauthenticate regelbundet.

| Användare | RAM |
| ----- | --- |
| 1-10,000 | 4 GB |
| 10,001-50,000 | 8 GB |
| 50,001-100,000 | 12 GB |
| 100,000-200,001 | 16 GB |
| 200,001+ | 32 GB |

Du behöver tooset in flera servrar för hög tillgänglighet eller belastningsutjämning? Det finns ett antal sätt tooset in den här konfigurationen med Azure MFA-Server. När du installerar ditt första Azure MFA-servern blir hello master. Eventuella ytterligare servrar blir underordnade och synkronisera användare och konfiguration med hello master. Sedan kan du konfigurera en primär server och har hello rest fungera som säkerhetskopiering, eller om du kan konfigurera belastningsutjämning bland alla hello-servrar.

När en master Azure MFA-Server kopplas från, kan hello underordnade servrar fortfarande bearbetar begäranden för verifiering av två steg. Du kan dock lägga till nya användare och befintliga användare kan inte uppdatera deras inställningar tills hello master är tillbaka online eller en underordnad blir befordrad.

### <a name="prepare-your-environment"></a>Förbered din miljö

Kontrollera hello-server som du använder för Azure Multi-Factor Authentication uppfyller hello följande krav:

| Krav för Azure Multi-Factor Authentication Server | Beskrivning |
|:--- |:--- |
| Maskinvara |<li>200 MB ledigt hårddiskutrymme</li><li>x32- eller x64-processor</li><li>Minst 1 GB RAM-minne</li> |
| Programvara |<li>Windows Server 2008 eller senare om hello värden är en OS-server</li><li>Windows 7 eller högre om hello värden är en klient-OS</li><li>Microsoft .NET 4.0 Framework</li><li>IIS 7.0 eller senare om installerar hello användaren portalen eller web service SDK</li> |

### <a name="azure-mfa-server-components"></a>Azure MFA-serverkomponenter

Tre webbkomponenter utgör Azure MFA-servern:

* Webbtjänst-SDK - aktiverar kommunikation med hello andra komponenter och installeras på hello Azure MFA-programserver
* Användarportalen - en IIS-webbplats som tillåter användare tooenroll i Azure Multi-Factor Authentication (MFA) och underhålla sina konton.
* Mobila App webbtjänsten - kan med hjälp av en mobilapp som hello Microsoft Authenticator-appen för tvåstegsverifiering.

Alla tre komponenter kan installeras på hello samma server om hello-servern mot internet. Om bryta hello komponenter, hello webbtjänst-SDK är installerat på hello Azure MFA-programserver och hello Användarportalen och Mobilappwebbtjänsten installeras på en server med internet-riktade.

### <a name="azure-multi-factor-authentication-server-firewall-requirements"></a>Krav för Azure Multi-Factor Authentication Server-brandvägg

Varje MFA-server måste vara kan toocommunicate på port 443 utgående toohello följande adresser:

* https://pfd.phonefactor.net
* https://pfd2.phonefactor.net
* https://css.phonefactor.net

Om utgående brandväggar begränsade på port 443, öppna hello efter IP-adressintervall:

| IP-undernät | Nätmask | IP-intervall |
|:---: |:---: |:---: |
| 134.170.116.0/25 |255.255.255.128 |134.170.116.1 – 134.170.116.126 |
| 134.170.165.0/25 |255.255.255.128 |134.170.165.1 – 134.170.165.126 |
| 70.37.154.128/25 |255.255.255.128 |70.37.154.129 – 70.37.154.254 |

Om du inte använder hello Händelsebekräftelse funktionen, och användarna inte använder tooverify för mobila appar från enheter på hello företagets nätverk, behöver du bara hello följande områden:

| IP-undernät | Nätmask | IP-intervall |
|:---: |:---: |:---: |
| 134.170.116.72/29 |255.255.255.248 |134.170.116.72 – 134.170.116.79 |
| 134.170.165.72/29 |255.255.255.248 |134.170.165.72 – 134.170.165.79 |
| 70.37.154.200/29 |255.255.255.248 |70.37.154.201 – 70.37.154.206 |

## <a name="download-hello-azure-multi-factor-authentication-server"></a>Hämta hello Azure Multi-Factor Authentication-servern

1. Logga in toohello [Azure-portalen](https://portal.azure.com) som administratör.
2. Hello vänster markerar **Active Directory**
3. Klicka på **Användare och grupper**
4. Klicka på **Alla användare**
5. Klicka på **Multi-Factor Authentication**
6. Under **multi-factor authentication** väjer du **tjänstinställningar**

   ![Sidan Tjänstinställningar](./media/multi-factor-authentication-get-started-server/servicesettings.png)

6. Hello tjänster på sidan Inställningar hello ned hello-skärmen klickar du på **gå toohello portal**. En ny sida öppnas.
7. Klicka på **Hämtningsbara filer**.
8. Klicka på hello **hämta** länka och spara hello installer.

   ![Ladda ned MFA-server](./media/multi-factor-authentication-get-started-server/download4.png)

9. Behåll den här sidan öppna som vi ska referera tooit efter körs hello installer.

## <a name="install-and-configure-hello-azure-multi-factor-authentication-server"></a>Installera och konfigurera hello Azure Multi-Factor Authentication-servern

Nu när du har hämtat hello-server kan du installera och konfigurera den. Se till att hello-server som du installerar på uppfyller krav som anges i hello planera avsnitt.

1. Dubbelklicka på hello körbara.
2. Kontrollera hello mappen är korrekt i hello Välj installationsmappen skärmen och klicka på **nästa**.
3. När hello installationen är klar klickar du på **Slutför**.  hello-konfigurationsguiden startar.
4. Kontrollera på hello guiden Välkommen skärm för konfiguration av **hoppa över med hello Autentiseringskonfigurationsguiden** och på **nästa**.  hello server startas när hello guiden stängs.

   ![Molnet](./media/multi-factor-authentication-get-started-server/skip2.png)

5. Tillbaka på hello som vi hämtat hello-server klickar du på hello **generera Aktiveringsreferenser** knappen. Kopiera informationen till hello Azure MFA-Server i hello rutor angivna och klicka på **aktivera**.

## <a name="send-users-an-email"></a>Skicka ett e-postmeddelande till användare

tooease distributionen Tillåt toocommunicate MFA-Server med dina användare. MFA-servern kan skicka ett e-tooinform dem att de har registrerats för tvåstegsverifiering.

hello e-post du skickar bestämmas med hur du konfigurerar dina användare för tvåstegsverifiering. Till exempel om du är kan tooimport telefonnummer från hello företagets katalog ska hello e-post innehålla hello standard telefonnummer så att användarna vet vilken tooexpect. Om du inte importerar telefonnummer eller om användarna ska toouse hello mobila appar, skicka dem ett e-postmeddelande som hänvisar dem toocomplete konto registreringen. Inkludera en hyperlänk toohello Azure Multi-Factor Authentication-Användarportalen i hello e-post.

hello innehållet i e-hello också varierar beroende på hello metod för verifiering som har angetts för hello användare (telefonsamtal, SMS eller mobilapp).  Till exempel om hello användaren krävs toouse en PIN-kod när de autentiserar, talar hello e-postmeddelandet om vad de första PIN-koden har ställts in på.  Användare är nödvändiga toochange sin PIN-kod under sin första kontrollen.

### <a name="configure-email-and-email-templates"></a>Konfigurera e-post och e-postmallar

Klicka på hello e-ikon på hello vänstra tooset hello inställningarna för att skicka dessa e-postmeddelanden. Den här sidan är där du kan ange hello SMTP-information för din e-server och skicka e-post genom att kontrollera hello **skicka e-post toousers** kryssrutan.

![E-postkonfiguration för MFA-server](./media/multi-factor-authentication-get-started-server/email1.png)

Du kan se hello e-mallar som är tillgängliga toochoose från på fliken för hello e-innehåll. Beroende på hur du har konfigurerat dina användare tooperform tvåstegsverifiering, väljer du hello-mall som passar dig bäst.

![E-postmallar för MFA-servern](./media/multi-factor-authentication-get-started-server/email2.png)

## <a name="import-users-from-active-directory"></a>Importera användare från Active Directory

Nu installeras hello servern ska tooadd användare. Du kan välja toocreate dem manuellt importera användare från Active Directory, eller konfigurera automatisk synkronisering med Active Directory.

### <a name="manual-import-from-active-directory"></a>Importera manuellt från Active Directory

1. Markera i hello Azure MFA-Server, hello vänster **användare**.
2. Hello längst ned i Välj **Import från Active Directory**.
3. Nu kan du antingen söka efter enskilda användare eller Sök hello AD-katalog för organisationsenheter med användare i dessa.  I det här fallet anger vi hello användare Organisationsenhet.
4. Markera alla hello användare på hello höger och klicka på **importera**.  Ett popup-meddelande visas som informerar dig om att åtgärden lyckades.  Stäng hello importera fönster.

   ![MFA-serveranvändarimport](./media/multi-factor-authentication-get-started-server/import2.png)

### <a name="automated-synchronization-with-active-directory"></a>Automatiserad synkronisering med Active Directory

1. Markera i hello Azure MFA-Server, hello vänster **katalogintegrering**.
2. Navigera toohello **synkronisering** fliken.
3. Hello längst ned i Välj **Lägg till**
4. I hello **Lägg till synkroniseringsobjekt** som visas väljer du hello domänen, Organisationsenheten **eller** säkerhetsgrupp, inställningar, standardinställningar för metod och språk som standard för synkroniseringen och på **Lägga till**.
5. Hello kryssruta med namnet **Aktivera synkronisering med Active Directory** och välj en **synkroniseringsintervall** mellan en minut och 24 timmar.

## <a name="how-hello-azure-multi-factor-authentication-server-handles-user-data"></a>Hur hello Azure Multi-Factor Authentication-servern hanterar användardata

När du använder hello Multi-Factor Authentication (MFA) Server lokalt, lagras en användares data i hello lokala servrar. Ingen beständig användarinformationen är lagrad i hello molnet. När hello användaren utför en tvåstegsverifiering, skickar hello MFA-servern data toohello Azure MFA cloud service tooperform hello verifiering. När dessa begäranden om autentisering skickas toohello Molntjänsten kan skickas hello följande fält i loggar och hello begäran så att de är tillgängliga i hello kundens autentisering/användningsrapporter. En del av hello fält är valfria så att de kan aktiveras eller inaktiveras i hello Multi-Factor Authentication-servern. hello kommunikation från hello MFA-Server toohello MFA molntjänst använder SSL/TLS via port 443 utgående. Dessa fält är:

* Unikt ID – antingen användarnamnet eller internt MFA Server-ID
* För- och efternamn (valfritt)
* E-postadress (valfritt)
* Telefonnummer – vid autentisering via röstsamtal eller SMS
* Enhetstoken – vid autentisering via mobilapp
* Autentiseringsläge
* Autentiseringsresultat
* MFA Server-namn
* MFA Server-IP
* Klientens IP – om det är tillgängligt

Dessutom toohello fälten ovan, hello verifiering resultatet (lyckade/DOS) och orsaken till några avbrott är också lagrade med hello autentiseringsdata och tillgänglig via hello autentisering/användningsrapporter.

## <a name="back-up-and-restore-azure-mfa-server"></a>Säkerhetskopiera och återställa Azure MFA Server

Se till att du har en bra säkerhetskopia är ett viktigt steg tootake med alla system.

tooback in Azure MFA-Server, se till att du har en kopia av hello **C:\Program program\multi-Factor Authentication Server\Data** mappen inklusive hello **PhoneFactor.pfdata** fil. 

Om en återställning är nödvändiga, fullständig hello följande steg:

1. Installera om Azure MFA-servern på en ny server.
2. Aktivera hello nya Azure MFA-Server.
3. Stoppa hello **MultiFactorAuth** service.
4. Skriv över hello **PhoneFactor.pfdata** med hello säkerhetskopierade kopia.
5. Starta hello **MultiFactorAuth** service.

hello nya servern är nu aktiv och körs med hello ursprungliga säkerhetskopierade konfigurations- och data.

## <a name="next-steps"></a>Nästa steg

- Installera och konfigurera hello [Användarportalen](multi-factor-authentication-get-started-portal.md) för självbetjäning.
- Installera och konfigurera hello Azure MFA-Server med [Active Directory Federation Service](multi-factor-authentication-get-started-adfs.md), [RADIUS-autentisering](multi-factor-authentication-get-started-server-radius.md), eller [LDAP-autentisering](multi-factor-authentication-get-started-server-ldap.md).
- Konfigurera [Remote Desktop Gateway och Azure Multi-Factor Authentication Server med hjälp av RADIUS](multi-factor-authentication-get-started-server-rdg.md).
- [Distribuera hello Azure Multi-Factor Authentication Server Mobilappwebbtjänsten](multi-factor-authentication-get-started-server-webservice.md).
- [Avancerade scenarier med Azure Multi-Factor Authentication och virtuella privata nätverk från tredje part](multi-factor-authentication-advanced-vpn-configurations.md).
