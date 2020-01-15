<!SLIDE >
# The problem

* Using execs when other resources exist
* A far too common beginner mistake

<!SLIDE >
# Just why??!!

    @@@ Puppet
    exec { 'Create directory':
      command => '/bin/mkdir /opt/foo',
      unless  => '/usr/bin/test -e /opt/foo',
    } ->
    exec { 'Change permissions':
      command => '/bin/chmod 0700',
      unless  => '/usr/bin/stat -c "%f" /opt/foo | grep -q 81b4'
    } ->
    exec ...

~~~SECTION:notes~~~
* Who's seen stuff like this?
~~~ENDSECTION~~~
<!SLIDE >

# Have you heard of the `File` resource??

    @@@ Puppet
    file { '/opt/foo':
      ensure => directory,
      mode   => '0700',
    }
