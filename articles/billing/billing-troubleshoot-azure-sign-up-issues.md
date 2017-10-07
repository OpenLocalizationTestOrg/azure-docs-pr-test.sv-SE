---
title: aaaTroubleshoot Azure registreringsproblem | Microsoft Docs
description: Beskriver hur tootroubleshoot vissa vanliga Azure registrering problem.
services: 
documentationcenter: 
author: JiangChen79
manager: adpick
editor: 
tags: billing,top-support-issue
ms.assetid: a0907da1-cb2d-41d1-a97f-43841fabe355
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: cjiang
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ee649bfc3be7ba1fe2dd863fac09e1c2311d835b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-sign-up-issues-for-azure"></a>Felsökning av problem med registreringen för Azure
Om du inte registrera dig för Azure använda hello tips i den här artikeln tootroubleshoot vanliga problem. Om du har ett problem med ditt kreditkort under registreringen, se [din bankkort eller kreditkort har nekats på Azure anmälan](billing-credit-card-fails-during-azure-sign-up.md). Om du har ett Azure-konto men kan inte logga in, se [jag kan inte logga in toomanage min Azure-prenumeration](billing-cannot-login-subscription.md).

## <a name="progress-bar-hangs-in-identity-verification-by-card-section"></a>Förloppsindikator låser sig i avsnittet ”identitetsverifiering kort”

toocomplete hello identitetsverifiering kort, cookies från tredje part måste tillåtas för webbläsaren.

![Skärmbild av hello identitetsverifiering kort avsnittet hänger sig under registreringen](./media/billing-troubleshoot-azure-sign-up-issues/identity-verification-hangs.PNG)

Använd följande steg tooupdate hello cookie-inställningar för din webbläsare.

1. Om du använder Chrome, gå för**inställningar** > **visa avancerade inställningar** > **sekretess** > **innehåll inställningar för**. Avmarkera **blockerar cookies från tredje part och platsdata**.
2. Om du använder Edge gå för**inställningar** > **visa avancerade inställningar** > **Cookies**. Välj **inte blockera cookies**.
3. Uppdatera hello Azure registreringssidan och kontrollera om hello problemet är löst.
4. Om hello uppdatering inte löser problemet hello, avsluta och starta om webbläsaren och försök sedan igen.

## <a name="credit-card-form-doesnt-support-my-billing-address"></a>Kreditkort formuläret har inte stöd för min faktureringsadress
Din faktureringsadress måste toobe i hello land som du valde i hello **om du** avsnitt. Kontrollera att du väljer rätt land för hello.

## <a name="no-text-messages-or-calls-during-sign-up-account-verification"></a>Inga textmeddelanden eller samtal vid registreringen konto verifieringen
Det är vanligtvis mycket snabbare, kan det ta upp toofour minuter för verifiering kod toobe levereras. hello telefonnummer som du anger för verifiering lagras inte som kontaktnummer för hello-kontot.

Här följer några ytterligare tips:
* Ett VOIP-telefonnummer kan inte användas för hello phone verifieringsprocessen.
* Kontrollera hello telefonnumret som du anger, inklusive hello landskoden som du har valt i listrutan hello.
* Om telefonen inte ta emot textmeddelanden (SMS), försök hello **ringa mig** alternativet.
* Se till att din telefon kan ta emot samtal eller SMS-meddelanden från USA baserat flera.

När du får hello SMS eller telefonsamtal, ange koden som visas i hello textruta.

## <a name="credit-card-declined-or-not-accepted"></a>Kreditkort avvisade eller accepterades inte
Virtuella eller förbetalt kort för kredit- eller inte är godkänd som betalning för Azure-prenumerationer. toosee vad som kan orsaka din kort toobe nekades, se [din bankkort eller kreditkort har nekats på Azure anmälan](billing-credit-card-fails-during-azure-sign-up.md).

## <a name="free-trial-is-not-available"></a>”Kostnadsfri utvärderingsversion är inte tillgänglig”
Har du använt en Azure-prenumeration i hello senaste? hello Azure användarvillkoren begränsar kostnadsfri utvärderingsversion aktivering endast för en användare som är nya tooAzure. Om du har haft någon annan typ av Azure-prenumeration kan du aktivera en kostnadsfri utvärderingsversion. Överväg att registrera dig för en [betala per användning prenumeration](https://azure.microsoft.com/offers/ms-azr-0003p/).

## <a name="i-saw-a-charge-on-my-free-trial-account"></a>Jag såg en avgift på mitt konto kostnadsfri utvärderingsversion
Du kan se en liten verifiering håller på ditt kreditkortskonto när du har loggat in som tas bort inom 3 too5 dagar. Om du är oroliga om att hantera kostnaderna kan du läsa mer om [förhindrar oväntade kostnader](https://docs.microsoft.com/azure/billing/billing-getting-started).

## <a name="cant-activate-azure-benefit-plan-like-msdn-bizspark-bizsparkplus-or-mpn"></a>Det går inte att aktivera Azure förmånsplan MSDN, BizSpark, BizSparkPlus eller MPN
Kontrollera att du använder hello rätt inloggning autentiseringsuppgifter. Kontrollera sedan hello förmånen programmet toomake att du är berättigad. 

* MSDN
  * Kontrollera din behörighet status i din [MSDN kontosida](https://msdn.microsoft.com/subscriptions/manage/default.aspx).
  * Om du inte kan verifiera din status, kontakta hello [kundtjänst för MSDN-prenumerationer](https://msdn.microsoft.com/subscriptions/contactus.aspx)
* BizSpark
  * Logga in toohello [BizSpark portal](https://www.microsoft.com/bizspark/default.aspx#start-two) och verifiera din behörighet status för BizSpark och BizSpark Plus.
  * Om du inte kan kontrollera din status, kan du [få hjälp på hello BizSpark forum](http://aka.ms/bzforums).
* MPN
  * Logga in toohello [MPN portal](https://mspartner.microsoft.com/en/us/Pages/Locale.aspx) och verifiera din behörighet status. Om du har rätt hello [Cloud Platform befogenheter](https://mspartner.microsoft.com/en/us/pages/membership/cloud-platform-competency.aspx), du kan vara berättigad till ytterligare fördelar.
  * Om du inte kan verifiera din status, kontakta [MPN support](https://mspartner.microsoft.com/en/us/Pages/Support/Premium/contact-support.aspx).

## <a name="cant-activate-new-azure-in-open-subscription"></a>Det går inte att aktivera Azure i Open prenumeration
Du måste ha en giltig Online Service aktivering OSA nyckel med minst en Azure i Open token associerade tooit toocreate en prenumeration på Azure i Open. Om du inte har en OSA nyckel, kontakta någon av Microsoft-Partners som anges i [Microsoft Pinpoint](http://pinpoint.microsoft.com/).

## <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten.
Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget problemet lösas snabbt.
