---
title: "Unity sammanslagning kulan självstudiekursen"
description: "Steg för att skapa klassiska kursen Unity Roll en spel som är ett krav för alla kurser i Mobile Engagement Unity"
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
ms.openlocfilehash: 6392d1f780b1bc2348fee5947550b05e86ea4de2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <span data-ttu-id="009a1-103"><a id="unity-roll-a-ball"></a>Skapa kursen Unity Roll en spel</span><span class="sxs-lookup"><span data-stu-id="009a1-103"><a id="unity-roll-a-ball"></a>Create Unity Roll a Ball game</span></span>
<span data-ttu-id="009a1-104">Den här kursen går igenom de viktigaste stegen för en lite annorlunda [kursen Unity Roll kulan självstudiekursen](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial).</span><span class="sxs-lookup"><span data-stu-id="009a1-104">This tutorial walks through the main steps for a slightly modified [Unity Roll a Ball tutorial](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial).</span></span> <span data-ttu-id="009a1-105">Det här exemplet spelet består av ett sfäriskt player-objekt som kontrolleras av app-användare och syftet med spelet är ”samla in ' domänbeständiga objekt av kollision player-objektet med objekten domänbeständiga.</span><span class="sxs-lookup"><span data-stu-id="009a1-105">This sample game consists of a spherical 'player' object which is controlled by the app user and the objective of the game is to 'collect' collectible objects by colliding the player object with these collectible objects.</span></span> <span data-ttu-id="009a1-106">Detta förutsätter grundläggande kunskaper med Unity Redigeraren för miljön.</span><span class="sxs-lookup"><span data-stu-id="009a1-106">This assumes basic familiarity with Unity editor environment.</span></span> <span data-ttu-id="009a1-107">Om du stöter på problem ska du gå till fullständig genomgång.</span><span class="sxs-lookup"><span data-stu-id="009a1-107">If you run into any issues then you should refer to the full tutorial.</span></span> 

### <a name="setting-up-the-game"></a><span data-ttu-id="009a1-108">Ställa in spelet</span><span class="sxs-lookup"><span data-stu-id="009a1-108">Setting up the game</span></span>
<span data-ttu-id="009a1-109">Stegen nedan är från den [kursen Unity](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)</span><span class="sxs-lookup"><span data-stu-id="009a1-109">The steps below are from the [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)</span></span>

1. <span data-ttu-id="009a1-110">Öppna **Unity Editor** och på **ny**.</span><span class="sxs-lookup"><span data-stu-id="009a1-110">Open **Unity Editor** and click **New**.</span></span> 
   
    ![][51] 
2. <span data-ttu-id="009a1-111">Ange en **projektnamn** & **plats**väljer **3D** och på **skapa projekt**.</span><span class="sxs-lookup"><span data-stu-id="009a1-111">Provide a **Project name** & **Location**, select **3D** and click **Create project**.</span></span>
   
    ![][52]
3. <span data-ttu-id="009a1-112">Spara den standard scen som precis har skapat som en del av det nya projektet som med namnet **befälet** i en ny  **\_i bakgrunden** mapp under **tillgångar** mapp:</span><span class="sxs-lookup"><span data-stu-id="009a1-112">Save the default scene just created as part of the new project as with the name **MiniGame** within a new **\_Scenes** folder under **Assets** folder:</span></span>
   
    ![][53]
4. <span data-ttu-id="009a1-113">Skapa en **3D-objekt -> plan** som den spela upp och Byt namn på objektet plan som **grunden**</span><span class="sxs-lookup"><span data-stu-id="009a1-113">Create a **3D Object -> Plane** as the playing field and rename this plane object as **Ground**</span></span>
   
    ![][1]
5. <span data-ttu-id="009a1-114">Återställa transform-komponenten för detta **grunden** objekt så att den har ursprung.</span><span class="sxs-lookup"><span data-stu-id="009a1-114">Reset the transform component for this **Ground** object so that it is at the Origin.</span></span> 
   
    ![][3]
6. <span data-ttu-id="009a1-115">Avmarkera **Visa rutnät** från **Gizmos menyn** för den **grunden** objekt.</span><span class="sxs-lookup"><span data-stu-id="009a1-115">Uncheck **Show Grid** from **Gizmos menu** for the **Ground** object.</span></span>
   
    ![][4]
7. <span data-ttu-id="009a1-116">Uppdatering av **skala** komponenten för den **grunden** objektet ska vara [X = 2, Y = 1, Z = 2].</span><span class="sxs-lookup"><span data-stu-id="009a1-116">Update the **Scale** component for the **Ground** object to be [X = 2,Y = 1, Z = 2].</span></span> 
   
    ![][5]
8. <span data-ttu-id="009a1-117">Lägg till en ny **3D-objekt -> sfär** till projektet och Byt namn på den här sfär objekt som **Player**.</span><span class="sxs-lookup"><span data-stu-id="009a1-117">Add a new **3D Object -> Sphere** to the project and rename this sphere object as **Player**.</span></span> 
   
    ![][6]
9. <span data-ttu-id="009a1-118">Välj den **Player** och klicka på **Återställ omvandling** liknar plan-objektet.</span><span class="sxs-lookup"><span data-stu-id="009a1-118">Select the **Player** object and click **Reset Transform** similar to the Plane object.</span></span> 
10. <span data-ttu-id="009a1-119">Uppdatera **Transform -> ställning -> Y-koordinaten** komponenten för Player Y as 0,5.</span><span class="sxs-lookup"><span data-stu-id="009a1-119">Update **Transform -> Position -> Y Coordinate** component for the Player Y as 0.5.</span></span>  
    
    ![][7]
11. <span data-ttu-id="009a1-120">Skapa en ny mapp som kallas **material** i projekt där vi skapar material som färg Windows Media player.</span><span class="sxs-lookup"><span data-stu-id="009a1-120">Create a new folder called **Materials** in the project where we will create the material to color the player.</span></span> 
12. <span data-ttu-id="009a1-121">Skapa en ny **Material** kallas **bakgrund** i den här mappen.</span><span class="sxs-lookup"><span data-stu-id="009a1-121">Create a new **Material** called **Background** in this folder.</span></span> 
    
    ![][8]
13. <span data-ttu-id="009a1-122">Uppdatera färgen på materialet genom att uppdatera den **Albedo** -egenskapen för den.</span><span class="sxs-lookup"><span data-stu-id="009a1-122">Update the color of the material by updating the **Albedo** property of it.</span></span> <span data-ttu-id="009a1-123">Du kan välja [0,32,64] RGB-värden.</span><span class="sxs-lookup"><span data-stu-id="009a1-123">You can select the RGB values of [0,32,64].</span></span> 
    
    ![][9]
14. <span data-ttu-id="009a1-124">Dra detta material till scen-vyn för att tillämpa färgen som den **grunden** objekt.</span><span class="sxs-lookup"><span data-stu-id="009a1-124">Drag this material into the scene view to apply color to the **Ground** object.</span></span> 
    
    ![][10]
15. <span data-ttu-id="009a1-125">Slutligen uppdaterar den **Transform -> Rotation Y ->** till 60 på riktad enstaka objekt för tydlighetens skull.</span><span class="sxs-lookup"><span data-stu-id="009a1-125">Finally update the **Transform -> Rotation -> Y** to 60 on the Directional Light object for clarity.</span></span> 
    
    ![][12]

### <a name="moving-the-player"></a><span data-ttu-id="009a1-126">Flyttar Windows Media player</span><span class="sxs-lookup"><span data-stu-id="009a1-126">Moving the player</span></span>
<span data-ttu-id="009a1-127">Stegen nedan är från den [kursen Unity](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)</span><span class="sxs-lookup"><span data-stu-id="009a1-127">The steps below are from the [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)</span></span>

1. <span data-ttu-id="009a1-128">Lägg till en **RigidBody** komponenten till den **Player** objekt.</span><span class="sxs-lookup"><span data-stu-id="009a1-128">Add a **RigidBody** component to the **Player** object.</span></span> 
   
    ![][13]
2. <span data-ttu-id="009a1-129">Skapa en ny mapp som kallas **skript** i projektet.</span><span class="sxs-lookup"><span data-stu-id="009a1-129">Create a new folder called **Scripts** in the Project.</span></span> 
3. <span data-ttu-id="009a1-130">Klicka på **Lägg till komponent -> Nytt skript -> C# skript för**.</span><span class="sxs-lookup"><span data-stu-id="009a1-130">Click **Add Component-> New Script -> C# Script**.</span></span> <span data-ttu-id="009a1-131">Ge den namnet **PlayerController**, och klicka på **skapa och Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="009a1-131">Name it **PlayerController**, and click **Create and Add**.</span></span> <span data-ttu-id="009a1-132">Detta skapar och bifogar ett skript till Player-objektet.</span><span class="sxs-lookup"><span data-stu-id="009a1-132">This will create and attach a script to the Player object.</span></span>  
   
    ![][14]
4. <span data-ttu-id="009a1-133">Flytta det här skriptet under den **skript** mappen i projektet.</span><span class="sxs-lookup"><span data-stu-id="009a1-133">Move this script under the **Scripts** folder in the project.</span></span> 
5. <span data-ttu-id="009a1-134">Öppna skriptet för redigering i din favorit Skriptredigeraren, uppdatera skriptkoden med följande kod och spara den.</span><span class="sxs-lookup"><span data-stu-id="009a1-134">Open the script for editing in your favorite script editor, update the script code with the following code and save it.</span></span> 
   
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
6. <span data-ttu-id="009a1-135">Observera att skriptet ovan använder en **hastighet** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="009a1-135">Note that the script above uses a **Speed** property.</span></span> <span data-ttu-id="009a1-136">Uppdatera egenskapen hastighet till 10 i Unity-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="009a1-136">In the Unity editor, update the speed property to 10.</span></span>  
   
    ![][15]
7. <span data-ttu-id="009a1-137">Träffa **spela upp** i Unity redigeraren.</span><span class="sxs-lookup"><span data-stu-id="009a1-137">Hit **Play** in the Unity Editor.</span></span> <span data-ttu-id="009a1-138">Du ska nu kunna styra kulan med hjälp av tangentbordet och den ska rotera och flytta runt.</span><span class="sxs-lookup"><span data-stu-id="009a1-138">Now you should be able to control the ball using the keyboard and it should rotate and move around.</span></span> 

### <a name="moving-the-camera"></a><span data-ttu-id="009a1-139">Flytta kameran</span><span class="sxs-lookup"><span data-stu-id="009a1-139">Moving the camera</span></span>
<span data-ttu-id="009a1-140">Stegen nedan är från den [kursen Unity](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) och Binder det **Main kamera** till den **Player** objekt.</span><span class="sxs-lookup"><span data-stu-id="009a1-140">The steps below are from the [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) and will tie the **Main Camera** to the **Player** object.</span></span> 

1. <span data-ttu-id="009a1-141">Uppdatering av **Transform.Position** ska X = 0, Y = 10.5 Z-= 10.</span><span class="sxs-lookup"><span data-stu-id="009a1-141">Update the **Transform.Position** to be X = 0,  Y = 10.5, Z=-10.</span></span>  
2. <span data-ttu-id="009a1-142">Uppdatering av **Transform.Rotation** ska X = 45, Y = 0, Z = 0.</span><span class="sxs-lookup"><span data-stu-id="009a1-142">Update the **Transform.Rotation** to be X = 45, Y = 0, Z = 0.</span></span>  
   
    ![][16]
3. <span data-ttu-id="009a1-143">Lägg till ett nytt skript som heter **CameraController** till den **MainCamera** och placera den under mappen skript.</span><span class="sxs-lookup"><span data-stu-id="009a1-143">Add a new script called **CameraController** to the **MainCamera** and move it under the Scripts folder.</span></span> 
   
    ![][17]
4. <span data-ttu-id="009a1-144">Öppna skriptet för redigering och Lägg till följande kod i den:</span><span class="sxs-lookup"><span data-stu-id="009a1-144">Open up the script for editing and add the following code in it:</span></span>
   
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
5. <span data-ttu-id="009a1-145">Miljö för Unity - dra Player-variabeln i Player fack för objektet Main kamera så att två är kopplade till varandra.</span><span class="sxs-lookup"><span data-stu-id="009a1-145">In Unity environment - drag the Player variable into the Player slot for the Main Camera object so that the two are associated with one another.</span></span> 
   
    ![][18]
6. <span data-ttu-id="009a1-146">Nu om du trycka på Play i Unity-redigeraren och rotera Player kulan visas sedan kameran efter den i rörelse.</span><span class="sxs-lookup"><span data-stu-id="009a1-146">Now if you hit Play in the Unity editor and rotate the Player Ball object then you will see the Camera following it in the movement.</span></span>  

### <a name="setting-up-the-play-area"></a><span data-ttu-id="009a1-147">Inställningar för området Play</span><span class="sxs-lookup"><span data-stu-id="009a1-147">Setting up the Play area</span></span>
<span data-ttu-id="009a1-148">Stegen nedan är från den [kursen Unity](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141).</span><span class="sxs-lookup"><span data-stu-id="009a1-148">The steps below are from the [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141).</span></span> <span data-ttu-id="009a1-149">Vi skapar väggar omgivande grunden så att objektet Player kulan inte släppa ut området play i dess rörlighet.</span><span class="sxs-lookup"><span data-stu-id="009a1-149">We will create the Walls surrounding the Ground so that the Player Ball object doesn't drop off the play area in its movement.</span></span> 

1. <span data-ttu-id="009a1-150">Klicka på **skapa -> Skapa tom spelet objekt ->** och ger den namnet **väggar**</span><span class="sxs-lookup"><span data-stu-id="009a1-150">Click **Create -> Create Empty -> Game Object** and name it **Walls**</span></span>
   
    ![][19]
2. <span data-ttu-id="009a1-151">Under det här objektet väggar - skapa en ny **3D-objekt -> kuben** och ge den namnet ”Väst brandvägg”.</span><span class="sxs-lookup"><span data-stu-id="009a1-151">Under this Walls object - create a new **3D Object -> Cube** and name it "West wall".</span></span> 
   
    ![][20]
3. <span data-ttu-id="009a1-152">Uppdatering av **Transform -> ställning** och **Transform -> skala** för det här objektet i West-brandvägg.</span><span class="sxs-lookup"><span data-stu-id="009a1-152">Update the **Transform -> Position** and **Transform -> Scale** for this West Wall object.</span></span> 
   
    ![][21]
4. <span data-ttu-id="009a1-153">Duplicera West-Brandvägg för att skapa en **Öst brandvägg** med den uppdaterade transformera placering och skala.</span><span class="sxs-lookup"><span data-stu-id="009a1-153">Duplicate the West wall to create an **East wall** with the updated transform position and scale.</span></span> 
   
    ![][22]
5. <span data-ttu-id="009a1-154">Duplicera Öst-Brandvägg för att skapa en **Nord brandvägg** med den uppdaterade transformera position & skala.</span><span class="sxs-lookup"><span data-stu-id="009a1-154">Duplicate the East wall to create a **North wall** with the updated transform position & scale.</span></span> 
   
    ![][23]
6. <span data-ttu-id="009a1-155">Duplicera Nord-brandvägg och skapa en **söder brandvägg** med den uppdaterade transformera position & skala.</span><span class="sxs-lookup"><span data-stu-id="009a1-155">Duplicate the North wall and create a **South wall** with the updated transform position & scale.</span></span> 
   
    ![][24]

### <a name="creating-collectible-objects"></a><span data-ttu-id="009a1-156">Skapa Domänbeständiga objekt</span><span class="sxs-lookup"><span data-stu-id="009a1-156">Creating Collectible objects</span></span>
<span data-ttu-id="009a1-157">Stegen nedan är från den [kursen Unity](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141).</span><span class="sxs-lookup"><span data-stu-id="009a1-157">The steps below are from the [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141).</span></span> <span data-ttu-id="009a1-158">Vi skapar vissa snygg söker objekt som utgör uppsättningen domänbeständiga objekt som objektet Player kulan måste samla in av kollision med dem.</span><span class="sxs-lookup"><span data-stu-id="009a1-158">We will create some attractive looking objects which will form the set of collectible objects which the Player Ball object needs to 'collect' by colliding with them.</span></span> 

1. <span data-ttu-id="009a1-159">Skapa en ny **3D-kub objekt** och ge den namnet hämtning.</span><span class="sxs-lookup"><span data-stu-id="009a1-159">Create a new **3D Cube object** and name it Pickup.</span></span> 
2. <span data-ttu-id="009a1-160">Justera det **Transform -> Rotation** & **Transform -> skala** i objektet Pickup.</span><span class="sxs-lookup"><span data-stu-id="009a1-160">Adjust the **Transform -> Rotation** & **Transform -> Scale** of the Pickup object.</span></span> 
   
    ![][25]
3. <span data-ttu-id="009a1-161">Skapa och koppla en **nytt C# skript** kallas **Rotator** till objektet Pickup.</span><span class="sxs-lookup"><span data-stu-id="009a1-161">Create and attach a **new C# Script** called **Rotator** to the Pickup object.</span></span> <span data-ttu-id="009a1-162">Se till att placera skriptet under mappen skript.</span><span class="sxs-lookup"><span data-stu-id="009a1-162">Make sure to put the script under the Scripts folder.</span></span> 
   
    ![][26]
4. <span data-ttu-id="009a1-163">Öppna det här skriptet för att redigera och uppdatera den om du vill ha följande:</span><span class="sxs-lookup"><span data-stu-id="009a1-163">Open this script for editing and update it to be the following:</span></span> 
   
        using UnityEngine;
        using System.Collections;
   
        public class Rotator : MonoBehaviour {
   
            void Update () 
            {
                transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
            }
        }
5. <span data-ttu-id="009a1-164">Nu träffar rotera Play-läge för Unity-redigeraren och visa din Pickup objektet på axeln.</span><span class="sxs-lookup"><span data-stu-id="009a1-164">Now hit the Play mode in the Unity Editor and your Pickup object show be rotating on its axis.</span></span>
6. <span data-ttu-id="009a1-165">Skapa en ny mapp som kallas **Prefabs**</span><span class="sxs-lookup"><span data-stu-id="009a1-165">Create a new folder called **Prefabs**</span></span> 
   
    ![][27]
7. <span data-ttu-id="009a1-166">Dra den **hämtning** objekt och placera den i mappen Prefabs.</span><span class="sxs-lookup"><span data-stu-id="009a1-166">Drag the **Pickup** object and put it in the Prefabs folder.</span></span>
   
    ![][28]
8. <span data-ttu-id="009a1-167">Skapa en ny **tom spelet objektet** kallas **Upphämtningar**.</span><span class="sxs-lookup"><span data-stu-id="009a1-167">Create a new **Empty Game object** called **Pickups**.</span></span> <span data-ttu-id="009a1-168">Återställa dess position till ursprung och dra objektet Pickup under detta spel objekt.</span><span class="sxs-lookup"><span data-stu-id="009a1-168">Reset its position to origin and then drag the Pickup object under this game object.</span></span>  
   
    ![][29]
9. <span data-ttu-id="009a1-169">Duplicera den **hämtning** objekt och sprids på den **grunden** objekt runt den **Player** objekt genom att uppdatera den **Transform.Position's X & Z** värden på lämpligt sätt.</span><span class="sxs-lookup"><span data-stu-id="009a1-169">Duplicate the **Pickup** object and spread it on the **Ground** object around the **Player** object by updating the **Transform.Position's X & Z** values appropriately.</span></span> 
   
    ![][30]
10. <span data-ttu-id="009a1-170">Skapa en **nytt material** kallas **hämtning** och uppdatera den om du vill ha röd färg genom att uppdatera den **Albedo egenskapen** liknar vi gjorde för att uppdatera objektet grunden.</span><span class="sxs-lookup"><span data-stu-id="009a1-170">Create a **new material** called **Pickup** and update it to be Red in color by updating the **Albedo property** similar to what we did for updating the Ground object.</span></span> 
    
    ![][31]
11. <span data-ttu-id="009a1-171">Använda material 4 pickup objekten.</span><span class="sxs-lookup"><span data-stu-id="009a1-171">Apply the material to all the 4 pickup objects.</span></span>
    
    ![][32]

### <a name="collecting-the-pickup-objects"></a><span data-ttu-id="009a1-172">Samla in Pickup objekt</span><span class="sxs-lookup"><span data-stu-id="009a1-172">Collecting the Pickup objects</span></span>
<span data-ttu-id="009a1-173">Stegen nedan är från den [kursen Unity](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141).</span><span class="sxs-lookup"><span data-stu-id="009a1-173">The steps below are from the [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141).</span></span> <span data-ttu-id="009a1-174">Windows Media Player kan uppdateras så att den är ”samla in ' pickup objekt av kollision med dem.</span><span class="sxs-lookup"><span data-stu-id="009a1-174">We will update the Player so that it is able to 'collect' the pickup objects by colliding with them.</span></span> 

1. <span data-ttu-id="009a1-175">Öppna den **PlayerController** skript kopplat till Player-objektet för redigering och uppdatera den till följande:</span><span class="sxs-lookup"><span data-stu-id="009a1-175">Open up the **PlayerController** script attached to the Player object for editing and update it to the following:</span></span>  
   
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
2. <span data-ttu-id="009a1-176">Skapa en ny **taggen** kallas **Välj in** (den måste matcha det som finns i skriptet)</span><span class="sxs-lookup"><span data-stu-id="009a1-176">Create a new **Tag** called **Pick Up** (it must match what is in the script)</span></span>  
   
    ![][33]
   
    ![][34]
3. <span data-ttu-id="009a1-177">Använd denna **taggen** till objektet Prefab hämtning.</span><span class="sxs-lookup"><span data-stu-id="009a1-177">Apply this **Tag** to the Prefab Pickup object.</span></span> 
   
    ![][35]
4. <span data-ttu-id="009a1-178">Aktivera **IsTrigger** kryssrutan för Prefab-objektet.</span><span class="sxs-lookup"><span data-stu-id="009a1-178">Enable **IsTrigger** checkbox for the Prefab object.</span></span>
   
    ![][36]
5. <span data-ttu-id="009a1-179">Lägg till en fast text till hämtning Prefab objekt.</span><span class="sxs-lookup"><span data-stu-id="009a1-179">Add a Rigid body to Pickup Prefab object.</span></span> <span data-ttu-id="009a1-180">Optimera prestanda uppdateras vi statiska collider som vi använde till en dynamisk collider.</span><span class="sxs-lookup"><span data-stu-id="009a1-180">For performance optimization we will update the static collider that we used to a Dynamic collider.</span></span> 
   
    ![][37]
6. <span data-ttu-id="009a1-181">Kontrollera slutligen den **IsKinematic** -egenskapen för prefab.</span><span class="sxs-lookup"><span data-stu-id="009a1-181">Finally check the **IsKinematic** property for the prefab object.</span></span> 
   
    ![][38]
7. <span data-ttu-id="009a1-182">Träffa **spela upp** i Unity-redigeraren och du kommer att kunna spelas upp **Återställ en boll** Spelenheter genom att flytta Player-objektet med tangenterna för riktning indata.</span><span class="sxs-lookup"><span data-stu-id="009a1-182">Hit **Play** in the Unity editor and you will be able to play this **Roll a Ball** game by moving the Player object using your keyboard keys for direction input.</span></span> 

### <a name="updating-the-game-for-mobile-play"></a><span data-ttu-id="009a1-183">Uppdatera spelet för mobila play</span><span class="sxs-lookup"><span data-stu-id="009a1-183">Updating the game for mobile play</span></span>
<span data-ttu-id="009a1-184">Ovan drar slutsatsen Unity grundläggande vägledningen.</span><span class="sxs-lookup"><span data-stu-id="009a1-184">The sections above concluded the basic tutorial from Unity.</span></span> <span data-ttu-id="009a1-185">Nu ska vi ändra spelet så att den mobila enheten eget.</span><span class="sxs-lookup"><span data-stu-id="009a1-185">Now we will modify the game to make it mobile device friendly.</span></span> <span data-ttu-id="009a1-186">Observera att vi tangentbordsinmatning för spelet hittills för testning.</span><span class="sxs-lookup"><span data-stu-id="009a1-186">Note that we used keyboard input for the game so far for testing.</span></span> <span data-ttu-id="009a1-187">Nu ändrar vi den så att vi kan styra Windows Media player genom att använda d.v.s. rörelse telefon</span><span class="sxs-lookup"><span data-stu-id="009a1-187">Now we will modify it so that we can control the player by using the motion of the phone i.e.</span></span> <span data-ttu-id="009a1-188">med hjälp av accelerationsmätare som indata.</span><span class="sxs-lookup"><span data-stu-id="009a1-188">using Accelerometer as the input.</span></span> 

<span data-ttu-id="009a1-189">Öppna den **PlayerController** skript för att redigera och uppdatera den **FixedUpdate** metod för att använda rörelse från accelerationsmätare för att flytta Player-objektet.</span><span class="sxs-lookup"><span data-stu-id="009a1-189">Open up the **PlayerController** script for editing and update the **FixedUpdate** method to use the motion from the accelerometer to move the Player object.</span></span> 

        void FixedUpdate()
        {
            //float moveHorizontal = Input.GetAxis("Horizontal");
            //float moveVertical = Input.GetAxis("Vertical");
            //Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
            rb.AddForce(Input.acceleration.x * Speed, 0, -Input.acceleration.z * Speed);
        }

<span data-ttu-id="009a1-190">Den här självstudiekursen avslutar en grundläggande spel skapas med Unity och du kan distribuera den på en enhet som du väljer att spela.</span><span class="sxs-lookup"><span data-stu-id="009a1-190">This tutorial concludes a basic game creation with Unity and you can deploy this on a device of your choice to play the game.</span></span> 

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













