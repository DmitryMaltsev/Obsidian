
1. Создаем скрипт
Пример:
[CreateAssetMenu]
public class ColorSettings : ScriptableObject
{
    [field: SerializeField] public Color[] Colors { get; private set; }
}

2.  Создаем ScriptableObject в контекстном меню.