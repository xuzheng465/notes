Rust标准库中并没有包含随机数字功能。



`crate` is a collection of Rust source code files.





##### [Cargo build hangs with “ Blocking waiting for file lock on the registry index” after building parity from source](https://stackoverflow.com/questions/47565203/cargo-build-hangs-with-blocking-waiting-for-file-lock-on-the-registry-index-a)



```bash
# option 1
pkill rls

# option 2
rm -rf ~/.cargo/registry/index/*

~/.cargo/.package-cache
```

What is trait?



The `gen_range` method takes two numbers as arguments and generates a random number between them. It’s **inclusive** on the lower bound but **exclusive** on the upper bound.



```rust
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main() {
  println!("Guess the number!");
  
  println!("The secret number is {}", secret_number);
  
  loop {
    println!("Please input your guess.");
    let mut guess = String::new();
    io::stdin()
    		.read_line(&mut guess)
    		.expect("Failed to read line");
    let guess: u32 = match guess.trim().parse() {
      Ok(num) => num,
      Err(_) => continue,
    };
    
    println!("You guessed: {}", guess);
    
    match guess.cmp(&secret_number) {
      Ordering::Less => println!("Too small!"),
      Ordering::Greater => println!("Too big"),
      Ordering::Equal => {
        println!("You win!");
        break;
      }
    }
  }
}
```



`parse` returns a `Result` type and `Result` is an enum that has the variants `Ok` or `Err`. 



`_` in the `Err(_)` is a catchall value.