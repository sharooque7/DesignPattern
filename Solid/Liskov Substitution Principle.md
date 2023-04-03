### Liskov Substitution Principle

	- The idea here to substitute base type for sub type
	
Here we have Rectangle and square class.For this example the square class is inheriting the rectangle. The square class hides base class (Rectangle) setter method and modifies both width and height to same value to calculate area for sqaure.


```
    public class Rectangle
    {
        public int width { get; set; }
        public int height { get; set; }
        public Rectangle()
        {

        }
        public Rectangle(int width, int height)
        {
            this.width = width;
            this.height = height;
        }

    }
    public class Square : Rectangle
    {
        public new int width
        {
            set { base.width = base.height = value; }
        }
        public new int height
        {
            set { base.width = base.height = value; }
        }
    }
```

This funtionality works fine when the object is created and assigned to the respective reference types.Like Rectangle,square object is assigned to rectangle and square reference types respectively.But when assigning square type object to the reference type of Rectangle and calcuate area for that obj we get zero . As when the base type holds only width value we initiazied and height will be zero by defualt .

````
 public class Program
    {
        public static void Main(string[] args)
        {
            // Method to calculate area 
            static int Area(Rectangle r) => r.width * r.height;

            Rectangle rectangle = new Rectangle(2, 3);

            Console.WriteLine(Area(rectangle)); //16

            Square sq1 = new Square();
            sq1.width = 3;

            Console.WriteLine(Area(sq1)); //9

            Rectangle sq2 = new Square();
            sq2.width = 4;

            Console.WriteLine(Area(sq2)); //0
        }
    }
````
The ideal solution is to override the setter method in square  and then substitue sub type for base type. By this we are following the principle of Liskov
```
 public class Rectangle
    {
        public virtual int width { get; set; }
        public virtual int height { get; set; }
        public Rectangle()
        {

        }
        public Rectangle(int width, int height)
        {
            this.width = width;
            this.height = height;
        }

    }

    public class Square : Rectangle
    {
        public override int width
        {
            set { base.width = base.height = value; }
        }

        public override int height
        {
            set { base.width = base.height = value; }
        }
    }
```

```
public class Program
    {
        public static void Main(string[] args)
        {
            // Method to calculate area 
            static int Area(Rectangle r) => r.width * r.height;

            Rectangle rectangle = new Rectangle(2, 3);

            Console.WriteLine(Area(rectangle)); //16

            Square sq1 = new Square();
            sq1.width = 3;

            Console.WriteLine(Area(sq1)); //9

            Rectangle sq2 = new Square();
            sq2.width = 4;

            Console.WriteLine(Area(sq2)); //16
        }
    }
````
```