## Flex Direction

 Flexbox is all about controlling the distribution of elements in a row or column. By default, items will stack side-by-side in a row
 - With `flex-direction: row`, the _primary axis_ runs horizontally, from left to right.
 - When we flip to `flex-direction: column`, the primary axis runs vertically, from top to bottom.

**In Flexbox, everything is based on the primary axis.**

All of the rules are structured around this primary axis, and the _cross axis_ that runs perpendicularly.

we can switch seamlessly from horizontal layouts to vertical ones.

The children will be positioned by default according to the following 2 rules:

1. **Primary axis:** Children will be bunched up at the _start_ of the container.
2. **Cross axis:** Children will stretch out to fill the entire container.

![[Pasted image 20221216092143.png]]

## Alignment

### primary axis

We can change how children are distributed along the primary axis using the `justify-content` property:

We can bunch all the items up in a particular spot (with `flex-start`, `center`, and `flex-end`), or we can spread them apart (with `space-between`, `space-around`, and `space-evenly`).

### cross axis

We use the `align-items` property:

It's interesting… With `align-items`, we have _some_ of the same options as `justify-content`, but there isn't a perfect overlap.

![[Pasted image 20221216092810.png]]



### align-self

`align-self` is applied to the _child element_, not the container. It allows us to change the alignment of a specific child along the cross axis:
![[Pasted image 20221216093018.png]]
`align-self` has all the same values as `align-items`.
