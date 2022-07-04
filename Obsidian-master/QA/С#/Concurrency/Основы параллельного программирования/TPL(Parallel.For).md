Метод Parallel.For позволяет выполнять итерации цикла параллельно. Он имеет следующее определение:
For(int, int, Action<int>)
Первый параметр метода задает начальный индекс элемента в цикле, а второй параметр - конечный индекс. Третий параметр - делегат Action - указывает на метод, который будет выполняться один раз за итерацию:
Parallel.For(1, 5, Square);`
	
`// вычисляем квадрат числа`
`void` `Square(``int` `n)`
`{`
    `Console.WriteLine($``"Выполняется задача {Task.CurrentId}"``);`
    `Console.WriteLine($``"Квадрат числа {n} равен {n * n}"``);`
    `Thread.Sleep(2000);
`}`
	
	
	**Выход из цикла**
	
В стандартных циклах for и foreach предусмотрен преждевременный выход из цикла с помощью оператора `break`. В методах Parallel.ForEach и Parallel.For мы также можем, не дожидаясь окончания цикла, выйти из него:
	
	`ParallelLoopResult result = Parallel.For(1, 10, Square);`
`if` `(!result.IsCompleted)`
    `Console.WriteLine($``"Выполнение цикла завершено на итерации {result.LowestBreakIteration}"``);`
`// вычисляем квадрат числа`
`void` `Square(int n, ParallelLoopState pls)
`{`
    `if` `(n == 5) pls.Break();`    `// если передано 5, выходим из цикла`
    `Console.WriteLine($``"Квадрат числа {n} равен {n * n}"``);`
    `Thread.Sleep(2000);`
`}`