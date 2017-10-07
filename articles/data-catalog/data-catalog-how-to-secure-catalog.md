---
title: "aaaHow toosecure åtkomst tooAzure Data Catalog | Microsoft Docs"
description: "Den här artikeln förklarar hur toosecure datakatalogen och dess datatillgångar."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/17/2017
ms.author: maroche
ms.openlocfilehash: d7c35fd57d8add1cdc152b75f111879288e1548f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosecure-access-toodata-catalog-and-data-assets"></a>Hur toosecure åt toodata katalog- och tillgångar
> [!IMPORTANT]
> Den här funktionen är endast tillgänglig i hello standardversionen av Azure Data Catalog.

Azure Data Catalog kan du toospecify som kan komma åt hello data catalog och vad operations (registrera, lägga till anteckningar, bli ägare) som kan utföras för metadata i hello-katalogen. 

## <a name="catalog-users-and-permissions"></a>Katalogens användare och behörigheter
toogive en användare eller en grupp hello öppnar tooa data catalog och ange behörigheter:

1. På hello [startsidan i data catalog](http://www.azuredatacatalog.com), klickar du på **inställningar** hello i verktygsfältet.

    ![data catalog – inställningar](media/data-catalog-how-to-secure-catalog/data-catalog-settings.png)
2. Expandera hello i hello inställningssidan **katalogens användare** avsnitt.
    ![Katalogen användare – Lägg till](media/data-catalog-how-to-secure-catalog/data-catalog-add-button.png)
3. Klicka på **Lägg till**.
4. Ange hello fullständigt kvalificerade **användarnamn** eller namnet på hello **säkerhetsgrupp** i hello Azure Active Directory (AAD) som är associerade med hello katalog. Kommatecken (', ') som avgränsare om du lägger till fler än en användare eller grupp.
    ![Katalogens användare - användare eller grupper](media/data-catalog-how-to-secure-catalog/data-catalog-users-groups.png)
5. Tryck på **RETUR** eller **FLIKEN** utanför hello textruta. 
6.  Bekräfta att alla behörigheter (**anteckna**, **registrera**, och **bli ägare**) tilldelas toothese användare eller grupper som standard. Det vill säga hello användare eller grupp kan [registrera datatillgångar]( data-catalog-how-to-register.md), [kommenterar du datatillgångar]( data-catalog-how-to-annotate.md), och [överta ägarskapet för datatillgångar]( data-catalog-how-to-manage.md). 
    ![Katalogens användare - standardbehörigheter](media/data-catalog-how-to-secure-catalog/data-catalog-default-permissions.png)
7.  toogive en användare eller grupp endast hello läsa åtkomst toohello katalog Rensa hello **kommentera** alternativ för användaren eller gruppen. När du gör det hello användare eller grupp kan inte kommenterar du datatillgångar i hello-katalogen, men de kan visa dem. 
8.  toodeny en användare eller grupp registrerar datatillgångar, avmarkera hello **registrera** alternativ för användaren eller gruppen.
9.  toodeny en användare från att bli ägare till en datatillgång, rensa hello **bli ägare** alternativ för användaren eller gruppen. 
10. toodelete användare/grupp från katalogens användare klickar du på **x** för hello användare/grupp hello botten hello-listan. 
    ![Katalogen användare – ta bort användaren](media/data-catalog-how-to-secure-catalog/data-catalog-delete-user.png)

    > [!IMPORTANT]
    > Vi rekommenderar att du lägger till säkerhet grupper toocatalog användare i stället för att lägga till användare direkt och tilldela behörigheter. Lägg sedan till säkerhetsgrupper för användare toohello som matchar deras roller och deras nödvändiga åtkomsten toohello katalogen.

## <a name="special-considerations"></a>Speciella överväganden

- hello behörigheter toosecurity grupper är additiva. En användare finns, i två grupper. En grupp har kommentera behörigheter och andra grupper inte har kommentera behörigheter. Användaren har sedan kommentera behörigheter. 
- hello behörigheter explicit tooa användaren åsidosättning hello behörigheter toogroups toowhich hello användaren tillhör. I föregående exempel hello säger du uttryckligen har lagt till hello toocatalog användare och tilldela inte kommentera behörigheter. hello-användare kan inte lägga till anteckningar datatillgångar även om hello användaren är medlem i en grupp som har kommentera behörigheter.

## <a name="next-steps"></a>Nästa steg
- [Kom igång med Azure Data Catalog](data-catalog-get-started.md)

