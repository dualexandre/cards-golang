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

### Slices are zero-indexed
```go
fruits := []string {"apple", "banana", "grape", "orange", "peach"}
fmt.Println(fruits[0]) // Output: apple 
fmt.Println(fruits[3]) // Output: orange 

// Syntax: fruits[start(Included):end(Not Included)].
fmt.Println(fruits[0:2]) // Output: [apple banana] -> Includes: 0, 1. Not includes: 2.
fmt.Println(fruits[:2]) // Output: [apple banana] -> It takes from index 0 to index 2, excluding index 2.
fmt.Println(fruits[2:]) // Output: [grape orange peach] -> Take from index 2 to the end of the slice.
fmt.Println(fruits[1:3]) // Output: [banana grape] -> Includes: 1, 2. Not includes: 3.
```

### Function that returns two variables
```go
func deal(d deck, handSize int) (deck, deck) {
	return d[:handSize], d[handSize:]
}

// How to use:
cards := newDeck()
hand, remainingCards := deal(cards, 5)
hand.print()
remainingCards.print()

// Output hand:
// 0 Ace of Spades
// 1 Two of Spades
// 2 Three of Spades
// 3 Four of Spades
// 4 Five of Spades

// Output remainingCards:
// 0 Six of Spades
// 1 Seven of Spades
// 2 Eight of Spades
// 3 Nine of Spades
// 4 Ten of Spades
// 5 Jack of Spades
// 6 Queen of Spades
// 7 King of Spades
// 8 Ace of Diamonds
// 9 Two of Diamonds
// 10 Three of Diamonds
// ...
```

### Byte
```go
hello := "Hi there!"
fmt.Println([]byte(hello)) // Transform to byte. Output: [72 105 32 116 104 101 114 101 33]
```

### Turn a slice of strings into a single string
```go
func (d deck) toString() string {
	return strings.Join([]string(d), ",") // Join concatenates the elements of its first argument to create a single string. The separator string sep is placed between elements in the resulting string.
}

// Output: Ace of Spades,Two of Spades,Three of Spades,Four of Spades,Five of Spades,Six of Spades,Seven of Spades,Eight of Spades...
```

### os.ReadFile & strings.Split
```go
func newDeckFromFile(filename string) deck {
	// ReadFile: Transforms a file into a byte array. [65 99 101 32 111 102 32 83 112 ...]
	byteSlice, err := os.ReadFile(filename) 
	if err != nil {
		fmt.Println("Error:", err)
		os.Exit(1)
	}
	// string(byteSlice): Creates a string from the bytes it receives (separated by a comma). Ace of Spades,Two of Spades,Three of Spades,Four of Spades, ...
	sliceString := strings.Split(string(byteSlice), ",")
	// Split(string(byteSlice), ","): Transforma a string separada por vírgulas em um array. [Ace of Spades Two of Spades Three of Spades Four of Spades ...]
	return deck(sliceString)
	// deck(): Add type deck to array.
}
```

### Shuffle method
```go
func (d deck) shuffle() {
	source := rand.NewSource(time.Now().UnixNano())
	r := rand.New(source)
	for i := range d {
		newPosition := r.Intn(len(d) - 1)
		d[i], d[newPosition] = d[newPosition], d[i]
	}
}
// Line-by-line explanation
// source := rand.NewSource(time.Now().UnixNano()): This line creates a new source of random numbers using the current time. rand.NewSource() returns a new source of random numbers that generates distinct values on each execution, while time.Now().UnixNano() returns the current time in nanoseconds. This ensures that every time the program is run, the random number source is initialized differently.
-
// r := rand.New(source): Here, a new random number generator (r) is created using the random number source created earlier.
-
// newPosition := r.Intn(len(d) - 1): Here, a random number is generated representing the new position of the card currently at i. r.Intn() generates a non-negative integer less than the provided argument (which is the size of the deck - 1).
-
// d[i], d[newPosition] = d[newPosition], d[i]: This line swaps the positions of the cards at the current position (i) and the new position (newPosition). This effectively shuffles the cards.
```