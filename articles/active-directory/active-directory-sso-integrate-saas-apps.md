---
title: aaaIntegrate Azure Active Directory enkel inloggning med SaaS-appar | Microsoft Docs
description: "Aktivera enkel inloggning, autentisering och centraliserad hantering av SaaS-appar i Azure Active Directory för användaretablering. En översikt över hur toointegrate Azure Active Directory tooSaaS appar."
services: active-directory
keywords: integrera Azure AD med SaaS-appar
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 53b9d341-d1fc-4bbb-ac7c-3f4c68fcf00a
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/17/2017
ms.author: curtand
ms.reviewer: aaronsm
ms.openlocfilehash: fe621a30429c81c32470635d105ae3e95184efa1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-active-directory-single-sign-on-with-saas-apps"></a>Integrera Azure Active Directory enkel inloggning med SaaS-appar
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-enterprise-apps-manage-sso.md)
> * [Klassisk Azure-portal](active-directory-sso-integrate-saas-apps.md)
>
>

[!INCLUDE [active-directory-sso-use-case-intro](../../includes/active-directory-sso-use-case-intro.md)]

tooget igång konfigurerar enkel inloggning för en app som du tillämpar i din organisation, du kommer att använda en befintlig katalog i Azure Active Directory (AD Azure). Du kan använda en Azure AD-katalog som du har fått via Microsoft Azure, Office 365 eller Windows Intune. Om du har två eller flera av dessa, se [administrera Azure AD-katalogen](active-directory-administer.md) toodetermine som en toouse.

> [!IMPORTANT]
> Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln. Hur tooassign administratörsroller i hello Azure AD admin center finns [Tilldela administratörsroller i Azure Active Directory](active-directory-enterprise-apps-manage-sso.md).

## <a name="authentication"></a>Autentisering
För program som stöder hello SAML 2.0 WS-Federation, eller OpenID Connect protokoll, Azure Active Directory använder signering certifikat tooestablish betrodda relationer. Mer information om detta finns [hantera certifikat för federerad enkel inloggning](active-directory-sso-certs.md).

Förtroenden i Azure Active Directory använder 'lösenordsvalv' tooestablish för program som stöder endast HTML-formulär för inloggning. Det här kan hello användare i din organisation toobe loggas in automatiskt tooa SaaS-program med Azure AD med hjälp av hello-kontoinformation från hello SaaS-program. Azure AD samlar in och hello-kontoinformation och hello relaterade lösenord lagras på ett säkert sätt. Mer information finns i [lösenordsbaserade enkel inloggning](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

## <a name="authorization"></a>Auktorisering
Ett etablerade konto kan en användare behörighet toobe toouse ett program när de har autentiserats via enkel inloggning. Användaretablering kan göras manuellt eller i vissa fall kan du lägga till och ta bort användarinformation från hello SaaS app baserat på ändringar som gjorts i Azure Active Directory. Läs mer om hur du använder befintliga Azure AD-kopplingar för automatiserad etablering av [automatisk användaretablering och avetablering för SaaS-program](active-directory-saas-app-provisioning.md).

Annars kan kan du manuellt lägga till användaren information tooan app eller använda andra etablering lösningar som är tillgängliga i hello marketplace.

## <a name="access"></a>Åtkomst
Azure AD innehåller flera anpassningsbara sätt toodeploy program tooend användare i din organisation. Du är inte låst i en viss distribution eller en lösning för åtkomst. Du kan använda [hello lösning som bäst passar dina behov](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

## <a name="additional-considerations-for-applications-already-in-use"></a>Ytterligare överväganden för program som redan används
Konfigurera enkel inloggning på för ett program som använder din organisation redan har en annan process från hello processen att skapa nya konton för ett nytt program. Det finns några förberedande steg, inklusive: mappning av användaridentiteter i hello programmet tooAzure AD identiteter och förstå hur användare får logga in tooan programmet när det har integrerats.

> [!NOTE]
> tooset in SSO för ett befintligt program du behöver toohave globala administratörsrättigheter i både Azure AD och hello SaaS-program.
>
>

### <a name="mapping-user-accounts"></a>Mappning av användarkonton
Användarens identitet har oftast en unik identifierare som kan vara en e-postadress eller användarens huvudnamn (UPN). Du behöver toolink (mappar) varje användares Programidentitet identitet tootheir respektive Azure AD. Det finns ett par sätt tooaccomplish detta beroende på hur hello krav på program-autentisering.

Mer information om hur du mappar programmet identiteter med Azure AD-identiteter finns [anpassa anspråk som utfärdats i SAML-token för hello](http://social.technet.microsoft.com/wiki/contents/articles/31257.azure-active-directory-customizing-claims-issued-in-the-saml-token-for-pre-integrated-apps.aspx) och [anpassa attributmappning för att etablera](active-directory-saas-customizing-attribute-mappings.md).

### <a name="understanding-hello-users-log-in-experience"></a>Förstå hello användarens inloggning
När du integrerar SSO för ett program som redan används, är det viktigt toorealize som hello användarupplevelsen påverkas. För alla program startar användarna använder sina Azure AD autentiseringsuppgifter toosign i. Det kan också bero på att de måste använda en annan portal tooaccess hello-program.

Enkel inloggning för vissa program kan göras på hello program loggar i gränssnittet, men andra program, hello användaren har toogo via en central portal som ([Mina appar](http://myapps.microsoft.com) eller [Office365](http://portal.office.com/myapps)) toosign i. Mer information om hello olika typer av enkel inloggning och deras användarupplevelser i [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

En annan viktig resurs är *utelämna användargodkännande* i hello [Guiding utvecklare](active-directory-applications-guiding-developers-for-lob-applications.md) artikel.

## <a name="next-steps"></a>Nästa steg
För SaaS-appar som du hittar i hello App galleriet Azure AD innehåller ett antal [självstudier om hur toointegrate SaaS-appar](active-directory-saas-tutorial-list.md).

Om appen inte är i App-galleriet, kan du [Lägg till den toohello Azure AD App-galleriet som ett anpassat program](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

Det finns mycket mer information om samtliga av dessa fel i hello Azure.com bibliotek, från och med [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).

Dessutom kan du inte missa hello [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md).
