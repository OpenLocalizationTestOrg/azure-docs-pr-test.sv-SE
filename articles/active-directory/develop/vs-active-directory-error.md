---
title: "aaaHow toodiagnose fel med hello guiden för Azure Active Directory-anslutning"
description: "hello active directory-Anslutningsguiden upptäckte ett inkompatibelt autentiseringstyp"
services: active-directory
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: dd89ea63-4e45-4da1-9642-645b9309670a
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 03/05/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: f71c5b41457c0c8db05042e8d5f723e58ad11844
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="diagnosing-errors-with-hello-azure-active-directory-connection-wizard"></a>Diagnostisera fel med hello guiden för Azure Active Directory-anslutning
Vid identifiering av föregående Autentiseringskod upptäckte hello guiden ett inkompatibelt autentiseringstyp.   

## <a name="what-is-being-checked"></a>Vad är kontrolleras?
**Obs:** toocorrectly identifiera tidigare Autentiseringskod i ett projekt, hello projektet måste skapas.  Om detta fel uppstod och du inte har en tidigare Autentiseringskod i projektet, återskapa och försök igen.

### <a name="project-types"></a>Projekttyper
hello guiden kontrollerar hello typ av projekt som du utvecklar så att den kan mata in hello rätt autentiseringslogiken i hello-projekt.  Om det inte finns några domänkontrollanter som härleds från `ApiController` i hello-projektet hello projekt betraktas som ett WebAPI-projekt.  Om det finns endast domänkontrollanter som härleds från `MVC.Controller` i hello-projektet hello projekt betraktas som en MVC-projektet.  Något annat stöds inte av hello guiden.

### <a name="compatible-authentication-code"></a>Kompatibel Autentiseringskod
hello guiden kontrollerar också autentiseringsinställningar som tidigare har konfigurerats med hello guiden eller som är kompatibel med hello guiden.  Om alla inställningar finns Reentrant fall är det och hello guiden öppnas visa hello inställningar.  Om endast vissa hello inställningar finns anses ett ärende för fel.

I ett projekt med MVC söker hello guiden efter någon av följande inställningar, som kommer från tidigare användning av hello guiden hello:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

Dessutom söker hello guiden efter någon av följande inställningar i ett Web API-projekt som kommer från tidigare användning av hello guiden hello:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

### <a name="incompatible-authentication-code"></a>Inkompatibel Autentiseringskod
Slutligen försöker hello guiden toodetect versioner av Autentiseringskod som har konfigurerats med tidigare versioner av Visual Studio. Om du har fått det här felet betyder det att projektet innehåller en inkompatibel autentiseringstyp. hello guiden identifierar hello följande typer av autentisering från tidigare versioner av Visual Studio:

* Windows-autentisering 
* Enskilda användarkonton 
* Organisationskonton 

toodetect Windows-autentisering i ett projekt med MVC hello guiden söker efter hello `authentication` element från din **web.config** fil.

<pre>
    &lt;configuration&gt;
        &lt;system.web&gt;
            <span style="background-color: yellow">&lt;authentication mode="Windows" /&gt;</span>
        &lt;/system.web&gt;
    &lt;/configuration&gt;
</pre>

toodetect Windows-autentisering i ett Web API-projekt hello guiden söker efter hello `IISExpressWindowsAuthentication` element från ditt projekt **.csproj** fil:

<pre>
    &lt;Project&gt;
        &lt;PropertyGroup&gt;
            <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication&gt;enabled&lt;/IISExpressWindowsAuthentication&gt;</span>
        &lt;/PropertyGroup>
    &lt;/Project&gt;
</pre>

toodetect enskilda användarkonton autentisering hello guiden söker efter hello paketet element från din **Packages.config-fil** fil.

<pre>
    &lt;packages&gt;
        <span style="background-color: yellow">&lt;package id="Microsoft.AspNet.Identity.EntityFramework" version="2.1.0" targetFramework="net45" /&gt;</span>
    &lt;/packages&gt;
</pre>

toodetect en gamla form av autentisering av Organisationskonto hello guiden söker efter hello följande element från **web.config**:

<pre>
    &lt;configuration&gt;
        &lt;appSettings&gt;
            <span style="background-color: yellow">&lt;add key="ida:Realm" value="***" /&gt;</span>
        &lt;/appSettings&gt;
    &lt;/configuration&gt;
</pre>

toochange hello autentiseringstypen, ta bort hello inkompatibla autentiseringstyp och kör hello guiden igen.

Mer information finns i [Autentiseringsscenarier för Azure AD](active-directory-authentication-scenarios.md).

#<a name="next-steps"></a>Nästa steg
- [Autentiseringsscenarier för Azure AD](active-directory-authentication-scenarios.md)