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
  
  
  
