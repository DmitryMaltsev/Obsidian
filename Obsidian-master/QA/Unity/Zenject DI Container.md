1. Скачиваем по ссылке https://github.com/modesttree/Zenject/releases/tag/9.2.0. 
2. Все установится в открытый проект.
3. Создаем папку Resources. В ней Create/Zenject/ProjectContext
4. Создаем на сцене gameObject System. Внутри создем SceneContext.  На него вешаем SceneContext(script). 
5. Внутри PlayerInstaller создадим PlayerInstaller.
 
6. Создадим PlayerInstallerScript(Create/Zenject/Monoinstaller). В папке Code создаем подпапку Installers. 

7.  Объявление.  Самое простое:
   [SerializeField] private PlayerUnit(тип) playerUnit
   public override void InstallBindings()
   {
	   var  playerInstance= Container.InstantiatePrefabForComponent<PlayerUnit>(playerUnit);
	   
	   Container.Bind<PlayerUnit>().FromInstanse(playerInstance).AsSingle().NoLazy();
   }
   
8. **Префаб с установкой в пространстве. 
     [SerializeField] private PlayerUnit(тип) playerUnit;
     [SerializeField] private Transform playerSpawnPoint;
   public override void InstallBindings()
   {
	   var  playerInstance= Container.InstantiatePrefabForComponent<PlayerUnit>(playerUnit,
	   playerSpawnPoint.position, Quaternion.Identity, null);
	   
	   Container.Bind<PlayerUnit>(). FromInstanse(playerInstance).AsSingle().NoLazy();
   }
   
9. **Если не GameObject не префаб**:
Убираем playerInstance. А в аргументы  вместо Instanse добавляем экземпляр GameObject(в данном случае playerUnit).

10. **Inject интерфейса**.
 Container.Bind<IPlayerInputService>().To<PlayerInputService>().
 FromComponentInPrefab(playerInputServicePrefab).AsSingle();
 
 11. В PlayerInstaller добавляем соответствующие GameObjects.
 
 12. В SceneContext в MonoInstaller добавляем соответствующие MonoInstallers(в данном случае PlayerInstaller).

13. Проверка сцены на ошибки: Edit/Zenject/ValidateCurrentScenes.

14. Использовать зарегистрированные объекты: 
[Inject] private PlayerUnit playerUnit;

