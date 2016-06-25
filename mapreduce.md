<!-- Michel Caradec -->

# MapReduce logic in different programming languages

## R

Operation: Sum of squares.

```r
m <- Map(function(x, p) { x ** 2}, 1:4)
Reduce("+", m)
```

Using **forward pipe** operator:

```r
library(dplyr)
Map(function(x, p) { x ** 2}, 1:4) %>% Reduce("+", .)
```

Output: **30**

## Python

Operation: Sum of squares.

```python
m = map(lambda x: x ** 2, range(1, 5))
reduce(lambda x, y: x + y, m)
```

Output: **30**

## CSharp

Operation: word count (using **Linq**).

```csharp
string text = "Tous les êtres humains naissent libres et égaux en dignité et en droits."
  + " Ils sont doués de raison et de conscience et doivent agir les uns envers les autres dans un esprit de fraternité.";
var words = Regex.Replace(text, @"\p{P}", " ")
     .Split()
     .Where(w => !string.IsNullOrWhiteSpace(w))
     .Select(word => word.ToLower().Trim());

var map = words.Select(word => new { Word = word, Count = 1 });
var shuffle = map.GroupBy(m => m.Word);
var reduce = shuffle.Select(s => new { Word = s.Key, Count = s.Select(t => t.Count).Sum() });

var top5 = reduce.OrderByDescending(word => word.Count).Take(5);
```

Output:

| Word | Count |
|------|-------|
| et   | 4     |
| les  | 3     |
| de   | 3     |
| en   | 2     |
| tous | 1     |
