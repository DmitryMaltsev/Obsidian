Если требуется выполнение одной или нескольких команд из родительского окна(View).

1. **В проекте Core** 
 public class ApplicationCommands:IApplicationCommands
    {
        private CompositeCommand  SaveCommand = new CompositeCommand();
    }
	**Interface**
	
  public interface IApplicationCommands
    {
        CompositeCommand SaveCommand { get; }
    }
2. **В App**

  public partial class App : PrismApplication
    {
        protected override void RegisterTypes(IContainerRegistry containerRegistry)
        {
            containerRegistry.RegisterSingleton<IApplicationCommands, ApplicationCommands>();
        }
    } -Регистрируем для di.
	
3. **Региструируем delegatecommand в CompositeCommand	В соответствующих моделях в конструкторе**
	applicationCommands.SaveAllCommand.RegisterCommand(UpdateCommand-сама команда);
	
4. **В MainWindowViewModel**
Создаем свойство applicationCommands.
В конструкторе присваиваем значение di.


Дополнительно
В Delegatecommand dc=new DelegateCommand.ObservesCanExecute(()=>CanUpdate);
-CanUpdate - bool true/false