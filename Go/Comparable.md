Compare two things in Go



```flow
st=>start: Start
cond=>condition: Are they same types?
eDif=>end: Different
op=>operation: Match
e2=>end: Match
condPrim=>condition: Are there primitive types?
condType=>condition: Only contain predictable types?

content=>condition: Are they same content?
content2=>condition: Are these predictable type same?

st->cond
cond(no)->eDif
cond(yes)->condPrim


condPrim(no)->condType
condPrim(yes)->content2

condType(no)->eDif
condType(yes)->content

content(no)->eDif
content(yes)->e2

content2(no)->eDif
content2(yes)->e2



```

