---
title: "aaaUnity Återställ kulan självstudiekursen"
description: "Steg toocreate hello klassiska kursen Unity Roll en spel som är ett krav för alla kurser i Mobile Engagement Unity"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 0afd0eca-f74a-43aa-bba8-436a0265c312
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 10d923682432961207594886b08e5db60cf60d9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a id="unity-roll-a-ball"></a>Skapa kursen Unity Roll en spel
Den här kursen går igenom hello viktigaste stegen för en lite annorlunda [kursen Unity Roll kulan självstudiekursen](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial). Det här exemplet spelet består av ett sfäriskt player-objekt som kontrolleras av hello app användaren och hello hello spelet syftar too'collect' domänbeständiga objekt av kollision hello player-objektet med dessa domänbeständiga objekt. Detta förutsätter grundläggande kunskaper med Unity Redigeraren för miljön. Om du stöter på problem ska du gå toohello fullständig genomgång. 

### <a name="setting-up-hello-game"></a>Konfigurera hello spel
hello stegen nedan är från hello [kursen Unity](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)

1. Öppna **Unity Editor** och på **ny**. 
   
    ![][51] 
2. Ange en **projektnamn** & **plats**väljer **3D** och på **skapa projekt**.
   
    ![][52]
3. Spara hello standard scen just har skapat som en del av hello nytt projekt som hello namnet **befälet** i en ny  **\_i bakgrunden** mapp under **tillgångar** mapp:
   
    ![][53]
4. Skapa en **3D-objekt -> plan** som hello spela upp och Byt namn på objektet plan som **grunden**
   
    ![][1]
5. Återställ hello transform-komponenten för detta **grunden** objekt så att den har hello ursprung. 
   
    ![][3]
6. Avmarkera **Visa rutnät** från **Gizmos menyn** för hello **grunden** objekt.
   
    ![][4]
7. Uppdatera hello **skala** komponenten för hello **grunden** objekt toobe [X = 2, Y = 1, Z = 2]. 
   
    ![][5]
8. Lägg till en ny **3D-objekt -> sfär** toohello projekt och Byt namn på den här sfär objekt som **Player**. 
   
    ![][6]
9. Välj hello **Player** och klicka på **Återställ omvandling** liknande toohello plan objekt. 
10. Uppdatera **Transform -> ställning -> Y-koordinaten** komponenten för hello Player Y 0,5.  
    
    ![][7]
11. Skapa en ny mapp som kallas **material** i hello-projekt där vi skapar hello väsentlig toocolor hello player. 
12. Skapa en ny **Material** kallas **bakgrund** i den här mappen. 
    
    ![][8]
13. Uppdatera hello färgen för hello material genom att uppdatera hello **Albedo** -egenskapen för den. Du kan välja hello RGB-värden i [0,32,64]. 
    
    ![][9]
14. Dra detta material till hello scen visa tooapply färg toohello **grunden** objekt. 
    
    ![][10]
15. Slutligen uppdaterar hello **Transform -> Rotation Y ->** too60 på hello riktad enstaka objekt för tydlighetens skull. 
    
    ![][12]

### <a name="moving-hello-player"></a>Flytta hello player
hello stegen nedan är från hello [kursen Unity](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)

1. Lägg till en **RigidBody** komponenten toohello **Player** objekt. 
   
    ![][13]
2. Skapa en ny mapp som kallas **skript** i hello projektet. 
3. Klicka på **Lägg till komponent -> Nytt skript -> C# skript för**. Ge den namnet **PlayerController**, och klicka på **skapa och Lägg till**. Detta skapar och koppla en skriptet toohello Player-objekt.  
   
    ![][14]
4. Flytta det här skriptet under hello **skript** mapp i hello-projekt. 
5. Öppna hello skript för redigering i din favorit Skriptredigeraren, uppdatera hello skriptkod med följande kod hello och spara den. 
   
        using UnityEngine;
        using System.Collections;
   
        public class PlayerController : MonoBehaviour 
        {
            public float speed;
            private Rigidbody rb;
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
                rb.AddForce (movement * speed);
            }
        }
6. Observera hello skriptet ovan använder en **hastighet** egenskapen. Uppdatera hello hastighet egenskapen too10 i hello Unity-redigeraren.  
   
    ![][15]
7. Träffa **spela upp** i hello Unity-redigeraren. Nu bör du kunna toocontrol hello kulan hello tangentbordet och den ska rotera och flytta runt. 

### <a name="moving-hello-camera"></a>Flytta hello kamera
hello stegen nedan är från hello [kursen Unity](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) och Binder hello **Main kamera** toohello **Player** objekt. 

1. Uppdatera hello **Transform.Position** toobe X = 0, Y = 10.5 Z-= 10.  
2. Uppdatera hello **Transform.Rotation** toobe X = 45, Y = 0, Z = 0.  
   
    ![][16]
3. Lägg till ett nytt skript som heter **CameraController** toohello **MainCamera** och placera den under mappen för hello-skript. 
   
    ![][17]
4. Öppna hello skript för redigering och Lägg till följande kod i den hello:
   
        using UnityEngine;
        using System.Collections;
   
        public class CameraController : MonoBehaviour {
   
            public GameObject player;
   
            private Vector3 offset;
   
            void Start ()
            {
                offset = transform.position - player.transform.position;
            }
   
            void LateUpdate ()
            {
                transform.position = player.transform.position + offset;
            }
        }
5. Miljö för Unity - dra hello Player variabeln till hello Player plats för hello Main kamera objekt så att hello två är kopplade till varandra. 
   
    ![][18]
6. Nu om du trycka på Play i hello Unity-redigeraren och rotera hello Player kulan objekt visas sedan hello kamera efter den i hello förflyttning.  

### <a name="setting-up-hello-play-area"></a>Inställningar för hello Play området
hello stegen nedan är från hello [kursen Unity](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141). Vi skapar hello väggar omgivande hello grunden så att hello Player kulan objektet inte släppa ut hello play område i dess rörlighet. 

1. Klicka på **skapa -> Skapa tom spelet objekt ->** och ger den namnet **väggar**
   
    ![][19]
2. Under det här objektet väggar - skapa en ny **3D-objekt -> kuben** och ge den namnet ”Väst brandvägg”. 
   
    ![][20]
3. Uppdatera hello **Transform -> ställning** och **Transform -> skala** för det här objektet i West-brandvägg. 
   
    ![][21]
4. Duplicera hello Väst brandvägg toocreate en **Öst brandvägg** med hello uppdateras transformera placering och skala. 
   
    ![][22]
5. Duplicera hello Öst brandvägg toocreate en **Nord brandvägg** transformera position & skala med hello uppdateras. 
   
    ![][23]
6. Duplicera hello Nord brandvägg och skapa en **söder brandvägg** transformera position & skala med hello uppdateras. 
   
    ![][24]

### <a name="creating-collectible-objects"></a>Skapa Domänbeständiga objekt
hello stegen nedan är från hello [kursen Unity](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141). Vi kommer att skapa några bra söker objekt som utgör hello uppsättning domänbeständiga objekt vilket hello Player kulan objekt måste too'collect' av kollision med dem. 

1. Skapa en ny **3D-kub objekt** och ge den namnet hämtning. 
2. Justera hello **Transform -> Rotation** & **Transform -> skala** i Pickup hello-objektet. 
   
    ![][25]
3. Skapa och koppla en **nytt C# skript** kallas **Rotator** toohello Pickup objekt. Se till att tooput hello skript hello skript i mappen. 
   
    ![][26]
4. Öppna det här skriptet för att redigera och uppdatera den toobe hello följande: 
   
        using UnityEngine;
        using System.Collections;
   
        public class Rotator : MonoBehaviour {
   
            void Update () 
            {
                transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
            }
        }
5. Nu att träffar hello Play läge för hello Unity-redigeraren och visa din Pickup objekt rotera på axeln.
6. Skapa en ny mapp som kallas **Prefabs** 
   
    ![][27]
7. Dra hello **hämtning** objektet och placera den i hello Prefabs mapp.
   
    ![][28]
8. Skapa en ny **tom spelet objektet** kallas **Upphämtningar**. Återställa sin position tooorigin och dra sedan hello Pickup objekt under detta spel objekt.  
   
    ![][29]
9. Dubbla hello **hämtning** objekt och sprids på hello **grunden** objektet runt hello **Player** objekt genom att uppdatera hello **Transform.Position's X & Z**  värden på lämpligt sätt. 
   
    ![][30]
10. Skapa en **nytt material** kallas **hämtning** och uppdatera toobe röd färg genom att uppdatera hello **Albedo egenskapen** liknande toowhat vi för att uppdatera hello grunden objekt. 
    
    ![][31]
11. Tillämpa hello väsentlig tooall hello 4 pickup objekt.
    
    ![][32]

### <a name="collecting-hello-pickup-objects"></a>Samla in hello Pickup objekt
hello stegen nedan är från hello [kursen Unity](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141). Vi kommer att uppdatera hello Player så att den kan too'collect' hello pickup objekt av kollision med dem. 

1. Öppna hello **PlayerController** skript bifogade toohello Player-objektet för redigering och uppdatera den toohello följande:  
   
        using UnityEngine;
        using System.Collections;
   
        public class PlayerController : MonoBehaviour {
   
            public float speed;
   
            private Rigidbody rb;
   
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
   
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
   
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
   
                rb.AddForce (movement * speed);
            }
   
            void OnTriggerEnter(Collider other) 
            {
                if (other.gameObject.CompareTag ("Pick Up"))
                {
                    other.gameObject.SetActive (false);
                }
            }
        }
2. Skapa en ny **taggen** kallas **Välj in** (den måste matcha i hello skript)  
   
    ![][33]
   
    ![][34]
3. Använd denna **taggen** toohello Prefab hämtning objekt. 
   
    ![][35]
4. Aktivera **IsTrigger** kryssrutan för hello Prefab objekt.
   
    ![][36]
5. Lägg till en fast tooPickup Prefab innehållsobjekt. Vi kommer att uppdatera hello statiska collider vi använde tooa dynamiska collider optimera prestanda. 
   
    ![][37]
6. Slutligen Kontrollera hello **IsKinematic** egenskapen för prefab hello-objekt. 
   
    ![][38]
7. Träffa **spela upp** i hello Unity-redigeraren och du kommer att kunna tooplay detta **Återställ en boll** spel genom att flytta hello Player-objektet med tangenterna för riktning indata. 

### <a name="updating-hello-game-for-mobile-play"></a>Uppdatera hello spelet för mobila play
hello avsnitt ovan ingångna hello grundläggande självstudierna från Unity. Nu ska vi ändra hello spel toomake den mobila enheten eget. Observera att vi tangentbordsinmatning för hello spelet hittills för testning. Nu ska vi ändra den så att vi kan kontrollera phone hello player genom att använda hello rörelse hello använda d.v.s. accelerationsmätare som hello indata. 

Öppna hello **PlayerController** skript för redigering och uppdatera hello **FixedUpdate** metoden toouse hello rörelse från hello accelerationsmätare toomove hello Player-objektet. 

        void FixedUpdate()
        {
            //float moveHorizontal = Input.GetAxis("Horizontal");
            //float moveVertical = Input.GetAxis("Vertical");
            //Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
            rb.AddForce(Input.acceleration.x * Speed, 0, -Input.acceleration.z * Speed);
        }

Den här självstudiekursen avslutar en grundläggande spel skapas med Unity och du kan distribuera den på en enhet för ditt val tooplay hello spel. 

<!-- Images -->
[1]: ./media/mobile-engagement-unity-roll-a-ball/1.png    
[2]: ./media/mobile-engagement-unity-roll-a-ball/2.png
[3]: ./media/mobile-engagement-unity-roll-a-ball/3.png
[4]: ./media/mobile-engagement-unity-roll-a-ball/4.png
[5]: ./media/mobile-engagement-unity-roll-a-ball/5.png
[6]: ./media/mobile-engagement-unity-roll-a-ball/6.png
[7]: ./media/mobile-engagement-unity-roll-a-ball/7.png
[8]: ./media/mobile-engagement-unity-roll-a-ball/8.png
[9]: ./media/mobile-engagement-unity-roll-a-ball/9.png    
[10]: ./media/mobile-engagement-unity-roll-a-ball/10.png    
[11]: ./media/mobile-engagement-unity-roll-a-ball/11.png    
[12]: ./media/mobile-engagement-unity-roll-a-ball/12.png
[13]: ./media/mobile-engagement-unity-roll-a-ball/13.png
[14]: ./media/mobile-engagement-unity-roll-a-ball/14.png
[15]: ./media/mobile-engagement-unity-roll-a-ball/15.png
[16]: ./media/mobile-engagement-unity-roll-a-ball/16.png
[17]: ./media/mobile-engagement-unity-roll-a-ball/17.png
[18]: ./media/mobile-engagement-unity-roll-a-ball/18.png
[19]: ./media/mobile-engagement-unity-roll-a-ball/19.png    
[20]: ./media/mobile-engagement-unity-roll-a-ball/20.png    
[21]: ./media/mobile-engagement-unity-roll-a-ball/21.png    
[22]: ./media/mobile-engagement-unity-roll-a-ball/22.png    
[23]: ./media/mobile-engagement-unity-roll-a-ball/23.png    
[24]: ./media/mobile-engagement-unity-roll-a-ball/24.png    
[25]: ./media/mobile-engagement-unity-roll-a-ball/25.png    
[26]: ./media/mobile-engagement-unity-roll-a-ball/26.png    
[27]: ./media/mobile-engagement-unity-roll-a-ball/27.png    
[28]: ./media/mobile-engagement-unity-roll-a-ball/28.png    
[29]: ./media/mobile-engagement-unity-roll-a-ball/29.png    
[30]: ./media/mobile-engagement-unity-roll-a-ball/30.png    
[31]: ./media/mobile-engagement-unity-roll-a-ball/31.png    
[32]: ./media/mobile-engagement-unity-roll-a-ball/32.png    
[33]: ./media/mobile-engagement-unity-roll-a-ball/33.png    
[34]: ./media/mobile-engagement-unity-roll-a-ball/34.png    
[35]: ./media/mobile-engagement-unity-roll-a-ball/35.png    
[36]: ./media/mobile-engagement-unity-roll-a-ball/36.png    
[37]: ./media/mobile-engagement-unity-roll-a-ball/37.png    
[38]: ./media/mobile-engagement-unity-roll-a-ball/38.png    
[51]: ./media/mobile-engagement-unity-roll-a-ball/new-project.png
[52]: ./media/mobile-engagement-unity-roll-a-ball/new-project-properties.png
[53]: ./media/mobile-engagement-unity-roll-a-ball/save-scene.png













