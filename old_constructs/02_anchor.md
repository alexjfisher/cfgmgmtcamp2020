<!SLIDE>
# `anchor`

* A 'type' that provides a way to 'contain' resources in a class

## Puppet 3
    @@@ Puppet
    class foo {
      include foo::install
      include foo::config
      include foo::service

      anchor { 'foo::begin': }
      -> Class['foo::install']
      -> Class['foo::config']
      ~> Class['foo::service']
      -> anchor { 'foo::end': }
    }

~~~SECTION:notes~~~

* anchor is an actual type, but with no properties.
* The resource name is irrelevant too.
* Classes not being 'contained' is a problem that is often confusing to new-comers.
* "Puppet isn't doing what I've told it to!"
* When you say one class comes before another, it only applies to *resources* in those classes.
* Other included classes could be included in any number of other places, so don't default to being 'contained' in anyway.
* `anchor` was conceived as a hack to solve this.
* Now we *can* contain the resources included in other classes.
* We assume the subclasses *also* anchor anything they need to.

~~~ENDSECTION~~~
<!SLIDE>

## Puppet 4+
    @@@ Puppet
    class foo {
    *  contain foo::install
    *  contain foo::config
    *  contain foo::service

      Class['foo::install']
      -> Class['foo::config']
      ~> Class['foo::service']
    }

* Actually available since Puppet 3.4, (but there were a few buggy versions)
* puppet-lint plugin available
* There may still be some use-cases

~~~SECTION:notes~~~

* This is *much* easier ever since the `contain` function was introduced.
* But *dont'* `contain` everything without thinking about it properly
* Very easy to create circular dependencies
* Maybe you meant `require` instead??
* Ewoud would like to take you through some where anchor can still be useful.
* Blog post please! ;)

~~~ENDSECTION~~~
