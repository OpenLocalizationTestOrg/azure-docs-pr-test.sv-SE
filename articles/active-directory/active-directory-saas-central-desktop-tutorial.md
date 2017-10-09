---
title: "Självstudier: Azure Active Directory-integrering med Central Desktop | Microsoft Docs"
description: "Lär dig hur toouse centrala skrivbordet med Azure Active Directory tooenable enkel inloggning, Automatisk etablering och mycket mer!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: b805d485-93db-49b4-807a-18d446c7090e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: 93036ae801c446ce476288c00579931ba10a843b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Självstudier: Azure Active Directory-integrering med Central Desktop
hello syftet med den här kursen är tooshow hello integreringen av Azure och centrala skrivbordet. hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:

* En giltig Azure-prenumeration
* En Central skrivbordet för enkel inloggning aktiverad prenumeration / centrala desktop-klient

hello-scenario som beskrivs i den här kursen består av följande byggblock hello:

* Aktivera hello programintegrationstyp för Central skrivbord
* Konfigurera enkel inloggning (SSO)
* Konfigurera användaretablering
* Tilldela användare

![Scenariot](./media/active-directory-saas-central-desktop-tutorial/IC769558.png "Scenario")

## <a name="enable-hello-application-integration-for-central-desktop"></a>Aktivera hello programintegrationstyp för Central skrivbord
hello syftet med det här avsnittet är toooutline hur tooenable hello programintegrationstyp för Central skrivbordet.

**tooenable hello programintegrationstyp för Central skrivbordet utför hello följande steg:**

1. I hello klassiska Azure-portalen på hello vänstra navigationsfönstret klickar du på **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Active Directory")
2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.
3. tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.
   
   ![Program](./media/active-directory-saas-central-desktop-tutorial/IC700994.png "program")
4. Klicka på **Lägg till** på hello hello sidans nederkant.
   
   ![Lägg till program](./media/active-directory-saas-central-desktop-tutorial/IC749321.png "lägga till program")
5. På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.
   
   ![Lägga till ett program från gallerry](./media/active-directory-saas-central-desktop-tutorial/IC749322.png "lägga till ett program från gallerry")
6. I hello **sökrutan**, typen **centrala Desktop**.
   
   ![Programgalleriet](./media/active-directory-saas-central-desktop-tutorial/IC769559.png "programgalleriet")
7. I resultatfönstret hello väljer **centrala Desktop**, och klicka sedan på **Slutför** tooadd hello program.
   
   ![Central Desktop](./media/active-directory-saas-central-desktop-tutorial/IC769560.png "centrala Desktop")
   
## <a name="configure-single-sign-on"></a>Konfigurera enkel inloggning

hello syftet med det här avsnittet är toooutline hur tooenable användare tooauthenticate tooCentral skrivbordet med sitt konto i Azure AD med hjälp av federation baserat på hello SAML-protokoll.

Som en del av den här proceduren är obligatoriska tooupload en Base64-kodade certifikatet tooyour centrala Desktop-klient.  
Om du inte är bekant med den här proceduren, se [hur tooconvert en binär certifikat i en textfil](http://youtu.be/PlgrzUZ-Y1o).

**tooconfigure enkel inloggning, utföra hello följande steg:**

1. I hello klassiska Azure-portalen på hello **centrala Desktop** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello ** Konfigurera enkel inloggning ** dialogrutan.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-central-desktop-tutorial/IC749323.png "Konfigurera enkel inloggning")
2. På hello **hur skulle du som användare toosign på tooCentral Desktop** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-central-desktop-tutorial/IC777628.png "Konfigurera enkel inloggning")
3. På hello **konfigurera App-URL** , utför följande steg hello, och klickar sedan på **nästa**: 
   
   1. I hello **Desktop tecken i URL: en Central** textruta typen hello URL för din centrala Desktop-klient (t.ex.: *http://contoso.centraldesktop.com*).
   2. Ange din centrala Desktop AssertionConsumerService URL i hello centrala Desktop Reply URL textruta (t.ex.: https://contoso.centraldesktop.com/saml2-assertion.php).
   
   >[!NOTE]
   >Du kan hämta hello värdet från hello centrala skrivbord metadata (t.ex.: *http://contoso.centraldesktop.com*).
   >  
   
   ![Konfigurera app-URL](./media/active-directory-saas-central-desktop-tutorial/IC769561.png "konfigurera app-URL")
4. På hello **Konfigurera enkel inloggning på centrala Desktop** sida, toodownload ditt certifikat klickar du på **hämta certifikat**, och sedan spara hello certifikatfilen på datorn.
   
  ![Konfigurera enkel inloggning](./media/active-directory-saas-central-desktop-tutorial/IC769562.png "Konfigurera enkel inloggning")
5. Logga in tooyour **centrala Desktop** klient.
6. Gå för**inställningar**, klickar du på **Avancerat**, och klicka sedan på **enkel inloggning**.
   
  ![-Installation - avancerade](./media/active-directory-saas-central-desktop-tutorial/IC769563.png "-installation - avancerade")
7. På hello **enkel inloggning på inställningar** utför hello följande steg:
   
  ![Enkel inloggning på inställningar](./media/active-directory-saas-central-desktop-tutorial/IC769564.png "enkel inloggning på inställningar")
   
  1. Välj **aktivera SAML v2 för enkel inloggning**.
  2. I hello klassiska Azure-portalen på hello **Konfigurera enkel inloggning på centrala Desktop** sidan, kopiera hello **utfärdar-URL** värdet och klistrar in den i hello **SSO URL** textruta.
  3. I hello klassiska Azure-portalen på hello **Konfigurera enkel inloggning på centrala Desktop** sidan, kopiera hello **Remote inloggnings-URL** värdet och klistrar in den i hello **SSO inloggnings-URL**textruta.
  4. I hello klassiska Azure-portalen på hello **Konfigurera enkel inloggning på centrala Desktop** sidan, kopiera hello **tjänst-URL för enkel Sign-Out** värdet och klistrar in den i hello **SSO logga ut URL** textruta.
8. I hello **meddelandet signatur verifieringsmetod** avsnittet, utföra hello följande steg:
   
   ![Meddelandet signatur verifieringsmetod](./media/active-directory-saas-central-desktop-tutorial/IC769565.png "meddelande signatur verifieringsmetod")
   
  1. Välj **certifikat**.
  2. Från hello **SSO certifikat** väljer **RSH SHA256**.
  3. Skapa en textfil från hello hämtat certifikat, kopiera hello innehåll på hello textfil och klistra in den i hello **SSO certifikat** fältet.  
     >[!TIP]
     >Mer information finns i [hur tooconvert en binär certifikat i en textfil](http://youtu.be/PlgrzUZ-Y1o)
      >  
   4. Välj **visas en länk tooyour SAMLv2 inloggningssida**.
9. Klicka på **uppdatering**.
10. Välj hello konfiguration för enkel inloggning bekräftelse på hello klassiska Azure-portalen, och klicka sedan på **Slutför** tooclose hello **Konfigurera enkel inloggning** dialogrutan.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-central-desktop-tutorial/IC769566.png "Konfigurera enkel inloggning")
    
## <a name="configure-user-provisioning"></a>Konfigurera användaretablering

De måste vara etablerade toohello centrala Desktop-programmet för AAD användare toobe kan toosign i. Det här avsnittet beskrivs hur toocreate AAD användarkonton i centrala Desktop.

**tooprovision användaren konton tooCentral skrivbordet:**
1. Logga in tooyour centrala Desktop-klient.
2. Gå för**personer \> inre medlemmar**.
3. Klicka på **lägga till inre medlemmar**.
   
  ![Personer](./media/active-directory-saas-central-desktop-tutorial/IC781051.png "personer")
4. I hello **e-postadressen för nya medlemmar** textruta skriver ett AAD-konto du vill tooprovision och klicka sedan på **nästa**.
   
  ![E-postadresser för nya medlemmar](./media/active-directory-saas-central-desktop-tutorial/IC781052.png "e-postadresser för nya medlemmar")
5. Klicka på **lägga till interna medlem(mar)**.
   
  ![Lägga till inre medlem](./media/active-directory-saas-central-desktop-tutorial/IC781053.png "lägga till inre medlem")
   
   >[!NOTE]
   >hello-användare som du har lagt till får ett e-postmeddelande som innehåller en bekräftelse-länk som de behöver tooclick tooactivate hello-konto.
   > 

>[!NOTE]
>Du kan använda något annat centrala Desktop användarens konto skapas verktyg eller API: er som tillhandahålls av centrala Desktop tooprovision AAD-användarkonton
>  

## <a name="assign-users"></a>Tilldela användare
tootest konfigurationen, måste toogrant hello Azure AD-användare som du vill med hjälp av ditt program åtkomst tooit genom att tilldela dem tooallow.

**tooassign användare tooCentral skrivbordet utför hello följande steg:**

1. Skapa ett testkonto i hello klassiska Azure-portalen.
2. På hello **centrala Desktop** integreringssidan för programmet, klickar du på **tilldela användare**.
   
   ![Tilldela användare](./media/active-directory-saas-central-desktop-tutorial/IC769567.png "tilldela användare")
3. Välj din testanvändare, klicka på **tilldela**, och klicka sedan på **Ja** tooconfirm tilldelningen.
   
   ![Ja](./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Ja")

Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen. Mer information om hello åtkomstpanelen finns [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

