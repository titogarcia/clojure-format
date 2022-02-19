# Rationale

The reasons here arise from sensibilities developed through practice.

## Some additional principles

Horizontal space is scarce.

Enforce the minimal set of rules that make developers not step onto each other with editing tools.

## 2-space indentation

Because

- It makes semantics apparent. The name of a function/macro is different from its args, so it is always beneficial to have it at a higher indentation-level.

- 1-space is not clear enough to make code structure apparent.

## Avoid post-formatting code

Clojure developers use code formatting deliberately for expressing program semantics (symmetries, similarities, importance, closeness, etc), which automatic formatters can't infer. Therefore, invasive formatters get in the way of producing the best-quality code that developers are able to produce.

Developer expressiveness has more value than ultimate code uniformity.

Clojure code is easily written in a uniform, final form, optionally assisted by tools for structured editing. There is little need to apply formatters in the aftermath, without the developer's intervention.

## Avoid tabular formatting

In my experience, most uses of tabular formatting just bring visual satisfaction, instead of helping make the meaning of the code apparent.

In particular, using tabular formatting in binding lists or maps is usually not appropriate, because:
- It makes values align to non-related values.
- It separates values form the assigned variable/keyword.
- It consumes more horizontal space.
- It adds formatting dependencies between lines of code.
- It may be more cumbersome to maintain.

## Comparisons with other proposals

### https://github.com/bbatsov/clojure-style-guide

The guide proposes 1-space indentation for the arguments of function/macros. The guide says:

> [...] the reasoning behind it is quite simple. Function calls are nothing but regular list literals and normally those are aligned in the same way as other collection type literals when spanning multiple lines.

But this _syntactic_ reasoning is not consistent with the reasoning applied to "body" arguments, for which it proposes a 2-space _semantic_ indentation. 

I find the reasons for using 2-space indentation to be of more weight than the ability of distinguishing purely "body" arguments from other arguments.

<table>
<tr>
<th>😊
<th>🤨
</tr>
<tr>
<td>

```clojure
(jdbc/execute-one! db
  (sql/format
    {:select :*
     :from :users
     :where [:= :id 
                user-id]}))
```

</td>
<td>

```clojure
(jdbc/execute-one! 
 db
 (sql/format 
  {:select :*
   :from :users
   :where [:= 
           :id 
           user-id]}))
```

</td>
</tr>
</table>

The guide also proposes special indentation for threading macros. Here I don't consider those a special case.

### https://tonsky.me/blog/clojurefmt/

Agree with the general 2-space indentation rule.

But here I propose that the developer can override it when needed.

Disagree on the need of an automatic Clojure formatter.
