---
title: "서비스: Calls Per Second"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: 6261d28d-d449-425a-b9fc-a4ee14079134
caps.latest.revision: "9"
author: dotnet-bot
ms.author: dotnetcontent
manager: wpickett
ms.workload: dotnet
ms.openlocfilehash: 0880ce3674f41367c46f4d0268a5391cfda210ab
ms.sourcegitcommit: 16186c34a957fdd52e5db7294f291f7530ac9d24
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/22/2017
---
# <a name="service-calls-per-second"></a>서비스: Calls Per Second
카운터 이름: Calls Per Second  
  
## <a name="description"></a>설명  
 이 서비스에 대한 초당 호출 횟수입니다.  
  
 이 카운터는 성능 카운터 형식 [PERF_COUNTER_COUNTER](http://go.microsoft.com/fwlink/?LinkID=94649), 값은 다음과 같은 수식으로 계산 합니다.  
  
 (N 1 - N 0 ) / ( (D 1 -D 0 ) / F)에서 분자(N)는 마지막 샘플 기간 동안 수행되는 작업 수를 나타내고, 분모(D)는 해당 기간 동안 경과되는 눈금 수를 나타내며 F는 눈금의 주기를 나타냅니다.
