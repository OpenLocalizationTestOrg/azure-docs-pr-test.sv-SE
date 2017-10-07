---
title: "aaaManage developer konton med hjälp av grupper i Azure API Management | Microsoft Docs"
description: "Lär dig hur toomanage developer användarkonton med grupper i Azure API Management"
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 33660b45-442f-44be-9a4a-1899d7f699b0
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: c46e010e41d9705ae161dcd60d734a76d19c9e93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-use-groups-toomanage-developer-accounts-in-azure-api-management"></a>Hur toocreate och Använd grupper toomanage developer konton i Azure API Management
Grupper finns i API Management används toomanage hello synligheten för produkter toodevelopers. Produkter är första gjorts synliga toogroups och utvecklare i dessa grupper kan sedan visa och prenumerera toohello produkter som är associerade med hello grupper. 

API Management har hello följande ändras system grupper.

* **Administratörer** – Administratörer av Azure-prenumerationer är medlemmar i den här gruppen. Administratörer hantera API Management-tjänstinstanser skapar hello-API: er, åtgärder och produkter som används av utvecklare.
* **Utvecklare** – Autentiserade användare av utvecklarportalen hör till den här gruppen. Utvecklare är hello-kunder som skapar program med hjälp av dina API: er. Utvecklare har beviljats åtkomst toohello developer-portalen och bygga program som anropar hello driften av ett API.
* **Gäster** -oautentiserad developer portal användare, till exempel potentiella kunder som besöker hello developer-portalen för en API-hantering instans återgång till den här gruppen. De kan beviljas vissa skrivskyddad åtkomst till exempel hello möjlighet tooview API: er men anropa inte dem.

Administratörer kan skapa anpassade grupper i tillägg toothese system grupper eller [utnyttja externa grupper i associerade Azure Active Directory-klienter][leverage external groups in associated Azure Active Directory tenants]. Anpassad och externa grupper kan användas tillsammans med system-grupper i vilket ger utvecklare synlighet och komma åt tooAPI produkter. Du kan till exempel skapa en anpassad grupp för utvecklare som är kopplad till en specifik kontopartner-organisation och ge dem åtkomst toohello API: er från en produkt som innehåller relevant API. En användare kan tillhöra mer än en grupp.

Den här guiden visar hur administratörer av en instans för API-hantering kan lägga till nya grupper och koppla dem till produkter och utvecklare.

> [!NOTE]
> Dessutom toocreating och hantera grupper i hello publisher portal, du kan skapa och hantera grupper med hello API Management REST API [grupp](https://msdn.microsoft.com/library/azure/dn776329.aspx) entitet.
> 
> 

## <a name="create-group"></a>Skapar du en grupp
toocreate en ny grupp klickar du på **Publisher portal** i hello Azure-portalen för API Management-tjänsten. Då kommer du toohello API Management publisher portal.

![Utgivarportalen][api-management-management-console]

> Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management] [ Get started with Azure API Management] kursen.
> 
> 

Klicka på **grupper** från hello **API Management** menyn på hello vänster och klicka sedan på **Lägg till grupp**.

![Lägg till ny grupp][api-management-add-group]

Ange ett unikt namn för hello grupp och en valfri beskrivning och klicka på **spara**.

![Lägg till ny grupp][api-management-add-group-window]

hello nya gruppen visas i hello grupper fliken tooedit hello **namn** eller **beskrivning** hello-gruppen, klicka hello namn hello i hello-listan. toodelete hello-gruppen, klicka på **ta bort**.

![Grupp som har lagts till][api-management-new-group]

Nu när hello grupp har skapats kan vara den associerad med produkter och utvecklare.

## <a name="associate-group-product"></a>Associera en grupp med en produkt
tooassociate en grupp med en produkt, klicka på **produkter** från hello **API Management** menyn på hello vänster och klicka sedan på hello namnet på hello önskade produkt.

![Ange synlighet][api-management-add-group-to-product]

Välj hello **synlighet** fliken tooadd och ta bort grupper och tooview hello aktuella grupper för hello produkten. tooadd eller ta bort grupper, markera eller avmarkera kryssrutorna för hello för hello önskad grupper och klicka på **spara**.

![Ange synlighet][api-management-add-group-to-product-visibility]

> [!NOTE]
> tooadd Azure Active Directory-grupper, se [hur tooauthorize developer användarkonton med Azure Active Directory på Azure API Management](api-management-howto-aad.md).
> 
> tooconfigure grupper från hello **synlighet** för en produkt klickar du på **hantera grupper**.
> 
> 

När en produkt är associerad med en grupp kan kan utvecklare i gruppen visa och prenumerera toohello produkten.

## <a name="associate-group-developer"></a>Associera grupper med utvecklare
tooassociate grupper med utvecklare, klickar på **användare** från hello **API Management** menyn på hello vänster och sedan hello kryssrutan bredvid hello utvecklare gärna tooassociate med en grupp.

![Lägg till toogroup för utvecklare][api-management-add-group-to-developer]

När hello önskad utvecklare kontrolleras, klickar du på önskad hello-grupp i hello **lägga till tooGroup** listrutan. Utvecklare kan tas bort från grupper med hjälp av hello **ta bort från gruppen** listrutan. 

![Utvecklare][api-management-add-group-to-developer-saved]

När hello association läggs mellan hello utvecklare och hello grupp, du kan visa i hello **användare** fliken.

## <a name="next-steps"> </a>Nästa steg
* När en utvecklare har lagts till tooa grupp, kan de visa och prenumerera toohello produkter som är associerade med gruppen. Mer information finns i [hur skapa och publicera en produkt i Azure API Management][How create and publish a product in Azure API Management],
* Dessutom toocreating och hantera grupper i hello publisher portal, du kan skapa och hantera grupper med hello API Management REST API [grupp](https://msdn.microsoft.com/library/azure/dn776329.aspx) entitet.

[api-management-management-console]: ./media/api-management-howto-create-groups/api-management-management-console.png
[api-management-add-group]: ./media/api-management-howto-create-groups/api-management-add-group.png
[api-management-add-group-window]: ./media/api-management-howto-create-groups/api-management-add-group-window.png
[api-management-new-group]: ./media/api-management-howto-create-groups/api-management-new-group.png
[api-management-add-group-to-product]: ./media/api-management-howto-create-groups/api-management-add-group-to-product.png
[api-management-add-group-to-product-visibility]: ./media/api-management-howto-create-groups/api-management-add-group-to-product-visibility.png
[api-management-add-group-to-developer]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer.png
[api-management-add-group-to-developer-saved]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer-saved.png

[api-management-]: ./media/api-management-howto-create-groups/api-management-.png

[Create a group]: #create-group
[Associate a group with a product]: #associate-group-product
[Associate groups with developers]: #associate-group-developer
[Next steps]: #next-steps

[How create and publish a product in Azure API Management]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[leverage external groups in associated Azure Active Directory tenants]: api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group
