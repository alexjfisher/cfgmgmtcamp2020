<!SLIDE >
# Shelling out just because

* Facts are written in Ruby, but I guess some people just love shell too much
* You really don't need to know much ruby to do better than this

<!SLIDE >
## Bad

    @@@ Ruby
    # Check that Mysql is installed and initiated by seeing that /var/lib/mysql/mysql exists
    Facter.add("mysql_is_setup") do
      setcode do
        answer = `if /usr/bin/test -d /var/lib/mysql/mysql; then echo -n true; else echo -n false; fi`
      end
    end

## Better (although was that ever really a great test?)

    @@@ Ruby
    Facter.add("mysql_is_setup") do
      setcode do
        File.directory?('/var/lib/mysql/mysql')
      end
    end

<!SLIDE >
## Bad

    @@@ Ruby
    Facter.add('role') do
      setcode do
        if system("/bin/hostname -f | grep -q '^db'")
          fact = 'database_server'
        elsif system("/bin/hostname -f | grep -q '^web'")
          fact = 'web_server'
        elsif system("/bin/hostname -f | grep -q '^mysql'")
          fact = 'database_server'
        end
        fact
      end
    end

## Better

    @@@ Ruby
    Facter.add('role') do
      setcode do
        case Facter.value(:fqdn)
        when /^(db|mysql)/
          'database_server'
        when /^web/
          'web_server'
        end
      end
    end

<!SLIDE bullets incremental>
# Writing better facts

* Ruby isn't *that* hard
* Use `Facter::Core::Execution.execute` not `system` or backticks
* You can also use 'external facts' if you really don't want to touch ruby
