---
title: "Azure AD Connect-synkronisering: ändra standardkonfigurationen för hello | Microsoft Docs"
description: "Innehåller metodtips för att ändra hello standardkonfigurationen av Azure AD Connect-synkronisering."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 7638a031-1635-4942-94c3-fce8f09eed5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: aa05e935edd02c49c3c3fdc198b854f50327847c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-best-practices-for-changing-hello-default-configuration"></a>Azure AD Connect-synkronisering: bästa praxis för att ändra hello standardkonfigurationen
hello syftet med det här avsnittet är toodescribe som stöds och som inte stöds ändringar tooAzure AD Connect-synkronisering.

hello-konfiguration som skapats av Azure AD Connect fungerar ”i befintligt skick” för de flesta miljöer används för att synkroniserar lokala Active Directory med Azure AD. I vissa fall är dock nödvändigt tooapply måste vissa ändringar tooa configuration toosatisfy en viss eller krav.

## <a name="changes-toohello-service-account"></a>Ändringar toohello-tjänstkontot
Azure AD Connect-synkronisering körs under ett tjänstkonto som skapats av hello installationsguiden. Tjänstkontot innehåller hello kryptering nycklar toohello databas som används av synkronisering. Den har skapats med ett långt lösenord 127 tecken och hello lösenordet anges toonot går ut.

* Det är **stöds inte** toochange eller återställa hello-lösenord för hello-tjänstkontot. Gör det förstör hello krypteringsnycklar och hello-tjänsten är inte kan tooaccess hello databasen och inte kan toostart.

## <a name="changes-toohello-scheduler"></a>Ändringar toohello Schemaläggaren
Från och med hello versioner från version 1.1 (februari 2016) kan du konfigurera hello [scheduler](active-directory-aadconnectsync-feature-scheduler.md) toohave en annan synkronisering växla än hello som standard 30 minuter.

## <a name="changes-toosynchronization-rules"></a>Ändringar tooSynchronization regler
hello installationsguiden innehåller en konfiguration som ska toowork för hello vanligaste scenarierna. Om du behöver toomake ändringar toohello konfiguration måste du följa de här reglerna toostill har en konfiguration som stöds.

* Du kan [ändra attributflöden](active-directory-aadconnectsync-change-the-configuration.md#other-common-attribute-flow-changes) om hello standard direkt attributflöden inte är lämpliga för din organisation.
* Om du vill använda för[inte flöda attributet](active-directory-aadconnectsync-change-the-configuration.md#do-not-flow-an-attribute) och ta bort alla befintliga attribut som värden i Azure AD måste toocreate en regel för det här scenariot.
* [Inaktivera en oönskade Synkroniseringsregel](#disable-an-unwanted-sync-rule) i stället för att ta bort den. Borttagna regeln återskapas under en uppgradering.
* för[ändra en regel för out-of-box](#change-an-out-of-box-rule), bör du se en kopia av hello ursprungliga regeln och inaktiverar hello out-of-box regeln. hello Sync Rule Editor uppmanar och hjälper dig.
* Exportera anpassade Synkroniseringsregler med hjälp av hello Synchronization regler Editor. hello editor ger dig med ett PowerShell-skript som du kan använda tooeasily återskapa dem i en katastrofåterställning.

> [!WARNING]
> hello out-of-box sync regler har ett tumavtryck. Om du ändrar toothese regler, matchar inte längre hello tumavtryck. Du kan få problem i hello framtida när du försöker tooapply en ny version av Azure AD Connect. Bara göra ändringar hello sätt som det beskrivs i den här artikeln.

### <a name="disable-an-unwanted-sync-rule"></a>Inaktivera en oönskade Synkroniseringsregel
Ta inte bort en synkroniseringsregel för av out-of-box. Den återskapas under nästa uppgraderingen.

I vissa fall har hello installationsguiden resulterade i en konfiguration som inte fungerar för din topologi. Till exempel om du har ett konto-resurs skogstopologi men du har utökat hello schema i hello kontoskog med hello Exchange-schemat, skapas regler för Exchange för hello kontoskog och hello resursskogen. I det här fallet behöver du toodisable hello Synkroniseringsregel för Exchange.

![Inaktiverade synkroniseringsregel](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/exchangedisabledrule.png)

Hello bilden ovan hittade hello installationsguiden ett gamla Exchange 2003-schema i hello kontoskog. Det här schemat har lagts till innan hello resursskogen infördes i Fabrikams miljö. tooensure inga attribut från hello gamla Exchange-implementeringen är synkroniserade, hello synkroniseringsregel ska inaktiveras som visas.

### <a name="change-an-out-of-box-rule"></a>Ändra en out-of-box-regel
hello endast tid bör du ändra en out-of-box-regel är när du behöver toochange hello anslutning till regeln. Om du behöver toochange ett attributflöde bör du skapa en synkroniseringsregel med högre prioritet än hello out-of-box-regler. Hej endast regeln som du behöver praktiskt taget tooclone är hello regeln **i från AD - användare ansluta**. Du kan åsidosätta alla regler med en regel på högre prioritet.

Om du behöver toomake ändringar tooan out-of-box regel bör du göra en kopia av hello out-of-box regeln och inaktiverar hello ursprungliga regeln. Gör hello ändringar toohello klonade regeln. hello Sync Rule Editor hjälper dig med de här stegen. När du öppnar en out-of-box-regel kan visas med den här dialogrutan:  
![Varning out-of-box-regel](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/warningoutofboxrule.png)

Välj **Ja** toocreate en kopia av hello regeln. hello klonade regeln sedan öppnas.  
![Klonade regel](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/clonedrule.png)

Gör alla nödvändiga ändringar tooscope och koppling omformningar på regeln klonade.

## <a name="next-steps"></a>Nästa steg
**Översiktsavsnitt**

* [Azure AD Connect-synkronisering: Förstå och anpassa synkronisering](active-directory-aadconnectsync-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
