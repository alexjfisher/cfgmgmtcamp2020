<!SLIDE bullets incremental>
# Making the most out of types

## Once upon a time...
    @@@ Puppet
    class foo(
      $servers,
    ){
      validate_array($servers)
    }
* Part of a family of deprecated stdlib functions
* Better than nothing
* But an Array of what?

<!SLIDE bullets incremental>
# Making the most out of types

## Since Puppet 4

    @@@ Puppet
    class foo(
    *  Array[Stdlib::Host,1] $servers,
    )

* *Much* better.
* We now also assert the contents of the Array
* Puppet-strings can use this to document your modules

<!SLIDE>
# Making the most out of types

* Sometimes you do still need to check type within your code
* There are plenty of ways to do this

##
    @@@ Puppet
    class foo(
      Variant[Array[Stdlib::Host,1],Hash] $servers,
    ){
       if is_hash($servers) {
         # ...
       } else {
         # ...
       }
    }

  * That's part of another family of deprecated stdlib functions
  * Data types to the rescue!

<!SLIDE>
# Making the most out of types
## `is_a()`

    @@@ Puppet
    if $enable_foo {
      # $foo is no longer allowed to be `undef`
      unless $foo.is_a(Array) {
       fail('foo is not an Array!')
      }
    }

* In this example, `$foo` is an `Optional[Array]`.
* A stdlib function returning `true` or `false`
* Better than `is_hash()`. Can use *any* valid data type.
* Much better than the old `validate_x` functions!

<!SLIDE>
# Making the most out of types
## Using =~

    @@@ Puppet
    unless $foo =~ Array {
     fail('foo is not an Array!')
    }

## assert\_type()

    @@@ Puppet
    assert_type(Array, $foo)

* `assert_type` can do more than just raise an error

<!SLIDE incremental>
# Making the most out of types

* You can use them in other constructs too

## `case` statements

    @@@ Puppet
    case $servers {
      Array[Stdlib::Fqdn]: { notice('servers is an array of fqdns') }
      Array[Stdlib::Host]: { notice('servers is an array of IPs')   }
      default:             { notice('servers is a Hash')            }
    }

## `selectors`

    @@@ Puppet
    $unwrapped_secret = $secret ? {
      Sensitive => $secret.unwrap,
      default   => $secret,
    }

* Internally `=~` is being used.
* Don't get me started on `Sensitive`!!
