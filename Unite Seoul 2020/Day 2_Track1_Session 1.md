# 최대 성능을 위한 최적화 팁

## Script Code 작성 팁

### Object Pooling
- 다수 동일한 객체를 자주 생성/삭제가 발생하면 GC의 동작이 많아지게 되고 이는 곧 전체 CPU 사용량에 부정적인 영향을 주게 된다.
- Object Pool을 통해 게임 내에서 사용할 객체를 미리 충분한 개수만큼 생성해 두었다 사용이 필요한 순간 꺼내서 사용을 하고 사용이 끝나면 Object Pool에 반납을 하는 시스템. 매번 생서아거나 삭제하지 않고 재활용하기 때문에 GC의 동작횟수를 줄이는 데 긍정적이고 메모리 단편화 예방에도 도움이 된다.
- Pool의 크기 결정이 중요하며, Pool의 크기를 넘는 개수를 사용해야 되는 상황에 대한 보조적인 조치가 필요하다.

### Scriptable Object
- 게임에서 변하지 않는 데이터를 위한 클래스를 작성하는 경우 MonoBehaviour 보다 Scriptable Object를 상속하여 객체를 작성하는 것을 추천한다.
- MonoBehaviour에 비해서 메모리 사용량도 적고 불필요한 (Start(), Update() 같은) 프로세스 콜을 줄일 수 있다.
- MonoBehaviour과 같이 Inspector 사용 가능하고 별도로 aseet 으로 저장하여 관리가 가능하다.
- Static 객체처럼 사용이 가능하기 때문에 Data 객체로 참조하여 Inspector 에 노출된 변수의 값을 변경하면 동일한 객체를 참조하고 있는 모든 클래스에서 동일하게 변경된 값을 사용할 수 있다.

### Variables / Properties
    #if DEVELOPMENT_BUILD/*사용자 커스텀 Define*/
      private int health;
      public int Health { get { return health; } }
    #else
      public int Health;
    #endif
- 단순한 getter / setter 형태로 사용하는 프로퍼티 사용 보다 변수를 직접 사용하는 것을 권장한다.
- 프로퍼티도 함수콜을 사용하기 때문에 스택 프레임에서 오버헤드가 발생한다.
  Start() 와 같이 1회 또는 적은 수의 호출이라면 큰 문제가 되지 않겠지만 Update()와 같은 루프문에서 잦은 호출이 일어나면 불필요한 오버헤드가 발생하게 된다.

### Resources 폴더
- Resources 폴더에 있는 모든 리소스가 사용 여부와 관계 없이 패키징 된다.  
  (최종 패키지의 용량 낭비)
- Resources 폴더에 있는 리소스의 개수는 앱의 구동 시간에 영향을 준다.  
  앱 구동시에 Resources 폴더 내의 모든 리소스를 탐색하여 Lookup 테이블을 작성하는데 이로 인해 리소스의 개수가 많다면 최초 구동이 느려지게 되고 또 Lookup 테이블이 차지하는 메모리 용량도 증가하게 된다.
- 가능하면 Addressable Assets 패키지를 활용하여 AssetBundle을 활용하는 것을 추천한다.

### 빈 Unity 이벤트 함수 삭제
- void Start() {}, void Update() {} 등..
- Unity 이벤트 함수는 비어 있어도 선언되어 있다면 내부의 각 이벤트 컨테이너에 추가되고 이벤트 발생시에 매번 호출되기 때문에 오버헤드가 발생한다.
- 스크립트의 개수보다 생성된 컴포넌트의 개수가 더 영향을 주기 때문에 게임 내에 생성된 컴퍼넌트 내에 사용되지 않는 이벤트 함수가 선언되어 있다면 상당히 많은 이벤트 콜에 의한 오버헤드가 발생하기 때문에 의외로 상한 비용을 소모하는 경우가 있을 수 있다.
- Release 에서는 별다른 구현이 없지만 디버깅 등의 용도로 사용하는 경우 define 분기를 통해 최종 빌드에서 제외될 수 있도록 관리하는 것을 권장한다.

### Awake() / Start() 함수는 최대한 가볍게
- 무거운 Awake() / Start() 는 객체의 초기화를 지연시키게 되고, 결국 Scene을 초기 동작을 지연시키게 된다.
- Unity의 처음 렌더링 결과물이 스크린에 표시되는 시점이 Scene의 모든 Object의 Start() 호출이 완료된 이후이기 때문에 초기화의 지연은 결국 Scene을 처음 띄우는 시간의 지연으로 이어진다.
- 무거운 초기화 로직은 가능하면 Update 내의 별도의 Finite State Machine 등을 통해서 별도의 초기화 구간을 구성하여 진행하는 것으로 권장한다.

### Hierarchy 복잡성을 줄여라
- Hierarchy의 계층 구조에서 Root GameObject는 모든 하위 오브젝트의 Transform 데이터를 배열로 들고 있다.
    - 조그마한 Transform 변화에도 하위의 모든 계층에 대한 Transform 계산을 다시 하기 때문에 가급적 Transform 변화가 있는 오브젝트를 별도의 트리로 관리하는 것이 좋다.
    - Hierarchy 구조가 변경되는 내부적으로 Root 오브젝트의 Transform 배열의 메모리가 재할당이 일어나며 모든 배열 값을 다시 설정된다. 이로 인한 GC 가 발생하기 때문에 Hierarchy 구조는 변경하지 않는 것이 좋다.
- 계층적 구조가 필요한 경우가 아니라면 가능하면 Root에 GameObject를 배치하는 것이 성능면에서 가장 좋다.
    - 탐색기의 폴더 방식으로 오브젝트를 관리하기 위한 트리 구조는 지양하는 것이 좋다.
    
### 참조 변수 캐싱
- GameObject.Find()
    - 생성되어 있는 모든 GameObject를 순회하며 조건 검사를 진행하기 때문에 상당한 오버헤드가 발생한다. 가능하면 직접 참조하거나 초기화 시점에 검색하여 검색결과를 캐싱하여 사용하자.
- GameObject.GetComponent(), GameObject.GetComponentChildren() 등
    - 마찬가지로 GameObject 내의 모든 컴퍼넌트에 대하여 검사를 수행하기 때문에 가능하면 미리 참조해두거나 초기화 시점에 캐싱하여 사용하자.
- 검사 대상도 많고 비교 검사 자체가 문자열 조건을 비교하기 때문에 비교식에 대한 오버헤드가 높은 편이다. 그렇기 때문에 자주 사용하는 것은 전체 성능에 부정적인 영향을 준다.

## Asset Import

### Texture Import Settings
- Generate Mipmaps
    - 밉맵이 필욯지 않는 텍스쳐의 경우 이 옵션을 해제하면 리소스 용량을 약 24% 정도 줄일 수 있다.
- Format
    - 디바이스 하드웽에서 지원하는 압축 포맷을 선택하게 되면 설정에 따라 다소 차이는 있지만 무압축 대비 평균적으로 1/4로 줄어든다.
    - 모바일의 경우 OpenGLES 3 이상인 경우 ETC2 / ASTC 포맷이 표준 규격으로 지정되어 있다.
      (ETC2는 ES 3.0, ASTC는 ES 3.1)
- compression
    - 옵션에 따라 텍스쳐의 용량과 퀄리티에 차이가 발생한다.
    - 동일한 설정이라도 플랫폼에 따라 결과가 다르기 때문에 변경 후 결과를 각 플랫폼별로 확인할 필요가 있다.
- POT 지정 또는 Atlas 사용
    -iOS의 PVRTC 포맷의 경우 강제로 POT 상태로 텍스처가 변경되면서 의도치 않은 낭비가 발생할 수 있다. 명시적으로 POT 값을 지정하여 잘못뵌 변경이 발생하지 않도록 한다.
    - 일부 플랫폼의 경우 POT 옵션이 지정되어 있지 않은 상태에서 Mipmap 설정을 하게 되면 Texture Compression Format이 유지가 되지 않는 경우가 있다. 이는 에디터에서 경고로 표시하고 있기 때문에 경고가 보이면 바로 해결하는 것이 좋다.
- 불필요시 알파 채널 삭제
    - 압축 포맷을 사용하는 알파채널 유/무는 용량에는 영향을 주지 않지만 압축 후의 이미지 퀄리티 손실을 감소시켜 준다.
- Read/Write Enable 해제
    - Read/Write Enable을 선택하면 비디오 메모리에서 읽기 버퍼 외에 쓰기용 버퍼를 추가로 할당하기 때문에 메모리 사용량이 2배가 된다.

### Mesh Import Setting
- Mesh Compression
    - Mesh의 Shape에 영향을 줄 수 있기 때문에 출력물을 확인하여 원본과 크게 다르지 않는 수준에서 최대한 높은 압축 옵션을 선택하는 것을 권장한다.
    - 패키지의 용량을 줄어들지만 Runtime 메모리에는 영향이 없다.
- Read/Write Enabled
     - Write용 버퍼를 추가로 할당하기 때문에 메모리 사용량이 2배가 된다.
- Blendshapes
- Normals and Tangetts
    - 불필요한 경우 해제하면 사용하지 않는 데이터가 제거되어 Mesh 데이터의 크기를 줄일 수 있다.
- Rig
    - 애니메이션을 사용하지 않는 경우 Animation Type을 None으로 설정한다.

### Audio Import Settings
- 스테레오가 아닌 것이 사용 가능하면 Force To Mono를 사용
- 가능한 Compression bitrate를 낮추자
    - 퀄리티 손상을 체크하면서 가능한 bitrate를 낮추면 그만큼 오디오파일의 크기가 작아져 패키지 용량과 메모리 용량을 아낄 수 있다.
- 추천 포맷
    - iOS: ADPCM (단순한 효과음), MP3 (배경음)
    - Android: Vorbis
- Audio Clip 크기 별 Load Type 추천 가이드
    - 200kb 미만: Decompress On Load
    - 200kb 이상: Compressed In Memory
    - 1mb이상의 큰 파일: Streaming
    - 압축 해제를 위한 버퍼가 200kb가 할당되기 때문에 200kb 미만은 그냥 압축을 풀어서 Load하는 편이 메모리 사용량을 절약할 수 있다.
    - [관련 매뉴얼](https://docs.unity3d.com/Manual/class-AudioClip.html)
- Mute (볼륨 0) 상태에서는 오디오 재생을 중지할 것을 권장한다.
    - 볼륨이 0이어도 내부적으로 오디오 재생 프로세스는 동작하고 있기 때문에 재생을 중지하여 불필요한 CPU 사용 및 메모리 할당을 막을 수 있다.
    - 추가로 가능하면 게임 옵션의 Mute 옵션이 활성화되면 기존의 Load되어 있는 Audio Clip 을 Unload 하여 메모리를 사용량을 절약하는 것을 추천한다.

## 그래픽스

### Bathcing
- 렌더링 비용 중 하나인 DrawCall 횟수를 줄이기 위해서는 가능하면 같은 Texture/Material을 사용하여 Batching이 성공적으로 이루어질 수 있도록 리소스를 디자인한다.
  Atlas Texture 사용을 권장한다.
- Dynamic Batching는 900개 이하의 Vertex를 가진 오브젝트를 대상으로 Position, Normal, UV0, U1, Tangent 등을 계산하며, 한 프레임당 한번 씩 평가된다. Batching 결과를 프로파일링 하여 Batching 성공률이 낮다면 Dynamic Batching 평가에 대한 CPU 비용을 줄이기 위해서 옵션을 비활성화하는 것도 좋은 방법이다.
- Static Batching은 Static Flag가 활성화된 정적 오브젝트를 대상으로 Build 타임에 Batching 평가가 수행된다. 만약 Runtime에 동적으로 Mesh를 추가하려면 StaticBatchingUtility.CombineAPI를 참고하여 Script를 통해 직접 구현하여야 한다.

## UI

## Q&A
