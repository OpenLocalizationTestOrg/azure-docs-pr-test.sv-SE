---
title: "aaaGet igång med villkorlig åtkomst i Azure Active Directory | Microsoft Docs"
description: "Testa villkorlig åtkomst med ett villkor för platsen."
services: active-directory
keywords: "villkorlig åtkomst tooapps, villkorlig åtkomst med Azure AD, säker åtkomst toocompany resurser, principer för villkorlig åtkomst"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 4521f5a34f5882e026f5e58a7127d8c55cba2f0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-conditional-access-in-azure-active-directory"></a>Kom igång med villkorlig åtkomst i Azure Active Directory

Villkorlig åtkomst är en funktion av Azure Active Directory som gör att du toodefine villkor under vilka behöriga användare kan komma åt dina appar. 

Det här avsnittet innehåller anvisningar för att testa en villkorad tillgång baserat på en plats-villkor i din miljö.  


## <a name="scenario-description"></a>Scenariobeskrivning

Ett vanligt krav i många organisationer är tooonly kräver Multi-Factor authentication för åtkomst tooapps som inte utförs från hello företagsintranät. Du kan enkelt göra det här målet genom att konfigurera en princip för plats-baserad villkorlig åtkomst med Azure Active Directory. Det här avsnittet ger detaljerade anvisningar för att konfigurera en relaterad metod. Hej princip använder [tillförlitliga IP-adresser](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips) toodistinguish mellan åtkomstförsök från hello företagets har alla andra platser i intranätet och.


## <a name="prerequisites"></a>Krav

hello scenariot beskrivs i det här avsnittet förutsätter att du är bekant med hello begrepp som beskrivs i [villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-azure-portal.md).

tootest i det här scenariot måste du:

- Skapa en testanvändare 

- Tilldela en Azure AD Premium-licens toohello testanvändare

- Konfigurera en hanterad app och tilldela din test användaren tooit

- Konfigurera tillförlitliga IP-adresser

Om du behöver mer information om betrodda IP-adresser, se [tillförlitliga IP-adresser](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).


## <a name="policy-configuration-steps"></a>Konfigurationssteg för principen

**tooconfigure principen för villkorlig åtkomst gör du:**

1. I hello Azure-portalen på hello vänstra navigeringsfält för, klickar du på **Azure Active Directory**. 

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/01.png)

2. På hello **Azure Active Directory** bladet i hello **hantera** klickar du på **villkorlig åtkomst**.

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/02.png)
 
3. På hello **villkorlig åtkomst** bladet, tooopen hello **ny** bladet i hello verktygsfältet hello överst, klickar du på **Lägg till**.

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/03.png)

4. På hello **ny** bladet i hello **namn** textruta, ange ett namn för principen.

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/04.png)

5. I hello **tilldelning** klickar du på **användare och grupper**.

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/05.png)

6. På hello **användare och grupper** bladet utföra hello följande steg:

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/06.png)

    a. Klicka på **Välj användare och grupper**.

    b. Klicka på **Välj**.

    c. På hello **Välj** bladet Välj din testanvändare och klicka sedan på **Välj**.

    d. På hello **användare och grupper** bladet, klickar du på **klar**.

7. På hello **ny** bladet, tooopen hello **Molnappar** bladet i hello **tilldelning** klickar du på **Molnappar**.

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/07.png)

8. På hello **Molnappar** bladet utföra hello följande steg:

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/08.png)

    a. Klicka på **Välj appar**.

    b. Klicka på **Välj**.

    c. På hello **Välj** bladet Välj en molnapp i och klicka sedan på **Välj**.

    d. På hello **Molnappar** bladet, klickar du på **klar**.

9. På hello **ny** bladet, tooopen hello **villkor** bladet i hello **tilldelning** klickar du på **villkor**.

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/09.png)

10. På hello **villkor** bladet, tooopen hello **platser** bladet, klickar du på **platser**.

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/10.png)

11. På hello **platser** bladet utföra hello följande steg:

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/11.png)

    a. Under **konfigurera**, klickar du på **Ja**.

    b. Under **inkludera**, klickar du på **alla platser**.

    c. Klicka på **undanta**, och klicka sedan på **alla betrodda IP-adresser**.

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/12.png)

    d. Klicka på **Klar**.

12. På hello **villkor** bladet, klickar du på **klar**.

13. På hello **ny** bladet, tooopen hello **bevilja** bladet i hello **kontroller** klickar du på **bevilja**.

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/13.png)

14. På hello **bevilja** bladet utföra hello följande steg:

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/14.png)

    a. Välj **kräver Multi-Factor authentication**.

    b. Klicka på **Välj**.

15. På hello **ny** bladet under **aktiverar principen**, klickar du på **på**.

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/15.png)

16. På hello **ny** bladet, klickar du på **skapa**.


## <a name="testing-hello-policy"></a>Testar hello policy

tootest din princip du ska komma åt din app från en enhet som: 

1. Har en IP-adress som ligger inom den konfigurerade betrodda IP-adresser 

1. Har en IP-adress som inte ligger inom den konfigurerade betrodda IP-adresser

Multifaktorautentisering bör endast krävas under ett anslutningsförsök gjordes från en enhet som inte ligger inom den konfigurerade betrodda IP-adresser. 


## <a name="next-steps"></a>Nästa steg

Om du vill läsa mer om villkorlig åtkomst toolearn finns [villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-azure-portal.md).

