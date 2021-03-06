---
title: "PLINQ 및 TPL의 람다 식"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-standard
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Func delegate, creating with lambda expression
- Action delegate, creating with lambda expression
- lambda expressions, with Action and Func
ms.assetid: 645b2c17-29d0-4ffa-8684-430743cc2f2d
caps.latest.revision: "12"
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.openlocfilehash: 79ab0f4427e0f37259f88cd3ec0762d1582481f1
ms.sourcegitcommit: bd1ef61f4bb794b25383d3d72e71041a5ced172e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/18/2017
---
# <a name="lambda-expressions-in-plinq-and-tpl"></a>PLINQ 및 TPL의 람다 식
병렬 라이브러리 TPL (작업) 중 하나를 수행 하는 여러 메서드가 포함 된 <xref:System.Func%601?displayProperty=nameWithType> 또는 <xref:System.Action?displayProperty=nameWithType> 대리자 입력된 매개 변수로 제품군입니다. 이러한 대리자를 사용하여 병렬 루프, 작업 또는 쿼리에 사용자 지정 프로그램 논리를 전달합니다. TPL 및 PLINQ에 대한 코드 예제는 람다 식을 사용하여 인라인 코드 블록으로 해당 대리자의 인스턴스를 만듭니다. 이 항목에서는 Func 및 Action에 대한 간략한 소개를 제공하고 작업 병렬 라이브러리 및 PLINQ에서 람다 식을 사용하는 방법을 보여 줍니다.  
  
 **참고** 대리자에 대한 일반적인 자세한 내용은 [대리자](../../csharp/programming-guide/delegates/index.md) 및 [대리자](../../visual-basic/programming-guide/language-features/delegates/index.md)를 참조하세요. C# 및 [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)]의 람다 식에 대한 자세한 내용은 [람다 식](~/docs/csharp/programming-guide/statements-expressions-operators/lambda-expressions.md) 및 [람다 식](~/docs/visual-basic/programming-guide/language-features/procedures/lambda-expressions.md)을 참조하세요.  
  
## <a name="func-delegate"></a>Func 대리자  
 `Func` 대리자는 값을 반환하는 메서드를 캡슐화합니다. Func 서명에서 마지막 또는 가장 오른쪽에 있는 형식 매개 변수는 항상 반환 형식을 지정합니다. 컴파일러 오류의 한 가지 일반적인 원인은 두 입력 매개 변수를 전달 하려고 하는 것을 <xref:System.Func%602?displayProperty=nameWithType>; 실제로 입력 매개 변수 하나에이 형식을 사용 합니다. Framework 클래스 라이브러리의 17 버전을 정의 `Func`: <xref:System.Func%601?displayProperty=nameWithType>, <xref:System.Func%602?displayProperty=nameWithType>, <xref:System.Func%603?displayProperty=nameWithType>, 위쪽 등을 통해 <xref:System.Func%6017?displayProperty=nameWithType>합니다.  
  
## <a name="action-delegate"></a>Action 대리자  
 A <xref:System.Action?displayProperty=nameWithType> 대리자 메서드를 캡슐화 (의 경우 Sub [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)]) 하는 값을 반환 하지 않는 열리거나 [void](~/docs/csharp/language-reference/keywords/void.md)합니다. Action 형식 서명에서 형식 매개 변수는 입력 매개 변수만을 나타냅니다. Func처럼 Framework 클래스 라이브러리는 16개의 형식 매개 변수가 있는 버전을 통해 형식 매개 변수가 없는 버전에서 17개 버전의 Action을 정의합니다.  
  
## <a name="example"></a>예제  
 다음의 예는 <xref:System.Threading.Tasks.Parallel.ForEach%60%602%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%601%7D%2CSystem.Func%7B%60%600%2CSystem.Threading.Tasks.ParallelLoopState%2C%60%601%2C%60%601%7D%2CSystem.Action%7B%60%601%7D%29?displayProperty=nameWithType> 메서드 Func 및 Action 대리자 람다 식을 사용 하 여 표현 하는 방법을 보여 줍니다.  
  
 [!code-csharp[System.Threading.Tasks.Parallel#02](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.threading.tasks.parallel/cs/parallelforeach.cs#02)]
 [!code-vb[System.Threading.Tasks.Parallel#02](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.threading.tasks.parallel/vb/parallelforeach.vb#02)]  
  
## <a name="see-also"></a>참고 항목  
 [병렬 프로그래밍](../../../docs/standard/parallel-programming/index.md)
