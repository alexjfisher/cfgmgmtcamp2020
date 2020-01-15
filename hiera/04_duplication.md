<!SLIDE bullets incremental>
# Duplicated data

* Very common to have identical data values across roles/profiles
* Lots of ways to reduce this duplication
* Lots of ways to do it badly

~~~SECTION:notes~~~

* Hopefully this is where I can share a pattern that's really worked out well.
* If you're drowning in your hiera, maybe this will help.

~~~ENDSECTION~~~
<!SLIDE >
# Bad example
    @@@ Puppet
    class profile::puppetdb(
      $database_password,
    ){
      ...
    }
#
    @@@ Yaml
    # hieradata/role/puppetdb/environment/prod.yaml
    ---
    profile::puppetdb::database_password: 'secret'
#
    @@@ Puppet
    class profile::postgresql_dbs::puppetdb {
      $db_password = hiera('profile::puppetdb::database_password')
    }
#

* Don't lookup some other profile's data directly
* Mixing automatic lookups with explicit calls is confusing
<!SLIDE >
# Slightly better, (but still bad).

    @@@ Yaml small
    # hieradata/role/puppetdb/environment/prod.yaml
    ---
    profile::puppetdb::database_password: 'secret'
#
    @@@ Yaml small
    # hieradata/role/postgresql_master/role.yaml
    ---
    profile::postgresql_dbs::puppetdb::db_password: "%{hiera('profile::puppetdb::database_password')}"
#
* We're still looking up another profile's data
* At least it's all in the hiera
<!SLIDE >
# A Solution
    @@@ Yaml small
    # hieradata/environment/prod.yaml
    ---
    puppetdb_db_password: 'secret'
#
    @@@ Yaml small
    # hieradata/role/puppetdb/role.yaml
    ---
    profile::puppetdb::database_password: "%{alias('puppetdb_db_password')}"
#
    @@@ Yaml small
    # hieradata/role/postgresql_master/role.yaml
    ---
    profile::postgresql_dbs::puppetdb::db_password: "%{alias('puppetdb_db_password')}"
#
* No profile is more important that the other
* A 'plain' key is something looked up by other keys
* Profiles only ever use automatic data lookup
* It's easier to follow
* It's easier to refactor

~~~SECTION:notes~~~

* Big reveal!
* Now I can just grep the hiera
* At a glance I know what something is for and whether I can nuke it.

~~~ENDSECTION~~~
