
1. **В ApplicationCommands**
public CompositeCommand InitOneSensor { get; } = new(true);
        /// <summary>
        /// Инициализация нескольких датчиков
        /// </summary>
        public CompositeCommand InitAllSensors { get; } = new();
		
2. **LaserscanTabViewModel**
 private DelegateCommand _init;
        /// <summary>
        /// Команда инициализации профилометра
        /// </summary>
        public DelegateCommand Init => _init ??= new DelegateCommand(ExecuteInitAsync);
		
**Реализация ExecuteInitAsync**
 private async void ExecuteInitAsync()
        {
            CurrentSensor.ProfileBuffer.Clear();
            CurrentSensor.ProfilePoints.Clear();
            CurrentSensor.ProfilePointsSum.Clear();
            CurrentSensor.XyzwBuffer.Clear();
            await SensorService.Init();
        }

**В конструкторе** 

 ApplicationCommands.InitOneSensor.RegisterCommand(Init);
 ApplicationCommands.InitAllSensors.RegisterCommand(Init);
 
 3. **LaserscanTabViewModel метод execute**
 
     private void OnCoverMaterialIndexChanged()
        {
            foreach (var systemSettings in SensorRepository.SystemSettings)
            {
                systemSettings.SensorsSyncMode = SyncMode.SyncCmd;
            }
            ApplicationCommands.InitAllSensors.Execute(null);
        }
		
