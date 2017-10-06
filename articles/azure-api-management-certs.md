---
title: aaaUpload ett certifikat om Azure API | Microsoft Docs
description: "Lär dig hur tooupload athe Management API-certifikatet för hello klassiska Azure-portalen."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: na
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: adegeo
ms.openlocfilehash: 8294d7131cfb01dba664bd4fd04b6fc22c1e93ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-an-azure-management-api-management-certificate"></a>Ladda upp ett Hanteringscertifikat för Azure API för hantering
Certifikat kan tooauthenticate med hello klassiska distributionsmodellen som tillhandahålls av Azure. Många program och verktyg (till exempel Visual Studio eller hello Azure SDK) använda dessa certifikat tooautomate konfiguration och distribution av olika Azure-tjänster. 

> [!WARNING]
> Var försiktig! Dessa typer av certifikat att alla som autentiserar med dem toomanage hello prenumeration de hör till.
>
>

Om du vill ha mer information om Azure certifikat (inklusive att skapa ett självsignerat certifikat), se [certifikat översikt för Azure Cloud Services](cloud-services/cloud-services-certs-create.md#what-are-management-certificates).

Du kan också använda [Azure Active Directory](https://azure.microsoft.com/en-us/services/active-directory/) tooauthenticate-klientkod för automatisering.

## <a name="upload-a-management-certificate"></a>Ladda upp ett hanteringscertifikat
När du har ett certifikat skapas, (.cer-fil med endast hello offentlig nyckel) kan du överföra den till hello-portalen. När hello certifikat är tillgänglig i hello portal, kan alla med ett matchande certifikat (privat nyckel) ansluta till hello Management API och komma åt hello resurser för hello associerade prenumeration.

1. Logga in toohello [Azure-portalen](http://portal.azure.com).
2. Klicka på **fler tjänster** hello längst ned i Azure-tjänsten listan Välj **prenumerationer** i hello _allmänna_ tjänstgruppen.

    ![Prenumerationen-menyn](./media/azure-api-management-certs/subscriptions_menu.png)

3. Kontrollera att tooselect hello korrekt prenumeration som du vill tooassociate med hello certifikat.     
4. När du har valt hello korrekt prenumeration, tryck på **hanteringscertifikat** i hello _inställningar_ grupp.

    ![Inställningar](./media/azure-api-management-certs/mgmtcerts_menu.png)

5. Tryck på hello **överför** knappen.

    ![Ladda upp på sidan för certifikat](./media/azure-api-management-certs/certificates_page.png)
6. Fyll i hello dialogrutan information och tryck på **överför**.

    ![Inställningar](./media/azure-api-management-certs/certificate_details.png)

## <a name="next-steps"></a>Nästa steg
Nu när du har ett certifikat som är associerade med en prenumeration kan du (när du har installerat hello matchande certifikat lokalt) via programmering ansluta toohello [klassiska distributionsmodellen REST API](https://msdn.microsoft.com/library/azure/mt420159.aspx) och automatisera Hej olika Azure-resurser som också är kopplade till den prenumerationen.
