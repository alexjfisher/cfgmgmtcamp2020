<!SLIDE bullets incremental>
# The Super Hash

* What is a super hash?
* My term for an overly broad hash of data used across otherwise unrelated profiles
* Why do I see it so often?

<!SLIDE >
# example
    @@@ Yaml
    oracle:

      jdk:
        ver: 1.7.0_07-fcs

      middleware:
        home: /u01/app/oracle/Middleware

      user:
        name: oracle
        group: dba
        pwd: $1$DdJ53vh6$4XREDACTEDBi/54rglGk3.

      database:
        base: /u01/app/oracle
        nexus:
          url:        "%{hiera('nexus:url')}"
          repository: thirdparty
~~~SECTION:notes~~~
* Has anyone else seen random hieradata like this?
~~~ENDSECTION~~~
<!SLIDE bullets incremental>
# What goes wrong

* `$oracle = hiera_hash('oracle')` called from multiple profiles
* Index into that hash `$nexus_url = $oracle['database']['nexus']['url']`
* Puppet explodes because something didn't exist for your host for some reason
* Some more specific yaml overrode `$oracle['database']`
* The `dig` function can help you, but still best to avoid creating monstrous multi-purpose hashes!
* It could be worse, (I suppose).
* `hiera_hash('data')` ??!
~~~SECTION:notes~~~
* hiera_hash('data') was an attempt at illustrating to a client that big top level hashes were probably a bad idea.
* Nobody dares touch anything or refactor for fear of breaking 'something'
~~~ENDSECTION~~~
