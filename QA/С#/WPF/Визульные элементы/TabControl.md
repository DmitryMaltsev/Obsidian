Отдельная вкладка представлена элементом TabItem

1. Для создания вкладок в WPF, как и в WinForms, предназначен элемент TabControl, а отдельная вкладка представлена элементом TabItem:

<TabControl>

 <TabItem  Header=Вкладка 1>Первая вкладка</TabItem>

 <TabItemHeader=Вкладка 2>Вторая вкладка</TabItem>

 </TabControl>
 
В неё можно вложить други элементы.

2.  В PRISM
 Добавление через регион
2.1 View Discovery
RegionManager. RegisterViewWithRegion("RegionName", typeof(View));-автоматическисоздает view и инициализует его
2.2   View Injection
IRegion=RegionManager.Regions["RegionName"];
View view=container.Resolve<View>();
region.add(view); 
region.Activate(view) создаем view, добавляем в регион.
2.3 Navigation 
RegionManager.RequestNavigate("RegName","View",Parameters)
как вариант В Initialize
	container.RegisterType<object, ViewA>("View");
	
	или
	  public void RegisterTypes(IContainerRegistry containerRegistry)
        {
            containerRegistry.RegisterForNavigation<TabPage, TabPageViewModel>("TabPage");
        }
	
	