---
title: "ServiceThrottlingBehavior를 사용하여 WCF 서비스 성능 제어"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords: behavior [WCF], service performance
ms.assetid: f9dc120c-dc24-49d5-930e-b22f5bc73423
caps.latest.revision: "8"
author: dotnet-bot
ms.author: dotnetcontent
manager: wpickett
ms.workload: dotnet
ms.openlocfilehash: 2b7e00e70bab0c5652bbc721d582a1b276e6a3fa
ms.sourcegitcommit: 16186c34a957fdd52e5db7294f291f7530ac9d24
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/22/2017
---
# <a name="using-servicethrottlingbehavior-to-control-wcf-service-performance"></a>ServiceThrottlingBehavior를 사용하여 WCF 서비스 성능 제어
<xref:System.ServiceModel.Description.ServiceThrottlingBehavior> 클래스는 응용 프로그램 수준에서 생성되는 인스턴스 또는 세션의 수를 제한하기 위해 사용할 수 있는 속성을 노출합니다. 이 동작을 사용하여 [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] 응용 프로그램의 성능을 세밀하게 조정할 수 있습니다.  
  
## <a name="controlling-service-instances-and-concurrent-calls"></a>서비스 인스턴스 및 동시 호출 제어  
 <xref:System.ServiceModel.Description.ServiceThrottlingBehavior.MaxConcurrentCalls%2A> 클래스를 통해 처리 중인 최대 메시지 수를 지정하려면 <xref:System.ServiceModel.ServiceHost> 속성을 사용하고, 서비스의 최대 <xref:System.ServiceModel.Description.ServiceThrottlingBehavior.MaxConcurrentInstances%2A> 개체 수를 지정하려면 <xref:System.ServiceModel.InstanceContext> 속성을 사용합니다.  
  
 에 대 한 응용 프로그램을 실행 하는 실제 경험 이후에 발생 하는 설정을 확인 하 고 이러한 속성에 대 한 일반적으로 하기 때문에 로드에 대 한 설정을 <xref:System.ServiceModel.Description.ServiceThrottlingBehavior> 속성은 일반적으로 사용 하면 응용 프로그램 구성 파일에 지정 됩니다는 [ \<serviceThrottling >](../../../../docs/framework/configure-apps/file-schema/wcf/servicethrottling.md) 요소입니다.  
  
 다음 코드 예제에서는 응용 프로그램 구성 파일에서 <xref:System.ServiceModel.Description.ServiceThrottlingBehavior> 클래스를 사용하여 <xref:System.ServiceModel.Description.ServiceThrottlingBehavior.MaxConcurrentSessions%2A>, <xref:System.ServiceModel.Description.ServiceThrottlingBehavior.MaxConcurrentCalls%2A> 및 <xref:System.ServiceModel.Description.ServiceThrottlingBehavior.MaxConcurrentInstances%2A> 속성을 1로 설정하는 간단한 예를 보여 줍니다. 실제 경험을 통해 특정 응용 프로그램에 대한 최적의 설정을 결정합니다.  
  
 [!code-xml[ServiceThrottlingBehavior#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/servicethrottlingbehavior/cs/hostapplication.exe.config#3)]  
  
 정확한 런타임 동작은 <xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A> 및 <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A> 속성의 값에 따라 다르며, 이 속성은 각각 작업에서 한 번에 실행 가능한 메시지 수와 수신 채널 세션 수를 기준으로 <xref:System.ServiceModel.InstanceContext> 서비스의 수명을 제어합니다.  
  
 자세한 내용은 <xref:System.ServiceModel.Description.ServiceThrottlingBehavior.MaxConcurrentCalls%2A> 및 <xref:System.ServiceModel.Description.ServiceThrottlingBehavior.MaxConcurrentInstances%2A>를 참조하세요.  
  
## <a name="see-also"></a>참고 항목  
 <xref:System.ServiceModel.Description.ServiceThrottlingBehavior>  
 <xref:System.ServiceModel.NetTcpBinding.MaxConnections%2A>
