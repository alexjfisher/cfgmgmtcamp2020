<!SLIDE >
# The problem

* 'Puppetising' installation docs
* Replacing with series of `Exec`s
* Chained together
* Overuse of `refreshonly`

<!SLIDE >
# Example (Don't do this!)
    @@@ Puppet
    exec { 'Download foo':
      command => '/usr/bin/curl http://... > /tmp/foo.zip',
      creates => '/tmp/foo.zip',
    }
    -> exec { 'Extract foo':
      command => '/bin/unzip /tmp/foo.zip',
      creates => '/usr/local/foo',
    }
    -> file { '/usr/local/foo/foo.config':
      content => template(...),
    }
    ~> exec { 'Run setup':
      command     => '/usr/local/foo/bin/setup',
      refreshonly => true,
    }
    ~> exec { 'More stuff':
      command     => '...',
      refreshonly => true,
    }
    ~> ...

~~~SECTION:notes~~~

* Switched to using notifies half way down
* This is really fragile.  If the exit code isn't 0, chained execs won't then be run (ever!)
* Also common to see lots of file resources chained together and just too many unneeded dependencies in general.
~~~ENDSECTION~~~

<!SLIDE bullets incremental>
# Solutions

> [Friends Don't Let Friends Use Refreshonly](http://ffrank.github.io/misc/2015/05/26/friends-don't-let-friends-use-refreshonly/) - Felix Frank

* Prefer `creates` over `refreshonly`
* Write a single script deployed with puppet
* Have a single `Exec` execute it
* Better to create an RPM package instead
* Can be installed and configured with package/config/service pattern
~~~SECTION:notes~~~
* If you're scared of building RPMs, maybe try FPM
* Incidentally, this is always better than chucking binaries in your modules.
~~~ENDSECTION~~~
