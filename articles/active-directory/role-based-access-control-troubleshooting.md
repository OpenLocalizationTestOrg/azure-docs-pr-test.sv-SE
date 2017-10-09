---
title: aaaTroubleshoot Azure RBAC | Microsoft Docs
description: "Få hjälp med eller frågor om rollbaserad åtkomstkontroll resurser."
services: azure-portal
documentationcenter: na
author: andredm7
manager: femila
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: 15feced32d8459d90c4c246d335932f90e1fc91f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="role-based-access-control-troubleshooting"></a>Rollbaserad åtkomstkontroll felsökning

Det här dokumentet artikeln innehåller svar på vanliga frågor om hello särskilda behörigheter som beviljas med roller, så att du vet vilka tooexpect när använder hello roller i hello Azure-portalen och kan felsöka problem med åtkomst till. Dessa roller omfattar alla typer av resurser:

* Ägare  
* Deltagare  
* Läsare  

Ägare och deltagare har fullständig åtkomst toohello management upplevelse, men en deltagare som inte kan ge åtkomst tooother användare eller grupper. Det blir lite mer intressant med rollen för hello läsare så att den där vi tillbringar lite tid. Se hello [rollbaserad åtkomstkontroll komma-igång artikel](role-based-access-control-configure.md) information om hur toogrant åt.

## <a name="app-service-workloads"></a>App service arbetsbelastningar
### <a name="write-access-capabilities"></a>Skrivåtkomst funktioner
Om du ger en användare läsåtkomst tooa enkel webbapp inaktiveras vissa funktioner som du kan förvänta inte sig. följande funktioner för hantering av hello kräver **skriva** komma åt tooa webbprogram (medarbetare eller ägare) och inte är tillgängliga i skrivskyddat läge scenarier.

* Kommandon (till exempel start, stopp, etc.)
* Ändra inställningar för allmän konfiguration, inställningar, inställningar för säkerhetskopiering och inställningarna för övervakning
* Åtkomst till publishing autentiseringsuppgifter och andra hemligheter som app-inställningar och anslutningssträngar
* Direktuppspelningsloggar
* Diagnostikloggar konfiguration
* Konsol (Kommandotolken)
* Aktiva och senaste distributioner (för kontinuerlig lokal git-distribution)
* Uppskattad tillbringar
* Webbtester
* Virtuella nätverk (endast synliga tooa läsare om ett virtuellt nätverk redan har konfigurerats av en användare med skrivbehörighet).

Om du inte kommer åt dessa paneler, måste tooask administratören för deltagare åtkomst toohello webbprogrammet.

### <a name="dealing-with-related-resources"></a>Hantera relaterade resurser
Webbprogram komplicerade av hello förekomsten av ett par olika resurser som samspelet. Här är en typisk resursgrupp med några webbplatser:

![Resursgruppen för Web app](./media/role-based-access-control-troubleshooting/website-resource-model.png)

Därför om du ger någon åtkomst toojust hello-webbprogram, är mycket hello funktioner hello webbplatsens blad i hello Azure-portalen inaktiverad.

Dessa artiklar kräver **skriva** komma åt toohello **programtjänstplanen** som motsvarar tooyour webbplats:  

* Visa hello webbprogrammet prisnivån (lediga eller Standard)  
* Skala configuration (antal instanser, storlek på virtuell dator, Autoskala inställningar)  
* Kvoter (lagring, bandbredd, CPU)  

Dessa artiklar kräver **skriva** åtkomst toohello hela **resursgruppen** som innehåller din webbplats:  

* SSL-certifikat och bindningar (SSL-certifikat kan delas mellan platser i hello samma resursgrupp och geografiska plats)  
* Aviseringsregler  
* Autoskala inställningar  
* Application insights-komponenter  
* Webbtester  

## <a name="virtual-machine-workloads"></a>Arbetsbelastningar på virtuella datorer
Mycket som med web apps vissa funktioner på hello virtuella bladet kräver skrivbehörighet toohello virtuell dator eller tooother resurser i hello resursgrupp.

Virtuella datorer är relaterade tooDomain namn, virtuella nätverk, storage-konton och Varningsregler.

Dessa artiklar kräver **skriva** komma åt toohello **virtuella**:

* Slutpunkter  
* IP-adresser  
* Diskar  
* Tillägg  

Dessa kräver **skriva** åtkomst tooboth hello **virtuella**, och hello **resursgruppen** (tillsammans med hello domännamn) som den är i:  

* Tillgänglighetsuppsättning  
* Belastningsutjämnade uppsättningen  
* Aviseringsregler  

Om du inte kommer åt dessa paneler kan du be administratören för deltagare åtkomst toohello resursgrupp.

## <a name="see-more"></a>Mer information
* [Rollbaserad åtkomstkontroll](role-based-access-control-configure.md): komma igång med RBAC på hello Azure-portalen.
* [Inbyggda roller](role-based-access-built-in-roles.md): få information om hello roller som redan finns i RBAC.
* [Anpassade roller i Azure RBAC](role-based-access-control-custom-roles.md): Lär dig hur toocreate anpassade roller toofit din behövs.
* [Skapa en profil över åtkomständringshistorik](role-based-access-control-access-change-history-report.md): hålla reda på hur du ändrar rolltilldelningar i RBAC.

