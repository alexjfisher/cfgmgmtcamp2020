<!SLIDE>
# `is_a()`
    @@@ Puppet
    unless $foo.is_a(Array) {
     fail('foo is not an Array!')
    }

* A stdlib function returning `true` or `false`

## Using =~

    @@@ Puppet
    unless $foo =~ Array {
     fail('foo is not an Array!')
    }

## assert\_type()

    @@@ Puppet
    assert_type(Array, $foo)

* `assert_type` can do more than just raise an error
