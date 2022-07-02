Параллельное выполнение задач может занимать много времени. И иногда может возникнуть необходимость прервать выполняемую задачу. Для этого платформа .NET предоставляет структуру CancellationToken из пространства имен `System.Threading`.

Общий алгоритм отмены задачи обычно предусматривает следующий порядок действий:

1.  Создание объекта CancellationTokenSource, который управляет и посылает уведомление об отмене токену.
    
2.  С помощью свойства CancellationTokenSource.Token получаем собственно токен - объект структуры CancellationToken и передаем его в задачу, которая может быть отменена.

3.  Определяем в задаче действия на случай ее отмены.
    
4.  Вызываем метод CancellationTokenSource.Cancel(), который устанавливает для свойства CancellationToken.IsCancellationRequested значение true. Стоит понимать, что сам по себе метод CancellationTokenSource.Cancel() не отменяет задачу, он лишь посылает уведомление об отмене через установку свойства `CancellationToken.IsCancellationRequested`. Каким образом будет происходить выход из задачи, это решает сам разработчик.
    
5.  Класс CancellationTokenSource реализует интерфейс IDisposable. И когда работа с объектом CancellationTokenSource завершена, у него следует вызвать метод Dispose для освобождения всех связанных с ним используемых ресурсов. (Вместо явного вызова метода Dispose можно использовать конструкцию using).


Теперь касательно третьего пункта - определения действий отмены задачи. Как именно завершить задачу? Конкретные действия на лежат целиком на разработчике, тем не менее есть два общих варианта выхода:

-   При получении сигнала отмены выйти из метода задачи, например, с помощью оператора return или построив логику метода соответствующим образом. Но следует учитывать, что в этом случае задача перейдет в состояние `TaskStatus.RanToCompletion`, а не в состояние `TaskStatus.Canceled`.
    
-   При получении сигнала отмены сгенерировать исключение OperationCanceledException, вызвав у токена метод ThrowIfCancellationRequested(). После этого задача перейдет в состояние `TaskStatus.Canceled`.
Пример:
async Task DownloadStringWithTimeout(HttpClient client, string uri)
{ using var cts = new  CancellationTokenSource(TimeSpan.FromSeconds(3)); 
 Task downloadTask = client.GetStringAsync(uri); 
 Task timeoutTask = Task.Delay(Timeout.InfiniteTimeSpan, cts.Token); Task completedTask = await Task.WhenAny(downloadTask, timeoutTask); 
 if (completedTask == timeoutTask) return null; 
 return await downloadTask;
}


**Пример с отменой If else**
`CancellationTokenSource cancelTokenSource =` `new` `CancellationTokenSource();`
`CancellationToken token = cancelTokenSource.Token;`
`// задача вычисляет квадраты чисел`
`Task task =` `new` `Task(() =>`
`{`
    `for` `(``int` `i = 1; i < 10; i++)`
    `{`
        `if` `(token.IsCancellationRequested)`  
		`// проверяем наличие сигнала отмены задачи`
        `{`
            `Console.WriteLine(``"Операция прервана"``);`
            `return``;`     `//  выходим из метода и тем самым завершаем задачу`
        `}`
        `Console.WriteLine($``"Квадрат числа {i} равен {i * i}"``);`
        `Thread.Sleep(200);`
    `}`
`}, token);`
`task.Start();`
`Thread.Sleep(1000);`
`// после задержки по времени отменяем выполнение задачи`
`cancelTokenSource.Cancel();`
`// ожидаем завершения задачи`
`Thread.Sleep(1000);`
`//  проверяем статус задачи`
`Console.WriteLine($``"Task Status: {task.Status}"``);`
`cancelTokenSource.Dispose();` `// освобождаем ресурсы`

**Пример с отменой try catch**
`CancellationTokenSource cancelTokenSource =` `new` `CancellationTokenSource();`
`CancellationToken token = cancelTokenSource.Token;`
`Task task =` `new` `Task(() =>`
`{`
    `for` `(``int` `i = 1; i < 10; i++)`
    `{`
        `if` `(token.IsCancellationRequested)`
            `token.ThrowIfCancellationRequested();` `// генерируем исключение`
        `Console.WriteLine($``"Квадрат числа {i} равен {i * i}"``);`
        `Thread.Sleep(200);`
    `}`
`}, token);`
`try`
`{`
    `task.Start();`
    `Thread.Sleep(1000);`
    `// после задержки по времени отменяем выполнение задачи`
    `cancelTokenSource.Cancel();`
    `task.Wait();` `// ожидаем завершения задачи`
`}`
`catch` `(AggregateException ae)`
`{`
    `foreach` `(Exception e` `in` `ae.InnerExceptions)`
    `{`
        `if` `(e` `is` `TaskCanceledException)`
            `Console.WriteLine(``"Операция прервана"``);`
        `else`
            `Console.WriteLine(e.Message);`
    `}`
`}`
`finally`
`{`
    `cancelTokenSource.Dispose();`
`}`
`//  проверяем статус задачи`
`Console.WriteLine($``"Task Status: {task.Status}"``);`