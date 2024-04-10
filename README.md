## Card - Code examples

### Basic Go Types
```go
bool, string, int, float64
```

### Declaration of variables
```go
var card = "Ace of Spades" // -> Without typing.
var card string = "Ace of Spades"
card := "Ace of Spades" // -> In this case, the variable should not be typed, it will be typed implicitly.
card = "Five of Diamonds" // -> You can reassign the value of the variable, as long as the value is of the same type.
```

### append() Method
```go
cards := []string{"Ace of Diamonds", newCard()}
fmt.Println(cards) // Output -> [Ace of Diamonds Five of Diamonds]
cards = append(cards, "Six of Spades") // Similar to array_push.
fmt.Println(cards) // Output -> [Ace of Diamonds Five of Diamonds Six of Spades]
```

### Function declaration
```go
func newCard() string { // It is mandatory to type the function return.
	return "Five of Diamonds"
}
```

### range
```go
cards := []string{"Ace of Diamonds", newCard()}
cards = append(cards, "Six of Spades")
for i, card := range cards { // The i corresponds to the slice index. The card corresponds to the value.
	fmt.Println(i, card)
}

// Output:
// 0 Ace of Diamonds
// 1 Five of Diamonds
// 2 Six of Spades
```

### Create a new type of 'deck'
```go
type deck []string
// Before the creation of the 'deck' type.
cards := []string{"Ace of Diamonds", newCard()}

// After creating the 'deck' type.
cards := deck{"Ace of Diamonds", newCard()}
```

### Receiver Function
```go
// The 'd' is the receiver of the method and the 'deck' is its type. Similar to 'this' in other languages.
func (d deck) print() { 
	for i, card := range d {
		fmt.Println(i, card)
	}
}

// How to use:
// Before:
cards := deck{"Ace of Diamonds", newCard()}
cards = append(cards, "Six of Spades")
for i, card := range cards {
	fmt.Println(i, card)
}

// After:
cards := deck{"Ace of Diamonds", newCard()}
cards = append(cards, "Six of Spades")
cards.print()
```

### Variable not used
```go
// The range method first returns the index of the slice. If this value is not used, use "_" instead.
for _, suit := range cardSuits {
	for _, value := range cardValues {
		cards = append(cards, value+" of "+suit)
	}
}
```
