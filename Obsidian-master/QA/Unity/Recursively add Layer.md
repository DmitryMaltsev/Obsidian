1. void SetLayerRecursively(GameObject obj, int newLayer)
    
2.     {
3.         if (null == obj)
4.         {
5.             return;
6.         }
8.         obj.layer = newLayer;
10.         foreach (Transform child in obj.transform)
11.         {
12.             if (null == child)
13.             {
14.                 continue;
15.             }
16.             SetLayerRecursively(child.gameObject, newLayer);
17.         }
18.     }