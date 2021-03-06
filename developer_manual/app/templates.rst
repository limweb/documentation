=========
Templates
=========

.. sectionauthor:: Bernhard Posselt <dev@bernhard-posselt.com>

ownCloud provides its own templating system which is basically plain PHP with some additional functions and preset variables. All the parameters which have been passed from the :doc:`controller <controllers>` are available in an array called **$_[]**, e.g.::
    
    array('key' => 'something')

can be accessed through::

    $_['key']


.. note:: To prevent XSS the following PHP **functions for printing are forbidden: echo, print() and <?=**. Instead use the **p()** function for printing your values. Should you require unescaped printing, **double check for XSS** and use: :php:func:`print_unescaped`.

Printing values is done by using the **p()** function, printing HTML is done by using **print_unescaped()**

:file:`templates/main.php`

.. code-block:: php

  <?php foreach($_['entries'] as $entry){ ?>
    <p><?php p($entry); ?></p>
  <?php
  }

  
Including templates
===================

Templates can also include other templates by using the **$this->inc('templateName')** method. 

.. code-block:: php

  <?php print_unescaped($this->inc('sub.inc')); ?>

The parent variables will also be available in the included templates, but should you require it, you can also pass new variables to it by using the second optional parameter as array for **$this->inc**.

:file:`templates/sub.inc.php`

.. code-block:: php

  <div>I am included, but I can still access the parents variables!</div>
  <?php p($_['name']); ?>
  
  <?php print_unescaped($this->inc('other_template', array('variable' => 'value'))); ?>

Including CSS and JavaScript
============================
To include CSS or JavaScript use the **addStyle** and **addScript** functions:

.. code-block:: php

  <?php
  \OCP\Util::addScript('myapp', 'script');  // add js/script.js
  \OCP\Util::addStyle('myapp', 'style');  // add css/style.css


Including images
================
To generate links to images use the **imagePath** function:

.. code-block:: php
 
  <img src="<?php\OCP\Util::imagePath('myapp', 'app.png'); // img/app.png?> />

