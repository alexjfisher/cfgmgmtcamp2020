<!SLIDE >
# The problem

* 'Puppetising' installation docs
* Replacing with series of `Exec`s
* Chained together
* Overuse of `refreshonly`

<!SLIDE >
# Example

TODO: Find or make up example

<!SLIDE bullets incremental>
# Solutions

* Prefer `creates` over `refreshonly`
* Write a single script deployed with puppet
* Have a single `Exec` execute it
* Better to create a RPM package instead
* Can be installed and configured with package/config/service pattern
