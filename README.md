# Rules for formatting Clojure code

Short list of rules that I find most important.

For the reasons behind, see the [rationale](rationale.md).

## General rules

- Indent using 2 spaces.

- Align semantically similars.

- Do not use tools that verify or enforce code format without the developer's intervention.

## Applications

### Function/macro arguments

In general, indent arguments using 2 spaces:

```clojure
; ๐
(filter 
  even?
  (range 1 10))

; ๐ Visually enforces the filter predicate and the operand.
(filter even?
  (range 1 10))

; ๐ Threading macros are not special.
(-> (range 1 10)
  (filter even?)
  (map inc))
```

You may want to align arguments that are semantically similar:

```clojure
; ๐
(or (:props result)
    {:count 0})
    
; ๐คจ Not appropriate.
; The first arg of a threading macro (an expression)
; is not similar to the rest (forms to evaluate).
(-> (range 1 10)
    (filter even?)
    (map inc))
```

### List and map literals
Indent with 1 space, for elements to align:

```clojure
; ๐ List
[1
 2
 3]

; ๐ Map
(sql/format
  {:select :*
   :from :users
   :where [:= :id 
              user-id]}) ; exception for applying semantic alignment
```

### Avoid using tabular formatting

In general, avoid using tabular formatting.

```clojure
; ๐
(let [n 1
      counter 4]
  ...)

; ๐
{:n 1
 :counter 4}

; ๐คจ Not appropriate. The values are not related to each other.
; They are better written closer to their binding name.
(let [n       1
      counter 4]
  ...)

; ๐คจ Not appropriate. The values are not related to each other.
; They are better written closer to their key.
{:n       1
 :counter 4}
  
; ๐ Acceptable (rare cases)
(def test-data
  ["Mary"        37 :blue]
  ["Bartholomew" 9  :vermilion])
```

### More examples

```clojure
; ๐
(http/post "/api/user"
  {:body post-body
   :auth user-id})
   
; ๐
(assoc user :name new-name
            :age new-age)

; ๐
(for [user users]
  (update user :friends
    #(conj % (assoc friend 
               :position (count %)
               :mark? true))))

; ๐
(throw
  (ex-info "Too many users in this group" 
    {:group-id id
     :users-count n}))

; ๐
(merge-with into
  (results)
  {:users new-users
   :notes new-notes})
```
