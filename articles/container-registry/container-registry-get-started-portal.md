---
title: aaaCreate privata Docker register - Azure-portalen | Microsoft Docs
description: "Börja skapa och hantera privata Docker behållare register med hello Azure-portalen"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: 53a3b3cb-ab4b-4560-bc00-366e2759f1a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 40f3ce44fea26e5fbeca865c9da6df55c2df9511
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-private-docker-container-registry-using-hello-azure-portal"></a>Skapa en privat Docker behållare registret med hjälp av hello Azure-portalen
Använd hello Azure portal toocreate ett register för behållaren och hantera dess inställningar. Du kan också skapa och hantera behållare register med hello [Azure CLI 2.0 kommandon](container-registry-get-started-azure-cli.md), [Azure PowerShell](container-registry-get-started-powershell.md) eller programmässigt med hello behållaren registret [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).

Bakgrund och begrepp finns [hello översikt](container-registry-intro.md).

## <a name="create-a-container-registry"></a>Skapa ett behållarregister
1. I hello [Azure-portalen](https://portal.azure.com), klickar du på **+ ny**.
2. Sök hello marketplace för **Azure-behållaren registret**.
3. Välj **Azure Container Registry**, med **Microsoft** som utgivare.
    ![Container Registry-tjänsten på Azure Marketplace](./media/container-registry-get-started-portal/container-registry-marketplace.png)
4. Klicka på **Skapa**. Hej **Azure Container registret** bladet visas.

    ![Inställningar för behållarregister](./media/container-registry-get-started-portal/container-registry-settings.png)
5. I hello **Azure Container registret** bladet ange hello följande information. Klicka på **Skapa** när du är klar.

    a. **Registernamn**: Ett globalt unikt domännamn på den översta nivån för ditt specifika register. I det här exemplet är hello registret namnet *myRegistry01*, men i stället använda ett unikt namn för din egen. hello namn får bara innehålla bokstäver och siffror.

    b. **Resursgruppen**: Välj en befintlig [resursgruppen](../azure-resource-manager/resource-group-overview.md#resource-groups) eller hello-typnamn för en ny.

    c. **Plats**: Välj en plats för Azure-datacenter där hello-tjänsten är [tillgängliga](https://azure.microsoft.com/regions/services/), som **södra centrala USA**.

    d. **Administratören**: Om du vill aktivera en admin tooaccess hello registerinställningar. Du kan ändra den här inställningen när du har skapat hello registret.

      > [!IMPORTANT]
      > Dessutom tooproviding komma åt via ett administratörskonto för användaren, behållare register stöder autentisering backas upp av Azure Active Directory-tjänstens huvudnamn. Mer information och saker att tänka på finns i [Authenticate with a container registry](container-registry-authentication.md) (Autentisera med ett behållarregister).
      >

    e. **Lagringskontot**: Använd hello standard inställningen toocreate en [lagringskonto](../storage/common/storage-introduction.md), eller välj ett befintligt lagringskonto i hello samma plats. Premium-lagring stöds inte för närvarande.

## <a name="manage-registry-settings"></a>Hantera registerinställningar
När du har skapat hello registret, hitta hello registerinställningar genom att starta vid hello **behållare register** bladet i hello portal. Till exempel kanske du måste hello inställningar toolog i tooyour registret eller du kanske vill tooenable eller inaktivera hello administratörsanvändare.

1. På hello **behållare register** bladet, klickar du på hello namn i registret.

    ![Bladet för behållarregister](./media/container-registry-get-started-portal/container-registry-blade.png)
2. inställningar för åtkomst av toomanage, klickar du på **åtkomstnyckeln**.

    ![Åtkomst till behållarregister](./media/container-registry-get-started-portal/container-registry-access.png)
3. Observera hello följande inställningar:

   * **Inloggningsserver** -hello fullständigt kvalificerade namn som du använder toolog i toohello registret. I det här exemplet är det `myregistry01.azurecr.io`.
   * **Administratörsanvändare** – växla tooenable eller inaktivera hello registret admin-användarkonto.
   * **Användarnamnet** och **lösenord** -hello autentiseringsuppgifter för användarkontot för Hej administratör (om aktiverat) du kan använda toolog i toohello registret. Alternativt kan du återskapa hello lösenord. Två lösenord skapas så att du kan upprätthålla anslutningar toohello registret med hjälp av ett lösenord när du återskapar hello andra lösenord. tooauthenticate med ett huvudnamn för tjänsten finns i stället [autentisera med ett privat Docker behållare register](container-registry-authentication.md).

## <a name="next-steps"></a>Nästa steg
* [Push-en avbildning med hjälp av hello Docker CLI](container-registry-get-started-docker-cli.md)
