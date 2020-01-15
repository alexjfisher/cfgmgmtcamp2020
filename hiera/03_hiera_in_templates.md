<!SLIDE bullets incremental>
# Calling hiera from templates

* Can I call hiera/lookup from a template?
* How can I make this a worse idea?
* Variables in scope are available to use in your hiera.yaml hierarchy
* **Never** do this.
* Please!

~~~SECTION:notes~~~
hiera parameters should be topscope.  eg facts or from an ENC
I've seen `site` being used in a global hiera.yaml where it was a local variable in a class that used a template and the template called hiera.
~~~ENDSECTION~~~
