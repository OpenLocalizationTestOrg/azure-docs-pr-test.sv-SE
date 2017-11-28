---
title: aaaUse Emoji uttryckssymboler i Azure Mobile Engagement
description: Hur toouse Emoji uttryckssymboler inom push-meddelanden
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 663317d7-3c93-4e8f-b13d-c6fb342124ee
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 63bfc59ef472e9fe9aa28b5ac8761017b9250e0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-emoji-emoticon-within-push-notifications"></a><span data-ttu-id="53be7-103">Använd Emoji uttryckssymbol inom Push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="53be7-103">Use Emoji emoticon within Push Notifications</span></span>
<span data-ttu-id="53be7-104">Du kan inkludera Emoji uttryckssymboler i du push-meddelanden i några enkla steg:</span><span class="sxs-lookup"><span data-stu-id="53be7-104">You can include Emoji emoticons in you push notification in a few easy steps:</span></span> 

1. <span data-ttu-id="53be7-105">Du måste först toofind hello Emoji som du vill toosend i hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="53be7-105">First of all you need toofind hello Emoji you want toosend in hello message.</span></span> <span data-ttu-id="53be7-106">Se till att hello Emoji som du väljer kommer att stödas av hello målenhet som enheten tillverkar ta viss tid tooadd nyligen godkända Emojis toohello enhetsplattformar.</span><span class="sxs-lookup"><span data-stu-id="53be7-106">Please ensure that hello Emoji you are selecting will be supported by hello target device as device manufactures take some time tooadd newly approved Emojis toohello device platforms.</span></span> 
2. <span data-ttu-id="53be7-107">På **Windows** -du kan navigera toothis [länk](http://apps.timwhitlock.info/emoji/tables/unicode) och kopiera hello ”interna”-ikonen.</span><span class="sxs-lookup"><span data-stu-id="53be7-107">On **Windows** - you can navigate toothis [link](http://apps.timwhitlock.info/emoji/tables/unicode) and copy hello 'Native' icon.</span></span>
   
    ![][7] 
3. <span data-ttu-id="53be7-108">På **Mac** -du kan hitta hello Emojis i ordlistan program under Redigera -> Emoji & symboler.</span><span class="sxs-lookup"><span data-stu-id="53be7-108">On **Mac** - you can find hello Emojis in Dictionary application under Edit -> Emoji & Symbols.</span></span>
   
    ![][6] 
4. <span data-ttu-id="53be7-109">Gå toohello **nå** fliken på hello hello Azure Mobile Engagement-portalen.</span><span class="sxs-lookup"><span data-stu-id="53be7-109">Now, go toohello **Reach** tab on hello hello Azure Mobile Engagement portal.</span></span> <span data-ttu-id="53be7-110">Ange hello push-meddelande (meddelande, avsökningar osv).</span><span class="sxs-lookup"><span data-stu-id="53be7-110">Select hello type of your push notification (Announcement, polls etc).</span></span> <span data-ttu-id="53be7-111">Det här exemplet väljer vi ett meddelande push.</span><span class="sxs-lookup"><span data-stu-id="53be7-111">For this example we choose an announcement push.</span></span>
5. <span data-ttu-id="53be7-112">Ange hello olika fält av hello-meddelande tills du når hello text av hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="53be7-112">Specify hello different fields of hello notification until you reach hello text of hello notification.</span></span> <span data-ttu-id="53be7-113">Det är där du ska lägga till uttryckssymbolen Emoji.</span><span class="sxs-lookup"><span data-stu-id="53be7-113">This is where you will add your Emoji Emoticon.</span></span> <span data-ttu-id="53be7-114">Du kan välja tooput i hello rubrik, hello-meddelande eller båda.</span><span class="sxs-lookup"><span data-stu-id="53be7-114">You can choose tooput it in hello title, hello message, or both.</span></span> <span data-ttu-id="53be7-115">Du behöver toodrag och släpp eller kopiera hello Emoji som du hittar hello platser ovan.</span><span class="sxs-lookup"><span data-stu-id="53be7-115">You will need toodrag and drop or copy hello Emoji that you find from hello locations above.</span></span> 
   
    ![][1]
6. <span data-ttu-id="53be7-116">Fullständig hello andra fält för hello-meddelande och spara den.</span><span class="sxs-lookup"><span data-stu-id="53be7-116">Complete hello other fields for hello notification and save it.</span></span> 
7. <span data-ttu-id="53be7-117">När du kör ett test eller aktivera hello-meddelande visas ett meddelande med hello uttryckssymbol som anges.</span><span class="sxs-lookup"><span data-stu-id="53be7-117">When you run a test or activate hello announcement you will see a notification with hello emoticon as specified.</span></span>   
   
    <span data-ttu-id="53be7-118">![][3] ![][4] ![][5]</span><span class="sxs-lookup"><span data-stu-id="53be7-118">![][3] ![][4] ![][5]</span></span>

<!-- Images. -->
[1]: ./media/mobile-engagement-use-emoji-with-push/notification_input.png
[3]: ./media/mobile-engagement-use-emoji-with-push/iOS_Emoji.png
[4]: ./media/mobile-engagement-use-emoji-with-push/Android_Emoji.png
[5]: ./media/mobile-engagement-use-emoji-with-push/WindowsPhone_Emoji.png
[6]: ./media/mobile-engagement-use-emoji-with-push/Mac_SelectEmoji.png
[7]: ./media/mobile-engagement-use-emoji-with-push/Windows_SelectEmoji.png

