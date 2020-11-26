### 统计行数

```go
input:="Spicy jalapeno pastrami ut ham turducken.\n Lorem sed ullamco, leberkas sint short loin strip steak ut shoulder shankle porchetta venison prosciutto turducken swine.\n Deserunt kevin frankfurter tongue aliqua incididunt tri-tip shank nostrud.\n"

scanner := bufio.NewScanner(strings.NewReader(input))
// Set the split function for the scanning operating
scanner.Split(bufio.ScanLines)
// Count the words.
count := 0
for scanner.Scan() {
  count++
}
if err := scanner.Err(); err != nil {
  fmt.Fprintln(os.Stderr, "reading input: %g", err)
}
fmt.Printf("%d\n", count)
```



### 统计单词数

```go
scanner := bufio.NewScanner(strings.NewReader(input))
// Set the split function for the scanning operation
scanner.Split(bufio.ScanLines)

count:=0
for scanner.Scan() {
  count++
}

if err := scanner.Err(); err != nil {
  fmt.Fprintln(os.Stderr, "reading input: %g", err)
}
```

