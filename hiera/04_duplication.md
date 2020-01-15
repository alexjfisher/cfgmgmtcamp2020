<!SLIDE bullets incremental>
# Duplicated data

* Very common to have identical data values across roles/profiles
* Lots of ways to reduce this duplication
* Lots of ways to do it badly

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

    @@@ Yaml
    # hieradata/role/puppetdb/environment/prod.yaml
    ---
    profile::puppetdb::database_password: 'secret'
#
    @@@ Yaml
    # hieradata/role/postgresql_master/role.yaml
    ---
    profile::postgresql_dbs::puppetdb::db_password: "%{hiera('profile::puppetdb::database_password')}"
#
* We're still looking up another profile's data
* At least it's all in the hiera
<!SLIDE >
# A Solution
    @@@ Yaml
    # hieradata/environment/prod.yaml
    ---
    puppetdb_db_password: 'secret'
#
    @@@ Yaml
    # hieradata/role/puppetdb/role.yaml
    ---
    profile::puppetdb::database_password: "%{alias('puppetdb_db_password')}"
#
    @@@ Yaml
    # hieradata/role/postgresql_master/role.yaml
    ---
    profile::postgresql_dbs::puppetdb::db_password: "%{alias('puppetdb_db_password')}"
#
* No profile is more important that the other
* A 'plain' key is something looked up by other keys
* Profiles only ever use automatic data lookup
* It's easier to follow
* It's easier to refactor
