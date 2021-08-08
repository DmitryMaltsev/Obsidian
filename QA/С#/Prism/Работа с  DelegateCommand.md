**Добавление события RaiseCanExeciteChanged** в методы или св-ва
1. **Сама команда** 
public DelegateCommand MoveToPrevModule => _moveToPrevModule ??= new DelegateCommand(ExecuteMoveToPrev, CanExecuteMoveToPrev);

2. В свойство
  public int Index
        {
            get => _index; set
            {
                _ = SetProperty(ref _index, value);
                MoveToPrevModule.RaiseCanExecuteChanged();
            }
        }

**Добавление метода ObservesProperty**
Добавляем в объявлении самой команды.

public DelegateCommand MoveToPrevModule => _moveToPrevModule ??= new DelegateCommand(ExecuteMoveToPrev, CanExecuteMoveToPrev).ObservesProperty(()=>MyProperty)-if  property changes. Можешь применить несколько раз

**Добавление метода ObservesCanExecute**
Определяет оповещение, может ли использоваться комнда

**Добавление параметров в DelegateCommand**
1. В методе DelegateCommand(ExecuteCommand)

ExecuteNavigate()
{
	var params=new NavigationParameters
	{
		{"Model", new Contact}
	}	
	или
RegionManager.RequestNavigate("MainPage" + navParameters.ToString());

RegionManager.RequestNavigate("RegionName", MainPage", navigationParams); 
}

2. Во ViewModel
Реализуем INavigationAware
В методе
OnNavigatedTo(INaveigationParameters parameters
{
 var color = parameters.GetValue<Color>("color");
или
var id=
}