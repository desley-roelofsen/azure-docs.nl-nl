---
title: Pakketten in een Jupyter-notebook op Azure installeren
description: Het installeren van Python, R, en F# pakketten uit binnen een Jupyter-notebook op Azure.
ms.topic: article
ms.date: 12/04/2018
ms.openlocfilehash: 5d85c8e936ce7c8bf38ec7bc9c27d9066cc8b155
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2019
ms.locfileid: "74277536"
---
# <a name="install-packages-from-within-a-notebook"></a>Installeren van pakketten van binnen een laptop

Hoewel u de [omgeving voor uw notitie blok kunt configureren op project niveau](configure-manage-azure-notebooks-projects.md#configure-the-project-environment), wilt u mogelijk pakketten rechtstreeks in een afzonderlijk notitie blok installeren.

Pakketten geïnstalleerd vanuit het notitieblok gelden alleen voor de huidige sessie. Installatie van het pakket zijn niet persistent gemaakt nadat de server wordt afgesloten.

## <a name="python"></a>Python

Pakketten in Python kunnen worden geïnstalleerd met behulp van pip of met behulp van de opdrachten binnen code cellen conda:

```bash
!pip install <package_name>

!conda install <package_name> -y
```

Als uitvoer van de opdracht geeft aan dat al door de voorwaarde is voldaan, wordt Azure notitieblokken kan het pakket bevatten standaard. Het pakket kan ook worden geïnstalleerd via een [installatie stap van een project omgeving](configure-manage-azure-notebooks-projects.md#configure-the-project-environment).

## <a name="r"></a>R

Pakketten in R kunnen worden geïnstalleerd vanuit KRANen of GitHub met behulp van de functie `install.packages` in een code-cel:

```r
install.packages("package_name")
```

U kunt ook prerelease-versies en andere ontwikkel pakketten van GitHub installeren met behulp van de devtools-bibliotheek:

```r
options(unzip = 'internal')
library(devtools)
install_github('<user>/<repo>')
```

## <a name="f"></a>F#

Pakketten in F# kunnen worden geïnstalleerd vanuit [nuget.org](https://www.nuget.org) door het aanroepen van de Paket dependency manager in code cellen. Laad eerst de Paket manager:

```fsharp
#load "Paket.fsx"
```

Installeer vervolgens de pakketten:

```fsharp
Paket.Package
  [ "MathNet.Numerics"
    "MathNet.Numerics.FSharp"
  ]
```

Laad vervolgens de Paket-Generator:
```fsharp
#load "Paket.Generated.Refs.fsx"
```

Open de bibliotheek:
```fsharp
open MathNet.Numerics
```

## <a name="next-steps"></a>Volgende stappen

- [Procedure: projecten configureren en beheren](configure-manage-azure-notebooks-projects.md)
- [Procedure: een diavoorstelling presen teren](present-jupyter-notebooks-slideshow.md)
