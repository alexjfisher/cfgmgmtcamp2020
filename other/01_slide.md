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

~~~SECTION:notes~~~

* Ask audience about strange ways they've seen companies use something other than r10k
* Please don't use a mono repository.  Mixing forge modules downloaded at random points in time.
~~~ENDSECTION~~~

<!SLIDE incremental bullets>
# Not Invented Here!

* How many of your own modules do you have?
* I have < 5 in my Puppetfile
* Is it something you could release, (contains no organisation specific logic and is useful to others), it's a module.
* Otherwise maybe it's just another profile class?
* Use the Puppet Forge!
* If something doesn't quite meet your requirements, 're-evaluate' your requirements or contribute enhancements.
* Please, please don't write your own Apache, PostgreSQL module etc.

~~~SECTION:notes~~~

* Tell story of 3 'postgresql' puppet modules being written at the same time on 3 different floors.
~~~ENDSECTION~~~
