Фреймворк предоставляет для этой цели метод Task.WhenAll. Метод получает несколько задач и возвращает задачу, которая завершается при завершении всех указанных задач.
**Пример**
sync Task DelayAndReturnAsync(int value)
{
	await Task.Delay(TimeSpan.FromSeconds(value)); return value; }
// Этот метод теперь выводит "1", "2" и "3".
async Task ProcessTasksAsync()
{ 
// Создать последовательность задач.
	Task taskA = DelayAndReturnAsync(2); 
	Task taskB = DelayAndReturnAsync(3); 
	Task taskC = DelayAndReturnAsync(1); 
	//
	Task[] tasks = new[] { taskA, taskB, taskC }; 
	Task[] processingTasks = tasks.Select(async t => 
	{ 
	var result = await t; 
	Trace.WriteLine(result); 
	}).ToArray(); 
	
// Ожидать завершения всей обработки. 
await Task.WhenAll(processingTasks);
}

 
**Получение исключения Task.WhenAll()**
async Task ThrowNotImplementedExceptionAsync() 
{ throw new NotImplementedException(); } 

async Task ThrowInvalidOperationExceptionAsync() 

{ throw new InvalidOperationException(); }
[Обозреваем одно исключение]
async Task ObserveOneExceptionAsync() 
{ 
var task1 = ThrowNotImplementedExceptionAsync();
var task2 = ThrowInvalidOperationExceptionAsync(); 
try 
{ 
	await Task.WhenAll(task1, task2);
} 
catch (Exception ex)
{
	["ex" - либо NotImplementedException, либо InvalidOperationException. ... } ]
} 
[Несколько исключений]
async Task ObserveAllExceptionsAsync() 
{ 
	var task1 = ThrowNotImplementedExceptionAsync();
	var task2 = ThrowInvalidOperationExceptionAsync(); 
	Task allTasks = Task.WhenAll(task1, task2); 
try 
{ 
await allTasks;
} 
catch
{ 
	AggregateException allExceptions = allTasks.Exception; 
}