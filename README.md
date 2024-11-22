# Scalafmt config

This [scalafmt](https://scalameta.org/scalafmt) config attempts to preserve [Laminar](https://github.com/raquo/laminar) / [Airstream](https://github.com/raquo/airstream) / com.raquo formatting style as much as possible.

Where scalafmt allows, we generally let the programmer do what they want (e.g. `newlines.source = keep`), instead of prescribing a rigid style. We're trying to preserve the style from unwanted changes by scalafmt, not the programmer.

We do choose a few prescriptive options ourselves, such as rewriting unicode `⇒` into good old `=>`. You may want to overwrite those in your `.scalafmt.conf` if you find it disagreeable.


## How to use

This config should work with both Scala 2 and Scala 3, including braceless syntax.

In addition to the settings I already defined, you **must** also define [runner.dialect](https://scalameta.org/scalafmt/docs/configuration.html#scala-dialects).

You can simply copy-paste this config into your project and adjust it as desired, or, if you don't want to copy-paste it across many projects, you can include this `.scalafmt.shared.conf` file into your local `.scalafmt.conf` file by reference, for example:

```
include ".scalafmt.shared.conf"

# Easily add your own rules, overriding the shared configs
runner.dialect = "scala213"
version = 3.8.3
project.excludePaths = [
  "glob:**/src/main/scala/com/raquo/laminar/defs/**",
  "glob:**/project/VersionHelper.scala"
]
```

The `include` directive does not allow URLs, so it will need to be a local file, or a symlink to a local file. For myself, I made a source downloader script configured via sbt. You can see it [Laminar](https://github.com/raquo/laminar) repo – see the usage of `SourceDownloader` in `build.sbt`. It's published as part of [buildkit](https://github.com/raquo/buildkit).


## Limitations and workarounds

Unfortunately, scalafmt is sometimes too invasive and not flexible enough to get me what I want.

For example, despite `newlines.source = keep`, it still removes some of my newlines. Depending on the specific case, configuring it differently is either impossible, or results in the opposite problem, where it forcefully adds unwanted newlines.

One way to prevent scalafmt from removing newlines is to insert empty `//` comments where you want to keep a newline. It works not only to preserve vertical space in between sections of code, but also to e.g. ensure that callback arguments stay on their own line.

```scala
for 
  foo <- fooF
yield //
  foo
```

(Without this `//`, scalafmt puts `foo` on the same line as `yield`)

In some cases, scalafmt will insist on adding a newline where we don't want one. Usually around various types of function calls with lambda callbacks. There is no general fix for this, but you may want to try adjusting the braces / parens that you use in that case to achieve the desired outcome.

Some things are not fixable, e.g. scalafmt will insert a newline after the type argument when calling a polymorphic function types, and as far as I can tell, there's nothing reasonable I can do to change that.


## Contributions

I'm always looking for better ways for format code, but in this matter specifically, "better" is strictly according to myself. I am very unlikely to accept large changes, only small fixes, and even then, only if they align with my preferences. If you want to submit a PR, I suggest running it on the [Airstream](https://github.com/raquo/airstream) repo to see what it will change. You can always chat me up in [Laminar discord](https://discord.gg/JTrUxhq7sj), for this purpose – in the `#frontend-random` channel.


## Versioning

SemVer does not apply. Every version of this project will change how your code is formatted.


## License

To the maximum extent allowed by the law, this project is released into the public domain.

Alternatively, if you wish, you may also use it under [MIT license](https://opensource.org/license/mit) (Copyright 2024 Nikita Gazarov)
