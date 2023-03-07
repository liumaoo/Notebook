# UE Actor 分析

## Actor

Actor更像是一个容器，里面由各种组件（Component）组成。

## Component

### USceneComponent

如果要在3D世界中放置一个对象，需要一个Transform表示其位置信息。UE4把Transform封装进了SceneComponent，当作RootComponent。因为还有很多非具象化的Actor，例如GameMode，是没有位置信息的。

Actor间的父子关系由SceneComponent确定，在Actor中的众多Component中，只有包含SceneComponent的Component具有嵌套功能。例如一辆车子，作为一个Actor，车身的SceneComponent作为根组件，四个轮子的SceneComponent嵌套于车身的SceneComponent。SceneComponent提供了方法用于SceneComponent之间的嵌套。

```c++
//用在构造函数中或Componet还没有被注册的时候，其他情况用AttachToComponent()
void SetupAttachment(USceneComponent* InParent, FName InSocketName = NAME_None);
//例如：
UPROPERTY()
UStaticMeshComponent* ItemMeshCom; //子网格体

AMyActor::AMyActor()
{
    ItemMeshCom = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("ItemMeshCom"));
    ItemMeshCom->SetupAttachment(RootComponent);//将创建的StaticMeshComponent嵌套到根组件中
    ItemMeshCom->SetCollisionProfileName(UCollisionProfile::NoCollision_ProfileName);//设置碰撞信息名字（会存储在Config\DefaultEngine.ini的Collision中）
    //UCollisionProfile::NoCollision_ProfileName为FName类型，NoCollision没有碰撞
}

bool AttachToComponent(USceneComponent* InParent, const FAttachmentTransformRules& AttachmentRules, FName InSocketName = NAME_None );
```



### UPrimitiveComponent

在UE4中，每个被渲染的物体都需要继承自UPrimitiveComponent。当然UStaticMeshComponent是继承自UPrimitiveComponent的，UPrimitiveComponent也是继承自USceneComponent的。

UPrimitiveComponent中有个变量是BodyInstance，他包含了相关的物理场景信息，含有一个多形状的刚体。具体作用就是我们经常设置的碰撞信息，特别是在StaticMesh或者添加CollisionBox等设置碰撞信息。

```c++
UPROPERTY()
UStaticMeshComponent* BoxMeshCom; //外部碰撞盒

AMyActor::AMyActor()
{
    BoxMeshCom = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("BoxMeshCom"));
    RootComponent = BoxMeshCom; //将新创建的静态网格体作为根组件
    BoxMeshCom->SetCollisionProfileName(TEXT("ShootTarget")); //设置碰撞信息名字（会存储在Config\DefaultEngine.ini的Collision中）
    BoxMeshCom->SetSimulatePhysics(true);  //设置模拟物理开启
}
```

