Интерполяция
1. ExecuteAlwayas-атрибут исполнения всегда.
2. Аттрибут [Range(0,1)]-значение скролом 0-1.
3. Mathf.Lerp(min,max,value)-value значение от 0 до1.
4. Vector3.Lerp(T1.position, T2.position, Value); - интерполяция Vector3.
5. Quaternion,Lerp(T1.Rotation,T2Rotation,Value); - интерполяция Quaternion.
6. Color.Lerp(M1.Color,M2.Color,Value); - интерполяция Цвета.
7. LerpAngle(angle1, angle2,value)-интерполяция углов.
8. Lerp.UnClamp-интеполяция больше 1 (используется во всех случаях).

MOVE  TORWARDS

MoveTorwards(yangle, targetY, offset)-смещение на offset к цели
MoveTorwards(Angle);
Все аналогично Lerp

ОКРГУЛЕНИЯ
MathF.Round-Округление до ближайшего числа.
Mathf.Floor-округление до меньшенго числа.
Mathf.Ceil-округление до большего числа.

Perlin noise-плавное распределение 0,1
