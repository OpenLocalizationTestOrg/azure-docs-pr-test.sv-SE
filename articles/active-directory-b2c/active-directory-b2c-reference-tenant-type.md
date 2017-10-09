---
title: "Azure Active Directory B2C: Region tillgänglighet & data land | Microsoft Docs"
description: "Ett ämne på hello typer av Azure Active Directory B2C-klienter"
services: active-directory-b2c
documentationcenter: 
author: gsacavdm
manager: krassk
editor: bryanla
ms.assetid: 8a0644da-b825-4edc-8ce9-541c3c976afb
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2017
ms.author: gsacavdm
ms.openlocfilehash: d382921fe9d62183b6d52c47d62f952dabd116c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-region-availability--data-residency"></a>Azure Active Directory B2C: Region tillgänglighet & data land
Regional tillgänglighet och data land är två mycket olika begrepp som tillämpas på olika sätt tooAzure AD B2C från hello resten av Azure. Den här artikeln förklarar hello skillnaderna mellan dessa två koncept och jämföra hur de används tooAzure jämfört med Azure AD B2C.

## <a name="summary"></a>Sammanfattning
Azure AD B2C är **allmänt tillgänglig över hela världen** med hello alternativet för **data land i USA eller Europa**.

## <a name="concepts"></a>Koncept
* **Regional tillgänglighet** refererar toowhere en tjänst är tillgänglig för användning.
* **Data land** refererar toowhere användare data lagras.

## <a name="region-availability"></a>Regional tillgänglighet
Azure AD B2C finns över hela världen via hello offentliga Azure-molnet. 

Detta skiljer sig från hello modell följer de flesta andra Azure-tjänster som ihop tillgänglighet med data land. Du kan se ett exempel på detta i både Azure [produkter tillgängliga efter Region](https://azure.microsoft.com/regions/services/) sida och hello [Active Directory B2C prisnivå Kalkylatorn](https://azure.microsoft.com/pricing/details/active-directory-b2c/).

## <a name="data-residency"></a>Dataplacering
Azure AD B2C lagrar användardata i USA och Europa.

Data land fastställs utifrån vilken land/region har valts när [att skapa en Azure AD B2C-klient](active-directory-b2c-get-started.md).

![Skärmbild av en klient för förhandsgranskning](./media/active-directory-b2c-reference-tenant-type/data-residency-b2c-tenant.png)

Data finns i hello USA för hello följande länder:

> USA, Kanada, Costa Rica, Dominikanska republiken, Salvadoranska, Guatemala, Mexiko, Panama, registreringen och Trinidad och Tobago

Data som finns i Europa för hello följande länder:

> Algeriet, Österrike, Azerbajdzjan, Bahrain, Vitryssland, Belgien, Bulgarien, Kroatien, Cypern, Tjeckien, Danmark, Egypten, Estland, Finland, Frankrike, Tyskland, Grekland, Ungern, Islands, Irland, Israel, Italien, Jordanien, Kazakstan, Kenya, Kuwait, Lettland, Libanon Liechtenstein Lituania Luxemburg Makedonien FYROM, Malta, Montenegro, Marocko, Nederländerna, Nigeria, Norge, Oman, Pakistan, Polen, Portugal, Qatar, Rumänien, Ryssland, Saudiarabien, Serbien, Slovakien, Slovenien, Sydafrika, Spanien, Sverige, Schweiz. Tunisien, Turkiet, Ukraina, Förenade Arabemiraten och Storbritannien.

hello återstående länder finns i hello processen att läggs toohello lista.  Du kan fortfarande använda Azure AD B2C genom att välja någon av hello länder ovan för tillfället.

> Afghanistan, Argentina, Australien, Brasilien, underordnad, Colombia, Ecuador, Hongkong SAR, Indien, Indonesien, Irak, Japan, Korea, Malaysia, Nya Zeeland, Paraguay, Peru, Filippinerna, Singapore, Sri Lanka, Taiwan, Thailand, Uruguay och Venezuela.

## <a name="preview-tenant"></a>Förhandsgranska klient
Om du har skapat en B2C-klient under Azure AD B2C-förhandsversionen, är förmodligen som din **klient typen** står **Preview klient**. Om så är fallet hello måste du använda din klient endast för utveckling och testning och inte för appar i produktion.

> [!IMPORTANT]
> Det finns ingen migreringssökväg från en förhandsgranskning B2C-klient tooa produktionsskala B2C-klient. Observera att det finns kända problem när du tar bort en förhandsversion B2C-klient och skapa en produktionsskala B2C-klienten med hello samma domännamn. Du har toocreate en produktionsskala B2C-klient med ett annat domännamn.


![Skärmbild av en klient för förhandsgranskning](./media/active-directory-b2c-reference-tenant-type/preview-b2c-tenant.png)
