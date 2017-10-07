---
title: aaaCharacteristics av Azure Active Directory-klient intercaction | Microsoft Docs
description: "Hantera dina Azure Active-klient-klienter genom att förstå klienterna som helt oberoende resurser"
services: active-tenant
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 2b862b75-14df-45f2-a8ab-2a3ff1e2eb08
ms.service: active-tenant
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/27/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: 57b677665c7cb4aee63f518c39d26754fe71a999
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-how-multiple-azure-active-directory-tenants-interact"></a>Förstå hur flera innehavare av Azure Active Directory interagera

I Azure Active Directory (Azure AD), varje klient är en helt oberoende resurser: en peer-dator som är logiskt oberoende av hello andra klienter som du hanterar. Det finns ingen överordnad-underordnad relation mellan klienter behålls. Detta oberoende mellan klienter innehåller resursoberoende, administrativt oberoende och synkroniseringsoberoende.

## <a name="resource-independence"></a>Resursoberoende
* Om du skapar eller tar bort en resurs i en klient har ingen inverkan på alla resurser i en annan innehavare med hello partiella undantag av externa användare. 
* Om du använder en av dina domännamn med en klient kan inte användas med andra innehavare.

## <a name="administrative-independence"></a>Administrativt oberoende
Om en icke-administrativa användare av innehavaren ”Contoso” skapar en Testklient ”Test” sedan:

* Som standard läggs hello-användare som skapar en klient som en extern användare i den nya innehavaren och tilldelade hello rollen som global administratör i den klienten.
* hello administratörer för ”Contoso”-klienten har ingen direkt administratörsbehörighet tootenant 'Test' om inte en administratör ”test” uttryckligen beviljar dem sådan behörighet. ”Contoso”-administratörerna kan dock styra åtkomst tootenant ”Test” om de styr hello användarkontot som skapade ”Test”.
* Om du lägger till/ta bort en administratörsroll för en användare i en klient, hello ändringen har inte påverkar hello administratörsroller som hello användare har i en annan klient.

## <a name="synchronization-independence"></a>Synkroniseringsoberoende
Du kan konfigurera varje Azure AD-klient oberoende tooget synkronisera data från en enda instans av antingen:

* hello Azure AD Connect-verktyget, toosynchronize data med en enda AD-skog.
* hello Azure Active-klient Connector för Forefront Identity Manager, toosynchronize data med en eller flera lokala skogar och/eller Azure-AD-datakällor.

## <a name="add-an-azure-ad-tenant"></a>Lägg till en Azure AD-klient
tooadd en Azure AD-klient i hello Azure-portalen, logga in för[hello Azure-portalen](https://portal.azure.com) med ett konto som är global administratör för Azure AD och hello vänster, Välj **ny**.

> [!NOTE]
> Till skillnad från andra Azure-resurser är klienterna inte underordnade resurser till en Azure-prenumeration. Om din Azure-prenumeration avbryts eller upphört att gälla, kan du fortfarande komma åt din klientdata med hjälp av Azure PowerShell, hello Azure Graph API eller hello Office 365 Admin Center. Du kan även associera en annan prenumeration med hello innehavaren.
>

## <a name="next-steps"></a>Nästa steg
En översikt av Azure AD licensproblem och bästa praxis, se [vad är Azure Active-klient licensiering?](active-directory-licensing-whatis-azure-portal.md)
