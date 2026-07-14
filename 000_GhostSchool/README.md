# 귀문학원

안드로이드 환경에서 구현한 2D 횡스크롤 달리기 게임

## 프로젝트 설명

3인 팀프로젝트로 진행되었으며, 개발 1명 아트 2명으로 구성됨.

### 설치

School.apk 파일을 핸드폰에 직접 설치


### 코딩스타일

유니티 자체 라이브러리인 리지드바디 2D와 콜라이더로 주인공 캐릭터 및 오브젝트 구현

주인공의 체력 상태와 특별한 상태 ( 공중부양, 무적) 등은 모두 주인공 캐릭터 cs에서 처리

```
    public int jumpForce = 700;
    public int jumpcount = 0;
    public int hp = 3;
    private bool oneHit = false;
    private bool twoHit = false;
    private bool healCycle = false;
    private bool oneFall = false;
    private bool twoFall = false;
    private bool isGround = true;
    private bool isRising = false;
    private bool isPower = false;
    public bool isSlide = false;
    public bool inputJump = false;
```

void update에서 캐릭터 상태변화를 계속 감지하고 실시간으로 애니메이션 등을 변화시킴
```
 void Update()
    {
        if(stopScroll.speed == 0)
        {
            transform.position = Vector3.Lerp(transform.position, new Vector2(6, transform.position.y), 0.01f);
            Invoke("ToEnding", 3);
        }
        ...
        ...
        ...
        if( isSlide == true && isGround == true)
        {
            capsuleCollider2D.offset = new Vector2(0, -0.16f);
            capsuleCollider2D.direction = CapsuleDirection2D.Horizontal;
            capsuleCollider2D.size = new Vector2(0.4f, 0.3f);
        }
        else
        {
            capsuleCollider2D.offset = new Vector2(0, 0);
            capsuleCollider2D.direction = CapsuleDirection2D.Vertical;
            capsuleCollider2D.size = new Vector2(0.4f, 0.6f);
        }
    }

```
씬 전환과 플레이어 캐릭터가 무적시간에 추락하지 않게 하는 오브젝트 생성 시간 등은 플레이어 캐릭터의 현재 상태에 영향을 받음 ( OnTriggerEnter )
```
    void Heal()
    {
        hp += 1;
        healCycle = true;
    }
    void NotFall()
    {
        notfallObj.SetActive(true);
        Invoke("OffNotFall", 3);
    }
    void OffNotFall()
    {
        notfallB = true;
        Invoke("QuitNotFall", 3);
    }
    void QuitNotFall()
    {
        notfallObj.SetActive(false);
    }

    void ToEnding()
    {
        SceneManager.LoadScene("03.Clear");
    }

```


## 사용된 기술

* Unity - 개발툴
* C# - 개발언어

## 제작자

* **강동형** - 전체 코드 작업, 화면구성 및 UI구성
* **조유나** - 아트작업
* **김가람** - 아트작업