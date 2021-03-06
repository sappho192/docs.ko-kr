---
title: "데이터 및 작업 병렬 처리에서 발생할 수 있는 문제"
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
helpviewer_keywords: parallel programming, pitfalls
ms.assetid: 1e357177-e699-4b8f-9e49-56d3513ed128
caps.latest.revision: "14"
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.openlocfilehash: 54fd201bd4eaef607f8917ea693876aec7fa2923
ms.sourcegitcommit: 4f3fef493080a43e70e951223894768d36ce430a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2017
---
# <a name="potential-pitfalls-in-data-and-task-parallelism"></a>데이터 및 작업 병렬 처리에서 발생할 수 있는 문제
대부분의 경우에서 <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> 및 <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> 일반 순차 루프를 통해 성능 향상을 제공할 수 있습니다. 그러나 루프를 병렬화하는 작업은 순차적 코드에서 일반적이지 않거나 전혀 발생하지 않는 문제를 일으킬 수 있는 복잡성을 도입합니다. 이 항목에서는 병렬 루프를 작성할 때 주의해야 할 사항을 나열합니다.  
  
## <a name="do-not-assume-that-parallel-is-always-faster"></a>병렬이 항상 더 빠르다고 가정하지 마세요.  
 일부 경우에서 병렬 루프는 순차적 루프보다 더 느리게 실행될 수 있습니다. 반복이 적고 빠른 사용자 대리인이 있는 병렬 루프의 속도가 훨씬 빠를 가능성이 없다는 것이 경험적인 기본 규칙입니다. 그러나 많은 요인이 성능에 관련되므로 항상 실제 결과를 측정하는 것이 좋습니다.  
  
## <a name="avoid-writing-to-shared-memory-locations"></a>공유 메모리 위치에 쓰기 방지  
 순차적 코드에서는 정적 변수 또는 클래스 필드에서 읽거나 쓰는 것은 일반적입니다. 그러나 여러 스레드가 해당 변수에 동시에 액세스할 때마다 경합 상태가 발생할 가능성이 큽니다. 잠금을 사용하여 변수에 대한 액세스를 동기화할 수 있지만 동기화의 비용으로 성능이 저하될 수 있습니다. 따라서 가능한 한 병렬 루프에서 공유된 상태에 대한 액세스를 방지하거나 최소한의 제한으로 액세스하는 것이 좋습니다. 이 작업을 수행 하는 가장 좋은 방법은 오버 로드를 사용 하는 <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> 및 <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> 사용 하는 한 <xref:System.Threading.ThreadLocal%601?displayProperty=nameWithType> 루프 실행 중 스레드 로컬 상태를 저장 하는 변수입니다. 자세한 내용은 [방법: 스레드 로컬 변수를 사용하는 Parallel.For 루프 작성](../../../docs/standard/parallel-programming/how-to-write-a-parallel-for-loop-with-thread-local-variables.md) 및 [방법: 스레드 로컬 변수를 사용하는 Parallel.ForEach 루프 작성](../../../docs/standard/parallel-programming/how-to-write-a-parallel-foreach-loop-with-thread-local-variables.md)을 참조하세요.  
  
## <a name="avoid-over-parallelization"></a>과도한 병렬화 방지  
 병렬 루프를 사용하여 소스 컬렉션을 분할하고 작업자 스레드를 동기화하는 오버헤드 비용이 발생합니다. 병렬화의 이점은 컴퓨터의 프로세서 수로 더 제한됩니다. 하나의 프로세서에서 여러 계산 바인딩된 스레드를 실행하여 얻을 수 있는 속도 향상이 없습니다. 따라서 루프를 과도하게 병렬화하지 않도록 주의해야 합니다.  
  
 과도한 병렬화가 발생할 수 있는 가장 일반적인 시나리오는 중첩된 루프에 있습니다. 대부분의 경우에서 다음 조건 중 하나 이상이 적용되지 않는 한 외부 루프만을 병렬화하는 것이 가장 좋습니다.  
  
-   내부 루프가 매우 긴 것으로 알려져 있습니다.  
  
-   각 순서에서 비용이 많이 드는 계산을 수행하고 있습니다. (이 예제에 나와 있는 작업은 비용이 많이 들지 않습니다.)  
  
-   대상 시스템은 `cust.Orders`에서 쿼리를 병렬화하여 생성될 스레드의 수를 처리할 충분한 프로세서를 가진 것으로 알려져 있습니다.  
  
 모든 경우에서 최적의 쿼리 형태를 결정하는 가장 좋은 방법은 테스트하고 측정하는 것입니다.  
  
## <a name="avoid-calls-to-non-thread-safe-methods"></a>스레드로부터 안전하지 않은 메서드에 대한 호출 방지  
 병렬 루프에서 스레드로부터 안전하지 않은 인스턴스 메서드를 작성하면 프로그램에서 감지하거나 감지하지 않을 수도 있는 데이터 손상이 발생할 수 있습니다. 예외가 발생할 수도 있습니다. 다음 예제에서는 여러 스레드 호출을 시도 합니다는 <xref:System.IO.FileStream.WriteByte%2A?displayProperty=nameWithType> 메서드 동시에 클래스에 의해 지원 되지 않습니다.  
  
 [!code-csharp[TPL_Pitfalls#04](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_pitfalls/cs/pitfalls.cs#04)]
 [!code-vb[TPL_Pitfalls#04](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_pitfalls/vb/pitfalls_vb.vb#04)]  
  
## <a name="limit-calls-to-thread-safe-methods"></a>스레드로부터 안전한 메서드에 대한 호출 제한  
 .NET Framework에서 대부분의 정적 메서드는 스레드로부터 안전하고 여러 스레드에서 동시에 호출될 수 있습니다. 그러나 이러한 경우에도 관련된 동기화로 인해 쿼리 속도가 상당히 느려질 수 있습니다.  
  
> [!NOTE]
>  테스트할 수 있습니다이 대 한 직접 호출을 몇 개를 삽입 하 여 <xref:System.Console.WriteLine%2A> 쿼리에서 합니다. 이 메서드는 설명 목적으로 설명서 예제에서 사용되지만 필요하지 않는 한 병렬 루프에서 사용하지 마세요.  
  
## <a name="be-aware-of-thread-affinity-issues"></a>스레드 선호도 문제 인식  
 STA(단일 스레드 아파트) 구성 요소에 대한 COM 상호 운용성, Windows Forms 및 WPF(Windows Presentation Foundation)와 같은 일부 기술은 특정 스레드에서 실행하는 코드를 필요로 하는 스레드 선호도 제한 사항이 적용됩니다. 예를 들어 Windows Forms 및 WPF에서 컨트롤은 작성된 스레드에서만 액세스될 수 있습니다. 즉, 예를 들어 스레드 스케줄러를 UI 스레드에서만 작업을 예약하도록 구성하지 않는 한 병렬 루프에서 목록 컨트롤을 업데이트할 수 없습니다. 자세한 내용은 [방법: UI(사용자 인터페이스) 스레드에서 작업 예약](http://msdn.microsoft.com/library/32a846a5-d628-4933-907b-4888ff72c663)을 참조하세요.  
  
## <a name="use-caution-when-waiting-in-delegates-that-are-called-by-parallelinvoke"></a>Parallel.Invoke에 의해 호출되는 대리자에서 대기하는 경우 주의  
 특정 상황에서 작업 병렬 라이브러리는 작업을 인라인합니다. 즉 현재 스레드를 실행 중인 작업을 실행합니다. (자세한 내용은 [작업 스케줄러](http://msdn.microsoft.com/library/638f8ea5-21db-47a2-a934-86e1e961bf65)를 참조하세요.) 이러한 성능 최적화로 인해 특정 경우에서 교착 상태가 발생할 수 있습니다. 예를 들어 두 작업은 이벤트가 발생할 때 신호를 받은 다음 다른 작업이 신호를 받기를 기다리는 동일한 대리자 코드를 실행할 수 있습니다. 두 번째 작업이 첫 번째와 동일한 스레드에서 인라인되고 첫 번째가 대기 상태로 되는 경우 두 번째 작업은 해당 이벤트의 신호를 받을 수 없게 됩니다. 이러한 발생을 방지하려면 대기 작업에 시간 제한을 지정하거나 명시적 스레드 생성자를 사용하여 하나의 작업이 다른 작업을 차단할 수 없도록 할 수 있습니다.  
  
## <a name="do-not-assume-that-iterations-of-foreach-for-and-forall-always-execute-in-parallel"></a>ForEach, For 및 ForAll의 반복이 항상 병렬로 실행된다고 가정하지 마세요.  
 이에 해당 개별 반복을 염두에 중요는 <xref:System.Threading.Tasks.Parallel.For%2A>, <xref:System.Threading.Tasks.Parallel.ForEach%2A> 또는 <xref:System.Linq.ParallelEnumerable.ForAll%2A> 년 5 월 반복 하지만 동시에 실행할 필요가 없습니다. 따라서 반복의 병렬 실행 또는 특정 순서로 반복 실행의 정확성에 의존하는 코드를 작성하지 마세요. 예를 들어 다음 코드는 교착 상태의 가능성이 있습니다.  
  
 [!code-csharp[TPL_Pitfalls#01](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_pitfalls/cs/pitfalls.cs#01)]
 [!code-vb[TPL_Pitfalls#01](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_pitfalls/vb/pitfalls_vb.vb#01)]  
  
 이 예제에서는 하나의 반복이 이벤트를 설정하고 다른 모든 반복은 이벤트를 기다립니다. 대기 중인 반복은 이벤트 설정 반복이 완료될 때까지 완료할 수 없습니다. 그러나 대기 중인 반복은 이벤트 설정 반복이 실행될 기회를 갖기 전에 병렬 루프를 실행하는 데 사용되는 모든 스레드를 차단할 수 있습니다. 이로 인해 교착 상태가 발생하고 이벤트 설정 반복은 실행되지 않으며 대기 중인 반복은 시작되지 않습니다.  
  
 특히 병렬 루프의 하나의 반복은 다른 루프의 반복이 진행되기를 기다리면 안 됩니다. 병렬 루프가 반대 순서로 순차적으로 반복되도록 결정하는 경우 교착 상태가 발생합니다.  
  
## <a name="avoid-executing-parallel-loops-on-the-ui-thread"></a>UI 스레드에서 병렬 루프 실행 방지  
 응용 프로그램의 UI(사용자 인터페이스)를 응답 가능한 상태로 유지하는 것은 중요합니다. 작업에 병렬화를 보장하는 충분한 작업을 포함하는 경우 해당 작업을 UI 스레드에서 실행되도록 하면 안 됩니다.  대신 해당 작업을 백그라운드 스레드에서 실행되도록 오프로드해야 합니다. 예를 들어 병렬 루프를 사용하여 UI 컨트롤로 렌더링되어야 하는 일부 데이터를 계산하려는 경우 UI 이벤트 처리기에서 직접 실행이 아닌 작업 인스턴스 내 루프 실행을 고려해야 합니다.  코어 계산이 완료된 경우에만 UI 업데이트를 UI 스레드로 마샬링해야 합니다.  
  
 UI 스레드에서 병렬 루프를 실행하면 실행하는 경우 루프 내에서 UI 컨트롤을 업데이트하지 않도록 주의해야 합니다. UI 스레드에서 실행 중인 병렬 루프 내에서 UI 컨트롤 업데이트를 시도하는 경우 UI 업데이트가 호출되는 방식에 따라 상태 손상, 예외, 지연된 업데이트 및 교착 상태가 발생할 수 있습니다. 다음 예제에서 병렬 루프는 모든 반복이 완료될 때까지 실행 중인 UI 스레드를 차단합니다. 그러나 반복 루프가 실행 되 고 있는지 백그라운드 스레드 (으로 <xref:System.Threading.Tasks.Parallel.For%2A> 할 수), Invoke 호출 하면 UI 스레드와 블록 처리 되도록 해당 메시지에 대 한 대기로 전송 될 메시지입니다. UI 스레드가 차단 되는 실행 하므로 <xref:System.Threading.Tasks.Parallel.For%2A>, 메시지를 처리할 수 없습니다, 및 UI 스레드가 교착 상태입니다.  
  
 [!code-csharp[TPL_Pitfalls#02](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_pitfalls/cs/pitfalls.cs#02)]
 [!code-vb[TPL_Pitfalls#02](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_pitfalls/vb/pitfalls_vb.vb#02)]  
  
 다음 예제는 작업 인스턴스 내에서 루프를 실행하여 교착 상태를 방지하는 방법을 보여 줍니다. UI 스레드는 루프에 의해 차단되지 않으며 메시지는 처리될 수 있습니다.  
  
 [!code-csharp[TPL_Pitfalls#03](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_pitfalls/cs/pitfalls.cs#03)]
 [!code-vb[TPL_Pitfalls#03](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_pitfalls/vb/pitfalls_vb.vb#03)]  
  
## <a name="see-also"></a>참고 항목  
 [병렬 프로그래밍](../../../docs/standard/parallel-programming/index.md)  
 [PLINQ에서 발생할 수 있는 문제](../../../docs/standard/parallel-programming/potential-pitfalls-with-plinq.md)  
 [병렬 프로그래밍 패턴: .NET Framework 4의 병렬 패턴 이해 및 적용](http://go.microsoft.com/fwlink/?LinkID=185142)
