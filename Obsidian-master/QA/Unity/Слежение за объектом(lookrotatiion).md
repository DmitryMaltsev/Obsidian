**Слежение за объектом:**

Vector3 toTarget = Target.position - transform.position;
transform.rotation = Quaternion.LookRotation(toTarget);

  

**Слежение за объектом в одной плоскости:**

Vector3 toTarget = Target.position - transform.position;

Vector3 toTargetXZ = new Vector3(toTarget.x, 0f, toTarget.z);

transform.rotation = Quaternion.LookRotation(toTargetXZ);