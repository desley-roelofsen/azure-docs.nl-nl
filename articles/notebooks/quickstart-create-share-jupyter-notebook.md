---
title: Maken en delen van een Jupyter-notebook op Azure
description: Snel maken en uitvoeren van een Jupyter-notebook op Azure-Notebooks en vervolgens die laptop met anderen delen.
ms.topic: quickstart
ms.date: 12/04/2018
ms.openlocfilehash: 71220fa5aa0367d1cb1694582b4f96459a3016e7
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2019
ms.locfileid: "74277504"
---
# <a name="quickstart-create-and-share-a-notebook"></a>QuickStart: Maken en delen van een laptop

1. Ga naar [Azure notebooks](https://notebooks.azure.com) en meld u aan. (Zie Quick Start ( [Aanmelden bij Azure notebooks](quickstart-sign-in-azure-notebooks.md)) voor meer informatie.

1. Selecteer op de pagina openbaar profiel **Mijn projecten** boven aan de pagina:

    ![Mijn projecten koppeling boven aan het browservenster](media/quickstarts/my-projects-link.png)

1. Selecteer op de pagina **Mijn projecten** **+ Nieuw project** (sneltoets: n); de knop kan alleen worden weer gegeven als **+** als het browser venster smal is:

    ![Nieuw Project-opdracht op de pagina Mijn projecten](media/quickstarts/new-project-command.png)

1. In de pop-up **Nieuw project maken** die wordt weer gegeven, voert u de volgende gegevens in of stelt u deze in en selecteert u **maken**:

   - **Project naam**: Hallo wereld in python
   - **Project-id**: Hallo-wereld-python
   - **Openbaar project**: (uitgeschakeld)
   - **Een README.MD maken**: (uitgeschakeld)

     ![Nieuw Project met ingevulde details](media/quickstarts/new-project-popup.png)

1. Na enkele ogenblikken navigeert Azure notitieblokken u naar het nieuwe project. Voeg een notitie blok toe aan het project door de vervolg keuzelijst **+ Nieuw** te selecteren (dit kan worden weer gegeven als alleen **+** ) en selecteer vervolgens **notebook**:

    [![](media/quickstarts/empty-project-new-notebook-button.png "A new, empty project and add notebook command")](media/quickstarts/empty-project-new-notebook-button.png#lightbox)

1. Voer in de pop-up **Nieuw notitie blok maken** die verschijnt een bestands naam in voor uw notitie blok, zoals *HelloWorldInPython. ipynb* ( *. ipynb* betekent ironpython (Jupyter) notebook) en selecteer **python 3,6** voor de taal (ook wel de *kernel*genoemd):

    ![Het pop-upvenster voor de nieuwe Notebook maken](media/quickstarts/new-notebook-popup.png)

1. Selecteer **Nieuw** om het notitie blok te maken dat vervolgens wordt weer gegeven in de lijst met bestanden van uw project:

    ![Nieuwe notebook wordt weergegeven in de lijst met bestanden van het project](media/quickstarts/new-notebook-created.png)

## <a name="run-the-notebook"></a>Het notitieblok uitvoeren

1. Selecteer de nieuwe notebook uit te voeren in de editor. de kernel die u hebt geselecteerd (Python 3.6 in dit voorbeeld) wordt automatisch geactiveerd voor dit notitieblok:

    ![Weergave van een nieuwe notebook in Azure-notitieblokken](media/quickstarts/create-notebook-first-open.png)

1. De notebook heeft standaard een lege codecel. Als u het type van de cel wilt wijzigen in **verlaagd, gebruikt**u de vervolg keuzelijst cellen type om **prijs verlaging**te selecteren:

    ![Wijzigen van het celtype in een nieuwe notebook](media/quickstarts/create-notebook-cell-type.png)

1. Typ of plak de volgende koptekst in de cel:

    ```markdown
    # Hello World in Python
    ```

1. Omdat het bewerken van Markdown, is de tekst wordt weergegeven als een header met de "#". Selecteer de knop **uitvoeren** om de prijs verlaging in HTML weer te geven. Azure-notitieblokken automatisch maakt vervolgens een nieuwe codecel daarna:

    ![De knop uitvoeren voor een cel en het gerenderde Markdown](media/quickstarts/run-cell-markdown-render.png)

1. Voer de volgende Python-code in de codecel:

    ```python
    from datetime import datetime

    now = datetime.now()
    msg = "Hello, Azure Notebooks! Today is %s"  % now.strftime("%A, %d %B, %Y")
    print(msg)
    ```

1. Selecteer **uitvoeren** (sneltoets: Shift + Enter) om de code uit te voeren. U ziet onder de cel geslaagde uitvoer is vergelijkbaar met de volgende tekst:

    ```output
    Hello, Azure Notebooks! Today is Thursday, 15 November, 2018
    ```

1. Selecteer de opslaan pictogram van uw werk op te slaan:

    ![Pictogram opslaan op de werkbalk Jupyter-notebook](media/quickstarts/hello-results-save-icon.png)

1. Selecteer de menu opdracht **bestand** > **sluiten en stoppen** om de server te stoppen en het browser venster te sluiten.

## <a name="share-the-notebook"></a>Delen van de notebook

Als u uw notitie blok wilt delen, gaat u terug naar de pagina project, klikt u met de rechter muisknop op het notitie blok, selecteert u **koppeling kopiëren** (sneltoets: y) en plakt u die koppeling in een geschikt bericht (e-mail adres, im, enzovoort).

Op de pagina project kunt u ook het menu **delen** gebruiken om een koppeling te verkrijgen, een e-mail bericht met de koppeling te maken of HTML-code op te nemen en in te sluiten:

![Opdracht voor project delen](media/quickstarts/share-project-command.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelf studie: een Jupyter-notitie blok maken om een lineaire regressie uit te voeren](tutorial-create-run-jupyter-notebook.md)
