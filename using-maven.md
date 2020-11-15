---
layout: base
title: "How to depend on sumcoinj with Maven using projects"
---

# How to depend on sumcoinj with Maven using projects

[Maven](http://maven.apache.org/) is a plugin based build system from Apache. It supports various other features, like automatically creating a website for your project and resolving dependencies.

If your project uses Maven for its build, you can depend on sumcoinj by adding the following snippet to your pom.xml file in the:

{% highlight xml %}
  <dependencies>
    <dependency>
      <groupId>org.sumcoinj</groupId>
      <artifactId>sumcoinj-core</artifactId>
      <version>0.15.7</version>
      <scope>compile</scope>
    </dependency>
  </dependencies>
{% endhighlight %}

To get the source you can also use git and check out your own copy:

~~~
git clone https://sumcoinlabs/sumcoinj.git
cd sumcoinj
git fetch --all
git checkout v0.15.7
mvn install
~~~

This will give you v0.15.7. If PGP is your thing, you can run `git tag -v v0.15.7` to check Andreas Schildbach's signature of the release.
