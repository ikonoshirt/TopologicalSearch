# Topological Sorting of Total Models
In Magento we have the problem, that `Mage_Sales_Model_Config_Ordered` should sort a bunch of items topological, but unfortunately it doesn't.

I encountered the problem with wrongly sorted totals a few times, but now it was enough. I wrote [a blog article about it](http://blog.fabian-blechschmidt.de/mage_weee-and-why-it-is-important-for-tax-calculation/). Thanks to [@Daniel_Sloof](https://twitter.com/daniel_sloof/status/610208889448058880), he pointed me to [more informations from @VinaiKopp](http://stackoverflow.com/a/9258826/1480397). I then found a question on [Stackoverflow from @s3lf](http://stackoverflow.com/questions/11953021/topological-sorting-in-php).

After reading all this I decided to implement a module which fixes this. [Thanks to Dan Mossop a topo sort library for PHP already exists](http://blog.calcatraz.com/php-topological-sort-function-384). But it was untested, so I decided to write a few tests for it and then write a Magento module. But we are all lazy and why write tests, if already someone else did. [So lets use the library from Marc J. Schmidt](https://packagist.org/packages/marcj/topsort). 

Unfortunately is the Magento bug in an abstract class, therefore we have to overwrite a core file in `app/code/local`


# Installation
## 1. Install the library dependency
Install the library fro Marc J. Schmidt as a composer dependency by running `composer require marcj/topsort`

## 2. Install the Magento part
Install this repo via modman or copy the code from [src/app/code/local/Mage/Sales/Model/Config/Ordered.php](https://github.com/ikonoshirt/TopologicalSearch/blob/master/src/app/code/local/Mage/Sales/Model/Config/Ordered.php) to `app/code/local/Mage/Sales/Model/Config/Ordered.php` manually.
    
    
# What this module does
This module takes a library for topological sorting and fixes the broken algorithm to sort lots of stuff which has `before` and `after` dependencies, like totals in quote, order and creditmemo.


#License
My work is under BSD 3-clause. But be careful, I only changed the `\Mage_Sales_Model_Config_Ordered::_getSortedCollectorCodes` implementation and added the topological sorting to the class. The rest is copied from Magento version 1.9.1
