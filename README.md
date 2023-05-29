

# Rust Basics

Rust is strongly typed and statically typed language.
- strongly typed: all variable type known and checked during compilation.
- strongly typed: type conversions need's to be explicit otherwise will result in compilation error. 
##### rust using an ownership system in order to maintain security of software. 


## Ownership
- set of rules of how a rust program manages memory. 
- strict rules that insures memory safety.
- alternative to a garbage collector. 

| garbage collector | Ownership | 
|-----------------|-----------------|
|software that during runtime checks for unused data  |release data based on program structure  |
| slow, software in the background | more efficient |

### rules of ownership: 
- any data has a unieqe "Owner"
- owner that goes out of the scope gets dropped.
- binding variables moves ownership 
- there are mutable references and immutable references (only 1 mutable reference or as many immutable references)

```
let mut t = String::from("hello"); //t is the owner
let x = t; // x is the new owner and t is not inuse. 
```
   
## Enums and Pattern Matching 
- similar to algebric data type in functional programming languages.
new type that has a finite number of variants. 
each variant can hold data using () notation. 

Enums enables pattern matching for more safe code: 
in pattern matching every possible variant should be considered
rust implement pattern matching in match keyword 
```
enum Color {  
    Blue,  
    Green,  
    Yellow,  
  
}  
fn main() {  
    let color = Color::Blue;
      
    let y = match color {  
        Color::Blue => true, // case color is variant blue 
        _ => false  // anything other then blue will match _ 
  };  
}
```

## Error Handling
###  alternative to NULL using Enum

- rust doesn't have a null value. 
- using an Option< T> enum instead, Like (Maybe a) in haskell. 
```
Option<T> {
	some(T),
	None,
}
```

#### there are two types of errors 

| Recoverable Errors   | Unrecoverable Errors | 
|-----------------|-----------------|
|handling Result<T, E> or Option<,T>  |panic!|
| Any Recoverable error |index out of bound etc|

```
enum Result<T, E> { 	 
	Ok(T),
	Err(E), // E is an error object that have methods based on the error returned
}
```

- some operations in rust returns Result or Option Enum
- the use of Enum as error handling mechanism force you to explicitly check all cases 
- handle using match 
- more readable code and safer code 

####  shortcuts for match handling
.unwrap() returns T of OK(T) if works else panic!
.expect (&str) panic! and return the message passed to the expect.
? returns T of OK(T) if works else return the error.



## Genetic Types, Traits, and Lifetimes
#### syntax
functions:
```
fn largest<T>(list: &[T]) -> &T {}// largest accepts array that contains any type. 
```
enums:
```
enum Result<T, E> { 	 
	Ok(T), // can hold any type value T
	Err(E), // E is an error object that have methods based on the error returned
}
```
### Traits:
- similar to interface in java (more similar to typeclass in haskell). 
- traits are used to define behiviour of types allowing for polymorphism and overloading functions.
- enables to define generic function with constrains on types that implement some trait (iterables, etc..)
#### we define trait using this syntax 
```
pub trait Summary { 
	fn summarize(&self) -> String;
}
```
each type can implement the trait Summery and has to implement all functions of that trait. 

traits as a parameters: 
```
pub  fn notify(item: &impl Summary) {
	println!("Breaking news! {}", item.summarize()); 
}
```
notify is a function that accepts as input only types that implement Summary trait.  


``pub  fn notify<T: Summary + Display>(item: &T) {}``
notify is a function that accepts as input only references to types that implement Summary trait **and** Display trait.  


