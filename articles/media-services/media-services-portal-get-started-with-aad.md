---
title: "aaaGet igång med Azure AD-autentisering med hjälp av hello Azure-portalen | Microsoft Docs"
description: "Lär dig hur toouse hello Azure portal tooaccess Azure Active Directory (AD Azure) autentisering tooconsume hello Azure Media Services API."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: 497ad1806b3fd1262802adefb6e12b65ee9f2d61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-ad-authentication-by-using-hello-azure-portal"></a>Komma igång med Azure AD-autentisering med hjälp av hello Azure-portalen

Lär dig hur toouse hello Azure portal tooaccess Azure Active Directory (AD Azure) autentisering tooaccess hello Azure Media Services API.

## <a name="prerequisites"></a>Krav

- Ett Azure-konto. Om du inte har ett konto, börja med en [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/). 
- Ett Media Services-konto. Mer information finns i [skapa ett Azure Media Services-konto med hjälp av hello Azure-portalen](media-services-portal-create-account.md).
- Kontrollera att du granskar hello [åtkomst till Azure Media Services API med Azure AD-autentiseringsöversikt](media-services-use-aad-auth-to-access-ams-api.md). 

När du använder Azure AD-autentisering med Azure Media Services, har du två alternativ för autentisering:

- **Användarautentisering**. Autentisera en person som använder hello app toointeract med Media Services-resurser. hello interaktiva program bör först fråga hello användaren om autentiseringsuppgifter. Ett exempel är en management konsolapp som används av behöriga användare toomonitor kodning jobb eller live strömning. 
- **Huvudnamn autentiseringen av tjänsten**. Autentisera en tjänst. Program som ofta använder den här autentiseringsmetoden är appar som körs daemon tjänster, mellannivå tjänster eller schemalagda jobb: webbappar, funktionen appar, logikappar, API: er eller en mikrotjänster.

> [!IMPORTANT]
> Media Services stöder för närvarande hello Azure Access Control service-autentisering. Access Control tillstånd att bli inaktuell på den 1 juni 2018. Vi rekommenderar att du migrerar toohello Azure AD-autentisering så snart som möjligt.

## <a name="select-hello-authentication-method"></a>Välj autentiseringsmetod för hello

1. I hello [Azure-portalen](https://portal.azure.com/), Välj Media Services-kontot.
2. Välj hur tooconnect toohello Media Services API.

    ![Välj metoden anslutningssidan](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started01.png)

## <a name="user-authentication"></a>Användarautentisering

tooconnect toohello Media Services API med hjälp av Hej alternativet för autentisering av användare, hello-klientappen måste toorequest en Azure AD-token har hello följande parametrar:  

* Azure AD-klient slutpunkt
* Media Services resurs-URI
* Klient-ID för Media Services (intern)-program 
* Media Services (intern)-programmet omdirigerings-URI 
* Resurs-URI för REST Media Services

Du kan hämta hello värden för parametrarna på hello **Media Services-API med användarautentisering med** sidan. 

![Ansluta till sidan för autentisering av användare](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started02.png)

Om du ansluter toohello Media Services API med hjälp av hello Media Services Microsoft .NET SDK krävs hello värden är tillgänglig tooyou som en del av hello SDK. Mer information finns i [använda Azure AD authentication tooaccess hello Azure Media Services-API med .NET](media-services-dotnet-get-started-with-aad.md).

Om du inte använder klient-hello Media Services .NET SDK, måste du manuellt skapa en begäran om Azure AD-token med hjälp av hello parametrar som nämnts tidigare. Mer information finns i [hur toouse hello Azure AD Authentication Library tooget hello Azure AD-token](../active-directory/develop/active-directory-authentication-libraries.md).

## <a name="service-principal-authentication"></a>Autentisering av tjänstens huvudnamn

tooconnect toohello Media Services API med hjälp av Hej service principal alternativet, appen mellannivå (web API eller webbprogram) måste toorequest en Azure AD-token har hello följande parametrar:  

* Azure AD-klient slutpunkt
* Media Services resurs-URI 
* Resurs-URI för REST Media Services
* Azure AD application värden: hello **klient-ID** och **klienthemlighet**

Du kan hämta hello värden för parametrarna på hello **ansluta tooMedia Services API med tjänstens huvudnamn** sidan. Använd den här sidan toocreate en ny Azure AD-program eller tooselect en befintlig. När du har valt hello Azure AD-appar kan du hämta hello klient-ID (program-ID) och skapa hello klienten hemligheten (nyckel) värden. 

![Ansluta till service principal sida](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started04.png)

När hello **tjänstens huvudnamn** blad öppnas, hello första Azure AD-program som uppfyller följande kriterier hello väljs:

- Det är en registrerad Azure AD-program.
- Det har behörighet deltagare eller Owner Role-Based åtkomstkontroll på hello-konto.

När du skapar eller välj en Azure AD-app, kan du skapa och kopiera en klienthemlighet (nyckel) och hello klient-ID (program-ID). hello-klient hemlighet och klient-ID är obligatoriska tooget hello åtkomst-token i det här scenariot.

Om du inte har behörigheter toocreate Azure AD-appar i din domän, hello Azure AD app kontroller av hello bladet visas inte och visas ett varningsmeddelande.

Om du ansluter toohello Media Services API med hjälp av hello Media Services .NET SDK, se [använda Azure AD authentication tooaccess hello Azure Media Services-API med .NET](media-services-dotnet-get-started-with-aad.md).

Om du inte använder klient-hello Media Services .NET SDK, måste du manuellt skapa en Azure AD-tokenbegäran med hello parametrar som nämnts tidigare. Mer information finns i [hur toouse hello Azure AD Authentication Library tooget hello Azure AD-token](../active-directory/develop/active-directory-authentication-libraries.md).

### <a name="get-hello-client-id-and-client-secret"></a>Hämta hello klient-ID och klienten hemlighet

När du väljer en befintlig Azure AD app eller välj hello alternativet toocreate en ny, visas hello följande knappar:

![Hantera behörigheter och hantera program](./media/media-services-portal-get-started-with-aad/media-services-portal-manage.png)

tooopen hello Azure AD application-bladet, klickar du på **hantera program**. På hello **hantera program** bladet kan du hämta hello appens klient-ID (program-ID). toogenerate en klienthemlighet (nyckel), Välj **nycklar**.

![Hantera program bladet nycklar alternativet](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started06.png) 

### <a name="manage-permissions-and-hello-application"></a>Hantera behörigheter och hello program

När du har valt hello Azure AD-program kan du hantera hello program och behörigheter. tooset in din Azure AD application tooaccess andra program klickar du på **hantera behörigheter**. För hanteringsuppgifter, till exempel ändra nycklar och svars-URL: er eller tooedit hello programmets manifest, klicka på **hantera program**.

### <a name="edit-hello-apps-settings-or-manifest"></a>Redigera inställningar för hello app eller manifest

tooedit hello app inställningar eller manifestet, klickar du på **hantera program**.

![Hantera appen på sidan](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started05.png)

## <a name="next-steps"></a>Nästa steg

Kom igång med [Överför filer tooyour konto](media-services-portal-upload-files.md).
