---
title: "Elegantly Handle Errors Returned by Deferred Functions"
date: 2019-09-22T12:37:41+05:30
draft: true
---

As a Go developer, you would have seen this code snippet very often:

{{< highlight go "linenos=table, hl_lines=6">}}
resource, err := some.Resource()
if err != nil {
    log.Fatal("error", err.Error())
}

defer resource.Close()

// do something with the resource
{{< / highlight >}}

Usually, above code snippets are seen when dealing with resources like database connections, file handlers etc. While nothing seems wrong with this code (there really is nothing wrong here to be honest), some IDEs, static analysis tools or linters will report an unhandled error warning on the line highlighted above.

There are different trains of thought on how best to deal with this error. There is no good way to handle or log these errors when using `defer`.

Consider the following snippet of code

{{< highlight go "linenos=table" >}}
func Close(close io.Closer) {
    err := close.Close()
    if err != nil {
        log.Fatal("error occurred wile trying to close the resource", err.Error())
    }
}
{{< / highlight >}}

The above code snippet is where Go