Время расчета физики  ProjectSettings/Time/Fixed timestep(0.02)-период срабатывания цикла FixedUpdate
1.  Если Input.GetKey(), то в FixedUpdate
	rigidBody.AddTorque(x,y,z);
 2.  Если в локальных координатах, то
     rigidBody.AddRelativeTorque(x,y,z);
 3.  Убрать ограничение на вращение
      rigidBody.MaxAngularVelocity=Mathf.Infity;


	
	