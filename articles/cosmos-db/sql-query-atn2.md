---
title: ATN2 in Azure Cosmos DB-query taal
description: Meer informatie over hoe de ATN2-functie van SQL System in Azure Cosmos DB de principal-waarde van de boog tangens van y/x retourneert, uitgedrukt in radialen
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 696e14e75998ead04c99fab2b84fc4c742d5f54a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2020
ms.locfileid: "78302658"
---
# <a name="atn2-azure-cosmos-db"></a>ATN2 (Azure Cosmos DB)
 Retourneert de principal-waarde van de boog tangens van y/x, uitgedrukt in radialen.  
  
## <a name="syntax"></a>Syntaxis
  
```sql
ATN2(<numeric_expr>, <numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumenten
  
*numeric_expr*  
   Is een numerieke expressie.  
  
## <a name="return-types"></a>Retour typen
  
  Retourneert een numerieke expressie.  
  
## <a name="examples"></a>Voorbeelden
  
  In het volgende voor beeld wordt de ATN2 voor de opgegeven x-en y-onderdelen berekend.  
  
```sql
SELECT ATN2(35.175643, 129.44) AS atn2  
```  
  
 Dit is de resultatenset.  
  
```json
[{"atn2": 1.3054517947300646}]  
```  

## <a name="remarks"></a>Opmerkingen

Deze systeem functie maakt geen gebruik van de index.

## <a name="next-steps"></a>Volgende stappen

- [Wiskundige functies Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systeem functies Azure Cosmos DB](sql-query-system-functions.md)
- [Inleiding tot Azure Cosmos DB](introduction.md)
