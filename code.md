
	private static DataManager instance;
	public static DataManager Instance
	{
		get
		{
			if (instance == null)
			{
				GameObject newObject = new GameObject("_DataManager");
				instance = newObject.AddComponent<DataManager>();
				if (instance == null)
					Debug.LogError("DataManager null");
			}
			return instance;
		}
	}
  
	public void NetData()
	{
		if (Application.internetReachability == NetworkReachability.NotReachable)
		{
			DataControl.i.isweb = false;
			LocalDataSet();
			MessageManager.i.onPanel("인터넷이 연결 없이 게임을 시작합니다.");
		}
		else if (Application.internetReachability == NetworkReachability.ReachableViaCarrierDataNetwork)
		{
			DataControl.i.isweb = true;
			StartCoroutine(Version());
			MessageManager.i.onPanel("데이터로 연결 합니다.");
		}
		else if (Application.internetReachability == NetworkReachability.ReachableViaLocalAreaNetwork)
		{
			DataControl.i.isweb = true;
			StartCoroutine(Version());
			MessageManager.i.onPanel("와이파이로 연결 합니다.");
		}
	}
  
	int _level = DataManager.Instance.saveData.attackLevel[id];
        coin0 = DataManager.Instance.opAttackList[id].coin;
        coin1 = (((_level * (_level + 1) / 2) - (_level - 1)) * coin0);
        valuecoin.text = coin1.ToString();
