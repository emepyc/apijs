apijs
=====

A sipmle javascript library to create api's for objects

###Overview

Given an object, <b>api</b> generates <i>getters/setters</i> and methods on it.
Consider the following example:
<code>

    var opts = {
        width  : 200,
        height : 500
    };

    var my_object = {};

    var api = apijs(my_object);
    
    // creates the 'width' and 'height' methods:
    api.getset(opts);

    // creates checks and transforms on the methods:
    my_object.width
        .transform (function (val) {
            return Math.abs(val);
        })
        .check (function (val) {
            return val > 0;
        });

</code>
Creates the <i>width</i> and <i>height</i> methods in the given given object. These methods can be used as follows:
<code>

    my_object.width(250)
             .height(800);
             
    my_object.width();  // returns 250
    my_object.height(); // returns 800
    
    my_object.width(-500);
    my_object.width();  // returns 500
</code>

### Getters / Setters
Getters/Setters can be created by name:
<code>

    api.getset('width', 200);  // 200 as default value
    api.getset('height', 500); // 500 as default value

</code>
Methods can also be created using an object. This way allows to access the exposed properties in the object without calling the accessor:
<code>

    var params = {
        width  : 200,
        height : 500
    };
    api.getset(params); // creates my_object.width and my_object.height methods

    my_object.width(800); // Set a new value for params.width
    console.log(params.width); // 800 

</code>
The library also allows to define getters:
<code>

    api.get('width', 200); // width is a getter with a default value of 200
    my_object.width(500);  // throws an error

</code>
Getters can also be created using objects:
<code>

    // creates two getters
    api.get ({
        'width'  : 200,
        'height' : 500
    });

### Checks
</code>
The library also exposes the <i>check</i> method to check the passed variables:
<code>

    api.getset('width', 200);
    api.check('width', function (val) {
        return (val > 100);
    });

</code>
More than one check can be defined for a method:
<code>

    api.getset('width', 200);
    api.check('width', function (val) {
        return (val > 100);
    });
    api.check('width', function (val) {
        return $.isNumeric(val);
    });

</code>
Checks can be defined on multiple methods at the same time:
<code>

    api.getset ({ width  : 200,
                  height : 500
                });
    api.check (['width', 'height'], function (val) {
        return $.isNumeric(val);
    });

</code>
Checks can be defined using the method itself of its name:
<code>

    var check_numeric = function (val) {
        return $.isNumeric(val);
    }
    // These two options are equivalent:
    api.check('width', check_numeric);
    api.check(my_object.width, check_numeric);

</code>
Checks accept an extra argument with the error message:
<code>

    api.getset('width', 200);
    api.check('width', check_numeric, 'Argument should be numeric');
    my_object.width({ w:100 }); // Throws 'Argument should be numeric'

</code>
### Transformations
api also exposes the <i>transform</i> method to make transformations on the passed values:
<code>

    api.getset('width', 200);
    api.transform('width', function (val) {
        return Math.abs(val);
    });

</code>
Transformations are run before checks:
<code>

    api.getset('width', 200);
    api.transform('width', function (val) {
        return Math.abs(val);
    });
    api.transform('width', function (val) {
        return val > 0;
    });
    my_object.width(-500);  // Doesn't complain
    my_object.width();      // 500

</code>
Transformations can be defined on multiple methods at the same time:
<code>

    api.getset ({ width  : 200,
                  height : 500
                });
    api.transform (['width', 'height'], function (val) {
        return Math.abs(val);
    });

</code>
Transformations can be defined using the method itself of its name:
<code>

    var abs_val = function (val) {
        return Math.abs(val);
    };
    // These two options are equivalent
    api.transform('width', abs_val);
    api.transform(my_object.width, abs_val);

</code>
Checks and transformations can be defined directly in the created methods:
<code>

    my_object.width.check(function (val) {
        return $.isNumeric(val);
    });
    my_object.width.transform(function (val) {
        return Math.abs(val);
    });

</code>
Checks and transformations are chainable if used via the method interface:
<code>

    my_object.width
        .transform (function (val) {
            return Math.abs(val);
        })
        .check (function (val) {
            return val > 0;
        });

</code>
### Methods
The library also allows to defined methods (not <i>getters/setters</i>
<code>

    var method1 = function () {
        //
    };
    api.method('method1');
    my_object.method1();

</code>
Methods can also be defined with an object containing the methods:
<code>

    var method1 = function () {
        //
    };
    var method2 = function () {
        //
    };
    api.method ({ method1 : method1,
                  method2 : method2
                });

</code>

copyright Miguel Pignatelli 2014
> Written with [StackEdit](https://stackedit.io/)

