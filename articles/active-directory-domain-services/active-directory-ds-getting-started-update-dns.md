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
ms.openlocfilehash: 484ff1a197a651bccb2b416448056acf69b0d8c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="update-dns-settings-for-hello-azure-virtual-network"></a>Uppdatera DNS-inställningarna för hello Azure-nätverk
## <a name="task-4-update-dns-settings-for-hello-azure-virtual-network"></a>Uppgift 4: Uppdatera DNS-inställningarna för hello Azure-nätverk
Du har aktiverat Azure Active Directory Domain Services för din katalog i hello som föregår konfigurationsåtgärder. hello nästa uppgift är tooensure att datorer i hello virtuella nätverk kan ansluta och använda dessa tjänster. I den här artikeln uppdatera hello DNS-serverinställningarna för virtuellt nätverk toopoint toohello två IP-adresserna där Azure Active Directory Domain Services är tillgängligt på hello virtuellt nätverk.

> [!NOTE]
> När du har aktiverat Azure Active Directory Domain Services för hello directory Observera hello IP-adresser för Azure Active Directory Domain Services som visas på hello **konfigurera** för din katalog.
>
>

tooupdate hello DNS-Serverinställningen för hello virtuella nätverk som du har aktiverat Azure Active Directory Domain Services, fullständig hello följande steg:

1. Gå toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. I hello vänster och välj **nätverk**.  
    Hej **nätverk** öppnas.

    ![Fönstret virtuellt nätverk](./media/active-directory-domain-services-getting-started/virtual-network-select.png)
3. På hello **virtuella nätverk** fliken, väljer hello virtuella nätverk som du har aktiverat Azure Active Directory Domain Services tooview dess egenskaper.
4. Klicka på hello **konfigurera** fliken.

    ![Fönstret virtuellt nätverk](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)
5. I hello **DNS-servrar** ange både hello IP-adresserna som visades i hello **Domain Services** avsnittet hello **konfigurera** för din katalog.
6. toosave hello DNS-serverinställningarna för det här virtuella nätverket hello i åtgärdsfönstret längst ned hello hello-fönstret klickar du på **spara**.

   ![Uppdatera hello DNS-serverinställningarna för hello virtuellt nätverk](./media/active-directory-domain-services-getting-started/update-dns.png)

> [!NOTE]
>  Virtuella datorer i nätverket hello bara hämta hello nya DNS-inställningarna efter en omstart. Om du behöver dem tooget hello uppdatera DNS-inställningar nu direkt kan utlösa en omstart genom hello portal, PowerShell eller hello CLI.
>
>

## <a name="next-steps"></a>Nästa steg
Uppgift 5: [aktivera lösenord synkronisering tooAzure Active Directory Domain Services](active-directory-ds-getting-started-password-sync.md)
