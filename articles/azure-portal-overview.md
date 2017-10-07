---
title: "Översikt över portal aaaAzure | Microsoft Docs"
description: "Lär dig hur toouse hello Microsoft Azure-portalen."
services: 
documentationcenter: 
author: davidwrede
manager: erikre
editor: jimbe
ms.assetid: 53cb9df1-c96a-4f4e-b022-18336cd3d697
ms.service: na
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/16/2015
ms.author: dwrede
ms.openlocfilehash: feca8b5e26e9cb373c5e8e688173d5856050bb28
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-azure-portal-overview"></a>Översikt över Microsoft Azure Portal
hello Microsoft Azure portal är en central plats där du kan etablera och hantera Azure-resurser.  Den här självstudiekursen kommer bekanta dig med portalen hello och visar hur toouse några av dessa nyckeln funktioner:

* En **omfattande marknadsplats** där du kan bläddra igenom tusentals objekt från Microsoft och andra leverantörer som du kan köpa eller etablera.
* En **enhetlig och skalbar navigeringsmiljö** som gör det enkelt toofind hello resurser som du är intresserad av och utföra olika hanteringsåtgärder.
* **Enhetliga hanteringssidor** (eller blad) där du kan hantera Azures många tjänster med hjälp av enhetliga inställningar, åtgärder, faktureringsinformation, hälsoövervakning, användningsdata och mycket mer.
* En **personlig upplevelse** som hjälper dig att skapa en anpassad startskärm som visar hello information som du vill toosee varje gång du loggar in.  Du kan också anpassa hello hanteringsbladen som innehåller paneler.
  
  ![Hitta rätt i användargränssnittet på Azure Portal][UIOrientation]

## <a name="before-you-get-started"></a>Innan du börjar
Du behöver ett giltig Azure-prenumeration toogo igenom den här kursen.  Om du inte har någon kan du [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).  När du har en prenumeration kan du komma åt hello-portalen på <https://portal.azure.com>.

## <a name="how-toocreate-a-resource"></a>Hur toocreate en resurs
Azure har en marknadsplats med tusentals objekt som du kan skapa från en och samma plats.  Anta att du vill toocreate en ny Windows Server 2012 VM.  hello + nytt-navet är ingången till en granskad uppsättning kategorier från hello marketplace.  Varje kategori har en liten uppsättning objekt samt en länk toohello fullständiga marknadsplatsen där du hittar alla kategorier och Sök. toocreate att nya Windows Server 2012 VM utför hello följande åtgärder:  

1. Windows Server 2012 publiceras, så du kan välja den från hello beräkning kategori.  
2. Fyll i grundläggande information i ett formulär.
3. Klicka på ”Skapa” och den virtuella datorn startar tooprovision omedelbart.

Hej meddelanden hubb meddelar dig när resursen har skapats och ett hanteringsblad öppnas (du kan alltid Bläddra tooresources senare).

![Portalkategorier][PortalCategories]

## <a name="how-toofind-your-resources"></a>Hur toofind dina resurser
Du kan fästa resurser som används ofta tooyour startsidan, men du kan behöva toobrowse toosomething som du inte använder ofta.  hello via bläddringsnavet nedan är ditt sätt tooget tooall av dina resurser.  Du kan filtrera efter prenumeration, välja/Ändra storlek på kolumner och gå toohello hanteringsbladen genom att klicka på enskilda objekt.

![Bläddringsnavet][BrowseHub]

## <a name="how-toomanage-and-delegate-access-tooa-resource"></a>Hur toomanage och delegera åtkomst till resursen tooa
Från det här bladet kan du ansluta toohello virtuell dator med hjälp av fjärrskrivbord, övervaka prestandarelaterade nyckeltal, styra åtkomst toothis VM med hjälp av rollbaserad åtkomst (RBAC), konfigurera hello VM och utföra andra viktiga hanteringsåtgärder.  Delegera åtkomst baserat på roller är kritiska toomanaging i större skala.  Klicka på [här](active-directory/role-based-access-control-configure.md) toolearn mer om den. toodelegate åtkomst tooa resursen, utföra hello följande åtgärder:

1. Bläddra tooyour resurs.
2. Klicka på alla inställningar i avsnittet för hello Essentials.
3. Klicka på 'Användare' hello inställningar.
4. Klicka på ”Lägg till' i hello kommandofältet.
5. Välj en användare och en roll.

![Hantera en resurs][ManageResource]

## <a name="how-tooget-help"></a>Hur tooget
Om du får problem finns vi här!  hello-portalen har en hjälp och support-sida där du kan i hello rätt riktning.  Beroende på din [supportavtal](https://azure.microsoft.com/support/plans/), du kan också skapa supportärenden direkt i hello-portalen.  När du har skapat ett supportärende kan du hantera hello livscykeln för hello-biljett från hello-portalen. Du kan få toohello hjälp och supportsidan genom att gå tooBrowse -> hjälp + support.  

![Hjälp och support][HelpSupport]

## <a name="summary"></a>Sammanfattning
Nu ska vi se vad du lärt dig i den här självstudiekursen:

* Du har lärt dig hur toosign, kommer en prenumeration och bläddra toohello portal
* Du har fått objektorienterad med hello portalens användargränssnitt och lärt dig hur toocreate och bläddra resurser
* Du har lärt dig hur toocreate en resurs och bläddrar bland resurser
* Du har lärt dig om hello strukturen eller hanteringsbladen och hur du kan hantera olika typer av resurser konsekvent
* Du har lärt dig hur toocustomize hello portal toobring hello information du är intresserad toohello överst
* Du har lärt dig hur toocontrol åtkomst tooresources via rollen baserade åtkomst (RBAC)
* Du har lärt dig hur tooget hjälp och support

hello Microsoft Azure-portalen enklare gör det betydligt bygga och hantera dina program i hello molnet.  Ta en titt på hello [hanteringsbloggen](https://azure.microsoft.com/blog/topics/management/) tookeep in toodate som vi ständigt [lyssnande toofeedback](https://feedback.azure.com/forums/223579-azure-preview-portal/) och gör ständigt förbättringar.  [Scottgus blogg](http://weblogs.asp.net/scottgu) är ett annat bra ställe toolook för alla Azure-uppdateringar.

[UIOrientation]: ./media/azure-portal-how-to-use/azure_portal_1.png
[PortalCategories]: ./media/azure-portal-how-to-use/azure_portal_2.png
[BrowseHub]: ./media/azure-portal-how-to-use/azure_portal_3.png
[ManageResource]: ./media/azure-portal-how-to-use/azure_portal_4.png
[CustomizeBlades]: ./media/azure-portal-how-to-use/azure_portal_5.png
[HelpSupport]: ./media/azure-portal-how-to-use/azure_portal_6.png
