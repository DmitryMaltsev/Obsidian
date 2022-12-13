
1. При столкновении RigidBody с объектом
private void OnCollisionEnter(Collision collision)
{
	 if(collision.gameobject.GetComponent<Enemy>)
	 collision.gameobject.GetComponent<Enemy>.OnHit()//Вызываем действие
	 
	 If(collision.GameObject.tag=="BulletMarkReciever")
	 {
		Vector3.position=collision.Contacts[o].Point;
		Quaternion rotation=Quaternion.LookRotation(collision.contacts[0].normal);//TODO 
		написать дома
		Instantiate(MarkPrefab,position,rotation);
	 }
}
Еще есть методы - OnTCollisionStay, OnCollisionExit;
2. Пример со звуком
AudioSource audioSource;
private void OnCollisionEnver(Collision collisioin)
{
	audioSource.volume=collision.Impulse.magnitude x 0.01f;
	audioSource.Play();
}

3. Работа с триггерами  
 private void OnTriggerEnter(Collider collider)
	{
	  collider.GameObject.GetComponent<Renderer>().material.Color=Color.red
	}
	private void OnTriggerStay(Collider collider)
	{
		collider.attacherRigidBody.AddForce(Vector3.up x 20f);
	}
	private void OnTriggerExit(Collider other)
	{
		collider.gameObject.GetComponent<Renderer>().material.color=Color.green;
	}