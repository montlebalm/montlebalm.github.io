---
title: "Swift Techniques: Strings"
layout: single
summary: "Tips and tricks for manipulating strings in Swift."
---

Sometimes manipulating strings can be a little cumbersome in Swift. Here's a handful of
techniques you might find useful.

### Split

Split a string into an.

```swift
func splitString(input: String) -> [String] {
	var split = [String]()
	for char in input {
		split.append(String(char))
	}
	return split
}
let letters = splitString("abcdefg")
// letters = ["a", "b", "c", "d", "e", "f", "g"]
```

### Trim whitespace

Remove all leading and trailing whitespace from a string.

```swift
let spaceSet = NSCharacterSet.whitespaceCharacterSet()
let trimmed = "  goo  ".stringByTrimmingCharactersInSet(spaceSet)
// trimmed == "goo"
```

There are [lots of other character
sets](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/nscharacterset_Class/Reference/Reference.html) including:

* alphanumericCharacterSet
* whitespaceCharacterSet
* symbolCharacterSet
* punctuationCharacterSet

In addition, you can get the opposite of any character set by adding `.inverted`
to the end.

### Remove substrings

Remove all occurrences of a substring by using a regular expression.

```swift
let dirty = "Pass the $!#@ salsa please"
let clean = input.stringByReplacingOccurrencesOfString(
  "[\\$\\!\\#\\@]",
  withString: "",
  options: .RegularExpressionSearch)
// clean == "Pass the salsa  please"
```

### Remove duplicate whitespace

We can also use `RegularExpressionSearch` to remove duplicate whitespace characters.

```swift
let fitted = "Too    much".stringByReplacingOccurrencesOfString(
  "\\s+",
  withString: " ",
  options: .RegularExpressionSearch)
// fitted == "Too much"
```

### Substring

Get a substring starting from the beginning of the string until index X.

```swift
let haystack = "You only care about yourself"
let endIndex = advance(haystack.startIndex, 3)
let needle = haystack.substringToIndex(endIndex)
// needle == "You"
```

Get a substring from index X to the end of the string.

```swift
let haystack = "Jump to the end"
let startIndex = advance(haystack.startIndex, 12)
let needle = haystack.substringFromIndex(startIndex)
// needle == "end"
```

Get a substring from index X to index Y.

```swift
let haystack = "Where's Waldo hiding?"
let startIndex = advance(haystack.startIndex, 8)
let endIndex = advance(startIndex, 5)
let range = Range(start: startIndex, end: endIndex)
let needle = haystack.substringWithRange(range)
// needle == "Waldo"
```

### Prefix/suffix

Check whether a string starts with a given substring.

```swift
if "Asking or telling?".hasSuffix("?") {
	println("He's asking")
}
```

Similarly, we can check whether a string ends with a substring.

```swift
if "Dude, I know".hasPrefix("Dude") {
	println("Dude...")
}
```

### Check for substring

Find the range of a string inside another string. This can also act as a Regex `test` method when passed the `.RegularExpressionSearch` option.

```swift
let range = "Buy 2 magic beans".rangeOfString(
  "[0-9]",
  options: .RegularExpressionSearch)
// range == Range(4, 5)
```

## Further reading

Here are some articles on Swift techniques I found helpful:

* [Official Swift blog](https://developer.apple.com/swift/blog/) by Swift
    developers
* [Regex in Swift](http://benscheirman.com/2014/06/regex-in-swift/) by Ben
    Scheirman

More tips to come as I discover them. Feel free to send me feedback at
[chris@chrismontrois.net](mailto:chris@chrismontrois.net?Subject=Swift Techniques: Strings)
