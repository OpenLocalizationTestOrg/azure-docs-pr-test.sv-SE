---
title: "aaaAzure Active Directory-program och tjänstens huvudnamn objekt | Microsoft Docs"
description: "En beskrivning av hello förhållandet mellan program och tjänstens huvudnamn objekt i Azure Active Directory"
documentationcenter: dev-center-name
author: dstrockis
manager: mbaldwin
services: active-directory
editor: 
ms.assetid: adfc0569-dc91-48fe-92c3-b5b4833703de
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/28/2016
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: ff7e308c0b326c3a32b101b7b323f2c0362763e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-and-service-principal-objects-in-azure-active-directory-azure-ad"></a>Programmet och service principal objekt i Azure Active Directory (AD Azure)
Ibland hello betydelse hello termen ”program” kan tror många när du använder hello Azure AD. hello syftet med den här artikeln är toomake den tydligare genom att förklara konceptuell och konkreta aspekter av Azure AD-integrering för programmet, med en illustration av registrering och ditt medgivande för en [flera innehavare programmet](active-directory-dev-glossary.md#multi-tenant-application).

## <a name="overview"></a>Översikt
Ett program som har integrerats med Azure AD har effekter som sträcker sig utöver hello programvara aspekt. ”Program” används ofta som en konceptuell term refererar toonot endast hello hello program, men dess Azure AD-registrering och roll i autentisering/auktorisering ”konversationer” vid körning. Per definition erbjuder ett program kan fungera i en [klienten](active-directory-dev-glossary.md#client-application) roll (förbrukar en resurs), en [resursservern](active-directory-dev-glossary.md#resource-server) roll (medför API: er tooclients) eller till och med båda. hello konversation protokollet definieras av ett [flödet för OAuth 2.0 Authorization Grant](active-directory-dev-glossary.md#authorization-grant), låta hello klient/resurs tooaccess/skydda en resurs data respektive. Nu går vi en djupare nivå och se hur hello Azure AD-programmodell representerar ett program vid designtillfället och körning. 

## <a name="application-registration"></a>Registrera programmet
När du registrerar en Azure AD-program i hello [Azure-portalen][AZURE-Portal], skapas två objekt i Azure AD-klienten: programobjekt och ett service principal-objekt.

#### <a name="application-object"></a>Programobjektet
Ett Azure AD-program definieras av det är ett och endast programobjektet, som finns i hello Azure AD-klient där programmet hello registrerades kallas ”hem” hello program-klient. hello Azure AD Graph [programmet entiteten] [ AAD-Graph-App-Entity] definierar hello schemat för ett program-objektets egenskaper. 

#### <a name="service-principal-object"></a>Tjänstens huvudnamn objekt
hello tjänstens huvudnamn objekt definierar hello principer och behörigheter för ett program i en viss klient tillhandahåller hello grund för en UPN toorepresent hello säkerhetsprogram vid körning. hello Azure AD Graph [ServicePrincipal entiteten] [ AAD-Graph-Sp-Entity] definierar hello schemat för en service principal objektets egenskaper. 

#### <a name="application-and-service-principal-relationship"></a>Program och tjänstens huvudnamn relation
Överväg att hello programobjektet som hello *globala* representation av ditt program för användning på alla klienter och hello tjänstens huvudnamn som hello *lokala* representation för användning i en specifik innehavaren. hello programobjektet fungerar som hello mallen från vilka vanliga och standardegenskaperna är *härledda* som ska användas när motsvarande service principal objekt. Ett programobjekt måste därför en 1:1-relation med hello program och en 1:many relation med dess motsvarande service principal objekt.

Ett huvudnamn för tjänsten måste skapas i varje klient där programmet hello ska användas för att aktivera den tooestablish en identitet för inloggning och/eller åtkomst tooresources som skyddas av hello-klient. En enskild klient-programmet har bara ett huvudnamn för tjänsten (i hemmet klient), vanligtvis skapas och godkänt för användning under registreringen av program. En flera innehavare Web application/API har också ett huvudnamn för tjänsten som skapats i varje klient där en användare från att klienten har godkänt tooits användning.  

> [!NOTE]
> Alla ändringar du gör tooyour programobjektet visas också i dess huvudsakliga serviceobjektet i hello program hemma klienten endast (hello klienter där den registrerades). För program med flera klienter, ändringar toohello programobjektet inte återspeglas i några konsumenten innehavare service principal objekt förrän hello-åtkomst tas bort via hello [programmet åtkomstpanelen](https://myapps.microsoft.com) och beviljas igen.
><br>  
> Observera också att interna program registreras som flera innehavare som standard.
> 
> 

## <a name="example"></a>Exempel
hello följande diagram illustrerar hello förhållandet mellan ett program programobjektet och motsvarande tjänst huvudnamn objekt hello kontexten för ett exempelprogram för flera innehavare kallad **HR app**. Det finns tre Azure AD-klienter i det här scenariot: 

* **Adatum** - hello-klienten som används av hello företag som utvecklats hello **HR-app**
* **Contoso** - hello klienten som används av hello Contoso-organisationen, vilket är konsumenter av hello **HR-app**
* **Fabrikam** - hello klienten som används av hello Fabrikam organisation som använder också hello **HR-app**

![Förhållandet mellan ett programobjekt och en tjänst för huvudobjekt](./media/active-directory-application-objects/application-objects-relationship.png)

I hello föregående diagram är steg 1 hello processen att skapa hello programmet och service principal objekt i hello program hemma klient.

I steg 2 när Contoso och Fabrikam administratörer Slutför samtycke, skapas ett huvudnamn tjänstobjekt i företagets Azure AD-klient och tilldelade hello behörigheter att hello-administratör beviljas. Tänk också på hello HR appen kan vara konfigurerat/utformats tooallow godkännande av användare för personligt bruk.

I steg3 har hello konsumenten innehavare av hello HR-program (Contoso och Fabrikam) varje sina egna tjänstens huvudnamn objekt. Representerar deras användning av en instans av programmet hello vid körning, regleras av hello behörigheter godkänt av respektive hello-administratör.

## <a name="next-steps"></a>Nästa steg
Programobjektet för ett program kan nås via hello Azure AD Graph API hello [Azure portal] [ AZURE-Portal] manifestet Redigeraren för programmet, eller [Azure AD PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0), som representeras av dess OData [programmet entiteten][AAD-Graph-App-Entity].

Programmets huvudsakliga webbtjänstobjektet kan nås via hello Azure AD Graph API eller [Azure AD PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0), som representeras av dess OData [ServicePrincipal entiteten] [ AAD-Graph-Sp-Entity].

Hej [Azure AD Graph Explorer](https://graphexplorer.azurewebsites.net/) är användbart för att fråga efter både hello program och tjänstens huvudnamn objekt.

<!--Image references-->

<!--Reference style links -->
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AZURE-Portal]: https://portal.azure.com
