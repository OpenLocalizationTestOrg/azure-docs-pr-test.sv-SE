---
title: "Azure Active Directory B2C: Anpassade Felsökningsprinciper | Microsoft Docs"
description: "Läs mer om metoderna toosolving fel när du arbetar med anpassade principer i Azure Active Directory."
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: joroja
ms.openlocfilehash: b9e1b46df1ddb29cdb90953e4a0d6f1dd085441f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-ad-b2c-custom-policies-and-identity-experience-framework"></a>Felsöka Azure AD B2C anpassade principer och identitet upplevelse Framework

Om du använder Azure Active Directory B2C (Azure AD B2C) anpassade principer, kan det uppstå problem konfigurerar hello identitet får Framework i sin princip språk XML-format.  Learning toowrite anpassade principer kan vara som Lär dig ett nytt språk. I den här artikeln beskrivs verktyg och tips som hjälper dig att snabbt identifiera och lösa problem. 

> [!NOTE]
> Den här artikeln fokuserar på att felsöka konfigurationen av Azure AD B2C anpassad princip. Den adressen inte hello förlitande partsprogram eller dess identitet-biblioteket.

## <a name="xml-editing"></a>XML-redigering

hello de vanligaste fel i Konfigurera anpassade principer är felaktigt formaterad XML. En bra XML-redigerare är nästan viktigt. En bra XML-redigerare visar XML internt, färgkodar innehåll, prefills vanliga termer, håller XML-element som indexeras och kan valideras med schemat. Här följer två av våra favorit XML-redigerare:

* [Visual Studio Code](https://code.visualstudio.com/)
* [Anteckningar ++](https://notepad-plus-plus.org/)

XML-schemavalideringen identifierar fel innan du laddar upp XML-filen. Hämta hello XML-schemadefinition TrustFrameworkPolicy_0.3.0.0.xsd i hello rotmapp hello startpaket. Mer information i dokumentationen för hello av XML-redigerare, leta efter *XML-verktyg* och *XML-verifiering*.

En granskning av XML-regler kan vara användbara. Azure AD B2C avvisar alla XML-formatering fel som upptäcks. Ibland kan kan felaktigt formaterad XML orsaka missvisande felmeddelanden.

## <a name="upload-policies-and-policy-validation"></a>Överför principer och Principverifiering

 XML-filen överför valideringen är automatisk. De flesta fel orsakar hello överför toofail. Verifieringen inkluderar hello principfil som överförs. Den omfattar också hello kedja av filer hello överför filen refererar för (hello principfil för förlitande part, hello tillägg och grundläggande hello-fil). 
 
 Vanliga valideringsfel inkludera hello följande.

Fel utdrag:`... makes a reference tooClaimType with id "displaName" but neither hello policy nor any of its base policies contain such an element`
* hello ClaimType värdet kan vara felstavat eller finns inte i hello schemat.
* ClaimType värden måste definieras i minst en av hello filer i hello princip. 
    Exempel: ` <ClaimType Id="socialIdpUserId">`
* Om ClaimType är definierad i hello tillägg, men den används också i ett TechnicalProfile värde i grundläggande hello-fil, överför hello grundläggande fil resulterar i ett fel.

Fel utdrag:`...makes a reference tooa ClaimsTransformation with id...`
* Hej orsaker för hello fel kan vara hello samma som för hello ClaimType fel.

Fel utdrag:`Reason: User is currently logged as a user of 'yourtenant.onmicrosoft.com' tenant. In order toomanage 'yourtenant.onmicrosoft.com', please login as a user of 'yourtenant.onmicrosoft.com' tenant`
* Kontrollera att hello TenantId värdet i hello  **\<TrustFrameworkPolicy\>**  och  **\<BasePolicy\>**  element matcha mål-Azure AD B2C innehavaren.  

## <a name="troubleshoot-hello-runtime"></a>Felsöka hello runtime

* Använd `Run Now` och `https://jwt.io` tootest dina principer oberoende av webb- eller mobila program. Den här webbplatsen fungerar som ett förlitande partsprogram. Den visar hello innehållet i hello JSON-Webbtoken (JWT) som genereras av Azure AD B2C-principen. toocreate ett testprogram i Identity upplevelse Framework, Använd hello följande värden:
    * Namn: TestApp
    * Webbapp/webb API: Nej
    * -Klienten: Nej

* tootrace hello utbyte av meddelanden mellan klientens webbläsare och Azure AD B2C, Använd [Fiddler](http://www.telerik.com/fiddler). Det kan hjälpa dig få en indikation på om dina användare resa misslyckas i orchestration-steg.

* I **utvecklingsläge**, använda **Programinsikter** tootrace hello aktiviteten hos din identitet upplevelse Framework användaren resa. I **utvecklingsläge**, du kan se hello utbyten av anspråk mellan hello identitet upplevelse Framework och hello olika anspråk providers som definieras av teknisk profiler, till exempel identitetsleverantörer, API-baserade tjänster hello Azure AD B2C användarkatalog och andra tjänster som Azure flera-Factor-autentisering.  

## <a name="recommended-practices"></a>Rekommenderade metoder

**Spara flera versioner av dina scenarier. Gruppera dem i ett projekt med ditt program.** hello bas, tillägg och förlitande part filer är direkt beroende av varandra. Spara dem som en grupp. När nya funktioner läggs tooyour principer, ha olika fungerande-versioner. Steget fungerar versioner i din egen filsystem med hello programkod som de nyttjar.  Dina program kan anropa många olika förlitande part principer i en klient. De kan bli beroende hello anspråk som de förväntar dig från dina Azure AD B2C-principer.

**Utveckla och testa teknisk profiler med kända användare resor.** Använd testade starter pack principer tooset in dina tekniska profiler. Testa dem separat innan du lägga till dem i din egen resor för användaren.

**Utveckla och testa användare resor med testade tekniska profiler.** Ändra hello orchestration-steg för en användare resa inkrementellt. Skapa progressivt din avsedda scenarier.

## <a name="next-steps"></a>Nästa steg

* Hämta hello [active-directory-b2c-custom-policy-starterpack] (https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) ZIP-filen i GitHub.
