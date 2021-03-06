A stream is an abstraction for input/output









##### getline

read a whole line

```c++
getline(istream& stream, string& line);
```



Don't mix `>>` with `getline`

* `>>` reads up to the next whitespace character and does not go past that whitespace character
* `getline` reads up to the next delimiter (by default `\n`), and does go past that delimiter



```c++
stream << std::flush; // flush what we have so far
stream << std::endl;	// flush with newlines
// this is equivalent to: stream << "\n" << std::flush;
```



##### Buffer takeaways

* The internal sequence of data stored in a stream is called a buffer
* `istream` use buffers to store data we haven't used yet.
* `ostream` use buffers to store data that hasn't been outputted yet.



streams have four state bits

* **Good bit**: whether ready for read/write
* **Fail bit**: previous operation failed, future operations frozen
* **EOF bit**: previous operation reached end of file
* **Bad bit**: external integrity error



##### Chaining >> and <<

`>>` and `<<` are actually functions!

```c++
std::ostream& operator<<(std::ostream& out, const std::string& s);

std::cout << "Hello";
operator<<(std::cout, "Hello");

cout<<"test"<<5;
(cout<<"test")<<5;

operator<<(operator<<(cout, "test"), 5);
operator<<(cout, 5);
cout;
```



##### using state bits

```c++
while (true) {
  stream >> temp;							// read data
  if (stream.fail()) break;		// checks for fail bit OR bad bit
  doSomething(temp);
}

// same as

while (true) {
  stream >> temp;  // this returns the stream itself
  if (!stream) break;
  doSomething(temp);
}

// here's a very common read loop
while (stream >> temp) {
  doSomething(temp);
}
```



