# Discussion Section - Week 4

## C++ Review - References

References are used all over C++ and are a very important concept to be familiar with. This includes using references both
for function parameters and for return values from classes.

Below is a class similar to the `Circle` class from the lecture exercices. It includes two different ways of getting the value
for the radius of the circle.

```c++
class Circle
{
   private:
      double radius_;

   public:
      Circle(double r) : radius_(r) {}

      double get_radius_1() { return radius_; }

      double & get_radius_2() { return radius_; }
};
```

Here are a few code snippets. Without trying to compile the code, predict if the code will compile.
If the code compiles, what is the value of `r` (if it exists)
and the internal value of the `radius_` attribute at the end of the snippet? Why do you think that?

After predicting, try writing a program and see if you were correct.

1. ```c++
   Circle c(1.0);
   double r = c.get_radius_1();
   r = 4.0;
   ```

1. ```c++
   Circle c(1.0);
   double r = c.get_radius_2();
   r = 4.0;
   ```
   
1. ```c++
   Circle c(1.0);
   c.get_radius_1() = 2.0;
   ```

1. ```c++
   Circle c(1.0);
   c.get_radius_2() = 2.0;
   ```

1. ```c++
   Circle c(1.0);
   double & r = c.get_radius_1();
   r = 4.0;
   ```

1. ```c++
   Circle c(1.0);
   double & r = c.get_radius_2();
   r = 4.0;
   ```

## Understanding Object State Through Prediction

One of the most important concepts in object-oriented programming is understanding when and how an object's state should change. This exercise will help you develop intuition about designing methods that interact with object state.

Below is a simple `Counter` class that will help us explore these concepts:

``` c++
class Counter {
private:
    int value_;

public:
    Counter(int initial_value = 0) : value_(initial_value) {}
    
    int get_value() const { return value_; }
    
    void increment() { value_++; }
    
    // Method 1: Returns a calculation without changing state
    int calculate_sum_to(int target) const {
        int sum = 0;
        for (int i = value_; i <= target; i++) {
            sum += i;
        }
        return sum;
    }
    
    // Method 2: Changes the object's state during calculation
    int count_up_to(int target) {
        int sum = 0;
        while (value_ <= target) {
            sum += value_;
            value_++;
        }
        return sum;
    }
};
```

### Part 1: Predict the Behavior

For each code snippet below, predict what will happen before running the code. Consider:
- What is the value of the counter's internal `value_` at each step?
- What gets printed?
- Does the object's state change?

**Snippet 1:**
```c++
Counter c(3);
std::cout << "Initial: " << c.get_value() << std::endl;
int result = c.calculate_sum_to(5);
std::cout << "Sum result: " << result << std::endl;
std::cout << "Counter after calculation: " << c.get_value() << std::endl;
```
**Your Prediction:**
- Initial value_: ___
- Sum result: ___
- Final value_: ___

**Snippet 2:**
```c++
Counter c(3);
std::cout << "Initial: " << c.get_value() << std::endl;
int result = c.count_up_to(5);
std::cout << "Count result: " << result << std::endl;
std::cout << "Counter after counting: " << c.get_value() << std::endl;
```
**Your Prediction:**
- Initial value_: ___
- Sum result: ___
- Final value_: ___

**Snippet 3:**
```c++
Counter c(2);
std::cout << "First calculation: " << c.calculate_sum_to(4) << std::endl;
std::cout << "Second calculation: " << c.calculate_sum_to(4) << std::endl;
std::cout << "Counter value: " << c.get_value() << std::endl;
```
**Your Prediction:**
- Initial value_: ___
- Sum result: ___
- Final value_: ___

### Part 2: Test Your Predictions
Write a complete C++ program that tests each snippet above. Compile and run it to see if your predictions were correct.

### Part 3: Design Discussion

Work with your group to discuss these questions:
1. **Consistency:** Which method (`calculate_sum_to` vs `count_up_to`) gives consistent results when called multiple times? Why might this be important?


1. **Single Responsibility**: Notice that `calculate_sum_to` only returns a value, while `count_up_to both` returns a value AND changes the object's state. This violates a key design principle: methods should generally do one thing well.


   - What problems arise when a method does both?
   - How does this make the code harder to understand and debug?

1. **Side Effects**: A "side effect" occurs when a method changes something beyond just returning a value. `count_up_to` has the side effect of changing `value_`.


   - Why might side effects in calculation methods be problematic?
   - When might side effects be intentional and useful?

1. **Method Design**: How can you tell from a method's signature whether it will change object state? (Hint: look at the `const` keyword)


   - Why is `calculate_sum_to` marked const but `count_up_to` is not?
   - What promise does `const` make to users of your class?

1. **Application to Scientific Computing**: In scientific code, calculations should generally be **reproducible** and **predictable**.


   - Should a method that calculates potential energy also change the molecule's position?
   - What problems could arise if energy calculations had unexpected side effects?
   - How does this principle apply to your harmonic oscillator assignment?
 
### Reflection
Write a brief paragraph about what you learned regarding object state management. How will this influence how you design the methods in your Diatomic class?
