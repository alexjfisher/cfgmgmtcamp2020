<!SLIDE incremental bullets>
# Puppet servers and code management

* How many puppet server environments have you got?
* With completely different code environments??
* 7?! (That I've found so far)
* The same codebase should be used across **all** of your infrastructure
* Hiera data is what differentiates 'server environments'
* Branches are for code development not 'server environments'
* Use R10K! (or maybe Puppet Librarian)
* Don't event your own 'better' way

<!SLIDE incremental bullets>
# Not Invented Here!

* How many of your own modules do you have?
* I have < 5 in my Puppetfile
* If it's something you could release, it's a module.  Otherwise maybe it's just another profile class.
* Use the Puppet Forge!
* If something doesn't quite meet your requrements, loosen your requirements or contribute enhancements.
* Please, please don't write your own Apache, PostgreSQL module etc.
