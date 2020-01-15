<!SLIDE>
# `create_resources`

* A hack to solve lack of native iteration
* Creates resources from a hash of hashes (usually sourced from hiera)
~~~SECTION:notes~~~
* This is probably the biggest
* Puppet 3 didn't have any good solutions
* There were some other 'hacks' like passing arrays to defined types.
* Ben Ford has often discussed writing a blog post 'create_resources considered evil'
* Can he sum up why he thinks it's *so* bad.
~~~ENDSECTION~~~

## Puppet 3
    @@@ Puppet
    create_resources('check_mk::agent::mrpe', $mrpe_checks)

<!SLIDE>
# `create_resources`

* A hack to solve lack of native iteration
* Creates resources from a hash of hashes (usually sourced from hiera)

## Puppet 3
    @@@ Puppet
    create_resources('check_mk::agent::mrpe', $mrpe_checks)

## Puppet 4+
    @@@ Puppet
    $mrpe_checks.each | String $key, Hash $attrs| {
      check_mk::agent::mrpe { $key:
        * => $attrs,
      }
    }
~~~SECTION:notes~~~
* Defaults are handled differently and their are some gotchas
~~~ENDSECTION~~~
<!SLIDE incremental>
# `create_resources`
## Resource Defaults

    @@@ Puppet
    $resources.each | String $key, Hash $attrs| {
      type { $key:
    *    ensure => present,
        *      => $attrs,
      }
    }

## Problem?

  * $attrs isn't allowed to duplicate/override the default keys


<!SLIDE>
# `create_resources`
## Resource Defaults

    @@@ Puppet
    $resources.each | String $key, Hash $attrs| {
      type { $key:
    *    ensure => present,
        *      => $attrs,
      }
    }

## Problem?

  * $attrs isn't allowed to duplicate/override the default keys

## Solution: Merge the hashes (using modern syntax!)
    @@@ Puppet
    $defaults = {
      ensure => present,
    }
    $resources.each | String $key, Hash $attrs| {
      type { $key:
        * => $defaults + $attrs,
      }
    }
~~~SECTION:notes~~~
* Maybe you could also get the defaults from hiera?
~~~ENDSECTION~~~
