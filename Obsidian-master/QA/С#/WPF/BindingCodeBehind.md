1. В коде 
   Binding binding = new Binding
                {
                    Source = secondViewModel,-Класс обладающий управляющим свойством
                    Path = new PropertyPath("ViewModelValue"),-имя своего свойства
                    StringFormat = "{0:N2}"
                };
 binding.Mode = BindingMode.TwoWay;-UserControl, наследующий DependencyObject
   slider.SetBinding(SliderTool.ValueProperty, binding);	
	