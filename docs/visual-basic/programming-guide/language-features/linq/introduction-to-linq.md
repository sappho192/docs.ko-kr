---
title: "Visual Basic의 LINQ 소개"
ms.custom: 
ms.date: 07/20/2015
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology: devlang-visual-basic
ms.topic: article
helpviewer_keywords:
- queries [LINQ in Visual Basic], about LINQ in Visual Basic queries
- query execution [LINQ in Visual Basic]
- range variables [Visual Basic]
- LINQ [Visual Basic]
- LINQ [Visual Basic], about LINQ in Visual Basic
- query expressions [LINQ in Visual Basic]
- LINQ
- deferred execution
- iteration variables [Visual Basic]
ms.assetid: 3047d86e-0d49-40e2-928b-dc02e46c7984
caps.latest.revision: "28"
author: dotnet-bot
ms.author: dotnetcontent
ms.openlocfilehash: 0bb55aecc1faafd812da212565a7a858c714e933
ms.sourcegitcommit: 34ec7753acf76f90a0fa845235ef06663dc9e36e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/21/2017
---
# <a name="introduction-to-linq-in-visual-basic"></a>Visual Basic의 LINQ 소개
LINQ(Language-Integrated Query)는 쿼리 기능을 [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)]에 추가하고 모든 종류의 데이터로 작업할 때 간단하면서도 강력한 기능을 제공합니다. 처리될 쿼리를 데이터베이스에 보내거나 검색하고 있는 데이터의 형식마다 다른 쿼리 구문으로 작업하는 대신 LINQ에서는 [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)] 언어의 일부로 쿼리를 도입합니다. LINQ는 데이터의 형식에 관계없이 통합된 구문을 사용합니다.  
  
 LINQ 사용 하면 데이터를 쿼리 하는 SQL Server 데이터베이스, XML, 메모리 내 배열 및 컬렉션에서 [!INCLUDE[vstecado](~/includes/vstecado-md.md)] 데이터 집합 또는 다른 모든 원격 또는 로컬 데이터 원본 LINQ를 지 원하는 합니다. 이 모든 작업은 공통적인 [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)] 언어 요소를 사용하여 수행할 수 있습니다. 쿼리가 [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)] 언어로 작성되기 때문에 쿼리 결과는 강력한 형식의 개체로 반환됩니다. 이러한 개체는 런타임 대신 컴파일 시간에 쿼리에서 더 빠르게 코드를 작성하고 오류를 catch할 수 있도록 하는 IntelliSense를 지원합니다. LINQ 쿼리를 추가 쿼리의 소스로 사용하여 결과를 구체화할 수 있습니다. 또한 사용자가 쿼리 결과를 쉽게 보고 수정할 수 있도록 LINQ 쿼리를 컨트롤에 바인딩할 수 있습니다.  
  
 예를 들어 다음 코드 예제에서는 컬렉션에서 고객의 목록을 반환하고 고객의 위치를 기준으로 고객을 그룹화하는 LINQ 쿼리를 보여 줍니다.  
  
 [!code-vb[VbVbalrIntroToLINQ#1](../../../../visual-basic/programming-guide/language-features/linq/codesnippet/VisualBasic/introduction-to-linq_1.vb)]  
  
 이 항목에서는 다음 영역에 대한 정보를 제공합니다.  
  
-   [예제 실행](#RunningtheExamples)  
  
-   [LINQ 공급자](#LINQProviders)  
  
-   [LINQ 쿼리의 구조](#StructureOfALINQQuery)  
  
-   [Visual Basic LINQ 쿼리 연산자](#VisualBasicLINQQueryOperators)  
  
-   [LINQ to SQL 사용 하 여 데이터베이스에 연결](#ConnectingToADatabase)  
  
-   [LINQ를 지원하는 Visual Basic 기능](#VisualBasicFeaturesThatSupportLINQ)  
  
-   [지연 된 실행과 즉시 쿼리 실행](#QueryExecution)  
  
-   [Visual Basic의 XML](#XMLInVisualBasic)  
  
-   [관련된 리소스](#RelatedResources)  
  
-   [하는 방법 및 연습 항목](#HowToAndWalkthroughTopics)  
  
##  <a name="RunningtheExamples"></a>예제 실행  
 소개 및 "LINQ 쿼리의 구조" 섹션의 예제를 실행하려면 고객과 주문의 목록을 반환하는 다음 코드를 포함합니다.  
  
 [!code-vb[VbVbalrIntroToLINQ#31](../../../../visual-basic/programming-guide/language-features/linq/codesnippet/VisualBasic/introduction-to-linq_2.vb)]  
  
##  <a name="LINQProviders"></a>LINQ 공급자  
 A *LINQ 공급자* 매핑합니다 프로그램 [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)] 쿼리 중인 데이터 소스에 대 한 LINQ 쿼리 합니다. LINQ 쿼리를 작성하는 경우 공급자는 해당 쿼리를 가져와서 데이터 소스가 실행할 수 있을 명령으로 변환합니다. 또한 공급자는 소스의 데이터를 쿼리 결과를 구성하는 개체로 변환합니다. 마지막으로 공급자는 데이터 소스에 업데이트를 보낼 때 개체를 데이터로 변환합니다.  
  
 [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)]에는 다음 LINQ 공급자가 포함되어 있습니다.  
  
|공급자|설명|  
|---|---|  
|LINQ to Objects|LINQ to Objects 공급자를 사용하면 메모리 내 컬렉션 및 배열을 쿼리할 수 있습니다. 개체가 <xref:System.Collections.IEnumerable> 또는 <xref:System.Collections.Generic.IEnumerable%601> 인터페이스를 지원하는 경우 LINQ to Objects 공급자를 사용하면 개체를 쿼리할 수 있습니다.<br /><br /> 모든 [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)] 프로젝트의 경우 기본적으로 가져오는 <xref:System.Linq> 네임스페이스를 가져와서 LINQ to Objects 공급자를 사용하도록 설정할 수 있습니다.<br /><br /> LINQ to Objects 공급자에 대 한 자세한 내용은 참조 [LINQ to Objects](http://msdn.microsoft.com/library/73cafe73-37cf-46e7-bfa7-97c7eea7ced9)합니다.|  
|LINQ to SQL|LINQ to SQL 공급자를 사용하면 SQL Server 데이터베이스의 데이터를 쿼리하고 수정할 수 있습니다. 이렇게 하면 응용 프로그램의 개체 모델을 데이터베이스의 테이블 및 개체에 쉽게 매핑할 수 있습니다.<br /><br /> [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)]에는 O/R 디자이너(개체 관계형 디자이너)가 포함되어 있으므로 LINQ to SQL로 더 쉽게 작업할 수 있습니다. 이 디자이너는 데이터베이스의 개체에 매핑되는 응용 프로그램의 개체 모델을 만드는 데 사용됩니다. 또한 저장된 프로시저를 매핑하는 기능을 제공 및 함수를 O/R 디자이너는 <xref:System.Data.Linq.DataContext> 데이터베이스와의 통신을 관리 하 고 낙관적 동시성 검사에 대 한 상태를 저장 하는 개체입니다.<br /><br /> LINQ to SQL 공급자에 대 한 자세한 내용은 참조 [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)합니다. 개체 관계형 디자이너에 대 한 자세한 내용은 참조 [LINQ to SQL 도구 Visual Studio에서](/visualstudio/data-tools/linq-to-sql-tools-in-visual-studio2)합니다.|  
|LINQ to XML|LINQ to XML 공급자를 사용하면 XML을 쿼리하고 수정할 수 있습니다. 메모리 내 XML을 수정하거나 XML을 파일에서 로드하고 파일에 저장할 수 있습니다.<br /><br /> 또한 LINQ to XML 공급자는 [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)] 코드에서 직접 XML을 작성하는 데 사용할 수 있는 XML 리터럴 및 XML 축 속성을 사용하도록 설정합니다. 자세한 내용은 참조 [XML](../../../../visual-basic/programming-guide/language-features/xml/index.md)합니다.|  
|LINQ to DataSet|LINQ to DataSet 공급자에서 데이터를 쿼리 및 업데이트를 수는 [!INCLUDE[vstecado](~/includes/vstecado-md.md)] 데이터 집합입니다. 데이터 집합의 데이터를 쿼리, 집계 및 업데이트하는 기능을 단순화하고 확장하기 위해 데이터 집합을 사용하는 응용 프로그램에 LINQ의 기능을 추가할 수 있습니다.<br /><br /> 자세한 내용은 [LINQ to DataSet](../../../../framework/data/adonet/linq-to-dataset.md)를 참조하세요.|  
  
##  <a name="StructureOfALINQQuery"></a>LINQ 쿼리의 구조  
 LINQ 쿼리를이 라 불리는 *쿼리 식*, 데이터 원본 및 쿼리에 대 한 반복 변수를 식별 하는 쿼리 절의 조합으로 구성 합니다. 쿼리 식에는 소스 데이터에 적용할 정렬, 필터링, 그룹화 및 조인 또는 계산을 위한 명령도 포함될 수 있습니다. 쿼리 식 구문은 SQL의 구문과 유사하므로 구문의 상당 부분이 익숙하게 느껴질 수 있습니다.  
  
 쿼리 식은 `From` 절로 시작됩니다. 이 절은 쿼리의 소스 데이터와 소스 데이터의 각 요소를 개별적으로 참조하는 데 사용되는 변수를 식별합니다. 이러한 변수 *범위 변수* 또는 *반복 변수*합니다. `From` 절은 `From` 절이 선택 사항인 `Aggregate` 쿼리를 제외한 쿼리에 필수적입니다. 쿼리의 범위와 소스가 `From` 또는 `Aggregate` 절에서 식별된 후 쿼리 절의 조합을 포함하여 쿼리를 구체화할 수 있습니다. 쿼리 절에 대한 자세한 내용은 이 항목의 뒷부분에 있는 [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)] LINQ 쿼리 연산자를 참조하세요. 예를 들어 다음 쿼리는 고객 데이터의 소스 컬렉션을 `customers` 변수로 식별하고 `cust`라는 반복 변수를 식별합니다.  
  
 [!code-vb[VbVbalrIntroToLINQ#2](../../../../visual-basic/programming-guide/language-features/linq/codesnippet/VisualBasic/introduction-to-linq_3.vb)]  
  
 이 예제는 그 자체로 유효한 쿼리이지만 쿼리는 쿼리 절을 더 추가하여 결과를 구체화할 때 훨씬 더 강력해집니다. 예를 들어 `Where` 절을 추가하여 하나 이상의 값을 기준으로 결과를 필터링할 수 있습니다. 쿼리 식은 코드 한 줄입니다. 추가 쿼리 절을 쿼리의 끝에 추가하기만 하면 됩니다. 밑줄(_) 줄 연속 문자를 사용하여 여러 줄의 텍스트로 쿼리를 분할하는 방법으로 가독성을 높일 수 있습니다. 다음 코드 예제에서는 `Where` 절이 포함된 쿼리의 예를 보여 줍니다.  
  
 [!code-vb[VbVbalrIntroToLINQ#3](../../../../visual-basic/programming-guide/language-features/linq/codesnippet/VisualBasic/introduction-to-linq_4.vb)]  
  
 다른 강력한 쿼리 절은 데이터 소스에서 선택된 필드만 반환하는 데 사용할 수 있는 `Select` 절입니다. LINQ 쿼리는 강력한 형식의 개체로 구성된 열거 가능한 컬렉션을 반환합니다. 쿼리는 무명 형식 또는 명명된 형식의 컬렉션을 반환할 수 있습니다. `Select` 절을 사용하여 데이터 소스에서 단일 필드만 반환할 수 있습니다. 이렇게 하는 경우 반환된 컬렉션의 형식은 단일 필드의 형식입니다. 또한 `Select` 절을 사용하여 데이터 소스에서 여러 필드를 반환할 수도 있습니다. 이렇게 하는 경우 반환된 컬렉션의 형식은 새로운 무명 형식입니다. 또한 쿼리가 반환하는 필드를 지정된 명명된 형식의 필드와 일치시킬 수도 있습니다. 다음 코드 예제에서는 데이터 소스의 선택된 필드에서 제공된 데이터로 채워진 멤버가 있는 무명 형식의 컬렉션을 반환하는 쿼리 식을 보여 줍니다.  
  
 [!code-vb[VbVbalrIntroToLINQ#4](../../../../visual-basic/programming-guide/language-features/linq/codesnippet/VisualBasic/introduction-to-linq_5.vb)]  
  
 LINQ 쿼리를 사용하여 여러 데이터 소스를 결합하고 단일 결과를 반환할 수도 있습니다. 이 작업은 하나 이상의 `From` 절을 사용하거나 `Join` 또는 `Group Join` 쿼리 절을 사용하여 수행할 수 있습니다. 다음 코드 예제에서는 고객 및 주문 데이터를 결합하고 고객 및 주문 데이터가 포함된 무명 형식의 컬렉션을 반환하는 쿼리 식을 보여 줍니다.  
  
 [!code-vb[VbVbalrIntroToLINQ#5](../../../../visual-basic/programming-guide/language-features/linq/codesnippet/VisualBasic/introduction-to-linq_6.vb)]  
  
 `Group Join` 절을 사용하여 고객 개체의 컬렉션이 포함된 계층적 쿼리 결과를 만들 수 있습니다. 각 고객 개체에는 해당 고객에 대한 모든 주문의 컬렉션이 포함된 속성이 있습니다. 다음 코드 예제에서는 계층적 결과로 고객 및 주문 데이터를 결합하고 무명 형식의 컬렉션을 반환하는 쿼리 식을 보여 줍니다. 쿼리는 고객에 대한 주문 데이터의 컬렉션을 포함하는 `CustomerOrders` 속성이 포함된 형식을 반환합니다. 또한 해당 고객에 대한 모든 주문의 총계 합이 포함된 `OrderTotal` 속성도 포함합니다. 이 쿼리는 LEFT OUTER JOIN과 동일합니다.  
  
 [!code-vb[VbVbalrIntroToLINQ#6](../../../../visual-basic/programming-guide/language-features/linq/codesnippet/VisualBasic/introduction-to-linq_7.vb)]  
  
 강력한 쿼리 식을 만드는 데 사용할 수 있는 몇 가지 추가 LINQ 쿼리 연산자가 있습니다. 이 항목의 다음 섹션에서는 쿼리 식에 포함할 수 있는 다양한 쿼리 절에 대해 설명합니다. 에 대 한 내용은 [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)] 쿼리 절, 참조 [쿼리](../../../../visual-basic/language-reference/queries/queries.md)합니다.  
  
##  <a name="VisualBasicLINQQueryOperators"></a>Visual Basic LINQ 쿼리 연산자  
 <xref:System.Linq> 네임스페이스와 LINQ 쿼리를 지원하는 다른 네임스페이스의 클래스에는 응용 프로그램의 요구 사항에 따라 쿼리를 만들고 구체화하기 위해 호출할 수 있는 메서드가 포함되어 있습니다. [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)]에는 다음 표에 설명된 대로 가장 일반적인 쿼리 절의 키워드가 포함되어 있습니다.  
  
|용어|정의|  
|---|---|  
|[From 절](../../../../visual-basic/language-reference/queries/from-clause.md)|`From` 절이나 `Aggregate` 절은 쿼리를 시작하는 데 필요합니다. `From` 절은 쿼리의 소스 컬렉션과 반복 변수를 지정합니다. 예:<br /><br /> [!code-vb[VbVbalrIntroToLINQ#7](../../../../visual-basic/programming-guide/language-features/linq/codesnippet/VisualBasic/introduction-to-linq_8.vb)]|  
|[Select 절](../../../../visual-basic/language-reference/queries/select-clause.md)|선택 사항입니다. 쿼리의 반복 변수 집합을 선언합니다. 예:<br /><br /> [!code-vb[VbVbalrIntroToLINQ#8](../../../../visual-basic/programming-guide/language-features/linq/codesnippet/VisualBasic/introduction-to-linq_9.vb)]<br /><br /> `Select` 절이 지정되지 않은 경우 쿼리의 반복 변수는 `From` 또는 `Aggregate` 절에서 지정된 반복 변수로 구성됩니다.|  
|[Where 절](../../../../visual-basic/language-reference/queries/where-clause.md)|선택 사항입니다. 쿼리의 필터링 조건을 지정합니다. 예:<br /><br /> [!code-vb[VbVbalrIntroToLINQ#9](../../../../visual-basic/programming-guide/language-features/linq/codesnippet/VisualBasic/introduction-to-linq_10.vb)]|  
|[Order By 절](../../../../visual-basic/language-reference/queries/order-by-clause.md)|선택 사항입니다. 쿼리의 열에 대한 정렬 순서를 지정합니다. 예:<br /><br /> [!code-vb[VbVbalrIntroToLINQ#10](../../../../visual-basic/programming-guide/language-features/linq/codesnippet/VisualBasic/introduction-to-linq_11.vb)]|  
|[Join 절](../../../../visual-basic/language-reference/queries/join-clause.md)|선택 사항입니다. 두 컬렉션을 단일 컬렉션으로 결합합니다. 예:<br /><br /> [!code-vb[VbVbalrIntroToLINQ#11](../../../../visual-basic/programming-guide/language-features/linq/codesnippet/VisualBasic/introduction-to-linq_12.vb)]|  
|[Group By 절](../../../../visual-basic/language-reference/queries/group-by-clause.md)|선택 사항입니다. 쿼리 결과의 요소를 그룹화합니다. 각 그룹에 집계 함수를 적용하는 데 사용할 수 있습니다. 예:<br /><br /> [!code-vb[VbVbalrIntroToLINQ#12](../../../../visual-basic/programming-guide/language-features/linq/codesnippet/VisualBasic/introduction-to-linq_13.vb)]|  
|[Group Join 절](../../../../visual-basic/language-reference/queries/group-join-clause.md)|선택 사항입니다. 두 컬렉션을 단일 계층 구조 컬렉션으로 결합합니다. 예:<br /><br /> [!code-vb[VbVbalrIntroToLINQ#13](../../../../visual-basic/programming-guide/language-features/linq/codesnippet/VisualBasic/introduction-to-linq_14.vb)]|  
|[Aggregate 절](../../../../visual-basic/language-reference/queries/aggregate-clause.md)|`From` 절이나 `Aggregate` 절은 쿼리를 시작하는 데 필요합니다. `Aggregate` 절은 하나 이상의 집계 함수를 컬렉션에 적용합니다. 예를 들어 `Aggregate` 절을 사용하여 쿼리가 반환하는 모든 요소의 합계를 계산할 수 있습니다.<br /><br /> [!code-vb[VbVbalrIntroToLINQ#14](../../../../visual-basic/programming-guide/language-features/linq/codesnippet/VisualBasic/introduction-to-linq_15.vb)]<br /><br /> 또한 `Aggregate` 절을 사용하여 쿼리를 수정할 수 있습니다. 예를 들어 `Aggregate` 절을 사용하여 관련된 쿼리 컬렉션에 대한 계산을 수행할 수 있습니다.<br /><br /> [!code-vb[VbVbalrIntroToLINQ#15](../../../../visual-basic/programming-guide/language-features/linq/codesnippet/VisualBasic/introduction-to-linq_16.vb)]|  
|[Let 절](../../../../visual-basic/language-reference/queries/let-clause.md)|선택 사항입니다. 값을 계산하고 쿼리의 새 변수에 할당합니다. 예:<br /><br /> [!code-vb[VbVbalrIntroToLINQ#16](../../../../visual-basic/programming-guide/language-features/linq/codesnippet/VisualBasic/introduction-to-linq_17.vb)]|  
|[Distinct 절](../../../../visual-basic/language-reference/queries/distinct-clause.md)|선택 사항입니다. 현재 반복 변수의 값을 제한하여 쿼리 결과에서 중복 값을 제거합니다. 예:<br /><br /> [!code-vb[VbVbalrIntroToLINQ#17](../../../../visual-basic/programming-guide/language-features/linq/codesnippet/VisualBasic/introduction-to-linq_18.vb)]|  
|[Skip 절](../../../../visual-basic/language-reference/queries/skip-clause.md)|선택 사항입니다. 컬렉션에서 지정된 수의 요소를 무시하고 나머지 요소를 반환합니다. 예:<br /><br /> [!code-vb[VbVbalrIntroToLINQ#18](../../../../visual-basic/programming-guide/language-features/linq/codesnippet/VisualBasic/introduction-to-linq_19.vb)]|  
|[Skip While 절](../../../../visual-basic/language-reference/queries/skip-while-clause.md)|선택 사항입니다. 지정된 조건이 `true`이면 컬렉션에 있는 요소를 무시하고 나머지 요소를 반환합니다. 예:<br /><br /> [!code-vb[VbVbalrIntroToLINQ#19](../../../../visual-basic/programming-guide/language-features/linq/codesnippet/VisualBasic/introduction-to-linq_20.vb)]|  
|[Take 절](../../../../visual-basic/language-reference/queries/take-clause.md)|선택 사항입니다. 컬렉션의 시작 위치에서 지정된 수의 연속 요소를 반환합니다. 예:<br /><br /> [!code-vb[VbVbalrIntroToLINQ#20](../../../../visual-basic/programming-guide/language-features/linq/codesnippet/VisualBasic/introduction-to-linq_21.vb)]|  
|[Take While 절](../../../../visual-basic/language-reference/queries/take-while-clause.md)|선택 사항입니다. 지정된 조건이 `true`이면 컬렉션의 요소를 포함하고 나머지 요소를 무시합니다. 예:<br /><br /> [!code-vb[VbVbalrIntroToLINQ#21](../../../../visual-basic/programming-guide/language-features/linq/codesnippet/VisualBasic/introduction-to-linq_22.vb)]|  
  
 에 대 한 내용은 [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)] 쿼리 절, 참조 [쿼리](../../../../visual-basic/language-reference/queries/queries.md)합니다.  
  
 LINQ에서 제공하는 열거 가능 형식과 쿼리 가능 형식의 멤버를 호출하여 추가 LINQ 쿼리 기능을 사용할 수 있습니다. 쿼리 식의 결과에 대해 특정 쿼리 연산자를 호출하여 이러한 추가 기능을 사용할 수 있습니다. 예를 들어 다음 코드 예제에서는 <xref:System.Linq.Enumerable.Union%2A> 메서드를 사용하여 두 쿼리의 결과를 하나의 쿼리 결과로 결합합니다. 이 예제에서는 <xref:System.Linq.Enumerable.ToList%2A> 메서드를 사용하여 쿼리 결과를 제네릭 목록으로 반환합니다.  
  
 [!code-vb[VbVbalrIntroToLINQ#22](../../../../visual-basic/programming-guide/language-features/linq/codesnippet/VisualBasic/introduction-to-linq_23.vb)]  
  
 추가 LINQ 기능에 대 한 세부 정보를 참조 하십시오. [표준 쿼리 연산자 개요](http://msdn.microsoft.com/library/24cda21e-8af8-4632-b519-c404a839b9b2)합니다.  
  
##  <a name="ConnectingToADatabase"></a>LINQ to SQL 사용 하 여 데이터베이스에 연결  
 [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)]에서는 LINQ to SQL 파일을 사용하여 액세스하려는 테이블, 뷰 및 저장 프로시저와 같은 SQL Server 데이터베이스 개체를 식별합니다. LINQ to SQL 파일의 확장명은 .dbml입니다.  
  
 SQL Server 데이터베이스에 올바르게 연결 해야 하는 경우 추가할 수 있습니다는 **LINQ to SQL 클래스** 항목 템플릿을 프로젝트에 있습니다. 이렇게 하면 O/R 디자이너(개체 관계형 디자이너)가 표시됩니다. O/R 디자이너를 사용 하면 코드에서 액세스 하려는 항목을 끌어는 **서버 탐색기**/**데이터베이스 탐색기** 디자이너 화면으로 합니다. LINQ to SQL 파일은 <xref:System.Data.Linq.DataContext> 개체를 프로젝트에 추가합니다. 이 개체에는 액세스하려는 테이블과 뷰의 속성 및 컬렉션과 호출하려는 저장 프로시저의 메서드가 포함되어 있습니다. 변경 내용을 LINQ to SQL(.dbml) 파일에 저장한 후 O/R 디자이너에서 정의된 <xref:System.Data.Linq.DataContext> 개체를 참조하여 코드에서 이러한 개체에 액세스할 수 있습니다. 프로젝트의 <xref:System.Data.Linq.DataContext> 개체는 LINQ to SQL 파일의 이름에 따라 명명됩니다. 예를 들어 Northwind.dbml이라는 LINQ to SQL 파일은 `NorthwindDataContext`라는 <xref:System.Data.Linq.DataContext> 개체를 만듭니다.  
  
 단계별 지침이 포함 된 예제를 보려면 [하는 방법: 데이터베이스 쿼리](../../../../visual-basic/programming-guide/language-features/linq/how-to-query-a-database-by-using-linq.md) 및 [하는 방법: 저장 프로시저 호출](../../../../visual-basic/programming-guide/language-features/linq/how-to-call-a-stored-procedure-by-using-linq.md)합니다.  
  
##  <a name="VisualBasicFeaturesThatSupportLINQ"></a>Visual Basic LINQ를 지 원하는 기능  
 [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)]에는 LINQ의 사용을 간단하게 만들고 LINQ 쿼리를 수행하기 위해 작성해야 하는 코드의 양을 줄이는 뛰어난 기타 기능이 포함되어 있습니다. 이러한 요구 사항은 다음과 같습니다.  
  
-   **익명 형식**, 쿼리 결과에 따라 새 형식을 만들 수 있도록 합니다.  
  
-   **암시적으로 형식화 된 변수**, 형식 지정을 지연 수 있도록 및 컴파일러를 통해 쿼리 결과에 따라 형식을 유추 합니다.  
  
-   **확장 메서드**, 형식 자체를 수정 하지 않고 고유한 방법으로 기존 형식을 확장할 수 있도록 합니다.  
  
 자세한 내용은 참조 [LINQ Visual Basic 기능을 지원](../../../../visual-basic/programming-guide/concepts/linq/features-that-support-linq.md)합니다.  
  
##  <a name="QueryExecution"></a>지연 된 실행과 즉시 쿼리 실행  
 쿼리 실행은 쿼리를 만드는 것과 구분됩니다. 쿼리를 만든 후 쿼리 실행은 별도의 메커니즘으로 트리거됩니다. 정의 된 대로 쿼리를 실행할 수 있습니다 (*즉시 실행*), 또는 정의 저장할 수 있습니다 및 쿼리는 나중에 실행할 수 있습니다 (*지연 된 실행*).  
  
 기본적으로 쿼리를 만들 때 쿼리 자체는 즉시 실행되지 않습니다. 대신 쿼리 정의가 쿼리 결과를 참조하는 데 사용되는 변수에 저장됩니다. 쿼리 결과 변수가 코드(예: `For…Next` 루프)에서 나중에 액세스될 때 쿼리가 실행됩니다. 이 프로세스 라고 *지연 된 실행*합니다.  
  
 정의 될 때,이 라고 하는 쿼리를 실행할 수도 있습니다 *즉시 실행*합니다. 쿼리 결과의 개별 요소에 액세스해야 하는 메서드를 적용하여 즉시 실행을 트리거할 수 있습니다. 이는 `Count`, `Sum`, `Average`, `Min` 또는 `Max`와 같은 집계 함수를 포함한 결과일 수 있습니다. 집계 함수에 대 한 자세한 내용은 참조 [Aggregate 절](../../../../visual-basic/language-reference/queries/aggregate-clause.md)합니다.  
  
 `ToList` 또는 `ToArray` 메서드를 사용하는 경우에도 즉시 실행이 시작됩니다. 이는 즉시 쿼리를 실행하고 결과를 캐시하려는 경우에 유용할 수 있습니다. 이러한 메서드에 대 한 자세한 내용은 참조 [데이터 형식 변환](../../../../visual-basic/programming-guide/concepts/linq/converting-data-types.md)합니다.  
  
 쿼리 실행에 대 한 자세한 내용은 참조 [쓰기 Your LINQ 쿼리 처음](../../../../visual-basic/programming-guide/concepts/linq/writing-your-first-linq-query.md)합니다.  
  
##  <a name="XMLInVisualBasic"></a>Visual Basic의 XML  
 [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)]의 XML 기능에는 코드에서 XML을 쉽게 만들고 액세스하고 쿼리하고 수정하는 데 사용할 수 있는 XML 리터럴과 XML 축 속성이 포함되어 있습니다. XML 리터럴을 사용하면 코드에서 직접 XML을 작성할 수 있습니다. Visual Basic 컴파일러는 XML을 첫 번째 클래스 데이터 개체로 처리합니다.  
  
 다음 코드 예제에서는 XML 요소를 만들고 하위 요소와 특성에 액세스한 다음 LINQ를 사용하여 요소의 콘텐츠를 쿼리하는 방법을 보여 줍니다.  
  
 [!code-vb[VbXmlSamples#8](../../../../visual-basic/language-reference/operators/codesnippet/VisualBasic/introduction-to-linq_24.vb)]  
  
 자세한 내용은 참조 [XML](../../../../visual-basic/programming-guide/language-features/xml/index.md)합니다.  
  
##  <a name="RelatedResources"></a>관련된 리소스  
  
|항목|설명|  
|---|---|  
|[XML](../../../../visual-basic/programming-guide/language-features/xml/index.md)|쿼리될 수 있으며 [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)] 코드에서 XML을 첫 번째 클래스 데이터 개체로 포함할 수 있도록 하는 [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)]의 XML 기능에 대해 설명합니다.|  
|[쿼리](../../../../visual-basic/language-reference/queries/queries.md)|[!INCLUDE[vbprvb](~/includes/vbprvb-md.md)]에서 사용할 수 있는 쿼리 절에 대한 참조 정보를 제공합니다.|  
|[LINQ(Language-Integrated Query)](http://msdn.microsoft.com/library/a73c4aec-5d15-4e98-b962-1274021ea93d)|LINQ에 대한 일반 정보, 프로그래밍 지침 및 샘플을 포함하고 있습니다.|  
|[LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)|LINQ to SQL에 대한 일반 정보, 프로그래밍 지침 및 샘플을 포함하고 있습니다.|  
|[LINQ to Objects](http://msdn.microsoft.com/library/73cafe73-37cf-46e7-bfa7-97c7eea7ced9)|LINQ to Objects에 대한 일반 정보, 프로그래밍 지침 및 샘플을 포함하고 있습니다.|  
|[LINQ to ADO.NET(포털 페이지)](http://msdn.microsoft.com/library/dd7d3c6a-ff98-47e9-a1a7-2d4cfc42d150)|LINQ to [!INCLUDE[vstecado](~/includes/vstecado-md.md)]에 대한 일반 정보, 프로그래밍 지침 및 샘플의 링크를 포함하고 있습니다.|  
|[LINQ to XML](http://msdn.microsoft.com/library/f0fe21e9-ee43-4a55-b91a-0800e5782c13)|LINQ to XML에 대한 일반 정보, 프로그래밍 지침 및 샘플을 포함하고 있습니다.|  
  
##  <a name="HowToAndWalkthroughTopics"></a>하는 방법 및 연습 항목  
 [방법: 데이터베이스 쿼리](how-to-query-a-database-by-using-linq.md)  
  
 [방법: 저장 프로시저 호출](how-to-call-a-stored-procedure-by-using-linq.md)  
  
 [방법: 데이터베이스의 데이터 수정](how-to-modify-data-in-a-database-by-using-linq.md)  
  
 [방법: 조인을 사용하여 데이터 결합](how-to-combine-data-with-linq-by-using-joins.md)  
  
 [방법: 쿼리 결과 정렬](how-to-sort-query-results-by-using-linq.md)  
  
 [방법: 쿼리 결과 필터링](how-to-filter-query-results-by-using-linq.md)  
  
 [방법: 데이터 개수, 합 또는 평균 계산](how-to-count-sum-or-average-data-by-using-linq.md)  
  
 [방법: 쿼리 결과의 최소값 또는 최대값 찾기](how-to-find-the-minimum-or-maximum-value-in-a-query-result.md)  
  
 [방법: 저장된 프로시저를 할당 업데이트, 삽입 및 삭제 (O/R 디자이너)를 수행 합니다.](http://msdn.microsoft.com/library/e88224ab-ff61-4a3a-b6b8-6f3694546cac)  
  
## <a name="featured-book-chapters"></a>중요 설명서 장  
 [17 장: LINQ](http://go.microsoft.com/fwlink/?LinkId=195277) 에 [Visual Basic 2008 프로그래밍](http://go.microsoft.com/fwlink/?LinkId=195383)  
  
## <a name="see-also"></a>참고 항목  
 [LINQ(Language-Integrated Query)](http://msdn.microsoft.com/library/a73c4aec-5d15-4e98-b962-1274021ea93d)  
 [Visual Basic의 LINQ to XML 개요](../../../../visual-basic/programming-guide/language-features/xml/overview-of-linq-to-xml.md)  
 [LINQ to DataSet 개요](../../../../framework/data/adonet/linq-to-dataset-overview.md)  
 [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)  
 [LINQ to SQL Tools in Visual Studio](/visualstudio/data-tools/linq-to-sql-tools-in-visual-studio2)(Visual Studio의 LINQ to SQL 도구)  
 [DataContext 메서드 (O/R 디자이너)](/visualstudio/data-tools/datacontext-methods-o-r-designer)
