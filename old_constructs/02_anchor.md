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

* Available since Puppet 3.4
* puppet-lint plugin available
* There may still be some use-cases
