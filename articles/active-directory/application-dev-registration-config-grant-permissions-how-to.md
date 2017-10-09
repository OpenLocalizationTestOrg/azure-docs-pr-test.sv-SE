---
title: "aaaHow toogrant behörigheter tooa anpassad utvecklat program | Microsoft Docs"
description: "Hur toogrant behörigheter tooyour anpassad utvecklat program med hjälp av hello Azure AD-portalen eller en URL-parameter"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: e43a105fff60fbf912bdf4f60260f86ee289328d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toogrant-permissions-tooa-custom-developed-application"></a>Hur toogrant behörigheter tooa anpassad utvecklade program

Om du vill toogrant medgivande förebyggande syfte på din app eller kör i ett fel som du inte har samtyckt tooan app, kan du försöka stegen nedan.

## <a name="how-tooperform-admin-consent-for-your-application"></a>Hur tooperform Admin medgivande för ditt program

Detta påverkar hello att bevilja tillstånd toohello program för alla användare i din organisation.

1. Navigera toohello **App registreringar** bladet som en **global administratör**och välj hello app.

2. Välj **nödvändiga behörigheter**, och slutligen tryck hello **bevilja med** knappen hello överst i hello-bladet.

Alternativt kan du skapa en begäran för*login.microsoftonline.com* med konfigurationerna för appen och Lägg till på *& fråga = admin\_medgivande*. Hello app har beviljats godkännande för alla användare för när de har loggat in med administratörsautentiseringsuppgifter.

## <a name="how-tooforce-user-consent-for-your-application"></a>Hur tooforce användarens medgivande för ditt program

* Lägga till auth-begäranden *& fråga = medgivande* som kräver att slutanvändarna tooconsent varje gång de autentiserar.

## <a name="next-steps"></a>Nästa steg

[Medgivande och integrera appar tooAzureAD](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications)

[Medgivande och Permissioning för AzureAD v2.0 konvergerat appar](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-scopes)<br>

[AzureAD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
