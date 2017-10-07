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
# <span data-ttu-id="68470-103"><a id="unity-roll-a-ball"></a>Skapa kursen Unity Roll en spel</span><span class="sxs-lookup"><span data-stu-id="68470-103"><a id="unity-roll-a-ball"></a>Create Unity Roll a Ball game</span></span>
<span data-ttu-id="68470-104">Den här kursen går igenom hello viktigaste stegen för en lite annorlunda [kursen Unity Roll kulan självstudiekursen](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial).</span><span class="sxs-lookup"><span data-stu-id="68470-104">This tutorial walks through hello main steps for a slightly modified [Unity Roll a Ball tutorial](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial).</span></span> <span data-ttu-id="68470-105">Det här exemplet spelet består av ett sfäriskt player-objekt som kontrolleras av hello app användaren och hello hello spelet syftar too'collect' domänbeständiga objekt av kollision hello player-objektet med dessa domänbeständiga objekt.</span><span class="sxs-lookup"><span data-stu-id="68470-105">This sample game consists of a spherical 'player' object which is controlled by hello app user and hello objective of hello game is too'collect' collectible objects by colliding hello player object with these collectible objects.</span></span> <span data-ttu-id="68470-106">Detta förutsätter grundläggande kunskaper med Unity Redigeraren för miljön.</span><span class="sxs-lookup"><span data-stu-id="68470-106">This assumes basic familiarity with Unity editor environment.</span></span> <span data-ttu-id="68470-107">Om du stöter på problem ska du gå toohello fullständig genomgång.</span><span class="sxs-lookup"><span data-stu-id="68470-107">If you run into any issues then you should refer toohello full tutorial.</span></span> 

### <a name="setting-up-hello-game"></a><span data-ttu-id="68470-108">Konfigurera hello spel</span><span class="sxs-lookup"><span data-stu-id="68470-108">Setting up hello game</span></span>
<span data-ttu-id="68470-109">hello stegen nedan är från hello [kursen Unity](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)</span><span class="sxs-lookup"><span data-stu-id="68470-109">hello steps below are from hello [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)</span></span>

1. <span data-ttu-id="68470-110">Öppna **Unity Editor** och på **ny**.</span><span class="sxs-lookup"><span data-stu-id="68470-110">Open **Unity Editor** and click **New**.</span></span> 
   
    ![][51] 
2. <span data-ttu-id="68470-111">Ange en **projektnamn** & **plats**väljer **3D** och på **skapa projekt**.</span><span class="sxs-lookup"><span data-stu-id="68470-111">Provide a **Project name** & **Location**, select **3D** and click **Create project**.</span></span>
   
    ![][52]
3. <span data-ttu-id="68470-112">Spara hello standard scen just har skapat som en del av hello nytt projekt som hello namnet **befälet** i en ny  **\_i bakgrunden** mapp under **tillgångar** mapp:</span><span class="sxs-lookup"><span data-stu-id="68470-112">Save hello default scene just created as part of hello new project as with hello name **MiniGame** within a new **\_Scenes** folder under **Assets** folder:</span></span>
   
    ![][53]
4. <span data-ttu-id="68470-113">Skapa en **3D-objekt -> plan** som hello spela upp och Byt namn på objektet plan som **grunden**</span><span class="sxs-lookup"><span data-stu-id="68470-113">Create a **3D Object -> Plane** as hello playing field and rename this plane object as **Ground**</span></span>
   
    ![][1]
5. <span data-ttu-id="68470-114">Återställ hello transform-komponenten för detta **grunden** objekt så att den har hello ursprung.</span><span class="sxs-lookup"><span data-stu-id="68470-114">Reset hello transform component for this **Ground** object so that it is at hello Origin.</span></span> 
   
    ![][3]
6. <span data-ttu-id="68470-115">Avmarkera **Visa rutnät** från **Gizmos menyn** för hello **grunden** objekt.</span><span class="sxs-lookup"><span data-stu-id="68470-115">Uncheck **Show Grid** from **Gizmos menu** for hello **Ground** object.</span></span>
   
    ![][4]
7. <span data-ttu-id="68470-116">Uppdatera hello **skala** komponenten för hello **grunden** objekt toobe [X = 2, Y = 1, Z = 2].</span><span class="sxs-lookup"><span data-stu-id="68470-116">Update hello **Scale** component for hello **Ground** object toobe [X = 2,Y = 1, Z = 2].</span></span> 
   
    ![][5]
8. <span data-ttu-id="68470-117">Lägg till en ny **3D-objekt -> sfär** toohello projekt och Byt namn på den här sfär objekt som **Player**.</span><span class="sxs-lookup"><span data-stu-id="68470-117">Add a new **3D Object -> Sphere** toohello project and rename this sphere object as **Player**.</span></span> 
   
    ![][6]
9. <span data-ttu-id="68470-118">Välj hello **Player** och klicka på **Återställ omvandling** liknande toohello plan objekt.</span><span class="sxs-lookup"><span data-stu-id="68470-118">Select hello **Player** object and click **Reset Transform** similar toohello Plane object.</span></span> 
10. <span data-ttu-id="68470-119">Uppdatera **Transform -> ställning -> Y-koordinaten** komponenten för hello Player Y 0,5.</span><span class="sxs-lookup"><span data-stu-id="68470-119">Update **Transform -> Position -> Y Coordinate** component for hello Player Y as 0.5.</span></span>  
    
    ![][7]
11. <span data-ttu-id="68470-120">Skapa en ny mapp som kallas **material** i hello-projekt där vi skapar hello väsentlig toocolor hello player.</span><span class="sxs-lookup"><span data-stu-id="68470-120">Create a new folder called **Materials** in hello project where we will create hello material toocolor hello player.</span></span> 
12. <span data-ttu-id="68470-121">Skapa en ny **Material** kallas **bakgrund** i den här mappen.</span><span class="sxs-lookup"><span data-stu-id="68470-121">Create a new **Material** called **Background** in this folder.</span></span> 
    
    ![][8]
13. <span data-ttu-id="68470-122">Uppdatera hello färgen för hello material genom att uppdatera hello **Albedo** -egenskapen för den.</span><span class="sxs-lookup"><span data-stu-id="68470-122">Update hello color of hello material by updating hello **Albedo** property of it.</span></span> <span data-ttu-id="68470-123">Du kan välja hello RGB-värden i [0,32,64].</span><span class="sxs-lookup"><span data-stu-id="68470-123">You can select hello RGB values of [0,32,64].</span></span> 
    
    ![][9]
14. <span data-ttu-id="68470-124">Dra detta material till hello scen visa tooapply färg toohello **grunden** objekt.</span><span class="sxs-lookup"><span data-stu-id="68470-124">Drag this material into hello scene view tooapply color toohello **Ground** object.</span></span> 
    
    ![][10]
15. <span data-ttu-id="68470-125">Slutligen uppdaterar hello **Transform -> Rotation Y ->** too60 på hello riktad enstaka objekt för tydlighetens skull.</span><span class="sxs-lookup"><span data-stu-id="68470-125">Finally update hello **Transform -> Rotation -> Y** too60 on hello Directional Light object for clarity.</span></span> 
    
    ![][12]

### <a name="moving-hello-player"></a><span data-ttu-id="68470-126">Flytta hello player</span><span class="sxs-lookup"><span data-stu-id="68470-126">Moving hello player</span></span>
<span data-ttu-id="68470-127">hello stegen nedan är från hello [kursen Unity](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)</span><span class="sxs-lookup"><span data-stu-id="68470-127">hello steps below are from hello [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)</span></span>

1. <span data-ttu-id="68470-128">Lägg till en **RigidBody** komponenten toohello **Player** objekt.</span><span class="sxs-lookup"><span data-stu-id="68470-128">Add a **RigidBody** component toohello **Player** object.</span></span> 
   
    ![][13]
2. <span data-ttu-id="68470-129">Skapa en ny mapp som kallas **skript** i hello projektet.</span><span class="sxs-lookup"><span data-stu-id="68470-129">Create a new folder called **Scripts** in hello Project.</span></span> 
3. <span data-ttu-id="68470-130">Klicka på **Lägg till komponent -> Nytt skript -> C# skript för**.</span><span class="sxs-lookup"><span data-stu-id="68470-130">Click **Add Component-> New Script -> C# Script**.</span></span> <span data-ttu-id="68470-131">Ge den namnet **PlayerController**, och klicka på **skapa och Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="68470-131">Name it **PlayerController**, and click **Create and Add**.</span></span> <span data-ttu-id="68470-132">Detta skapar och koppla en skriptet toohello Player-objekt.</span><span class="sxs-lookup"><span data-stu-id="68470-132">This will create and attach a script toohello Player object.</span></span>  
   
    ![][14]
4. <span data-ttu-id="68470-133">Flytta det här skriptet under hello **skript** mapp i hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="68470-133">Move this script under hello **Scripts** folder in hello project.</span></span> 
5. <span data-ttu-id="68470-134">Öppna hello skript för redigering i din favorit Skriptredigeraren, uppdatera hello skriptkod med följande kod hello och spara den.</span><span class="sxs-lookup"><span data-stu-id="68470-134">Open hello script for editing in your favorite script editor, update hello script code with hello following code and save it.</span></span> 
   
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
6. <span data-ttu-id="68470-135">Observera hello skriptet ovan använder en **hastighet** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="68470-135">Note that hello script above uses a **Speed** property.</span></span> <span data-ttu-id="68470-136">Uppdatera hello hastighet egenskapen too10 i hello Unity-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="68470-136">In hello Unity editor, update hello speed property too10.</span></span>  
   
    ![][15]
7. <span data-ttu-id="68470-137">Träffa **spela upp** i hello Unity-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="68470-137">Hit **Play** in hello Unity Editor.</span></span> <span data-ttu-id="68470-138">Nu bör du kunna toocontrol hello kulan hello tangentbordet och den ska rotera och flytta runt.</span><span class="sxs-lookup"><span data-stu-id="68470-138">Now you should be able toocontrol hello ball using hello keyboard and it should rotate and move around.</span></span> 

### <a name="moving-hello-camera"></a><span data-ttu-id="68470-139">Flytta hello kamera</span><span class="sxs-lookup"><span data-stu-id="68470-139">Moving hello camera</span></span>
<span data-ttu-id="68470-140">hello stegen nedan är från hello [kursen Unity](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) och Binder hello **Main kamera** toohello **Player** objekt.</span><span class="sxs-lookup"><span data-stu-id="68470-140">hello steps below are from hello [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) and will tie hello **Main Camera** toohello **Player** object.</span></span> 

1. <span data-ttu-id="68470-141">Uppdatera hello **Transform.Position** toobe X = 0, Y = 10.5 Z-= 10.</span><span class="sxs-lookup"><span data-stu-id="68470-141">Update hello **Transform.Position** toobe X = 0,  Y = 10.5, Z=-10.</span></span>  
2. <span data-ttu-id="68470-142">Uppdatera hello **Transform.Rotation** toobe X = 45, Y = 0, Z = 0.</span><span class="sxs-lookup"><span data-stu-id="68470-142">Update hello **Transform.Rotation** toobe X = 45, Y = 0, Z = 0.</span></span>  
   
    ![][16]
3. <span data-ttu-id="68470-143">Lägg till ett nytt skript som heter **CameraController** toohello **MainCamera** och placera den under mappen för hello-skript.</span><span class="sxs-lookup"><span data-stu-id="68470-143">Add a new script called **CameraController** toohello **MainCamera** and move it under hello Scripts folder.</span></span> 
   
    ![][17]
4. <span data-ttu-id="68470-144">Öppna hello skript för redigering och Lägg till följande kod i den hello:</span><span class="sxs-lookup"><span data-stu-id="68470-144">Open up hello script for editing and add hello following code in it:</span></span>
   
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
5. <span data-ttu-id="68470-145">Miljö för Unity - dra hello Player variabeln till hello Player plats för hello Main kamera objekt så att hello två är kopplade till varandra.</span><span class="sxs-lookup"><span data-stu-id="68470-145">In Unity environment - drag hello Player variable into hello Player slot for hello Main Camera object so that hello two are associated with one another.</span></span> 
   
    ![][18]
6. <span data-ttu-id="68470-146">Nu om du trycka på Play i hello Unity-redigeraren och rotera hello Player kulan objekt visas sedan hello kamera efter den i hello förflyttning.</span><span class="sxs-lookup"><span data-stu-id="68470-146">Now if you hit Play in hello Unity editor and rotate hello Player Ball object then you will see hello Camera following it in hello movement.</span></span>  

### <a name="setting-up-hello-play-area"></a><span data-ttu-id="68470-147">Inställningar för hello Play området</span><span class="sxs-lookup"><span data-stu-id="68470-147">Setting up hello Play area</span></span>
<span data-ttu-id="68470-148">hello stegen nedan är från hello [kursen Unity](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141).</span><span class="sxs-lookup"><span data-stu-id="68470-148">hello steps below are from hello [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141).</span></span> <span data-ttu-id="68470-149">Vi skapar hello väggar omgivande hello grunden så att hello Player kulan objektet inte släppa ut hello play område i dess rörlighet.</span><span class="sxs-lookup"><span data-stu-id="68470-149">We will create hello Walls surrounding hello Ground so that hello Player Ball object doesn't drop off hello play area in its movement.</span></span> 

1. <span data-ttu-id="68470-150">Klicka på **skapa -> Skapa tom spelet objekt ->** och ger den namnet **väggar**</span><span class="sxs-lookup"><span data-stu-id="68470-150">Click **Create -> Create Empty -> Game Object** and name it **Walls**</span></span>
   
    ![][19]
2. <span data-ttu-id="68470-151">Under det här objektet väggar - skapa en ny **3D-objekt -> kuben** och ge den namnet ”Väst brandvägg”.</span><span class="sxs-lookup"><span data-stu-id="68470-151">Under this Walls object - create a new **3D Object -> Cube** and name it "West wall".</span></span> 
   
    ![][20]
3. <span data-ttu-id="68470-152">Uppdatera hello **Transform -> ställning** och **Transform -> skala** för det här objektet i West-brandvägg.</span><span class="sxs-lookup"><span data-stu-id="68470-152">Update hello **Transform -> Position** and **Transform -> Scale** for this West Wall object.</span></span> 
   
    ![][21]
4. <span data-ttu-id="68470-153">Duplicera hello Väst brandvägg toocreate en **Öst brandvägg** med hello uppdateras transformera placering och skala.</span><span class="sxs-lookup"><span data-stu-id="68470-153">Duplicate hello West wall toocreate an **East wall** with hello updated transform position and scale.</span></span> 
   
    ![][22]
5. <span data-ttu-id="68470-154">Duplicera hello Öst brandvägg toocreate en **Nord brandvägg** transformera position & skala med hello uppdateras.</span><span class="sxs-lookup"><span data-stu-id="68470-154">Duplicate hello East wall toocreate a **North wall** with hello updated transform position & scale.</span></span> 
   
    ![][23]
6. <span data-ttu-id="68470-155">Duplicera hello Nord brandvägg och skapa en **söder brandvägg** transformera position & skala med hello uppdateras.</span><span class="sxs-lookup"><span data-stu-id="68470-155">Duplicate hello North wall and create a **South wall** with hello updated transform position & scale.</span></span> 
   
    ![][24]

### <a name="creating-collectible-objects"></a><span data-ttu-id="68470-156">Skapa Domänbeständiga objekt</span><span class="sxs-lookup"><span data-stu-id="68470-156">Creating Collectible objects</span></span>
<span data-ttu-id="68470-157">hello stegen nedan är från hello [kursen Unity](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141).</span><span class="sxs-lookup"><span data-stu-id="68470-157">hello steps below are from hello [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141).</span></span> <span data-ttu-id="68470-158">Vi kommer att skapa några bra söker objekt som utgör hello uppsättning domänbeständiga objekt vilket hello Player kulan objekt måste too'collect' av kollision med dem.</span><span class="sxs-lookup"><span data-stu-id="68470-158">We will create some attractive looking objects which will form hello set of collectible objects which hello Player Ball object needs too'collect' by colliding with them.</span></span> 

1. <span data-ttu-id="68470-159">Skapa en ny **3D-kub objekt** och ge den namnet hämtning.</span><span class="sxs-lookup"><span data-stu-id="68470-159">Create a new **3D Cube object** and name it Pickup.</span></span> 
2. <span data-ttu-id="68470-160">Justera hello **Transform -> Rotation** & **Transform -> skala** i Pickup hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="68470-160">Adjust hello **Transform -> Rotation** & **Transform -> Scale** of hello Pickup object.</span></span> 
   
    ![][25]
3. <span data-ttu-id="68470-161">Skapa och koppla en **nytt C# skript** kallas **Rotator** toohello Pickup objekt.</span><span class="sxs-lookup"><span data-stu-id="68470-161">Create and attach a **new C# Script** called **Rotator** toohello Pickup object.</span></span> <span data-ttu-id="68470-162">Se till att tooput hello skript hello skript i mappen.</span><span class="sxs-lookup"><span data-stu-id="68470-162">Make sure tooput hello script under hello Scripts folder.</span></span> 
   
    ![][26]
4. <span data-ttu-id="68470-163">Öppna det här skriptet för att redigera och uppdatera den toobe hello följande:</span><span class="sxs-lookup"><span data-stu-id="68470-163">Open this script for editing and update it toobe hello following:</span></span> 
   
        using UnityEngine;
        using System.Collections;
   
        public class Rotator : MonoBehaviour {
   
            void Update () 
            {
                transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
            }
        }
5. <span data-ttu-id="68470-164">Nu att träffar hello Play läge för hello Unity-redigeraren och visa din Pickup objekt rotera på axeln.</span><span class="sxs-lookup"><span data-stu-id="68470-164">Now hit hello Play mode in hello Unity Editor and your Pickup object show be rotating on its axis.</span></span>
6. <span data-ttu-id="68470-165">Skapa en ny mapp som kallas **Prefabs**</span><span class="sxs-lookup"><span data-stu-id="68470-165">Create a new folder called **Prefabs**</span></span> 
   
    ![][27]
7. <span data-ttu-id="68470-166">Dra hello **hämtning** objektet och placera den i hello Prefabs mapp.</span><span class="sxs-lookup"><span data-stu-id="68470-166">Drag hello **Pickup** object and put it in hello Prefabs folder.</span></span>
   
    ![][28]
8. <span data-ttu-id="68470-167">Skapa en ny **tom spelet objektet** kallas **Upphämtningar**.</span><span class="sxs-lookup"><span data-stu-id="68470-167">Create a new **Empty Game object** called **Pickups**.</span></span> <span data-ttu-id="68470-168">Återställa sin position tooorigin och dra sedan hello Pickup objekt under detta spel objekt.</span><span class="sxs-lookup"><span data-stu-id="68470-168">Reset its position tooorigin and then drag hello Pickup object under this game object.</span></span>  
   
    ![][29]
9. <span data-ttu-id="68470-169">Dubbla hello **hämtning** objekt och sprids på hello **grunden** objektet runt hello **Player** objekt genom att uppdatera hello **Transform.Position's X & Z**  värden på lämpligt sätt.</span><span class="sxs-lookup"><span data-stu-id="68470-169">Duplicate hello **Pickup** object and spread it on hello **Ground** object around hello **Player** object by updating hello **Transform.Position's X & Z** values appropriately.</span></span> 
   
    ![][30]
10. <span data-ttu-id="68470-170">Skapa en **nytt material** kallas **hämtning** och uppdatera toobe röd färg genom att uppdatera hello **Albedo egenskapen** liknande toowhat vi för att uppdatera hello grunden objekt.</span><span class="sxs-lookup"><span data-stu-id="68470-170">Create a **new material** called **Pickup** and update it toobe Red in color by updating hello **Albedo property** similar toowhat we did for updating hello Ground object.</span></span> 
    
    ![][31]
11. <span data-ttu-id="68470-171">Tillämpa hello väsentlig tooall hello 4 pickup objekt.</span><span class="sxs-lookup"><span data-stu-id="68470-171">Apply hello material tooall hello 4 pickup objects.</span></span>
    
    ![][32]

### <a name="collecting-hello-pickup-objects"></a><span data-ttu-id="68470-172">Samla in hello Pickup objekt</span><span class="sxs-lookup"><span data-stu-id="68470-172">Collecting hello Pickup objects</span></span>
<span data-ttu-id="68470-173">hello stegen nedan är från hello [kursen Unity](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141).</span><span class="sxs-lookup"><span data-stu-id="68470-173">hello steps below are from hello [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141).</span></span> <span data-ttu-id="68470-174">Vi kommer att uppdatera hello Player så att den kan too'collect' hello pickup objekt av kollision med dem.</span><span class="sxs-lookup"><span data-stu-id="68470-174">We will update hello Player so that it is able too'collect' hello pickup objects by colliding with them.</span></span> 

1. <span data-ttu-id="68470-175">Öppna hello **PlayerController** skript bifogade toohello Player-objektet för redigering och uppdatera den toohello följande:</span><span class="sxs-lookup"><span data-stu-id="68470-175">Open up hello **PlayerController** script attached toohello Player object for editing and update it toohello following:</span></span>  
   
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
2. <span data-ttu-id="68470-176">Skapa en ny **taggen** kallas **Välj in** (den måste matcha i hello skript)</span><span class="sxs-lookup"><span data-stu-id="68470-176">Create a new **Tag** called **Pick Up** (it must match what is in hello script)</span></span>  
   
    ![][33]
   
    ![][34]
3. <span data-ttu-id="68470-177">Använd denna **taggen** toohello Prefab hämtning objekt.</span><span class="sxs-lookup"><span data-stu-id="68470-177">Apply this **Tag** toohello Prefab Pickup object.</span></span> 
   
    ![][35]
4. <span data-ttu-id="68470-178">Aktivera **IsTrigger** kryssrutan för hello Prefab objekt.</span><span class="sxs-lookup"><span data-stu-id="68470-178">Enable **IsTrigger** checkbox for hello Prefab object.</span></span>
   
    ![][36]
5. <span data-ttu-id="68470-179">Lägg till en fast tooPickup Prefab innehållsobjekt.</span><span class="sxs-lookup"><span data-stu-id="68470-179">Add a Rigid body tooPickup Prefab object.</span></span> <span data-ttu-id="68470-180">Vi kommer att uppdatera hello statiska collider vi använde tooa dynamiska collider optimera prestanda.</span><span class="sxs-lookup"><span data-stu-id="68470-180">For performance optimization we will update hello static collider that we used tooa Dynamic collider.</span></span> 
   
    ![][37]
6. <span data-ttu-id="68470-181">Slutligen Kontrollera hello **IsKinematic** egenskapen för prefab hello-objekt.</span><span class="sxs-lookup"><span data-stu-id="68470-181">Finally check hello **IsKinematic** property for hello prefab object.</span></span> 
   
    ![][38]
7. <span data-ttu-id="68470-182">Träffa **spela upp** i hello Unity-redigeraren och du kommer att kunna tooplay detta **Återställ en boll** spel genom att flytta hello Player-objektet med tangenterna för riktning indata.</span><span class="sxs-lookup"><span data-stu-id="68470-182">Hit **Play** in hello Unity editor and you will be able tooplay this **Roll a Ball** game by moving hello Player object using your keyboard keys for direction input.</span></span> 

### <a name="updating-hello-game-for-mobile-play"></a><span data-ttu-id="68470-183">Uppdatera hello spelet för mobila play</span><span class="sxs-lookup"><span data-stu-id="68470-183">Updating hello game for mobile play</span></span>
<span data-ttu-id="68470-184">hello avsnitt ovan ingångna hello grundläggande självstudierna från Unity.</span><span class="sxs-lookup"><span data-stu-id="68470-184">hello sections above concluded hello basic tutorial from Unity.</span></span> <span data-ttu-id="68470-185">Nu ska vi ändra hello spel toomake den mobila enheten eget.</span><span class="sxs-lookup"><span data-stu-id="68470-185">Now we will modify hello game toomake it mobile device friendly.</span></span> <span data-ttu-id="68470-186">Observera att vi tangentbordsinmatning för hello spelet hittills för testning.</span><span class="sxs-lookup"><span data-stu-id="68470-186">Note that we used keyboard input for hello game so far for testing.</span></span> <span data-ttu-id="68470-187">Nu ska vi ändra den så att vi kan kontrollera phone hello player genom att använda hello rörelse hello använda d.v.s. accelerationsmätare som hello indata.</span><span class="sxs-lookup"><span data-stu-id="68470-187">Now we will modify it so that we can control hello player by using hello motion of hello phone i.e. using Accelerometer as hello input.</span></span> 

<span data-ttu-id="68470-188">Öppna hello **PlayerController** skript för redigering och uppdatera hello **FixedUpdate** metoden toouse hello rörelse från hello accelerationsmätare toomove hello Player-objektet.</span><span class="sxs-lookup"><span data-stu-id="68470-188">Open up hello **PlayerController** script for editing and update hello **FixedUpdate** method toouse hello motion from hello accelerometer toomove hello Player object.</span></span> 

        void FixedUpdate()
        {
            //float moveHorizontal = Input.GetAxis("Horizontal");
            //float moveVertical = Input.GetAxis("Vertical");
            //Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
            rb.AddForce(Input.acceleration.x * Speed, 0, -Input.acceleration.z * Speed);
        }

<span data-ttu-id="68470-189">Den här självstudiekursen avslutar en grundläggande spel skapas med Unity och du kan distribuera den på en enhet för ditt val tooplay hello spel.</span><span class="sxs-lookup"><span data-stu-id="68470-189">This tutorial concludes a basic game creation with Unity and you can deploy this on a device of your choice tooplay hello game.</span></span> 

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













