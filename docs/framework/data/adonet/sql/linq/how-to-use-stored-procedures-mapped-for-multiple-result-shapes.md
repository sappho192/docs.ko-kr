---
title: "방법: 여러 결과 도형에 매핑된 저장 프로시저 사용"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-ado
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- csharp
- vb
ms.assetid: c2b84dfe-7fec-489a-92de-45215cec4518
caps.latest.revision: "2"
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.workload: dotnet
ms.openlocfilehash: fde4dd9044ff2bc6d781d7ceafec2bde3df7e14d
ms.sourcegitcommit: ed26cfef4e18f6d93ab822d8c29f902cff3519d1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/17/2018
---
# <a name="how-to-use-stored-procedures-mapped-for-multiple-result-shapes"></a>방법: 여러 결과 도형에 매핑된 저장 프로시저 사용
저장 프로시저에서 여러 결과 도형을 반환할 수 있는 경우 반환 형식은 단일 프로젝션 도형에 대해 강력하게 형식화될 수 없습니다. [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]에서는 모든 가능한 프로젝션 형식을 생성할 수 있더라도 해당 형식들이 반환되는 순서는 알 수는 없습니다.  
  
 이 시나리오와 달리 순차적으로 여러 결과 도형을 생성하는 저장 프로시저에 대한 자세한 내용은 참조 [하는 방법: 프로시저 순차적 결과 도형에 매핑된 저장 하 사용](../../../../../../docs/framework/data/adonet/sql/linq/how-to-use-stored-procedures-mapped-for-sequential-result-shapes.md)합니다.  
  
 <xref:System.Data.Linq.Mapping.ResultTypeAttribute> 특성을 여러 결과 형식을 반환하는 저장 프로시저에 적용하여 프로시저에서 반환할 수 있는 형식 집합을 지정합니다.  
  
## <a name="example"></a>예  
 다음 SQL 코드 예제에서 결과 도형은 입력(`shape =1` 또는 `shape = 2`)에 따라 다릅니다. 따라서 사용자는 처음 반환되는 프로젝션을 알 수 없습니다.  
  
```  
CREATE PROCEDURE VariableResultShapes(@shape int)  
AS  
if(@shape = 1)  
    select CustomerID, ContactTitle, CompanyName from customers  
else if(@shape = 2)  
    select OrderID, ShipName from orders  
```  
  
 [!code-csharp[DLinqSprox#4](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqSprox/cs/northwind-sprox.cs#4)]
 [!code-vb[DLinqSprox#4](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqSprox/vb/northwind-sprox.vb#4)]  
  
## <a name="example"></a>예  
 다음과 유사한 코드를 사용하여 이 저장 프로시저를 실행할 수 있습니다.  
  
> [!NOTE]
>  저장 프로시저에 대한 정보에 따라 <xref:System.Data.Linq.IMultipleResults.GetResult%2A> 패턴을 사용하여 올바른 형식의 열거자를 가져와야 합니다.  
  
 [!code-csharp[DLinqSprox#5](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqSprox/cs/Program.cs#5)]
 [!code-vb[DLinqSprox#5](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqSprox/vb/Module1.vb#5)]  
  
## <a name="see-also"></a>참고 항목  
 [저장 프로시저](../../../../../../docs/framework/data/adonet/sql/linq/stored-procedures.md)
