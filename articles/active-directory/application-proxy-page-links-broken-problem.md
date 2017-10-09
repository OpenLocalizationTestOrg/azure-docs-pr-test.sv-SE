---
title: "aaaLinks på hello sidan fungerar inte för ett program med Application Proxy | Microsoft Docs"
description: "Hur tootroubleshoot problem med brutna länkar på Application Proxy-program som du har integrerat med Azure AD"
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
ms.openlocfilehash: 77c1e22d27c7a6436d8e57e105037c2328180481
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="links-on-hello-page-dont-work-for-an-application-proxy-application"></a>Länkar på hello sidan fungerar inte för ett program med Application Proxy

Den här artikeln hjälper dig tootroubleshoot varför länkar på ditt Azure Active Directory Application Proxy-program inte fungerar korrekt.

## <a name="overview"></a>Översikt 
När du har publicerat en app i Application Proxy publicerade hello endast länkar att arbete som standard i programmet hello är länkar toodestinations som ingår i hello rot-URL. hello länkar i hello program fungerar inte, hello Intern URL för programmet hello inte innehåller förmodligen alla hello mål för länkar i hello program.

**Varför händer det här?** När du klickar på en länk i ett program, hello Application Proxy försöker tooresolve hello URL: en som en intern URL inom samma program eller som ett externt tillgänglig URL. Om hello länken pekar tooan Intern URL inte är inom hello samma program inte tillhör tooeither dessa buckets och resultera i ett hittades inte.

## <a name="ways-you-can-resolve-broken-links"></a>Sätt som du kan lösa brutna länkar

Det finns tre sätt tooresolve problemet. hello alternativen nedan finns i listan i komplexitet.

1.  Kontrollera att hello Intern URL är en rotcertifikatutfärdare som innehåller alla relevanta hello-länkar för hello program. Detta gör att alla länkar toobe löst som innehåll som publiceras inom hello samma program.

    Om du ändrar hello Intern URL men inte vill toochange hello Landningssida för användare publicerade ändra hello URL-Adressen toohello tidigare Intern URL. Detta kan göras genom att gå för ”Azure Active Directory -”&gt; App registreringar -&gt; väljer programmet hello -&gt; egenskaper. Den här fliken visas i hello fältet ”startsidan URL” som du kan justera toobe hello önskad landningssida.

2.  Om dina program använder fullständigt kvalificerade domännamn (FQDN), använda [anpassade domäner](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) toopublish dina program. Den här funktionen gör hello samma URL toobe används både internt och externt.

    Det här alternativet ser till att hello länkar i ditt program är externt tillgänglig via Application Proxy eftersom hello länkar i hello toointernal URL: er identifieras också externt. Observera att alla länkar fortfarande toobelong tooa publicerade program. Med det här alternativet hello länkar inte behöver dock toobelong toohello samma program och kan höra toomultiple program.

3.  Om inget av dessa alternativ är möjlig, ansluta hello förhandsgranskning för en ny funktion som gör översättning/skriva om URL: en. Med det här alternativet vara intern URL: er eller länkar som finns i dina program hello HTML brödtext översättas, eller ”mappas”, toohello publicerade externa App Proxy URL: er. Den här metoden fungerar för länkar i hello HTML- eller CSS och det hjälper inte om länken genereras via JS. 

Därför rekommenderar vi starkt med hello [anpassade domäner](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) lösning om möjligt. Om du vill toojoin hello preview, e- < aadapfeedback@microsoft.com > med hello applicationId(s).

## <a name="next-steps"></a>Nästa steg
[Arbeta med befintliga lokala proxyservrar](application-proxy-working-with-proxy-servers.md)

