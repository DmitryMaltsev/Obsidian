1. Distance/Magnitude-скалярная величина расстояния между векторами
Distance=(posA-posB).magnitude; или vector3 Distance=Vector3.Distance(posA,poaB)
2.  Нормаль- вектор с дистанцией 1.
	Vector2 aToB=b-a;//Вычитаем из конца начало ЭТО ВАЖНО
1. Vector2 normal=aToB.normalized;
2. Рисуем круг Handles.DrawWireDisc(origin, Vector3.forward, Radius, 2);
3.  Vector.dot -Проекция одного вектора на нормаль(или продолженную нормаль) 
другого вектора