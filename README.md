# Rules for formatting Clojure code

## General rules
Indent using 2 spaces.

Align similar to semantically similar.

Feel free to use formatting tools under the supervision of the developer, but do not enforce a format with automatic tools at later stages.

### Function/macro arguments
In general, indent arguments using 2 spaces:

```clojure
; Visually enforces the filter predicate and the operand.
(filter even?
  (range 1 10))

; Threading macros are not special.
(-> (range 1 10)
  (filter even?)
  (map inc))
```

You may want to align arguments that are semantically similar:

```clojure
(or (:props result)
    {:count 0})
    
; Not appropriate. The first arg of threading macros is not similar to the rest.
(-> (range 1 10)
    (filter even?)
    (map inc))
```

### List and map literals
Indent with 1 space, for elements to align:

```clojure
; List
[1
 2
 3]

; Map
(sql/format
  {:select :*
   :from :users
   :where [:= :id 
              user-id]}) ; exception for applying semantic alignment
```

### Avoid using tabular formatting

In general, avoid using tabular formatting. In particular, don't do it in binding lists and map literals.

```clojure
(let [n 1
      counter 4]
  ...)

{:n 1
 :counter 4}

; Avoid
(let [n       1
      counter 4]
  ...)
  
; Avoid
{:n       1
 :counter 4}
  
; Acceptable (rare cases)
(def test-data
  ["Mary"        37 :blue]
  ["Bartholomew" 9  :vermilion])
```

## Principles
Make semantics apparent.

Enforce the minimal set of rules that make developers not step onto each other with edit-time tooling.

Horizontal space is scarce.

## More examples

```clojure
(http/post "/api/user"
  {:body post-body
   :auth user-id}))
   
(assoc user :name new-name
            :age new-age)

(for [user users]
  (update user :friends
    #(conj % (assoc friend 
               :position (count %)
               :mark? true]

(throw
  (ex-info "Inconsistent number of foozers" 
    {:id id
    :count n}))
    
(merge-with into
  (results)
  {:users new-users
   :notes new-notes})
```

## Experience
Expresiveness of Clojure code depends on formatting decisions by the developer. This is different from most other languages, which have more tokens and syntactic rules.

Clojure code is easily written in its final form, and restructuring is easily taken care of by edit tooling (like Parinfer and such).

Therefore, avoid postformatting tools. (Uniformity of code is overvalued?).

## Comparison with other proposals

### https://github.com/bbatsov/clojure-style-guide

1-space indentation is not clear enough for the eye.

The reasoning applied is contradictory. The guide says:

> [...] the reasoning behind it is quite simple. Function calls are nothing but regular list literals and normally those are aligned in the same way as other collection type literals when spanning multiple lines.

...but it later applies semantic indentation to "body" arguments.

Also, I don't see a need for distinguishing purely "body" arguments from other arguments. Some arguments that seem "ordinary", can also be semantically viewed as a "body". (DB queries).

<table>
<tr>
<th>meh?
<th>better
</tr>
<tr>
<td>

```clojure
(jdbc/execute-one! 
 db
 (sql/format
   {:select :*
    :from :users
    :where [:= :id user-id]}))
```
      
</td>
<td>
      
```clojure
(jdbc/execute-one! db
  (sql/format
    {:select :*
     :from :users
     :where [:= :id user-id]}))
```
    
</td>
</tr>
</table>

### https://tonsky.me/blog/clojurefmt/

Agree with the general 2-space indentation rule.

Disagree on the need of an automatic Clojure formatter.
