Требуется (асинхронно) приостановить выполнение программы на некоторый период времени. Такая ситуация часто встречается при модульном тестировании или реализации задержки для повторного использования. Она также возникает при программировании простых тайм-аутов.

async Task DownloadStringWithTimeout(HttpClient client, string uri)
{ 
	using var cts = new CancellationTokenSource(TimeSpan.FromSeconds(3));
	Task downloadTask = client.GetStringAsync(uri); 
	Task timeoutTask = Task.Delay(Timeout.InfiniteTimeSpan, cts.Token); 
	Task completedTask = await Task.WhenAny(downloadTask, timeoutTask);
	if (completedTask == timeoutTask) return null; return await downloadTask; 
}

И хотя Task.Delay можно использовать для реализации «мягкого» таймаута, у такого подхода есть свои ограничения. Если в операции происходит тайм-аут, она не отменяется; в предыдущем примере задача загрузки продолжит прием данных и загрузит весь ответ перед тем, как потерять его. Рекомендуемое решение основано на использовании маркера отмены (cancellation token) в качестве тайм-аута и передаче его операции напрямую (GetStringAsync в последнем примере). При этом операция может оказаться неотменяемой; в этом случае Task.Delay может использоваться другим кодом для имитации действий, выполняемых по тайм-ауту.