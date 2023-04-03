### Open Closed Principle


	- A Class should be open for extension but closed for modification.
	- By this principle we cannot modify our class but we can extend our funtionality using inheritance.
---
<p>We get a requirement to add filter and suppose then after some time requirement changes to filter by color and size or both together .</p>

| Requirement | FilterBy | RequirementDate |
| ----------- | ----------- |  ----------  |
| FilterBy | Color |  Month  |
| FilterBy | Size |  2 Months Later |
| FilterBy | SizeAndColor | Very Next Month |

<p>
Because of this we will modify the ProductFilter Class to meet the new requirements. But following design principles we can comprehend that modification of existing class violates the open closed principle . We can use **Inheritance** to acheive extension of class without modification.
</p>

```
public enum Color
    {
        Red, Green, Blue
    }
public enum Size
    {
        Small, Medium, Large, Yuge
    }
public class ProductFilter
    {
        //Fiter by color
        public IEnumerable<Product> FilterByColor(IEnumerable<Product> products, Color color)
        {
            foreach (var p in products)
                if (p.Color == color)
                    yield return p;
        }
        //Fiter by size
        public static IEnumerable<Product> FilterBySize(IEnumerable<Product> products, Size size)
        {
            foreach (var p in products)
                if (p.Size == size)
                    yield return p;
        }
        //Fiter by sizeandcolor
        public static IEnumerable<Product> FilterBySizeAndColor(IEnumerable<Product> products, Size size, Color color)
        {
            foreach (var p in products)
                if (p.Size == size && p.Color == color)
                    yield return p;
        }
    }
```

```
public class Demo
    {
        public static void Main(string[] args)
        {
            var apple = new Product("Apple", Color.Green, Size.Small);
            var tree = new Product("Tree", Color.Green, Size.Large);
            var house = new Product("House", Color.Blue, Size.Large);

            Product[] products = { apple, tree, house };

            var pf = new ProductFilter();
            WriteLine("Green products (old):");
            foreach (var p in pf.FilterByColor(products, Color.Green))
                WriteLine($" - {p.Name} is green");

        }
    }
```


Here we have implemented OCP without modifying the BetterFilter classs using inheritance and then and there if specification changes we can inherit the interface and introduce new class to adopt the requirement just by extending the funtionality.

Better filter extends IFilter it itereates through the products and returns true if it matches the color ,size or. both

```
 public enum Color
    {
        Red, Green, Blue
    }
 public enum Size
    {
        Small, Medium, Large, Yuge
    }

 public interface ISpecification<T>
    {
        bool IsSatisfied(Product p);
    }

 public interface IFilter<T>
    {
        IEnumerable<T> Filter(IEnumerable<T> items, ISpecification<T> spec);
    }
	
 public class BetterFilter : IFilter<Product>
    {
 public IEnumerable<Product> Filter(IEnumerable<Product> items, ISpecification<Product> spec)
        {
            foreach (var i in items)
                if (spec.IsSatisfied(i))
                    yield return i;
        }
    }        I
```

```
 public class ColorSpecification : ISpecification<Product>
    {
        private Color color;
        public ColorSpecification(Color color)
        {
            this.color = color;
        }
        public bool IsSatisfied(Product p)
        {
            return p.Color == color;
        }
    } 
 public class SizeSpecification : ISpecification<Product>
    {
        private Size size;
        public SizeSpecification(Size size)
        {
            this.size = size;
        }
        public bool IsSatisfied(Product p)
        {
            return p.Size == size;
        }
    }
```

```
 public class AndSpecification<T> : ISpecification<T>
    {
        private ISpecification<T> first, second;
        public AndSpecification(ISpecification<T> first, ISpecification<T> second)
        {
            this.first = first ?? throw new ArgumentNullException(paramName: nameof(first));
            this.second = second ?? throw new ArgumentNullException(paramName: nameof(second));
        }
        public bool IsSatisfied(Product p)
        {
            return first.IsSatisfied(p) && second.IsSatisfied(p);
        }
    }
```
We can pass the ProductsList and ColorSpec to the BetterFilter's filter method .So that we can filter by color.
```
public class Demo
    {
        static void Main(string[] args)
        {
            var bf = new BetterFilter();
            WriteLine("Green products (new):");
            foreach (var p in bf.Filter(products, new ColorSpecification(Color.Green)))
                WriteLine($" - {p.Name} is green");

            WriteLine("Large products");
            foreach (var p in bf.Filter(products, new SizeSpecification(Size.Large)))
                WriteLine($" - {p.Name} is large");

            WriteLine("Large blue items");
            foreach (var p in bf.Filter(products,
              new AndSpecification<Product>(new ColorSpecification(Color.Blue), new SizeSpecification(Size.Large)))
            )
            {
                WriteLine($" - {p.Name} is big and blue");
            }
        }
    }
```