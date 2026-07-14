# 겐지맨

PC 환경에서 구현한 2D + 3D 게임

## 프로젝트 설명

- 1인 개발이며, 도트, 3D, 애니메이션은 에셋사용. 일부 부족한 에셋은 직접 제작했습니다.
- 스테이지를 진행해나가며, 마지막에 있는 보스캐릭터를 물리지는 것이 목표
- 3개의 스테이지로 구성했으며, 각 파트가 지날때마다 죽은 지점부터 다시 시작하도록 구현했습니다.

### 설치

빌드파일을 다운로드받은 후 PortPolio.exe를 실행

### 코딩 스타일

Buttons라는 컴포넌트에 모든 버튼 기능을 구현하고 버튼 기능이 필요할때마다 불러와서 각 기능에 맞는 함수를 실행함

```
public void StartButton()
{
    GameManager.Instance.GoStage_01();
}

public void OnHowToPlayButton()
{
    howtoplay.SetActive(true);
}

public void OffHowToPlayButton()
{
    howtoplay.SetActive(false);
}

public void MainMenuButton()
{
    GameManager.Instance.GoMainMenu();
}

public void EndGame()
{
    GameManager.Instance.QuitGame();
}

public void SlightlyFadeIn()
{
    animator.SetTrigger("OnClick");
}

public void Again()
{
    GameManager.Instance.TryAgian();
}
```

플레이어 캐릭터는 인스턴스화하여 다른 곳에서 쉽게 접근할 수 있도록 만듬

```
// 인스턴스화
// Unity 메시지 참조 0개
private void Awake()
{
    if (null == instance)
    {
        instance = this;
    }
    else
    {
        Destroy(this.gameObject);
    }
}

참조 10개
public static PlayerC Instance
{
    get
    {
        if (null == instance)
        {
            return null;
        }
        return instance;
    }
}
```

각 스킬 (질풍참, 튕겨내기, 점프, 좌클릭 공격, 우클릭 공격 )에 키할당을 하고 각 스킬에 맞는 코드를 작성함
```
스킬별 상세코드는 ppt 확인
```

적 오브젝트는 한 개의 cs 파일에서 모두 관리함

각 요소를 item으로 주어 각 요소별 데미지를 차등을 주고 플레이어가 적 오브젝트를 죽일 수 있는 지 여부도 코드를 통해 판별하여, 데미지 시스템을 추가함
```
private void OnTriggerEnter2D(Collider2D collision)
{
    if (collision.gameObject.tag == "Shuriken" && cankill == true)
    {
        GameObject Obj = Resources.Load<GameObject>("Prefabs/Enemy_Destroy_small");
        Obj.transform.localScale = new Vector3(effectSize, effectSize, 1);
        health -= 1;
        if (health <= 0)
        {
            Instantiate(Obj, transform.position, transform.rotation);
            Destroy(gameObject);
        }
    }

    if (collision.gameObject.tag == "Player")
    {
        UIManager.Instance.currentHP -= damage;
    }
}

// Unity 메시지 참조 0개
private void Update()
{
    if (PlayerC.Instance.transform.position.x - gameObject.transform.position.x >= destroyRocate)
    {
        Destroy(gameObject);
    }
}

```

체력 회복 아이템은 두 종류만 존재하여 임의로 체력 회복량을 유니티에서 수정할 수 없게 만듬

플레이어의 체력은 UIManager 라이브러리에서 실시간으로 받아오게 작성함
```
public bool BigSize;

private void OnTriggerEnter2D(Collider2D collision)
{
    if (collision.gameObject.tag == "Player")
    {
        if (BigSize == true)
        {
            UIManager.Instance.currentHP += 250;
        }
        else
        {
            UIManager.Instance.currentHP += 75;
        }

        GameObject Obj = Resources.Load<GameObject>("Prefabs/Healing");
        Instantiate(Obj, transform.position, transform.rotation);
        Destroy(gameObject);
    }
}

```

## 사용된 기술

* Unity - 개발툴
* C# - 개발언어

## 제작자

* **강동형** - 1인 개발