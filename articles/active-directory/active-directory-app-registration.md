---
title: aaaAzure Active Directory App Registration | Microsoft Docs
description: "Den här artikeln beskriver hur hello toouse Azure portal tooregister ett program med Azure Active Directory"
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: 7dc7b89f-653f-405a-b5f4-2c1288720c15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: priyamo
ms.reviewer: elisol
ms.openlocfilehash: 0134e299dcc53919a6f789a0878a1cf64a8e244d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="register-your-application-with-your-azure-active-directory-tenant"></a>Registrera ditt program med Azure Active Directory-klient

Du kan använda hello Azure portal tooregister ditt program med Azure Active Directory (Azure AD)-klient. Detta skapar ett program-ID för programmet hello och aktiverar det. tooreceive token.

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Välj Azure AD-klienten genom att välja kontot i hello övre högra hörnet av hello-sidan.
3. Hello vänstra navigeringsfönstret och välj **fler tjänster**, klickar du på **App registreringar**, och klicka på **Lägg till**.
4. Följ anvisningarna för hello och skapa ett nytt program. Om du vill att specifika exempel för webbprogram och interna program, Kolla in våra [Snabbstart](active-directory-developers-guide.md).
  * Ange hello för webbprogram, **inloggnings-URL**, vilket är hello bas-URL för din app, där användarna kan logga in t.ex `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: hello **App ID URI** is a unique identifier for your application. hello convention is toouse `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Interna program, ange en **omdirigerings-URI**, vilka Azure AD använder tooreturn token svar. Ange ett värde specifika tooyour program. t.ex.`http://MyFirstAADApp`
5. När du har slutfört registreringen, tilldelar Azure AD ditt program ett unikt klient-ID, hello program-ID.

## <a name="update-application-settings-from-hello-azure-portal"></a>Uppdatera inställningar för program från hello Azure-portalen

Du kan enkelt ändra ett befintligt program inställningar med hjälp av hello Azure-portalen. Du kan exempelvis vilja tooconfigure en reply-URL som är där Azure AD utfärdar token svar. Du kan också tooconfigure behörigheter tooother program, för instansen tooallow program-tooaccess hello Microsoft Graph API. Du kan göra allt detta via hello programinställningssidan.

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Välj Azure AD-klienten genom att välja kontot i hello övre högra hörnet av hello-sidan.
3. Hello vänstra navigeringsfönstret och välj **fler tjänster**, klickar du på **App registreringar**, och välj programmet hello-listan.
4. Klicka på **inställningar** tooopen in hello inställningssidan för hello program.
  * Hej **egenskaper** sida kan du ändra hello allmän information för hello program. Detta inkluderar hello programnamn, hello inloggnings-URL och hello logga ut URL.
  * Hej **Reply URL: er** sidan kan du tooadd en reply-URL som är där Azure AD skickar token svar.
  * Hej **ägare** sidan kan du tooadd ägare.
  * Hej **behörigheter** sidan kan du tooconfigure behörigheter för hello app. Exempel på tooaccess hello Microsoft Graph API **Lägg till** och välj **Microsoft Graph** i hello API selector Välj hello behörighet som krävs, till exempel **läsa katalogdata** .
  * Hej **nycklar** sidan kan du tooadd programmet hemligheter. hello hemlighet visas bara en gång omedelbart efter att skapa, så se till att toocopy den för framtida användning.

## <a name="use-hello-inline-manifest-editor"></a>Använd hello infogade manifestet redigerare

Du kan använda hello infogade manifestet editor toomodify vissa egenskaper som inte exponeras direkt i hello Azure-portalen. Exempelvis kan du använda det toomodify hello programmet App-ID URI eller tooenable hello OAuth2.0 implicita flödet i stället för hello standard auktorisering ge koden flödet.

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Välj Azure AD-klienten genom att välja kontot i hello övre högra hörnet av hello-sidan.
3. Hello vänstra navigeringsfönstret och välj **fler tjänster**, klickar du på **App registreringar**, och välj programmet hello-listan.
4. Klicka på **Manifest** från hello programmet tooopen hello infogade manifestet sidredigeraren.
5. Direkt kan du göra ändringar toohello manifest och spara den när du är klar. Alternativt kan du hämta hello manifestet tooopen uppdateras den i din favorit-redigeraren och ladda upp hello manifest.

## <a name="next-steps"></a>Nästa steg

1. Kolla in hello [Snabbstart](active-directory-developers-guide.md) för detaljerad genomgång av program som utför autentisering med hjälp av Azure AD.
2. Kolla in våra fullständig lista över kodexempel i [GitHub](https://github.com/azure-samples).
