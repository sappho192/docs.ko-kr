---
title: "LINQ 및 제네릭 형식(C#) | Microsoft 문서"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
helpviewer_keywords:
- LINQ [C#], generic types
- generic types [LINQ]
- generics [LINQ]
ms.assetid: 660e3799-25ca-462c-8c4a-8bce04fbb031
caps.latest.revision: 18
author: BillWagner
ms.author: wiwagn
translation.priority.ht:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- ru-ru
- zh-cn
- zh-tw
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 1951d53b069104f3439aa2fe3ee3975bae0e1659
ms.lasthandoff: 03/13/2017

---
# <a name="linq-and-generic-types-c"></a>LINQ 및 제네릭 형식(C#)
[!INCLUDE[vbteclinq](../../../../csharp/includes/vbteclinq_md.md)] 쿼리는 [!INCLUDE[dnprdnshort](../../../../csharp/getting-started/includes/dnprdnshort_md.md)] 버전 2.0에서 도입된 제네릭 형식을 기반으로 합니다. 제네릭에 대한 세부 지식이 없어도 쿼리 작성을 시작할 수 있습니다. 그러나 다음 두 가지 기본 개념은 이해하는 것이 좋습니다.  
  
1.  <xref:System.Collections.Generic.List%601> 같은 제네릭 컬렉션 클래스의 인스턴스를 만들 때 "T"를 목록에 포함할 개체 형식으로 대체합니다. 예를 들어 문자열 목록은 `List<string>`으로 표현되고, `Customer` 개체 목록은 `List<Customer>`로 표현됩니다. 제네릭 목록은 강력한 형식이어야 하며 해당 요소를 <xref:System.Object>로 저장하는 컬렉션에 비해 많은 장점을 제공합니다. `List<string>`에 `Customer`를 추가하려고 하면 컴파일 시간에 오류가 발생합니다. 런타임 형식 캐스팅을 수행할 필요가 없기 때문에 제네릭 컬렉션을 사용하기가 쉽습니다.  
  
2.  <xref:System.Collections.Generic.IEnumerable%601>는 `foreach` 문을 사용하여 제네릭 컬렉션 클래스를 열거할 수 있는 인터페이스입니다. 제네릭 컬렉션 클래스는 <xref:System.Collections.ArrayList>와 같은 제네릭이 아닌 컬렉션 클래스에서 <xref:System.Collections.IEnumerable>을 지원하는 것처럼 <xref:System.Collections.Generic.IEnumerable%601>를 지원합니다.  
  
 제네릭에 대한 자세한 내용은 [제네릭](../../../../csharp/programming-guide/generics/index.md)을 참조하세요.  
  
## <a name="ienumerablet-variables-in-linq-queries"></a>LINQ 쿼리의 IEnumerable<T\> 변수  
 [!INCLUDE[vbteclinq](../../../../csharp/includes/vbteclinq_md.md)] 쿼리 변수는 <xref:System.Collections.Generic.IEnumerable%601> 형식이거나 <xref:System.Linq.IQueryable%601>와 같은 파생 형식입니다. 형식이 `IEnumerable<Customer>`인 쿼리 변수가 표시되면 쿼리가 실행될 때 0개 이상의 `Customer` 개체 시퀀스를 생성한다는 의미입니다.  
  
 [!code-cs[csLINQGettingStarted#34](../../../../csharp/programming-guide/concepts/linq/codesnippet/CSharp/linq-and-generic-types_1.cs)]  
  
 자세한 내용은 [LINQ 쿼리 작업의 형식 관계](../../../../csharp/programming-guide/concepts/linq/type-relationships-in-linq-query-operations.md)를 참조하세요.  
  
## <a name="letting-the-compiler-handle-generic-type-declarations"></a>컴파일러에서 제네릭 형식 선언을 처리하도록 허용  
 원하는 경우 [var](../../../../csharp/language-reference/keywords/var.md) 키워드를 사용하여 제네릭 구문을 방지할 수 있습니다. `var` 키워드는 `from` 절에 지정된 데이터 소스를 확인하여 쿼리 변수의 형식을 유추하도록 컴파일러에 지시합니다. 다음 예제에서는 이전 예제와 동일하게 컴파일된 코드를 생성합니다.  
  
 [!code-cs[csLINQGettingStarted#35](../../../../csharp/programming-guide/concepts/linq/codesnippet/CSharp/linq-and-generic-types_2.cs)]  
  
 `var` 키워드는 변수의 형식이 명확하거나 그룹 쿼리에 의해 생성되는 형식과 같이 중첩된 제네릭 형식을 명시적으로 지정하는 것이 중요하지 않은 경우에 유용합니다. 일반적으로 `var`을 사용하는 경우 다른 사용자가 코드를 읽기가 더 어려워질 수 있습니다. 자세한 내용은 [암시적으로 형식화된 지역 변수](../../../../csharp/programming-guide/classes-and-structs/implicitly-typed-local-variables.md)를 참조하세요.  
  
## <a name="see-also"></a>참고 항목  
 [C#에서 LINQ 시작](../../../../csharp/programming-guide/concepts/linq/getting-started-with-linq.md)   
 [제네릭](../../../../csharp/programming-guide/generics/index.md)