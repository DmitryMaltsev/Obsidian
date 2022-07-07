Ссылки на источник потокобезопаных коллекций
https://docs.microsoft.com/ru-ru/dotnet/standard/collections/thread-safe/

https://docs.microsoft.com/ru-ru/dotnet/api/system.collections.concurrent?view=net-6.0

[BlockingCollection<T>](https://docs.microsoft.com/ru-ru/dotnet/api/system.collections.concurrent.blockingcollection-1) — это потокобезопасный класс коллекции, обеспечивающий следующие возможности:
	
-   реализует шаблон "производитель-получатель";
-   поддерживает параллельное добавление и извлечение элементов из нескольких потоков;
-   допускает указание максимальной емкости;
-   поддерживает операции вставки и удаления, блокирующиеся при опустошении или заполнении коллекции;
-   поддерживает условные операции вставки и удалении, не блокирующиеся или блокирующиеся лишь на определенное время;
-   инкапсулирует все типы коллекций, реализующие интерфейс [IProducerConsumerCollection<T>](https://docs.microsoft.com/ru-ru/dotnet/api/system.collections.concurrent.iproducerconsumercollection-1);
-   поддерживает отмену с помощью токенов отмены;
-   поддерживает два вида перечисления с помощью оператора `foreach` (`For Each` в Visual Basic):
    1.  перечисление "только для чтения";
    2.  перечисление, при котором элементы по мере перечисления удаляются.
	
	##**Поддержка границ блокировки**
	
	[BlockingCollection<T>](https://docs.microsoft.com/ru-ru/dotnet/api/system.collections.concurrent.blockingcollection-1) поддерживает границы и блокировку. Поддержка границ означает возможность задать максимальную емкость коллекции. Границы важны в ряде сценариев, поскольку они позволяют контролировать максимальный размер коллекции в памяти, предотвращая ситуации, в которых потоки-создатели слишком сильно обгоняют потоки-потребители.

Элементы коллекции могут параллельно добавляться из нескольких задач или потоков. Если коллекция достигает максимальной емкости, то потоки-создатели перейдут в состояние блокировки, пока не будет удален хотя бы один элемент. Элементы коллекции могут параллельно удаляться несколькими потребителями. Если коллекция становится пустой, то потоки-потребители перейдут в состояние блокировки, пока поток-создатель не добавит хотя бы один элемент. Поток-создатель может вызвать метод [CompleteAdding], чтобы указать, что он больше не будет добавлять элементы. Потребители могут отслеживать свойство [IsCompleted], позволяющее определить, что коллекция опустела, а новые элементы добавляться не будут.
	Пример 
	// A bounded collection. It can hold no more
// than 100 items at once.
	
BlockingCollection<Data> dataItems = new BlockingCollection<Data>(100);
// A simple blocking consumer with no cancellation.
Task.Run(() =>
{
	// Blocks if dataItems.Count == 0.
    while (!dataItems.IsCompleted)
    {
        Data data = null;
        try
        {
            data = dataItems.Take();
        }
        catch (InvalidOperationException) { }

        if (data != null)
        {
            Process(data);
        }
    }
    Console.WriteLine("\r\nNo more items to take.");
});

// A simple blocking producer with no cancellation.
Task.Run(() =>
{
    while (moreItemsToAdd)
    {
        Data data = GetData();
        // Blocks if numbers.Count == dataItems.BoundedCapacity
        dataItems.Add(data);
    }
    // Let consumer know we are done.
    dataItems.CompleteAdding();
});
	
**Указание типа коллекции**

При создании класса [BlockingCollection<T>]1) можно указать не только ограничиваемую емкость, но и тип используемой коллекции. Например, можно задать объект [ConcurrentQueue<T>] для использования принципа "первым поступил — первым обслужен" или объект [ConcurrentStack<T>] для использования принципа "последним поступил — первым обслужен". Использовать можно любой класс коллекции, реализующий интерфейс [IProducerConsumerCollection<T>]. Тип коллекции, используемый в классе [BlockingCollection<T>] по умолчанию — это [ConcurrentQueue<T>]. В следующем примере кода показано, как создать строковую коллекцию [BlockingCollection<T>]
емкостью в 1000 элементов, которая использует класс [ConcurrentBag<T>]:
	
BlockingCollection<string> bc = new BlockingCollection<string>(new ConcurrentBag<string>(), 1000 );