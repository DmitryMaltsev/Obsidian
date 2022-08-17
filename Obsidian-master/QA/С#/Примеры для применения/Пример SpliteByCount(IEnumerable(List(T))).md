public static IEnumerable<List<T>> SplitByCount<T>(this T[] collection, int chunksize)
{
            IEnumerator enumerator = collection.GetEnumerator();
            int position = 0;
            while (position < collection.Length)
            {
                yield return SubCollection().ToList();

                IEnumerable<T> SubCollection()
                {
                    for (int i = 0; i < chunksize; i++)
                    {
                        if (enumerator.MoveNext())
                        {
                            yield return (T)enumerator.Current;
                        }
                        position++;
                    }
                }
            }
}