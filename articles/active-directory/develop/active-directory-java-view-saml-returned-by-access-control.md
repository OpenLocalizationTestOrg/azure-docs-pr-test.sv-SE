---
title: Visa SAML som returneras av Access Control Service (Java)
description: "Lär dig hur du visar SAML som returneras av åtkomstkontrolltjänsten i Java-program i Azure."
services: active-directory
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 6cd216f9-eb43-46b4-b30d-f194d0ae2d48
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.custom: aaddev
ms.openlocfilehash: 1552e624a4703138ab82f7133ceaec3dbd04e1db
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-view-saml-returned-by-the-azure-access-control-service"></a><span data-ttu-id="fb53a-103">Så här visar du SAML som returnerats av tjänsten Azure Access Control</span><span class="sxs-lookup"><span data-stu-id="fb53a-103">How to view SAML returned by the Azure Access Control Service</span></span>
<span data-ttu-id="fb53a-104">Den här guiden visar hur du visar den underliggande Security Assertion Markup Language (SAML) returneras till ditt program med Azure Access Control Service (ACS).</span><span class="sxs-lookup"><span data-stu-id="fb53a-104">This guide will show you how to view the underlying Security Assertion Markup Language (SAML) returned to your application by the Azure Access Control Service (ACS).</span></span> <span data-ttu-id="fb53a-105">Guiden bygger på den [så Web autentisera användare med Azure Access Control Service med Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) avsnittet genom att ange koden som visar SAML-information.</span><span class="sxs-lookup"><span data-stu-id="fb53a-105">The guide builds on the [How to Authenticate Web Users with Azure Access Control Service Using Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) topic, by providing code that displays the SAML information.</span></span> <span data-ttu-id="fb53a-106">Det färdiga programmet kommer att se ut ungefär så här.</span><span class="sxs-lookup"><span data-stu-id="fb53a-106">The completed application will look similar to the following.</span></span>

![Exempel SAML-utdata][saml_output]

<span data-ttu-id="fb53a-108">Mer information om ACS finns i [nästa steg](#next_steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="fb53a-108">For more information on ACS, see the [Next steps](#next_steps) section.</span></span>

> [!NOTE]
> <span data-ttu-id="fb53a-109">Azure Access tjänstfilter kontrollen är en community technology preview.</span><span class="sxs-lookup"><span data-stu-id="fb53a-109">The Azure Access Services Control Filter is a community technology preview.</span></span> <span data-ttu-id="fb53a-110">Som förhandsversionen stöds den formellt inte av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="fb53a-110">As pre-release software, it is not formally supported by Microsoft.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="fb53a-111">Krav</span><span class="sxs-lookup"><span data-stu-id="fb53a-111">Prerequisites</span></span>
<span data-ttu-id="fb53a-112">Om du vill utföra åtgärderna i den här guiden Slutför exemplet på [så Web autentisera användare med Azure Access Control Service med Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) och använda den som utgångspunkt för den här kursen.</span><span class="sxs-lookup"><span data-stu-id="fb53a-112">To complete the tasks in this guide, complete the sample at [How to Authenticate Web Users with Azure Access Control Service Using Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) and use it as the starting point for this tutorial.</span></span>

## <a name="add-the-jspwriter-library-to-your-build-path-and-deployment-assembly"></a><span data-ttu-id="fb53a-113">Lägga till JspWriter bibliotek i sammansättningen build sökväg och distribution</span><span class="sxs-lookup"><span data-stu-id="fb53a-113">Add the JspWriter library to your build path and deployment assembly</span></span>
<span data-ttu-id="fb53a-114">Lägg till biblioteket som innehåller den **javax.servlet.jsp.JspWriter** klassen för att skapa sökvägen och distribution av sammansättningen.</span><span class="sxs-lookup"><span data-stu-id="fb53a-114">Add the library that contains the **javax.servlet.jsp.JspWriter** class to your build path and deployment assembly.</span></span> <span data-ttu-id="fb53a-115">Om du använder Tomcat biblioteket är **jsp-api.jar**, som finns i Apache **lib** mapp.</span><span class="sxs-lookup"><span data-stu-id="fb53a-115">If you are using Tomcat, the library is **jsp-api.jar**, which is located in the Apache **lib** folder.</span></span>

1. <span data-ttu-id="fb53a-116">I Eclipses Projektutforskaren, högerklickar du på **MyACSHelloWorld**, klickar du på **byggsökväg**, klickar du på **konfigurera byggsökväg**, klicka på den **bibliotek** fliken och klicka sedan på **lägga till externa JAR: er**.</span><span class="sxs-lookup"><span data-stu-id="fb53a-116">In Eclipse's Project Explorer, right-click **MyACSHelloWorld**, click **Build Path**, click **Configure Build Path**, click the **Libraries** tab, and then click **Add External JARs**.</span></span>
2. <span data-ttu-id="fb53a-117">I den **JAR markeringen** dialogrutan, navigera till den nödvändiga JAR markerar du den och klicka sedan på **öppna**.</span><span class="sxs-lookup"><span data-stu-id="fb53a-117">In the **JAR Selection** dialog, navigate to the necessary JAR, select it, and then click **Open**.</span></span>
3. <span data-ttu-id="fb53a-118">Med den **egenskaper för MyACSHelloWorld** fortfarande öppna dialogrutan **distribution sammansättningen**.</span><span class="sxs-lookup"><span data-stu-id="fb53a-118">With the **Properties for MyACSHelloWorld** dialog still open, click **Deployment Assembly**.</span></span>
4. <span data-ttu-id="fb53a-119">I den **Web distribution sammansättningen** dialogrutan klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="fb53a-119">In the **Web Deployment Assembly** dialog, click **Add**.</span></span>
5. <span data-ttu-id="fb53a-120">I den **nytt sammansättningen direktiv** dialogrutan klickar du på **Java Build Path poster** och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fb53a-120">In the **New Assembly Directive** dialog, click **Java Build Path Entries** and then click **Next**.</span></span>
6. <span data-ttu-id="fb53a-121">Välj lämpligt bibliotek och klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="fb53a-121">Select the appropriate library and click **Finish**.</span></span>
7. <span data-ttu-id="fb53a-122">Klicka på **OK** att stänga den **egenskaper för MyACSHelloWorld** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fb53a-122">Click **OK** to close the **Properties for MyACSHelloWorld** dialog.</span></span>

## <a name="modify-the-jsp-file-to-display-saml"></a><span data-ttu-id="fb53a-123">Ändra JSP-fil om du vill visa SAML</span><span class="sxs-lookup"><span data-stu-id="fb53a-123">Modify the JSP file to display SAML</span></span>
<span data-ttu-id="fb53a-124">Ändra **index.jsp** att använda följande kod.</span><span class="sxs-lookup"><span data-stu-id="fb53a-124">Modify **index.jsp** to use the following code.</span></span>

    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
        <%@ page import="javax.xml.parsers.*"
                 import="javax.xml.transform.*"
                 import="org.w3c.dom.*"
                 import="java.io.*"
                 import="javax.xml.transform.stream.*"
                 import="javax.xml.transform.dom.*"
                 import="javax.xml.xpath.*"
                 import="javax.servlet.jsp.JspWriter" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Sample ACS Filter</title>
    </head>
    <body>
        <h3>SAML information from sample ACS program</h3>
        <%!
        void displaySAMLInfo(Node node, String parent, JspWriter out)
        {

            try
            {
                String nodeName;
                int nChild, i;

                nodeName = node.getNodeName();
                out.println("<br>");
                out.println("<u>Examining <b>" + parent + nodeName + "</b></u><br>");

                   // Attributes.
                   NamedNodeMap attribsMap = node.getAttributes();
                   if (null != attribsMap)
                   {
                         for (i=0; i < attribsMap.getLength(); i++)
                         {
                                Node attrib = attribsMap.item(i);
                                out.println("Attribute: <b>" + attrib.getNodeName() + "</b>: " + attrib.getNodeValue()  + "<br>");
                         }
                   }

                   // Child nodes.
                   NodeList list = node.getChildNodes();
                   if (null != list)
                    {
                          nChild = list.getLength();
                          if (nChild > 0)
                          {                    

                                 // If it is a text node, just print the text.
                                 if (list.item(0).getNodeName() == "#text")
                                 {
                                     out.println("Text value: <b>" + list.item(0).getTextContent() + "</b><br>");
                                 }
                                 else
                                 {
                                     // Print out the child node names.
                                     out.print("Contains " + nChild + " child node(s): ");   
                                        for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);

                                        out.print("<b>" + temp.getNodeName() + "</b>");
                                        if (i < nChild - 1)
                                        {
                                            // Separate the names.
                                            out.print(", ");
                                        }
                                        else
                                        {
                                            // Finish the sentence.
                                            out.print(".");
                                        }

                                     }
                                     out.println("<br>");

                                     // Process the child nodes.
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        displaySAMLInfo(temp, parent + nodeName + "\\", out);
                                     }
                               }
                          }
                      }
                  }
                catch (Exception e)
                {
                    System.out.println("Exception encountered.");
                    e.printStackTrace();            
                }
            }
        %>

        <%
        try
        {
            String data  = (String) request.getAttribute("ACSSAML");

            DocumentBuilder docBuilder;
            Document doc = null;
            DocumentBuilderFactory docBuilderFactory = DocumentBuilderFactory.newInstance();
            docBuilderFactory.setIgnoringElementContentWhitespace(true);
            docBuilder = docBuilderFactory.newDocumentBuilder();
            byte[] xmlDATA = data.getBytes();

            ByteArrayInputStream in = new ByteArrayInputStream(xmlDATA);
            doc = docBuilder.parse(in);
            doc.getDocumentElement().normalize();

            // Iterate the child nodes of the doc.
            NodeList list = doc.getChildNodes();

            for (int i=0; i < list.getLength(); i++)
            {
                displaySAMLInfo(list.item(i), "", out);
            }
        }
        catch (Exception e)
        {
            out.println("Exception encountered.");
            e.printStackTrace();
        }

        %>
    </body>
    </html>

## <a name="run-the-application"></a><span data-ttu-id="fb53a-125">Köra programmet</span><span class="sxs-lookup"><span data-stu-id="fb53a-125">Run the application</span></span>
1. <span data-ttu-id="fb53a-126">Kör ditt program i emulatorn dator eller distribuera till Azure med hjälp av stegen som beskrivs i [så Web autentisera användare med Azure Access Control Service med Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md).</span><span class="sxs-lookup"><span data-stu-id="fb53a-126">Run your application in the computer emulator or deploy to Azure, using the steps documented at [How to Authenticate Web Users with Azure Access Control Service Using Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md).</span></span>
2. <span data-ttu-id="fb53a-127">Starta en webbläsare och öppna ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="fb53a-127">Launch a browser and open your web application.</span></span> <span data-ttu-id="fb53a-128">När du loggar in på ditt program, ser du SAML-information, inklusive den security assertion tillhandahållits av identitetsleverantören.</span><span class="sxs-lookup"><span data-stu-id="fb53a-128">After you log on to your application, you'll see SAML information, including the security assertion provided by the identity provider.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fb53a-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fb53a-129">Next steps</span></span>
<span data-ttu-id="fb53a-130">Utforska att ytterligare funktioner för Gransknings-och insamlingstjänstens och om du vill experimentera med mer avancerade scenarier, se [Access Control Service 2.0][Access Control Service 2.0].</span><span class="sxs-lookup"><span data-stu-id="fb53a-130">To further explore ACS's functionality and to experiment with more sophisticated scenarios, see [Access Control Service 2.0][Access Control Service 2.0].</span></span>

[Prerequisites]: #pre
[Modify the JSP file to display SAML]: #modify_jsp
[Add the JspWriter library to your build path and deployment assembly]: #add_library
[Run the application]: #run_application
[Next steps]: #next_steps
[Access Control Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[How to Authenticate Web Users with Azure Access Control Service Using Eclipse]: active-directory-java-authenticate-users-access-control-eclipse
[saml_output]: ./media/active-directory-java-view-saml-returned-by-access-control/SAML_Output.png
