---
title: "Översikt över Azure StorSimple Data Manager aaaMicrosoft | Microsoft Docs"
description: "En översikt över hello StorSimple Data Manager-tjänsten (privat förhandsvisning)"
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/22/2016
ms.author: vidarmsft
ms.openlocfilehash: 5d29f7d26be9f2b36857526bdea770d991cece6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-data-manager-overview-private-preview"></a>Översikt över StorSimple Data Manager (privat förhandsvisning)

## <a name="overview"></a>Översikt

Microsoft Azure StorSimple är en hybrid cloud lagringslösning som adresser hello svårigheter av Ostrukturerade data som vanligtvis är kopplad till filresurser. StorSimple använder lagringsutrymmet i molnet som ett tillägg av hello lokalt lösning och automatiskt nivåer data över hello lokal lagring och lagringsutrymmet i molnet. Inbyggt dataskydd med lokala och molnbaserade ögonblicksbilder, eliminerar hello behovet av att en sprawling lagringsinfrastruktur. Arkiveringen och katastrofåterställning är också sömlös hello moln fungerar som en annan plats.

hello omvandling datatjänst som som presenteras i detta dokument, kan du tooseamlessly åtkomst hello StorSimple data i hello molnet. Den här tjänsten innehåller API: er tooextract data från StorSimple och presentera den tooother Azure services i ett format som lätt kan användas. hello-format som stöds i den här förhandsgranskningen är Azure-blobbar och Azure Media Services tillgångar. Den här omvandlingen kan du tooeasily överföring av tjänster, till exempel Azure Media Services, Azure HDInsight, Azure Machine Learning och Azure Search toooperate på StorSimple 8000-serien lokala enhet.

En övergripande blockdiagram som illustrerar detta visas nedan.

![Översiktsdiagram](./media//storsimple-data-manager-overview/high-level-diagram.png)

Det här dokumentet förklarar hur du registrerar dig för en privat förhandsgranskning av den här tjänsten. Här beskrivs också hur du kan använda den här tjänsten toowrite program som använder StorSimple data och andra Azure-tjänster i molnet hello.

## <a name="sign-up-for-data-manager-preview"></a>Registrera dig för förhandsversionen Data Manager
Granska hello följande krav innan du registrerar dig för hello Data Manager-tjänsten.

### <a name="prerequisites"></a>Krav

Den här övningen förutsätter att du har
* En aktiv Azure-prenumeration.
* åtkomst tooa registrerad StorSimple 8000-serieenhet
* alla hello nycklar som är associerade med hello StorSimple 8000-serieenhet.

### <a name="sign-up"></a>Registrera dig

Hej StorSimple Data Manager är i privat förhandsvisning. Utför hello följande steg toosign för en privat förhandsgranskning av den här tjänsten:

1.  Logga in på hello Azure-portalen med hello StorSimple Data Manager-tillägg på: [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager). Använd dina autentiseringsuppgifter för Azure-toolog i.

2.  Klicka på hello  **+**  ikonen toocreate en tjänst. Klicka på **lagring** och klicka sedan på **finns alla** i hello bladet som öppnas.

    ![Sökikonen StorSimple Data Manager](./media/storsimple-data-manager-overview/search-data-manager-icon.png)

3. Ikonen hello StorSimple Data Manager.

    ![Välj Data StorSimple Manager-ikon](./media/storsimple-data-manager-overview/select-data-manager-icon.png)

4. StorSimple Data Manager-ikonen och klicka sedan på **skapa**. Välj hello prenumeration som du vill tooenable för hello privat förhandsgranskning och klicka sedan på **registrera mig!**

    ![Registrera mig](./media/storsimple-data-manager-overview/sign-me-up.png)

5. Detta skickar en begäran tooonboard du. Vi kommer att publicera du så snart som möjligt. När prenumerationen har aktiverats kan skapa du en StorSimple Data Manager-tjänsten.

6. tooeasily komma åt hello StorSimple Data Manager-tjänsten klickar du på hello stjärnikonen toopin den tooyour Favoriter.

    ![Åtkomst till StorSimple Data chefer](./media/storsimple-data-manager-overview/access-data-managers.png)


## <a name="next-steps"></a>Nästa steg

[Använda StorSimple Data-hanterarens användargränssnitt tootransform dina data](storsimple-data-manager-ui.md).
