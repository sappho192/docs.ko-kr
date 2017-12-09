---
title: "지 속성 계층 인프라를 디자인합니다."
description: "컨테이너 화 된.NET 응용 프로그램에 대 한.NET Microservices 아키텍처 | 지 속성 계층 인프라를 디자인합니다."
keywords: "Docker, 마이크로 서비스, ASP.NET, 컨테이너"
author: CESARDELATORRE
ms.author: wiwagn
ms.date: 05/26/2017
ms.prod: .net-core
ms.technology: dotnet-docker
ms.topic: article
ms.openlocfilehash: ce0f1d608eed909a7707f3c580afc5253f3eef06
ms.sourcegitcommit: bd1ef61f4bb794b25383d3d72e71041a5ced172e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/18/2017
---
# <a name="designing-the-infrastructure-persistence-layer"></a><span data-ttu-id="c246c-104">지 속성 계층 인프라를 디자인합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-104">Designing the infrastructure persistence layer</span></span>

<span data-ttu-id="c246c-105">데이터 지 속성 구성 요소는 마이크로 서비스 (즉, 마이크로 서비스의 데이터베이스)의 경계 내에서 호스팅되는 데이터에 대 한 액세스를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-105">Data persistence components provide access to the data hosted within the boundaries of a microservice (that is, a microservice’s database).</span></span> <span data-ttu-id="c246c-106">실제 구현은 저장소 등의 구성 요소로 포함 및 [작업 단위](http://martinfowler.com/eaaCatalog/unitOfWork.html) 사용자 지정 EF DBContexts 같은 클래스.</span><span class="sxs-lookup"><span data-stu-id="c246c-106">They contain the actual implementation of components such as repositories and [Unit of Work](http://martinfowler.com/eaaCatalog/unitOfWork.html) classes, like custom EF DBContexts.</span></span>

## <a name="the-repository-pattern"></a><span data-ttu-id="c246c-107">리포지토리 패턴</span><span class="sxs-lookup"><span data-stu-id="c246c-107">The Repository pattern</span></span>

<span data-ttu-id="c246c-108">저장소는 클래스 또는 데이터 원본에 액세스 하는 데 필요한 논리를 캡슐화 하는 구성 요소.</span><span class="sxs-lookup"><span data-stu-id="c246c-108">Repositories are classes or components that encapsulate the logic required to access data sources.</span></span> <span data-ttu-id="c246c-109">이러한 공통 데이터 액세스 기능을 유지 관리 작업이 편리를 제공 하 고 인프라 또는 도메인 모델 계층에서 데이터베이스에 액세스 하는 데 사용 되는 기술 분리 중앙 집중화 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-109">They centralize common data access functionality, providing better maintainability and decoupling the infrastructure or technology used to access databases from the domain model layer.</span></span> <span data-ttu-id="c246c-110">Entity Framework와 같은 ORM을 사용 하면 LINQ 및 강력한 형식 지정 덕분에 구현 해야 하는 코드 간소화 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-110">If you use an ORM like Entity Framework, the code that must be implemented is simplified, thanks to LINQ and strong typing.</span></span> <span data-ttu-id="c246c-111">이 데이터 보다 데이터 지 속성 논리에 집중할 수 있도록 내부 작업에 액세스 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-111">This lets you focus on the data persistence logic rather than on data access plumbing.</span></span>

<span data-ttu-id="c246c-112">저장소 패턴은 데이터 원본으로 작업의 자세한 설명이 포함된 하는 방법.</span><span class="sxs-lookup"><span data-stu-id="c246c-112">The Repository pattern is a well-documented way of working with a data source.</span></span> <span data-ttu-id="c246c-113">책에 [엔터프라이즈 응용 프로그램 아키텍처 패턴](https://www.amazon.com/Patterns-Enterprise-Application-Architecture-Martin/dp/0321127420/), Martin Fowler 다음과 같이 저장소를 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-113">In the book [Patterns of Enterprise Application Architecture](https://www.amazon.com/Patterns-Enterprise-Application-Architecture-Martin/dp/0321127420/), Martin Fowler describes a repository as follows:</span></span>

<span data-ttu-id="c246c-114">저장소는 중간자 도메인 모델 계층 및 데이터 매핑 비슷한 방식으로 메모리에 대 한 도메인 개체의 집합에 속하는 작업을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-114">A repository performs the tasks of an intermediary between the domain model layers and data mapping, acting in a similar way to a set of domain objects in memory.</span></span> <span data-ttu-id="c246c-115">선언적으로 클라이언트 개체 쿼리를 작성 하 대답에 대 한 저장소로 보내야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-115">Client objects declaratively build queries and send them to the repositories for answers.</span></span> <span data-ttu-id="c246c-116">개념적으로, 저장소 데이터베이스 및 지 속성 계층에 가까운 방법을 제공 하 여 수행할 수 있는 작업에 저장 된 개체의 집합을 캡슐화 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-116">Conceptually, a repository encapsulates a set of objects stored in the database and operations that can be performed on them, providing a way that is closer to the persistence layer.</span></span> <span data-ttu-id="c246c-117">버전 저장소는 또한 명확 하 고 한 방향으로 작업 도메인 및 데이터 할당 간 종속성 구분 기호 또는 매핑을의 목적을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-117">Repositories, also, support the purpose of separating, clearly and in one direction, the dependency between the work domain and the data allocation or mapping.</span></span>

### <a name="define-one-repository-per-aggregate"></a><span data-ttu-id="c246c-118">집계 마다 리포지토리가 한 개를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-118">Define one repository per aggregate</span></span>

<span data-ttu-id="c246c-119">각 집계 또는 집계 루트에 대 한 하나의 저장소 클래스를 만들어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-119">For each aggregate or aggregate root, you should create one repository class.</span></span> <span data-ttu-id="c246c-120">도메인 기반 디자인 패턴을 기반으로 하는 마이크로 서비스를 사용 하 여 데이터베이스를 업데이트 해야 하는 유일한 채널 리포지토리 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-120">In a microservice based on domain-driven design patterns, the only channel you should use to update the database should be the repositories.</span></span> <span data-ttu-id="c246c-121">즉, 집계의 고정 항목과 트랜잭션 일관성을 제어 하는 집계 루트와 일대일 관계를 갖습니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-121">This is because they have a one-to-one relationship with the aggregate root, which controls the aggregate’s invariants and transactional consistency.</span></span> <span data-ttu-id="c246c-122">통해 다른 데이터베이스를 쿼리할 수 채널 (CQRS 방법은 다음을 수행할 수 있습니다)으로, 쿼리 데이터베이스의 상태는 변경 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-122">It is okay to query the database through other channels (as you can do following a CQRS approach), because queries do not change the state of the database.</span></span> <span data-ttu-id="c246c-123">그러나 트랜잭션 영역-업데이트-저장소 및 집계 루트에 의해 제어 항상 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-123">However, the transactional area—the updates—must always be controlled by the repositories and the aggregate roots.</span></span>

<span data-ttu-id="c246c-124">기본적으로, 저장소 도메인 엔터티의 형태로 데이터베이스에서 제공 되는 메모리의 데이터를 채울 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-124">Basically, a repository allows you to populate data in memory that comes from the database in the form of the domain entities.</span></span> <span data-ttu-id="c246c-125">엔터티를 메모리에 있는, 변경 하 고 수 트랜잭션을 통해 데이터베이스에 다시 적용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-125">Once the entities are in memory, they can be changed and then persisted back to the database through transactions.</span></span>

<span data-ttu-id="c246c-126">앞에서 설명한 대로, CQS/CQRS 아키텍처 패턴을 사용 하는 경우 초기 쿼리 Dapper를 사용 하 여 간단한 SQL 문을 수행한 도메인 모델에서 쪽 쿼리에 의해 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-126">As noted earlier, if you are using the CQS/CQRS architectural pattern, the initial queries will be performed by side queries out of the domain model, performed by simple SQL statements using Dapper.</span></span> <span data-ttu-id="c246c-127">이 방법은 해야 쿼리 하 고 모든 테이블을 조인할 수 있으므로 저장소 보다 유연 하 고 이러한 쿼리는의 제한을 받지 규칙에 따라 집계에서 훨씬 더 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-127">This approach is much more flexible than repositories because you can query and join any tables you need, and these queries are not restricted by rules from the aggregates.</span></span> <span data-ttu-id="c246c-128">해당 데이터 프레젠테이션 계층 또는 클라이언트 응용 프로그램으로 이동 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-128">That data will go to the presentation layer or client app.</span></span>

<span data-ttu-id="c246c-129">사용자를 변경 하면 경우에 응용 프로그램 계층 (예: 웹 API 서비스)에 데이터를 업데이트할 수는 클라이언트 응용 프로그램 또는 프레젠테이션 계층에서 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-129">If the user makes changes, the data to be updated will come from the client app or presentation layer to the application layer (such as a Web API service).</span></span> <span data-ttu-id="c246c-130">명령 처리기에는 명령 (데이터)를 받으면 데이터베이스에서 업데이트할 데이터를 가져올 저장소를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-130">When you receive a command (with data) in a command handler, you use repositories to get the data you want to update from the database.</span></span> <span data-ttu-id="c246c-131">명령을 전달 되는 정보를 메모리에 업데이트 하 고을 추가 하거나 (도메인 엔터티) 트랜잭션을 통해 데이터베이스의 데이터를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-131">You update it in memory with the information passed with the commands, and you then add or update the data (domain entities) in the database through a transaction.</span></span>

<span data-ttu-id="c246c-132">해야 내용을 강조 다시는 그림 9-17과 같이 각 집계 루트에 대해 하나의 저장소를 정의 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-132">We must emphasize again that only one repository should be defined for each aggregate root, as shown in Figure 9-17.</span></span> <span data-ttu-id="c246c-133">집계 내 모든 개체 간에 트랜잭션 일관성을 유지 하기 위해 집계 루트의 목표를 달성 하기를 만들지 마십시오 각 테이블에 대 한 저장소 데이터베이스에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-133">To achieve the goal of the aggregate root to maintain transactional consistency between all the objects within the aggregate, you should never create a repository for each table in the database.</span></span>

![](./media/image18.png)

<span data-ttu-id="c246c-134">**그림 9-17**합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-134">**Figure 9-17**.</span></span> <span data-ttu-id="c246c-135">저장소, 집계 및 데이터베이스 테이블 간의 관계</span><span class="sxs-lookup"><span data-stu-id="c246c-135">The relationship between repositories, aggregates, and database tables</span></span>

### <a name="enforcing-one-aggregate-root-per-repository"></a><span data-ttu-id="c246c-136">저장소당 하나의 집계 루트를 적용합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-136">Enforcing one aggregate root per repository</span></span>

<span data-ttu-id="c246c-137">만 집계 루트 저장소에 있어야 하는 규칙을 적용 하는 방식으로 저장소 디자인을 구현 하려면 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-137">It can be valuable to implement your repository design in such a way that it enforces the rule that only aggregate roots should have repositories.</span></span> <span data-ttu-id="c246c-138">IAggregateRoot 표시 인터페이스 갖도록와 함께 작동 하는 엔터티 형식을 제한 하는 제네릭 또는 기본 저장소 유형을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-138">You can create a generic or base repository type that constrains the type of entities it works with to ensure they have the IAggregateRoot marker interface.</span></span>

<span data-ttu-id="c246c-139">따라서 각 저장소 인프라 계층에서 구현 또는 구현 하는 자체 계약 인터페이스를 다음 코드에 나와 있는 것 처럼:</span><span class="sxs-lookup"><span data-stu-id="c246c-139">Thus, each repository class implemented at the infrastructure layer implements its own contract or interface, as shown in the following code:</span></span>

```csharp
namespace Microsoft.eShopOnContainers.Services.Ordering.Infrastructure.Repositories
{
    public class OrderRepository : IOrderRepository
    {
```

<span data-ttu-id="c246c-140">각 특정 리포지토리 인터페이스 제네릭 IRepository 인터페이스를 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-140">Each specific repository interface implements the generic IRepository interface:</span></span>

```csharp
public interface IOrderRepository : IRepository<Order>
{
    Order Add(Order order);
    // ...
}
```

<span data-ttu-id="c246c-141">그러나 하는 규칙을 적용 하는 코드에 더 좋은 방법은 명시적 특정 집계를 대상으로 하는 데 저장소를 사용 하는 일반 저장소 유형을 구현 하는 것 단일 집계를 각 저장소는 관련 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-141">However, a better way to have the code enforce the convention that each repository should be related to a single aggregate would be to implement a generic repository type so it is explicit that you are using a repository to target a specific aggregate.</span></span> <span data-ttu-id="c246c-142">하는 다음 코드 에서처럼 IRepository 기본 인터페이스에서 해당 제네릭을 구현 하 여 쉽게 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-142">That can be easily done by implementing that generic in the IRepository base interface, as in the following code:</span></span>

```csharp
  public interface IRepository<T> where T : IAggregateRoot
```

### <a name="the-repository-pattern-makes-it-easier-to-test-your-application-logic"></a><span data-ttu-id="c246c-143">리포지토리 패턴을 사용 하면 쉽게 응용 프로그램 논리를 테스트 하려면</span><span class="sxs-lookup"><span data-stu-id="c246c-143">The Repository pattern makes it easier to test your application logic</span></span>

<span data-ttu-id="c246c-144">리포지토리 패턴을 사용 하면 단위 테스트 응용 프로그램을 쉽게 테스트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-144">The Repository pattern allows you to easily test your application with unit tests.</span></span> <span data-ttu-id="c246c-145">단위 테스트만 테스트 코드, 인프라가 아니라 때문에 저장소 추상화 쉽게 이러한 작업을 수행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-145">Remember that unit tests only test your code, not infrastructure, so the repository abstractions make it easier to achieve that goal.</span></span>

<span data-ttu-id="c246c-146">이전 단원에서 설명 했 듯이 것이 좋습니다 정의 하 고 배치 리포지토리 인터페이스 도메인 모델 계층에서 응용 프로그램 계층 (사용자 웹 API 마이크로 서비스 예를 들어,) 인프라 계층 직접에 종속 되지 않아야 하므로 있는 경우 실제 저장소 클래스를 구현 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-146">As noted in an earlier section, it is recommended that you define and place the repository interfaces in the domain model layer so the application layer (for instance, your Web API microservice) does not depend directly on the infrastructure layer where you have implemented the actual repository classes.</span></span> <span data-ttu-id="c246c-147">이 작업을 수행 하 고 Web API 컨트롤러의 종속성 주입을 사용 하 여 데이터베이스에서 데이터 대신 가짜 데이터를 반환 하는 모의 리포지토리를 구현할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-147">By doing this and using Dependency Injection in the controllers of your Web API, you can implement mock repositories that return fake data instead of data from the database.</span></span> <span data-ttu-id="c246c-148">분리 된 접근 방식을 통해 만들 수 있습니다 및 실행된 단위 테스트는 데이터베이스에 연결 하지 않고도 응용 프로그램 논리를 테스트할 수입니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-148">That decoupled approach allows you to create and run unit tests that can test just the logic of your application without requiring connectivity to the database.</span></span>

<span data-ttu-id="c246c-149">데이터베이스에 대 한 연결이 실패할 수 하며, 무엇 보다도 데이터베이스에 대해 수백 번의 테스트를 실행 합니다. 잘못 된 두 가지 이유로 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-149">Connections to databases can fail and, more importantly, running hundreds of tests against a database is bad for two reasons.</span></span> <span data-ttu-id="c246c-150">먼저, 테스트 수가 많은 때문에 시간이 많이 걸릴 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-150">First, it can take a lot of time because of the large number of tests.</span></span> <span data-ttu-id="c246c-151">둘째, 데이터베이스 레코드 변경 하 고 일관 되지 않을 수도 있습니다 수 있도록 테스트, 결과 영향을 줄 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-151">Second, the database records might change and impact the results of your tests, so that they might not be consistent.</span></span> <span data-ttu-id="c246c-152">데이터베이스에 대해 테스트가 단위 테스트 하지만 통합 테스트 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-152">Testing against the database is not a unit tests but an integration test.</span></span> <span data-ttu-id="c246c-153">많은 단위 테스트를 신속 하 고 실행 해야 하지만 더 적은 통합 데이터베이스에 대해 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-153">You should have many unit tests running fast, but fewer integration tests against the databases.</span></span>

<span data-ttu-id="c246c-154">단위 테스트에 대 한 문제의 분리, 측면에서 논리에는 메모리의 엔터티를 도메인에서 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-154">In terms of separation of concerns for unit tests, your logic operates on domain entities in memory.</span></span> <span data-ttu-id="c246c-155">저장소 클래스가 전달한 것을 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-155">It assumes the repository class has delivered those.</span></span> <span data-ttu-id="c246c-156">도메인 엔터티를 수정 하는 논리 되 면 저장소 클래스 올바르게 저장할 됩니다 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-156">Once your logic modifies the domain entities, it assumes the repository class will store them correctly.</span></span> <span data-ttu-id="c246c-157">여기서 중요 한 점은 도메인 모델 및 해당 도메인 논리에 대 한 단위 테스트를 만드는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-157">The important point here is to create unit tests against your domain model and its domain logic.</span></span> <span data-ttu-id="c246c-158">집계 루트는 ddd에서 주 일관성 경계입니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-158">Aggregate roots are the main consistency boundaries in DDD.</span></span>

### <a name="the-difference-between-the-repository-pattern-and-the-legacy-data-access-class-dal-class-pattern"></a><span data-ttu-id="c246c-159">리포지토리 패턴 및 레거시 데이터 액세스 클래스 (DAL 클래스) 패턴의 차이</span><span class="sxs-lookup"><span data-stu-id="c246c-159">The difference between the Repository pattern and the legacy Data Access class (DAL class) pattern</span></span>

<span data-ttu-id="c246c-160">데이터 액세스 개체는 직접 데이터 액세스 및 지 속성 저장소에 대 한 작업을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-160">A data access object directly performs data access and persistence operations against storage.</span></span> <span data-ttu-id="c246c-161">작업 개체 (예: EF DbContext를 사용 하는 경우), 하지만 이러한 업데이트는 단위의 메모리에서 수행 하려는 작업을 사용 하 여 데이터를 즉시 수행 하지 것입니다. 리포지토리 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-161">A repository marks the data with the operations you want to perform in the memory of a unit of work object (as in EF when using the DbContext), but these updates will not be performed immediately.</span></span>

<span data-ttu-id="c246c-162">작업의 단위를 단일 트랜잭션으로 여러 insert와 관련 된 업데이트 또는 삭제 작업 이라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-162">A unit of work is referred to as a single transaction that involves multiple insert, update, or delete operations.</span></span> <span data-ttu-id="c246c-163">간단히 말해서에서는 특정 사용자 동작 (예를 들어 웹 사이트에서 등록)에 대 한 모든 삽입, 업데이트 및 삭제 트랜잭션이 처리 단일 트랜잭션에서 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-163">In simple terms, it means that for a specific user action (for example, registration on a website), all the insert, update, and delete transactions are handled in a single transaction.</span></span> <span data-ttu-id="c246c-164">이것이 chattier 방식으로 여러 데이터베이스 트랜잭션 처리 보다 더 효율적입니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-164">This is more efficient than handling multiple database transactions in a chattier way.</span></span>

<span data-ttu-id="c246c-165">이 코드는 응용 프로그램 계층에서 명령 시 이러한 여러 지 속성 작업에서는 단일 작업에서 나중에 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-165">These multiple persistence operations will be performed later in a single action when your code from the application layer commands it.</span></span> <span data-ttu-id="c246c-166">에 따라 일반적으로 실제 데이터베이스 저장소에 메모리에 변경 내용을 적용 하는 방법에 대 한 결정은 [작업 단위로 패턴](http://martinfowler.com/eaaCatalog/unitOfWork.html)합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-166">The decision about applying the in-memory changes to the actual database storage is typically based on the [Unit of Work pattern](http://martinfowler.com/eaaCatalog/unitOfWork.html).</span></span> <span data-ttu-id="c246c-167">EF, 작업 단위로 패턴으로 DBContext 구현 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-167">In EF, the Unit of Work pattern is implemented as the DBContext.</span></span>

<span data-ttu-id="c246c-168">대부분의 경우에서이 패턴 또는 저장소에 대 한 작업을 적용 한 방식으로 응용 프로그램 성능을 향상 하 고 불일치 가능성을 줄일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-168">In many cases, this pattern or way of applying operations against the storage can increase application performance and reduce the possibility of inconsistencies.</span></span> <span data-ttu-id="c246c-169">또한, 의도 된 모든 작업은 한 트랜잭션으로의 일부로 커밋할 때문에 트랜잭션 데이터베이스 테이블에 차단 줄어듭니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-169">Also, it reduces transaction blocking in the database tables, because all the intended operations are committed as part of one transaction.</span></span> <span data-ttu-id="c246c-170">이 데이터베이스에 대해 많은 격리 된 작업 실행에 비해 더 효율적입니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-170">This is more efficient in comparison to executing many isolated operations against the database.</span></span> <span data-ttu-id="c246c-171">따라서 선택한 ORM 많은 소형 및 별도 트랜잭션 실행 하는 대신 동일한 트랜잭션 내에서 여러 가지 업데이트 작업을 그룹화 하 여 데이터베이스에 대해 실행을 최적화할 수 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-171">Therefore, the selected ORM will be able to optimize the execution against the database by grouping several update actions within the same transaction, as opposed to many small and separate transaction executions.</span></span>

### <a name="repositories-should-not-be-mandatory"></a><span data-ttu-id="c246c-172">저장소를 필수적 이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-172">Repositories should not be mandatory</span></span>

<span data-ttu-id="c246c-173">사용자 지정 저장소는 앞에서 언급 된 이유로 유용 있고 eShopOnContainers에 정렬 마이크로 서비스에 대 한 접근 방식입니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-173">Custom repositories are useful for the reasons cited earlier, and that is the approach for the ordering microservice in eShopOnContainers.</span></span> <span data-ttu-id="c246c-174">그러나 DDD 디자인에서 구현 하거나 일반적에.NET에서 개발 하는 필수 패턴 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-174">However, it is not an essential pattern to implement in a DDD design or even in general development in .NET.</span></span>

<span data-ttu-id="c246c-175">예를 들어, Jimmy Bogard이이 가이드에 대 한 직접적인 피드백을 제공 하는 경우 다음 말합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-175">For instance, Jimmy Bogard, when providing direct feedback for this guide, said the following:</span></span>

<span data-ttu-id="c246c-176">이 가장 큰 의견 되었을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-176">This’ll probably be my biggest feedback.</span></span> <span data-ttu-id="c246c-177">실제로 하지 않겠습니다 리포지토리의 팬 기본 지 속성 메커니즘의 중요 한 세부 정보를 숨기려면 기능이 있기 때문에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-177">I’m really not a fan of repositories, mainly because they hide the important details of the underlying persistence mechanism.</span></span> <span data-ttu-id="c246c-178">그 이유를 얻을 수 MediatR 명령에 대 한 너무 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-178">It’s why I go for MediatR for commands, too.</span></span> <span data-ttu-id="c246c-179">지 속성 계층의 모든 기능을 사용할 수 있으며 모든 도메인 동작 내 집계 루트에 밀어 I.</span><span class="sxs-lookup"><span data-stu-id="c246c-179">I can use the full power of the persistence layer, and push all that domain behavior into my aggregate roots.</span></span> <span data-ttu-id="c246c-180">내 저장소의 모의 알림 않으려는 일반적으로 – 여전히 해당 통합에 필요한 실제 항목을 사용 하 여 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-180">I don’t usually want to mock my repositories – I still need to have that integration test with the real thing.</span></span> <span data-ttu-id="c246c-181">CQRS 라인으로 전환 하지 않은 실제로 있는지 저장소에 대 한 요구가 더 이상 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-181">Going CQRS meant that we didn’t really have a need for repositories any more.</span></span>

<span data-ttu-id="c246c-182">저장소 유용 하 게 알 승인할 우리는 프로그램 DDD, 집계 패턴 및 풍부한 도메인 모델은 하는 방식에 중요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-182">We find repositories useful, but we acknowledge that they are not critical for your DDD, in the way that the Aggregate pattern and rich domain model are.</span></span> <span data-ttu-id="c246c-183">따라서 리포지토리 패턴 사용할지, 나와 있는 것 처럼 적합 합니다.</span><span class="sxs-lookup"><span data-stu-id="c246c-183">Therefore, use the Repository pattern or not, as you see fit.</span></span>

#### <a name="additional-resources"></a><span data-ttu-id="c246c-184">추가 리소스</span><span class="sxs-lookup"><span data-stu-id="c246c-184">Additional resources</span></span>

##### <a name="the-repository-pattern"></a><span data-ttu-id="c246c-185">리포지토리 패턴</span><span class="sxs-lookup"><span data-stu-id="c246c-185">The Repository pattern</span></span>

-   <span data-ttu-id="c246c-186">**Edward Hieatt 및 Rob me입니다. 리포지토리 패턴입니다. ** 
     [ *http://martinfowler.com/eaaCatalog/repository.html*](http://martinfowler.com/eaaCatalog/repository.html)</span><span class="sxs-lookup"><span data-stu-id="c246c-186">**Edward Hieatt and Rob Mee. Repository pattern.**
[*http://martinfowler.com/eaaCatalog/repository.html*](http://martinfowler.com/eaaCatalog/repository.html)</span></span>

-   <span data-ttu-id="c246c-187">**리포지토리 패턴**
    [*https://msdn.microsoft.com/en-us/library/ff649690.aspx*](https://msdn.microsoft.com/en-us/library/ff649690.aspx)</span><span class="sxs-lookup"><span data-stu-id="c246c-187">**The Repository pattern**
[*https://msdn.microsoft.com/en-us/library/ff649690.aspx*](https://msdn.microsoft.com/en-us/library/ff649690.aspx)</span></span>

-   <span data-ttu-id="c246c-188">**리포지토리 패턴: 데이터 지 속성 추상화**
    [*http://deviq.com/repository-pattern/*](http://deviq.com/repository-pattern/)</span><span class="sxs-lookup"><span data-stu-id="c246c-188">**Repository Pattern: A data persistence abstraction**
[*http://deviq.com/repository-pattern/*](http://deviq.com/repository-pattern/)</span></span>

-   <span data-ttu-id="c246c-189">**Eric Evans. 도메인 기반 디자인: 소프트웨어 핵심에서 복잡성을 다루는 합니다.**</span><span class="sxs-lookup"><span data-stu-id="c246c-189">**Eric Evans. Domain-Driven Design: Tackling Complexity in the Heart of Software.**</span></span> <span data-ttu-id="c246c-190">(예약; 리포지토리 패턴에 대 한 설명이 포함) [ *https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215/*](https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215/)</span><span class="sxs-lookup"><span data-stu-id="c246c-190">(Book; includes a discussion of the Repository pattern) [*https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215/*](https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215/)</span></span>

##### <a name="unit-of-work-pattern"></a><span data-ttu-id="c246c-191">작업 패턴의 단위</span><span class="sxs-lookup"><span data-stu-id="c246c-191">Unit of Work pattern</span></span>

-   <span data-ttu-id="c246c-192">**Martin Fowler. 단위 작업 패턴입니다. ** 
     [ *http://martinfowler.com/eaaCatalog/unitOfWork.html*](http://martinfowler.com/eaaCatalog/unitOfWork.html)</span><span class="sxs-lookup"><span data-stu-id="c246c-192">**Martin Fowler. Unit of Work pattern.**
[*http://martinfowler.com/eaaCatalog/unitOfWork.html*](http://martinfowler.com/eaaCatalog/unitOfWork.html)</span></span>

<!-- -->

-   <span data-ttu-id="c246c-193">**ASP.NET MVC 응용 프로그램에서 저장소 및 단위 작업 패턴의 구현은**
    [*https://www.asp.net/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/ implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application*](https://www.asp.net/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)</span><span class="sxs-lookup"><span data-stu-id="c246c-193">**Implementing the Repository and Unit of Work Patterns in an ASP.NET MVC Application**
[*https://www.asp.net/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application*](https://www.asp.net/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)</span></span>


>[!div class="step-by-step"]
<span data-ttu-id="c246c-194">[이전] (도메인-이벤트-디자인-implementation.md) [다음] (infrastructure-persistence-layer-implemenation-entity-framework-core.md)</span><span class="sxs-lookup"><span data-stu-id="c246c-194">[Previous] (domain-events-design-implementation.md) [Next] (infrastructure-persistence-layer-implemenation-entity-framework-core.md)</span></span>