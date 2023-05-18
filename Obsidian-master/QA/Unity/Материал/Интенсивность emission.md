public Material material; 
// ссылка на материал void Update()
{ Color color = material.GetColor("_EmissionColor"); // получить текущий цвет материала 
float intensity = Mathf.Sin(Time.time); // использовать условный код для изменения интенсивности 
color *= intensity; // умножить цвет на интенсивность
material.SetColor("_EmissionColor", color); // установить новый цвет эмиссии материала }