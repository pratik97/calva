# Formatting

We have tried to make Calva's formatter so that it _just works_. It is enabled by default for Clojure files, and unconfigured it mostly follows Bozhidar Batsov's [Clojure Style Guide](https://github.com/bbatsov/clojure-style-guide). Calva uses [cljfmt](https://github.com/weavejester/cljfmt) for the formatting.

With the default settings, Calva's formatting behaves like so:

* formats as you type (when entering new lines)
* formats the current enclosing form when you hit `tab`
* formats pasted code
* formats according to community standards (see above link)
* formats the current form, _aligning map key, values_, when you press `ctrl+alt+l`.

Also: If you have **Format on Save** enabled in VS Code, it will be Calva doing the formatting for Clojure files.

Calva's formatting is mostly about indenting, but it also (again, defaults):

* trims whitespace at the end of the line
* trims whitespace inside brackets
  * this also folds trailing brackets (a k a _the paren trail_) up on the same line
* inserts whitespace between forms

 Not a fan of some default setting? The formatter is quite configurable.

 ## Configuration

 You configure Calva's formatting using [cljfmt's configuration EDN](https://github.com/weavejester/cljfmt#configuration). This means that you can adjust the mentioned defaults, including the indenting.

 To start changing the defaults, paste the following map into a file and save the file somewhere in the project workspace:

```clojure
{:remove-surrounding-whitespace? true
 :remove-trailing-whitespace? true
 :remove-consecutive-blank-lines? false
 :insert-missing-whitespace? true
 :align-associative? false}
```

Then set `calva.fmt.configPath` to the path to the file. The path should be relative to the project root directory. So, if you named the file `.cljfmt.edn` and saved it in the root of the project, then this setting should be `.cljfmt.edn`.

Since you are editing the file in Calva (you are, right?), you can quickly test how different settings affect the formatting. Try set `:align-associative` to `true`, then save, then hit `tab`, and see what happens. (This particular setting is experimental and known to cause troubles together with namespaced keywords. Consider using `ctrl+alt+l` instead of `tab` as your formatting command, instead of enabling this setting.)

### Indentation rules

The `cljfmt` indents are highly configurable. They, and the rest of the configuration options are masterly detailed [here](https://github.com/weavejester/cljfmt#configuration).

Again, Calva is an extra good tool for experimenting with these settings. `cljfmt` doesn't care about keys in the map that it doesn't now about so you can sneak in test code there to quickly see how it will get formatted by certain rules. Try this, for instance:

```clojure
{:remove-surrounding-whitespace? true
 :remove-trailing-whitespace? true
 :remove-consecutive-blank-lines? false
 :insert-missing-whitespace? false
 :align-associative? false
 :indents ^:replace {#"^\w" [[:inner 0]]}
 :test-code
 (concat [2]
         (map #(inc (* % 2))
              (filter #(aget sieved %)
                      (range 1 hn))))}
```

Save, then hit `tab`, and the code should get formatted like so:

```clojure
 :test-code
 (concat [2]
    (map #(inc (* % 2))
      (filter #(aget sieved %)
        (range 1 hn))))
```

That's somewhat similar to Nikita Prokopov's [Better Clojure Formatting](https://tonsky.me/blog/clojurefmt/) suggestion. (Pleae be aware, that setting might not be sufficient to get complete **Tonsky Formatting**, please share any settings you do to get full compliance.)

## Under Construction

Much of this formatting configurability is newly released. There might be dragons. And also, we probably should make Calva pick the `:cljfmt` config up from Leiningen project files. If you agrre, and there isn't an issue about that already, please file one.