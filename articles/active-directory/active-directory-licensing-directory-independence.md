---
title: "Egenskaperna för Azure Active Directory-klient interaktion | Microsoft Docs"
description: "Hantera dina Azure Active-klient-klienter genom att förstå klienterna som helt oberoende resurser"
services: active-tenant
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 2b862b75-14df-45f2-a8ab-2a3ff1e2eb08
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/10/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: 2c00f382546a0e5f1e9da9dc90777dcf9622ec94
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/03/2018
---
# <a name="understand-how-multiple-azure-active-directory-tenants-interact"></a>Förstå hur flera innehavare av Azure Active Directory interagera

I Azure Active Directory (Azure AD), varje klient är en helt oberoende resurser: en peer-dator som är logiskt oberoende av andra klienter som du hanterar. Det finns ingen överordnad-underordnad relation mellan klienter behålls. Detta oberoende mellan klienter innehåller resursoberoende, administrativt oberoende och synkroniseringsoberoende.

## <a name="resource-independence"></a>Resursoberoende
* Om du skapar eller tar bort en resurs i en klient har ingen inverkan på alla resurser i en annan innehavare, delvis med undantag av externa användare. 
* Om du använder en av dina domännamn med en klient kan inte användas med andra innehavare.

## <a name="administrative-independence"></a>Administrativt oberoende
Om en icke-administrativa användare av innehavaren ”Contoso” skapar en Testklient ”Test” sedan:

* Som standard är den användare som skapar en klient läggas till som en extern användare i den nya klienten och tilldelats rollen global administratör i den klienten.
* Administratörer av innehavaren ”Contoso” har ingen direkt administratörsbehörighet till klient 'Test' om inte en administratör ”test” uttryckligen beviljar dem sådan behörighet. Dock kan ”Contoso”-administratörerna styra åtkomsten till klient ”Test” om de kontrollera att användarkontot som skapade ”Test”.
* Om du lägger till/ta bort en administratörsroll för en användare i en klient, påverkar inte ändringen administratörsroller som användaren har i en annan klient.

## <a name="synchronization-independence"></a>Synkroniseringsoberoende
Du kan konfigurera varje Azure AD-klient separat om du vill synkronisera data från en enda instans av antingen:

* Verktyget Azure AD Connect att synkronisera data med en enda AD-skog.
* Azure Active innehavaren Connector för Forefront Identity Manager för att synkronisera data med en eller flera lokala skogar och/eller Azure-AD-datakällor.

## <a name="add-an-azure-ad-tenant"></a>Lägg till en Azure AD-klient
Om du vill lägga till en Azure AD-klient i Azure-portalen, logga in på [Azure-portalen](https://portal.azure.com) med ett konto som är global administratör för Azure AD och till vänster, Välj **ny**.

> [!NOTE]
> Till skillnad från andra Azure-resurser är klienterna inte underordnade resurser till en Azure-prenumeration. Om din Azure-prenumeration avbryts eller upphört att gälla, kan du fortfarande komma åt din klientdata med hjälp av Azure PowerShell, Azure Graph API eller Office 365 Admin Center. Du kan också [associera en annan prenumeration med innehavaren](active-directory-how-subscriptions-associated-directory.md).
>

## <a name="next-steps"></a>Nästa steg
En översikt av Azure AD licensproblem och bästa praxis, se [vad är Azure Active-klient licensiering?](active-directory-licensing-whatis-azure-portal.md).
