1. **В LaserScanTVM регистрация **

 public DelegateCommand SetMode => _setMode ??= new DelegateCommand(ExecuteSetMode);
 **Реализация метода**
        private void ExecuteSetMode()
        {
            SyncMode mode = SensorService.SystemSettings.SensorsSyncMode;
            switch (mode)
            {
                case SyncMode.SyncExt:
                    break;
                case SyncMode.SyncNone:
                    break;
                case SyncMode.SyncCmd:
                    _profileBuffer.Clear();
                    _pointsBuffer.Clear();
                    CurrentSensor.ProfileBuffer.Clear();
                    CurrentSensor.ProfilePoints.Clear();
                    CurrentSensor.ProfilePointsSum.Clear();
                    CurrentSensor.XyzwBuffer.Clear();
                    break;
                case SyncMode.SyncFromStrobe:
                    break;
            }
            SensorService.SetMode();
        }
2. **При попытке реализовать только активную команду в CompositeCommand в bool пишем true. Иначае передаем без параметра