Асинхронные потоки — механизм асинхронного получения нескольких элементов данных. Они строятся на основе асинхронных перечисляемых объектов (IAsyncEnumerable). Асинхронный перечисляемый объект представляет собой асинхронную версию перечисляемого объекта (enumerable); т. е. он может производить элементы по требованию для потребителя, и каждый элемент может быть произведен асинхронно. На мой взгляд, полезно сравнить асинхронные потоки с другими, более знакомыми типами и проанализировать различия. Это помогает лучше запомнить, в каких случаях следует использовать асинхронные потоки, а в каких случаях более уместными будут другие типы.
Возвращение нескольких значений из метода может осуществляться командой yield return, а асинхронные методы используют async и await. С асинхронными потоками можно объединить эти два подхода; просто используйте возвращаемый тип IAsyncEnumerable:
async IAsyncEnumerable GetValuesAsync(HttpClient client) 
{ 
	int offset = 0; 
	const int limit = 10; 
	while (true) { // Получить текущую страницу результатов и разобрать их. 
{
	string result = await client.GetStringAsync( $"https://example.com/api/values?offset={offset}&limit={limit}"); 
	string[] valuesOnThisPage = result.Split('\n');
	// Произвести результаты для этой страницы.
	foreach (string value in valuesOnThisPage) 
		yield return value; 
	// Если это последняя страница, работа закончена. 
	if (valuesOnThisPage.Length != limit) break; 
	// В противном случае перейти к следующей странице. 
	offset += limit; 
 } 
}
//Потребление
IAsyncEnumerable GetValuesAsync(HttpClient client); public async Task ProcessValueAsync(HttpClient client) { await foreach (string value in GetValuesAsync(client)) { Console.WriteLine(value); } }