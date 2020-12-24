### Variable Name Guidelines

* Never a single letter
* Always specific
* Ideally 1-2 words
* Booleans prefixed with "is", for example `isActive` or `isValid`

* use camelCase
* use ALL_CAPS with uderscores for constants



### Method Name Guidelines

* Should reveal intent
* Functionality fully understandable from the name

If you have to look inside the method to understand what it does - the name needs improvement



### Method Name Anti-patterns

Method does more than the name says

Name contains "and", 'or', 'if'



```java
getSalesData() {
  // query DB
  // format data
  // precalculate
}

// separate it
getSalesData();

formatSalesDate(data);

preCalcSalesData(data);
```



## Breaking Method Name Rules

* Static Factory Methods

* Builder and Fluent Interface patterns





Don't use abbreviations



universal is ok

```
kg km 
```

Careful with typos and spelling

### Summary

* Classes - Single Responsibility
* Variables - descriptive and concise
* Methods - reveal intent and no multi-tasking