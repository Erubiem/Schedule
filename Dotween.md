# Dotween에 대해서 ( Button )

---

<aside>
💡 **HEADER**

</aside>

---

# 개요

---

유니티에서 애니메이팅, 추적, 이동 등에 사용되는 Dotween에 대하여 알아보기 특히 버튼 관련

<aside>
⚠️ 작성시기 2023년 02월

</aside>

<aside>
⚠️ unity 2021.3.1f1 에서 진행되었습니다.

</aside>

---

# import

[DOTween (HOTween v2)](https://assetstore.unity.com/packages/tools/animation/dotween-hotween-v2-27676)

위 링크에서 유니티 프로젝트에 임포트 진행해주세요

## 반응형 버튼 만들기

---

### 1. 토글

---

```csharp
using UnityEngine;
using UnityEngine.UI;
using TMPro;
using DG.Tweening;

/// <summary>
/// Sample of button Asset
/// </summary>
public class ButtonController : MonoBehaviour
{
    /// <summary>
    /// Toggle Button 
    /// </summary>
    private bool Isclicked;
    void Awake()
    {
        Button button = GetComponent<Button>();
        button.onClick.AddListener(OnClick);
    }

    void OnClick()
    {
        if (Isclicked == false)
        {
            Isclicked = true;
            //「transform의scale의값을1.1f로、0.5초간。보간방법은OutElastic
            transform.DOScale(1.1f, 0.5f).SetEase(Ease.OutElastic);
            GetComponentInChildren<TMP_Text>().text = "Clicked!";
            
        }
        else
        {
            Isclicked = false;
            transform.DOScale(1.0f, 0.5f).SetEase(Ease.OutElastic);
            GetComponentInChildren<TMP_Text>().text = "Button";
        }
    }
}
```

해당 코드를 일반적인 UI 버튼에 추가만 해주면 클릭 시 크기가 커졌다 줄어드는 반응을 합니다.


### 2. 루프

---

```csharp
using UnityEngine;
using DG.Tweening;

public class ButtonController_2 : MonoBehaviour
{
    void Awake()
    {
        StartStrongButtonAnim();
    }

    void StartStrongButtonAnim()
    {
        transform.DOScale(0.15f, 1f)
        .SetRelative(true)
        .SetEase(Ease.OutQuart)
        .SetLoops(-1, LoopType.Restart);
    }
}
```

코드를 일반적인 UI 버튼에 추가만 해주면 커졌다 작아졌다 하는 애니메이션을 기본적으로 무한반복합니다.
<aside>
* 위 두개의 코드로 기본적인 반응형 ui를 만들 수 있습니다.
</aside>
