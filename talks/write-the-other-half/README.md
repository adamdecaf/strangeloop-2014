# Write the Other Half of Your Program: From Functional to Logic Programming

## Goals

- Use functions to express everything.
  - We don't need special forms. Can re-write languages in logic programming concepts

## Let programs succeed

```scheme
> (run* (q) (== q 'b'))
'(b)
> (run* (q) (fresh (y) (== q y)))
'(_.0)
> (run* (q) (fresh (y x)
              (conde
                ((== q y))
                ((== y x))
                ((== x 'cat'))
              )))
'(_.0, _.0, _.0)
> (run* (q) (fresh (y x)
              (conde
                ((=/= q y))
                ((== y x))
                ((== x 'cat'))
              )))
'((_.0 (=/= ((_.0 dog)))))
```

## Appending

```scheme
> (run 1 (q) (appo '(a b c) '(d c e) q))
'((a b c d c e))
```
