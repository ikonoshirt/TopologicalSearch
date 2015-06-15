#Topological Sorting of Total Models
In Magento we have the problem, that `Mage_Sales_Model_Config_Ordered` should sort a bunch of items topological, but unfortunately it doesn't.

I encountered the problem with wrongly sorted totals a few times, but now it was enough. I wrote [a blog article about it](http://blog.fabian-blechschmidt.de/mage_weee-and-why-it-is-important-for-tax-calculation/). Thanks to [@Daniel_Sloof](https://twitter.com/daniel_sloof/status/610208889448058880), he pointed me to [more informations from @VinaiKopp](http://stackoverflow.com/a/9258826/1480397). I then found a question on [Stackoverflow from @s3lf](http://stackoverflow.com/questions/11953021/topological-sorting-in-php).

After reading all this I decided to implement a module which fixes this. [Thanks to Dan Mossop there existed already an algorithm](http://blog.calcatraz.com/php-topological-sort-function-384). But it was untested, so I decided to write a few tests for it and then write a Magento module. But we are all lazy and why write tests, if already someone else did. [So lets take the algorithm from Marc J. Schmidt](https://packagist.org/packages/marcj/topsort). 

Unfortunately is the bug in an abstract class, therefore we have to overwrite a core file in `app/code/local`


# Installation
##composer
 Add the following to your composer.json

    {
     "require": {
         "marcj/topsort": "~0.1"
     }
    }
    
The problem is, that magento doesn't load libraries, that use namespaces. But as always, there is a [stackoverflow question, how to add the composer autoloader](http://magento.stackexchange.com/questions/1375/integrating-composers-autoloader-into-magento). I personally just added `require 'vendor/autoload.php';` to the first line of `index.php`. Not best practice, but our index.php is already edited, so it doesn't matter.

# What this module is doing
This module takes a library for topological sorting and fixes the broken algorithm to sort lots of stuff which has `before` and `after` dependencies, like totals in quote, order and creditmemo.
