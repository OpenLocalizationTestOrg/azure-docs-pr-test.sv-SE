---
title: aaaIntro tooauthentication i Azure Automation | Microsoft Docs
description: "Den här artikeln innehåller en översikt över Automation säkerhets- och hello olika autentiseringsmetoder tillgängliga för Automation-konton i Azure Automation."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
keywords: automation security, secure automation; automation authentication
ms.assetid: 4a6bc2f5-c5a2-4dfb-b10d-7950d750dee8
ms.service: automation
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2017
ms.author: magoedte
ROBOTS: NOINDEX
ms.openlocfilehash: 4b4409b5be010c16f7bf00a9a0f617e3617d4663
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooauthentication-in-azure-automation"></a>Introduktion tooauthentication i Azure Automation  
Azure Automation kan du tooautomate åtgärder mot resurser i Azure, lokalt, och med andra molntjänstleverantörer som Amazon Web Services (AWS).  För en runbook-tooperform åtgärderna som krävs, måste den ha behörigheter toosecurely åtkomst hello resurser med hello lägsta möjliga rättigheter som krävs inom hello prenumeration.

Den här artikeln beskriver hello olika autentiseringsscenarier som stöds av Azure Automation och visa hur tooget igång baserat på hello miljö eller miljöer måste toomanage.  

## <a name="automation-account-overview"></a>Översikt för Automation-konton
När du startar Azure Automation hello första gången, måste du skapa minst ett Automation-konto. Automation-konton kan du tooisolate ditt Automation-resurser (runbooks, tillgångar, konfigurationer) från hello resurser som ingår i andra Automation-konton. Du kan använda Automation-konton tooseparate resurser i separata logiska miljöer. Du kan exempelvis använda ett konto för utveckling, ett annat för produktion och ett annat för din lokala miljö.  Ett Azure Automation-konto skiljer sig från ditt eller dina Microsoft-konton som skapats i Azure-prenumerationen.

hello Automation resurser för varje Automation-konto som är associerade med en enda Azure region, men Automation-konton kan hantera alla hello resurser i din prenumeration. hello främsta skälet toocreate Automation-konton i olika regioner skulle vara om du har principer som kräver data och resurser toobe isolerade tooa specifik region.

> [!NOTE]
> Automation-konton och hello resurser som de innehåller som skapas i hello Azure-portalen, kan inte komma åt hello klassiska Azure-portalen. Om du vill toomanage dessa konton och sina resurser med Windows PowerShell, måste du använda hello Azure Resource Manager moduler.
>

Alla hello uppgifter som du utför mot resurser med Azure Resource Manager och hello Azure-cmdlets i Azure Automation måste autentisera tooAzure med hjälp av Azure Active Directory organisationens identitet credential-baserad autentisering.  Certifikatbaserad autentisering har hello ursprungliga autentiseringsmetod med Azure Service Management-läge, men det var komplicerad toosetup.  Autentisera tooAzure med Azure AD-användare har introducerades tillbaka i 2014 toonot endast förenkla hello processen tooconfigure ett konto för autentisering, men också stöd hello möjlighet toonon interaktivt autentisera tooAzure med ett enda användarkonto som fungerade med både Azure Resource Manager och klassiska resurser.   

För närvarande när du skapar ett nytt Automation-konto i hello Azure-portalen, skapas automatiskt:

* Kör som-konto som skapar ett nytt huvudnamn för tjänsten i Azure Active Directory, ett certifikat och tilldelar hello deltagare rollbaserad åtkomstkontroll (RBAC) som kommer att användas toomanage Resource Manager-resurser med hjälp av runbooks.
* Klassiska kör som-konto genom att ladda upp ett hanteringscertifikat som kommer att använda toomanage Azure Service Management eller klassiska resurser med hjälp av runbooks.  

Rollbaserad åtkomstkontroll är tillgängligt med Azure Resource Manager toogrant tillåtna åtgärder tooan Azure AD-användarkontot och kör som-konto och autentisering som tjänstens huvudnamn.  Läs [rollbaserad åtkomstkontroll i Azure Automation-artikel](automation-role-based-access-control.md) mer toohelp utveckla din modell för att hantera behörigheter för automatisering.  

Runbooks som körs på en Hybrid Runbook Worker i ditt datacenter eller mot computing tjänster i AWS kan inte använda hello samma metod som används oftast för runbooks som autentiserar tooAzure resurser.  Detta beror på att resurserna körs utanför Azure och därför kräver sin egen säkerhetsreferenser som definierats i Automation tooauthenticate tooresources som de ska komma åt lokalt.  

## <a name="authentication-methods"></a>Autentiseringsmetoder
hello följande tabell sammanfattas hello olika autentiseringsmetoder för varje miljö som stöds av Azure Automation och hello artikel som beskriver hur toosetup autentisering för dina runbooks.

| Metod | Miljö | Artikel |
| --- | --- | --- |
| Azure AD-användarkonto |Azure Resource Manager och Azure Service Management |[Autentisera Runbooks med Azure AD-användarkonto](automation-create-aduser-account.md) |
| Kör som-konto i Azure |Azure Resource Manager |[Autentisera runbooks med ett ”Kör som”-konto i Azure](automation-sec-configure-azure-runas-account.md) |
| Klassiskt Kör som-konto i Azure |Azure Service Management |[Autentisera runbooks med ett ”Kör som”-konto i Azure](automation-sec-configure-azure-runas-account.md) |
| Windows-autentisering |Lokalt datacenter |[Autentisera runbooks för Hybrid Runbook Worker](automation-hybrid-runbook-worker.md) |
| AWS-autentiseringsuppgifter |Amazon Web Services |[Autentisera runbooks med Amazon Web Services (AWS)](automation-config-aws-account.md) |
