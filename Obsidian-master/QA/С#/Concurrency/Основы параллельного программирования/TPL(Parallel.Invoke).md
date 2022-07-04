4.3. Параллельный вызов 
Задача Имеется набор методов, которые должны вызываться параллельно. 
Эти методы (в основном) независимы друг от друга.
Решение Класс Parallel содержит простой метод Invoke, спроектированный для таких сценариев.
В следующем примере массив разбивается надвое, и две половины обрабатываются независимо:
void ProcessArray(double[] array)
{ 
Parallel.Invoke
   ( 
	 () => ProcessPartialArray(array, 0, array.Length / 2), 
	 () => ProcessPartialArray(array, array.Length / 2, array.Length) 
	);
} 

void ProcessPartialArray(double[] array, int begin, int end)
{ 
// Обработка, интенсивно использующая процессор...
} 

Методу Parallel.Invoke также можно передать массив делегатов, если количество вызовов неизвестно до момента выполнения: 
Parallel.Invoke поддерживает отмену, как и другие методы класса Parallel: 
 public void Rotate(int degrees)
        {
        Action[] act = 
        {
          () => Console.WriteLine($"Выполняется задача"),
          ()=>Print()
        };
            CancellationTokenSource ctSource= new CancellationTokenSource(TimeSpan.FromSeconds(1));
            CancellationToken token = ctSource.Token;
            Parallel.Invoke(new ParallelOptions {CancellationToken=token}, act);
        }

Метод Parallel.Invoke — отличное решение для простого параллельного вызова. Отмечу, что он уже не так хорошо подходит для ситуаций, в которых требуется активизировать действие для каждого элемента входных данных (для этого лучше использовать Parallel.ForEach), или если каждое действие производит некоторый вывод (вместо этого следует использовать Parallel LINQ).