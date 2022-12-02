Время расчета физики  ProjectSettings/Time/Fixed timestep(0.02)-период срабатывания цикла FixedUpdate
1.Делаем в FixedUpdate если Getkey
rigidBody.AddForce(x,y,z);

2.  Если GetKeyDown, то в  Update
if(Input.GetKeyDown(KeyCode.Space))
rigidBody.addForce(x,y,z);

3. rigibody.AddRelativeForce(добавляем силу в локальных координатах, если направление фигуры изменилось)

**ForceMode**
Время действия силы рассчитывается  ProjectSettings/Time/Fixed timestep
1. По умолчанию  ForceMode.Force(Работает в зависимости от массы
 F=MxA
2. ForceMode.Acceleration
	A=F/m
3.  ForceMode.Impulse. 
  P=FxT
  4. Forcemode.VelocityChange
  v=F/MxT

**AddForceAtPoint**
1. Если Input.GetKeyDown(), то в Update
	if(Input.GetKeyDown())
		TargetRigidBody.AddForceAtPoint(transform.up, transform.position);