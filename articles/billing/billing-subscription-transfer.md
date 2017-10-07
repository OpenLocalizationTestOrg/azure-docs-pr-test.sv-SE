---
title: "aaaTransfer Azure-prenumeration ägarskap tooanother konto | Microsoft Docs"
description: "Beskriver hur tootransfer en Azure-prenumeration tooanother användare och några vanliga frågor (FAQ) om hello-processen"
keywords: "överföra prenumerationen för överföring av azure-prenumeration, azure, flytta azure-prenumeration tooanother konto, azure ändra prenumerationen ägare, överföring azure prenumerationskonto tooanother"
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing,top-support-issue
ms.assetid: c8ecdc1e-c9c5-468c-a024-94ae41e64702
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d2d9f5d9ccd34738701493e5f31c2f83a02f5d48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="transfer-ownership-of-an-azure-subscription-tooanother-account"></a>Överför ägarskapet för ett Azure-prenumeration tooanother konto

Du kan överföra din prenumeration tooanother användare för betala per användning, Visual Studio, Action Pack eller BizSpark prenumerationer i hello Account Center. Vi stöder hello överföringen av Azure-externa tjänster för typerna prenumeration. 

Du kanske vill tootransfer ägarskap för en Azure-prenumeration om du:

* Behöver toohand via faktureringsägarskapet för din Azure-prenumeration toosomeone annan.
* Vill toochange hello konto används toosign för Azure. Kanske du använt ditt Microsoft-Account men avsedda toouse ditt arbets- eller skolkonto konto i stället.
* Om du vill toomove din Azure-prenumeration från en katalog tooanother.
* Har Azure och Office 365 i olika klienter och vill tooconsolidate.

toochange din prenumeration tooa olika erbjuder, se [växla Azure-prenumeration tooanother erbjudandet](billing-how-to-switch-azure-offer.md). 

## <a name="transfer-ownership-of-an-azure-subscription"></a>Överför ägarskapet för en Azure-prenumeration
> [!VIDEO https://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/Transfer-an-Azure-subscription/player]
>
>

1. Logga in på <https://account.windowsazure.com/Subscriptions>. Du har toobe hello kontoadministratör tooperform ägarskap-överföring. toofind reda på vem som är hello kontoadministratör för hello prenumerationen finns hello [vanliga frågor och](#faq).

2. Välj hello prenumeration tootransfer.

3. Klicka på hello **överföra prenumerationen** alternativet. Se [vanliga frågor och svar](#no-button) om du inte ser hello-knappen.

   ![Fliken abonnemang för Azure-konto](./media/billing-subscription-transfer/image1.png)
4. Ange hello mottagare.

   ![Överföra prenumerationen dialogrutan](./media/billing-subscription-transfer/image2.PNG)
5. hello mottagare får automatiskt ett e-postmeddelande med en länk för godkännande.

   ![Prenumerationen överföring e-toorecipient](./media/billing-subscription-transfer/image3.png)
6. hello mottagaren klickar hello länken och följer hello instruktioner, inklusive att ange sina betalningsinformation.

   ![Prenumerationen överföring webbsida](./media/billing-subscription-transfer/image4.png)

   ![Webbsida för överföring till andra prenumeration](./media/billing-subscription-transfer/image5.png)
7. Lyckades! hello prenumerationen överförs nu.

## <a name="transfer-subscription-ownership-for-enterprise-agreement-ea-customers"></a>Överför ägarskapet för prenumerationen för kunder med Enterprise-avtal (EA)
hello företagsadministratör kan överföra ägarskap för prenumerationer inom en registrering. tooget igång, se [överföra ägarskap för kontot](https://ea.azure.com/helpdocs/changeAccountOwnerForASubscription) hello EA-portalen.

## <a name="next-steps-after-accepting-ownership-of-a-subscription"></a>Nästa steg efter att acceptera ägarskap för en prenumeration
1. Du är nu hello kontoadministratör. Granska och uppdatera hello tjänstadministratören och Medadministratörer. Hantera administratörer i hello [klassiska Azure-portalen](https://manage.windowsazure.com) genom att gå tooSettings. [Lär dig mer om administratörsroller](billing-add-change-azure-subscription-administrator.md).

2. Du kan också använda rollbaserad åtkomstkontroll (RBAC) för din prenumeration och tjänster. Besök hello [Azure-portalen](https://portal.azure.com). [Mer information om RBAC](../active-directory/role-based-access-control-configure.md)

3. Uppdatera autentiseringsuppgifterna som associeras med den här prenumerationens tjänster, inklusive:
   
   * Hanteringscertifikat som ger hello admin användarrättigheter toosubscription resurser. Mer information finns i [skapa och ladda upp ett certifikat för Azure](../cloud-services/cloud-services-certs-create.md)
   
   * Snabbtangenter för tjänster som lagring. Mer information finns i [om Azure storage-konton](../storage/common/storage-create-storage-account.md)
   
   * Remote Access-autentiseringsuppgifter för tjänster som Azure virtuella datorer. 

4. [Uppdatera fakturaaviseringar för den här prenumerationen](billing-set-up-alerts.md) på hello [Azure Kontocenter](https://account.windowsazure.com/Subscriptions). 

5. Om du arbetar med en partner Överväg att uppdatera hello partner-ID för den här prenumerationen. Du kan uppdatera hello partner-ID i hello [Azure Kontocenter](https://account.windowsazure.com/Subscriptions).

<a id="faq"></a>

## <a name="frequently-asked-questions-faq"></a>Vanliga frågor och svar (FAQ)
* <a name="whoisaa"></a>**Som är hello kontoadministratör för hello prenumerationen?**

  hello kontoadministratör är hello person som registrerade sig för eller köpt hello Azure-prenumeration. De är godkända tooaccess hello [Kontocenter](https://account.windowsazure.com/Home/Index) och utföra olika hanteringsuppgifter som att skapa prenumerationer, avbryta prenumerationer, ändra hello fakturering för en prenumeration eller ändra hello tjänstadministratör. Om du inte vet vem hello kontoadministratör är för en prenumeration, använder du följande steg toofind ut hello.

  1. Logga in toohello [Azure-portalen](https://portal.azure.com).
  2. På navmenyn hello väljer **prenumeration**.
  3. Välj hello prenumeration du vill använda toocheck och tittar du under **inställningar**.
  4. Välj **egenskaper**. hello kontoadministratör för hello prenumerationen visas i hello **kontoadministratören** rutan.  

* **Överförs allt? Inklusive resursgrupper, virtuella datorer, diskar och andra tjänster som körs?**

  Ja, alla resurser som virtuella datorer, diskar och webbplatser överför toohello nya ägaren. Men alla [administratörsroller](billing-add-change-azure-subscription-administrator.md) och [rollbaserad åtkomstkontroll (RBAC)](../active-directory/role-based-access-control-configure.md) principer som du har konfigurerat överför inte i olika kataloger.

* <a id="no-button"></a>**Varför ser jag hello överföra prenumerationen knappen?**

  Om du inte ser hello **överföring prenumeration** knappen sedan vi inte stöder prenumerationsöverföring för ditt erbjudande. [Kontakta supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

* **Resulterar en prenumerationsöverföring i avbrott i tjänsten?**

  Det finns ingen inverkan toohello tjänst. Överföring av prenumeration hello avbryter hello prenumeration under hello aktuella kontot Administratör och skapar en prenumeration under hello mottagarens konto. hello prenumeration är associerad toohello underliggande Azure-tjänster. hello prenumerations-ID förblir hello samma.

* **Hur använder den här processen toochange hello katalogen för prenumerationen?**

  En Azure-prenumeration har skapats i hello directory som hello kontoadministratör tillhör. toochange hello directory användarkonto för överföring hello prenumeration tooa i hello målkatalogen. När användaren är klar hello steg tooaccept överföring är hello prenumerationen automatiskt flyttade toohello målkatalogen.

* **Om jag tar över faktureringsägarskapet för en prenumeration från en annan organisation måste de fortsätter toohave åtkomst toomy resurser?**

  Om hello prenumeration är överförda tooanother-klient, förlora hello användare som associeras med hello tidigare innehavaren åtkomst toohello prenumeration. Även om en användare är inte en tjänstadministratör eller medadministratör längre de kan fortfarande ha åtkomst toohello prenumerationen via andra säkerhetsmekanismer, inklusive:

  * Hanteringscertifikat som ger hello admin användarrättigheter toosubscription resurser. Mer information finns i [skapa och ladda upp ett Hanteringscertifikat för Azure](../cloud-services/cloud-services-certs-create.md).
  * Snabbtangenter för tjänster som lagring. Mer information finns i [om Azure storage-konton](../storage/common/storage-create-storage-account.md).
  * Remote Access-autentiseringsuppgifter för tjänster som Azure virtuella datorer.

 Om mottagaren hello måste toorestrict åtkomst tootheir resurser, bör uppdatera alla hemligheter som associeras med hello-tjänsten. De flesta resurser kan uppdateras med hjälp av hello följande steg:

    1. Gå toohello [Azure-portalen](https://portal.azure.com).
    2. På navmenyn hello väljer **alla resurser**.
    3. Välj hello resurs. 
    4. I hello resursbladet klickar du på **inställningar**. Här kan du visa och uppdatera befintliga hemligheter.

* **Om jag överför hello prenumeration hello mitten av hello fakturering cykel hello mottagande betala för hello hela fakturering cykel.**

  hello avsändaren ansvarar för betalning för användning som rapporterades in toohello punkt att hello överföringen har slutförts. hello mottagaren är ansvarig för användning som rapporterats från hello tidpunkten för överföringen och senare. Det kan finnas vissa användningen som har ägt rum före överföringen men rapporterades efteråt. hello användning ingår i hello mottagaren faktura.

* **Har hello mottagaren åtkomst toousage och faktureringshistorik?**

  hello endast information tillgänglig toohello mottagaren är hello mängden hello senaste faktura eller om hello prenumeration överfördes innan hello första faktura genererades hello saldo. hello resten av hello användnings- och faktureringshistorik överför inte med hello prenumeration.

* **Kan hello erbjudandet ändras under en överföring?**

  hello erbjudandet måste vara hello samma. toochange erbjudandet, se [växla Azure-prenumeration tooanother erbjudandet](billing-how-to-switch-azure-offer.md).

* **Kan jag överföra en prenumeration tooa användarkontot i ett annat land?**

  Nej, överföra en prenumeration tooa användarkontot i ett annat land stöds inte. hello mottagaren användarkontot måste vara i hello samma land.

* **Hello mottagaren kan använda en annan betalningsmetod?**

  Ja. Men hello prenumeration faktureringshistorik delas upp mellan två konton.  

* **Påverkas hello betalningsmetod när jag överförde en Azure-prenumeration?**

  tooaccept en prenumerationsöverföring, ett kreditkort eller liknande betalningsmetod måste tillhandahållas toopay för hello prenumeration. Om Daniel överför en prenumeration tooJane och Jane accepterar hello överföringen, måste Jane till exempel ange en betalning metoden toopay för hello prenumeration. När hello överföringen är klar, debiteras Jane för hello prenumerationen inte Bob.

* **Hur jag migrera data och tjänster för min Azure-prenumeration toonew prenumeration?**

  Om du inte kan överföra ägarskapet för prenumerationen kan du manuellt migrera dina resurser. Se [flytta resurser toonew resursgrupp eller prenumeration](../azure-resource-manager/resource-group-move-resources.md).



## <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten.
Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget problemet lösas snabbt. 


