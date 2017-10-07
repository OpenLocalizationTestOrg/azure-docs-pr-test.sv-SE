---
title: "Azure AD Connect: Uppdatera hello SSL-certifikat för en grupp i Active Directory Federation Services (AD FS) | Microsoft Docs"
description: "Det här dokumentet information hello steg tooupdate hello SSL-certifikatet för en AD FS-servergrupp med hjälp av Azure AD Connect."
services: active-directory
keywords: "Azure ad connect, AD FS ssl-uppdatering, uppdatering för AD FS-certifikat, ändra AD FS-certifikat, nya AD FS-certifikat, adfs certifikat, uppdatera AD FS ssl-certifikat, uppdatera ssl-certifikat adfs, konfigurera AD FS ssl-certifikat, AD FS, ssl, certifikat, adfs-tjänsten certifikat för kommunikation, update-federation, konfigurera federation, aad-anslutning"
authors: anandyadavmsft
manager: femila
editor: billmath
ms.assetid: 7c781f61-848a-48ad-9863-eb29da78f53c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: anandy
ms.openlocfilehash: bce7f75aab83b6abacb8472a6895054d137e10e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="update-hello-ssl-certificate-for-an-active-directory-federation-services-ad-fs-farm"></a>Uppdatera hello SSL-certifikat för en grupp i Active Directory Federation Services (AD FS)

## <a name="overview"></a>Översikt
Den här artikeln beskriver hur du kan använda Azure AD Connect tooupdate hello SSL-certifikat för en grupp i Active Directory Federation Services (AD FS). Du kan använda hello Azure AD Connect-verktyget tooeasily uppdatering hello SSL-certifikat för hello AD FS-servergrupp, även om hello användaren inloggningsmetod valt inte är AD FS.

Du kan utföra hello hela åtgärden för att uppdatera SSL-certifikat för hello AD FS-servergrupp över alla federation och Webbprogramproxy (WAP) servrar i tre enkla steg:

![Tre steg](./media/active-directory-aadconnectfed-ssl-update/threesteps.png)


>[!NOTE]
>toolearn mer information om certifikat som används av AD FS finns [Förstå certifikat som används av AD FS](https://technet.microsoft.com/library/cc730660.aspx).

## <a name="prerequisites"></a>Krav

* **AD FS-servergrupp**: Kontrollera att AD FS-gruppen är baserade på Windows Server 2012 R2 eller senare.
* **Azure AD Connect**: se till att hello versionen av Azure AD Connect är 1.1.443.0 eller senare. Du ska använda hello uppgiften **uppdatering AD FS SSL-certifikat**.

![Uppdatera SSL-aktivitet](./media/active-directory-aadconnectfed-ssl-update/updatessltask.png)

## <a name="step-1-provide-ad-fs-farm-information"></a>Steg 1: Ange information för AD FS-servergrupp

Azure AD Connect försöker automatiskt av tooobtain information om hello AD FS-servergrupp:
1. Hämtar information om hello servergruppen från AD FS (Windows Server 2016 eller senare).
2. Refererar till hello information från tidigare körs som lagras lokalt med Azure AD Connect.

Du kan ändra hello listan över servrar som visas genom att lägga till eller ta bort hello servrar tooreflect hello aktuella konfiguration hello AD FS-servergrupp. Så snart hello serverinformation anges, visar Azure AD Connect hello anslutning och aktuell status för SSL-certifikat.

![AD FS-serverinformation](./media/active-directory-aadconnectfed-ssl-update/adfsserverinfo.png)

Om hello listan innehåller en server som inte längre tillhör hello AD FS-servergrupp, klickar du på **ta bort** toodelete hello server hello listan över servrar i din AD FS-servergrupp.

![Offline-server i listan](./media/active-directory-aadconnectfed-ssl-update/offlineserverlist.png)

>[!NOTE]
> Ta bort en server hello listan över servrar för en AD FS-grupp i Azure AD Connect är en lokal åtgärd och uppdateringar hello information för hello AD FS-servergrupp som Azure AD Connect underhåller lokalt. Azure AD Connect ändra inte hello konfigurationen på AD FS tooreflect hello ändras.    

## <a name="step-2-provide-a-new-ssl-certificate"></a>Steg 2: Ange ett nytt SSL-certifikat

När du har bekräftat hello information om AD FS-servergruppen, begär Azure AD Connect hello nytt SSL-certifikat. Ange en lösenordsskyddad PFX toocontinue hello certifikatinstallation.

![SSL-certifikat](./media/active-directory-aadconnectfed-ssl-update/certificate.png)

När du har angett hello certifikat går Azure AD Connect igenom ett antal krav. Kontrollera hello certifikat tooensure som hello certifikatet är korrekt för hello AD FS-grupp:

-   hello är ämne namn/alternativa ämnesnamnet för certifikatet hello antingen hello samma som hello federationstjänstens namn eller så är ett jokerteckencertifikat.
-   hello certifikatet är giltigt för mer än 30 dagar.
-   hello certifikatkedjan för certifikatet är giltigt.
-   hello certifikatet är lösenordsskyddat.

## <a name="step-3-select-servers-for-hello-update"></a>Steg 3: Välj servrar för hello uppdatering

Välj hello-servrar som behöver toohave hello SSL-certifikat uppdateras i hello nästa steg. Servrar som är offline kan inte väljas för hello uppdatering.

![Välj servrar tooupdate](./media/active-directory-aadconnectfed-ssl-update/selectservers.png)

När du har slutfört konfigurationen hello visar Azure AD Connect hello-meddelande som anger hello status för hello-uppdatering och ger en alternativ tooverify hello AD FS-inloggning.

![Slutför konfiguration](./media/active-directory-aadconnectfed-ssl-update/configurecomplete.png)   

## <a name="faqs"></a>Vanliga frågor och svar

* **Vad bör vara hello ämnesnamnet för certifikatet för hello för hello nya AD FS SSL-certifikat?**

    Azure AD Connect kontrollerar om hello Certifikatämnets namn/alternativa ämnesnamnet för certifikatet hello innehåller hello federationstjänstens namn. Om ditt namn för federationstjänsten är fs.contoso.com, till exempel vara hello Certifikatämnets namn/alternativa ämnesnamn fs.contoso.com.  Jokerteckencertifikat också accepteras.

* **Varför jag tillfrågas om autentiseringsuppgifter igen på serversidan för hello WAP?**

    Om hello autentiseringsuppgifter som du anger för att ansluta tooAD FS-servrarna inte också har hello privilegium toomanage hello WAP-servrar, frågar Azure AD Connect för autentiseringsuppgifter som har administratörsbehörighet på hello WAP-servrar.

* **hello-server visas som offline. Vad ska jag göra?**

    Azure AD Connect kan inte utföra någon åtgärd om hello-servern är offline. Om hello server är en del av hello AD FS-servergrupp, kontrollerar du hello anslutningen toohello server. När du har löst problemet hello, trycker du på hello uppdatera ikonen tooupdate hello status i hello guiden. Om hello servern var en del av hello servergruppen tidigare men nu finns inte längre, klickar på **ta bort** toodelete den hello listan över servrar som Azure AD Connect upprätthåller. Ta bort hello server hello listan i Azure AD Connect ändra inte hello AD FS-konfigurationen sig själv. Om du använder AD FS i Windows Server 2016 eller senare, hello server finns kvar i hello konfigurationsinställningar och kommer att visas igen körs hello nästa gång hello aktiviteten.

* **Kan jag uppdatera en delmängd av servrarna i servergruppen med hello nytt SSL-certifikat?**

    Ja. Du kan alltid köra hello uppgiften **uppdatering SSL-certifikat** igen tooupdate hello återstående servrar. På hello **väljer du servrar för SSL-certifikatet uppdatering** sidan kan du sortera hello listan över servrar **SSL utgångsdatum** tooeasily hello klientåtkomstservrar som ännu inte har uppdaterats.

* **Jag bort hello servern i hello tidigare kör, men det fortfarande visas som offline och visas på sidan för hello AD FS-servrarna. Varför är hello kvar offline servern även när du tagit bort det?**

    Ta bort hello server hello listan i Azure AD Connect inte ta bort den i hello AD FS-konfigurationen. Azure AD Connect refererar till AD FS (Windows Server 2016 eller senare) för information om hello servergruppen. Om hello server fortfarande är kvar i hello AD FS-konfigurationen, visas den i hello lista.  

## <a name="next-steps"></a>Nästa steg

- [Azure AD Connect och federation](active-directory-aadconnectfed-whatis.md)
- [Active Directory Federation Services-hantering och anpassning med Azure AD Connect](active-directory-aadconnect-federation-management.md)
