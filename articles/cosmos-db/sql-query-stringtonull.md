---
title: StringToNull in Azure Cosmos DB-query taal
description: Meer informatie over de SQL-functie StringToNull in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: c7ea3ca82cadb1351d8581f3d652ce060d67969b
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349241"
---
# <a name="stringtonull-azure-cosmos-db"></a>StringToNull (Azure Cosmos DB)
 Retourneert een expressie die is vertaald naar null. Als expressie niet kan worden vertaald, retourneert ongedefinieerd.  
  
## <a name="syntax"></a>Syntaxis
  
```sql
StringToNull(<str_expr>)  
```  
  
## <a name="arguments"></a>Argumenten
  
*str_expr*  
   Is een teken reeks expressie die als een null-expressie moet worden geparseerd.
  
## <a name="return-types"></a>Retour typen
  
  Retourneert een null-expressie of een niet-gedefinieerde waarde.  
  
## <a name="examples"></a>Voorbeelden
  
  In het volgende voor beeld ziet u hoe `StringToNull` zich gedraagt in verschillende typen. 

Hier volgen enkele voor beelden met geldige invoer.

 Witruimte is alleen toegestaan vóór of na null.

```sql
SELECT 
    StringToNull("null") AS n1, 
    StringToNull("  null ") AS n2,
    IS_NULL(StringToNull("null   ")) AS n3
```  
  
 Hier volgt de resultatenset.  
  
```json
[{"n1": null, "n2": null, "n3": true}]
```  

Hier volgen enkele voor beelden met ongeldige invoer.

Null is hoofdletter gevoelig en moet worden geschreven met alleen kleine letters, dat wil zeggen ' null '.

```sql
SELECT    
    StringToNull("NULL"),
    StringToNull("Null")
```  
  
 Hier volgt de resultatenset.  
  
```json
[{}]
```  

De door gegeven expressie wordt geparseerd als een null-expressie. deze invoer kan niet worden geëvalueerd om null te typen en daarom ongedefinieerd te retour neren.

```sql
SELECT    
    StringToNull("true"), 
    StringToNull(false), 
    StringToNull(undefined),
    StringToNull(NaN) 
```  
  
 Hier volgt de resultatenset.  
  
```json
[{}]
```  

## <a name="next-steps"></a>Volgende stappen

- [Teken reeks functies Azure Cosmos DB](sql-query-string-functions.md)
- [Systeem functies Azure Cosmos DB](sql-query-system-functions.md)
- [Inleiding tot Azure Cosmos DB](introduction.md)
