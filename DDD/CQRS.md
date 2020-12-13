https://martinfowler.com/bliki/CQRS.html



CQRS stands for **Command Query Responsibility Segregation**. It's a pattern that I first heard described by [Greg Young](https://twitter.com/gregyoung). At its heart is the notion that you can use a different model to update information than the model you use to read information. For some situations, this separation can be valuable, but beware that for most systems CQRS adds risky complexity.

你可以使用不同的模型去更新信息，而不是那个你用来读取信息的模型。对于某些情况下，这种分离是很有价值的，但要注意，对于大多数系统来说，CQRS会增加风险复杂度。

