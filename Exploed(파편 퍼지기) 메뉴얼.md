
<h1> 개요 </h1>

* 사용 목적
  1. 적이 죽거나 물건을 공격한 후 파편 등이 튀어오르는 모습을 연출
   
* 주의사항
  1. 유니티 3.10f1에서 제작됨
  2. ThrowItem(던지기) 로직이 사용됨
  3. 추후 설정값에서 최대값이 최소값보다 작지 않게 설정

* 구성 요소
    1. Exploed 스크립트
    2. ThrowItem 스크립트
    3. 사용 예시 폴더 (Demo 폴더)

* 사용 방법
    1. 터지는 대상에게 Exploed 스크립트 부여
    2. 적용된 Exploed 스크립트 설정 (후에 자세한 설명)
    3. 다른 스크립트에서 적용된 Exploed스크립트 참조 후 ExploedStart() 실행

* 사용 예시
  *  <img src="https://github.com/sesiprojectTexasPostalUnion/PrivateTexasPostalUnionSchedule/blob/main/image/ExploedDemoPlay.gif">

 <h1> 코드 내용 </h1>

```csharp
    public GameObject[] exploedParts;

    public int partCountMin;
    public int partCountMax;

    [Range(0, 1)]
    public float randomRateRangeMin;
    [Range(1, 2)]
    public float randomRateRangeMax;




    public void ExploedStart()
    {
        int partCount = Random.Range(partCountMin, partCountMax + 1);
        for (int i = 0; i < partCount; i++)
        {
            GameObject exploedItem = Instantiate(exploedParts[Random.Range(0, exploedParts.Length)]);
            exploedItem.transform.position = transform.position;

            ThrowItem throwItem = exploedItem.GetComponent<ThrowItem>();
            float originalTime = throwItem.time;

            throwItem.distance *= Random.Range(randomRateRangeMin, randomRateRangeMax);
            throwItem.time *= Random.Range(randomRateRangeMin, randomRateRangeMax);
    
            // 높이는 곡사에서만
            if (throwItem.isCurveShot == true)
                throwItem.height *= Random.Range(randomRateRangeMin, randomRateRangeMax);

            // 그래도 시간에 따라 튕기는 횟수 조절
            if ( throwItem.time <= originalTime)
            {
                throwItem.bounceCount *= (int)Random.Range(randomRateRangeMin, 1);
                throwItem.bounceRate *= Random.Range(randomRateRangeMin, 1);
            }
            else if ( throwItem.time > originalTime)
            {
                throwItem.bounceCount *= (int)Random.Range(1, randomRateRangeMax);
                throwItem.bounceRate *= Random.Range(1, randomRateRangeMax);
            }

            // 발사
            int angle = Random.Range(0, 360);
            throwItem.ThrowStart(angle);
        }

    }
```

 <h1> 코드 설명 </h1>
 <h2> 변수 부분 </h2>

   * exploedParts: ThrowItem이 적용된 오브젝트 할당
     * 이 변수에 할당된 오브젝트중 랜덤으로 날아가게됨
   * partCountMin / Max: 부품이 날아갈 최소 개수와 최대 개수
   * randomRateRangeMin / Max: 날아갈때 설정이 변하게될 비율
     * ThrowItem의 값으로 되어있는 거리, 시간 등을 랜덤으로 변화시킨다.
     * 각각의 값이 각자 다른 랜덤값으로 설정된다.
    
 <h2> ExploedStart() 부분 </h2>

   * 공이 날라가는데 필요한 여러 설정을 랜덤한 값으로 변화
   * 마지막에 각도와 함께 ThrowItem.ThrowStart(각도)로 던지기 시작
   * 변수 부분에서 설정된 partCount만큼 반복한다.


<h1> 예시 폴더 설명 </h1>

   * Player 스크립트는 Enemy 생성과 제거용 스크립트
     * 게임 시작후 R을 누를시 Enemy를 생성한다.
     * 게임 시작후 D를 누를시 생성된 Enemy중 랜덤으로 Dead()를 실행한다.
   * Enemy 프리펩에 Enemy 스크립트와 DemoExploed 스크립트가 적용되어있다.
     * Enemy 스크립트에서 Dead() 실행시 DemoExploed의 ExploedStart()를 실행하고 자신의 게임 오브젝트를 Destroy 한다.
