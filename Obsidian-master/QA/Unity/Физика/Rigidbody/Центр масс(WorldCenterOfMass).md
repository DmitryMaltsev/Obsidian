Отслыживать физические тела на поле можно при помощи Window/Analizes/Physics Debug.
1. Для отрисовки центра массы.
void OnDrawGizmos()
{
 Gizmos.Color=Color.Black;
 Gizmos.DrawSphere(GetComponent<RigidBody>.WorldCenterOfMass,01.f);
}

2. Сменить центр масс
GetComponent<RigidBody>.centerOfMass=centerOfMass;

3. Разбудить объект 
  Getcomponent<RigidBody>().WakeUp
4.  Для упрощения можно создаеть пустой объект и центр масс присвоить позиции этого тела умноженной на масштаб исходного элемента
 GetComponent<RigidBody>.centerOfMass=Vector3.Scale(object.Transform.localPosition,
 transform.localScale)
5. Тензор инерции определяется размером стороны объекта