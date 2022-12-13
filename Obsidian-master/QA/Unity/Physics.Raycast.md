Public Transform Pointer; 
1. Ray ray=new Ray(transform.origin, transform.forward x 100f,color.yellow);
    Physics.Raycast(ray, out RaycastHit hit(сведениея об объекте)) //если пересек
    Pointer.position=hit.point;
    hit.gameObject.GetComponent<Renderer>().material.Color=Color.yellow;

2. Чтобы не менялся размер точки, которую указываем. 
//Важный метод!
 float scale= Vector3.Distance(transform.position, CameraTransform.Position);
 transform.localScale=Vector3.one*scale*Size;
 
3. Используя курсор мыши.
Ray ray=Camera.main.ScreenPointToRay(Input.mousePosition);
Debug.DrawRay(transform.position, transform.forward*100f, Color.yellow);

4. Ограничить Рейкаст Physics.Raycast(ray, Distance);

5. Physics.RayCastAll(); чтобы пройти через все объекты на пути луча.

6.  У компонента коллайдер тоже есть RayCast
 Ray ray=Camera.main.ScreenPointToRay(Input.mousePosition);
	 if(GetComponent<Collider>.Raycast(ray, out hit,100f))
	 {
		 If(Input.GetMouseButtonDown(0))
		 {
			 GetComponent<Renderer>().material.color=color.white;
		 }
	 }