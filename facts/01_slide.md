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

* Invokes shell commands with backticks
* Returns literal strings "true" and "false"
* Performs something trivial to do with Ruby
* If you think "you don't know Ruby", try googling the answer!

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

* This time uses `system`, which returns `true` or `false` (or `nil`)
* In this example, already up to 3 times!
* Spawns yet more processes by piping to grep
* This can get SLOW, especially as you starting adding more roles

<!SLIDE >
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

* Custom facts can reference other facts
* Ruby cases aren't difficult to understand
* You can use regexes with them

<!SLIDE bullets incremental>
# Writing better facts

* Ruby isn't *that* hard
* Do you know how to google?
* Use `Facter::Core::Execution.execute` not `system` or backticks
* You can also use 'external facts' if you really don't want to touch ruby
