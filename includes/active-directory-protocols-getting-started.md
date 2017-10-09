---
title: "aaaAzure AD .NET Protokollöversikt | Microsoft Docs"
description: "Hur åt toouse HTTP meddelanden tooauthorize tooweb program och webb-API: er i din klient med hjälp av Azure AD."
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/21/2016
ms.author: priyamo
ms.openlocfilehash: 5bd54af028c445afd3f35d67d47d7c84b476c757
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## Registrera ditt program med din AD-klient
Först måste tooregister ditt program med Azure Active Directory (Azure AD)-klient. Detta kommer ger dig ett program-ID för ditt program, samt aktivera det tooreceive token.

* Logga in toohello [Azure Portal](https://portal.azure.com).
* Välj Azure AD-klienten genom att klicka på ditt konto i hello övre högra hörnet av hello-sidan.
* Hello vänstra navigeringsfönstret och klicka på **Azure Active Directory**.
* Klicka på **Appregistreringar** och klicka på **Lägg till**.
* Följ anvisningarna för hello och skapa ett nytt program. För den här kursen, spelar det ingen roll om det är en webbapp eller ett internt program, men om du behöver specifika exempel på webbprogram eller interna program kan du kolla vår [snabbstart](../articles/active-directory/develop/active-directory-developers-guide.md).
  * Ange hello för webbprogram, **inloggnings-URL** som är hello bas-URL för din app, där användarna kan logga in t.ex `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: hello **App ID URI** is a unique identifier for your application. hello convention is toouse `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Interna program, ange en **omdirigerings-URI**, vilka Azure AD används tooreturn token svar. Ange ett värde specifika tooyour program. t.ex.`http://MyFirstAADApp`
* När du har slutfört registreringen, ska Azure AD tilldela en unik klient-ID för ditt program, hello program-ID. Du behöver det här värdet i nästa avsnitt hello så kopiera den från hello appen på sidan.
