---
title: First steps in using C++11 with Rcpp
author: Dirk Eddelbuettel
license: GPL (>= 2)
tags: basics c++11 featured
summary: This post shows how to experiment with C++11 features
layout: post
src: 2013-01-09-first-steps-with-C++11.Rmd
---                                                                                                                                                          
The recent release of the C++11 standard has brought a lot of
attention to the new language features.  Rcpp, as a CRAN package,
follows CRAN policy in not (yet at least) supporting this standard for its
purported _non-portable_ status. As even the current `g++` version
does of course support C++11 by default, one
still needs to explicitly enable C++11 support which we can do here from R
prior to compiling:


{% highlight r %}
Sys.setenv("PKG_CXXFLAGS"="-std=c++11")
{% endhighlight %}


Starting with version 0.10.3 of Rcpp, one can also enable this via the
'cpp11' plugin as shown below.

C++11 has a lot of nice features; the [Wikipedia
page](http://en.wikipedia.org/wiki/C%2B%2B11) is pretty thorough in
describing them, and the recently-created 
[ISO C++](http://isocpp.org/) website has a lot of additional material.

In a first example, we look at the `auto` keyword which allows the compiler
to infer the type based on the assignment.


{% highlight cpp %}

#include <Rcpp.h>

// Enable C++11 via this plugin (Rcpp 0.10.3 or later)
// [[Rcpp::plugins(cpp11)]]

// [[Rcpp::export]]
int useAuto() {
    auto val = 42;		// val will be of type int
    return val;
}
{% endhighlight %}


Testing it:


{% highlight r %}
useAuto()
{% endhighlight %}



<pre class="output">
[1] 42
</pre>


Another lovely feature are _initialiser lists_:


{% highlight cpp %}

#include <Rcpp.h>

// [[Rcpp::export]]
std::vector<std::string> useInitLists() {
    std::vector<std::string> vec = {"larry", "curly", "moe"};
    return vec;
}
{% endhighlight %}


Testing it:


{% highlight r %}
useInitLists()
{% endhighlight %}



<pre class="output">
[1] &quot;larry&quot; &quot;curly&quot; &quot;moe&quot;  
</pre>


Lastly, we can appreciate the addition _loops over ranges_:


{% highlight cpp %}

#include <Rcpp.h>

// [[Rcpp::export]]
int simpleProd(std::vector<int> vec) {
    int prod = 1;
    for (int &x : vec) {       // loop over all values of vec
       prod *= x;              // access each elem., comp. product
    }
    return prod;
}
{% endhighlight %}


This third example we can also look at:


{% highlight r %}
simpleProd(1:5)
{% endhighlight %}



<pre class="output">
[1] 120
</pre>


Again, we need to remind the reader that this still requires setting the
`-std=c++11` option for `g++`, and that CRAN will not allow this in uploads.
Yet.  That time will come though.  And in the meantime, this can of course be
used for non-CRAN projects.  

