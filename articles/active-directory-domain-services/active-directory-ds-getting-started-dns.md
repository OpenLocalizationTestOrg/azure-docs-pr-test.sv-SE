---
title: "Azure Active Directory Domain Services: Uppdatera DNS-inställningarna för hello virtuella Azure-nätverket | Microsoft Docs"
description: "Komma igång med Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: d4f3e82c-6807-4690-b298-4eabad2b7927
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/27/2017
ms.author: maheshu
ms.openlocfilehash: e6eaff555cb9b7bb89ab7581d8de0b8cfc844529
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-preview"></a>Aktivera Azure Active Directory Domain Services (förhandsversion)

## <a name="task-4-update-dns-settings-for-hello-azure-virtual-network"></a>Uppgift 4: uppdatera DNS-inställningarna för hello Azure-nätverk
Du har aktiverat Azure Active Directory Domain Services för din katalog i hello som föregår konfigurationsåtgärder. hello nästa uppgift är tooensure att datorer i hello virtuella nätverk kan ansluta och använda dessa tjänster. I den här artikeln uppdatera hello DNS-serverinställningarna för virtuellt nätverk toopoint toohello två IP-adresserna där Azure Active Directory Domain Services är tillgängligt på hello virtuellt nätverk.

tooupdate hello DNS-Serverinställningen för hello virtuella nätverk som du har aktiverat Azure Active Directory Domain Services, fullständig hello följande steg:

1. Hej **översikt** fliken visas en uppsättning **krävs konfigurationssteg** toobe utföras när din hanterade domän är helt etablerad. hello första konfigurationssteget är **uppdatera DNS-serverinställningarna för det virtuella nätverket**.

    ![Domain Services - översiktsflik vid full etablering](./media/getting-started/domain-services-provisioned-overview.png)

2. När din domän är helt etablerad, visas två IP-adresser i den här panelen. IP-adresserna representerar en domänkontrollant för din hanterade domän.

3. toocopy hello första IP-Adressen tooclipboard och klickar på hello Kopiera knappen Nästa tooit. Klicka på hello **konfigurera DNS-servrar** knappen.

4. Klistra in hello första IP-adressen i hello **lägga till DNS-server** TextBox-kontroll i hello **DNS-servrar** bladet. Rullar vågrätt toohello lämnas toocopy hello andra IP-adressen och klistra in den i hello **lägga till DNS-server** textruta.

    ![Domain Services – uppdatera DNS](./media/getting-started/domain-services-update-dns.png)

5. Klicka på **spara** när du är klar tooupdate hello DNS-servrar för hello virtuellt nätverk.

> [!NOTE]
> Virtuella datorer i nätverket hello bara hämta hello nya DNS-inställningarna efter en omstart. Om du behöver dem tooget hello uppdatera DNS-inställningar nu direkt kan utlösa en omstart genom hello portal, PowerShell eller hello CLI.
>
>

## <a name="next-step"></a>Nästa steg
[Uppgift 5: aktivera lösenord synkronisering tooAzure Active Directory Domain Services](active-directory-ds-getting-started-password-sync.md)
