В этом примере показано, как использовать контейнер [ConcurrentBag<T>](https://docs.microsoft.com/ru-ru/dotnet/api/system.collections.concurrent.concurrentbag-1) для реализации пула объектов. Пулы объектов позволяют улучшить производительность приложения, когда требуется несколько экземпляров класса, которые "дорого" создавать или уничтожать. Когда клиентская программа запрашивает новый объект, сперва происходит поиск в пуле объектов ранее созданного и возвращенного в пул объекта. Новый объект создается, только если в пуле не нашлось нужного объекта.

[ConcurrentBag<T>]  используется для хранения объектов, так как поддерживает быстрое добавление и удаление, особенно при добавлении и удалении элементов одним потоком. Этот пример можно расширить до построения на основе интерфейса [IProducerConsumerCollection<T>] который реализует структура данных контейнера, например [ConcurrentQueue<T>] и [ConcurrentStack<T>](
	
namespace ObjectPoolExample
{
    public class ObjectPool<T>
    {
        private readonly ConcurrentBag<T> _objects;
        private readonly Func<T> _objectGenerator;

        public ObjectPool(Func<T> objectGenerator)
        {
            _objectGenerator = objectGenerator ?? throw new ArgumentNullException(nameof(objectGenerator));
            _objects = new ConcurrentBag<T>();
        }

        public T Get() => _objects.TryTake(out T item) ? item : _objectGenerator();

        public void Return(T item) => _objects.Add(item);
    }

    class Program
    {
        static void Main(string[] args)
        {
            using var cts = new CancellationTokenSource();

            // Create an opportunity for the user to cancel.
            _ = Task.Run(() =>
            {
                if (char.ToUpperInvariant(Console.ReadKey().KeyChar) == 'C')
                {
                    cts.Cancel();
                }
            });

            var pool = new ObjectPool<ExampleObject>(() => new ExampleObject());

            // Create a high demand for ExampleObject instance.
            Parallel.For(0, 1000000, (i, loopState) =>
            {
                var example = pool.Get();
                try
                {
                    Console.CursorLeft = 0;
                    // This is the bottleneck in our application. All threads in this loop
                    // must serialize their access to the static Console class.
                    Console.WriteLine($"{example.GetValue(i):####.####}");
                }
                finally
                {
                    pool.Return(example);
                }

                if (cts.Token.IsCancellationRequested)
                {
                    loopState.Stop();
                }
            });

            Console.WriteLine("Press the Enter key to exit.");
            Console.ReadLine();
        }
    }

    // A toy class that requires some resources to create.
    // You can experiment here to measure the performance of the
    // object pool vs. ordinary instantiation.
    class ExampleObject
    {
        public int[] Nums { get; set; }

        public ExampleObject()
        {
            Nums = new int[1000000];
            var rand = new Random();
            for (int i = 0; i < Nums.Length; i++)
            {
                Nums[i] = rand.Next();
            }
        }
        public double GetValue(long i) => Math.Sqrt(Nums[i]);
    }
}