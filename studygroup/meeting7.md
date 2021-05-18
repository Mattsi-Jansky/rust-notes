# Meeting 7- Lifetimes, tests

## Lifetimes (Liam presenting)

* "It takes the overlap of the two lifetimes"- like the diagram
  * It'll choose the lifetime that encompasses the other lifetimes
* "Result must be within the same lifetime as the shortest of the parameter lifetimes"
  * We thought a lot about how to phrase an explanation and this seems to make sense
* A bit confused about where the compiler draws the line between what it can imply and what it can't- in the example in the chapter it seems like it would be trivial to infer the correct answer, because there are only a few possibilities and only one of them is valid.

## Unit testing (Ivan presenting)

* Unit tests live in same placeas library
  * That way they have access to the private fields of the objects under test
  * But there are other ways that could have been implemented- why did they decide to put them in the same folder?
