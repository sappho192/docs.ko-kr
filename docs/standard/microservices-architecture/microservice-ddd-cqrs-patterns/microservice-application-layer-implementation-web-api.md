---
title: "마이크로 서비스 응용 프로그램 계층 웹 API를 사용 하 여 구현"
description: "컨테이너 화 된.NET 응용 프로그램에 대 한.NET Microservices 아키텍처 | 마이크로 서비스 응용 프로그램 계층 웹 API를 사용 하 여 구현"
keywords: "Docker, 마이크로 서비스, ASP.NET, 컨테이너"
author: CESARDELATORRE
ms.author: wiwagn
ms.date: 05/26/2017
ms.prod: .net-core
ms.technology: dotnet-docker
ms.topic: article
ms.openlocfilehash: d505a2561ae9b8dee05e803fd639387b63b28b70
ms.sourcegitcommit: bd1ef61f4bb794b25383d3d72e71041a5ced172e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/18/2017
---
# <a name="implementing-the-microservice-application-layer-using-the-web-api"></a>마이크로 서비스 응용 프로그램 계층 웹 API를 사용 하 여 구현

## <a name="using-dependency-injection-to-inject-infrastructure-objects-into-your-application-layer"></a>종속성 주입을 사용 하 여 개체를 삽입할 인프라 응용 프로그램 계층

앞에서 설명한 대로 응용 프로그램 계층을 작성 하는 등 웹 API 프로젝트 또는 웹 응용 프로그램 MVC 프로젝트 내의 대로 아티팩트의 일환으로 구현할 수 있습니다. ASP.NET Core를 사용 하 여 작성 하는 마이크로 서비스의 경우 응용 프로그램 계층은 일반적으로 웹 API 라이브러리를 수 있습니다. 향후 예정 사항 ASP.NET Core (인프라 및 컨트롤러)에서 사용자 지정 응용 프로그램 계층 코드에서 구분 하려는 경우 별도 클래스 라이브러리에 응용 프로그램 계층을 배치할 수도 수 있지만 선택 사항입니다.

정렬 마이크로 서비스의 응용 프로그램 계층 코드의 일부로 직접 구현 되는 예를 들어,는 **Ordering.API** 프로젝트 (ASP.NET Core 웹 API 프로젝트)에 표시 된 그림 9-19로 합니다.

![](./media/image20.png)

**그림 9-19**합니다. Ordering.API ASP.NET Core 웹 API 프로젝트에 응용 프로그램 계층

ASP.NET Core 간단한 포함 [기본 제공 IoC 컨테이너](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) 생성자 삽입 기본적으로 지원 (IServiceProvider 인터페이스 표시 됨) 하 고 ASP.NET을 사용 하면 특정 서비스 DI를 통해 사용할 수 있습니다. ASP.NET Core는 단어를 사용 하 여 *서비스* DI를 통해 삽입 하는 형식 등록에 대 한 합니다. 응용 프로그램의 시작 클래스의 ConfigureServices 메서드에서 기본 제공 컨테이너의 서비스를 구성합니다. 종속성은 서비스에 필요한 형식에서 구현 됩니다.

일반적으로 인프라 개체를 구현 하는 종속성 주입 하려고 합니다. 매우 일반적인 종속성 주입을 리포지토리입니다. 하지만 발생할 수 있는 다른 인프라 종속성 주입할 수 있습니다. 간단한 구현에서는 대 한 DBContext 인프라 지 속성 개체의 구현 이기도 하므로 작업 단위가 패턴 개체 (EF DbContext 개체)를 직접 삽입할 수 있습니다.

다음 예제에서는.NET Core는 생성자를 통해 필요한 저장소 개체를 삽입 하는 방법을 확인할 수 있습니다. 클래스는 명령 처리기에 다음 섹션에서 설명 합니다.

```csharp
// Sample command handler
public class CreateOrderCommandHandler
    : IAsyncRequestHandler<CreateOrderCommand, bool>
{
    private readonly IOrderRepository _orderRepository;

    // Constructor where Dependencies are injected
    public CreateOrderCommandHandler(IOrderRepository orderRepository)
    {
        if (orderRepository == null)
        {
            throw new ArgumentNullException(nameof(orderRepository));
        }
        _orderRepository = orderRepository;
    }

    public async Task<bool> Handle(CreateOrderCommand message)
    {
        //
        // ... Additional code
        //
        // Create the Order AggregateRoot
        // Add child entities and value objects through the Order aggregate root
        // methods and constructor so validations, invariants, and business logic
        // make sure that consistency is preserved across the whole aggregate
        var address = new Address(message.Street, message.City, message.State,
            message.Country, message.ZipCode);
        var order = new Order(address, message.CardTypeId, message.CardNumber,
            message.CardSecurityNumber,
            message.CardHolderName,
            message.CardExpiration);

        foreach (var item in message.OrderItems)
        {
            order.AddOrderItem(item.ProductId, item.ProductName, item.UnitPrice,
                item.Discount, item.PictureUrl, item.Units);
        }

        //Persist the Order through the Repository
        _orderRepository.Add(order);
        var result = await _orderRepository.UnitOfWork
            .SaveEntitiesAsync();
        return result > 0;
    }
}
```

클래스는 삽입 된 저장소를 사용 하 여 트랜잭션을 실행 하 고 상태 변경 내용을 유지 합니다. 또는 해당 클래스는 ASP.NET Core 웹 API 컨트롤러 메서드를 사용 하는 명령 처리기 인지 여부를 중요 하지 않습니다 [DDD 응용 프로그램 서비스](https://lostechies.com/jimmybogard/2008/08/21/services-in-domain-driven-design/)합니다. 궁극적으로 명령 처리기와 비슷한 방식에서 저장소, 도메인 엔터티 및 다른 응용 프로그램 조정이 사용 하는 간단한 클래스입니다. DI를 사용 하 여 예제와 같이 언급 한 모든 클래스에 동일한 방식으로 생성자에 따라 종속성 주입 작동 합니다.

### <a name="registering-the-dependency-implementation-types-and-interfaces-or-abstractions"></a>종속성 구현 형식 및 인터페이스 또는 추상화를 등록합니다.

생성자를 통해 삽입 된 개체를 사용 전에 DI 통해 응용 프로그램 클래스에 주입 된 개체를 생성 하는 클래스 및 인터페이스를 등록 하는 위치를 알 필요가 있습니다. (DI 같은 기준 생성자를 이전에 표시 된 것 처럼.)

#### <a name="using-the-built-in-ioc-container-provided-by-aspnet-core"></a>ASP.NET Core에서 제공 하는 기본 제공 IoC 컨테이너를 사용 하 여

ASP.NET Core에서 제공 하는 기본 제공 IoC 컨테이너를 사용 하면를 ConfigureServices 메서드에 다음 코드 에서처럼 Startup.cs 파일에 넣으려면 형식을 등록 합니다.

```csharp
// Registration of types into ASP.NET Core built-in container
public void ConfigureServices(IServiceCollection services)
{
    // Register out-of-the-box framework services.
    services.AddDbContext<CatalogContext>(c =>
    {
        c.UseSqlServer(Configuration["ConnectionString"]);
    },
    ServiceLifetime.Scoped
    );
    services.AddMvc();
    // Register custom application dependencies.
    services.AddScoped<IMyCustomRepository, MyCustomSQLRepository>();
}
```

한 쌍의 종류를 등록 하는 IoC 컨테이너의 형식을 등록 하는 경우 가장 일반적인 패턴-인터페이스와 관련 된 구현 클래스입니다. 그런 다음 모든 생성자를 통해 IoC 컨테이너에서 개체를 요청할 때 인터페이스의 특정 종류의 개체를 요청 합니다. 예를 들어, 앞의 예에서 마지막 줄에 따르면 IMyCustomRepository (인터페이스 또는 추상화)에서 종속성을 갖는 사용자 생성자 IoC 컨테이너가 MyCustomSQLServerRepository 구현의 인스턴스 주입 됩니다. 클래스입니다.

#### <a name="using-the-scrutor-library-for-automatic-types-registration"></a>Scrutor 라이브러리를 사용 하 여 자동 형식 등록

.NET Core에서는 DI를 사용 하는 경우에 어셈블리를 검사 하 고 해당 형식을 규칙에 따라 자동으로 등록할 수 있게 되기를 것이 좋습니다. 이 기능은 ASP.NET Core에서 현재 사용할 수 없습니다. 사용할 수 있습니다는 [Scrutor](https://github.com/khellang/Scrutor) 라이브러리입니다. 이 방법은 IoC 컨테이너에 등록 해야 하는 형식의 많을 때 편리 합니다.

#### <a name="additional-resources"></a>추가 리소스

-   **Matthew 킹 합니다. 서비스를 Scrutor 등록**
    [*https://mking.io/blog/registering-services-with-scrutor*](https://mking.io/blog/registering-services-with-scrutor)

<!-- -->

-   **Kristian Hellang 합니다. Scrutor 합니다.** GitHub 리포지토리 합니다.
    [*https://github.com/khellang/Scrutor*](https://github.com/khellang/Scrutor)

#### <a name="using-autofac-as-an-ioc-container"></a>Autofac IoC 컨테이너로 사용 하 여

또한 추가 IoC 컨테이너를 사용할 수 있으며 ASP.NET Core 파이프라인에서 사용 하 여 eShopOnContainers 정렬 마이크로 서비스와 같이 연결 [Autofac](https://autofac.org/)합니다. Autofac를 사용 하는 경우 일반적으로 분할 위치 형식에 있는지에 따라, 클래스 라이브러리를 여러 분산 응용 프로그램 종류를 가질 수 없습니다 것 처럼 여러 파일 간에 등록 형식을 할 수 있는 모듈을 통해 형식을 등록 합니다.

예를 들어 다음은는 [Autofac 응용 프로그램 모듈](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Infrastructure/AutofacModules/ApplicationModule.cs) 에 대 한는 [Ordering.API 웹 API](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Services/Ordering/Ordering.API) 삽입 하려는 형식 사용 하 여 프로젝트.

```csharp
public class ApplicationModule
    :Autofac.Module
{
    public string QueriesConnectionString { get; }
    public ApplicationModule(string qconstr)
    {
        QueriesConnectionString = qconstr;
    }

    protected override void Load(ContainerBuilder builder)
    {
        builder.Register(c => new OrderQueries(QueriesConnectionString))
            .As<IOrderQueries>()
            .InstancePerLifetimeScope();
        builder.RegisterType<BuyerRepository>()
            .As<IBuyerRepository>()
            .InstancePerLifetimeScope();
        builder.RegisterType<OrderRepository>()
            .As<IOrderRepository>()
            .InstancePerLifetimeScope();
        builder.RegisterType<RequestManager>()
            .As<IRequestManager>()
            .InstancePerLifetimeScope();
   }
}
```

등록 프로세스 및 개념 기본 제공 ASP.NET Core iOS 컨테이너와 형식을 등록할 수 있는 방식으로 매우 유사 하지만 Autofac를 사용 하는 경우 구문이 약간 달라 집니다.

예제 코드에 추상화 IOrderRepository OrderRepository 구현 클래스와 함께 등록 됩니다. 이 생성자를 선언 IOrderRepository 추상화 또는 인터페이스를 통해 종속성을 때마다 IoC 컨테이너는 OrderRepository 클래스의 인스턴스 삽입 의미 합니다.

인스턴스 범위 형식 인스턴스는 동일한 서비스 또는 종속성에 대 한 요청 간에 공유 되는 방식을 결정 합니다. 종속성에 대 한 요청이 이루어지면 IoC 컨테이너 다음 반환할 수 있습니다.

-   수명 범위 당 단일 인스턴스 (으로 ASP.NET Core IoC 컨테이너에서 참조 *범위*).

-   종속성 당 새 인스턴스 (으로 ASP.NET Core IoC 컨테이너에서 참조 *일시적인*).

-   IoC 컨테이너를 사용 하 여 모든 개체에서 공유 하는 단일 인스턴스 (으로 ASP.NET Core IoC 컨테이너에서 참조 *singleton*).

#### <a name="additional-resources"></a>추가 리소스

-   **ASP.NET Core의 종속성 주입 소개**
    [*https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection*](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection)

-   **Autofac 합니다.** 공식 설명서입니다.
    [*http://docs.autofac.org/en/latest/*](http://docs.autofac.org/en/latest/)

-   **Cesar de la Torre. ASP.NET Core IoC 컨테이너 서비스 수명을 Autofac IoC 컨테이너 인스턴스 범위를 비교**
    [*https://blogs.msdn.microsoft.com/cesardelatorre/2017/01/26/ comparing-asp-net-core-ioc-service-life-times-and-autofac-ioc-instance-scopes/*](https://blogs.msdn.microsoft.com/cesardelatorre/2017/01/26/comparing-asp-net-core-ioc-service-life-times-and-autofac-ioc-instance-scopes/)

## <a name="implementing-the-command-and-command-handler-patterns"></a>명령 및 명령 처리기 패턴 구현

이전 섹션에 표시 된 생성자를 통해 DI 예에서 IoC 컨테이너 클래스에서 생성자를 통해 저장소를 삽입 했습니다. 하지만 정확히 여기서 된은 삽입? 간단한 웹 API (예를 들어는 카탈로그 마이크로 서비스 eShopOnContainers에)에 있습니다 이러한 구성 요소는 컨트롤러 생성자에서 MVC 컨트롤러 수준의 합니다. 그러나이 섹션의 초기 코드에서는 (의 [CreateOrderCommandHandler](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Application/Commands/CreateOrderCommandHandler.cs) eShopOnContainers의 Ordering.API 서비스에서 클래스), 종속성 주입 특정 명령 생성자를 통해 수행 됩니다 처리기입니다. 설명 하겠습니다 명령 처리기 이며 그 사용 하는 이유입니다.

이 가이드 앞부분에 도입 된 CQRS 패턴 명령 패턴 기본적으로 관련 되어 있습니다. CQRS에는 양측이 있습니다. 첫 번째 영역은 간단한 쿼리를 사용 하 여 쿼리는 [Dapper](https://github.com/StackExchange/dapper-dot-net) 마이크로 ORM에서는 이전에 설명 했습니다. 두 번째 영역은 트랜잭션의 시작 지점 및 서비스 외부의 입력된 채널에서 명령을 보여 줍니다.

패턴은 클라이언트 쪽에서 명령을 수락 기반 그림 9-20과 같이 도메인 모델 규칙에 따라 처리 하 고 마지막으로 트랜잭션 사용 하 여 상태를 유지 합니다.

![](./media/image21.png)

**그림 9-20**합니다. 명령 또는 CQRS 패턴의 "트랜잭션 쪽" 상위 수준 보기

### <a name="the-command-class"></a>명령 클래스

명령에는 시스템의 상태를 변경 하는 작업을 수행 하려면 시스템에 대 한 요청입니다. 명령과 필수적인 경우는 한 번만 처리 해야 합니다.

명령을 요건 되므로 일반적으로 (예를 들어 "만들기" 또는 "업데이트")를 명령적 흥미가 동사와 이름 지정 및 집계 유형을 CreateOrderCommand 등을 예로 들 수 있습니다. 이벤트와 달리 명령이 아닙니다; 과거의 팩트를 요청에만 되며 거부 될 수 있습니다.

프로세스 관리자가 작업을 수행 하는 집계를 보내는 경우 명령 요청을 시작 하는 사용자의 결과 UI 또는 프로세스 관리자에서 만들 수 있습니다.

명령의 중요 한 특징은 단일 수신자가 한 번만 처리 되어야 한다는 것입니다. 즉, 단일 작업이 나 트랜잭션 응용 프로그램에서 수행 하려는 명령입니다. 예를 들어 동일한 순서 생성 명령은 두 번 이상 처리 되지 않도록 합니다. 이것이 명령 및 이벤트에 대 한 중요 한 차이점입니다. 이벤트는 시스템 또는 microservices 많은 이벤트에 관심을 가질 수 있으므로 여러 번 처리 될 수 있습니다.

또한이 경우에 명령 idempotent 명령을 한 번만 처리할 수 있는지 중요 합니다. 명령을 idempotent 이면에 있는 명령의 특성으로 인해 또는 시스템에서 명령을 처리 하는 방식 때문에 결과 변경 하지 않고 여러 번 실행할 수 있습니다.

명령을 확인 하는 것이 좋습니다 및 업데이트 것은 idempotent 도메인의 비즈니스 규칙 및 고정에서 필요 합니다. 예를 들어, 동일한 예제를 어떤 이유로 든 (예: 재시도 논리, 해킹, 등)에 대 한 동일한 CreateOrder 명령에 도달 하면 시스템 여러 번 사용 하려면 식별 하 고 여러 주문 만들지 않을 수 있어야 있습니다. 이렇게 하려면 작업에는 id의 몇 가지 종류를 연결 및 명령 또는 업데이트가 이미 처리 여부를 확인 해야 합니다.

단일 수신자; 명령 보내기 명령을 게시 하지 마십시오. 팩트는이 상태는 통합 이벤트에 대 한 게시를-이벤트 수신기에 관심이 있을 무언가 현재 상황 및 수도 있습니다. 이벤트의 경우 게시자에 대 한 수신기를 얻거나 이벤트 수행할 것 문제 없이 있습니다. 하지만 통합 이벤트는 이미 이전 섹션에서 도입 된 다른 문제입니다.

데이터 필드 또는 명령을 실행 하는 데 필요한 모든 정보가 포함 된 컬렉션을 포함 하는 클래스는 명령을 구현 됩니다. 명령에는 특별 한 종류의 데이터 전송 개체 DTO (), 특히 변경 또는 트랜잭션을 요청 하는 데 사용 되는 하나입니다. 명령 자체 정확히 명령을 하며 그 이상의 처리에 필요한 정보를 기반으로 합니다.

다음 예제에서는 간단한 CreateOrderCommand 클래스를 보여 줍니다. EShopOnContainers에 정렬 마이크로 서비스에 사용 되는 변경할 수 없는 명령입니다.

```csharp
// DDD and CQRS patterns comment
// Note that it is recommended that yuo implement immutable commands
// In this case, immutability is achieved by having all the setters as private
// plus being able to update the data just once, when creating the object
// through the constructor.
// References on immutable commands:
// http://cqrs.nu/Faq
// https://docs.spine3.org/motivation/immutability.html
// http://blog.gauffin.org/2012/06/griffin-container-introducing-command-support/
// https://msdn.microsoft.com/en-us/library/bb383979.aspx
[DataContract]
public class CreateOrderCommand
    :IAsyncRequest<bool>
{
    [DataMember]
    private readonly List<OrderItemDTO> _orderItems;

    [DataMember]
    public string City { get; private set; }

    [DataMember]
    public string Street { get; private set; }

    [DataMember]
    public string State { get; private set; }

    [DataMember]
    public string Country { get; private set; }

    [DataMember]
    public string ZipCode { get; private set; }

    [DataMember]
    public string CardNumber { get; private set; }

    [DataMember]
    public string CardHolderName { get; private set; }

    [DataMember]
    public DateTime CardExpiration { get; private set; }

    [DataMember]
    public string CardSecurityNumber { get; private set; }

    [DataMember]
    public int CardTypeId { get; private set; }

    [DataMember]
    public IEnumerable<OrderItemDTO> OrderItems => _orderItems;

    public CreateOrderCommand()
    {
        _orderItems = new List<OrderItemDTO>();
    }

    public CreateOrderCommand(List<OrderItemDTO> orderItems, string city,
        string street,
        string state, string country, string zipcode,
        string cardNumber, string cardHolderName, DateTime cardExpiration,
        string cardSecurityNumber, int cardTypeId) : this()
    {
        _orderItems = orderItems;
        City = city;
        Street = street;
        State = state;
        Country = country;
        ZipCode = zipcode;
        CardNumber = cardNumber;
        CardHolderName = cardHolderName;
        CardSecurityNumber = cardSecurityNumber;
        CardTypeId = cardTypeId;
        CardExpiration = cardExpiration;
    }

    public class OrderItemDTO
    {
        public int ProductId { get; set; }
        public string ProductName { get; set; }
        public decimal UnitPrice { get; set; }
        public decimal Discount { get; set; }
        public int Units { get; set; }
        public string PictureUrl { get; set; }
    }
}
```

기본적으로, 명령 클래스는 도메인 모델 개체를 사용 하 여 비즈니스 거래를 수행 하는 데 필요한 모든 데이터를 포함 합니다. 따라서 읽기 전용 데이터와 없는 동작을 포함 하는 데이터 구조 단순히 주석은 있습니다. 명령 이름은 용도 나타냅니다. C와 같은 다양 한 언어로\#, 명령을 클래스로 표현 됩니다 있지만 true 클래스에는 실제 개체 지향적 의미 하지 않습니다.

프로그램 추가 특징으로 명령을 변경할 수 없는 도메인 모델에서 직접 처리 하 여 예상된 사용량 이므로 합니다. 예상된 수명 동안 변경할 필요가 없습니다. c에서\# 클래스 불변성 고 다니지 않아도 모든 setter 또는 내부 상태를 변경 하는 다른 방법으로 구현할 수 있습니다.

예를 들어 주문을 만들기 위한 명령 클래스는 순서를 만들려는 데이터 측면에서 거의 유사 필요는 없지만 아마도 하지 동일한 특성입니다. 예를 들어, CreateOrderCommand 않아도 주문 ID, 순서 아직 생성 되지 않았기 때문에 됩니다.

다 수의 명령 클래스는 단순 변경 해야 하는 일부 상태에 대 한 필드를 몇 개만 필요 수 있습니다. 되는 경우 방금 "에 프로세스에서" 주문 상태를 변경 하는 경우 "지불" 또는 "배송" 다음과 비슷한 명령을 사용 하 여:

```csharp
[DataContract]
public class UpdateOrderStatusCommand
    :IAsyncRequest<bool>
{
    [DataMember]
    public string Status { get; private set; }

    [DataMember]
    public string OrderId { get; private set; }

    [DataMember]
    public string BuyerIdentityGuid { get; private set; }
}
```

일부 개발자가 자신의 명령 Dto 별개의 UI 요청 개체 확인 수 있지만 기본 설정 만으로. 매우 부가 가치와 번거로운 분리 되며 개체는 같은 형태로 거의 정확 하 게 합니다. 예를 들어, eShopOnContainers, 명령을 클라이언트 쪽에서 직접 제공 됩니다.

### <a name="the-command-handler-class"></a>명령 처리기 클래스

각 명령에 대 한 특정 명령 처리기 클래스를 구현 해야 합니다. 패턴의 작동 방식 이며 명령 개체, 도메인 개체 및 인프라 리포지토리 개체를 사용 하는 것이 있습니다. 명령 처리기가 실제로 CQRS 및 DDD 측면에서 응용 프로그램 계층의 핵심입니다. 그러나 도메인 클래스 내에서 모든 도메인 논리를 포함 되어야 합니다-집계 루트 (루트 엔터티) 내에서 자식 엔터티 또는 [도메인 서비스](https://lostechies.com/jimmybogard/2008/08/21/services-in-domain-driven-design/), 하지만 명령 처리기 내 어떤 버전이 응용 프로그램에서 클래스 계층입니다.

명령 처리기 명령을 수신 하 고 사용 되는 집계 결과 가져옵니다. 결과 중 하나에 있는 명령의 성공적인 실행 또는 예외 여야 합니다. 예외가 발생 한 경우 시스템 상태 변경 되지 않도록 합니다.

명령 처리기는 일반적으로 다음 단계를 수행 합니다.

-   명령 개체는 DTO 같은 받은 (에서 [중재자](https://en.wikipedia.org/wiki/Mediator_pattern) 또는 다른 인프라 개체).

-   이 명령은 (유효성을 검사 하지 중재자) 하는 경우 유효한 지 유효성을 검사 합니다.

-   현재 명령 대상 집계 루트 인스턴스를 인스턴스화합니다.

-   명령에서 필요한 데이터를 가져오는 집계 루트 인스턴스 메서드를 실행 합니다.

-   집계 하는 관련된 데이터베이스의 새 상태를 유지 합니다. 이 마지막 작업은 실제 트랜잭션.

일반적으로 명령 처리기 집계 루트 (루트 엔터티)에 의해 발생 한 단일 집계를 다룹니다. 여러 집계 단일 명령을 수신에 의해 영향을 받음 해야, 하는 경우 여러 집계에 상태나 동작 전파 도메인 이벤트를 사용할 수 있습니다.

여기서 중요 한 점은 명령이 처리 되는 도메인 논리를 모든 도메인 모델 (집계)를 완벽 하 게 캡슐화 하 고 단위 테스트를 위한 준비 내부 여야 합니다. 명령 처리기 방금와 역할을 수행, 데이터베이스에서 도메인 모델을 얻을 수 있는 방법 모델 변경 되 면 변경 내용을 유지 하려면 인프라 계층 (저장소)을 구별 하는 마지막 단계입니다. 이 방법의 장점은 응용 프로그램 또는 인프라 레이어는 연결 수준 (명령 처리기, 웹 API는 코드를 변경 하지 않고 격리 된, 완벽 하 게 캡슐화 된, 풍부한, 동작 도메인 모델에는 도메인 논리를 리팩터링할 수입니다. 저장소, 등)입니다.

명령 처리기 코드 냄새 일 수 있는 너무 많은 논리를 사용 하 여 복잡 한 가져올 때 검토 하 고 도메인 논리를 찾을 경우 해당 도메인 동작 (집계 루트와 자식 엔터티) 도메인 개체의 메서드를 이동 하려면 코드를 리팩터링 합니다.

명령 처리기 클래스의 예를 들어 다음 코드는이 장의 시작 부분에서 본 것과 동일한 CreateOrderCommandHandler 클래스를 보여줍니다. 이 경우 핸들 메서드 및 작업 도메인 모델 개체/집계를 강조 표시 되어 있습니다.

```csharp
public class CreateOrderCommandHandler
    : IAsyncRequestHandler<CreateOrderCommand, bool>
{
    private readonly IBuyerRepository _buyerRepository;
    private readonly IOrderRepository _orderRepository;

    public CreateOrderCommandHandler(IBuyerRepository buyerRepository,
        IOrderRepository orderRepository)
    {
        if (buyerRepository == null)
        {
            throw new ArgumentNullException(nameof(buyerRepository));
        }
        if (orderRepository == null)
        {
            throw new ArgumentNullException(nameof(orderRepository));
        }

        _buyerRepository = buyerRepository;
        _orderRepository = orderRepository;
    }

    public async Task<bool> Handle(CreateOrderCommand message)
    {
        //
        // Additional code ...
        //
        // Create the Order aggregate root
        // Add child entities and value objects through the Order aggregate root
        // methods and constructor so validations, invariants, and business logic
        // make sure that consistency is preserved across the whole aggregate
        var order = new Order(buyer.Id, payment.Id,
            new Address(message.Street,
            message.City, message.State,
            message.Country, message.ZipCode));

        foreach (var item in message.OrderItems)
        {
            order.AddOrderItem(item.ProductId, item.ProductName, item.UnitPrice,
                item.Discount, item.PictureUrl, item.Units);
        }

        // Persist the Order through the aggregate's repository
        _orderRepository.Add(order);
        return await _orderRepository.UnitOfWork.SaveChangesAsync();
    }
}
```

다음은 명령 처리기에서 수행 해야 하는 추가 단계입니다.

-   명령의 데이터를 사용 하 여 집계 루트의 메서드 및 동작을 마련입니다.

-   내부적으로 도메인 개체 내에서 이벤트를 발생 시킬 도메인 동안 트랜잭션이 실행 되는 명령 처리기 관점에서 투명 합니다.

-   집계의 작업 결과 성공 및 트랜잭션이 완료 된 후 통합 이벤트 명령 처리기를 발생 시킵니다. (이러한도 발생할 수 있습니다 저장소와 같은 인프라 클래스에서.)

#### <a name="additional-resources"></a>추가 리소스

-   **Seemann 표시 합니다. 경계에서 응용 프로그램은 개체가 아니라 지향**
    [*http://blog.ploeh.dk/2011/05/31/AttheBoundaries, ApplicationsareNotObject 지향 /*](http://blog.ploeh.dk/2011/05/31/AttheBoundaries,ApplicationsareNotObject-Oriented/)

-   **명령 및 이벤트**
    [*http://cqrs.nu/Faq/commands-and-events*](http://cqrs.nu/Faq/commands-and-events)

-   **명령 처리기의 기능은 무엇입니까? ** 
     [ *http://cqrs.nu/Faq/command-handlers*](http://cqrs.nu/Faq/command-handlers)

-   **Jimmy Bogard. 도메인 명령 패턴 – 처리기**
    [*https://jimmybogard.com/domain-command-patterns-handlers/*](https://jimmybogard.com/domain-command-patterns-handlers/)

-   **Jimmy Bogard. 도메인 명령 패턴 – 유효성 검사**
    [*https://jimmybogard.com/domain-command-patterns-validation/*](https://jimmybogard.com/domain-command-patterns-validation/)

## <a name="the-command-process-pipeline-how-to-trigger-a-command-handler"></a>명령 프로세스 파이프라인: 명령 처리기를 트리거하는 방법

다음 질문은 명령 처리기를 호출 하는 방법. 각 관련 ASP.NET Core 컨트롤러에서 수동으로 호출할 수 있습니다. 그러나 방법은 너무 것 결합 된 않으며 바람직하지 않습니다.

다른 두 가지 기본 옵션을 요소인 권장된 하는 옵션은:

-   통해 메모리 내 중재자 패턴 아티팩트입니다.

-   컨트롤러 및 처리기 사이에서 비동기 메시지 큐입니다.

### <a name="using-the-mediator-pattern-in-memory-in-the-command-pipeline"></a>명령 파이프라인에 중재자 패턴 (메모리)를 사용 하 여

그림 9-21 같이 CQRS 접근 방식을 사용 하 여 메모리 버스 명령 또는 수신 되는 DTO의 형식을 기반으로 올바른 명령 처리기를 리디렉션하려면 정도로 스마트를 비슷합니다는 지능형 중재자. 구성 요소 간에 단일 검은색 화살표 상호 관련된 된 개체 (대부분의 경우 DI를 통해 삽입) 간의 종속성을 나타냅니다.

![](./media/image22.png)

**그림 9-21**합니다. 중재자 패턴을 사용 하 여 프로세스에서 단일 CQRS 마이크로 서비스

중재자 패턴을 사용 하 여 쉽게 알아볼 수 있는 이유는는 엔터프라이즈 응용 프로그램에서 처리 요청이 복잡 해질 수 있습니다. 열려 있는 수가 로깅, 유효성 검사, 감사 및 보안 같은 일반적인 문제를 추가할 수 하려고 합니다. 이러한 경우에는 중재자 파이프라인에 사용 (참조 [중재자 패턴](https://en.wikipedia.org/wiki/Mediator_pattern)) 이러한 추가 동작 또는 일반적인 문제에 대 한 방법을 제공 합니다.

중재 역할에서 "방법"이이 프로세스의 캡슐화 하는 개체는: 실행 상태에 따라 조정, 방식으로 한 명령 처리기가 호출 또는 처리기에 제공 하는 페이로드입니다. 중재자 구성 요소에 적용할 수 일반적인 문제 중앙 및 투명 한 방식으로 데코레이터를 적용 하 여 (또는 [동작 파이프라인](https://github.com/jbogard/MediatR/wiki/Behaviors) 중재자 v3 이후). (자세한 내용은 참조는 [Decorator 패턴](https://en.wikipedia.org/wiki/Decorator_pattern).)

동작 및 데코레이터는 유사 [AOP 관점 지향 프로그래밍 ()](https://en.wikipedia.org/wiki/Aspect-oriented_programming), 중재자 구성 요소에 의해 관리 되는 특정 프로세스 파이프라인에만 적용 합니다. 일반적인 문제를 구현 하는 AOP의 측면에 따라 적용 됩니다 *측면 weavers* 개체 호출 인터 셉 션에 따라 또는 컴파일 시에 삽입 합니다. 쉽게 AOP는 작업을 수행 하는 방법을 확인할 수 없기 때문에 "매직" 처럼 작동을 일반적인 AOP 두 가지 방법은 모두 때때로 이라고 합니다. 심각한 문제 또는 버그를 처리할 때 AOP 디버그 하기가 어려워질 수 있습니다. 반면에 이러한 데코레이터/동작은 명시적 및의 중재자 역할의 경우에만 적용 되므로 훨씬 더 예측 가능 하 고 쉽게 디버깅은입니다.

예를 들어 마이크로 서비스를 주문 eShopOnContainers에서 구현한 두 명의 샘플 데코레이터는 [LogDecorator](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Application/Decorators/LogDecorator.cs) 클래스 및 [ValidatorDecorator](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Application/Decorators/ValidatorDecorator.cs) 클래스입니다. 데코레이터의 구현은 다음 섹션에 설명 되어 있습니다. 이후 버전에서 eShopOnContainers는로 마이그레이션합니다는 참고 [MediatR 3](https://www.nuget.org/packages/MediatR/3.0.0) 이동 [동작](https://github.com/jbogard/MediatR/wiki/Behaviors) 데코레이터를 사용 하는 대신 합니다.

### <a name="using-message-queues-out-of-proc-in-the-commands-pipeline"></a>명령의 파이프라인에서 메시지 큐 (아웃 프로시저)를 사용 하 여

다른 선택 그림 9-22와 같이 브로커 또는 메시지 큐에 따라 비동기 메시지를 사용 하는 것입니다. 명령 처리기 직전 중재자 구성 요소가 해당 옵션을 함께 사용할 수도 있습니다.

![](./media/image23.png)

**그림 9-22**합니다. CQRS 명령을 사용 하 여 프로세스와 프로세스 간 통신) (중 메시지 큐를 사용 하 여

두 프로세스 파이프라인 분할 해야 하기 때문에 명령을 추가 수를 허용 하도록 큐에 명령 파이프라인 복잡 하 게 메시지를 사용 하 여 외부 메시지 큐를 통해 연결 합니다. 여전히, 확장성 및 성능 비동기 메시징에 따라 더 효율적으로 필요한 경우 사용 될 해야 합니다. 다음을 고려해 야의 경우 그림 9-22, 컨트롤러에서 방금 명령 메시지 큐에 게시 하 고 반환 합니다. 다음 명령 처리기 원하는 속도로 메시지를 처리 합니다. 큐의 많은 이점이 즉-하이퍼 확장성이 필요한, 주식 또는 고용량 유입 되는 데이터와는 다른 모든 시나리오의 경우와 같은 경우 메시지 큐의 경우에서 버퍼 역할도 할 수 있습니다.

그러나 메시지 큐의 비동기 특성 때문에 해야 명령의 프로세스의 성공 여부에 대 한 클라이언트 응용 프로그램와 통신 하는 방법을 알아보세요. 일반적으로 하지 않습니다 "fire and forget" 명령을 사용 해야 합니다. 모든 비즈니스 응용 프로그램 알아야 하는 경우 명령을 처리 성공적으로 또는 이상 유효성을 검사 하 고 했으면 허용 합니다.

따라서 비동기 큐에 전송 된 명령 메시지를 확인 한 후 클라이언트에 반응 하는 트랜잭션을 실행 한 후 작업의 결과 반환 하는 프로세스에 명령 프로세스를 비교 하 여이 시스템에 복잡성을 추가 합니다. 큐를 사용 하 여 시스템에서 추가 구성 요소 및 사용자 지정 통신 해야 하는 다른 작업 결과 메시지를 통해 명령 프로세스의 결과 반환 하려면 할 수 있습니다.

또한 비동기 명령을 필요 하지 않습니다, Burtsev Alexey 사이의 Greg Young에 다음과 같은 흥미로운 교환에서 설명한 대로 대부분의 경우에는 단방향 명령은 [온라인 대화](https://groups.google.com/forum/#!msg/dddcqrs/xhJHVxDx2pM/WP9qP8ifYCwJ):

\[Burtsev Alexey\] 필자는 코드의 많은 사람들이 사용 비동기 명령 처리 하거나 어떤 이유로 이렇게 하려면 없이 메시징 하는 한 가지 방법은 명령을 (긴 일부 작업을 수행 하지 않습니다, 외부 비동기 코드 실행 아닌, 응용 프로그램도 교차 하지 않는 메시지 버스를 사용 하 여 수 경계)입니다. 불필요 한 복잡성이를 적용 하는 이유은? 및 실제로 듣지 못했습니다 명령 처리기를 지금까지 차단 CQRS 코드 예제에서는 경우에 대부분의 경우에서 문제 없이 작동 합니다.

\[Greg Young\] \[... \] 비동기 명령을 존재 하지 않습니다; 실제로 다른 이벤트입니다. I 보내는 메시지에 동의 하 고 동의 하는 경우 이벤트를 발생 시키는 경우 더 이상 다르다는 작업을 수행할 수 없습니다. 되기 다르다는 것 조건이 있습니다. 처음에 약간의 차이만 처럼 보이지만 많은 특성을 갖습니다.

비동기 명령을 간단한 오류를 나타낼 방법이 없기 때문에 시스템의 복잡성이 크게 증가 합니다. 따라서 비동기 명령은 권장 되지 않습니다 아닌 다른 크기 조정 요구 사항을 필요할 경우 또는 같은 특수 한 경우 내부 microservices 메시징을 통해 통신할 때. 이러한 경우에는 별도 보고 및 복구 시스템 오류에 대 한 디자인 해야 합니다.

EShopOnContainers의 초기 버전에 HTTP 요청에서 시작 하 고 중재자 패턴에 의해 발생 동기 명령 처리를 사용 하기로 결정 했습니다. 사용자가 쉽게에서 같이 프로세스의 성공 여부를 반환할 수 있습니다 허용 하는 [CreateOrderCommandHandler](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Application/Commands/CreateOrderCommandHandler.cs) 구현 합니다.

어떤 경우 든,이 응용 프로그램의 또는 마이크로 서비스의 비즈니스 요구 사항에 따라 결정 해야 합니다.

## <a name="implementing-the-command-process-pipeline-with-a-mediator-pattern-mediatr"></a>명령 프로세스 파이프라인 (MediatR) 중재자 패턴 구현

샘플 구현으로이 가이드에서는 드라이브 명령 수집 하는 중재자 패턴을 기반으로 하며 라우팅하고 메모리 올바른 명령 처리기에서에서 처리 중인 파이프라인을 사용 하 여을 제안 합니다. 이 가이드는 또한 데코레이터 적용 제안 또는 [동작](https://github.com/jbogard/MediatR/wiki/Behaviors) 일반적인 문제를 구분 하기 위해서입니다.

.NET Core에서는 구현에는 사용할 수 있는 중재자 패턴을 구현 여러 오픈 소스 라이브러리입니다. 이 가이드에 사용 된 라이브러리는는 [MediatR](https://github.com/jbogard/MediatR) (Jimmy Bogard에서 만든) 오픈 소스 라이브러리 있지만 다른 방식을 사용할 수 있습니다. MediatR는 데코레이터 또는 동작을 적용 하는 동안, 명령 처럼 메모리에 메시지를 처리할 수 있는 작고 간단한 라이브러리입니다.

중재자 패턴을 사용 하면 결합을 줄이는 하 고 요청된 된 작업을 자동으로 해당 작업을 수행 하는 처리기에 연결 하는 동안 문제를 찾으려면 수-명령 처리기에이 경우.

중재자 패턴을 사용 하는 다른 이유는이 가이드를 검토할 때 Jimmy Bogard로 설명 했습니다.

시스템의 동작에 대 한 좋은 일관성 있는 창을 제공 – 테스트 여기 언급 두면 유용할 수 있는 것 같습니다. 요청, 응답 아웃 합니다. आ म क ा 그러한 측면 일관성 있게 동작 하는 테스트 구성에서 매우 중요 합니다.

먼저 알려 주세요 검토할 컨트롤러 코드에 실제로 사용 되는 중재자 개체. 중재자 개체를 사용 하지 않은 경우에 모든 종속성 해당 컨트롤러에 대 한로 거 개체 등과 같은 항목을 삽입 해야 합니다. 따라서 생성자는 매우 복잡 한 것입니다. 반면에 중재자 개체를 사용 하면 컨트롤러의 생성자에 다음 예제와 같이 상호 작업당 하나를 사용 하도록 해야 하는 많은 종속성 대신 몇 개의 종속성이 있는 훨씬 더 간단 될 수 있습니다.

```csharp
public class OrdersController : Controller
{
    public OrdersController(IMediator mediator,
        IOrderQueries orderQueries)
    // ...
```

중재자 명확 하 고 간단한 Web API 컨트롤러 생성자를 제공 하는지 확인할 수 있습니다. 또한 컨트롤러 메서드 내에서 중재자 개체에 명령을 보낼 코드는 거의 한 줄입니다.

```csharp
[Route("new")]
[HttpPost]
public async Task<IActionResult> CreateOrder([FromBody]CreateOrderCommand
    createOrderCommand)
{
    var commandResult = await _mediator.SendAsync(createOrderCommand);
    return commandResult ? (IActionResult)Ok() : (IActionResult)BadRequest();
}
```

MediatR 알고 명령 처리기 클래스를 IoC 컨테이너의 중재자 클래스 및 명령 처리기 클래스를 등록 해야 합니다. 기본적으로 MediatR Autofac IoC 컨테이너를 사용 하지만 또한 기본 제공 ASP.NET Core IoC 컨테이너 또는 MediatR에서 지 원하는 다른 컨테이너를 사용할 수 있습니다.

다음 코드 Autofac 모듈을 사용 하는 경우 중재자의 형식 및 명령을 등록 하는 방법을 보여 줍니다.

```csharp
public class MediatorModule : Autofac.Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterAssemblyTypes(typeof(IMediator).GetTypeInfo().Assembly)
            .AsImplementedInterfaces();
        builder.RegisterAssemblyTypes(typeof(CreateOrderCommand)
            .GetTypeInfo().Assembly)
            .As(o => o.GetInterfaces()
            .Where(i => i.IsClosedTypeOf(typeof(IAsyncRequestHandler<,>)))
            .Select(i => new KeyedService("IAsyncRequestHandler", i)));
        builder.RegisterGenericDecorator(typeof(LogDecorator<,>),
            typeof(IAsyncRequestHandler<,>), "IAsyncRequestHandler");

        // Other types registration
    }
}
```

각 명령 처리기 제네릭 IAsyncRequestHandler 사용 하 여 인터페이스를 구현 하기 때문에&lt;T&gt; 는 RegisteredAssemblyTypes를 검사 한 다음 개체를 사용 하므로 처리기에서 해당 명령 처리기로 각 명령에 연결할 수 있는 관계는 다음 예제와 같이 CommandHandler 클래스에 설명 되어 있습니다.

```csharp
public class CreateOrderCommandHandler
  : IAsyncRequestHandler<CreateOrderCommand, bool>
{
```

명령 처리기와 명령을 상관 관계가 있는 코드입니다. 처리기는 간단한 클래스 RequestHandler에서 상속 하지만&lt;T&gt;, MediatR 되도록 올바른 페이로드를 사용 하 여 호출을 가져옵니다.

## <a name="applying-cross-cutting-concerns-when-processing-commands-with-the-mediator-and-decorator-patterns"></a>중재자 및 Decorator 패턴을 사용 하 여 명령을 처리 하는 경우 일반적인 문제를 적용 합니다.

한 가지는: 중재자 파이프라인에 일반적인 문제를 적용할 수 있습니다. 볼 수 있습니다 Autofac 등록 모듈 코드의 끝에 데코레이터 등록 방법을 구체적으로, 사용자 지정 LogDecorator 클래스를 입력 합니다.

이후 버전의 eShopOnContainers 것은 마이그레이션하는 여기서도 마찬가지로 [MediatR 3](https://www.nuget.org/packages/MediatR/3.0.0) 로 이동 하 고 [동작](https://github.com/jbogard/MediatR/wiki/Behaviors) 데코레이터를 사용 하는 대신 합니다.

[LogDecorator](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Application/Decorators/LogDecorator.cs) 명령 처리기가 실행 되 고 성공 했는지 여부에 대 한 정보를 기록 하는 경우 다음 코드와 클래스를 구현할 수 있습니다.

```csharp
public class LogDecorator<TRequest, TResponse>
    : IAsyncRequestHandler<TRequest, TResponse>
    where TRequest : IAsyncRequest<TResponse>
{
    private readonly IAsyncRequestHandler<TRequest, TResponse> _inner;
    private readonly ILogger<LogDecorator<TRequest, TResponse>> _logger;

    public LogDecorator(
        IAsyncRequestHandler<TRequest, TResponse> inner,
        ILogger<LogDecorator<TRequest, TResponse>> logger)
    {
        _inner = inner;
        _logger = logger;
    }

    public async Task<TResponse> Handle(TRequest message)
    {
        _logger.LogInformation($"Executing command {_inner.GetType().FullName}");
        var response = await _inner.Handle(message);
        _logger.LogInformation($"Succeeded executed command{_inner.GetType().FullName}");
        return response;
    }
}
```

이 decorator 클래스를 구현 하 여 및 함께 파이프라인 데코레이팅하여 MediatR를 통해 처리할 모든 명령을 실행 하는 방법에 대 한 정보를 로깅 하기 됩니다.

기본 유효성 검사에 대 한 두 번째 decorator 마이크로 서비스에도 적용 됩니다 순서 eShopOnContainers는 [ValidatorDecorator](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Application/Decorators/ValidatorDecorator.cs) 클래스를 사용 하 여 [FluentValidation](https://github.com/JeremySkinner/FluentValidation) 라이브러리에 표시 된 대로 다음 코드:

```csharp
public class ValidatorDecorator<TRequest, TResponse>
    : IAsyncRequestHandler<TRequest, TResponse>
    where TRequest : IAsyncRequest<TResponse>
{
    private readonly IAsyncRequestHandler<TRequest, TResponse> _inner;
    private readonly IValidator<TRequest>[] _validators;

    public ValidatorDecorator(
        IAsyncRequestHandler<TRequest, TResponse> inner,
        IValidator<TRequest>[] validators)
    {
        _inner = inner;
        _validators = validators;
    }

    public async Task<TResponse> Handle(TRequest message)
    {
        var failures = _validators
            .Select(v => v.Validate(message))
            .SelectMany(result => result.Errors)
            .Where(error => error != null)
            .ToList();
            if (failures.Any())
            {
                throw new OrderingDomainException(
                $"Command Validation Errors for type {typeof(TRequest).Name}",
                new ValidationException("Validation exception", failures));
            }
            var response = await _inner.Handle(message);
        return response;
    }
}
```

다음을 기반으로 [FluentValidation](https://github.com/JeremySkinner/FluentValidation) 라이브러리가 자동으로 다음 코드 에서처럼 CreateOrderCommand를 통해 전달 된 데이터에 대 한 유효성 검사 생성 했습니다.

```csharp
public class CreateOrderCommandValidator : AbstractValidator<CreateOrderCommand>
{
    public CreateOrderCommandValidator()
    {
        RuleFor(command => command.City).NotEmpty();
        RuleFor(command => command.Street).NotEmpty();
        RuleFor(command => command.State).NotEmpty();
        RuleFor(command => command.Country).NotEmpty();
        RuleFor(command => command.ZipCode).NotEmpty();
        RuleFor(command => command.CardNumber).NotEmpty().Length(12, 19);
        RuleFor(command => command.CardHolderName).NotEmpty();
        RuleFor(command => command.CardExpiration).NotEmpty().Must(BeValidExpirationDate).
            WithMessage("Please specify a valid card expiration date");
        RuleFor(command => command.CardSecurityNumber).NotEmpty().Length(3);
        RuleFor(command => command.CardTypeId).NotEmpty();
        RuleFor(command => command.OrderItems).
            Must(ContainOrderItems).WithMessage("No order items found");
    }

    private bool BeValidExpirationDate(DateTime dateTime)
    {
        return dateTime >= DateTime.UtcNow;
    }

    private bool ContainOrderItems(IEnumerable<OrderItemDTO> orderItems)
    {
        return orderItems.Any();
    }
}
```

추가 유효성 검사를 만들 수 있습니다. 이것이 프로그램 명령 유효성 검사를 구현 하는 매우 명확 하 고 세련 된 방법입니다.

이와 비슷한 방식으로 처리 하는 경우 명령에 적용 하려는 일반적인 문제 또는 측면에 대 한 다른 데코레이터를 구현할 수 있습니다.

#### <a name="additional-resources"></a>추가 리소스

##### <a name="the-mediator-pattern"></a>중재자 패턴

-   **중재자 패턴**
    [*https://en.wikipedia.org/wiki/Mediator\_패턴*](https://en.wikipedia.org/wiki/Mediator_pattern)

##### <a name="the-decorator-pattern"></a>Decorator 패턴

-   **Decorator 패턴**
    [*https://en.wikipedia.org/wiki/Decorator\_패턴*](https://en.wikipedia.org/wiki/Decorator_pattern)

##### <a name="mediatr-jimmy-bogard"></a>MediatR (Jimmy Bogard)

-   **MediatR 합니다.** GitHub 리포지토리 합니다.
    [*https://github.com/jbogard/MediatR*](https://github.com/jbogard/MediatR)

-   **MediatR 및 AutoMapper CQRS**
    [*https://lostechies.com/jimmybogard/2015/05/05/cqrs-with-mediatr-and-automapper/*](https://lostechies.com/jimmybogard/2015/05/05/cqrs-with-mediatr-and-automapper/)

-   **컨트롤러에 먹고에 배치: 게시물 및 명령. ** 
     [ *https://lostechies.com/jimmybogard/2013/12/19/put-your-controllers-on-a-diet-posts-and-commands/*](https://lostechies.com/jimmybogard/2013/12/19/put-your-controllers-on-a-diet-posts-and-commands/)

-   **중재자 파이프라인이 포함 된 일반적인 문제를 수행 하는 작업량과**
    [*https://lostechies.com/jimmybogard/2014/09/09/tackling-cross-cutting-concerns-with-a-mediator-pipeline/*](https://lostechies.com/jimmybogard/2014/09/09/tackling-cross-cutting-concerns-with-a-mediator-pipeline/)

-   **CQRS 및 REST:와 완벽 하 게**
    [*https://lostechies.com/jimmybogard/2016/06/01/cqrs-and-rest-the-perfect-match/*](https://lostechies.com/jimmybogard/2016/06/01/cqrs-and-rest-the-perfect-match/)

-   **MediatR 파이프라인 예제**
    [*https://lostechies.com/jimmybogard/2016/10/13/mediatr-pipeline-examples/*](https://lostechies.com/jimmybogard/2016/10/13/mediatr-pipeline-examples/)

-   **MediatR 및 ASP.NET Core에 대 한 세로 조각 테스트 설비**
    *<https://lostechies.com/jimmybogard/2016/10/24/vertical-slice-test-fixtures-for-mediatr-and-asp-net-core/>*

-   **출시 Microsoft 종속성 주입을 위한 MediatR 확장**
    [*https://lostechies.com/jimmybogard/2016/07/19/mediatr-extensions-for-microsoft-dependency-injection-released/*](https://lostechies.com/jimmybogard/2016/07/19/mediatr-extensions-for-microsoft-dependency-injection-released/)

##### <a name="fluent-validation"></a>Fluent 유효성 검사

-   **Jeremy Skinner 합니다. FluentValidation 합니다.** GitHub 리포지토리 합니다.
    [*https://github.com/JeremySkinner/FluentValidation*](https://github.com/JeremySkinner/FluentValidation)

>[!div class="step-by-step"]
[이전] [다음]을 (microservice-application-layer-web-api-design.md) (... /implement-resilient-applications/index.md)
