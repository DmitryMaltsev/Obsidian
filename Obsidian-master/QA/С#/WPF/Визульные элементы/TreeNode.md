In treeNode we use the HierarchicalDataTemplate, which allows us to template both the tree node itself, while controlling which property to use as a source for child items of the node.
Example for bidning 
https://wpf-tutorial.com/treeview-control/treeview-data-binding-multiple-templates/
In the XAML markup, I have specified a HierarchicalDataTemplate for the **ItemTemplate** of the TreeView. I instruct it to use the **Items** property for finding child items, by setting the **ItemsSource** property of the template, and inside of it I define the actual template, which for now just consists of a TextBlock bound to the **Title** property.
