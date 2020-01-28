<!SLIDE>
# `create_resources`

* A hack to solve lack of native iteration
* Creates resources from a hash of hashes (usually sourced from hiera)

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
