# 개요
유니티 ECS & Job System 공부 목적용 저장소입니다.

### ECS & C# Job System & Burst Compiler?

- **ECS(엔티티 컴포넌트 시스템) :** 새로운 C# 잡 시스템은 간편하면서도 안전한 방식으로 멀티코어를 활용합니다. C# 스크립트를 활용하여 사용자가 빠르게 잡 코드를 작성할 수 있으므로 간편하며, 경합 조건과 같은 멀티스레딩의 일부 위험을 방지하므로 더욱 안전합니다.

- **C# Joyb System(멀티 스레딩 활용 API) :** ECS는 실제 해결해야 하는 문제, 즉 게임을 구성하는 데이터와 동작을 집중적으로 다루는 코드 작성 방식입니다.
ECS를 사용하면 디자인 측면에서 더 나은 방식의 게임 프로그래밍이 가능할 뿐 아니라, Unity의 C# 잡 시스템과 버스트 컴파일러를 더욱 효과적으로 이용하여 최첨단 멀티코어 프로세서의 기능을 빠짐없이 활용할 수 있습니다.
Unity는 ECS를 통해 오브젝트 중심의 디자인에서 데이터 중심의 디자인으로 이동하고자 합니다. 이러한 변화를 통해 사용자 간에 코드를 이해하고 작성에 참여하거나 코드를 재사용하기가 더욱 수월해질 것입니다.
- **Burst Compiler(고도의 최적화 컴파일러) :** 새로운 LLVM 기반의 연산 인식 백엔드 컴파일러 기술을 통해 C# 작업을 처리하고 고도로 최적화된 코드를 작성할 수 있습니다.
또한 ECS 기반의 코드라면 코드 수정을 크게 하지 않고 쉽게 적용할 수 있습니다.

- **ECS & JobSystem & Burst Compolier 퍼포먼스**
  - 오브젝트를 만개 단위로 뿌려 FPS 30까지 얼마나 많은 오브젝트를 동시 실행할 수 있는지


| 작동 방식 | 오브젝트 개수 | 유튜브 영상 링크 |
| :---: | :---: | :---: |
| Classic | 18500개 | [링크](https://youtu.be/WZ6-LxwxWEI?t=2m51s) |
| Classic & Job System | 25000개 | [링크](https://www.youtube.com/watch?v=j2z5KRWZTDA) |
| ECS & Job System | 96000개 | [링크](https://youtu.be/D1KShj8ZV_I?t=12s) |
| ECS & Job System & Burst Compolier | 156000개 | [링크](https://youtu.be/D1KShj8ZV_I?t=1m57s) |


- 출처 링크 - 유니티 홈페이지에 소개된 설명
  - https://unity3d.com/kr/unity/features/job-system-ECS

### ECS 개발 세팅
- Unity 2018.01 버젼 이상 설치
- Unity - PlayerSetting - API Compatibility Level - .net 3.x -> .net 4.x로 변경
- Unity 2018버젼부터 생긴 Package Manager - ECS 설치

- Visual Studio 최신으로 설치
( VS 버젼이 오래되었을 경우 Unity 컴파일러와 VS 컴파일러가 따로 구동되어 VS에서 ECS 관련 스크립트 작성 불가 )

- 추천 개발 세팅 비디오 - Brackeys / New way of CODING in Unity! ECS Tutorial
https://www.youtube.com/watch?v=_U9wRgQyy6s

### ECS 장단점

#### 장점

- 데이터 지향 개발을 사용하기 때문에, CPU 캐쉬메모리와 멀티 쓰레딩에 친화적이다. ( 성능 UP )
- 개발이 진행됨에 따라 생기는 예기치 못한 기능 추가 및 변경에 대해 보다 빠르게 대처할 수 있다.
- 코드의 상호 의존성이 없어 코드를 재사용하기 용이하다.
- 위와 마찬가지로 상호 의존성이 없어, 전체적으로 코드가 짧아져 코드 분석 및 유지보수가 용이하다.

![](https://pbs.twimg.com/media/DTOOMadX0AEdkVH.jpg)
사진 출처 유니티 테크니컬 에반젤리스트 Mike Geig 트윗 : https://twitter.com/mikegeig/status/951260836538003458

#### 단점

- 기존 객체 지향 개발 방식 **(OOP Object Oriented Develop)** 을 버리고 새로운 설계방식 **(DOP(Data Oriented Develop) - 데이터 지향 개발)** 을 사용해야 하기 때문에, 학습 및 개발 비용이 생긴다.
- 나온지 얼마 안된 기능으로, Research & Develop하기가 힘들다.
- Physics관련 기능을 ECS로 사용할 수 없다.

- 디버깅 관련
  - VS 디버그 - 중단점 기능을 지원하지 않는다. ( 시도 시 VS 멈춤 & 유니티 응답 없음 )
  - Classic처럼 게임을 중단하여 렌더링 된 오브젝트를 클릭 시 오브젝트를 선택할 수 없다. ( Entity Debug - Entity를 선택해야 한다. )
  - Job System을 사용할 경우 멀티 스레딩 방식이기에 디버깅하기가 더 힘들다.

#### 결론

- **장단점은 결국 OOP와 DOP의 차이점이다.**
- 모든 스크립트를 ECS 기반으로 설계하는 것은 비효율적이다.
  - 대규모 오브젝트(성능을 요하는) 혹은 UI(재사용을 요하는)부분에만 ECS로 구현하는것이 좋다.
  - 절차위주나 그 외 **OCP(Open Close Principle, OOP - 개방 폐쇄의 원칙)** 을 적용하기 용이한 경우 - 구체적으로 게임 규칙의 경우 기존의 MonoBehaviour & Scriptable Object 방식으로 구현하는 것이 좋다.


---
## ECS

### ECS 예시

#### Pure ECS
```csharp
using Unity.Collections;
using Unity.Entities;
using Unity.Jobs;
using Unity.Mathematics;
using Unity.Transforms;
using UnityEngine;

[System.Serializable]
public struct Rotater_Pure : IComponentData
{
    public float fSpeed;
}

public class Rotater_PureComponent : ComponentDataWrapper<Rotater_Pure> { }

public class RotaterSystem_Pure : JobComponentSystem
{
    struct RotationJob : IJobProcessComponentData<Rotation, Rotater_Pure>
    {
        public float fDeltaTime;

        public void Execute(ref Rotation sRotation, [ReadOnly]ref Rotater_Pure sRotater)
        {
            sRotation.Value = math.mul(math.normalize(sRotation.Value), quaternion.axisAngle(math.up(), sRotater.fSpeed * fDeltaTime));
        }
    }

    protected override JobHandle OnUpdate(JobHandle inputDeps)
    {
        var job = new RotationJob() { fDeltaTime = Time.deltaTime };
        return job.Schedule(this, 64, inputDeps);
    }
}
```

- Pure ECS의 경우 오브젝트에
Rendrer Component -> MeshInstanceRendererComponent
Transform Component -> Position & Rotation Component
를 Attach해야 동작합니다.

#### Hybrid ECS
```csharp
using System.Collections;
using System.Collections.Generic;
using Unity.Entities;
using UnityEngine;

public class Rotater : MonoBehaviour
{
    public float fSpeed;
}

class RotaterSystem : ComponentSystem
{
    struct Components
    {
        public Rotater rotater;
        public Transform transform;
    }

    protected override void OnUpdate()
    {
        float fDeltaTime = Time.deltaTime;
        foreach (var e in GetEntities<Components>())
        {
            e.transform.Rotate(0f, e.rotater.fSpeed * fDeltaTime, 0f);
        }
    }
}
```


#### Class & Interface

- ComponentDataWrapper

- ComponentSystem

- IComponentData

- ISharedComponentData


---
## Job System

---
## Burst Compiler


---
## 도움이 되는 링크

### Github

비디오를 시청 후 번호 순서대로 보시는 것을 추천합니다.

#### 1. Unity Tehcnologies / EntitiyComponentSystemSamples
- 유니티에서 제작한 ECS Sample 예제이며, 항상 최신버젼으로 업데이트됩니다.
  - https://github.com/Unity-Technologies/EntityComponentSystemSamples


#### 2. stella3d / job - system - cookbook
- 유니티에서 소개하는 C# Job System Sample 예제입니다.
  - https://github.com/stella3d/job-system-cookbook

#### 3. KptEmreU / Roll - A - Ball - ECS - style
- 어떤 개발자가 제작한 Pure ECS Style Roll a Ball 예제입니다.
  - https://github.com/KptEmreU/Roll-A-Ball-ECS-style

---
### Youtube Video

번호 순서대로 보시는 것을 추천합니다.

#### 1. Unity / Overview - Intro To The Entity Component System And C# Job System 1/5
- 유니티에서 ECS & Job System에 대해 개요부터 Classic ~ ECS & Job System 퍼포먼스 체크까지 설명합니다.
- 7~11여분 길이의 비디오가 총 5편으로, 코드보단 왜 ECS를 사용해야 하는지에 대해 초점이 맞춰져 있습니다.
  - https://www.youtube.com/watch?v=WLfhUKp2gag&list=PLX2vGYjWbI0S4yHZwjDI1boIrYStpBCdN

#### 2. Brackeys / New way of CODING in Unity! ECS Tutorial
- 유니티 및 개발 전문 방송 Brackeys에서 설명하는 ECS 튜토리얼입니다.
- 10분정도 길이이며, Hybrid ECS만 간단하게 설명합니다.
  - https://www.youtube.com/watch?v=_U9wRgQyy6s

#### 3. Infallible Code / Unity ECS Tutorial • Introduction
- 유니티 및 개발 전문 방송 Infallible Code에서 설명하는 ECS 튜토리얼 입니다.
- 10여분 정도 길이의 비디오가 총 7편으로, Hybrid ~ Pure ECS, Job System까지 슈팅게임 제작 기준으로 설명합니다.
  - https://www.youtube.com/watch?v=yzhsgaFVpZY
