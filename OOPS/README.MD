Java Garbage Collection
In java, garbage means unreferenced objects.
Garbage Collection is process of reclaiming the runtime unused memory automatically. In other words, it is a way to destroy the unused objects.
To do so, we were using free() function in C language and delete() in C++. But, in java it is performed automatically. So, java provides better memory management.
Advantage of Garbage Collection
o	It makes java memory efficient because garbage collector removes the unreferenced objects from heap memory.
o	It is automatically done by the garbage collector(a part of JVM) so we don't need to make extra efforts.
________________________________________
How can an object be unreferenced?
There are many ways:
o	By nulling the reference
o	By assigning a reference to another
o	By anonymous object etc.
 
1) By nulling a reference:




1.	Employee e=new Employee();  
2.	e=null;  
2) By assigning a reference to another:




1.	Employee e1=new Employee();  
2.	Employee e2=new Employee();  
3.	e1=e2;//now the first object referred by e1 is available for garbage collection  
3) By anonymous object:




1.	new Employee();  
________________________________________
finalize() method
The finalize() method is invoked each time before the object is garbage collected. This method can be used to perform cleanup processing. This method is defined in Object class as:




1.	protected void finalize(){}  
Note: The Garbage collector of JVM collects only those objects that are created by new keyword. So if you have created any object without new, you can use finalize method to perform cleanup processing (destroying remaining objects).
gc() method
The gc() method is used to invoke the garbage collector to perform cleanup processing. The gc() is found in System and Runtime classes.




1.	public static void gc(){}  
Note: Garbage collection is performed by a daemon thread called Garbage Collector(GC). This thread calls the finalize() method before object is garbage collected.
Simple Example of garbage collection in java




1.	public class TestGarbage1{  
2.	 public void finalize(){System.out.println("object is garbage collected");}  
3.	 public static void main(String args[]){  
4.	  TestGarbage1 s1=new TestGarbage1();  
5.	  TestGarbage1 s2=new TestGarbage1();  
6.	  s1=null;  
7.	  s2=null;  
8.	  System.gc();  
9.	 }  
10.	}  

       object is garbage collected
       object is garbage collected
Note: Neither finalization nor garbage collection is guaranteed.


Important Concepts Related to Garbage Collection in Java
1. Unreachable objects: An object is said to be unreachable if it doesn’t contain any reference to it. Also, note that objects which are part of the island of isolation are also unreachable. 
Integer i = new Integer(4);
// the new Integer object is reachable  via the reference in 'i' 
i = null;
// the Integer object is no longer reachable. 
 
2. Eligibility for garbage collection: An object is said to be eligible for GC(garbage collection) if it is unreachable. After i = null, integer object 4 in the heap area is suitable for garbage collection in the above image.
Ways to make an object eligible for Garbage Collector
•	Even though the programmer is not responsible for destroying useless objects but it is highly recommended to make an object unreachable(thus eligible for GC) if it is no longer required.
•	There are generally four ways to make an object eligible for garbage collection.
1.	Nullifying the reference variable
2.	Re-assigning the reference variable
3.	An object created inside the method
4.	Island of Isolation
Ways for requesting JVM to run Garbage Collector
•	Once we make an object eligible for garbage collection, it may not destroy immediately by the garbage collector. Whenever JVM runs the Garbage Collector program, then only the object will be destroyed. But when JVM runs Garbage Collector, we can not expect.
•	We can also request JVM to run Garbage Collector. There are two ways to do it : 
1.	Using System.gc() method: System class contain static method gc() for requesting JVM to run Garbage Collector.
2.	Using Runtime.getRuntime().gc() method: Runtime class allows the application to interface with the JVM in which the application is running. Hence by using its gc() method, we can request JVM to run Garbage Collector.
3.	There is no guarantee that any of the above two methods will run Garbage Collector.
4.	The call System.gc() is effectively equivalent to the call : Runtime.getRuntime().gc()



garbage collector(gc) will see 2 objects free. Now to decrement nextId,gc(garbage collector) will call method to finalize() only when we programmers have overridden it in our class. And as mentioned previously, we have to request gc(garbage collector), and for this, we have to write the following 3 steps before closing brace of sub-block.  
1.	Set references to null(i.e X = Y = null;)
2.	Call, System.gc();
3.	Call, System.runFinalization();
Now the correct code for counting the number of employees(excluding interns)  
// Correct code to count number
// of employees excluding interns.
 
class Employee {
   
    private int ID;
    private String name;
    private int age;
    private static int nextId = 1;
   
    // it is made static because it
    // is keep common among all and
    // shared by all objects
    public Employee(String name, int age)
    {
        this.name = name;
        this.age = age;
        this.ID = nextId++;
    }
    public void show()
    {
        System.out.println("Id=" + ID + "\nName=" + name
                           + "\nAge=" + age);
    }
    public void showNextId()
    {
        System.out.println("Next employee id will be="
                           + nextId);
    }
    protected void finalize()
    {
        --nextId;
        // In this case,
        // gc will call finalize()
        // for 2 times for 2 objects.
    }
}
 
public class UseEmployee {
    public static void main(String[] args)
    {
        Employee E = new Employee("GFG1", 56);
        Employee F = new Employee("GFG2", 45);
        Employee G = new Employee("GFG3", 25);
        E.show();
        F.show();
        G.show();
        E.showNextId();
        F.showNextId();
        G.showNextId();
 
        {
            // It is sub block to keep
            // all those interns.
            Employee X = new Employee("GFG4", 23);
            Employee Y = new Employee("GFG5", 21);
            X.show();
            Y.show();
            X.showNextId();
            Y.showNextId();
            X = Y = null;
            System.gc();
            System.runFinalization();
        }
        E.showNextId();
    }
}
Output
Id=1
Name=GFG1
Age=56
Id=2
Name=GFG2
Age=45
Id=3
Name=GFG3
Age=25
Next employee id will be=4
Next employee id will be=4
Next employee id will be=4
Id=4
Name=GFG4
Age=23
Id=5
Name=GFG5
Age=21
Next employee id will be=6
Next employee id will be=6
Next employee id will be=4






How to make object eligible for garbage collection in Java?

An object is eligible to be garbage collected if its reference variable is lost from the program during execution.Sometimes they are also called unreachable objects.
What is reference of an object?
The new operator dynamically allocates memory for an object and returns a reference to it. This reference is the address in memory of the object allocated by new. A reference is an address that indicates where an object’s variables, methods etc. are stored.
The objects are not actually used when assigned to a variable or passed as an argument to a method . The references to objects are used everywhere. Example:
Box mybox =  new Box();   //referencing to object
Role of an unreachable objects in java
In java, the memory allocated at runtime i.e. heap area can be made free by the process of garbage collection. It is nothing but just a method of making the memory free which is not being used by the programmer. Only the objects who have no longer reference to them are eligible for garbage collection in java.
Ways to make an object eligible for garbage collection:
Please note that the object can not become a candidate for garbage collection until all references to it are discarded.
1.	Object created inside a method : When a method is called it goes inside the stack frame. When the method is popped from the stack, all its members dies and if some objects were created inside it then these objects becomes unreachable or anonymous after method execution and thus becomes eligible for garbage collection
.Example:
/* Java program to demonstrate that 
objects created inside a method will becomes
eligible for gc after method execution terminate */
  
class Test
{
      
    // to store object name
    String obj_name;
      
    public Test(String obj_name) 
    {
        this.obj_name = obj_name;
    }
      
    static void show()
    {
        //object t1 inside method becomes unreachable when show() removed
        Test t1 = new Test("t1"); 
        display();
          
    }
    static void display()
    {
        //object t2 inside method becomes unreachable when display() removed
        Test t2 = new Test("t2"); 
    }
      
    // Driver method
    public static void main(String args[])
    {
        // calling show()
        show();
          
        // calling garbage collector
        System.gc();
    }
      
    @Override
    /* Overriding finalize method to check which object
    is garbage collected */
    protected void finalize() throws Throwable 
    {
        // will print name of object
        System.out.println(this.obj_name + " successfully garbage collected");
    }
}
Output:
t2 successfully garbage collected
t1 successfully garbage collected
2.	Note : If a method returns the object created inside it and we store this object reference by using a reference-type variable than it is no longer eligible for garbage collection.
3.	Reassigning the reference variable: When reference id of one object is referenced to reference id of some other object then the previous object has no any longer reference to it and becomes unreachable and thus becomes eligible for garbage collection.Example:
/* Java program to demonstrate gc
 when one object referred to other object */
  
class Test
{
    // to store object name
    String obj_name;
      
    public Test(String obj_name) 
    {
        this.obj_name = obj_name;
    }
      
    // Driver method
    public static void main(String args[])
    {
        Test t1 = new Test("t1");
        Test t2 = new Test("t2");
          
        //t1 now referred to t2
        t1 = t2;
          
        // calling garbage collector
        System.gc();
    }
      
    @Override
    /* Overriding finalize method to check which object
     is garbage collected */
    protected void finalize() throws Throwable 
    {
        // will print name of object
        System.out.println(this.obj_name + " successfully garbage collected");
    }
}
Output:
t1 successfully garbage collected
4.	Nullifying the reference variable : When all the reference variables of an object are changed to NULL, it becomes unreachable and thus becomes eligible for garbage collection.Example:
/* Java program to demonstrate gc
 when object reference changed to NULL */
  
class Test
{
    // to store object name
    String obj_name;
      
    public Test(String obj_name) 
    {
        this.obj_name = obj_name;
    }
      
    // Driver method
    public static void main(String args[])
    {
        Test t1 = new Test("t1");
           
        /* t1 being used for some purpose in program */
   
        /* When there is no more use of t1, make the object
           referred by t1 eligible for garbage collection */        
        t1 = null;
   
        // calling garbage collector
        System.gc();
    }
      
    @Override
    /* Overriding finalize method to check which object
     is garbage collected */
    protected void finalize() throws Throwable 
    {
        // will print name of object
        System.out.println(this.obj_name + " successfully garbage collected");
    }
}
Output:
t1 successfully garbage collected
5.	Anonymous object : The reference id of an anonymous object is not stored anywhere. Hence, it becomes unreachable.
Example:
/* Java program to demonstrate gc
 of anonymous objects */
  
class Test
{
    // to store object name
    String obj_name;
      
    public Test(String obj_name) 
    {
        this.obj_name = obj_name;
    }
      
    // Driver method
    public static void main(String args[])
    {
        //anonymous object without reference id
        new Test("t1"); 
   
        // calling garbage collector
        System.gc();
    }
      
    @Override
    /* Overriding finalize method to check which object
     is garbage collected */
    protected void finalize() throws Throwable 
    {
        // will print name of object
        System.out.println(this.obj_name + " successfully garbage collected");
    }
}
Output:
t1 successfully garbage collected

